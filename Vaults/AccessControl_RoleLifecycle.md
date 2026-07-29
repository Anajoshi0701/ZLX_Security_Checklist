# Vaults · Access Control & Role Lifecycle

## 1. Role Hierarchy

### Management Hierarchy
- Critical admin actions should require a higher authority (Admin, Timelock, Multisig).
- Avoid granting owner-level accounts unrestricted administrative powers.

### Peer Authority
- Prevent peers from removing or modifying each other's privileges.

---

## 2. Separation of Duties

### Least Privilege
- Separate operational roles from administrative roles.
- Traders/strategies should not manage permissions.

### Revocation Safety
- Ensure compromised roles cannot block or revoke their own removal.

---

## 3. Administrative Power

### Arbitrary Calls
- Restrict admin functions capable of arbitrary external calls.
- Limit callable targets/selectors where possible.

### Approval Abuse
- Verify privileged contracts cannot misuse user token approvals.

---

## 4. Permission Bookkeeping

### Role Removal
- Ensure role removal updates the correct user's role list.

### Swap-and-Pop
- Verify array compaction cannot accidentally grant new permissions.

---

## 5. Role Lifecycle

### Initialization
- Check constructor/initializer for unintended role overlap.

### State Transitions
Review:
- Grant
- Revoke
- Renounce
- Transfer

Ask:

> What happens if the role holder becomes malicious at this step?

---

# Quick Audit Test

- Can peers remove each other?
- Can compromised roles resist revocation?
- Does any role have unnecessary privileges?
- Can admins execute arbitrary external calls?
- Can permission removal accidentally grant another role?
- Are role transitions always consistent?
- Are critical functions protected by a multisig/timelock?