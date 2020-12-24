# ⛽ Gas Usage

PoolTogether is conscious that to become a truly lossless prize protocol the transaction fees involved must be minimal. The current design utilizes the Minimal Proxy Factory design where possible to reduce gas usage. 

Note that these fees are paid to the Ethereum Network and not to PoolTogether.  The amount a transaction costs in USD is calculated as: the amount of gas used \* gasPrice \* USD/ETH.

Here is a list of common actions and their costs:

| Function Call | Estimated Gas Cost | $USD \(40 GWei, $600/ETH\) |
| :--- | :--- | :--- |
| **Creating Pools with the Builder** |  |  |
| createCompoundPoolMultipleWinners\(\) | 1.3M  | 33 |
| createStakePoolMultipleWinners\(\) | 1.25M | 30 |
| createVaultPoolMultipleWinners\(\) | 1.2M | 30 |
| **Entering and Leaving Pools** |  |  |
| depositTo\(\) | 0.5M | 12 |
| withdrawInstantlyFrom\(\) | 0.5M | 12 |
| **Award Process** |  |  |
| RNG request - [Chainlink VRF](random-number-generator/chainlink-vrf.md) | 2 LINK | 20 \(@ 10 USD/LINK\) |
| startAward\(\) | 200k | 4.8 |
| completeAward\(\) | 250k+ \(variable\) | 6 |
| **Transferring Tickets** | 290k | 7 |



