---
description: Create a Prize Pool that Uses a Custom Yield Source
---

# Custom Yield Sources

If you want to generate yield using a protocol that isn't currently supported by PoolTogether, you can build a custom **Yield Source**.  Then when you create a Prize Pool, you can configure it to use your custom yield source.

The yield source just needs these properties:

*  The deposit asset is the same as the asset that accrues.  I.e. if users deposit Dai into the yield source, then it should yield Dai as well
* Yield must always be increasing.  The mechanics of the Prize Pool require yield to always go up, as it's a no-loss system.  The yield source must protect depositor's collateral.

## Yield Source Interface

The yield source interface is very simple; it just needs to support four functions:

```javascript
interface YieldSourceInterface {

  function token() external view returns (address);

  function balanceOf(address addr) external returns (uint256);

  function supplyTo(uint256 amount, address to) external;

  function redeem(uint256 amount) external returns (uint256);
  
}
```

### token\(\)

The token function must return the ERC20 address of the token used to deposit.  Users will deposit this token into the yield source.

### balanceOf\(address\)

The balanceOf function returns the balance of the given address, denominated in the ERC20 token mentioned above.  This balance must increase as yield accrues.

### supplyTo\(uint256, address\)

The supplyTo function allows a user to deposit tokens into the yield source.  The tokens will be the above ERC20 tokens, and they may deposit to themselves or any other Ethereum address.

### redeem\(uint256\)

The redeem function allows users to withdraw their tokens.  Their withdrawal will be the ERC20 tokens mentioned above and be transferred to them.  The actual amount transferred is the return value of the function, as some yield sources may incur fees.



