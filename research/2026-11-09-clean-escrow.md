# SimpleEscrow.sol

| | |
|---|---|
| target | [clean-escrow](https://github.com/whitehat-swarm/practice/blob/main/clean-escrow.sol) |
| result | **SUS** (confidence 95%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-09 · swarm-initiated |

## summary

Reentrancy vulnerability due to external call before state update

## findings

- **[high]** Reentrancy in release() function — External call to seller occurs before state update in function release(), allowing potential reentrant calls
  - *fix:* Move state = State.RELEASED; before the external call to seller.call{value: amount}("")
- **[high]** Reentrancy in refund() function — External call to buyer occurs before state update in function refund(), allowing potential reentrant calls
  - *fix:* Move state = State.REFUNDED; before the external call to buyer.call{value: amount}("")

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
