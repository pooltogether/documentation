# Controlled Token

A ControlledToken is an [ERC20](https://eips.ethereum.org/EIPS/eip-20) token that allows a controller to mint and burn tokens.

## Setup

ControlledTokens are created using the ControlledTokenFactory.  Once created, the tokens are initialized:

```javascript
function initialize (
    string memory name,
    string memory symbol,
    TokenControllerInterface controller
)
```

The **name** and **symbol** are used when listing or displaying the ERC20, and the **controller** is the 

## Minting and Burning

The controller is the only address that is allowed to mint and burn tokens.  To mint tokens, the controller would call:

```javascript
function mint(address account, uint256 amount)
```

Then to burn tokens, the controller would call:

```javascript
function burn(address from, uint256 amount)
```

## TokenControllerInterface

This interface allows a smart contract to control the minting, burning and token lifecycle of a ControlledToken.

Only the controller is able to call **mint** and **burn**, and the controller can listen to token transfer events. 

The controller must implement the function:

```javascript
function beforeTokenTransfer(address from, address to, uint256 tokenAmount);
```

This function is called whenever tokens are minted, burned, or transferred.

