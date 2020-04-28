# Ticket

The Ticket contract is a [ControlledToken](../interestpool/controlledtoken.md) that additionally allows users to be selected by a token index.  It is compatible with the ERC20 token interface.

To select a user by token index, use the function:

```javascript
function draw(uint256 randomNumber) public view returns (address)
```

The **randomNumber** will be used as a token index into a specialized data structure that stores the user balances.  The randomNumber is constrained to the token supply, and modulo bias is corrected.

The returned address is the user who holds the token index corresponding to the random number.

