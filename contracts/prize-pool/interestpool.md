---
description: Pool deposits and allocate interest
---

# Interest Pool

Interest Pools allow users to pool their assets together and deposit them into a Compound Money Market.  Users will always control their principal, but the Interest Pool allocator controls the accrued interest on deposits.

A Prize Pool acts as the allocator for an Interest Pool.  Users can deposit directly into the Interest Pool to provide "sponsorship", or they can buy tickets from the Prize Pool.  The Prize Pool deposits the ticket deposits into the Interest Pool.

## Setup

InterestPools are created using the InterestPoolFactory.  The factory instantiates a minimal proxy to the current implementation.  The current implementation is not upgradeable, so the contract will not change once deployed.

```javascript
function initialize (
    CTokenInterface _cToken,
    ControlledToken _collateral,
    address _allocator
) external;
```

Called after an InterestPool is created, this function will bind the pool to the given CTokenInterface \(Compound cToken address\) and controlled token. 

The [ControlledToken](controlledtoken.md) will be used to represent a claim on the underlying collateral.  I.e. if a user deposits Dai into the interest pool, they will be minted collateral tokens so that they may redeem them at any time.  The ControlledToken's controller is required to be this InterestPool.

## Supplying and Redeeming Assets

Users may supply the underlying asset to the InterestPool.  In exchange they will receive collateral tokens at an exchange rate of 1:1.

A user may supply the underlying asset using:

```javascript
function supply(uint256 amount) external override;
```

Then when the user wishes to redeem their collateral tokens for the underlying assets they use:

```javascript
function redeem(uint256 amount) external override;
```

## Allocating Interest

You can check to see how much interest is available \(measured in the underlying asset\):

```javascript
function availableInterest() public view returns (uint256);
```

This is the amount of collateral tokens that is available for the **allocator** to mint.

When the allocator wishes to mint new collateral tokens, they may call:

```javascript
function allocateInterest(address to, uint256 amount) external onlyAllocator;
```



