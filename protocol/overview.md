---
description: What are No-Loss Prize Games?
---

# 🌐 Overview

No-loss prize games are pools of funds whose accrued interest is distributed as prizes.

The high level protocol architecture is outlined below. The code is available on [Github](https://github.com/pooltogether/pooltogether-pool-contracts).

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
* The prize strategy used to determine frequency and distribution&#x20;
* The rewards offered by the prize pool
* The asset type the prize pool accepts for deposits&#x20;
* The fairness parameters&#x20;

### [Prize Strategies](prize-strategy/)

Prize Strategies determine the prize distribution for the Prize Pool.  They can define any logic to allocate tokens that the prize pool accrues.  Specifically they can:

* Award yield in the Prize Pool as pool tokens
* Award ERC20 tokens held by the Prize Pool
* Award ERC721 tokens held by the Prize Pool

### [Builders](broken-reference)

Builders make it easy to create pre-configured prize games.  There are currently three Prize Pool types paired with the MultipleWinners, documentation available [here](broken-reference).&#x20;

### [Random Number Generator](random-number-generator/)

There are many different ways to generate a random number, so we've abstracted them as request-based Random Number Generator services.  Each RNG service has a different security profile, so be sure to use the appropriate one for your game.

## Conventions

Fixed point math is used extensively in PoolTogether.  We used fixed point math with 18 decimal places for all fractional numbers.  You can think of this as being just like Ether and wei: a value of "1" Ether is represented as "1000000000000000000" wei.

When a number is a fixed point 18 number we always suffix the number with _mantissa._  For example the credit rate is written as _creditRateMantissa_, because it is a fixed point number.

