# 🤑 Multiple Winners

The Multiple Winnners prize strategy periodically selects a predefined number of winners and awards to them an equal share of the prizes available in the Prize Pool.

A Multiple Winners prize strategy is initialized with:

**Prize Period Start:** the timestamp at which the prize period should start

**Prize Period Seconds**: the duration of time between prizes

**The PrizePool address**: the address of the prize pool (../../prize-pool) that implements the pool functionality such as deposit and withdraw

\*\*\*\*[**Ticket**](../ticket.md)**:** The interface to use to select winners

\*\*\*\*[**Sponsorship**]../(sponsorship.md): The token that represents sponsorship

\*\*\*\*[**Random Number Generator**](../../random-number-generator/): used to generate random numbers for winner selection

**Number of Winners**: the number of winners in a prize period. This can be later changed by the owner calling set. 


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






