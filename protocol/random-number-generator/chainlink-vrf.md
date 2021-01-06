# Chainlink VRF

**A verifiable random function is a pseudo-random function whose output is unique and can be publicly verified.**

ChainLink has implemented their VRF using public key cryptography.  It works like so:

1. The user creates a “seed” value
2. A ChainLink operator, who has publicly committed to a keypair, uses their secret to sign the seed value.
3. The user is able to verify that the operator has signed the seed value, and consume the signature as the “random number”.  

**ChainLink VRF Documentation:** [**https://docs.chain.link/docs/chainlink-vrf**](https://docs.chain.link/docs/chainlink-vrf)

This approach has some benefits in that the operator cannot “lie”: they must sign the seed using the secret they have committed to.  The algorithm is also instantaneous: there is no delay or waiting period to get the answer. ****

## Usage

To use the [RNGChainlink](../../networks.md) RNG service, create a new prize pool using the service or set it on an existing pool.

🚨🚨🚨 **Chainlink RNG requires 2 LINK tokens per RNG request** 🚨🚨🚨

🚨🚨🚨 **You must deposit LINK into the PRIZE STRATEGY** 🚨🚨🚨



