---
description: Pool deposits and award accrued interest periodically as a prize
---

# 🏆 Prize Pool

## Introduction

Prize Pools are general-purpose contracts that allow users to safely deposit collateral into a yield-generating service and have the accrued interest disbursed by a separate Prize Strategy contract. PoolTogether currently provides a Compound Prize Pool that uses Compound as the yield generating service.  More are coming!

The Prize Pool is not upgradeable and has no admin controls beyond an emergency shutdown function.

When the Prize Pool is created it must be initialized with a set of controlled tokens.  The Prize Pool is able to mint and burn these tokens as needed; it is their Token Controller.  The default [Compound Prize Pool Builder](../builders.md) creates a Ticket controlled token and a Sponsorship controlled token.  These tokens can be looked up on the corresponding [Prize Strategy](../prize-strategy.md).

## Depositing

Users can deposit into the Prize Pool using the **depositTo** function. 

```javascript
function depositTo(
    address to,
    uint256 amount,
    address controlledToken,
    bytes calldata data
) external;
```

| Parameter | Description |
| :--- | :--- |
| to | The address to whom the controlled tokens should be minted |
| amount | The amount of the underlying asset the user wishes to deposit.  The Prize Pool contract should have been pre-approved by the caller to transfer the underlying ERC20 tokens. |
| controlledToken | The address of the token that they wish to mint.  For our default Prize Strategy this will either be the Ticket address or the Sponsorship address.  Those addresses can be looked up on the Prize Strategy. |
| data | Additional call data that can be passed to the Prize Strategy.  Typically this includes referral information. |

Depositing fires the event:

```javascript
event Deposited(
    address indexed operator,
    address indexed to,
    address indexed token,
    uint256 amount
);
```

| Event Data | Description |
| :--- | :--- |
| operator | The address that made the deposit |
| to | The address that received the minted controlled tokens |
| token | The address of the controlled token that was minted |
| amount | The amount of both the underlying asset that was transferred and the tokens that were minted. |

## Withdrawing

Collateral can be withdrawn in two ways: the withdrawal can be redeemed losslessly after a timelock expires, or the user can pay an early exit fee.

### Lossless Redemption

Collateral can be withdrawn without any fees by time-locking the funds.  The withdrawal amount will be unlocked at a later date at which point the funds can be swept back to the user.  The timelock duration is determined by the [Prize Strategy](../prize-strategy.md).

To start a lossless withdrawal a user may call:

```javascript
function withdrawWithTimelockFrom(
    address from,
    uint256 amount,
    address controlledToken,
    bytes calldata data
) external returns (uint256 unlockTimestamp);
```

| Parameter Name | Parameter Description |
| :--- | :--- |
| from | The user from whom to withdraw.  This means you may withdraw on another user's behalf if they have given you an ERC20 allowance. |
| amount | The amount of collateral to withdraw. |
| controlledToken | The type of controlled token to withdraw. |
| data | Call data that is passed on to the Prize Strategy. |

### Sweeping Funds

When a user's withdrawal timelocks have ended, the funds may be swept to their wallets:

```javascript
function sweepTimelockBalances(
    address[] memory users
) external returns (uint256 totalWithdrawal);
```

The function accepts an array of addresses and will attempt to sweep the time-locked funds for each one.  The funds will be transferred back to the users wallets.

After funds have been timelocked, you can see when they'll be available:

```javascript
function timelockBalanceAvailableAt(address user) external view returns (uint256)
```

### Instant Withdrawal

If a user would like their tickets right away, they may pay an early exit fee to the prize.  The early exit fee is determined by the [Prize Strategy](../prize-strategy.md).

To withdraw instantly:

```javascript
function withdrawInstantlyFrom(
    address from,
    uint256 amount,
    address controlledToken,
    uint256 sponsorAmount,
    uint256 maximumExitFee,
    bytes calldata data
  )
    external
    returns (uint256 exitFee);
```

| Parameter Name | Parameter Description |
| :--- | :--- |
| from | The address to withdraw from.  This means you can withdraw on another user's behalf if you have an allowance for the controlled token. |
| amount | The amount to withdraw |
| controlledToken | The controlled token to withdraw from |
| sponsorAmount | The amount of the early exit fee the caller wishes to cover themselves.  They should have approved of the Prize Pool spending their funds prior to calling withdraw. |
| maximumExitFee | The maximum early exit fee the caller is willing to pay.  This prevents the Prize Strategy from changing the fee on-the-fly. |
| data | Call data that is passed on to the Prize Strategy. |



