---
description: A summary of governance-managed controls
---

# 🕹️ Controls

PoolTogether governance primarily controls:

* Protocol prize pools
* Token Faucets \(liquidity mining\)
* Protocol treasury
* Reserve

## Protocol Prize Pools

The protocol owns a subset of the prize pools.  Ownership means that governance can execute privileged actions only available to the owner.

Each prize pool consists of the prize pool contract and the prize strategy contract.  These two contracts can have different owners, but typically the owner is the same.

Prize Pool actions include:

* Setting a prize pool's early exit fees
* Setting a prize pool's liquidity cap
* Setting the prize strategy for a prize pool

Prize Strategy actions include:

* Setting the number of winners
* Setting whether to split external awards among the winners
* Configuring the Random Number Generator
* Managing external awards
* Configuring token and prize listener contracts

## Token Faucets

The protocol owns a set of token faucets, and can create more.  Each faucet is bound to a prize pool as a token listener and drips POOL tokens to the users.  The original program is time-limited, but governance can:

* Deposit more tokens into each faucet
* Change the drip rate of each faucet
* Create new token faucets for new prize pools

## Protocol Treasury

The initial token distribution allocated 60% of the POOL token supply to the protocol treasury.  These tokens will be unlocked over two years by the TreasuryVesterForTreasury contract.  Anyone can execute the vesting contract to disburse more tokens to the protocol treasury.

The protocol treasury is held by the Timelock contract, which is the contract that execute proposals submitted by governance.  Proposals could do things like:

* Transfers POOL tokens to a recipient
* Approve POOL token spend by another contract, and then call the contract

## Reserve

The reserve contract is owned by governance, and determines the portion of interest earned by each prize pool that is captured as reserve funds.  Every prize pool created by the builders is linked to the reserve.  Governance can:

* Change the reserve rate.  This rate is the portion of interest that is captured for the reserve.
* Withdraw the reserve from a prize pool.

