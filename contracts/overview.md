# Overview

The core PoolTogether protocol consists of Prize Pools, Prize Strategies and Prize Pool Builders.  The code can be found on [Github](https://github.com/pooltogether/pooltogether-contracts).

### Prize Pools

[Prize Pools](prize-pool/) wrap Interest Pools with game mechanics.  Users deposits funds in exchange for [Tickets](prize-pool/ticket.md), which places them in an eligibility structure in addition to accounting for their collateral.   The [Prize Strategy](prize-strategy/) for the Pool determines how, when and to whom the prizes go to.

### Prize Strategies

[Prize Strategies](prize-strategy/) distribute prizes.

### Prize Pool Builders

Prize Pool Builders allow users to easily create preconfigured Prize Pools.  There is a builder to create Single Randomly Selected Winner Prize Pools, and one to create Prize Pools with a custom prize strategy.



