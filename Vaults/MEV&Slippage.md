# Vault MEV & Slippage Checklist

## Slippage
- [ ] Does every swap enforce a meaningful `minOut` / `maxIn`?
- [ ] Can slippage bounds be manipulated or bypassed?
- [ ] Are internal swaps (harvest, rebalance, compound, etc.) also protected?

## Transaction Ordering (MEV)
- [ ] Can deposits, withdrawals, harvests, or rebalances be front-run, back-run, or sandwiched?
- [ ] Can transaction ordering extract value from existing vault users?
- [ ] Can temporary liquidity (e.g., flash loans) amplify the attack?

## Share Price & Accounting
- [ ] Can users profit by entering before or exiting after a predictable accounting/share-price update?
- [ ] Are share-price calculations resistant to temporary price manipulation?

## External Dependencies
- [ ] Can oracle updates or external protocol interactions be manipulated to benefit an attacker?
- [ ] Does the protocol rely on assumptions about the mempool, sequencer, or transaction ordering for security?

> **Invariant:** No user should be able to extract value solely by manipulating transaction ordering or temporary price movements.