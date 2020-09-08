# 💰 Rewards

Governance can reward users of any Prize Pool with ERC20 tokens.  These rewards are called **Drips** and they are managed by the protocol [Comptroller](overview.md#comptroller).

Rewards are defined per-pool, and can reward users based on the balance they hold, their deposit volume for a time period, or for their deposit referral volume for a time period.

Users accrue a claimable balance of reward tokens.  The Comptroller will hold a balance of the token being dripped, and transfer tokens to the user when tokens are claimed.

## Balance Drips

Balance Drips distribute **drip tokens** at a certain rate per second.  If a Balance Drip drips out 1 token per second, then after 100 seconds 100 tokens will have been dripped out.  ****Drip tokens are simply the ERC20s that are being distributed.

The "Balance" portion of a Balance Drip refers to the fact that each user is dripped tokens at a rate proportional to their share of tokens of the total supply of **measure** tokens.  Measure tokens are the ERC20s that determine a user's share of the drip.

For example, let's define a Balance Drip for a Compound Single Random Winner Prize Pool called Weekly Dai:

* The measure token is the Ticket token
* The drip token will be Dai \(users will accrue Dai by holding Tickets\)
* Drip rate of 0.01 tokens per second \(about 40 Dai per hour\)

If a user holds 50% of the total supply of Tickets for one hour, then they will accrue ~20 Dai as a claimable balance.







