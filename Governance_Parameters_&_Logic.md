# Governance Parameters & Logic

## Category 1: Governance Snapshot & Voting Integrity

This section focuses on the synchronization between voting power and
governance requirements (like Quorum).

-   **Synchronization of Requirements and Power:** Verify if the
    variables used for quorum (e.g., total supply) and voting power
    (e.g., user balance) are snapshotted at the exact same point in
    time.\
    **The Risk:** If quorum is based on "Pre-Action" supply but voting
    is based on "Post-Action" supply, a user can manipulate the outcome
    in a single transaction.

-   **Checkpoint Utilization:** Check if the protocol uses historical
    checkpoints for all governance-sensitive values rather than
    instantaneous current values.\
    **The Risk:** Instantaneous values are vulnerable to
    "flash-manipulation" within a single block.

-   **Impact of Massive Minting/Claims:** Identify if there are
    mechanisms (like Merkle claims or large-scale migrations) that allow
    for a sudden, massive increase in circulating supply.\
    **The Risk:** Large-scale distributions can "hijack" a DAO if the
    governance parameters weren't designed for high-velocity supply
    changes.

## Category 2: Timing Parameters & Logic Flow

This section covers the validation of durations and the relationships
between different system stages.

-   **Global Parameter Sanity Checks:** Ensure all timing-related
    variables (warm-up, duration, cooling-off) have require statements
    to prevent them from being set to 0 or excessively high values.\
    **The Risk:** Setting durations to 0 can cause logical deadlocks
    where a proposal's start and end times are identical, making it
    impossible to interact with.

-   **Inter-parameter Relationship Validation:** Verify that timing
    parameters maintain a logical sequence (e.g., StartBlock \< EndBlock
    or VotingPeriod \> MinimumDelay).\
    **The Risk:** If parameters are updated independently without
    checking their relationship to one another, the governance timeline
    can be broken.

-   **State-Locked Safety Functions:** Check if critical safety
    functions (like cancel) are restricted to specific states that could
    be "skipped" if delays are minimal.\
    **The Risk:** If a function is only available in a "Pending" state,
    but the "Pending" state lasts 0 or 1 blocks, the function becomes
    effectively unreachable.

## Category 3: Edge Case & Environment Handling

This section focuses on how the code interacts with the blockchain
environment and unexpected configurations.

-   **Zero-Value Fallback Logic:** Review helper functions that convert
    time to blocks or units. Ensure that "safe" fallbacks (like rounding
    0 up to 1) do not break the intended user experience.\
    **The Risk:** A safety fallback designed to prevent a 0-value error
    might unintentionally move a contract into a new state faster than a
    user can react.

-   **Dynamic vs. Static Context:** Check if environmental identifiers
    (like chainId or address(this)) are hardcoded in the constructor or
    re-calculated dynamically.\
    **The Risk:** Hardcoding a chainId makes signatures valid only for
    that specific ID; if the chain forks, the hardcoded value remains
    unchanged, allowing for cross-chain replay attacks.

-   **Default Configuration Security:** Audit the deployment scripts and
    default settings. Does the "out of the box" configuration (like
    votingDelay = 0) disable any security features?\
    **The Risk:** Developers often focus on the contract code but
    overlook how the deployment parameters change the contract's
    behavior in production.

## Category 4: Mitigation & Recovery

-   **Emergency Pause/Veto Mechanisms:** If governance logic fails, is
    there a secondary layer (like a Vetoer or Emergency Multisig) to
    stop malicious execution?\
    **The Risk:** Purely decentralized systems with no "emergency stop"
    are at total risk if the underlying governance logic contains a
    single flaw.

-   **Governance Delays for System Changes:** Is there a mandatory delay
    between a proposal passing and being executed?\
    **The Risk:** Instant execution prevents the community or admins
    from reacting to exploits.
