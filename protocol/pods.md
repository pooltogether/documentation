---
description: Combine tickets and split the prize
---

# 🐋 Pods

A "Pod" is a smart contract that allows users to combine their deposits together for a higher chance to win.  If the Pod wins, users get to split the prize according to how much they contributed.

Pods introduce two major enhancements:

* Reduced gas costs when entering PrizePools via batching.
* Increased winning odds through collective deposits.

Relative to the traditional PoolTogether deposits, Pods offer a unique value proposition that may appeal to a range of users. Whether it's a small "fish" just trying to spend less on gas or a "whale" interested in increasing their chances of winning \(_while also sharing their winnings with others_\) Pods introduce a novel set of features for participating in a no-loss lottery.

**Primary Smart Contracts**

* [Pod.sol](https://github.com/pooltogether/pods-v3-contracts/blob/master/contracts/Pod.sol)
* [TokenDrop.sol](https://github.com/pooltogether/pods-v3-contracts/blob/master/contracts/TokenDrop.sol)
* [PodFactory.sol](https://github.com/pooltogether/pods-v3-contracts/blob/master/contracts/PodFactory.sol)
* [TokenDropFactory.sol](https://github.com/pooltogether/pods-v3-contracts/blob/master/contracts/TokenDropFactory.sol)

#### OpenZeppelin Inheritance

Pods inherit functionality from OpenZeppelin smart contracts: _ERC20Upgradeable, OwnableUpgradeable, ReentrancyGuardUpgradeable._

**Module:** __@openzeppelin/contracts-upgradeable": "^3.4.0"

## Overview

### Sharing Tickets & External ERC20/ERC721 Winnings

**The Pod is designed to distribute PrizePool winnings i.e. tokens/tickets.**

_Secondary awards \(LOOT Box\) are liquidated and converted to the underlying balance._ ****

In other words, if a Pod contains 1,000,000 tokens \(for example $1,000,000 ****worth of USDC\) split evenly between 10 depositors \(100,000 each\) and the Pod is awarded 50,000 pcUSDC, each depositor will have their underlying balance increase by 5,000 USDC \(10% of the total winnings\).

Distribution of non-ticket winnings, such as external ERC20 and ERC721 tokens is handled via liquidation and conversion to the underlying balance token. Simply put, as of now Pods \(v1\) instead of splitting secondary awards \(_which is not possible for non-fungible tokens_\) the Pod manager liquidates the LOOT Box and converts the assets into the underlying token or reward ticket.

The liquidated/converted assets can be withdrawn by Pod share holders.

In short, **all winnings** are ultimately available as the **deposit token**.

_Example:_

* Pod awarded 50,000 ticket prize
* Pod awarded LOOT Box \(external ERC20/ERC721 tokens\)
* Pod liquidates LOOT Box assets into underlying asset \(token\)
* Underlying asset distributed across Pod holders  

### Owner & Manager

Pods include two roles: _owner and manager_. The owner is responsible for updating \(if necessary\) external contract references and the manager is responsible for liquidating and distributing LOOT Box winnings.

#### Owner

The owner is responsible for updating external contract references.

**Manager**

The manager is responsible for liquidating secondary prizes.

## How It Works 

From a technical perspective a Pod is a **single user** in relation to the PrizePool protocol - ****even though 10's, 100's or even 1000's of users may have deposited funds.

**The Pod smart contract adheres to the ERC20 specification**. While Pods include functionality to interact with a strict set of external smart contracts: _PrizePool, TokenListeners and TokenDrops,_ at the core Pods can be thought of as a token.

When tokens are deposited into a Pod, via `depositTo` new shares are minted.

Minted shares, represented as an ERC20 balance, represent a user's claim on the underlying balance managed via the Pod smart contract.

In other words, user's deposits tokens directly into a Pod, and in-turn the Pod will batch those deposits into a single transaction and deposit into the PoolTogether V3 PrizePool smart contracts, but the shares minted by the Pod can be traded just like any other ERC20 token.

## Smart Contract Functions

#### `depositTo` - Deposit Tokens & Mint Shares 

```javascript
function depositTo(
 address to, 
 uint256 tokenAmount
) external override nonReentrant returns (uint256)
```

The `depositTo` function interface is similar to the PrizePool `depositTo` function interface, minus the `controlledToken` and `referrer` inputs, which are automatically added by the Pod during the batch process.

As with all ERC20 smart contracts transfers/deposits, **users must first set a positive allowance for the target contract**. Afterwards users deposit funds into the Pod by calling the `depositTo` function with the desired `to` and `tokenAmount` inputs.

Normally users will enter their personal wallet address, unless a deposit is made on behalf of user, which might be the case for a periphery smart contract. For example, a third-party contract might convert ETH into the underlying Pod token before calling depositTo - sometimes referred to as a zap.

When a user deposits tokens, the Pod mints shares - representing a claim on the deposit.

#### `withdraw` - Burn Shares & Withdraw Tokens

```javascript
function withdraw(
 uint256 shareAmount,
 uint256 maxFee
) external override nonReentrant returns (uint256)
```

The `withdraw` function, as expected, handles withdraws from the Pod. To withdraw from a Pod, the user must have a positive share balance. Whether that's via depositing tokens or being transferred Pod shares.

When a users withdraw the shares are burned and the underlying balance is transferred. 

In addition to entering a valid `shareAmount` users must also specify the `maxFee` amount. When exiting a PrizePool a fee may be applied, depending on the last deposit timestamp. Due to the nature of a Pod's regular deposits/withdrawals the early exit is constantly updating.

The early exit fee can be calculated by calling the `getEarlyExitFee` view function and entering the total underlying balance to be withdrawn.

First, a user may want to calculate the total underlying balance relative to their shares by calling `balanceOfUnderlying(address user) returns (uint256 amount)`which will calculate the underlying balance via the user's share balance.

After a user has determined their total underlying balance, they can proceed to calculate the early exit by inputting the desire withdraw amount, relative to their share balance.

#### `drop` - Claim Reward Tokens, Batch User Deposits and TokenDrop

```javascript
function drop() external override nonReentrant returns (uint256)
```

The `drop` function is responsible for claiming and distributing rewards tokens \(i.e. POOL\) to the TokenDrop smart contract and executing `batch` which transfers recent token deposits into the PrizePool.

The average user will not need to interact with the `drop` function. Instead it's up to the Pod owner/manager to regularly call `drop` function and batch deposits.

Pods deployed by the PoolTogether Inc team are automatically managed using the OpenZeppelin Defender system. Eliminating the need for administrator to manually manage a Pod's deposits.

[PoolTogether Pods Upkeep](https://github.com/pooltogether/pooltogether-pods-upkeep)

#### `batch` - Batch User Deposits

```javascript
function batch() external override nonReentrant returns (uint256)
```

The `batch` function is responsible for moving deposited tokens from the Pod smart contract into the PrizePool smart contract.

Overall, the batching functionality is a simple process:

* Read the current underlying token balance.
* Deposit the underlying token balance into the PrizePool.

After the Pod batches token deposits, converting the tokens into tickets, the Pod is instantly eligible for winning the PrizePool award.

Generally, the `batch` function will be called indirectly via the `drop` function. The `batch` function is called indirectly because, in addition to converting tokens to tickets, it's important to claim and distribute the reward token, which is handled via the `drop` function.

The batching functionality is the reason for an average 3x in gas savings.

