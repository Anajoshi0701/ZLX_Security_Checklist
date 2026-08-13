# Staking · Emergency Controls & Access Control

## 1. Emergency Pause

### Pause Coverage
- Ensure all fund-moving functions enforce `whenNotPaused`.
- Check deposits, staking, withdrawals, claims, and other state-changing paths.

### Bypass Paths
- Verify pause cannot be bypassed through indirect calls or alternate entry points.
- Ensure emergency flows block the intended actions during a crisis.

---

## 2. Role Mapping

### Correct Roles
- Verify each function uses its intended operational role.
- Ensure emergency actions are not accidentally restricted to `DEFAULT_ADMIN_ROLE`.

### Operational Access
- Confirm the assigned role can actually execute emergency actions quickly.
- Check that governance/timelock requirements do not prevent emergency response.

---

## 3. Registry Integrity

### Access Control
- Restrict all functions that add, remove, or modify registries.
- Check helper/configuration functions for missing access control.

### Duplicate Entries
- Prevent unauthorized or duplicate registry entries.
- Check whether duplicates can corrupt reward loops, balances, or accounting.

---

## 4. Array Manipulation

### Swap-and-Pop
- Ensure replacement elements come from the **same array** being modified.
- Verify deletion cannot accidentally grant another role, asset, or permission.

---

# Quick Audit Test

- Does every fund-moving path respect pause?
- Can pause be bypassed indirectly?
- Are emergency roles correctly assigned?
- Can unauthorized users modify registries?
- Can duplicate entries corrupt accounting?
- Does swap-and-pop stay within the correct array?