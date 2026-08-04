# Bridges · Finality Before Release

* [ ] Verify source-chain deposits/messages reach sufficient finality **before** funds are minted or released on the destination chain.
* [ ] Ensure finality/confirmation requirements are configured **per chain** rather than using one universal confirmation count.
* [ ] Check that confirmation thresholds have safe bounds and cannot be set to unsafe values (e.g. `0` or excessively low).
* [ ] Verify the protocol handles **source-chain reorgs** and does not permanently honor deposits from blocks that are no longer canonical.
* [ ] Check whether pending/observed messages are revalidated against the canonical chain before destination execution.
* [ ] Ensure every cross-chain deposit/message has a **unique identifier** and can be processed only once.
* [ ] Test whether multiple relayers or repeated submissions can race to process the same deposit before it is marked handled.
* [ ] Ensure processed state is updated safely before external transfers/mints to prevent duplicate execution.
* [ ] Verify sufficient events are emitted so off-chain infrastructure can detect reorgs, failed finality assumptions, and abnormal message processing.
* [ ] Confirm destination-chain releases can be **paused quickly** if the source chain experiences abnormal reorgs or its finality guarantees become unreliable.
* [ ] Identify dependencies on external consensus, relayers, upgradeable components, or incomplete functionality whose security assumptions are not fully enforced or reviewable on-chain.

> **Key invariant:** A destination-chain release must never become irreversible before the corresponding source-chain deposit is sufficiently final, and one source deposit must never produce more than one destination release.

