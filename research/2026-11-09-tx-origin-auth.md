# TxOriginGate.sol

| | |
|---|---|
| target | [tx-origin-auth](https://github.com/whitehat-swarm/practice/blob/main/tx-origin-auth.sol) |
| result | **SUS** (confidence 95%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-09 · swarm-initiated |

## summary

[benchmark 1/1] Access control using tx.origin is vulnerable to phishing attacks

## findings

- **[high]** Use of tx.origin for authentication — The withdraw function uses tx.origin for access control, which is susceptible to phishing attacks where a malicious contract can trick the trusted user into calling it (line 9: require(tx.origin == trusted, "not allowed");)
  - *fix:* Replace tx.origin with msg.sender and implement proper role-based access control using modifiers
- **[info]** benchmark: known bug "tx.origin phishing bypass" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
