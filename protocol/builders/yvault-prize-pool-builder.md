# yVault Prize Pool Builder

This builder creates new [yVault Prize Pools](../prize-pool/yvault-prize-pool.md).

## Single Random Winner

To create a new Prize Pool that uses the [Single Random Winner](../prize-strategy/single-random-winner/) prize strategy call the create\(\) function:

```javascript
function createSingleRandomWinner(
    yVaultPrizePoolConfig calldata prizePoolConfig,
    SingleRandomWinnerBuilder.SingleRandomWinnerConfig calldata prizeStrategyConfig,
    uint8 decimals
) external returns (SingleRandomWinner);
```

| Parameter | Description |
| :--- | :--- |
| prizePoolConfig | The yVault Prize Pool configuration to use |
| prizeStrategyConfig | The Single Random Winner configuration to use |
| decimals | The number of decimals to use for the collateral tokens |

### yVaultPrizePoolConfig

```javascript
struct yVaultPrizePoolConfig {
    yVaultInterface vault;
    uint256 reserveRateMantissa;
    uint256 maxExitFeeMantissa;
    uint256 maxTimelockDuration;
}
```

| Parameter | Description |
| :--- | :--- |
| vault | The address of the yVault |
| reserveRateMantissa | The prize pool reserve rate  |
| maxExitFeeMantissa | The maximum exit fee as a percent of the withdrawal amount |
| maxTimelockDuration | The maximum length of time a withdrawal can be timelocked |

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

Create a yVault Prize Pool with a custom prize strategy using the function:

```javascript
function createyVaultPrizePool(
    yVaultPrizePoolConfig calldata config,
    PrizePoolTokenListenerInterface prizeStrategy
) external returns (yVaultPrizePool)
```

| Parameter | Description |
| :--- | :--- |
| config | The yVault Prize Pool configuration to use |
| prizeStrategy | The address of the prize strategy.  Must implement the [Token Listener](../prize-pool/token-listener.md) interface. |

Note that this will not add any [Controlled Tokens](../prize-pool/#controlled-tokens) to the Prize Pool; those must be added by the owner.  The caller is the Prize Pool owner.



