---
description: Easily create preconfigured Prize Pools
---

# 🛠 Prize Pool Builders

Prize Pool Builders allow users to create preconfigured Prize Pools.  Currently there is a Compound Prize Pool Builder that creates Compound Prize Pools. 

New prize pool builders supporting new yield sources can be initiated via the governor contract. 

All prize pools generated are non-upgradeable. Although they are not upgradeable prize pools have an admin owner with extremely limited privileges. Those privileges are 1\) initiating emergency shutdown of a prize pool by detaching its prize strategy and only allow funds to be withdrawn. 2\) Adding a new controlled token to the prize pool. 

