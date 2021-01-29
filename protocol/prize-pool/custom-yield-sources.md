---
description: Create a Prize Pool that Uses a Custom Yield Source
---

# Custom Yield Sources

If you would like to create a Prize Pool that generates prizes with a custom yield source, you'll need to:

1. Subclass the [Prize Pool](https://github.com/pooltogether/pooltogether-pool-contracts/blob/master/contracts/prize-pool/PrizePool.sol).
2. Implement the [Yield Source](https://github.com/pooltogether/pooltogether-pool-contracts/blob/master/contracts/prize-pool/YieldSource.sol) interface

Once the subclass is written, you'll need to deploy it.  The simplest approach will be to follow the [PoolWithMultipleWinnersBuilder](https://github.com/pooltogether/pooltogether-pool-contracts/blob/master/contracts/builders/PoolWithMultipleWinnersBuilder.sol) and create a contract that builds your custom prize pool with the appropriate strategy.



