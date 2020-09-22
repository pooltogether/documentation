# yVault Prize Pool

The yVault Prize Pool is a prize pool that uses a yEarn [yVault](https://yearn.finance/vaults) as a yield source.  When a yVault Prize Pool is created it is configured with a yVault to deposit and withdraw from.  This particular prize pool retains a small reserve in order to cover yVault withdrawal fees.  

## Retrieving the Underlying yVault

The underyling yVault address can be retrieve using the function:

```javascript
function vault() returns (address);
```

## Prize Pool Reserve

yVaults may charge users a fee upon withdrawal if the amount exceeds the vault holdings.  In order to compensate for this the yVault Prize Pool retains a reserve at a rate matching the fee.  For example, if the yVault fee is 0.5% then the yVault Prize Pool will retain 0.5% of the deposits as reserve.  The reserve will **not** be exposed to the Prize Strategy for distribution: it will be retained to cover withdrawal fees.

{% hint style="info" %}
It may appear that users will immediately lose 0.5% upon deposit, but it is important to note that the reserve works in tandem with the [credit system](fairness.md).  The credit system ensures users participate long enough to contribute to the prize and the reserve.
{% endhint %}

The reserve rate can be retrieved using:

```javascript
function reserveRateMantissa() returns (uint256);
```

This returns the reserve rate as a 18 decimal fixed point number \(like Ether\).

The _owner_ of the prize pool can set the reserve rate using the function:

```javascript
function setReserveRateMantissa(
    uint256 _reserveRateMantissa
) external onlyOwner
```



