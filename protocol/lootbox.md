---
description: What is a PoolTogether LootBox?
---

# 🌐 Overview

The PoolTogether LootBox is a permissionless container to hold Prize Pool tokens. 

Many different tokens can be controlled simply by one counterfactual address. 
The contract is created and destroyed within the same transaction. 
This cheap deployment and immediate destruction of the contract minimizes the gas overhead involved with containerization. 

The code can be found here: https://github.com/pooltogether/loot-box

## How it works

A LootBox contract ephemerally exists within a transaction. The owner of an `ControlledERC-721` owns the LootBox. 

1. A `ControlledERC-721` is created by calling `createERC721Controlled()` on the `ControlledERC-721ProxyFactory` by anyone:

```javascript
  function createERC721Controlled(
    string memory name,
    string memory symbol,
    string memory baseURI
  ) external returns (ERC721Controlled)
```

2. `mint()` can then be called on the `ControlledERC721` which effectively creates a LootBox with an Owner defined by the `to` field:

```javascript
function mint(address to) external onlyAdmin returns (uint256) 
```

3. The LootBox address is calculated by calling: 

```javascript
computeAddress(address erc721, uint256 tokenId)
```

4. Tokens are transferred/minted to this address. In the case of PoolTogether, these are usually external ERC-20, ERC-721 and ERC-1155 rewards for a Prize Period. 

5. Anyone can call `plunder()` on the LootBox controller which will transfer all the passed tokens to the LootBox owner. 

```javascript
plunder(
  address erc721,
  uint256 tokenId,
  address[] calldata erc20s,
  WithdrawERC721[] calldata erc721s,
  WithdrawERC1155[] calldata erc1155s
)
```
where `erc20s` is defined as an array of ERC-20 addresses,

`erc721s` is defined as:

```javascript
struct WithdrawERC721 {
  address token;
  uint256[] tokenIds;
}
```

and `erc1155s` as:
```javascript
struct WithdrawERC1155 {
  address token;
  uint256[] ids;
  uint256[] amounts;
  bytes data;
}
```


