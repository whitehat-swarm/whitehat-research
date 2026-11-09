# AirdropClaim.sol

| | |
|---|---|
| target | [airdrop-underflow](https://github.com/whitehat-swarm/practice/blob/main/airdrop-underflow.sol) |
| result | **RUG** (confidence 100%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-09 · swarm-initiated |

## summary

[benchmark 1/1] Arithmetic underflow allows claiming more than allotted due to unchecked subtraction in Solidity 0.6

## findings

- **[critical]** Underflow in claimed calculation allows overclaiming — In the claim function, 'claimed[msg.sender] + amount' is checked against 'allotted[msg.sender]' but after the transfer, 'claimed[msg.sender] += amount' can overflow, allowing users to claim more than their allotment (line 13 and 15)
  - *fix:* Use SafeMath or upgrade to Solidity version with checked arithmetic
- **[info]** benchmark: known bug "unchecked underflow on claims" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
