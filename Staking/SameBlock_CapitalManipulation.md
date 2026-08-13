# Staking · Same-Block & Capital Manipulation

## 1. Instantaneous Balance Checks

### Balance-Based Privileges
- Find uses of `balanceOf()` / `totalSupply()` for:
  - Voting power
  - Rewards
  - Shares
  - Privileges
- Ask: **Can a flash loan temporarily inflate the balance?**

### Reward Accounting
- Don't calculate rewards directly from transferable token balances.
- Prefer internal staking records or non-transferable accounting.

### Governance Weight
- Avoid using current balances for voting power.
- Prefer historical/checkpointed balances.

---

## 2. Oracle & Price Checks

### Atomic Oracle Updates
- Check whether users can trade at a stale oracle price and trigger an update/rebalance in the same transaction.

### Intra-Block Pricing
- Ensure oracle/rate updates cannot create different prices within the same block.
- Ask: **Can an attacker sandwich an oracle update for profit?**

---

## 3. Same-Block State Transitions

### Deposit → Privilege
Trace:

```text
Deposit → Gain privilege/reward → Withdraw