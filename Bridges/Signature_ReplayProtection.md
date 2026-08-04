# Bridges · Signature & Replay Protection

## 1. Replay Protection

### Nonces
- Every signed message should include a unique nonce/message ID.
- Mark nonces as used immediately after execution.

---

## 2. Signature Domain

### Context Binding
- Include `block.chainid`.
- Include `address(this)`.
- Prefer EIP-712 (`DOMAIN_SEPARATOR`).

---

## 3. Signature Lifetime

### Expiration
- Include a deadline/expiry timestamp.
- Include session/round IDs when applicable.

---

## 4. Execution Tracking

### Replay Storage
- Record executed messages in the contract performing the action.
- Ensure replay protection survives upgrades or contract replacements.

---

## 5. Payload Integrity

### Complete Commitment
- Sign every critical parameter:
  - Amount
  - Recipient
  - Price
  - Strategy
  - Token
  - Any execution-critical data

---

# Quick Audit Test

- Is every signature unique (nonce/message ID)?
- Are used messages/nonces recorded?
- Is the signature bound to the correct chain and contract?
- Does the signature expire?
- Does replay protection survive upgrades?
- Does the signature commit to all critical parameters?