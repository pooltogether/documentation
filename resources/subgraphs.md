---
description: Information on PoolTogether's subgraph integration.
---

# 📈 Subgraphs

Both the PoolTogether [app](https://app.pooltogether.com) and [reference](https://reference-app.pooltogether.com/) app use [subgraphs](https://thegraph.com) to index the protocols smart contract events. A cryptocurrency powered economy of participants work together to index various blockchains and make this data available in configurable and consumable form to front-end users. PoolTogether was the first set of subgraphs to be indexed in the The Graph's decentralized indexer network.

There are PoolTogether v3 subgraphs available for most networks that PoolTogether has been deployed on. There are separate subgraphs available for PoolTogether's [LootBox](../protocol/lootbox.md) on Ethereum Mainnet and Rinkeby \(fun fact: the LootBox indexes every single ERC20, ERC721 and ERC1155 transfer since the LootBox launched!\).

There are currently 3 separate subgraphs for different versions of deployed prize pool contracts. Some networks only have 1 or 2 subgraphs as they were introduced after the v3.3.8 changes.

### PrizePool Subgraphs

##### [Subgraph Code on GitHub](https://github.com/pooltogether/pooltogether-subgraph-v3)

<table>
  <thead>
    <tr>
      <th style="text-align:left">Subgraph</th>
      <th style="text-align:left">Version(s)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="2" style="text-align:left"><strong>Ethereum Mainnet</strong></td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/pooltogether-v3_1_0">Explorer</a>
      </td>
      <td style="text-align:left">3.0.0 - 3.3.1</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/pooltogether-v3_3_2">Explorer</a>
      </td>
      <td style="text-align:left">3.3.2 - 3.3.7</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/pooltogether-v3_3_8">Explorer</a>
      </td>
      <td style="text-align:left">3.3.8 and up</a>
      </td>
    </tr>
    <tr>
      <td colspan="2">
        &nbsp;
      </td>
    </tr>
    <tr>
      <td colspan="2" style="text-align:left"><strong>Rinkeby Testnet</strong></td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/rinkeby-staging-v3_1_0">Explorer</a>
      </td>
      <td style="text-align:left">3.0.0 - 3.3.1</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/rinkeby-v3_3_2">Explorer</a>
      </td>
      <td style="text-align:left">3.3.2 - 3.3.7</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/rinkeby-v3_3_8">Explorer</a>
      </td>
      <td style="text-align:left">3.3.8 and up</a>
      </td>
    </tr>
    <tr>
      <td colspan="2">
        &nbsp;
      </td>
    </tr>
    <tr>
      <td colspan="2" style="text-align:left"><strong>Polygon</strong></td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/pooltogether-polygon-v3_3">Explorer</a>
      </td>
      <td style="text-align:left">3.3.2 - 3.3.7</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/polygon-v3_3_8">Explorer</a>
      </td>
      <td style="text-align:left">3.3.8 and up</a>
      </td>
    </tr>
    <tr>
      <td colspan="2">
        &nbsp;
      </td>
    </tr>
    <tr>
      <td colspan="2" style="text-align:left"><strong>Xdai</strong></td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/pooltogether-xdai-v3_3">Explorer</a>
      </td>
      <td style="text-align:left">3.3.8 and up</a>
      </td>
    </tr>
    <tr>
      <td colspan="2">
        &nbsp;
      </td>
    </tr>
    <tr>
      <td colspan="2" style="text-align:left"><strong>POA Sokol</strong></td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether-sokol-v3_3">Explorer</a>
      </td>
      <td style="text-align:left">3.3.8 and up</a>
      </td>
    </tr>
  </tbody>
</table>

### LootBox Subgraphs

##### [Subgraph Code on GitHub](https://github.com/pooltogether/loot-box-subgraph)

<table>
  <tbody>
    <tr>
      <td colspan="2">
        &nbsp;
      </td>
    </tr>
    <tr>
      <td colspan="2" style="text-align:left"><strong>Ethereum Mainnet</strong></td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/lootbox-v1_0_0">Explorer</a>
      </td>
      <td style="text-align:left">3.0.0 and up</a>
      </td>
    </tr>
    <tr>
      <td colspan="2">
        &nbsp;
      </td>
    </tr>
    <tr>
      <td colspan="2" style="text-align:left"><strong>Rinkeby Testnet</strong></td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://thegraph.com/explorer/subgraph/pooltogether/ptv3-lootbox-rinkeby-staging">Explorer</a>
      </td>
      <td style="text-align:left">3.0.0 and up</a>
      </td>
    </tr>
  </tbody>
</table>
