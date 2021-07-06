---
description: What is a PoolTogether Loot Box?
---

# 🏴‍☠️ Loot Box

## Overview

A Loot Box is an address that can be controlled by the owner of an ERC721. Any ERC721 can have an associated Loot Box address, to which tokens and pretty much anything can be sent to. Anyone can "plunder" the Loot Box for tokens, and those tokens will be sent to the owner of The ERC721.

In this way, Loot Boxes allow addresses to be traded like NFTs.

The code can be found here: [https://github.com/pooltogether/loot-box](https://github.com/pooltogether/loot-box)

## How it works

A LootBox contract ephemerally exists within a transaction. The owner of an `ERC721` owns the LootBox.

1. A `ERC721` is created by calling `createERC721Controlled()` on the `ERC721ControlledFactory` by anyone:

```javascript
  function createERC721Controlled(
    string memory name,
    string memory symbol,
    string memory baseURI
  ) external returns (ERC721Controlled)
```

1. `mint()` can then be called on the `ControlledERC721` which effectively creates a LootBox with an Owner defined by the `to` field:

```javascript
function mint(address to) external onlyAdmin returns (uint256)
```

1. The LootBox address is calculated by calling: 

```javascript
computeAddress(address erc721, uint256 tokenId)
```

1. Tokens are transferred/minted to this address. In the case of PoolTogether, these are usually external ERC20, ERC721 and ERC1155 rewards for a Prize Period.
2. Anyone can call `plunder()` on the LootBox controller which will transfer all the passed tokens to the LootBox owner.

```javascript
function plunder(
  address erc721,
  uint256 tokenId,
  address[] calldata erc20s,
  WithdrawERC721[] calldata erc721s,
  WithdrawERC1155[] calldata erc1155s
)
```

where `erc20s` is defined as an array of ERC-20 addresses,

`erc721s` is defined as:

```c
struct WithdrawERC721 {
  address token;
  uint256[] tokenIds;
}
```

and `erc1155s` as:

```c
struct WithdrawERC1155 {
  address token;
  uint256[] ids;
  uint256[] amounts;
  bytes data;
}
```

