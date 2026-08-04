# Bridges · The key that kept working after the job was done

> Temporary authority must not survive beyond its intended operation, scope, or lifetime.

- [ ] Trace every privileged key, permit, approval, role, delegate, and session permission from **grant → use → expiry/revocation**.

- [ ] Verify one-time or temporary permissions are explicitly revoked or made unusable immediately after the intended operation.

- [ ] Check whether permissions can remain active after successful execution, partial execution, caught failures, callbacks, or alternative execution paths.

- [ ] Verify delegated authority is narrowly scoped by **action, account, contract, amount, chain, and lifetime** where applicable.

- [ ] Check that privileged keys and roles have a functional revocation mechanism and cannot retain authority after removal or replacement.

- [ ] Verify key rotation procedures actually invalidate old keys across every contract, signer set, relayer, validator, and off-chain component.

- [ ] Check whether compromised or deprecated keys can still satisfy signature thresholds or participate in privileged operations after rotation.

- [ ] Verify multisig / threshold authority matches the documented security model and cannot effectively collapse to a smaller set of operators.

- [ ] Check for approvals or permits granted to routers, adapters, managers, or helper contracts that remain usable across subsequent transactions.

- [ ] Verify privileged grants, revocations, rotations, and sensitive key usage emit sufficient events for off-chain detection and monitoring.

- [ ] Check that emergency controls can rapidly disable compromised keys, signers, roles, or permissioned execution paths.

- [ ] Compare documented permission/key-management invariants against the implementation and attempt to falsify each one.