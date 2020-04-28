# Builder

Users can create a Prize Pool using the Prize Pool Builder.  They must supply their own Prize Strategy.

## Creating a Prize Pool

Users can create new prize pools using the function:

```javascript
function createPrizePool(
    CTokenInterface cToken,
    PrizeStrategyInterface _prizeStrategy,
    string memory _collateralName,
    string memory _collateralSymbol,
    string memory _ticketName,
    string memory _ticketSymbol
) public returns (PrizePool)
```

The function takes:

* cToken: The Compound cToken to use for yield and to determine the underlying asset
* \_prizeStrategy: The prize strategy to use
* collateralName: The token name for the sponsorship token
* collateralSymbol: The token symbol for the sponsorship token
* ticketName: The token name for tickets
* ticketSymbol: The token symbol for tickets

