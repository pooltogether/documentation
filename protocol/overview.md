---
description: What are Prize Games?
---

# 🌐 Overview

Prize games are pools of funds whose accrued interest is distributed as prizes.

## How it works

1. Users deposit funds into a Prize Pool.  They receive pool tokens in exchange.
2. The funds earn interest.
3. The interest is distributed by the Prize Strategy as pool tokens.
4. Users withdraw their funds by telling the Prize Pool to burn their pool tokens.

## [Prize Pools](prize-pool/)

Prize Pools are the central building block of prize games.  They pool user funds in a **yield source** and expose the yield to their **Prize Strategy**, which then disburses as desired.

Prize Pools can be differentiated in four primary ways:

* The yield source that generates no loss return
* The prize strategy used to determine frequency and distribution 
* The rewards offered to referrals and depositors
* The asset type the prize pool accepts for deposits
* Fairness parameters as determined by the owner

## [Prize Strategies](prize-strategy/)

Prize Strategies determine the prize distribution for the Prize Pool.  They can define any logic to allocate tokens that the prize pool accrues.  Specifically they can:

* Award yield in the Prize Pool as pool tokens
* Award ERC20 tokens held by the Prize Pool
* Award ERC721 tokens held by the Prize Pool

## Get started by

### [Buying Tickets in a Prize Pool](../tutorials/buying-tickets.md)

### Proposing to Governance:

* new prize pools
* new prize distribution strategies
* new yield sources for prize pools 
* anything you want.

The management of the protocol is facilitated via the Governor and Comptroller contracts. The core value of the protocol is facilitated by the Prize Pools, Prize Strategies and Prize Pool Builder contracts. The code can be found on [Github](https://github.com/pooltogether/pooltogether-contracts).

## Architecture

### [Prize Pool](prize-pool/)

The core primitive in PoolTogether is the Prize Pool, which manages ticket purchases, ticket redemptions, sponsorship, yield generation, and exposes the interest earned to the prize strategy.  

\*Prize Pools are not upgradeable, and have no admin functions other than an emergency shutdown.\*

### [Prize Strategy](prize-strategy/)

When a Prize Pool is created it is configured with a Prize Strategy.  The [Prize Strategy](prize-strategy/) determines how prizes are distributed, the early withdrawal exit fee for users, and the timelocked withdrawal duration.  The Prize Strategy is upgradeable and governed by the owner of the pool.

### [Prize Pool Builder](builders/)

The Prize Pool Builder generates new prize pools. The creation of new prize pools are initiated by the governor contract. Because prize pools are not upgradeable the yield generation source is set when the pool is created.

### [Comptroller](../governance/comptroller.md)

The comptroller sets the global reserve rate and manages the protocol reserves. The reserve rate is a global parameter that takes a percent of the yield accrued on a prize pool. 

### [Governor](../governance/governor.md)

The governor contract manages the protocol. As of September 1st 2020 the governor contract is managed by the PoolTogether Inc team. Management of this contract and the protocol itself will be decentralized.  

## Gas Station Network

PoolTogether supports the [Gas Station Network 2.0](https://github.com/opengsn/gsn).  The Gas Station Network is an open source system for meta-transactions on Ethereum.  The GSN consists of smart contracts, relayers, and a protocol to support a decentralized meta transaction relayer network.

The PoolTogether smart contracts support GSNv2 by inheriting from the [BaseRelayRecipient](https://github.com/opengsn/gsn/blob/master/contracts/BaseRelayRecipient.sol) contract and being bound to a trusted forwarder.  The contracts are tooled so that they support GSN transactions.

