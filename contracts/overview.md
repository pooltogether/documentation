---
description: This is an overview
---

# Overview

The core PoolTogether protocol consists of Interest Pools, Prize Pools, and Prize Strategies.

### Interest Pools

[Interest Pools](interestpool.md) allow users to pool their funds to produce interest.  The pooled funds are exchanged for collateral tokens, which allow users to claim their original collateral back.  However, the interest accrued from the pooled funds is distributed according to the address assigned as the "allocator".

### Prize Pools

[Prize Pools](prize-pool/) wrap Interest Pools with game mechanics.  Users deposits funds in exchange for [Tickets](ticket.md), which places them in an eligibility structure in addition to accounting for their collateral.   The [Prize Strategy](prize-strategy/) for the Pool determines how, when and to whom the prizes go to.

### Prize Strategies

Prize Strategies govern how, when and to whom Prize Pools award prizes.

## Conventions

* "Builder" contracts construct many contracts at once.
* "Factory" contracts cheaply create instances of their respective contract using minimal proxies.

