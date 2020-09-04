---
description: Customize how a Prize Pool distributes prizes
---

# 💸 Prize Strategies

A Prize Strategy handles prize distribution for a [Prize Pool](../prize-pool/).  When a Prize Pool is constructed it is configured with a Prize Strategy.  The Prize Strategy has the privileged ability to award tokens from the Prize Pool.

The first Prize Strategy that PoolTogether is offering is the [Single Random Winner](single-random-winner/) strategy.

Prize Strategies must implement the [Token Listener](../prize-pool/token-listener.md) interface so that they can be aware of the full token lifecycle.



