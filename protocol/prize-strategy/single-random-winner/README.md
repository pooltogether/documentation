# Single Random Winner

The Single Random Winner prize strategy periodically selects a random winner and awards to them all the prizes available in the Prize Pool.

A Single Random Winner prize strategy is initialized with:

**Prize Period Start:** the timestamp at which the prize period should start

**Prize Period Seconds**: the duration of time between prizes

**Ticket:** The interface to use to select winners

**Sponsorship**: The token that represents sponsorship

**Random Number Generator**: used to generate random numbers for winner selection

## Awarding

When the current time is greater than or equal to the prize period start + duration, anyone may start the award process.

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

## Prizes

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
```

## Miscellaneous

| Function | Description |
| :--- | :--- |
| function sponsorship\(\) returns \([ERC20](https://eips.ethereum.org/EIPS/eip-20)\) | Returns the address of the sponsorship token. |
| function ticket\(\) returns \([Ticket](ticket.md)\) | Returns the address of the ticket token. |



