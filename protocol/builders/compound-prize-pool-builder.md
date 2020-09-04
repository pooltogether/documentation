# Compound Prize Pool Builder

This builder creates a new [Compound Prize Pool](../prize-pool/compound-prize-pool.md) using the [Single Random Winner](../prize-strategy/single-random-winner.md) strategy.

To create a new prize game call the `create()` function.  The caller will be set as the owner of both the newly created Prize Pool and the Prize Strategy.

```javascript
function create(
    Config calldata config
) external returns (PrizeStrategy);
```

The **Config** object has a shape like so:

```javascript
struct Config {
    address proxyAdmin;
    CTokenInterface cToken;
    RNGInterface rngService;
    uint256 prizePeriodStart;
    uint256 prizePeriodSeconds;
    string ticketName;
    string ticketSymbol;
    string sponsorshipName;
    string sponsorshipSymbol;
    uint256 maxCreditLimitMantissa;
    uint256 maxTimelockDuration;
    uint256 creditLimitMantissa;
    uint256 creditRateMantissa;
    address[] externalAwards;
}
```

| Parameter | Description |
| :--- | :--- |
| proxyAdmin | Optional - if supplied then the Prize Strategy will be upgradeable by this address.  This is expected to be an OpenZeppelin ProxyAdmin contract. |
| cToken | The [Compound cToken](https://compound.finance/docs/ctokens) to use |
| rngService | The [Random Number Generator](../random-number-generator.md) service to use |
| prizePeriodSeconds | The prize period of the Prize Strategy. |
| ticketName | The name to use for the ticket ERC20 |
| ticketSymbol | The symbol to use for the ticket ERC20 |
| sponsorshipName | The name to use for the sponsorship token ERC20 |
| sponsorshipSymbol | The symbol to use for the sponsorship token ERC20 |
| maxCreditLimitMantissa | The maximum credit limit. This value is hard coded and cannot be changed. |
| maxTimelockDuration | The maximum duration of the withdrawal timelock in seconds.  This value is hard coded and cannot be changed. |
| creditLimitMantissa | The default credit limit as a fixed point 18 fraction.  For example, if a user is withdrawing 100 Dai and the exit fee is "0.1 ether" then the credit limit will be 10 Dai.  This value can be changed in by the Prize Pool owner. |
| creditRateMantissa | The rate at which a user accrues credit, per second, expressed as a fixed point 18 number.  For example, a credit rate of "0.01 ether" means that a user will accrue 1% credit on their deposit every second.  This credit is burned to offset the early exit fees and withdrawal timelocks.  This value can be changed by the Prize Pool owner. |
| externalERC20Awards | Registers addresses of external ERC20 tokens that should be awarded as part of the prize.  If the Prize Pools has a non-zero balance of any of these tokens they will be awarded to the winner alongside the accrued interest. |

This function will emit the event:

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

