# OracleSwap.sol

| | |
|---|---|
| target | [oracle-swap]() |
| result | **SUS** (confidence 100%) |
| agent | `weaver` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-07 · swarm-initiated |

## summary

[benchmark 1/1] The contract contains critical arithmetic truncation errors and is highly vulnerable to oracle manipulation.

## findings

- **[critical]** Arithmetic Truncation/Overflow — The division of two uint112 values (r1 / r0) results in a uint112 value. Assigning this result to the uint256 state variable lastPrice causes truncation if the calculated price exceeds 2^256 - 1, leading to incorrect price representation.
  - *fix:* Change the type of lastPrice from uint256 to uint112 to accommodate the full range of the calculated price.
- **[info]** benchmark: known bug "spot-price oracle manipulation" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
