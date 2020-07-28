---
description: Easily create preconfigured Prize Pools
---

# 🛠 Prize Pool Builders

Prize Pool Builders allow users to create preconfigured Prize Pool.  Currently there is a Compound Prize Pool Builder that creates Compound Prize Pools.

## Compound Prize Pool Builder

This builder creates a new [Prize Pool](prize-pool/) that uses a Compound cToken as the yield source.

```javascript
function create(
    Config calldata config
) external returns (PrizeStrategy);
```

The **Config** object has a shape like so:

```javascript
struct Config {
    CTokenInterface cToken;
    uint256 prizePeriodSeconds;
    string ticketName;
    string ticketSymbol;
    string sponsorshipName;
    string sponsorshipSymbol;
    uint256 maxExitFeeMantissa;
    uint256 maxTimelockDuration;
    uint256 exitFeeMantissa;
    uint256 creditRateMantissa;
    address[] externalAwards;
}
```

| Parameter | Description |
| :--- | :--- |
| cToken | The address of the Compound cToken to use |
| prizePeriodSeconds | The prize period to use |
| ticketName | The name to use for the ticket ERC20 |
| ticketSymbol | The symbol to use for the ticket ERC20 |
| sponsorshipName | The name to use for the sponsorship token ERC20 |
| sponsorshipSymbol | The symbol to use for the sponsorship token ERC20 |
| maxExitFeeMantissa | The maximum exit fee that the Prize Strategy can use as a fixed point 18 fraction of the withdrawal amount.  For example, "0.5 ether" would be a fraction of 0.5, so if a user is withdrawing 100 Dai the maximum fee would be 50 Dai.  This value is hard coded and cannot be changed. |
| maxTimelockDuration | The maximum duration of the withdrawal timelock in seconds.  This value is hard coded and cannot be changed. |
| exitFeeMantissa | The default early exit fee as a fixed point 18 fraction.  For example, if a user is withdrawing 100 Dai and the exit fee is "0.1 ether" then the early exit fee will be 10 Dai.  This value can be changed in the Prize Strategy by the owner. |
| creditRateMantissa | The rate at which a user accrues credit, per second, expressed as a fixed point 18 number.  For example, a credit rate of "0.01 ether" means that a user will accrue 1% credit on their deposit every second.  This credit is burned to offset the early exit fees and withdrawal timelocks.  This value can be changed in the Prize Strategy by the owner. |
| externalAwards | Registers addresses of external ERC20 tokens that should be awarded as part of the prize.  If the Prize Pools has a non-zero balance of any of these tokens they will be awarded to the winner alongside the accrued interest. |

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

