# 👂 Token Listener

The Token Listener interface allows contracts to "listen" to the complete token lifecycle of mint, transfer and burn.  There are two functions that must be implemented: `beforeTokenMint` and `beforeTokenTransfer`

The beforeTokenMint function must be called whenever a token is minted.

```javascript
function beforeTokenMint(
    address to,
    uint256 amount,
    address controlledToken,
    address referrer
) external;
```

| Paramater Name | Parameter Description |
| :--- | :--- |
| to | The address that is receiving the newly minted tokens |
| amount | The amount of new tokens |
| controlledToken | The token being minted |
| referrer | The address that referred the user \(for rewards\) |

The beforeTokenTransfer function must be called whenever a token is transferred or burned.

```javascript
function beforeTokenTransfer(
    address from,
    address to,
    uint256 amount,
    address controlledToken
) external;
```

| Paramater Name | Parameter Description |
| :--- | :--- |
| from | The address that is sending the tokens |
| to | The address that is receiving tokens.  May be the zero address if burning. |
| amount | The amount of tokens |
| controlledToken | The token being transferred |



