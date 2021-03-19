---
description: Customize how a Prize Pool distributes prizes
---

# 💸 Prize Strategies

A Prize Strategy handles prize distribution for a [Prize Pool](../prize-pool/).  When a Prize Pool is constructed it is configured with a Prize Strategy.  The Prize Strategy has the privileged ability to award tokens from the Prize Pool.

The most popular Prize Strategy offering is the [Multiple Winner](multiple-winners.md) strategy. Earlier versions \(&lt; v3.1.0\) of the protocol used the Single Random Winner strategy, which is now a trivial subset of Multiple Winners \(with `numberofWinners = 1`\).

Prize Strategies must implement the Token Listener interface so that they can be aware of the full token lifecycle.

See the [Token Listener Interface on Github](https://github.com/pooltogether/token-listener-interface)

## Privileged Actions

A [Prize Pool's](../prize-pool/) Prize Strategy is able to award tokens held by the Prize Pool contract. The Prize Strategy is able to:

* [award yield](../prize-pool/#awarding-yield) that has accrued in the Prize Pool
* [award any ERC20 balance](../prize-pool/#awarding-erc-20-s) held by the Prize Pool
* [award any ERC721](../prize-pool/#awarding-erc-721-s-nfts) owned by the Prize Pool

## Required Behaviour

A Prize Strategy must implement the [Token Listener]() interface so that it can listen to pool token mint, transfer and burn actions by the Prize Pool.

