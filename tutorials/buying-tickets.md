---
description: How to Buy Tickets in a PoolTogether Prize Pool
---

# Depositing into a Prize Pool

Let's deposit into a Prize Pool.  Each step of the way we'll show you how to do it in both Solidity and Javascript \(using [Ethers.js](https://docs.ethers.io)\).

First, we'll need to identify what the underlying asset to be deposited is.  If it's a Compound Prize Pool, we'll want to check which cToken it is.

Let's assume we have a Compound Prize Pool that is built against cDai.

## Approve

Let's approve the Prize Pool to spend 1 of our Dai:

**Solidity**

```text
DAI_ERC20.approve(CDAI_PRIZE_POOL, 1 ether);
```

**JavaScript**

```javascript
const ethers = require('ethers')
const signer = ethers.Wallet.createRandom()

const dai = new ethers.Contract(
    DAI_ADDRESS,
    ERC20_ABI,
    signer
)

const daiPrizePool = new ethers.Contract(
    DAI_PRIZE_POOL_ADDRESS,
    PRIZE_POOL_ABI,
    signer
)

await dai.approve(daiPrizePool.address, ethers.utils.parseEther('1'))

```

## Deposit

Now let's deposit into the Prize Pool.  Since we're depositing into a [Compound Prize Pool](../protocol/prize-pool/compound-prize-pool.md) that uses a [Single Random Winner](../protocol/prize-strategy/single-random-winner/) strategy, we'll want to mint Tickets so that we're eligible for prizes.

We can retrieve the Ticket controlled token address from the Single Random Winner prize strategy:

**Solidity**

```javascript
address ticketAddress = SINGLE_RANDOM_WINNER.ticket();
```

**JavaScript**

```javascript
const strategyAddress = await daiPrizePool.prizeStrategy()

const singleRandomWinner = new ethers.Contract(
    strategyAddress,
    SINGLE_RANDOM_WINNER_ABI,
    signer
)

const ticketAddress = await singleRandomWinner.ticket()
```

Now let's deposit to mint tickets for ourselves:

**Solidity**

```javascript
CDAI_PRIZE_POOL.depositTo(
    MY_ADDRESS,
    1 ether,
    ticketAddress,
    address(0)
);    
```

**JavaScript**

```javascript
await daiPrizePool.depositTo(
    signer._address,
    ethers.utils.parseEther(1),
    ticketAddress,
    ethers.constants.AddressZero
)
```

## Depositing Sponsorship

If you wish to deposit and receive sponsorship, or any other controlled token, you simply need to pass it in as the `controlledToken` argument.

## Depositing for Someone Else

If you'd like to deposit on someone else's behalf, you can simply change the `to` address in the call to whomever you want to receive the tickets.  Note that the caller will not be able to withdraw the funds; those funds can only be withdrawn by the recipient unless they increase your allowance.

## Capturing Referral Rewards

The last parameter to the depositTo function is the referral address.  The protocol may drip referral awards globally to Prize Pools.  Referrals can earn tokens based on the fraction of referral volume they supply.

Any interface for a PrizePool will want to pass it's own address as the referrer so that it can capture sweet, sweet rewards.

For more information see [Rewards](../governance/untitled.md)

