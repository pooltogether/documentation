# Stake Prize Pool

The Stake Prize Pool is a prize pool that uses an ERC-20 compatible token as the underlying asset.

Users's can stake their tokens to become eligible for whatever prize is defined as the prize strategy for that pool. 

This is particularly useful for protocols that are sitting inactively in users's wallets - why not stake them in a pool and become eligible for rewards?

The returned ticket (ticket.md) can be thought of as a "proof-of -liquidity"

## Retrieving the Underlying ERC-20

The underyling staked asset can be retrieve using the function:

```javascript
function token() returns (address);
```