# Bounties

We value contributions from the community to strengthen the security of the core contracts. We want to reward any hackers in good faith who report vulnerabilities.

The scope of this bounty includes the PoolTogether smart contracts. The determination of the bug severity will be made by the PoolTogether team.  We determine the severity of an issue according to the [Smart Contract Security Alliance Severity Levels](https://www.smartcontractsecurityalliance.com/)

Payouts will be as follows:

High: $25,000 DAI  
Medium: $10,000 DAI  
Low: $1,000 DAI

The issue must:

* be a previously unreported, non-public vulnerability.
* include enough detail for us to identify and reproduce the problem

All reports should start with an email to [hello@pooltogether.us](mailto:hello@pooltogether.us) and they will receive a response within 24 hours. Non-security issues are not eligible for this bounty.

Determinations of eligibility and all terms related to this award are at the sole and final discretion of the PoolTogether team.

## Past Bounties

### PermitAndDepositDai Contract: Unrestricted Sender

Severity: Medium / High  
Date: Thursday, October 22nd, 2020  
Reporter: Kevin Foesenek  
Payout: $20,000 USD of WETH \([transaction](https://etherscan.io/tx/0xdd9fcf07a29a376b811c775d34cef4ceddf6e720981da34ac7142a8c38e7e7a6)\)

**Vulnerability**  
Just prior to launch a security researcher discovered a flaw in the PermitAndDepositDai contract.  This flaw would have allowed an attacker to front-run the "deposit" transaction and take the deposited amount.  This would have affected any new deposits to the system.

**Mitigation**  
References to the contract were removed from the user interface, and a fix was immediately deployed to mainnet and published via NPM.

