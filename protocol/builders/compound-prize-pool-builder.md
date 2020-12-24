# Compound Prize Pool Builder

This builder creates a new prize pool game using a [Compound Prize Pool](../prize-pool/compound-prize-pool.md).  
You can create one that uses the [Multiple Winners](../prize-strategy/multiple-winners/) strategy or build your own.

The easiest way to create a Compound Prize Pool is from the PoolWithMultipleWinnersBuilder described [here](./).

The following describes a lower level building option with a custom prize strategy.

### Custom Prize Strategy

To create a [Compound Prize Pool](../prize-pool/compound-prize-pool.md) with a custom [Prize Strategy](../prize-strategy/) you can call the createCompoundPrizePool\(\) function:

```javascript
function createCompoundPrizePool(
    CompoundPrizePoolConfig calldata config,
)
external
returns (CompoundPrizePool)
```

This function will create a new Compound Prize Pool bound to the given Prize Strategy, but it will not create any [Controlled Tokens](../prize-pool/#controlled-tokens). Those must be added by the caller. The caller will be the owner of the Prize Pool.

| Parameter | Description |
| :--- | :--- |
| config | The Compound Prize Pool configuration to use |

#### CompoundPrizePoolConfig

```c
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

### Events

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

