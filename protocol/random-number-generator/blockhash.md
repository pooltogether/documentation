# Blockhash

The Blockhash RNG uses a future blockhash as the random number.  This is the least secure method of random number generation, but also the simplest and cheapest.

When a user request a random number their lock block will be the current block.  Their request is considered 'complete' when at least one block has been mined since the lock block.  Upon retrieval the last blockhash will be stored as the random number and returned.

## Usage

A prize strategy can use a [RNGBlockhash](../../resources/networks/) RNG service.  No additional work is needed: the blockhash service is free.

