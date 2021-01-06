# 🧐 Why Prize Pools?

**PoolTogether V3 allows developers to create a unique prize games for any ERC20 asset. This section will give an overview of the different types of Prize Pools, what a Prize Strategy is, and offer some use cases.**

## Prize Pool

Prize Pools are contracts where users stake a token to be eligible for a prize.  Tokens are given away as prizes by a Prize Strategy that uses a random number to select winners.  There are two different types of Prize Pools: Compound Prize Pools and Stake Prize Pools.

#### Compound Prize Pools

Compound Prize Pools allow users to stake an ERC20 into a Prize Pool contract which then locks the staked asset into Compound. The yield from the all staked tokens is awarded as a prize.

#### ![](https://lh4.googleusercontent.com/Ko7A7PVLKdS9jX5sOQK939-I26X5FgeieSGEc0YaFemosButAKAJn0yVLWWvTNLPp6O2zEtxi_1SgwWly_ljGFSYVj3HAJRNccpGnziK1O5pkNzghZlsv3Pb1YwgYd3nPWKpBqXq)

#### Stake Prize Pools

Stake Prize Pools allow any ERC20 token to be staked, but do not place the token into a yield source.  In this way Stake Prize Pools can make a game out of **any ERC20 token**; which allows for a wide variety of Prize Pools.  ERC20 and ERC721 prizes must be added manually to the prize pool.  
****![](https://lh3.googleusercontent.com/R2qdUOkcD-9q392jEC8StwHhC5FLArq-ehIx8EnxNDe0tZZNUKQ2MGKTdkEKpl06O0ajNKqTEf0Oexhwf2hesi65Hf3L_rLyYm3rWWdyYbbjUMwjsYSVRO36sDP700cZKf_EyvV8)  
****

## Prize Strategy

The Prize Strategy's main role is distributing the prize. It is completely customizable and allows for everything from the number of winners, how often the prize is awarded, and other parameters to be changed. 

The Strategy Contract has the ability to give out any tokens that have accrued to the Prize Pool contract as well any excess balance of the staked token. Excess balance can be from either accrued interest or fairness fees paid by users \(more on that later\).  Excess balance is awarded to winners as tickets, and ERC20 and ERC721 tokens that have accrued to the prize pool are simply transferred to the winners.![](https://lh6.googleusercontent.com/7PC8t_bC5sRRc6OhvewACQpq2QTwRbQCXo0gkkFj3XIpRp-pBjlnVAslrCMTy4gFVAZrKSFa_vjFQdX4MVvyFNJ0d7udXFQlCwGjYL3AtdwK7PQaJUInxsartHuWWWwVkZDXmz9b)  ****![](https://lh3.googleusercontent.com/uE63a9iqZLb8E0I5EcqrHoioBwX_mYstfP_Cn3KvkT_LZQe2jfeqha7_wESvnMJDOxy74YJMSd_YzLns0g21Wn96nc85IbXJO4tgpk63da82uFFd0Kqvj3LKYrQyY13JlgcuPHTA)  
****

