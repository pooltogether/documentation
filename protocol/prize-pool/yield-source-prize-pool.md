---
description: A prize pool that uses a yield source to generate prizes.
---

# 📈 Yield Source Prize Pool

The Yield Source Prize Pool uses a yield source contract to generate prizes.  Funds that are deposited into the prize pool are then deposited into a yield source.

## Retrieving the Yield Source

You can access the yield source contract by using this function:

```javascript
function yieldSource() public view returns (IYieldSource);
```

See the [IYieldSource](../yield-sources.md#yield-source-interface) interface for more information on the yield source.



