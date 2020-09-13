---
description: How Prize Pools Ensure Fair Play
---

# ⚖️ Fairness

When users play a game they want it to be fair. In PoolTogether, this means that everyone has contributed the same amount of interest to prizes they are eligible to win. Interest accrues over time, so the Prize Pool needs to measure and enforce the time that funds are held. Without this mechanism, it would be very easy to game the system by depositing right before a prize, having a chance to win, and withdrawing right after. 

Prize Pools measure the duration of time funds are held by accruing **credit** for each user at the **credit rate**. The longer a user holds tokens, the more credit they accrue.

Prize Pools enforce the duration of time funds are held by setting a **credit limit**. Once a credit limit is reached a user can withdraw instantly with no loss. If the credit limit has not been reached the user can either use a withdrawal **timelock** or pay an early exit contribution to the prize.  

## Credit

After a user deposits funds they begin to accrue credit according to the credit rate.  The credit rate is expressed in tokens per second.

For example: if the user deposits 100 DAI and the credit rate is 0.1, then they will have accrued 1 DAI in credit after 10 seconds.  Note that they cannot withdraw the 1 DAI credit; it's simply a measure of their contribution.

Users will accrue credit up until the **credit limit**.  The credit limit is a fraction, so a users credit limit is that fraction of their entire balance.  For example, if the user holds 100 DAI and the credit limit is 0.1, then they will accrue a maximum of 10 DAI in credit.

Once a deposit has accrued maximum credit, it is considered **matured**.

## Timelock

A deposit can be withdrawn instantly from the Prize Pool if it has matured.  Otherwise, upon withdrawal the deposit will be timelocked until it has matured, at which point the funds can be swept back to the user by anyone.

The timelock duration is calculated based on the spare credit the user has.  The spare credit for a withdrawal is their credit balance _less the credit limit for their remaining balance of tokens_.  For example: let's say a user has 100 DAI and is attempting to withdraw 10 DAI.  They currently have 9 DAI in credit and the credit limit is 0.1.  The user's spare credit is 9 - \(100 - 10\) \* 0.1 = 0 DAI.  If the user was instead withdrawing 50 DAI, then they would have 9 - \(100 - 50\) \* 0.1 = 4 DAI in spare credit.

The duration of the timelock is the time it takes for the withdrawal to mature less the spare credit.  For example: if the withdrawal amount is 100 DAI, and the user has 5 DAI in spare credit, and the credit rate is 0.1, then the timelock will be \(\(100 \* 0.1\) - 5\) / 0.1 = 50 seconds.  The spare credit is burned and the funds are placed in a timelock that can be swept after the duration has elapsed.

### Paying Off the Timelock

It's possible for a user to withdraw their funds instantly.  Instead of a timelock, the Prize Pool will capture the remaining contribution directly from the withdrawal amount. The user will receive the withdrawal amount less the remaining contribution.  Their spare credit will be burned.  We call this an **instant withdrawal.**

## What should the credit rate and credit limit for a pool be?

In principal, we want the timelock to be as short as possible and most users should never encounter it. We are trying to prevent abuse of the system by a small subset of users while keeping the smoothest experience for the majority of users.

At first glance the credit limit should simply be equal to the amount of interest a deposit would contribute over a prize period. But there are several factors that can change the cost / benefit balance for depositors, specifically:

* Any subsidies to the prize \(whether through sponsored deposits or direct additions\)
* Any rewards given to deposits through the token drips
* Fluctuations in the yield rate
* Total amount of outstanding tickets for a given prize
* Gas fees of entering and exiting the pool

To find the ideal credit limit it is best to estimate the **effective apr** a pool is offering.

**Example**

Assume a pool has 100,000 tickets sold and its yield source is returning 5% APR, that would infer a $96 weekly prize. It means over the course of 7 days, one ticket \(equaling 1 dollar deposited\) contributes $0.0009 to the prize and over the course of one day contributes $0.00012 to the prize.

In this situation, a credit limit should be set to $0.0009 and the credit rate and the credit rate set to $0.00012.

But this pool also has a $1,000 weekly prize subsidy. So the actual prize is $1,096 per week. That would mean the effective APR is 57%! If the credit rate and limit were set to the above it could be very profitably gamed. At 57% APR one ticket \(equaling 1 dollar deposited\) effectively represents $0.01 to the prize and over the course a week and $0.0015 to the prize each day.

Therefore the credit rate and credit limit need to at minimum be matched to these rates.

