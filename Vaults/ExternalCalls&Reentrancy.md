# Vaults · Every external call hands someone else the wheel

## 1. External Call Safety

### Identify External Calls
Flag every:
- Native token transfer(triggers the receive or fallback handing control to the recipient mid-function)
- ERC20 
- ERC721 transfer (calls onERC721Received(), recipient gets execution control)
- Strategy/protocol call
- Oracle/external state read

### CEI & Reentrancy Guard
- Follow **Checks → Effects → Interactions**.
- Apply `nonReentrant` to exposed state-changing paths involving external calls.
- Don't assume tokens are callback-free.

---

## 2. Cross-Function Reentrancy

- If Function A is guarded, can the callback enter through Function B?
- Functions sharing/interacting with the same state should share appropriate reentrancy protection.
- Check whether callbacks can exploit another function while state is partially updated.

---

## 3. Read-Only Reentrancy

### Internal State
- Can `view` functions be called while state is only partially updated?
- Could temporary balances/reserves produce incorrect:
  - Prices
  - Share values
  - Liquidity quotes
  - Fees

### External State
- When consuming data from another protocol, can that protocol be mid-transition?
- Be especially suspicious of calculations such as:

```text
price = reserveA / reserveB
sharePrice = assets / totalSupply
```

where one value may update before the other.

- Consider checking reentrancy-guard status before sensitive reads.

---

## 4. Callback Entry Points

Treat these as potentially handing execution to an attacker:

- Native token sends
- Token callback/hooks
- `safeTransfer` callbacks (ERC721/ERC1155)
- Arbitrary strategy/external calls

Ask:

> What can the receiver do before this function finishes?

---

# Quick Audit Test

At **every external call**, mentally freeze execution and ask:

- Is all important state already consistent?
- Would every public `view` return the correct value right now?
- Can the callback enter another function?
- Can another protocol consume this temporary state?
- Can the callback manipulate pricing, shares, fees, or accounting?

> **External call = attacker gets execution. Assume they will use every available entry point and every observable intermediate state.**