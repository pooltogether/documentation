# Compound Prize Pool

The Compound Prize Pool is a Prize Pool that uses [Compound](https://compound.finance) as the yield source.  When a Compound Prize Pool is created it is configured with the cToken to use for minting and redeeming.

## Retrieving the Underlying cToken

To access the underlying [cToken](https://compound.finance/docs/ctokens), simply call this function:

```javascript
function cToken() returns (address);
```

