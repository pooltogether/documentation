---
description: What are Prize Games?
---

# 🌐 Overview

Prize games are pools of funds whose accrued interest is distributed as prizes.

The code is available on [Github](https://github.com/pooltogether/pooltogether-pool-contracts).

## How it works

1. Users deposit funds into a Prize Pool.  They receive pool tokens in exchange.
2. The funds earn interest.
3. The interest is distributed by the Prize Strategy as pool tokens.
4. Users withdraw their funds at any time by telling the Prize Pool to burn their pool tokens.

## Architecture

### [Prize Pools](prize-pool/)

Prize Pools are the central building block of prize games.  They pool user funds in a **yield source** and expose the yield to their **Prize Strategy**, which then disburses as desired.

Prize Pools can be differentiated in four primary ways:

* The yield source the prize pool uses to generate no loss return
* The prize strategy used to determine frequency and distribution 
* The rewards offered by the prize pool
* The asset type the prize pool accepts for deposits 
* The fairness parameters 

### [Prize Strategies](prize-strategy/)

Prize Strategies determine the prize distribution for the Prize Pool.  They can define any logic to allocate tokens that the prize pool accrues.  Specifically they can:

* Award yield in the Prize Pool as pool tokens
* Award ERC20 tokens held by the Prize Pool
* Award ERC721 tokens held by the Prize Pool

### [Builders](builders/)

Builders make it easy to create pre-configured prize games.  The first builder offered by PoolTogether is the [Compound Single Random Winner Builder](builders/compound-prize-pool-builder.md), which creates a [Compound Prize Pool](prize-pool/compound-prize-pool.md) bound to a [Single Random Winner](prize-strategy/single-random-winner/) prize strategy and a [Ticket](prize-strategy/single-random-winner/ticket.md) token as the pool token.

### [Random Number Generator](random-number-generator/)

There are many different ways to generate a random number, so we've abstracted them as request-based Random Number Generator services.  Each RNG service has a different security profile, so be sure to use the appropriate one for your game.

### [Comptroller](comptroller.md)

Comptrollers make it simple to "drip" tokens to players.  Comptrollers listen for token mints, transfers and burns and drip tokens accordingly.  The global PoolTogether governance comptroller is baked into every Prize Pool, so rewards can be dripped to all prize games.

## Conventions

Fixed point math is used extensively in PoolTogether.  We used fixed point math with 18 decimal places for all fractional numbers.  You can think of this as being just like Ether and wei: a value of "1" Ether is represented as "1000000000000000000" wei.

When a number is a fixed point 18 number we always suffix the number with _mantissa._  For example the credit rate is written as _creditRateMantissa_, because it is a fixed point number.

## Gas Station Network

PoolTogether supports the [Gas Station Network 2.0](https://github.com/opengsn/gsn).  The Gas Station Network is an open source system for meta-transactions on Ethereum.  The GSN consists of smart contracts, relayers, and a protocol to support a decentralized meta transaction relayer network.

The PoolTogether smart contracts support GSNv2 by inheriting from the [BaseRelayRecipient](https://github.com/opengsn/gsn/blob/master/contracts/BaseRelayRecipient.sol) contract and being bound to a trusted forwarder.  The contracts are tooled so that they support GSN transactions.

