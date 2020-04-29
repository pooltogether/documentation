---
description: Easily create preconfigured Prize Pools
---

# Builders

Builders allow users to create new Prize Pools in a single transaction.

## Prize Pool Builder

The Prize Pool Builder is the core Builder of the system.  It allows users to create new Prize Pool with their desired Prize Strategy.

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

## Single Random Winner Prize Pool Builder

This builder creates a new [Prize Pool](prize-pool/) that uses a [Single Random Winner](prize-strategy/singlerandomwinnerprizestrategy.md) prize strategy.  This strategy awards the prize periodically to a randomly selected winner.

Users can create a new single random winner prize pool using:

```javascript
function createSingleRandomWinnerPrizePool(
    CTokenInterface cToken,
    uint256 prizePeriodInSeconds,
    string calldata _collateralName,
    string calldata _collateralSymbol,
    string calldata _ticketName,
    string calldata _ticketSymbol
) external returns (SingleRandomWinnerPrizeStrategy)
```

The function takes:

* cToken: The Compound cToken to use
* prizePeriodInSeconds: The interval over which prizes should be awarded.
* collateralName: The token name for the sponsorship token
* collateralSymbol: The token symbol for the sponsorship token
* ticketName: The token name for tickets
* ticketSymbol: The token symbol for tickets

