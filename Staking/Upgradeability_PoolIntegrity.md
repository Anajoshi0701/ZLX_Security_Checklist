# Staking · Upgradeability & Pool Integrity

## 1. Upgradeability

### Initializers
- Disable implementation initializers (`_disableInitializers()`).
- Prevent implementation takeover.

### Storage Layout
- Reserve storage gaps (`__gap`).
- Prevent storage collisions across upgrades.

### Function Selectors
- Check proxy ↔ implementation selector clashes.
- Ensure implementation functions remain reachable.

---

## 2. Reward & Pool Security

### Reward Configuration
- Restrict reward pool configuration to privileged roles.
- Prevent malicious reward redirection.

### Withdrawal Fairness
- Ensure pool deficits are shared proportionally.
- Prevent first-withdrawer advantage.

### Reserve Validation
- Validate active reserves before creating pending/virtual orders.
- Prevent unbacked orders and broken pool accounting.

### Monotonic Values
- Verify cumulative values (e.g. `pricePerShare`, liquidity index) never decrease unexpectedly.
- Reject abnormal updates.

---

# Quick Audit Test

- Are implementation contracts initialized safely?
- Are storage gaps reserved?
- Any proxy/function selector collisions?
- Who can modify reward pools?
- Are pool losses distributed fairly?
- Can virtual orders be created without liquidity?
- Can cumulative accounting values decrease?