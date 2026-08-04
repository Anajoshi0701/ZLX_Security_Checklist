# Bridges · Cross-Chain Message Validation

## 1. Identity & Origin Verification

### Source Verification
- Verify both **source chain** and **sender**.
- Never trust addresses without `chainId` validation.
- Maintain trusted sender mappings per chain.

---

## 2. Validation Consistency

### Unified Validation
- All proof verification paths should follow identical logic.
- Check edge cases:
  - Single-node proofs
  - Empty proofs
  - Boundary conditions

### Validation Invariants
- Ensure all validators agree on what constitutes a valid message.

---

## 3. Batch Execution

### Partial Failure Handling
- Verify one failed message cannot unnecessarily block the entire batch.
- Handle failures gracefully where appropriate.

### Front-Running
- Check batches containing:
  - Permits
  - Nonces
  - One-time signatures

- Ensure front-running one operation cannot brick the batch.

---

## 4. Economic Security

### Subsidy Abuse
- Verify users cannot claim execution incentives through self-interaction or Sybil identities.

### Resource Limits
- Apply limits to:
  - Fee reimbursements
  - Sponsor vaults
  - Incentive pools

---

# Quick Audit Test

- Is both sender **and** source chain verified?
- Can validation logic disagree across implementations?
- Can one bad message DoS an entire batch?
- Can permits/nonces be front-run?
- Can incentives be Sybil exploited?
- Are message processing events emitted?
- Is emergency pause available for bridge failures?