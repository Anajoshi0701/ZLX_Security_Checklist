# Lending · Oracle Integrity & State Transitions

## 1. Oracle Bounds & Outliers

### Price Bounds

* Check whether the oracle can return `minAnswer` / `maxAnswer` instead of the real market price.
* Reject or safely handle boundary values.

### Outlier Detection

* Don't blindly trust a single feed.
* Compare against a secondary source/TWAP where appropriate.

---

## 2. Oracle Fallback & Freshness

### Fallbacks

* Verify the fallback actually executes when the primary oracle reverts.
* Check `try/catch` or equivalent error handling.

### Freshness

* Validate `price > 0`, timestamps, round data, and staleness/heartbeat limits.
* Avoid deprecated oracle APIs that omit important metadata.

---

## 3. Dynamic State Transitions

### Live Balance Manipulation

* Check whether rate calculations use live `balanceOf()` values instead of trusted internal state.
* Ask: **Can an attacker donate tokens to manipulate the calculation?**

### `timeDelta`

* Test both very small and very large `timeDelta` values.
* Check whether a long inactivity period can cause a rate to jump instantly.

### Rate Limits

* Ensure gradual transitions cannot be bypassed through flash loans, direct transfers, or repeated calls.
* Verify the intended rate-change limit still applies after extreme state changes.

---

# Quick Audit Test

* Can the oracle return a boundary or absurd price?
* Is stale data rejected?
* Does the fallback actually work when the primary reverts?
* Are all critical oracle metadata checks performed?
* Can direct token transfers manipulate a calculated rate?
* What happens when `timeDelta` is extremely large?
* Can a supposedly gradual state transition jump instantly?


