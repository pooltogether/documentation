---
description: >-
  Distribute a percentage of the prize (before awarding winners) on every draw
  to fixed address.
---

# 🖖 Prize Splits

## Introduction

The prize split features \(introduced in v3.4.0\) allows contract owners to designate a percentage of the awarded prize to a fixed wallet address in every draw period. In other words, prize pools can now directly award a wallet, whether that's a charity organization, prize pool manager, decentralized autonomous organization or any unique combination of prize splits every time a draw occurs.

### Example

#### Setup

**Prize Award:** $10,000 USDC  
**Loot Box:** $2,500 COMP   
**Number of Winners:** 3  
**Prize Splits:**   
   - 5% Sponsorship to Gitcoin Developer  
   - 5% Ticket to PrizePool manager

#### Outcome

**Main Winner:** $3,000 and Loot Box \($2,500\)  
**Secondary Winners:** $3,000  
**Prize Splits:**   
    - $500 to Gitcoin Developer Address  
    - $500 to PrizePool manager

#### Code

```text
PrizeSplitConfig {
  target: 0x0000000000000000000000000000000000000000 // Gitcoin Wallet
  percentage: 50 // 5%
  token: 1 // Sponsorship Token
}

PrizeSplitConfig {
  target: 0x0000000000000000000000000000000000000001 // Owner Wallet 
  percentage: 50 // 5%
  token: 0 // Ticket Token
}
```

## How It Works

The prize split configuration is an **abstract contract** inherited by any PrizePool smart contract. The PrizeSplit.sol smart contract is a minimal smart contract with limited external functions and a single primary `internal` function named `_distributePrizeSplits` which should be called at the top of the inheriting smart contracts `_distribute` function.

**Public Functions**

* `setPrizeSplits`
* `setPrizeSplit`

**Primary Internal Functions**

* `_distributePrizeSplits`

**Helper Internal Functions**

* `_getPrizeSplitAmount`
* `_totalPrizeSplitPercentageAmount`

### Prize Split Config \(PrizeSplitConfig\)

Prize split configurations are saved in a PrizeSplitConfig struct and stored in the internal \_prizeSplits array. During the award distribution \(before awarding the winners\) prize split recipients are awarded the designated token type \(Ticket or Sponsorship\). After the recipients have been awarded, the remaining updated prize award amount is distributed to the winners.

### Prize Splits Configuration

Each prize splits is stored in a `PrizeSplitConfig` struct with 3 configurable variables: target, percentage and token. The `target` variable designates the receiver of the prize split. The `percentage` variable stores single point decimal precision percentage using a number with a range between 0-to-1000. And finally the `token` parameter references the index position of the `ControlledToken` in the `PrizePool.tokens` array.

```text
PrizeSplits.sol
---------------

struct PrizeSplitConfig {
  address target;
  uint16 percentage;
  uint8 token;
}
```

Target is self-explanatory, but percentage and token require a deeper understanding of percentages are calculated and how an external contract stores token references.

#### Percentage  

The `percentage` uses a range of 0-to-1000 so single decimal precision can easily be calculated without requiring a fixed point math library.

```text
PrizeSplits.sol
---------------

function _getPrizeSplitAmount(uint256 amount, uint16 percentage) internal pure returns (uint256) {
  return (amount * percentage).div(1000);
}
```

When calculating a prize split distribution amount, the result of totalPrizeAmount \* prizeSplitPercentage  is divided by 1,000. Thus if the prize split percentage is set to `1000` than the distribution amount would result in 100% of the prize being awarded to the prize split. If the the prize split percentage is set to `505` distribution amount would equate to `50.5%` of the original award amount. 

#### Token

If using the `PoolWithMultipleWinnersBuilder.sol` instance to deploy a new PrizePool, the first token stored in the `tokens` array is the `Ticket` and the second is the `Sponsorship` token. Thus, when deciding which token is minted during the prize split distribution, the `token` variable must be set to either `0` or `1` depending on the desired  ticket to be awarded.

```text
PoolWithMultipleWinnersBuilder.sol
----------------------------------

function _tokens(MultipleWinners _multipleWinners) internal view returns (ControlledTokenInterface[] memory) {
  ControlledTokenInterface[] memory tokens = new ControlledTokenInterface[](2);
  tokens[0] = ControlledTokenInterface(address(_multipleWinners.ticket()));
  tokens[1] = ControlledTokenInterface(address(_multipleWinners.sponsorship()));
  return tokens;
}

```

## Awarding the Prize Splits

Before the draw winners are award `Tickets` the prize splits must first be distributed, so the remaining prize award amount can be used to calculate the winners distribution amount.

Thus when the `_distribute` function is called within `MultipleWinners.sol` \(_responsible for capturing the interest earned during that award period_\) the captured award balance is immediately passed into the internal`_distributePrizeSplits` function. Distributing the prize splits to target recipients and returning the updated `prize` amount for winner award distribution.

```text
MultipleWinners.sol
-------------------

function _distribute(uint256 randomNumber) internal override {
  uint256 prize = prizePool.captureAwardBalance();
  
  // distributes prize to prize splits and returns remaining award.
  prize = _distributePrizeSplits(prize);

  if (IERC20Upgradeable(address(ticket)).totalSupply() == 0) {
    emit NoWinners();
    return;
  }

  ...
}
```

