# 🤖 Comptroller

The Comptroller contract sets the protocol reserve rate and manages reward distribution  

### Protocol Reserve Rate

The protocol reserve rate is a global percent of pool yield that is captured as "sponsorship". Sponsorship is collateral deposited into a prize pool contributing yield but without tickets \(and therefore not eligible for yield distribution\). 

Functionality, sponsorship increases the size of prizes without decreasing the chance of winning for depositors. 

### Protocol Rewards

The protocol has the ability to reward tokens to users based on 1\) user balances and 2\) users referral volume. 

These rewards are set as "drip rates" and can be set specific to each prize pool. They can drip out any ERC-20 token.  

