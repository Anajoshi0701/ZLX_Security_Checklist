# Lending · The attack that steals nothing, it just jams the machine

## Category 1: Input Validation for Multi-Step/Batch Operations

This section focuses on validating user-controlled inputs used in
multi-step interactions with external protocols such as DEXs, bridges,
and yield aggregators.

-   **Path/Route Integrity:** If a function accepts a "route" or "path"
    for a trade, does it explicitly verify that the starting token is
    the specific asset the protocol intended to sell and that the ending
    token is the intended target?\
    **The Risk:** Incorrect route validation can allow attackers to
    redirect swaps or trade unintended assets.

-   **Collateral & Reward Segregation:** Can internal protocol assets
    (such as earned rewards or underlying collateral) be "swept" into a
    trade path because they share an approval or address with the
    primary asset being traded?\
    **The Risk:** Internal assets may be unintentionally or maliciously
    traded, disrupting protocol accounting or asset safety.

-   **Restrictive Defaults:** Evaluate if "batch" or "arbitrary data"
    features are strictly necessary. Where security risk outweighs
    utility, consider restricting complex trades to hardcoded or
    admin-approved routes.\
    **The Risk:** Excessively flexible execution paths increase the
    attack surface and enable malicious transaction construction.

## Category 2: Functional Parity Across Lifecycle Entry Points

This section focuses on ensuring security restrictions remain consistent
across every function capable of changing an entity's lifecycle.

-   **Consistency Check:** If a security gate (e.g., a blacklist,
    whitelist, or pause mechanism) is implemented in a `create()` or
    `initialize()` function, is that same gate enforced in `migrate()`,
    `upgrade()`, `transfer()`, or `reconfigure()`?\
    **The Risk:** Attackers may bypass restrictions through alternative
    lifecycle paths.

-   **State Bypassing:** Can a user circumvent an administrative
    restriction by moving an existing account or asset into a deprecated
    or vulnerable logic version through an upgrade path?\
    **The Risk:** Vulnerable implementations remain reachable despite
    administrative protections.

-   **Logic Inheritance:** Verify that migration functions do not skip
    the validation steps required for a fresh deployment, as the end
    result carries the same security assumptions.\
    **The Risk:** Missing validation during migrations creates
    inconsistent security guarantees.

## Category 3: Caller Verification in Receiver Hooks

This section focuses on validating callback-based entry points used when
receiving assets.

-   **Authentication of the Caller:** Does the hook (e.g.,
    `onERC721Received`) trust parameters like `from` or `operator` for
    authorization, or does it verify that `msg.sender` is the legitimate
    token contract?\
    **The Risk:** Attackers can spoof callback parameters to trigger
    unauthorized protocol behavior.

-   **State Poisoning Prevention:** Can an attacker call a hook directly
    to update internal records (such as "pair already exists" mappings)
    using fake data?\
    **The Risk:** Fake state updates may permanently block legitimate
    operations or create denial-of-service conditions.

-   **Pull vs. Push Patterns:** Wherever possible, replace push
    mechanisms (relying on callbacks) with pull mechanisms (admin or
    protocol initiated transfers) to maintain protocol control over
    execution.\
    **The Risk:** Callback-driven execution exposes critical logic to
    attacker-controlled execution flow.

## Category 4: Temporal Security & Transaction Windows

This section focuses on timing assumptions that protect protocol
operations from manipulation.

-   **Adequate Cooldowns:** Are the minimum intervals between updates
    (such as oracle refreshes or epoch transitions) significantly longer
    than standard block times to reduce front-running opportunities?\
    **The Risk:** Extremely short update windows make transaction
    ordering attacks more practical.

-   **Manipulatable Gaps:** If an update interval is too short, can an
    attacker observe a pending update and inject their own transaction
    into the gap before others react?\
    **The Risk:** Attackers can profit from temporary inconsistencies in
    protocol state.

-   **Safety Overrides:** If timing logic is implemented in a virtual
    function, ensure inheriting contracts cannot override safety
    constants with dangerously low values.\
    **The Risk:** Unsafe overrides can silently weaken protocol security
    guarantees.
