---
description: How Prize Pools Ensure Fair Play
---

# ⚖️ Fairness

When users play a game they want it to be fair.  In PoolTogether, this means that everyone has contributed the same amount of interest to the prize.  Interest accrues over time, so the Prize Pool needs to measure and enforce the time that funds are held.

Prize Pools measure the duration of time funds are held by accruing **credit** for each user at the **credit rate**.  The longer a user holds tokens, the more credit they accrue.

Prize Pools enforce the duration of time funds are held using a withdrawal **timelock**.  If a user withdraws too early, then they have to wait.  If a user withdraws after a long enough time, then they get their funds back instantly.

### Credit

After a user deposits funds they begin to accrue credit according to the credit rate.  The credit rate is expressed in tokens per second.

For example: if the user deposits 100 DAI and the credit rate is 0.1, then they will have accrued 1 DAI in credit after 10 seconds.  Note that they cannot withdraw the 1 DAI credit; it's simply a measure of their contribution.

Users will accrue credit up until the **credit limit**.  The credit limit is a fraction, so a users credit limit is that fraction of their entire balance.  For example, if the user holds 100 DAI and the credit limit is 0.1, then they will accrue a maximum of 10 DAI in credit.

Once a deposit has accrued maximum credit, it is considered **matured**.

### Timelock

A deposit can be withdrawn instantly from the Prize Pool if it has matured.  Otherwise, upon withdrawal the deposit will be timelocked until it has matured, at which point the funds can be swept back to the user by anyone.

The timelock duration is calculated based on the spare credit the user has.  The spare credit for a withdrawal is their credit balance _less the credit limit for their remaining balance of tokens_.  For example: let's say a user has 100 DAI and is attempting to withdraw 10 DAI.  They currently have 9 DAI in credit and the credit limit is 0.1.  The user's spare credit is 9 - \(100 - 10\) \* 0.1 = 0 DAI.  If the user was instead withdrawing 50 DAI, then they would have 9 - \(100 - 50\) \* 0.1 = 4 DAI in spare credit.

The duration of the timelock is the time it takes for the withdrawal to mature less the spare credit.  For example: if the withdrawal amount is 100 DAI, and the user has 5 DAI in spare credit, and the credit rate is 0.1, then the timelock will be \(\(100 \* 0.1\) - 5\) / 0.1 = 50 seconds.  The spare credit is burned and the funds are placed in a timelock that can be swept after the duration has elapsed.

### Paying Off the Timelock

It's possible for a user to withdraw their funds instantly.  Instead of a timelock, the Prize Pool will capture the remaining contribution directly from the withdrawal amount. The user will receive the withdrawal amount less the remaining contribution.  Their spare credit will be burned.  We call this an **instant withdrawal.**

