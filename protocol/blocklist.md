---
description: Block addresses from being selected for award distribution
---

# 🛑 Blocklist

## Introduction

The blocklist feature \(introduced in 3.4.0\) allows the MultipleWinners prize strategy smart contract to prevent addresses from being eligible for winning a prize. 

⚠️ **WARNING:** The blocklist feature **SHOULD NOT** be used to prevent normal users from being eligible to win a prize. The intended use case is to prevent smart contracts, which are unable to withdraw prizes.

### Why

The blocklist feature was built specifically to overcome the problem of adding tickets to decentralized exchange, so users can easily exchange assets for a `ticket` in their desired `PrizePool` without awarding the liquidity pool address with a prize.

Depositing directly into existing `PrizePools` like USDC and DAI on average costs between 450,000 to 550,000 gas. During times of gas costs \(e.x. between 90 - 200+\) these can be cost prohibitive for users depositing smaller amounts.

### Side Effects of Blocked Addresses When Selecting Winners

In certain circumstances it's possible to select few winners, then the amount designated by the internal `__numberOfWinners` variable due to the number of attempts \(limited by `blocklistRetryCount`\) available in the `_distribute` function.

For example, if a Uniswap liquidity pool holds 50% of the available tickets, but is blocked from winning, and the number of winners is set to 3 and the try attempt is limited to 5, it's possible the liquidity pool address would be selected 5 times, before 3 winners are randomly drawn.

Resulting in only 2 of 3 winners being selected before the `blocklistRetryCount`ceiling was reached. Depending on the state of `carryOverBlocklist` the 2 of 3 selected winners would either...

A\) split the total awarded prize evenly in this draw period.  
B\) split the total awarded prize based off `__numberOfWinners`

If **Scenario B** occurs the remaining awarded interest would roll over to next draw period. In the next draw period the increased prize amount \(_due to the carried over prize_\) would be given to the selected prize.

In every scenario the selected winner\(s\) will **always** receive the expected prize amount.

## **How It Works**

The MultipleWinners smart contract now includes the ability to block individual address from being selected as a winner for prize distribution.  

**Public Functions**

* `setBlocklisted`
* `setCarryBlocklist`
* `setBlocklistRetryCount`

### Blocking Address from Winning

A simple mapping called `isBlocklisted` correlates a user address `isBlocked` boolean status.

`mapping(address => bool) public isBlocklisted;`

During the contract initialization all address default `isBlocked` false. After the contract has been initialized the contract owner can toggle a user's `isBlocklisted` status by calling `setBlocklisted` with the `_user` **address** and `_isBlocked` set **true**.

```text
function setBlocklisted(address _user, bool _isBlocked) external onlyOwner requireAwardNotInProgress returns (bool) {
    isBlocklisted[_user] = _isBlocked;

    emit BlocklistSet(_user, _isBlocked);

    return true;
  }
```

To reverse the `isBlocked` status a contract owner calls the same function. Passing the same `_user` **address** with the `_isBlocked`set **false**. 

When winners are being selected during

### Blocklist Retry Attempt Count

When attempting to select a winner during the draw process it's still possible to select a blocked address. However, instead of adding the blocked address to the array of winners, the contract will attempt to select a new winner with a false `isBlocked` status.

By default the `blocklistRetryCount` will be set to 0.

To update the retry count a contract owner can call `setBlocklistRetryCount` with the number of retries.  

```text
function setBlocklistRetryCount(uint256 _count) external onlyOwner requireAwardNotInProgress returns (bool) {
  blocklistRetryCount = _count;

  emit BlocklistRetryCountSet(_count);

  return true;
}
```

As you already know, each Ethereum transaction consumes gas. The more complex a transaction the higher the gas costs. To avoid running out of gas in a if a blocked address is selected multiple times, a retry limit is set and the transaction will continue with awarding the selected winners.

In short, the prize strategy will do its best to select winners without running out of gas.

It's important to mention, even though a a retry attempt count must be set, it's very unlikely the retry attempt limit be reached, unless a blocked address holds an unusually high amount of tickets. 

### Prize Award Carry Over

In the very unlikely circumstance the retry count is reached due to a high number of `tickets` held by a blocked address and the maximum number of winners has not been selected, the prize strategy must decide how to handle the remaining awarded interest: **evenly** **split the interest between selected winners or carry over the interest for the next draw.**

By default the award interest is carried over to the next round if the maximum of winners **IS NOT** selected.

To evenly split the interest between winners the contract owner calls `setCarryBlocklist` with true.

```text
function setCarryBlocklist(bool _carry) external onlyOwner requireAwardNotInProgress returns (bool) {
  carryOverBlocklist = _carry;

  emit BlocklistCarrySet(_carry);

  return true;
}
```

#### External ERC20 Carry Over

If a PrizeStrategy also awards external ERC20 tokens during award distribution the same carry over rules to the external tokens. In other words, if the main prize is split evenly between the selected winners or carried over to the next draw period, the same logic will be applied to the tokens.

## **Smart Contract Functions**  

