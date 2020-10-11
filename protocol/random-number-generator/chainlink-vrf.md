# Chainlink VRF

**A verifiable random function is a pseudo-random function whose output is unique and can be publicly verified.**

ChainLink has implemented their VRF using public key cryptography.  It works like so:

1. The user creates a “seed” value
2. A ChainLink operator, who has publicly committed to a keypair, uses their secret to sign the seed value.
3. The user is able to verify that the operator has signed the seed value, and consume the signature as the “random number”.  

**ChainLink VRF Documentation:** [**https://docs.chain.link/docs/chainlink-vrf**](https://docs.chain.link/docs/chainlink-vrf)

This approach has some benefits in that the operator cannot “lie”: they must sign the seed using the secret they have committed to.  The algorithm is also instantaneous: there is no delay or waiting period to get the answer. ****

This approach also has some drawbacks.  Given a seed value, the operator will know ahead of time what the random number will be.  The operator may also \*withhold\* the answer if they don’t like it**.**  We mitigate the operator's foreknowledge by locking the Tickets; even if the operator knows the answer they are unable to manipulate the Ticket structure.  If the operator withholds the answer, we can simply switch operators.

## Usage

A [Single Random Winner](../prize-strategy/single-random-winner/) prize strategy can be created with a [RNGChainlink](../../networks.md) RNG service.  However, the RNGChainlink service requires LINK tokens as payment.

**LINK tokens must be supplied to the prize strategy to pay for the RNG service charges**

