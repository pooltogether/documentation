# Stake Prize Pool Builder

This builder creates a new [Stake Prize Pool](../prize-pool/stake-prize-pool.md).

The easiest way to create a Stake Prize Pool is from the PoolWithMultipleWinnersBuilder described [here] (README.md).

The following describes a lower level building option with a custom prize strategy. 

## Custom Prize Strategy

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

### StakePrizePoolConfig

```javascript
struct StakePrizePoolConfig {
    IERC20Upgradeable token;
    uint256 maxExitFeeMantissa;
    uint256 maxTimelockDuration;
}
```


Note that this will not add any [Controlled Tokens](../prize-pool/#controlled-tokens) to the Prize Pool; those must be added by the owner.  The caller is the Prize Pool owner.



