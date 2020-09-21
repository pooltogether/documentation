# Compound Prize Pool Builder

This builder creates a new prize game using a [Compound Prize Pool](../prize-pool/compound-prize-pool.md).  You can create one that uses the [Single Random Winner](../prize-strategy/single-random-winner/) strategy or build your own.

## Single Random Winner

To create a prize game using the [Compound Prize Pool](../prize-pool/compound-prize-pool.md) and the [Single Random Winner](../prize-strategy/single-random-winner/) prize strategy you can call the createSingleRandomWinner\(\) function:

```javascript
function createSingleRandomWinner(
    CompoundPrizePoolConfig calldata prizePoolConfig,
    SingleRandomWinnerBuilder.SingleRandomWinnerConfig calldata prizeStrategyConfig
) external returns (SingleRandomWinner);
```

| Parameter | Description |
| :--- | :--- |
| prizePoolConfig | The Compound Prize Pool configuration details |
| prizeStrategyConfig | The Single Random Winner configuration details |

### CompoundPrizePoolConfig

```javascript
struct CompoundPrizePoolConfig {
    address cToken;
    uint256 maxExitFeeMantissa;
    uint256 maxTimelockDuration;
}
```

| Parameter | Description |
| :--- | :--- |
| cToken | The [Compound cToken](https://compound.finance/docs/ctokens) to use. |
| maxCreditLimitMantissa | The maximum credit limit. This value is hard coded and cannot be changed. |
| maxTimelockDuration | The maximum duration of the withdrawal timelock in seconds.  This value is hard coded and cannot be changed. |

### SingleRandomWinnerConfig

```javascript
struct SingleRandomWinnerConfig {
    address proxyAdmin;
    address rngService;
    uint256 prizePeriodStart;
    uint256 prizePeriodSeconds;
    string ticketName;
    string ticketSymbol;
    string sponsorshipName;
    string sponsorshipSymbol;
    uint256 ticketCreditLimitMantissa;
    uint256 ticketCreditRateMantissa;
    address[] externalERC20Awards;
}
```

| Parameter | Description |
| :--- | :--- |
| proxyAdmin | Optional - if supplied then the Prize Strategy will be upgradeable by this address.  This is expected to be an OpenZeppelin ProxyAdmin contract. |
| rngService | The [Random Number Generator](../random-number-generator/) service to use |
| prizePeriodSeconds | The prize period of the Prize Strategy. |
| ticketName | The name to use for the ticket ERC20 |
| ticketSymbol | The symbol to use for the ticket ERC20 |
| sponsorshipName | The name to use for the sponsorship token ERC20 |
| sponsorshipSymbol | The symbol to use for the sponsorship token ERC20 |
| ticketCreditLimitMantissa | The default credit limit as a fixed point 18 fraction.  For example, if a user is withdrawing 100 Dai and the exit fee is "0.1 ether" then the credit limit will be 10 Dai.  This value can be changed in by the Prize Pool owner. |
| ticketCreditRateMantissa | The rate at which a user accrues credit, per second, expressed as a fixed point 18 number.  For example, a credit rate of "0.01 ether" means that a user will accrue 1% credit on their deposit every second.  This credit is burned to offset the early exit fees and withdrawal timelocks.  This value can be changed by the Prize Pool owner. |
| externalERC20Awards | Registers addresses of external ERC20 tokens that should be awarded as part of the prize.  If the Prize Pools has a non-zero balance of any of these tokens they will be awarded to the winner alongside the accrued interest. |

## Custom Prize Strategy

To create a [Compound Prize Pool](../prize-pool/compound-prize-pool.md) with a custom [Prize Strategy](../prize-strategy/) you can call the createCompoundPrizePool\(\) function:

```javascript
function createCompoundPrizePool(
    CompoundPrizePoolConfig calldata config,
    TokenListenerInterface prizeStrategy
)
external
returns (CompoundPrizePool)
```

This function will create a new Compound Prize Pool bound to the given Prize Strategy, but it will not create any [Controlled Tokens](../prize-pool/#controlled-tokens).  Those must be added by the caller.  The caller will be the owner of the Prize Pool.

| Parameter | Description |
| :--- | :--- |
| config | The Compound Prize Pool configuration to use |
| prizeStrategy | The address of the Prize Strategy to bind to.  Must implement the [Token Listener](../prize-pool/token-listener.md) interface. |

## Events

Both of these function emit the event:

```javascript
event CompoundPrizePoolCreated (
    address indexed creator,
    address indexed prizePool,
    address indexed prizeStrategy
);
```

| Event Parameter | Description |
| :--- | :--- |
| creator | The address that called this contract and who is the owner. |
| prizePool | The address of the Prize Pool that was created |
| prizeStrategy | The address of the Prize Strategy that was created |

