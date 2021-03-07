# Stake Prize Pool

This builder creates a new [Stake Prize Pool](../yield-sources/stake-prize-pool.md).

The easiest way to create a Stake Prize Pool is from the PoolWithMultipleWinnersBuilder described [🛠 Builders](./)

The following describes a lower level building option with a custom prize strategy.

### Custom Prize Strategy

Create a Stake Prize Pool with a custom prize strategy using the function:

```javascript
function createStakePrizePool(
  StakePrizePoolConfig calldata config
)
external
returns (StakePrizePool)
```

| Parameter | Description |
| :--- | :--- |
| config | The Stake Prize Pool configuration to use |

#### StakePrizePoolConfig

```c
struct StakePrizePoolConfig {
    IERC20Upgradeable token;
    uint256 maxExitFeeMantissa;
    uint256 maxTimelockDuration;
}
```



| Parameter | Description |
| :--- | :--- |
| token | The stake token the pool is to initialized to |
| maxExitFeeMantissa | See [⚖️ Fairness](../prize-pool/fairness.md#what-should-the-credit-rate-and-credit-limit-for-a-pool-be) |
| maxTimelockDuration | The maximum duration of the withdrawal timelock in seconds.  This value is hard coded and cannot be changed. |

Note that this will not add any [Controlled Tokens](../prize-pool/#controlled-tokens) to the Prize Pool; those must be added by the owner. The caller is the Prize Pool owner.

