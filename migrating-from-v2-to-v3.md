# 💱 Migrating from V2 to V3

Users of PoolTogether V2 can exchange their old tickets for new ones using the [new interface](https://app.pooltogether.com).  See the "accounts" section.

Users can exchange:

* Dai Pool tickets
* USDC Pool tickets
* Dai Pod tickets
* USDC Pod tickets

All of these tickets will be exchange 1:1 for new V3 Pool Dai tickets.  Note that you will not receive USDC back, you will be **exchanging your USDC Tickets for Dai Tickets**.

## Migrating Your Tickets

1. Do an ER20 token transfer to the migration contract.
2. The migration contract will transfer PcDAI to the sender.

PoolTogether V2 tickets and Pod shares are ERC777 tokens; this token standard triggers a callback on the recipient when the recipient is a contract.  When the migration contract is triggered it automatically send PcDAI back to the sender.

For the latest address see the [Networks](networks.md) page.  Look for the Migration contract on mainnet.

## Troubleshooting

The migration contract needs to be stocked with liquidity, so if your ticket exchange fails please contact us on [our Discord](https://discord.gg/hxPhPDW).

