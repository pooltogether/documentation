---
description: Easily create preconfigured Prize Pools
---

# Prize Pool Builders

Builders allow users to create preconfigured Prize Pools.  Users may provide their own Prize Strategy, or use the Single Random Winner strategy.

## Single Random Winner Prize Pool Builder

This builder creates a new [Prize Pool](prize-pool/) that uses a [Single Random Winner]() prize strategy.  This strategy awards the prize periodically to a randomly selected winner.

Users can create a new single random winner prize pool using:

```javascript
function createSingleRandomWinnerPrizePool(
    CTokenInterface cToken,
    uint256 prizePeriodInSeconds,
    string calldata ticketName,
    string calldata ticketSymbol,
    string calldata sponsorshipName,
    string calldata sponsorshipSymbol
) external returns (SingleRandomWinnerPrizeStrategy)
```

| Function Parameter | Description |
| :--- | :--- |
| cToken | The address of the Compound cToken to use |
| prizePeriodInSeconds | The prize period to use |
| ticketName | The name to use for the ticket ERC20 |
| ticketSymbol | The symbol to use for the ticket ERC20 |
| sponsorshipName | The name to use for the sponsorship token ERC20 |
| sponsorshipSymbol | The symbol to use for the sponsorship token ERC20 |

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
function createPeriodicPrizePool(
    CTokenInterface cToken,
    PrizeStrategyInterface prizeStrategy,
    uint256 prizePeriodSeconds,
    string memory ticketName,
    string memory ticketSymbol,
    string memory sponsorshipName,
    string memory sponsorshipSymbol
) public returns (PrizePool)
```

| Function Parameter | Description |
| :--- | :--- |
| cToken | The Compound cToken to use for yield.  The underlying asset for the Prize Pool will be the underlying asset for the cToken. |
| prizeStrategy | The address of the prize strategy contract. |
| ticketName | The name to use for the ticket ERC20 |
| ticketSymbol | The symbol to use for the ticket ERC20 |
| sponsorshipName | The name to use for the sponsorship ERC20 |
| sponsorshipSymbol | The symbol to use for the sponsorship ERC20 |

This function will emit the event:

```javascript
event PrizePoolCreated(
    address indexed creator,
    address indexed prizePool,
    address interestPool,
    address ticket,
    address prizeStrategy,
    uint256 prizePeriodSeconds
);
```

| Event Parameter | Description |
| :--- | :--- |
| creator | The address that called the contract |
| prizePool | The address of the Prize Pool |
| interestPool | The address of the Compound Interest Pool |
| ticket | The address of the ticket token ERC20 |
| prizeStrategy | The address of the Prize Strategy |
| prizePeriodSeconds | The prize period in seconds. |



