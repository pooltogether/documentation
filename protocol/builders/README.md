---
description: Easily create preconfigured Prize Pools
---

# 🛠 Builders

Prize Pool Builders allow users to create preconfigured Prize Pools. Builders create all contracts using teeny tiny proxies, so creating prize games is cheap!

There are currently three specific prize builders, but the starting point for all Prize Pool Builders is the PoolWithMultipleWinnersBuilder contract. This allows you to build any of the below pools:

* \*\*\*\*[**Compound Prize Pool Builder**](compound-prize-pool-builder.md) ****creates Compound Prize Pools using the Multiple Winners prize strategy.

The following function should be called to create a Compound Prize Pool: 

```javascript
  function createCompoundMultipleWinners(
    CompoundPrizePoolBuilder.CompoundPrizePoolConfig memory prizePoolConfig,
    MultipleWinnersBuilder.MultipleWinnersConfig memory prizeStrategyConfig,
    uint8 decimals
  ) external returns (CompoundPrizePool) 
```
*CompoundPrizePoolConfig (parameter 1):
{cToken: "0x5d3a536e4d6dbd6114cc1ead35777bab948e3643", maxExitFeeMantissa: "10000000000000000", maxTimelockDuration: "1000000" }

*MultipleWinnersConfig (parmeter 2):
{ rngService: "0xA932e74d5263A754Ea04432E5c53658434b0484B", prizePeriodStart: "1607534563", prizePeriodSeconds: "120", ticketName: "Ticketz", ticketSymbol: "TICK", sponsorshipName: "Sponsorshipz", sponsorshipSymbol: "SPON", ticketCreditLimitMantissa: "10000000000000000", ticketCreditRateMantissa: "83333333333333", numberOfWinners: "3", splitExternalErc20Awards: "true" }

*Decimals (parameter 3): 
18

The return value is the address of the newly created Compound Prize Pool.

* \*\*\*\*[**Stake Prize Pool Builder**](stake-prize-pool-builder.md) ****creates Stake Prize Pools using the Multiple Winners prize strategy.

The following function should be called to create a Stake Prize Pool for a particular token: 

```javascript
  function createStakeMultipleWinners(
    StakePrizePoolBuilder.StakePrizePoolConfig memory prizePoolConfig,
    MultipleWinnersBuilder.MultipleWinnersConfig memory prizeStrategyConfig,
    uint8 decimals
  ) external returns (StakePrizePool) 
```

For example the following tuples will create a Stake Prize Pool for the WBTC-WETH Uniswap Liquidity Provider token: 

*StakePrizePoolConfig (parameter 1):
{token: "0xbb2b8038a1640196fbe3e38816f3e67cba72d940", maxExitFeeMantissa: "10000000000000000", maxTimelockDuration: "1000000" }

*MultipleWinnersConfig (parameter 2):
{ rngService: "0xA932e74d5263A754Ea04432E5c53658434b0484B", prizePeriodStart: "1607534563", prizePeriodSeconds: "120", ticketName: "Ticketz", ticketSymbol: "TICK", sponsorshipName: "Sponsorshipz", sponsorshipSymbol: "SPON", ticketCreditLimitMantissa: "10000000000000000", ticketCreditRateMantissa: "83333333333333", numberOfWinners: "3", splitExternalErc20Awards: "true" }

*Decimals (parameter 3): 
18

The return value is the address of the newly created Stake Prize Pool.


* \*\*\*\*[**yVault Prize Pool Builder**](yvault-prize-pool-builder.md) ****creates yVault Prize Pools using the Multiple Winners prize strategy.

For example the following tuples will create a yVault Prize Pool: 

```javascript
  function createVaultMultipleWinners(
    VaultPrizePoolBuilder.VaultPrizePoolConfig memory prizePoolConfig,
    MultipleWinnersBuilder.MultipleWinnersConfig memory prizeStrategyConfig,
    uint8 decimals
  ) external returns (yVaultPrizePool) 
```
*VaultPrizePoolConfig (parameter 1):
{vault: "0x5dbcF33D8c2E976c6b560249878e6F1491Bca25c", reserveRateMantissa: "10000000000000000", maxExitFeeMantissa: "10000000000000000", maxTimelockDuration: "1000000" }

*MultipleWinnersConfig (parameter 2):
{ rngService: "0xA932e74d5263A754Ea04432E5c53658434b0484B", prizePeriodStart: "1607534563", prizePeriodSeconds: "120", ticketName: "Ticketz", ticketSymbol: "TICK", sponsorshipName: "Sponsorshipz", sponsorshipSymbol: "SPON", ticketCreditLimitMantissa: "10000000000000000", ticketCreditRateMantissa: "83333333333333", numberOfWinners: "3", splitExternalErc20Awards: "true" }

*Decimals (parameter 3): 
18

The return value is the address of the newly created yVault Prize Pool.


### PrizePoolCreated event
All three of the above builder functions emit the PrizePoolCreated event:

```javascript
event PrizePoolCreated (
    address indexed creator,
    address indexed prizePool
);
```