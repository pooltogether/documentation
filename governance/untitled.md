# 💰 Comptroller

All Prize Pools link to a global protocol Comptroller.  The Comptroller is owned by governance, and determines the reserve rate and rewards.

## Reserve Rate

The reserve rate is the portion of the prize that is allocated to the protocol and retained as sponsorship in the Prize Pool.

## Rewards

Governance can reward users of any Prize Pool with ERC20 tokens.  Rewards are defined per-pool, and can reward users based on the balance they hold, their deposit volume for a time period, or for their deposit referral volume for a time period.

Users accrue a claimable balance of reward tokens.  The Comptroller will hold a balance of the token being dripped, and transfer tokens to the user when tokens are claimed.

## Balance Drips

Balance Drips distribute **drip tokens** at a rate per second.  If a Balance Drip drips out 1 token per second, then after 100 seconds 100 tokens will have been dripped out.  ****Drip tokens are simply the ERC20s that are being distributed.

The "Balance" portion of a Balance Drip refers to the fact that each user is dripped tokens at a rate proportional to their share of tokens of the total supply of **measure** tokens.  Measure tokens are the ERC20s that determine a user's share of the drip.

For example, let's define a Balance Drip for a Compound Single Random Winner Prize Pool called Weekly Dai:

* The measure token is the Ticket token
* The drip token will be Dai \(users will accrue Dai by holding Tickets\)
* Drip rate of 0.01 tokens per second \(about 40 Dai per hour\)

If a user holds 50% of the total supply of Tickets for one hour, then they will accrue ~20 Dai as a claimable balance.

The Comptroller will hold a balance of the drip tokens.  When a user claims their unclaimed balance, the Comptroller will transfer the unclaimed balance of tokens to the user.

## Volume Drips

Volume Drips distribute an amount of **drip tokens** per time period.  For example, a volume drip may drip 1000 Dai tokens each week.

The "Volume" portion of the drip refers to the fact that a user receives drip tokens proportional to their portion of the deposit volume for that period.

For example, let's define a Volume Drip for a Compound Single Random Winner Prize Pool called Daily USDC:

* The measure token is Tickets
* The drip token will be USDC
* The drip amount will be 1000 USDC
* The drip period is one week and it starts on Friday at midnight

If a user deposits 5000 USDC during that week and a total of 10,000 USDC was deposited, then they will be able to claim 500 USDC from the Comptroller.

## Referral Volume Drips

Referral Volume Drip work identically to volume drips, but they only apply to referrals.

You may recall the [PrizePool\#depositTo](../protocol/prize-pool/#depositing) function: its last parameter is a referrer address.  This address is used to calculate referral volume in the system.

Referral rewards allow Dapps to capture rewards and themselves benefit from the protocol.



