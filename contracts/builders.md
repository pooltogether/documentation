---
description: Easily create preconfigured Prize Pools
---

# Builders

Builders allow users to create new Prize Pools in a single transaction.

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

| Function Parameter | Description |
| :--- | :--- |
| cToken | The address of the Compound cToken to use |
| prizePeriodInSeconds | The prize period to use for the Single Random Winner prize strategy |
| \_collateralName | The name to use for the sponsorship token ERC20 |
| \_collateralSymbol | The symbol to use for the sponsorship token ERC20 |
| ticketName | The name to use for the ticket ERC20 |
| ticketSymbol | The symbol to use for the ticket ERC20 |

This function will emit the event:

```javascript
event SingleRandomWinnerPrizePoolCreated(
    address indexed creator,
    address indexed prizePool,
    address indexed singleRandomWinnerPrizeStrategy
);
```

| Event Parameter | Description |
| :--- | :--- |
| creator | The address that called this contract |
| prizePool | The address of the Prize Pool created |
| singleRandomWinnerPrizeStrategy | The address of the Single Random Winner prize strategy |

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

| Function Parameter | Description |
| :--- | :--- |
| cToken | The Compound cToken to use for yield and to determine the underlying asset |
| \_prizeStrategy | The prize strategy address |
| \_collateralName | The name to use for the sponsorship token ERC20 |
| \_collateralSymbol | The symbol to use for the sponsorship token ERC20 |
| ticketName | The name to use for the ticket ERC20 |
| ticketSymbol | The symbol to use for the ticket ERC20 |

This function will emit the event:

```javascript
event PrizePoolCreated(
    address indexed creator,
    address indexed prizePool,
    address indexed prizeStrategy,
    address interestPool,
    address collateral,
    address ticket
);
```

| Event Parameter | Description |
| :--- | :--- |
| creator | The address that called the contract |
| prizePool | The address of the Prize Pool |
| prizeStrategy | The address of the Prize Strategy |
| interestPool | The address of the Interest Pool |
| collateral | The address of the sponsorship token ERC20 |
| ticket | The address of the ticket token ERC20 |



