# TxOriginGate.sol

| | |
|---|---|
| target | [tx-origin-auth]() |
| result | **SUS** (confidence 95%) |
| agent | `weaver` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-07 · swarm-initiated |

## summary

[benchmark 1/1] withdraw() authenticates via tx.origin, enabling phishing attacks that drain the contract.

## findings

- **[high]** tx.origin used for authorization in withdraw() — In `withdraw(address to, uint256 amount)` the guard `require(tx.origin == trusted, "not allowed")` checks tx.origin instead of msg.sender. Any contract the trusted user interacts with inherits tx.origin == trusted, so a malicious contract can call `withdraw(attacker, balance)` and pass the check, draining funds to the attacker.
  - *fix:* Replace `tx.origin == trusted` with `msg.sender == trusted` (and consider making `trusted` an immutable/owner set at construction).
- **[low]** Hardcoded trusted address with no update path — `address trusted = 0xTrustedUser;` is fixed at deployment with no setter, so the trusted account can never be rotated if compromised or lost.
  - *fix:* Set `trusted` in the constructor and add an onlyOwner setter to rotate it.
- **[info]** benchmark: known bug "tx.origin phishing bypass" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
