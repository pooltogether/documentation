---
description: Illustrating governance process with examples
---

# 🗳️ Process

The governance system is very new, so it might be difficult for some people to imagine how it works.  Here we're going to give some example proposals to illustrate how governance could work.  The examples will be both PoolTogether-specific and refer to proposals created in other governance systems.  We'll cover:

* How to create a new protocol-owned prize pool
* How to create a Uniswap-style grants program
* Rewarding contributors with Sablier streams

It's important to mention that a proposal is much more likely to be successful if it is first discussed in the [governance forum](https://gov.pooltogether.com).  Ideally the outcome of a proposal will be known before it is created.

## Proposal: Create a Protocol Prize Pool

As new assets become available and new types of prize pools are added to the [Builder](broken-reference), users may wish to create new governance owned & operated prize pools.  By having ownership only governance will be able to change parameters such as the exit fee and number of winners.  See [Controls](controls.md) for more info.

If POOL holders decide to create a new prize pool after thorough discussion on the governance forums, then they would need to follow these steps:

1. A user creates the appropriate prize pool using the [Prize Pool Builder app](https://builder.pooltogether.com).
2. Once created, the user transfers the ownership of the resulting prize pool and prize strategy contracts to the Timelock contract (see the Governance section in [Networks](../networks.md)).  The only interface for this right now is Etherscan.
3. Finally, the user creates a new governance proposal.  The proposal will include:
   1. Adding the prize pool address to the official Protocol Prize Pool Registry (coming soon!)
   2. Possible compensation for the gas spent by the user that created the pool
   3. Possible compensation for the gas costs of creating the proposal

POOL holders will need to verify that the ownership of the proposed prize pool has, in fact, been transferred to the Timelock contract.  They should also verify that the prize pool is safe and has been created by the builder app.

## Proposal: Create a Grants Program

Many protocols have created a grants program to make it easy to fund the protocol ecosystem.  Typically, grant programs have a trustworthy steward to manage the program.  The [Uniswap Grant Proposal](https://app.uniswap.org/#/vote/3) is a great example.

For Uniswap, a professional grants manager was selected to lead the program.  Him and five other people were added to a Gnosis Safe multisig.  The other people were well-known leaders in the crypto space and proved that they held the wallet addresses.  The multisig was configured to require 4-of-6 confirmations, making it quite secure.

The proposal included:

* Quarterly budget for grants, with two quarters of budget requested at the time of proposal.
* Compensation for the grants manager
* A complete description of the proposed grants program, including timeline, budget and scope.

The actual proposal was a simple token transfer from the treasury to the Gnosis Safe multisig.

## Proposal: Reward Contributors with a Sablier Stream

SushiSwap has formalized their hiring guidelines, and as part of those guidelines new hires will be paid a signing bonus and their "salary" will be sent to them as a Sablier stream.  You can read their [complete hiring process here](https://forum.sushiswapclassic.org/t/sushi-hiring-guidelines-v2/1866).

If a community member wished to apply to work for the protocol and have a salary of X tokens, they could set up a proposal like so:

1. Approve Sablier spending X tokens
2. Create a new stream in Sablier for X tokens for the given timeframe.
3. Include token transfer as a signing bonus (if applicable)

