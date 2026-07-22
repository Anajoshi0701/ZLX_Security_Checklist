# Lending · The thin line between healthy and liquidated

## Category 1: Oracle Reliability & Availability

This section focuses on ensuring oracle failures or stale data cannot compromise protocol safety or solvency.

- **Avoid Safety-Critical Reverts on Staleness:** Ensure that staleness checks (e.g., checking if `block.timestamp - updatedAt > MAX_AGE`) do not prevent critical functions like liquidations.  
  **The Risk:** If a price is stale, a revert can lock the system during market volatility, allowing borrowers to "self-liquidate" for profit once the feed recovers.

- **Implement Fallback Oracles:** For critical safety checks, provide a secondary/fallback oracle to query if the primary feed is stale or failing, ensuring the protocol remains solvent even if one data source goes offline.  
  **The Risk:** Relying on a single oracle can leave the protocol unable to determine solvency during oracle outages.

- **Beware of Pull-Based Oracle Latency:** Be aware that "pull-based" oracles (like Pyth or Redstone) allow users to choose prices within a specific time window. Ensure the protocol can handle an attacker selecting the most "profitable" price from that window to create bad debt.  
  **The Risk:** Attackers may intentionally submit the most favorable valid price within the oracle's latency window to manipulate borrowing power or liquidations.

## Category 2: Liquidation & LTV Mechanics

This section focuses on ensuring borrowers have sufficient safety margins and liquidation logic behaves as intended.

- **Enforce a Safety Gap (Borrow vs. Liquidation LTV):** Never allow users to borrow assets at an LTV ratio that is identical or too close to the liquidation threshold.  
  **The Risk:** Without a buffer, minor price fluctuations or oracle updates can trigger immediate liquidations.

- **Configure Asset-Specific Buffers:** Ensure the gap between borrow and liquidation LTV is flexible—volatile assets require a wider gap than stable ones to protect users from instant liquidation.  
  **The Risk:** Using identical buffers across all assets ignores differences in volatility and liquidation risk.

- **Prevent Same-Block Exploits:** Implement a "cool-off period" where an account cannot be liquidated in the same block it was deemed healthy.  
  **The Risk:** This prevents attackers from setting up and liquidating a position in a single, risk-free transaction.

## Category 3: TWAP & Price Manipulation

This section focuses on the integrity of TWAP oracles and resistance against price manipulation.

- **Use Price-Weighted Averages, Not Balance-Weighted:** Ensure your TWAP oracle averages the actual spot price rather than using pool reserves/balances as a proxy.  
  **The Risk:** Balance-based TWAPs are highly susceptible to manipulation by large capital injections in a single block.

- **Verify TWAP Windows:** Ensure the time window for a TWAP is long enough to make manipulation prohibitively expensive, but not so long that the price becomes too lagged for the protocol's needs.  
  **The Risk:** Poorly chosen TWAP windows either become easy to manipulate or too slow to reflect real market conditions.

- **Capital-Intensity Requirements:** Check if a malicious actor with significant capital can skew the "cumulative" values of an oracle to cause a Denial of Service (DOS) on minting or redeeming functions.  
  **The Risk:** Even if funds cannot be stolen directly, oracle manipulation may disable core protocol functionality.

## Category 4: Anti-Arbitrage & Timing Attacks

This section focuses on attacks that exploit oracle update timing or liquidation incentives.

- **Prevent Oracle Sandwiching:** Examine if an attacker can time transactions around an on-chain oracle update to bypass "siphoning" or anti-arbitrage mechanisms.  
  **The Risk:** Oracle update timing can be exploited to extract value before the protocol adjusts to new prices.

- **Implement Minimum Lock-up Periods:** To stop "sandwich" attacks where users enter and exit a vault around a single oracle update, implement a minimum deposit duration (e.g., 24 hours).  
  **The Risk:** Without a holding period, users can repeatedly exploit predictable oracle updates.

- **Analyze Health Score Erosion:** Ensure that liquidation bonuses do not leave the borrower more unhealthy than they were before the liquidation began.  
  **The Risk:** If the bonus/discount is too high, it may incentivize "dust" liquidations that intentionally drain the vault.