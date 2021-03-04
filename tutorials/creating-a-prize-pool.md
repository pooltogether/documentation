---
description: How to Create Your Own Prize Pools using the Builder
---

# 🔨 Create a Prize Pool

{% embed url="https://pooltogether.wistia.com/medias/tn44htw6s4" %}

This tutorial will show you how to create a Compound Prize Pool using the [PoolTogether Builder Interface](https://builder.pooltogether.com/).   The Prize Pool will accept Dai, generate yield from Compound, and split the prize among three winners.

## Pool Type

First, select the Yield Prize Pool \(Compound\) since the prize will be the interest from the Dai locked in Compound. The Pool type allows for either a Yield or Stake Prize Pool to be created.

![](https://lh6.googleusercontent.com/WYL5SM0msm37Td5wP-eLpenrwi5VNLvrcSQtgXAVENol7XDKzrwzzs9x_U-hTcQ3Fvc70szZqSN1pcY7uTY0ejhZToK5efdMB2fdTaoU2oc3Boy-GwykbOxrT5uD_QcT0N8dYC8G)

## Deposit Token

The deposit token will be the token that users stake into the Prize Pool.  For Compound Prize Pools, you are limited to the available Compound Markets.  For Stake Prize Pools you can use any ERC20.  

Select Dai as the token that will be staked in the Prize Pool.

When a user deposits into a Prize Pool, they will get a token in return.  This token represents the underlying collateral at 1:1 exchange rate.

The Builder will create a Prize Pool with two types of tokens: Ticket and Sponsorship.  Ticket tokens represent collateral and the chance to win a prize.  The chance to win is proportional to the number of tickets held.  Sponsorship tokens represent collateral only.  Sponsorship tokens are useful when you want to sponsor a prize.

The name and symbol for the Ticket and Sponsorship tokens are filled with presets, but can be configured.

![](../.gitbook/assets/screen-shot-2021-01-05-at-5.11.24-pm.png)

## RNG Service

A Random Number Generator Service is used to randomly select the winners.  Currently there are two options: the [Blockhash RNG](../protocol/random-number-generator/blockhash.md) and the [Chainlink RNG](../protocol/random-number-generator/chainlink-vrf.md).

The Blockhash RNG is free to use, although it is the least secure method of randomness.

The Chainlink VRF has better security guarantees, but requires **2 LINK per RNG request**.  If you use this RNG, you must 🚨**supply LINK to the Prize Strategy** 🚨.  Note that you must supply LINK to the prize strategy, not the prize pool.

![](https://lh3.googleusercontent.com/f6WerG_nQmbkff2v5Pny0aIH9rF4IqAQgsX7pKCBWSwoGN13GaJ__kcqn7_131wRt2nsOTVvOZ5J5rQ_ZCTCdBnt1v-4xWqEcXGMV0YJ4UTTbX4rvUwsxuYNwrqGvmyZIVk3HvHx)

## Prize Period

This will be a weekly awarded prize, so let's set the Prize Period to 7 days. This means that a minimum of 7 days must have elapsed since the previous prize before the next prize can be awarded.  The start time will be the time at which the pool is created.

![](https://lh6.googleusercontent.com/9_CNInN-U3wblarNhPQm66n_9Nh-LoSvBo9N53c6WAQntgtAG6lFiKe5qJr72yp9yjsPPhX8ji5wBzQmIgvoBDd4U7WACDanUgpgY-H51VfFudCj6KHippnT5b2XhM0oc6yyCDbZ)

## Number of Winners

The Prize Pool is created with the [Multiple Winners](../protocol/prize-strategy/multiple-winners.md) prize strategy that splits the prize among multiple winners.  Note that the contract will transfer the tokens to each of the winners, so the more winners you have the higher the gas cost of rewarding will be.

![](../.gitbook/assets/screen-shot-2021-01-05-at-4.39.03-pm.png)

## Fairness

It's possible for a user to deposit immediately before the prize is awarded, then withdraw immediately after.  To mitigate this, prize pools impose an early exit fee to ensure fairness.

The early exit fee is the percent of the withdrawal amount that must be contributed to the prize.  In this case the early exit fee is 1%, so upon withdrawal a user must contribute 1% of the withdrawal amount to the prize.  However, the exit fee decays over time to zero, so that most users can withdraw instantly without any fee.  In this example the exit fee decays to zero over 14 days.  

**Please view the** [**Fairness**](https://docs.pooltogether.com/protocol/prize-pool/fairness) **section of the documentation for more details.**

![](../.gitbook/assets/screen-shot-2021-01-05-at-5.08.06-pm.png)

## Now Create Your Pool!

Push the big button at the bottom.  You're all done!  You're now the proud owner of a Compound Prize Pool.  Ownership comes with special privileges, so that the pool can be managed properly.

To find out how to operate a pool, see the tutorial [Operate a Prize Pool](operate-a-prize-pool/)

