# Vaults · The safety net with holes in the math

## 1. Thresholds & Safety Buffers

### Borrow LTV vs Liquidation Threshold
- Ensure a sufficient gap exists.
- Prevent instant liquidation from minor price movement.

### Atomicity & Cool-Offs
- Check same-block borrow + liquidation.
- Consider cooldowns after opening positions.

### Volatility Scaling
- Verify thresholds are configurable per asset.
- More volatile assets → larger safety margins.

---

## 2. Liquidation Incentives

### Bonus Calculation
- Bonus should be based on debt repaid.
- Avoid equity/surplus-based calculations.

### Reward Integrity
- Don't assume fixed asset ratios.
- Prevent reserve imbalance manipulation.

### Economic Viability
- Verify smallest liquidation remains profitable after gas and fees.

---

## 3. Bad Debt Handling

### Dust Protection
- Don't trigger bad debt only at zero collateral.
- Use minimum collateral thresholds.

### Insolvency Accounting
- Verify uncovered debt is fully accounted for.
- No phantom debt or balances.

### Bonus Limits
- Ensure liquidation bonus cannot create bad debt.

---

## 4. Position Consistency

### Uniform Health Checks
Apply identical health/LTV checks to:
- Borrow
- Withdraw
- Transfer
- Liquidate

### Oracle Integrity
- Prevent stale or user-selected oracle prices.
- Use recent prices for borrowing/liquidation.

---

# Quick Edge Cases

- Max Borrow LTV
- Liquidation Threshold boundary
- 1% price movement
- Same-block borrow + liquidation
- Dust collateral
- Smallest liquidation
- Maximum liquidation bonus

---

# Auditor Questions

- Is the LTV gap sufficient?
- Can users be liquidated immediately?
- What is the bonus calculated from?
- Can rewards be manipulated?
- Can dust hide bad debt?
- Who absorbs bad debt?
- Do all state-changing functions run the same health checks?
- Are oracle prices always fresh?