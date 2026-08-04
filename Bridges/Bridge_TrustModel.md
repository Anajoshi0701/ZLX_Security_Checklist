# Bridges · Trust Model & Validator Security

## 1. Role Management

### Ownership Transfer
- Use two-step ownership/role transfers.
- Prevent accidental lockout from incorrect addresses.

---

## 2. Parameter Validation

### Critical Parameters
- Validate bridge fees, execution fees, and protocol parameters.
- Reject zero, invalid, or unset values.

---

## 3. Validation Logic

### Consistency
- Ensure all proof/verification paths use the same logic.
- Verify edge cases behave identically.

---

## 4. Validator Security

### Trust Assumptions
- Review validator threshold and signer permissions.
- Check validator rotation and access control.

### Decentralization
- Avoid signer centralization or single points of failure.

---

# Quick Audit Test

- Are role transfers two-step?
- Can ownership be lost by mistake?
- Are fees and critical parameters validated?
- Do all validators follow the same verification logic?
- Are validator thresholds secure?
- Is the signer set sufficiently decentralized?