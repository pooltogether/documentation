---
description: 'Creates a new, uninitialized Prize Pool cheaply'
---

# Prize Pool Factory

The Prize Pool Factory can create a new Prize Pool contract very cheaply.  The factory deploys a minimal proxy contract that points to a deployed Prize Pool implementation.  The implementation is not upgradeable, so neither is the proxy.

The factory itself is upgradeable, however, by the PoolTogether team.

To create a new, uninitialized Prize Pool call:

```javascript
function createPrizePool() external returns (PrizePool)
```

You will need to initialize the Prize Pool itself after:

```javascript
contract PrizePool {
    function initialize (
        Ticket _ticket,
        InterestPoolInterface _interestPool,
        PrizeStrategyInterface _prizeStrategy
    ) public;
}
```

The Prize Pool must be initialized with the [Ticket](ticket.md) token, an [Interest Pool](interestpool.md), and a [Prize Strategy](../prize-strategy/).  The ticket token's controller must be the newly created Prize Pool.

