# Prize Pool

Prize Pools award the accrued interest on deposits as prizes.  The prizes are distributed according to the configured [Prize Strategy](../prize-strategy/).  Uninitialized Prize Pools can be cheaply created using the [Prize Pool Factory](prize-pool-factory.md).

## Buying Tickets

Users can purchase tickets using the **mintTickets** function:

```javascript
function mintTickets(uint256 tickets) external
```

Tickets will be minted at a rate of 1:1 to the underlying asset, so minted 10 tickets will require 10 of the asset.  This function should be pre-approved to spend **tickets** amount of the underlying asset.

## Redeeming Tickets

Tickets can be redeemed for the underlying asset in two ways: either losslessly through a timelock, or instantly by paying a fee that goes toward the prize.

### Lossless Redemption

Tickets can be redeemed without any additional fees by timelocking the funds.  The withdrawal amount will be available after the next prize.

To start a withdrawal timelock a user may call:

```javascript
function redeemTicketsWithTimelock(uint256 tickets) external returns (uint256)
```

The **tickets** represents the number of tickets to redeem and the function will return the timestamp after which the funds will be available to sweep into the user's wallet.

When enough time has passed the funds may be swept by anyone into the user's wallet:

```javascript
function sweepTimelockFunds(address[] calldata users) external returns (uint256)
```

The function accepts an array of addresses and will attempt to sweep the timelocked funds for each one.

### Instant Redemption

If a user would like their tickets right away, they may pay a fee to the prize.  This fee is calculated based on the previous prize and is designed to prevent people from gaming the system.

To withdraw instantly:

```javascript
function redeemTicketsInstantly(uint256 tickets) external returns (uint256)
```

The **tickets** represents how many tickets will be redeemed.  The amount transferred to the user, less the fee, is the value returned from the function.

## Timelocked Withdrawals

When a user has a timelocked withdrawal, their funds will be available after a certain time.

To find out their timelocked balance a user can call:

```javascript
function lockedBalanceOf(address user) external view returns (uint256)
```

The timelocked balance of the user will be returned.

To find out when a user is able to withdraw their funds:

```javascript
function lockedBalanceAvailableAt(address user) external view returns (uint256)
```

The timestamp after which the funds will be available is returned.

## Awarding Prizes

The Prize Strategy that was configured when the Prize Pool was created is the only address that is able to award prizes.

To award prizes, the Prize Strategy can call:

```javascript
function award(address winner, uint256 amount) external onlyPrizeStrategy
```

The function will award **amount** of tickets to the address **winner**.

