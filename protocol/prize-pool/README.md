---
description: Pool deposits and award accrued interest periodically as a prize
---

# 🏆 Prize Pools

## Introduction

Prize Pools allow funds to be pooled together into a no-loss yield source, such as Compound, and have the yield safely exposed to a separate Prize Strategy.  They are the primary way through which users interact with PoolTogether prize games.

There is a type of prize pool for each yield source.  For example, if you wish to use Compound you will use the Compound Prize Pool.

All Prize Pools share the functionality below.

## Owner

When a Prize Pool is created, the creator is set as the pool's "owner".  The owner is able to:

* Add additional pool tokens
* Change the Prize Strategy
* Set the credit rate and credit limit
* Shutdown the prize pool
* Transfer ownership
* Renounce ownership

## Pool Tokens

When a user deposits into a Prize Pool, they also request what type of pool token they receive in exchange.  These pool tokens are configured when the Prize Pool is created, and more can be added later.  These tokens can represent a claim on collateral, or anything the owner likes.

The default [Compound Prize Pool Builder](../builders/) creates a Ticket controlled token and a Sponsorship controlled token.  These tokens can be looked up on the corresponding [Prize Strategy](../prize-strategy/).

## Depositing  

Users can deposit into the Prize Pool using the **depositTo** function. A user is instantly minted tokens upon deposit.

```javascript
function depositTo(
    address to,
    uint256 amount,
    address controlledToken,
    address referrer
) external;
```

| Parameter | Description |
| :--- | :--- |
| to | The address to whom the controlled tokens should be minted |
| amount | The amount of the underlying asset the user wishes to deposit.  The Prize Pool contract should have been pre-approved by the caller to transfer the underlying ERC20 tokens. |
| controlledToken | The address of the token that they wish to mint.  For our default Prize Strategy this will either be the Ticket address or the Sponsorship address.  Those addresses can be looked up on the Prize Strategy. |
| referrer | The address that should receive referral awards, if any. |

Depositing fires the event:

```javascript
event Deposited(
    address indexed operator,
    address indexed to,
    address indexed token,
    uint256 amount
);
```

| Event Data | Description |
| :--- | :--- |
| operator | The caller that made the deposit |
| to | The address that received the minted tokens |
| token | The address of the controlled token that was minted |
| amount | The amount of both the underlying asset that was transferred and the tokens that were minted. |

## Withdrawing

When a user withdraws they may need to contribute to the prize according to the [fairness rules](../fairness.md).  They may either cover the contribution by time-locking their funds, or cover the contribution explicitly using funds.

### **Withdraw with Timelock**

Funds can be withdrawn losslessly by time-locking the funds.  The withdrawal amount will be unlocked at a later date at which point the funds can be swept back to the user.  The timelock duration is calculated based on the users accrued credit, the credit rate, and the fairness fee.

If the user has sufficient credit, the unlockTimestamp may be "now" and the funds are instantly swept to the `from` address.

To start a lossless withdrawal a user may call:

```javascript
function withdrawWithTimelockFrom(
    address from,
    uint256 amount,
    address controlledToken
) external returns (uint256 unlockTimestamp);
```

| Parameter Name | Parameter Description |
| :--- | :--- |
| from | The user from whom to withdraw.  This means you may withdraw on another user's behalf if they have given you an ERC20 allowance. |
| amount | The amount of collateral to withdraw. |
| controlledToken | The type of controlled token to withdraw. |

### Checking Timelock Balances

To see how many funds have been timelocked for a user you can call:

```javascript
function timelockBalanceOf(address user) external view returns (uint256)
```

After funds have been time-locked, you can see at what timestamp they'll be available:

```javascript
function timelockBalanceAvailableAt(address user) external view returns (uint256)
```

### Sweeping Timelocked Funds

When a user's withdrawal timelocks have ended, the funds may be swept to their wallets:

```javascript
function sweepTimelockBalances(
    address[] memory users
) external returns (uint256 totalWithdrawal);
```

The function accepts an array of addresses and will attempt to sweep the time-locked funds for each one.  The funds will be transferred back to the users wallets.

### Depositing Using Timelocked Funds

If a user wishes to re-deposit their timelocked funds, they can do so using this function:

```javascript
function timelockDepositTo(
    address to,
    uint256 amount,
    address controlledToken
)
    external;
```

| Parameter | Description |
| :--- | :--- |
| to | The address to whom the controlled tokens should be minted |
| amount | The amount of the underlying asset the user wishes to deposit.  The Prize Pool contract should have been pre-approved by the caller to transfer the underlying ERC20 tokens. |
| controlledToken | The address of the token that they wish to mint.  For our default Prize Strategy this will either be the Ticket address or the Sponsorship address.  Those addresses can be looked up on the Prize Strategy. |

### Withdraw Instantly

If a user would like their tickets right away, they may pay an early exit fee to the prize.  The early exit fee is determined by the [Prize Strategy](../prize-strategy/).

To withdraw instantly:

```javascript
function withdrawInstantlyFrom(
    address from,
    uint256 amount,
    address controlledToken,
    uint256 maximumExitFee
  )
    external
    returns (uint256 exitFee);
```

| Parameter Name | Parameter Description |
| :--- | :--- |
| from | The address to withdraw from.  This means you can withdraw on another user's behalf if you have an allowance for the controlled token. |
| amount | The amount to withdraw |
| controlledToken | The controlled token to withdraw from |
| maximumExitFee | The maximum early exit fee the caller is willing to pay.  This prevents the Prize Strategy from changing the fee on-the-fly. |

## Awarding

Only the Prize Strategy can call the award functions.  These functions allow prizes to be disbursed to users.

### Awarding Yield

Yield that accrues in the Prize Pool can be awarded by the Prize Strategy using the `award` function.  The yield must be awarded as one of the controlled tokens configured in the Prize Pool.

```javascript
function award(
    address to,
    uint256 amount,
    address controlledToken
) external;
```

| Parameter Name | Parameter Description |
| :--- | :--- |
| to | The address to receive the newly minted tokens |
| amount | The amount of tokens to mint |
| controlledToken | The type of token to mint |


