# Lending · Liquidation Math & Accounting Integrity

## 1. Input Handling & Special Value Logic

### Resolution of Special Constants
- Check if special values (e.g. `type(uint256).max`) are used as shortcuts for "entire balance" or "maximum repay."
- Ensure these values are resolved to the user's **current on-chain balance or debt** at execution time.
- Verify that no arithmetic is performed directly on the sentinel value before resolution.
- Test boundary inputs:
  - `0`
  - `1`
  - exact debt/collateral amount
  - `type(uint256).max`

### Input Array Uniqueness
- Verify whether batch functions allow duplicate Pool IDs, Token IDs, or Asset Addresses.
- Ensure duplicate entries cannot:
  - double-count collateral
  - double-count debt repayment
  - inflate liquidation rewards
  - seize collateral multiple times

### Internal State Consistency
- Ensure all validation logic (Health Factor, LTV, solvency checks) uses the exact same accounting and state as execution.
- Confirm no mismatch exists between preview calculations and actual liquidation execution.

---

## 2. State Consistency (User vs Global Accounting)

### Mirrored State Updates
Whenever liquidation changes a user's position, verify corresponding global state updates.

Examples:
- User Debt ↔ Total Debt
- User Borrow Shares ↔ Total Borrow Shares
- User Collateral ↔ Total Collateral
- User Supply Shares ↔ Total Supply Shares

### Accounting Drift
Audit every liquidation path for missing global updates.

Watch for:
- Full liquidation
- Partial liquidation
- Bad debt write-off
- Emergency liquidation
- Forced position closure

Missing aggregate updates can slowly corrupt protocol accounting.

### Symmetry of Operations
Every decrease in user state should produce an equal decrease in protocol state.

Examples:
- `userBorrow -= X`
- `totalBorrow -= X`

Likewise for:
- shares
- collateral
- reserves
- accrued fees

Ensure both values use the same unit (Assets vs Shares).

---

## 3. Profit & Fee Capture Integrity

### Trustless Profit Accounting
Never trust an external actor (liquidator, swapper, executor) to report protocol profit.

Instead:
- compute profits internally
- verify balances before/after swaps
- derive fee amounts on-chain

### Slippage & Minimum Return Checks
If liquidation performs external swaps:

Verify:
- minimum output validation
- slippage protection
- revert on insufficient return

Protocol fees should never depend on swap honesty.

### Verification of Returned Assets
If fees depend on:

```
borrowed - returned
```

ensure:
- returned amount is measured by contract balances
- not supplied as an arbitrary user parameter

---

## 4. Economic Logic & Liquidation Incentives

### Rational Liquidation Incentives
Verify liquidation bonus covers realistic execution costs.

Consider:
- gas costs
- swap fees
- flash loan fees
- bridge fees (if applicable)
- MEV competition

A bonus that's too small discourages liquidations.

### Solvency of Liquidators
Ensure liquidation does not force the liquidator into an unhealthy position.

Verify they can:
- repay debt
- receive collateral
- safely exit
- remain profitable

Otherwise, liquidations may stop during market stress.

### Repayment Asset Liquidity
Check whether liquidation requires repayment using an illiquid asset.

Questions:
- Can the repayment asset realistically be sourced?
- Does low liquidity create liquidation deadlocks?

---

## 5. Conversion & Precision Safety

### Asset ↔ Share Conversions
Trace every conversion individually.

Examples:

```
Assets
 ↓
Borrow Shares
 ↓
Repayment Shares
 ↓
Assets
```

Ensure no precision loss accumulates unexpectedly.

### Rounding Direction
Verify rounding always favors protocol solvency.

Typical expectations:
- Debt → round up
- Collateral → round down

Review handling of:
- dust positions
- final repayments
- full liquidations

### Oracle Freshness
Verify liquidation calculations use the latest available oracle price.

Especially before:
- collateral seizure
- debt valuation
- liquidation bonus computation

Prevent stale prices from creating unfair liquidations.

---

# Edge Case Testing

Always test liquidation with:

- Zero amounts
- `type(uint256).max`
- Exact health factor boundary
- Exact liquidation threshold
- Very small ("dust") positions
- Maximum position size
- Multiple liquidations in one transaction
- Duplicate assets/IDs in batch operations
- Partial liquidation
- Full liquidation
- Oracle price changes during execution

---

# Invariant Checklist

After every liquidation, verify:

- Sum of user debts == Total Debt
- Sum of borrow shares == Total Borrow Shares
- Sum of collateral == Total Collateral
- Sum of supply shares == Total Supply Shares
- Protocol fees are fully accrued
- No collateral is created or destroyed unexpectedly
- No debt disappears without accounting
- Health Factor improves (or position is fully closed)
- Liquidator reward matches protocol rules
- Protocol remains solvent

---

# Auditor Questions

- How is the liquidation amount calculated?
- Who computes the seize amount?
- Who verifies it?
- Can any user-controlled input influence protocol fee calculations?
- Are all global aggregates updated?
- Are asset/share conversions symmetric?
- Does rounding always favor protocol solvency?
- Can liquidation become economically irrational?
- Are stale oracle prices exploitable?
- Does every liquidation path preserve accounting invariants?