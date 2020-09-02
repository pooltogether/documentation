---
description: How Prize Pools Ensure Fair Play
---

# ⚖️ Fairness

When users play a game they want it to be fair.  In PoolTogether, this means that everyone has contributed the same amount of interest to the prize.  Interest accrues over time, so the protocol needs to measure and enforce the time that funds are held.

The protocol measures the duration of time funds are held by accruing **credit** for each user at the **credit rate**.  The longer a user holds tokens, the more credit they accrue.

The protocol enforces the duration of time funds are held using a withdrawal **timelock**.  If a user withdraws too early, then they have to wait.  If a user withdraws after a long enough time, then they get their funds back instantly.

### Credit

After a user deposits funds they begin to accrue credit according to the credit rate.  The credit rate is expressed in tokens per second.

For example: if the user deposits 100 DAI and the credit rate is 0.1, then they will have accrued 1 DAI in credit after 10 seconds.  Note that they cannot withdraw the 1 DAI credit; it's simply a measure of their contribution.

Credit does not accrue perpetually, however, as there is a credit limit.  More on that later.

### Timelock

When a user wishes to withdraw the protocol will first check to see if they have contributed enough to the prize. The minimum contribution is determined by the **fairness fee**.  The fairness fee is expressed as a fraction of the amount being withdrawn. For example: if the fairness fee is 0.01 and a user is withdrawing 100 DAI, then they must contribute 1 DAI to the Prize Pool.

Next, the protocol will calculate how much spare credit the user has.  The user's spare credit is their credit balance _less the fairness fee for their remaining balance of tokens_.  For example: let's say a user has 100 DAI and is attempting to withdraw 10 DAI.  They currently have 9 DAI in credit and the fairness fee is 0.1.  The user's spare credit is 9 - \(100 - 10\) \* 0.1 = 0 DAI.  If the user was instead withdrawing 50 DAI, then they would have 9 - \(100 - 50\) \* 0.1 = 4 DAI in spare credit.

Finally, the protocol determines the duration of the timelock. The minimum contribution less the spare credit is the **remaining contribution**.  The duration of the timelock is the amount of time the remaining contribution would take to accrue at the credit rate.  For example: if the minimum contribution is 10 DAI, the user has 5 DAI in spare credit, and the credit rate is 0.1, then the timelock will be \(10 - 5\) / 0.1 = 50 seconds.  The spare credit is burned and the funds are placed in a timelock that can be withdrawn after the duration has elapsed.

### Paying Off the Timelock

It's possible for a user to withdraw their funds instantly.  Instead of a timelock, the protocol will capture the remaining contribution directly from the withdrawal amount. The user will receive the withdrawal amount less the remaining contribution.  Their spare credit will be burned.  We call this an **instant withdrawal.**

### Credit Limit

Users will accrue credit up until the **credit limit**.  The credit limit is the fairness fee for their entire balance.  For example, if the user holds 100 DAI and the fairness fee is 0.1, then they will accrue a maximum of 10 DAI in credit.

