# Ticket

The Ticket contract is an ERC20-compatible token that allows users to be selected by a token index.

In addition to the standard ERC20 functions, a user can be selected using the function:

```javascript
function draw(uint256 randomNumber) public view returns (address)
```

The **randomNumber** will be used as a token index into a specialized data structure that stores the user balances.  The randomNumber is constrained to the token supply, and modulo bias is corrected.

The returned address is the user who holds the token index corresponding to the random number.

