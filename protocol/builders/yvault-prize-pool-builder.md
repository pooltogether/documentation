# yVault Prize Pool Builder

This builder creates a new [yVault Prize Pool](../prize-pool/yvault-prize-pool.md).

The easiest way to create a yVault Prize Pool is from the PoolWithMultipleWinnersBuilder described [here](./).

The following describes a lower level building option with a custom prize strategy.

### Custom Prize Strategy

Create a yVault Prize Pool with a custom prize strategy using the function:

```javascript
function createVaultPrizePool(
    VaultPrizePoolConfig calldata config
) 
external 
returns (yVaultPrizePool)
```

| Parameter | Description |
| :--- | :--- |
| config | The yVault Prize Pool configuration to use |

```c
struct VaultPrizePoolConfig {
    yVaultInterface vault;
    uint256 reserveRateMantissa;
    uint256 maxExitFeeMantissa;
    uint256 maxTimelockDuration;
  }
```

Note that this will not add any [Controlled Tokens](../prize-pool/#controlled-tokens) to the Prize Pool; those must be added by the owner. The caller is the Prize Pool owner.

