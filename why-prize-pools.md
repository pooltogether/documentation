# 🧐 Why Prize Pools?

**PoolTogether V3 allows developers to create a unique prize games for any ERC20 asset. This section will give an overview of the different types of Prize Pools, what a Prize Strategy is, and offer some use cases.**

## Prize Pools

Prize Pools are contracts where users stake a token to be eligible for a prize.  Tokens are given away as prizes by a Prize Strategy that uses a random number to select winners.  There are two different types of Prize Pools: Compound Prize Pools and Stake Prize Pools.

#### Compound Prize Pools

Compound Prize Pools allow users to stake an ERC20 into a Prize Pool contract which then locks the staked asset into Compound. The yield from the all staked tokens is awarded as a prize.

#### ![](https://lh4.googleusercontent.com/Ko7A7PVLKdS9jX5sOQK939-I26X5FgeieSGEc0YaFemosButAKAJn0yVLWWvTNLPp6O2zEtxi_1SgwWly_ljGFSYVj3HAJRNccpGnziK1O5pkNzghZlsv3Pb1YwgYd3nPWKpBqXq)

#### Stake Prize Pools

Stake Prize Pools allow any ERC20 token to be staked, but do not place the token into a yield source.  In this way Stake Prize Pools can make a game out of **any ERC20 token**; which allows for a wide variety of Prize Pools.  ERC20 and ERC721 prizes must be added manually to the prize pool.  
****![](https://lh3.googleusercontent.com/R2qdUOkcD-9q392jEC8StwHhC5FLArq-ehIx8EnxNDe0tZZNUKQ2MGKTdkEKpl06O0ajNKqTEf0Oexhwf2hesi65Hf3L_rLyYm3rWWdyYbbjUMwjsYSVRO36sDP700cZKf_EyvV8)  
****

## Prize Strategy

The Prize Strategy Contract’s main role is distributing the prize. It is completely customizable and allows for everything from the number of winners, how often the prize is awarded, and other parameters to be changed. 

The Strategy Contract has the ability to give out any tokens that have accrued to the Prize Pool contract as well any excess balance of the staked token. Excess balance can be from either accrued interest or fairness fees paid by users \(more on that later\).  Excess balance is awarded to winners as tickets, and ERC20 and ERC721 tokens that have accrued to the prize pool are simply transferred to the winners.![](https://lh6.googleusercontent.com/7PC8t_bC5sRRc6OhvewACQpq2QTwRbQCXo0gkkFj3XIpRp-pBjlnVAslrCMTy4gFVAZrKSFa_vjFQdX4MVvyFNJ0d7udXFQlCwGjYL3AtdwK7PQaJUInxsartHuWWWwVkZDXmz9b)  ****![](https://lh3.googleusercontent.com/uE63a9iqZLb8E0I5EcqrHoioBwX_mYstfP_Cn3KvkT_LZQe2jfeqha7_wESvnMJDOxy74YJMSd_YzLns0g21Wn96nc85IbXJO4tgpk63da82uFFd0Kqvj3LKYrQyY13JlgcuPHTA)

## Use Cases

### NFT Art Giveaway

An artist could use a Stake Prize Pool to giveaway a piece of their art each month.  The artist could require that the users stake their personal token for a chance to win.  

1. Users stake artist's personal token in the prize pool
2. Artist adds NFT to the prize pool
3. NFT is awarded randomly to a staked user!

**This would drive demand for the personal token; so that users can participate in this giveaway.**

![NFT Art Giveaway](https://lh4.googleusercontent.com/YEeplkrmMaKy6K83Im4XDFRK1wimGYg540Eway0cHCvNtMq-dEK2oqjjplExqM8jQs0QfU4oxl6m2T-Fn9iH3nBCbuRAjQcJ5u5O9c5MmRISd-kOqKMRbXUNc6mHT69EjpDaLYdD)

### Rewarding Liquidity Providers

A new project could also incentivize liquidity provision on Uniswap by creating a Stake Prize Pool for the Uniswap pair's LP token.

1. User provides liquidity to a Uniswap pair and receives the LP token in exchange
2. The user stakes the LP token in the pair's Stake Prize Pool
3. The protocol to which liquidity is being provided can give away prizes. 

![Incentivizing Liquidity Providers](https://lh4.googleusercontent.com/RH74S-iBaMr98BIktGTv1icki2ppZCzffrp46NmLweKDNyUDmSP0EXqQDuBcS3hpmlPWFF6rW8GH7nTYAnuW1ICTSULtc09XaGda0-DOXLycZ27qPBxDHuERngNfc5QChaEsCJnB)

### Fair Launch Game

If a project wants to award governance tokens in a lottery-like fashion, they could use a Stake Prize Pool that award prizes at a higher frequency.  The users could stake LP tokens as above, but the prizes could be governance tokens.

![Fair Launch Game](https://lh5.googleusercontent.com/JfbieQz6jQywXpR83XS67XMdpYGjRh2nWU71o544pc7zsgN3lUgFJnNBOOvfHNgB_25Fn6FyMWi1OLyd4XuxVxkUaZWU3NhCCabF0iVNPHnOZq7-1SztChoDNc-Su4Ij-9N534CN)

  
****

