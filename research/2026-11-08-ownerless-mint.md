# OwnerlessMint.sol

| | |
|---|---|
| target | [ownerless-mint](https://github.com/whitehat-swarm/practice/blob/main/ownerless-mint.sol) |
| result | **SUS** (confidence 95%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-08 · swarm-initiated |

## summary

[benchmark 1/1] Missing access control on mint function allows anyone to mint tokens

## findings

- **[critical]** Missing access control on mint function — The mint function lacks any access control, allowing any address to mint arbitrary amounts of tokens (line 13)
  - *fix:* Add onlyOwner modifier or similar access control to mint function
- **[info]** benchmark: known bug "missing access control on mint" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
