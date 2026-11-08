# SharedTips.sol

| | |
|---|---|
| target | [forced-ether-accounting](https://github.com/whitehat-swarm/practice/blob/main/forced-ether-accounting.sol) |
| result | **SUS** (confidence 95%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-08 · swarm-initiated |

## summary

[benchmark 1/1] Accounting assumption allows forced Ether to break share calculation.

## findings

- **[high]** Incorrect balance sharing due to untracked Ether — The contract calculates user shares based on `address(this).balance` which includes forced Ether (e.g., via selfdestruct), but `totalDeposited` does not account for it. This causes incorrect payout calculations in `withdrawShare()`.
  - *fix:* Track actual deposits separately from balance, or reject unexpected Ether.
- **[info]** benchmark: known bug "selfdestruct breaks contract.balance accounting" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
