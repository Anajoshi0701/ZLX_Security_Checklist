# Governance · Policy Controls

## 1. Timelocks & Change Delays

### Parameter Changes
- Check whether critical governance changes take effect immediately.
- Ensure sensitive changes have a sufficient timelock.

### Exit Window
- Give users enough time to react, withdraw, or close positions before changes take effect.

### Emergency Paths
- Check whether emergency functions can bypass governance/timelocks.
- Verify emergency powers are narrowly scoped.

---

## 2. Parameter Validation

### Bounds
- Enforce minimum and maximum values when parameters are changed.
- Check zero and extreme values.

### Parameter Dependencies
- Ensure related parameters cannot be set to inconsistent values.
- Validate relationships when **either** parameter changes.

### Timing Parameters
- Check delays, cooldowns, and durations cannot be set to unsafe values.

---

## 3. Guard Execution

### Pre-Execution Checks
- Ensure governance restrictions are enforced **before** the function executes.
- Check custom modifiers for incorrect placement of `_;`.

### Fail Closed
- Invalid parameters should revert before state changes occur.
- Don't rely on checks that only detect problems after execution.

---

## 4. Voting Security

### Voting Math
- Check whether averaging or weighting can be manipulated through extreme votes.
- Ensure voting math does not reward dishonest/exaggerated preferences.

### Flash Governance
- Prevent flash loans/flash mints from temporarily increasing voting power.
- Prefer historical snapshots, checkpoints, or vote-locking.

### Quorum
- Check whether `totalSupply` or other mutable values can be manipulated to bypass quorum requirements.

---

## 5. Admin & Role Security

### Address Updates
- Validate critical addresses against zero addresses and expected interfaces.

### Privilege Expansion
- Check whether admins can freely add new admins or privileged addresses.
- Verify critical role changes follow the intended governance process.

---

# Quick Audit Test

- Can governance changes take effect immediately?
- Is the timelock long enough for users to react?
- Can emergency paths bypass it?
- Are parameters bounded and mutually consistent?
- Do guards execute **before** the protected action?
- Can voting power be flash-loaned?
- Can voting math be strategically manipulated?
- Can quorum be manipulated?
- Can admins expand their own privileges?