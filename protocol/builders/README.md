---
description: Easily create preconfigured Prize Pools
---

# 🛠 Builders

Prize Pool Builders allow users to create preconfigured Prize Pools. Builders create all contracts using teeny tiny proxies, so creating prize games is cheap!

There are currently three specific prize builders, but the starting point for all Prize Pool Builders is the PoolWithMultipleWinnersBuilder contract.

## PoolWithMultipleWinnersBuilder

### **Create a Multiple Winners Compound Prize Pool**

The following function ****can be called to create a Compound Prize Pool using the Multiple Winners prize strategy:

```javascript
function createCompoundMultipleWinners(
  CompoundPrizePoolBuilder.CompoundPrizePoolConfig memory prizePoolConfig,
  MultipleWinnersBuilder.MultipleWinnersConfig memory prizeStrategyConfig,
  uint8 decimals
) external returns (CompoundPrizePool) 
```

* **CompoundPrizePoolConfig**: 

| Parameter | Description | Example |
| :--- | :--- | :--- |
| cToken | address | 0x5dbcF33D8c2E~~9~~76c6b560249878e6F1491Bca25c |
| reserveRateMantissa | uint | 10000000000000000 |
| maxExitFeeMantissa | uint | 10000000000000000 |
| maxTimelockDuration | seconds uint | 1000000 |

* **MultipleWinnersConfig**: 

| Parameter | Description | Example |
| :--- | :--- | :--- |
| rngService | The Random Number Generator address | 0xA932e74d5263A754Ea04432E5c53658434b0484B |
| prizePeriodStart | Unix Time | 1607534563 |
| prizePeriodSeconds | In seconds | 604800 |
| ticketName | string | Ticketz |
| ticketSymbol | string | TICK |
| sponsorshipName | string | Sponsorshipz |
| sponsorshipSymbol | string | SPON |
| ticketCreditLimitMantissa | uint | 10000000000000000 |
| ticketCreditRateMantissa | uint | 83333333333333 |
| numberOfWinners | uint | 3 |
| splitExternalErc20Awards | Boolean | true |

* **Decimals**: 

| Parameter | Description | Example |
| :--- | :--- | :--- |
| decimal | uint  | 18 |

The return value is the address of the newly created Compound Prize Pool.

### Create a Multiple Winners Stake Prize Pool

The following function should be called to create a Stake Prize Pool for a particular token using the multiple winners strategy:

```javascript
  function createStakeMultipleWinners(
    StakePrizePoolBuilder.StakePrizePoolConfig memory prizePoolConfig,
    MultipleWinnersBuilder.MultipleWinnersConfig memory prizeStrategyConfig,
    uint8 decimals
  ) external returns (StakePrizePool)
```

For example the following tuples will create a Stake Prize Pool for the WBTC-WETH Uniswap Liquidity Provider token:

* **StakePrizePoolConfig**

| Parameter | Description | Example |
| :--- | :--- | :--- |
| token | stake token | 0xbb2b8038a1640196fbe3e38816f3e67cba72d940 |
| maxExitFeeMantissa | uint | 10000000000000000 |
| maxTimelockDuration | seconds uint | 1000000 |

* **MultipleWinnersConfig**

| Parameter | Description | Example |
| :--- | :--- | :--- |
| rngService | The Random Number Generator address | 0xA932e74d5263A754Ea04432E5c53658434b0484B |
| prizePeriodStart | Unix Time | 1607534563 |
| prizePeriodSeconds | In seconds | 604800 |
| ticketName | string | Ticketz |
| ticketSymbol | string | TICK |
| sponsorshipName | string | Sponsorshipz |
| sponsorshipSymbol | string | SPON |
| ticketCreditLimitMantissa | uint | 10000000000000000 |
| ticketCreditRateMantissa | uint | 83333333333333 |
| numberOfWinners | uint | 3 |
| splitExternalErc20Awards | Boolean | true |

* **Decimals**

| Parameter | Description | Example |
| :--- | :--- | :--- |
| decimal | uint  | 18 |

The return value is the address of the newly created Stake Prize Pool.

### **Create a Multiple Winner yVault Pool**

The following function can be called to create a yVault Prize Pool using the Multiple Winners prize strategy:

```javascript
  function createVaultMultipleWinners(
    VaultPrizePoolBuilder.VaultPrizePoolConfig memory prizePoolConfig,
    MultipleWinnersBuilder.MultipleWinnersConfig memory prizeStrategyConfig,
    uint8 decimals
  ) external returns (yVaultPrizePool)
```

* **VaultPrizePoolConfig**:

| Parameter | Description | Example |
| :--- | :--- | :--- |
| cToken | address | 0x5dbcF33D8c2E~~9~~76c6b560249878e6F1491Bca25c |
| reserveRateMantissa | uint | 10000000000000000 |
| maxExitFeeMantissa | uint | 10000000000000000 |
| maxTimelockDuration | seconds uint | 1000000 |

* **MultipleWinnersConfig**:   


  | Parameter | Description | Example |
  | :--- | :--- | :--- |
  | rngService | The Random Number Generator address | 0xA932e74d5263A754Ea04432E5c53658434b0484B |
  | prizePeriodStart | Unix Time | 1607534563 |
  | prizePeriodSeconds | In seconds | 604800 |
  | ticketName | string | Ticketz |
  | ticketSymbol | string | TICK |
  | sponsorshipName | string | Sponsorshipz |
  | sponsorshipSymbol | string | SPON |
  | ticketCreditLimitMantissa | uint | 10000000000000000 |
  | ticketCreditRateMantissa | uint | 83333333333333 |
  | numberOfWinners | uint | 3 |
  | splitExternalErc20Awards | Boolean | true |

* **Decimals**

| Parameter | Description | Example |
| :--- | :--- | :--- |
| decimal | uint  | 18 |

The return value is the address of the newly created yVault Prize Pool.

### PrizePoolCreated event

All three of the above builder functions emit the PrizePoolCreated event:

```javascript
event PrizePoolCreated (
    address indexed creator,
    address indexed prizePool
);
```

