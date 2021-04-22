---
description: Yield sources generate yield for prize pools.
---

# 👨‍🌾 Yield Sources

A **Yield Source** contract is used by a Yield Source Prize Pool to generate yield for prizes.

[See the Specification on Github ](https://github.com/pooltogether/yield-source-interface)

The yield source just needs these properties:

*  The deposit asset is the same as the asset that accrues.  I.e. if users deposit Dai into the yield source, then it should yield Dai as well
* Yield must always be increasing.  The mechanics of the Prize Pool require yield to always go up, as it's a no-loss system.  The yield source must protect depositor's collateral.

There are implementations for all of the major yield sources, such as Compound, Aave, and Yearn.

## Yield Source Interface

The yield source interface is very simple; it just needs to support four functions:

```javascript
/// @title Defines the functions used to interact with a yield source.  The Prize Pool inherits this contract.
/// @notice Prize Pools subclasses need to implement this interface so that yield can be generated.
interface IYieldSource {

  /// @notice Returns the ERC20 asset token used for deposits.
  /// @return The ERC20 asset token
  function depositToken() external view returns (address);

  /// @notice Returns the total balance (in asset tokens).  This includes the deposits and interest.
  /// @return The underlying balance of asset tokens
  function balanceOfToken(address addr) external returns (uint256);

  /// @notice Supplies tokens to the yield source.  Allows assets to be supplied on other user's behalf using the `to` param.
  /// @param amount The amount of `token()` to be supplied
  /// @param to The user whose balance will receive the tokens
  function supplyTokenTo(uint256 amount, address to) external;

  /// @notice Redeems tokens from the yield source.
  /// @param amount The amount of `token()` to withdraw.  Denominated in `token()` as above.
  /// @return The actual amount of tokens that were redeemed.
  function redeemToken(uint256 amount) external returns (uint256);

}
```



