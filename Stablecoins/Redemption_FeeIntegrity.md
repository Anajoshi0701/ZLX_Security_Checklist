# Stablecoins · Redemption & Fee Integrity

## 1. Redemption Value

### Price Manipulation
- Check whether redemption/minting value depends on manipulable spot reserves.
- Ask: **Can an attacker manipulate the price and redeem for more than they should?**

### Exchange Rate
- Verify redemption rates use manipulation-resistant pricing where required.
- Check whether reserves can be temporarily skewed within one transaction.

---

## 2. Transfer Accounting

### Actual Amount Received
- Don't assume `amount` sent == amount received.
- Check balance before/after transfers when fee-on-transfer tokens are supported.

### Accounting Consistency
- Ensure fees, balances, and reserves use the **actual transferred amount**.
- Check downstream logic for assumptions about the expected balance.

---

## 3. Fee-on-Transfer Tokens

### Double Fees
- Trace transfers through intermediate contracts.
- Ensure the same token isn't unintentionally charged twice.

### Fee Bypass
- Check whether users can mint/redeem while the protocol accounts for the pre-fee amount.

### Token Behavior
- Verify supported tokens cannot unexpectedly enable transfer fees or other transfer restrictions.
- Consider upgradeable/togglable token configurations.

---

## 4. Fee Configuration

### Fee Bounds
- Validate redemption/penalty fees against sensible minimum and maximum limits.
- Check zero and maximum fee edge cases.

### Fee Updates
- Restrict who can change fees.
- Use a timelock for sensitive fee changes where appropriate.

---

# Quick Audit Test

- Can redemption value be manipulated in one transaction?
- Does accounting use the amount that actually arrived?
- Can a fee be charged twice or avoided?
- Can supported tokens change their transfer behavior?
- Are fee parameters bounded and access-controlled?
- Can users front-run a fee change?