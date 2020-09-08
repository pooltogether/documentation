---
description: How to Withdraw Funds from a Prize Pool
---

# Withdrawing from a Prize Pool

When it comes time to withdraw funds from a Prize Pool you can withdraw funds for yourself, or for others if they have given you an allowance.

If the funds have [**matured**](https://docs.pooltogether.com/protocol/fairness#credit), then they can be withdrawn fully and instantly.  If they haven't, then the funds will first enter a timelock before being unlocked and available to sweep back to the user.  Alternatively, you can "pay off" the timelock and forfeit a small fee that goes to the prize.

Let's see how these two approaches work.

## Withdrawing Your Funds With a Timelock

Assuming you have previously [deposited into a Prize Pool](buying-tickets.md), let's see how to withdraw funds.

The primary means of withdrawal is using a timelock.  Most people will have accrued enough credit such that the timelock duration will be zero, but they still have to call the same function.

**Solidity**

```javascript
uint256 unlockTimestamp = PRIZE_POOL.withdrawWithTimelockFrom(
    fromAddress,
    amount,
    controlledToken
);
```

* **fromAddress** is the address from which we are withdrawing funds.  This address must have a balance of the **controlledToken** greater than the given **amount**.  This address must either be the caller, or the caller must an allowance greater than or equal to the amount for the given controlledToken. 
* **amount** is the amount of controlledTokens to burn in exchange for funds.  Pool tokens are exchanged 1:1 with the underlying asset supplied to [depositTo\(\)](buying-tickets.md)
* **controlledToken** is the address of the pool token you wish to burn.

**JavaScript**

```javascript
const ethers = require('ethers')
const signer = ethers.Wallet.createRandom()

const daiPrizePool = new ethers.Contract(
    DAI_PRIZE_POOL_ADDRESS,
    PRIZE_POOL_ABI,
    signer
)

const controlledTokens = await daiPrizePool.tokens();

await daiPrizePool.withdrawWithTimelockFrom(
    signer._address, 
    ethers.utils.parseEther('1'),
    controlledTokens[0]
)

const unlockTimestamp = await daiPrizePool.balanceAvailableAt(signer._address)   
```

## Withdrawing Funds Instantly

If the user doesn't have sufficient [credit](../protocol/fairness.md#credit) to cover the timelock they can "pay off" the timelock by contributing to the prize.

**Solidity**

```javascript
uint256 exitFee = PRIZE_POOL.calculateEarlyExitFee(
    controlledToken,
    amount
);

uint256 actualExitFee = PRIZE_POOL.withdrawInstantlyFrom(
    fromAddress,
    amount,
    controlledToken,
    exitFee
);
```

* **from** is the address to withdraw from.  This must be the caller, or the caller must have receive prior approval via the controlled token.
* **amount** is the amount of controlled token to burn.
* **controlledToken** is the pool token the user wishes to burn in exchange for assets.
* **maximumExitFee** allows the caller to declare the maximum fee that is taken from their withdrawal amount.

This function returns the actual exit fee that was consumed.  The exit fee is taken from the withdrawal amount.  For example, if the user is withdrawing 100 Dai and the exit fee is 10 Dai, then they will burn 100 pool tokens and be transferred 90 Dai.

**JavaScript**

```javascript
const ethers = require('ethers')
const signer = ethers.Wallet.createRandom()

const daiPrizePool = new ethers.Contract(
    DAI_PRIZE_POOL_ADDRESS,
    PRIZE_POOL_ABI,
    signer
)

const controlledTokens = await daiPrizePool.tokens();

const exitFee = await daiPrizePool.calculateEarlyExitFee(
    controlledTokens[0],
    ethers.utils.parseEther('1')
)

await daiPrizePool.withdrawInstantlyFrom(
    signer._address, 
    ethers.utils.parseEther('1'),
    controlledTokens[0],
    exitFee
)

```

## Getting Approval

Both methods of withdrawal allow users to withdraw on behalf of others, as long as they have sufficient ERC20 allowance for the user and controlled token in question.

The funds will be transferred back to the original user, not to the caller.

