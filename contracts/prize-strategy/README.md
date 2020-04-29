# Prize Strategy

A Prize Strategy governs the operation of [Prize Pools](../prize-pool/).  The strategy determines:

* The instant withdrawal fairness fee
* The lossless withdrawal timelock duration
* How, when, and to whom prizes are awarded

When a Prize Pool is constructed it is configured with a Prize Strategy.  The Prize Pool requires the Prize Strategy to implement a interface in order to be supported.

## Prize Strategy Interface

### Calculating the Fairness Fee

When a user wishes to redeem their tickets instantly, the prize pool will ask the strategy what the fairness fee should be. 

The function signature is:

```javascript
function calculateExitFee(address user, uint256 tickets) external view returns (uint256)
```

The strategy will be given the user, the number of tickets they are attempting to withdraw, and is expected to return the fee amount.

### Calculating the Withdrawal Timelock

When a user wishes to redeem their tickets without paying any fees, then prize pool will ask the strategy what the unlock timestamp is:

```javascript
function calculateUnlockTimestamp(address user, uint256 tickets) external view returns (uint256);
```

The strategy will be passed the user and the amount of tickets and be expected to return the timestamp after which the user may withdraw.

## Prize Strategy Privileges

Only the Prize Strategy is able to award prizes on its associated [Prize Pool](../prize-pool/).   See [Awarding Prizes](../prize-pool/#awarding-prizes).





