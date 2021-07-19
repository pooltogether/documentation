# Blockhash

The Blockhash RNG uses a future blockhash as the random number. This is the least secure method of random number generation, but also the simplest and cheapest.

When a user request a random number their lock block will be the current block. Their request is considered 'complete' when at least one block has been mined since the lock block. Upon retrieval the last blockhash will be stored as the random number and returned.

## Usage

A prize strategy can use a [RNGBlockhash](https://github.com/pooltogether/documentation/tree/6c30eec9a3787b298b041b5d864e955c716185ba/networks.md) RNG service. No additional work is needed: the blockhash service is free.

