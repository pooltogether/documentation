# 🤑 Multiple Winners

The Multiple Winnners prize strategy periodically selects a predefined number of winners and awards to them an equal share of the prizes available in the Prize Pool.

A Multiple Winners prize strategy is initialized with:

**Prize Period Start:** the timestamp at which the prize period should start

**Prize Period Seconds**: the duration of time between prizes

**The PrizePool address**: the address of the prize pool (../../prize-pool) that implements the pool functionality such as deposit and withdraw

\*\*\*\*[**Ticket**](../ticket.md)**:** The interface to use to select winners

\*\*\*\*[**Sponsorship**]../(sponsorship.md): The token that represents sponsorship

\*\*\*\*[**Random Number Generator**](../../random-number-generator/): used to generate random numbers for winner selection

**Number of Winners**: the number of winners in a prize period. This can be later changed. 

<!-- ## Awarding

Anyone may start the award process.

```javascript
function startAward() external
```

**startAward\(\):**

* Requires that either:
  * the current time is greater than or equal to start time + prize period
  * there is an active random number request and it has timed out
* Locks the pool. Tickets cannot be minted or redeemed
* Requests a random number from the RNG service

Once the random number is available, a user can call:

```javascript
function completeAward() external
```

**completeAward\(\):**

* Requires that **startAward\(\)** has been called
* Unlocks the pool
* Disburses the prize
* Moves the prize start time forward
* Clears the random number request

You can check the above required conditions using canStartAward\(\) and canCompleteAward\(\):

```javascript
function canStartAward() external view returns (bool);
```

and

```javascript
function canCompleteAward() external view returns (bool);
``` -->



## Strategy Settings

The number of winners in a prize period can be set by calling:
```javascript
function setNumberOfWinners(uint256 count) external onlyOwner requireAwardNotInProgress
```
* Requires that the Award process is not in progress
* Count must be greater than 0


The SplitExternalErc20Awards flag can be set by calling: 
```javascript
function setSplitExternalErc20Awards(bool _splitExternalErc20Awards) external onlyOwner requireAwardNotInProgress
```

This controls how externally added ERC-20's are distributed. Setting to `true` results in the ERC-20's paid out uniformly (similar to the main prize), while `false` does not pay out external ERC-20's for that award. 

### View Strategy Settings

The number of winners setting can be viewed by calling the following:

```javascript
function numberOfWinners() external view returns (uint256) {
```



<!-- ## Prizes

### Current Yield Prize

To retrieve amount of accrued prize interest so far you may call:

```javascript
function currentPrize() external returns (uint256);
```

### Estimating Prize

To estimate what the prize will be you can call:

```javascript
function estimatePrize() external returns (uint256);
```

## External Prizes

The owner can add "external" ERC20 tokens as prizes.  The strategy will award the entire balance held by the Prize Pool to the winner.

```javascript
function addExternalErc20Award(address _externalErc20) external onlyOwner;
```

The owner can add "external" ERC721 tokens as prizes.  These tokens will be transferred to the winner.

```javascript
function addExternalErc721Award(
    address _externalErc721,
    uint256[] calldata _tokenIds
) external onlyOwner
```

## Time

To retrieve when the current prize started:

```javascript
function prizePeriodStartedAt() external view returns (uint256)
```

To retrieve when the prize will end:

```javascript
function prizePeriodEndAt() external view returns (uint256)
``` -->

<!-- ## Miscellaneous

| Function | Description |
| :--- | :--- |
| function sponsorship\(\) returns \([ERC20](https://eips.ethereum.org/EIPS/eip-20)\) | Returns the address of the sponsorship token. |
| function ticket\(\) returns \([Ticket](ticket.md)\) | Returns the address of the ticket token. | -->



