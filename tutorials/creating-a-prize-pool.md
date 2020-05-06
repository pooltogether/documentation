---
description: How to create your own PoolTogether Prize Pool
---

# Creating a Prize Pool

[Prize Pools](../contracts/prize-pool/) can be created using our [Builder](https://builder.pooltogether.com/) interface or programmatically using the [Prize Pool Builders](../contracts/builders.md).

Before creating the Prize Pool, you'll need to decide:

* Which Compound cToken to use for yield
* How frequently prizes should be awarded
* Whether you wish to award the prize to a single winner or employ a custom strategy

## Using the Builder Tool

The PoolTogether Prize Pool Builder allows you to create a Prize Pool that awards the prize to a single user.

To begin, navigate to the [Builder](https://builder.pooltogether.com).  Once you connect your wallet, you should see something like:

![](../.gitbook/assets/screen-shot-2020-05-05-at-4.52.53-pm.png)

This will be the configuration to use for the Prize Pool.  The Prize Period is expressed in seconds; so the above configuration is showing a daily prize pool, since there are 86400 seconds in a day.

The above configuration uses cDai as the yield mechanism.  This means that it will be a Dai pool: users will need to deposit Dai into the Pool in order to get tickets or sponsorship.

FINISH TUTORIAL HERE



