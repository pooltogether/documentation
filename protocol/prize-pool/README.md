---
description: Pool deposits and award accrued interest periodically as a prize
---

# 🏆 Prize Pools

## Introduction

Prize Pools allow funds to be pooled together into a no-loss yield source, such as Compound, and have the yield safely exposed to a separate Prize Strategy. They are the primary way through which users interact with PoolTogether prize games.

Prize Pools provide controls to the owner so that participation can be made fair. See [Fairness](https://app.gitbook.com/s/-M58QPye9-PujrSjSWqv-887967055/protocol/fairness.md) for more information.

There is a different type of prize pool for each yield source. For example, if you wish to use Compound you will use the Compound Prize Pool.

All Prize Pools share the functionality below.

## Owner

When a Prize Pool is created, the creator is set as the pool's "owner". The owner is able to:

* Add additional pool tokens
* Change the Prize Strategy
* Set the [credit rate and credit limit](https://app.gitbook.com/s/-M58QPye9-PujrSjSWqv-887967055/protocol/fairness.md#credit)
* Shutdown the prize pool
* Transfer ownership
* Renounce ownership

**The prize pool is not upgradeable and therefore the owner can never seize the funds deposited into the prize pool**

## Limits

When a Prize Pool is created it is initialized with some hard-coded limits to protect users. See [Fairness](https://app.gitbook.com/s/-M58QPye9-PujrSjSWqv-887967055/protocol/fairness.md) for more details.

### **Maximum Timelock Duration**

The maximum timelock duration ensures that a user has to wait at most X amount of time to withdraw their funds loss-lessly. If the owner of a pool sets the credit rate to be way too low, this limit ensures users will still be able to withdraw.

If using the Single Random Winner Prize Strategy, it would make sense to set the maximum timelock duration to 2x the prize period. That way the owner has some flexibility when adjusting the credit limit and credit rate.

### **Maximum Credit Limit**

The maximum credit limit ensures that the credit limit cannot be set higher than this number. This prevents the owner of the Prize Pool from capturing \*all\* of a user's deposit at withdrawal time.

### **Maximum Liquidity Limit**

The maximum liquidity limit allows the PrizePool owner to set a cap on the amount of liquidity the pool can hold. This can be set by calling:

```javascript
function setLiquidityCap(uint256 _liquidityCap) external override onlyOwner
```

## Token Model

A Prize Pool accepts a single type of ERC20 token for deposits. This token depends on the implementation: for a Compound Prize Pool bound to cDai it will be Dai, for a yEarn yUSDC vault it will be USDC. This is the underlying **asset** of the Prize Pool.

Prize Pools use **Controlled Tokens** for their internal accounting. These tokens are minted when depositing or awarding prizes. Controlled Tokens are burned when users withdraw. They are exchanged at a ratio of 1:1 to the asset.

The tokens associated with a PrizePool can be seen by calling:

```javascript
function tokens() external override view returns (address[] memory)
```

### Controlled Tokens

A Controlled Token is a standard ERC20 that is bound to a **Token Controller**.

The Token Controller has the privileged ability to mint and burn tokens on user's behalf, and has a callback that listens for token transfers. Controlled Tokens are expected to trigger this callback on any mint, burns or transfers.

The Prize Pool must be the Token Controller for the controlled tokens that it is initialized with at construction.

The default [Compound Prize Pool Builder](https://app.gitbook.com/s/-M58QPye9-PujrSjSWqv-887967055/builders/) creates a Ticket controlled token and a [Sponsorship](../tokens/sponsorship.md) controlled token.

A Controlled Token can added by the PrizePool owner by calling:

```javascript
function addControlledToken(ControlledTokenInterface _controlledToken) 
external override onlyOwner
```

### Minting

When a user deposits into a Prize Pool they must request what type of controlled token they receive in exchange. This token will be minted to them at an exchange rate of 1:1 for the asset.

### Burning

When a user wishes to withdraw from a Prize Pool they must burn controlled tokens.

## Depositing

Users can deposit into the Prize Pool using the **depositTo** function. A user is instantly minted tokens upon deposit.

```javascript
function depositTo(
    address to,
    uint256 amount,
    address controlledToken,
    address referrer
) external;
```

| Parameter       | Description                                                                                                                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| to              | The address to whom the controlled tokens should be minted                                                                                                                                                   |
| amount          | The amount of the underlying asset the user wishes to deposit.  The Prize Pool contract should have been pre-approved by the caller to transfer the underlying ERC20 tokens.                                 |
| controlledToken | The address of the token that they wish to mint.  For our default Prize Strategy this will either be the Ticket address or the Sponsorship address.  Those addresses can be looked up on the Prize Strategy. |
| referrer        | The address that should receive [referral awards](https://app.gitbook.com/s/governance/untitled.md#referral-volume-drips), if any.                                                                           |

Depositing fires the event:

```javascript
event Deposited(
    address indexed operator,
    address indexed to,
    address indexed token,
    uint256 amount
);
```

| Event Data | Description                                                                                   |
| ---------- | --------------------------------------------------------------------------------------------- |
| operator   | The caller that made the deposit                                                              |
| to         | The address that received the minted tokens                                                   |
| token      | The address of the controlled token that was minted                                           |
| amount     | The amount of both the underlying asset that was transferred and the tokens that were minted. |

### Depositing Using Timelocked Funds

If a user wishes to re-deposit their timelocked funds, they can do so using this function:

```javascript
function timelockDepositTo(
    address to,
    uint256 amount,
    address controlledToken
) external;
```

| Parameter       | Description                                                                                                                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| to              | The address to whom the controlled tokens should be minted                                                                                                                                                   |
| amount          | The amount of the underlying asset the user wishes to deposit.  The Prize Pool contract should have been pre-approved by the caller to transfer the underlying ERC20 tokens.                                 |
| controlledToken | The address of the token that they wish to mint.  For our default Prize Strategy this will either be the Ticket address or the Sponsorship address.  Those addresses can be looked up on the Prize Strategy. |

## Withdrawing

When a user withdraws they may need to contribute to the prize according to the [fairness rules](https://app.gitbook.com/s/-M58QPye9-PujrSjSWqv-887967055/protocol/fairness.md). They may either cover the contribution by time-locking their funds, or cover the contribution explicitly using funds.

### **Withdraw with Timelock**

Funds can be withdrawn losslessly by time-locking the funds. The withdrawal amount will be unlocked at a later date at which point the funds can be swept back to the user. The timelock duration is calculated based on the users accrued credit, the credit rate, and the fairness fee.

If the user has sufficient credit, the unlockTimestamp may be "now" and the funds are instantly swept to the `from` address.

Tip: You can call this function in a constant way to see when the users funds will be unlocked.

To start a lossless withdrawal a user may call:

```javascript
function withdrawWithTimelockFrom(
    address from,
    uint256 amount,
    address controlledToken
) external returns (uint256 unlockTimestamp);
```

| Parameter Name  | Parameter Description                                                                                                            |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| from            | The user from whom to withdraw.  This means you may withdraw on another user's behalf if they have given you an ERC20 allowance. |
| amount          | The amount of collateral to withdraw.                                                                                            |
| controlledToken | The type of controlled token to withdraw.                                                                                        |

### Checking Timelock Balances

To see how many funds have been timelocked for a `user` call:

```javascript
function timelockBalanceOf(address user) external view returns (uint256)
```

After funds have been time-locked, you can see at what timestamp they'll be available:

```javascript
function timelockBalanceAvailableAt(address user) external view returns (uint256)
```

### Checking Timelock Duration

To calculate a timelocked withdrawal duration and credit consumption call:

```javascript
function calculateTimelockDuration(address from, address controlledToken, uint256 amount) 
external override returns (uint256 durationSeconds,uint256 burnedCredit)
```

| Parameter Name  | Description                               |
| --------------- | ----------------------------------------- |
| from            | The user who is withdrawing.              |
| amount          | The amount the user is withdrawing.       |
| controlledToken | The type of controlled token to withdraw. |

**returns**:

| Returned Parameter Name | Description                               |
| ----------------------- | ----------------------------------------- |
| `durationSeconds`       | The duration of the timelock in seconds   |
| `burned`                | The amount of credit that would be burned |

### Estimating Credit Accrual Time

Similarly it is also possible to calculate how long a user must keep their funds in the pool:

```javascript
function estimateCreditAccrualTime(address _controlledToken,
 uint256 _principal,
 uint256 _interest) 
 external override view returns (uint256 durationSeconds)
```

| Parameter Name    | Parameter Description                               |
| ----------------- | --------------------------------------------------- |
| \_controlledToken | The type of controlled token.                       |
| \_principal       | The principal amount on which interest is accruing. |
| \_interest        | The amount of interest that must accrue.            |

### Sweeping Timelocked Funds

When a user's withdrawal timelocks have ended, the funds may be swept to their wallets:

```javascript
function sweepTimelockBalances(
    address[] memory users
) external returns (uint256 totalWithdrawal);
```

The function accepts an array of addresses and will attempt to sweep the time-locked funds for each one. The funds will be transferred back to the users wallets.

### Withdraw Instantly

If a user would like their tickets right away, they may pay an early exit fee to the prize. The early exit fee is determined by the [Prize Strategy](https://app.gitbook.com/s/-M58QPye9-PujrSjSWqv-887967055/prize-strategy/).

The instant withdrawal function returns the amount of the withdrawal that was retained as payment. This means you can call this function in a constant way to check to see what the exit fee will be. When it comes time to run the tx, that exit fee can be passed as the `maximumExitFee` to ensure it doesn't exceed the expected limit.

```javascript
function withdrawInstantlyFrom(
    address from,
    uint256 amount,
    address controlledToken,
    uint256 maximumExitFee
  )
    external
    returns (uint256 exitFee);
```

| Parameter Name  | Parameter Description                                                                                                                  |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| from            | The address to withdraw from.  This means you can withdraw on another user's behalf if you have an allowance for the controlled token. |
| amount          | The amount to withdraw                                                                                                                 |
| controlledToken | The controlled token to withdraw from                                                                                                  |
| maximumExitFee  | The maximum early exit fee the caller is willing to pay.  This prevents the Prize Strategy from changing the fee on-the-fly.           |

This early exit fee can also be calculated by calling:

```javascript
function calculateEarlyExitFee(address from, address controlledToken, uint256 amount)
 external override returns (uint256 exitFee, uint256 burnedCredit)
```

| Parameter Name  | Parameter Description                 |
| --------------- | ------------------------------------- |
| from            | The address to withdraw from          |
| controlledToken | The controlled token to withdraw from |
| amount          | The amount to withdraw                |

returns the `exitFee` that would be paid along with the credit that would be burned (`burnedCredit`).

## Awarding

Only the Prize Strategy can call the award functions. These functions allow prizes to be disbursed to users.

### Awarding Yield

Yield that accrues in the Prize Pool can be awarded by the Prize Strategy. The yield must first be **captured** and then it can be **awarded.**

To capture the yield the prize strategy can call the `captureAwardBalance` function:

```javascript
function captureAwardBalance() external onlyPrizeStrategy returns (uint256);
```

This function will:

* add the current yield balance to the available award balance
* capture a portion for the reserve
* return the total available award balance.

To award the captured yield to an address, the Prize strategy uses the `award` function. The yield must be awarded as one of the controlled tokens configured in the Prize Pool.

```javascript
function award(
    address to,
    uint256 amount,
    address controlledToken
) external onlyPrizeStrategy;
```

| Parameter Name  | Parameter Description                          |
| --------------- | ---------------------------------------------- |
| to              | The address to receive the newly minted tokens |
| amount          | The amount of tokens to mint                   |
| controlledToken | The type of token to mint                      |

### Awarding ERC20s

The Prize Strategy can award ERC20 tokens that are held by the Prize Pool.

```javascript
function awardExternalERC20(
    address to,
    address externalToken,
    uint256 amount
) external onlyPrizeStrategy;
```

However, some tokens are be blacklisted if they need to be held to generate yield (i.e. Compound cTokens).

| Parameter Name | Parameter Description               |
| -------------- | ----------------------------------- |
| to             | The address to receive the transfer |
| externalToken  | The ERC20 to transfer               |
| amount         | The amount of tokens to transfer    |

### Awarding ERC721s (NFTs)

The Prize Strategy can award ERC721 tokens that are held by the Prize Pool.

```javascript
function awardExternalERC721(
    address to,
    address externalToken,
    uint256[] calldata tokenIds
  )
    external
    onlyPrizeStrategy;
```

| Parameter Name | Parameter Description           |
| -------------- | ------------------------------- |
| to             | The address to receive the NFTs |
| externalToken  | The ERC721 contract address     |
| tokenIds       | The NFT token ids to transfer.  |

## Credit

Credit accrues differently for each of the Prize Pool's controlled tokens, so each token will have its own credit rate and credit limit.

### Credit Balance

To get a users credit balance for a controlled token:

```javascript
function balanceOfCredit(
    address user,
    address controlledToken
) external returns (uint256);
```

| Parameter Name  | Parameter Description                                   |
| --------------- | ------------------------------------------------------- |
| user            | The user whose credit balance should be returned        |
| controlledToken | The token for which the credit balance should be pulled |

### Credit Rate

The credit rate for a controlled token can be checked like so:

```javascript
function creditRateOf(
    address controlledToken
) external view returns (
    uint128 creditLimitMantissa,
    uint128 creditRateMantissa
);
```

| Parameter Name  | Parameter Description                                                |
| --------------- | -------------------------------------------------------------------- |
| controlledToken | The controlled token whose credit limit and rate should be returned. |

Note that the returned values are "mantissas": i.e. fixed point numbers with 18 decimal places.

### Credit Plan

The credit plan associated with a `controlledToken` can be found by calling:

```javascript
function creditPlanOf(address controlledToken) external override view returns (uint128 creditLimitMantissa, uint128 creditRateMantissa)
```

## Prizes

### Current Award Balance

The following function returns the amount calculated by `captureAwardBalance()`:

```javascript
function awardBalance() external override view returns (uint256)
```

### Total Balances

The total of all controlled tokens (including timelocked) can be obtained by calling:

```javascript
function accountedBalance() external override view returns (uint256)
```

The total underlying balance of all assets (including both principal and interest) can be obtained by calling:

```javascript
function balance() external returns (uint256)
```

## External Prizes

### Adding Tokens

The owner can add "external" ERC20 tokens as prizes. The strategy will award the entire balance held by the Prize Pool to the winner.

```javascript
function addExternalErc20Award(address _externalErc20) external onlyOwner;
```

The owner can add "external" ERC721 tokens as prizes. These tokens will be transferred to the winner.

```javascript
function addExternalErc721Award(
    address _externalErc721,
    uint256[] calldata _tokenIds
) external onlyOwner
```

### Checking Tokens

Checks with the Prize Pool if a specific token type (`_externalToken`) may be awarded as an external prize:

```javascript
function canAwardExternal(address _externalToken) external view returns (bool)
```

## Prize Time Periods

To retrieve when the current prize started:

```javascript
function prizePeriodStartedAt() external view returns (uint256)
```

To retrieve when the prize will end:

```javascript
function prizePeriodEndAt() external view returns (uint256)
```

## Reserve

### Calculate Reserve Fee

Calculates the reserve portion of the given `amount` of funds. If there is no reserve address, the Reserve fee portion will be zero.

```javascript
function calculateReserveFee(uint256 amount) public view returns (uint256)
```

## Prize Strategy

### Set the Prize Strategy

The associated Prize Strategy can be set by calling:

```javascript
function setPrizeStrategy(TokenListenerInterface _prizeStrategy) external override onlyOwner
```

Only the Prize Pool owner can call this function.

