# Single Random Winner

## Prize Strategy Interface

### Calculating the Early Exit Fee

When a user wishes to redeem their tickets instantly, the prize pool will ask the strategy what the fairness fee should be. 

The function signature is:

```javascript
function calculateInstantWithdrawalFee(
    address from,
    uint256 amount,
    address controlledToken
  )
    external
    returns (uint256 remainingFee, uint256 burnedCredit)
```

The strategy will be given the user, the number of tickets they are attempting to withdraw, and is expected to return the fee amount.

### Calculating the Withdrawal Timelock

When a user wishes to redeem their tickets without paying any fees, then prize pool will ask the strategy what the unlock timestamp is:

```javascript
function calculateTimelockDurationAndFee(
    address from,
    uint256 amount,
    address controlledToken
  )
    external
    returns (uint256 durationSeconds, uint256 burnedCredit)
```

The strategy will be passed the user and the amount of tickets and be expected to return the timestamp after which the user may withdraw.

## Prize Strategy Privileges

Only the Prize Strategy is able to award prizes on its associated [Prize Pool](../prize-pool.md).   See [Awarding Prizes](../prize-pool.md#awarding-prizes).

## Awarding Prizes

Prizes are awarded in two phases.  The first phase locks the Pool and requests a random number from the Random Number Generation service.  The second phase retrieves the random number, award the prize, and unlocks the pool.

The first phase of the award process begins by calling:

```javascript
function startAward() external
```

**startAward\(\)** will:

* Lock the pool: minting and redemption not allowed.
* Request a random number from the RNG service

Once the random number is available, a user can call:

```javascript
function completeAward() external
```

**completeAward\(\)** will:

* Unlock the pool
* Disburse the prize
* Start the new prize

Both startAward\(\) and completeAward\(\) have functions to check whether they can be called:

```javascript
function canStartAward() external view returns (bool);
```

and

```javascript
function canCompleteAward() external view returns (bool);
```

## Reporting

### Current Prize

To retrieve amount of accrued prize interest so far you may call:

```javascript
function currentPrize() external returns (uint256);
```

### Estimating Prize

To estimate what the prize will be you can call:

```javascript
function estimatePrize() external returns (uint256);
```

### Time

To retrieve when the current prize started:

```javascript
function prizePeriodStartedAt() external view returns (uint256)
```

To retrieve when the prize will end:

```javascript
function prizePeriodEndAt() external view returns (uint256)
```

## Miscellaneous

| Function | Description |
| :--- | :--- |
| function sponsorship\(\) returns \([ERC20](https://eips.ethereum.org/EIPS/eip-20)\) | Returns the address of the sponsorship token. |
| function ticket\(\) returns \([Ticket](../ticket.md)\) | Returns the address of the ticket token. |



