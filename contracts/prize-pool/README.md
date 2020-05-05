---
description: Pool deposits and award accrued interest periodically as a prize
---

# Prize Pool

Prize Pools allow users to pool their assets together and award the accrued interest periodically as a prize.

* Prize Pools are created using the [Prize Pool Builder](../builders.md)
* Prize distribution is determined by the [Prize Strategy](../prize-strategy.md) configured at pool creation
* Interest accrues on the underlying assets by supplying them to Compound

There are two types of membership in a Prize Pool; users may hold tickets or sponsorship.  Ticket tokens  represent both the underlying collateral and chances to win.  Sponsorship tokens represent the underlying collateral, but cannot win.

## Buying Tickets

Users can purchase tickets using the **mintTickets** function. Tickets will be minted at a ratio of 1:1 to the underlying asset, so minted 10 tickets will require 10 of the asset.  This function should be pre-approved to transfer the underlying asset on behalf of the sender, as in the ERC20 spec.

```javascript
function mintTickets(uint256 amount) external
```

| Parameter | Description |
| :--- | :--- |
| amount | The amount of the underlying asset the user wishes to deposit.  The Prize Pool contract should have been pre-approved by the caller to transfer the underlying ERC20 tokens. |

Alternatively, a ticket can be purchased for someone else.  The sender must still supply the underlying tokens as the deposit:

```javascript
function mintTicketsTo(address to, uint256 amount) external
```

Minting fires the event:

```javascript
event TicketsMinted(address indexed from, address indexed to, uint256 amount);
```

| Event Data | Description |
| :--- | :--- |
| from | The address that triggered and paid for the mint |
| to | The address that received the tickets |
| amount | The amount of both the underlying asset that was transferred and the tickets that were minted. |

## Redeeming Tickets

Tickets can be redeemed for the underlying asset in two ways: either by waiting for a time lock to expire, or instantly by paying a fee that goes toward the prize.

### Lossless Redemption

Tickets can be redeemed without any additional fees by time-locking the funds.  The withdrawal amount will be available after the next prize.

To start a withdrawal time lock a user may call:

```javascript
function redeemTicketsWithTimelock(uint256 tickets) external returns (uint256)
```

The **tickets** represents the number of tickets to redeem and the function will return the timestamp after which the funds will be available to sweep into the user's wallet.

The timestamp at which funds will be unlocked can be calculated using:

```javascript
function calculateUnlockTimestamp(address sender, uint256 tickets) external view
```

When enough time has passed the funds may be swept by anyone into the user's wallet:

```javascript
function sweepTimelockFunds(address[] calldata users) external returns (uint256)
```

The function accepts an array of addresses and will attempt to sweep the time-locked funds for each one.

The time-locked balance for a user can be retrieved using:

```javascript
function lockedBalanceOf(address user) external view returns (uint256)
```

The timestamp at which the time-locked balance will be available can be retrieved using:

```javascript
function lockedBalanceAvailableAt(address user) external view returns (uint256)
```

### Instant Redemption

If a user would like their tickets right away, they may pay a fee to the prize.  This fee is calculated based on the previous prize and is designed to prevent people from gaming the system.

To withdraw instantly:

```javascript
function redeemTicketsInstantly(uint256 tickets) external returns (uint256)
```

The **tickets** parameter determines how many tickets will be redeemed.  The amount transferred to the user, less the fee, is the value returned from the function.

The exit fee can be calculated using:

```javascript
function calculateExitFee(address sender, uint256 tickets) external view returns (uint256)
```

## Sponsoring

A user may sponsor the pool by contributing interest to the prize without being eligible to win.

A user's sponsorship is tokenized as Sponsorship Tokens.  Sponsorship tokens are minted at a ratio of 1:1 to the underlying asset.  To deposit the asset as sponsorship a user may call:

```javascript
function mintSponsorship(uint256 tickets) external
```

To redeem their sponsorship a user calls:

```javascript
 function redeemSponsorship(uint256 tickets) external
```

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

## Miscellaneous

| Function | Description |
| :--- | :--- |
| function sponsorship\(\) returns \([ERC20](https://eips.ethereum.org/EIPS/eip-20)\) | Returns the address of the sponsorship token. |
| function ticket\(\) returns \([Ticket](ticket.md)\) | Returns the address of the ticket token. |

