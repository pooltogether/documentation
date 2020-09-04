# 🎟️ Ticket

The Ticket contract is an [ERC20](https://eips.ethereum.org/EIPS/eip-20)-compatible token that allows users to be selected by a token index.

The contract organizes the balances into a sum tree data structure, so that each address holds a "range" of tokens.  A number can be used as an index within that range, and the holder of the tokens in that range is selected:

```javascript
function draw(uint256 randomNumber) public view returns (address)
```

The **randomNumber** will be used as a token index into a specialized data structure that stores the user balances.  The randomNumber is constrained to the token supply and modulo bias is corrected.

The returned address is the user who holds the token index corresponding to the random number.

