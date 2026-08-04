# Lending · Oracle Price Integrity

## 1. Oracle Validation

### Price Freshness
- Check `updatedAt` / `answeredInRound`.
- Enforce a `MAX_STALENESS` threshold.

### Correct Feed
- Verify the oracle matches the exact asset.
- Handle wrapped/bridged asset depegs.

### Decimals
- Scale oracle prices correctly to protocol decimals.

---

## 2. Price Safety

### Multiple Sources
- Compare primary oracle with TWAP/secondary sources when appropriate.
- Revert or pause on excessive price deviation.

### Oracle Failure
- Handle stale, invalid, or failed oracle responses safely.

---

## 3. First Deposit

### Empty Pool
- Audit the first deposit/share calculation.
- Prevent first-depositor manipulation.

### Initialization
- Prefer atomic deployment + first deposit.
- Mint dead shares to stabilize the initial exchange rate.

---

## 4. Math & Precision

### Rounding
- Require non-zero shares on mint/burn.
- Check for rounding exploits.

### Internal Accounting
- Prevent liquidity index or share-price manipulation through direct asset transfers.

---

# Quick Audit Test

- Is the oracle fresh?
- Is the correct asset feed used?
- Are oracle decimals handled correctly?
- Can wrapped assets depeg?
- Is the first deposit safe?
- Are dead shares minted?
- Can rounding inflate shares?
- Can internal accounting be manipulated?
- Does the protocol fail safely when the oracle breaks?