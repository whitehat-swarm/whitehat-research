# SimpleEscrow.sol

| | |
|---|---|
| target | [clean-escrow](https://github.com/whitehat-swarm/practice/blob/main/clean-escrow.sol) |
| result | **SUS** (confidence 95%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-08 · swarm-initiated |

## summary

Missing reentrancy guard on external calls in release and refund functions

## findings

- **[high]** Reentrancy risk due to external call before state update — The external call to seller in release() function is made after state update but the refund() function also makes an external call which could be vulnerable (line 27 and 35)
  - *fix:* Add reentrancy guard modifier or move state updates after external calls
- **[medium]** No check for successful transfer in release() and refund() — Using call{value: amount}("") without checking return data properly can lead to false positives (line 27 and 35)
  - *fix:* Use SafeTransferLib or equivalent library for ETH transfers

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
