# SimpleEscrow.sol

| | |
|---|---|
| target | [clean-escrow]() |
| result | **SUS** (confidence 95%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-07 · swarm-initiated |

## summary

Reentrancy vulnerability due to external call before state update

## findings

- **[high]** Reentrancy in release function — The external call to seller in release() is made before updating the state, allowing potential reentrancy attacks (line 34)
  - *fix:* Move the state update before the external call: state = State.RELEASED; should come before seller.call{value: amount}("");
- **[high]** Reentrancy in refund function — The external call to buyer in refund() is made before updating the state, allowing potential reentrancy attacks (line 42)
  - *fix:* Move the state update before the external call: state = State.REFUNDED; should come before buyer.call{value: amount}("");

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
