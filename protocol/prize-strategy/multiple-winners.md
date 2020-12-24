# 🤑 Multiple Winners

The Multiple Winners prize strategy periodically selects a predefined number of winners and awards to them an equal share of the prizes available in the Prize Pool.

## Initialization

A Multiple Winners prize strategy is initialized with:

**Prize Period Start:** the timestamp at which the prize period should start

**Prize Period Seconds**: the duration of time between prizes

**PrizePool Address**: the address of the [prize pool](../prize-pool/) that implements the pool functionality such as deposit and withdraw

[**Ticket**](../tokens/ticket.md)**:** The interface to use to select winners

\*\*\*\*[**Sponsorship**](../tokens/sponsorship.md)**:** The token that represents sponsorship

\*\*\*\*[**Random Number Generator**](../random-number-generator/): used to generate random numbers for winner selection

**Number of Winners**: the number of winners in a prize period. This can be later changed by the owner calling set number of winners.

## Strategy Settings

### Set Number of Winners

The number of winners in a prize period can be set by calling:

```javascript
function setNumberOfWinners(uint256 count) 
external onlyOwner requireAwardNotInProgress
```

* Requires that the Award process is not in progress
* `count` must be greater than 0

### View Number of Winners

The number of winners setting can be viewed by calling:

```javascript
function numberOfWinners() external view returns (uint256
```

### Set Split External ERC-20 Awards

The `SplitExternalErc20Awards` flag can be set by calling:

```javascript
function setSplitExternalErc20Awards(bool _splitExternalErc20Awards) 
external onlyOwner requireAwardNotInProgress
```

This controls how externally added ERC-20's are distributed. Setting to `true` results in the ERC-20's paid out uniformly \(similar to the main prize\), while `false` does not pay out external ERC-20's for that award.

### Set Random Number Generation Service

The [Random Number Generation](../random-number-generator/) Service can be set when the award process has not started by the Prize Pool owner by calling:

```javascript
  function setRngService(RNGInterface rngService) 
  external onlyOwner requireAwardNotInProgress 
```

### Set Random Number Generator Request Timeout

The RNG request timeout parameter can be set \(in seconds\) when the award process has not started by the Prize Pool owner by calling:

```javascript
function setRngRequestTimeout(uint32 _rngRequestTimeout)
external onlyOwner requireAwardNotInProgress {
```

## Prize Period Information

#### View if the Prize Period is Over

To check if the prize period is finished call:

```javascript
function isPrizePeriodOver() external view returns (bool) 
```

Returns `true` if the prize period is over, `false` otherwise.

#### View when the Prize Period Finishes

To check the unix time when the prize period ends call:

```javascript
function prizePeriodEndAt() external view returns (uint256)
```

#### View Estimate of Number of Blocks to Prize Block

To estimate the remaining blocks until the prize given a number of seconds per block call `estimateRemainingBlocksToPrize` with `secondPerBlockMantissa` set to 15 seconds for Ethereum mainnet:

```javascript
function estimateRemainingBlocksToPrize(uint256 secondsPerBlockMantissa) public
view returns (uint256) 
```

#### View Prize Period Remaining Time \(in seconds\)

To get the number of seconds remaining until the prize can be awarded call:

```javascript
 function prizePeriodRemainingSeconds() external view returns (uint256) 
```

#### View Next Prize Period Start Time

To get the Unix timestamp of when the next prize period will start call `calculateNextPrizePeriodStartTime` with `currentTime` set to the current Unix time: 

```javascript
function calculateNextPrizePeriodStartTime(uint256 currentTime) 
external view returns (uint256)
```

## Award Process

At the end of the prize period, anyone can begin the award process. This happens in two main stages - `startAward` and `completeAward`. `startAward` triggers the configured Random Number Generator request, which will take some blocks. `completeAward` can then be called, which selects the winners using the RNG result and pushes the tokens out to the winners. 

### Start Award

The award process can be started by calling `startAward`.  This function starts the award process by starting the configured random number request. The prize period must have ended. The RNG-Request-Fee is expected to be held within this contract before calling this function. 

```text
function startAward() external requireCanStartAward
```

