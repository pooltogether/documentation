---
description: How Prize Pools Ensure Fair Play
---

# ⚖️ Fairness

When users play a game they want it to be fair. In PoolTogether, this means that everyone has contributed the same amount of interest to prizes they are eligible to win. Interest accrues over time, so the Prize Pool needs to measure and enforce the time that funds are held. Without this mechanism, it would be very easy to game the system by depositing right before a prize, having a chance to win, and withdrawing right after.

Prize Pools measure the duration of time funds are held by accruing **credit** for each user at the **credit rate**. The longer a user holds tokens, the more credit they accrue.

Prize Pools enforce the duration of time funds are held by setting a **credit limit**. Once a credit limit is reached a user can withdraw instantly with no loss. If the credit limit has not been reached the user can either use a withdrawal **timelock** or pay an early exit contribution to the prize.

## Credit

After a user deposits funds they begin to accrue credit according to the credit rate. The credit rate is expressed in tokens per second.

For example: if the user deposits 100 DAI and the credit rate is 0.1, then they will have accrued 1 DAI in credit after 10 seconds. Note that they cannot withdraw the 1 DAI credit; it's simply a measure of their contribution.

Users will accrue credit up until the **credit limit**. The credit limit is a fraction, so a users credit limit is that fraction of their entire balance. For example, if the user holds 100 DAI and the credit limit is 0.1, then they will accrue a maximum of 10 DAI in credit.

Once a deposit has accrued maximum credit, it is considered **matured**.