Upon completion this function fires the following event:

```csharp
event PrizePoolAwardStarted(
    address indexed operator,
    address indexed prizePool,
    uint32 indexed rngRequestId,
    uint32 rngLockBlock
);
```

### Complete Award

The award process can be finished by calling `completeAward`. The random number must have been requested and now available \(is can be checked by calling `isRngCompleted()`\).

```javascript
function completeAward() external requireCanCompleteAward
```

This function fires two events upon completion:

```csharp
event PrizePoolAwarded(
    address indexed operator,
    uint256 randomNumber
);
```

Since Prize Pools are continuously rolling the next prize period is now open:

```csharp
event PrizePoolOpened(
    address indexed operator,
    uint256 indexed prizePeriodStartedAt
);
```

### Cancel Award

This function can be called by anyone to unlock the tickets if the RNG has timed out:

```javascript
function cancelAward() public
```

This function will fire the event:

```csharp
event PrizePoolAwardCancelled(
    address indexed operator,
    address indexed prizePool,
    uint32 indexed rngRequestId,
    uint32 rngLockBlock
);
```

### Listeners

A prize strategy can have both a [token listener](https://github.com/pooltogether/pooltogether-pool-contracts/blob/master/contracts/token/TokenListener.sol) and a periodic prize strategy listener in order execute code for certain callbacks \(event hooks\).

#### Set Token Listener

The token listener can be set by the prize pool owner when the award process is not in progress by calling `setTokenListener` with the address of the new `tokenList` :

```javascript
function setTokenListener(TokenListenerInterface _tokenListener)
  external onlyOwner requireAwardNotInProgress
```

#### Set Periodic Prize Strategy Listener

The periodic prize strategy listener can be set by the prize pool owner when the award process is not in progress by calling `setPeriodicPrizeStrategyListener` with the address of the new `PeriodicPrizeStrategyListener`:

```javascript
function setPeriodicPrizeStrategyListener(PeriodicPrizeStrategyListenerInterface _periodicPrizeStrategyListener) 
 external onlyOwner requireAwardNotInProgress
```

This function will ensure the Listener Interface is implementing using ERC-165 introspection, and upon completion fire the following event:

```csharp
event PeriodicPrizeStrategyListenerSet(
    PeriodicPrizeStrategyListenerInterface indexed periodicPrizeStrategyListener
);
```

### External ERC20 and ERC721 Awards

External awards can be added to the pool. This is particularly useful in the case of the stake pool. Although still possible for either the token listener or the owner to manually add or remove ERC-20's and ERC-721's, it is recommended to add a single [LootBox](../lootbox.md) per prize period and direct the external awards to this LootBox address. 

The pool owner or the token listener can add/remove ERC721's by calling: 

```javascript
function addExternalErc721Award(IERC721Upgradeable _externalErc721,
  uint256[] calldata _tokenIds) 
  external onlyOwnerOrListener requireAwardNotInProgress 
```

```javascript
function removeExternalErc721Award(
  IERC721Upgradeable _externalErc721,
  IERC721Upgradeable _prevExternalErc721)
  external onlyOwner requireAwardNotInProgress
```

The pool owner or the token listener can add/remove ERC20's by calling: 

```javascript
function addExternalErc20Awards(IERC20Upgradeable[] calldata _externalErc20s) 
    external onlyOwnerOrListener requireAwardNotInProgress
```

```javascript
function removeExternalErc20Award(
  IERC20Upgradeable _externalErc20,
  IERC20Upgradeable _prevExternalErc20) 
  external onlyOwner requireAwardNotInProgress 
```

Corresponding events are fired for each ERC type added or removed:

```csharp
  event ExternalErc721AwardAdded(
    IERC721Upgradeable indexed externalErc721,
    uint256[] tokenIds
  );

  event ExternalErc20AwardAdded(
    IERC20Upgradeable indexed externalErc20
  );

  event ExternalErc721AwardRemoved(
    IERC721Upgradeable indexed externalErc721Award
  );

  event ExternalErc20AwardRemoved(
    IERC20Upgradeable indexed externalErc20Award
  );
```

