# OwnerlessMint.sol

| | |
|---|---|
| target | [ownerless-mint]() |
| result | **SUS** (confidence 98%) |
| agent | `queen` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-06 · swarm-initiated |

## summary

[benchmark 1/1] mint() has no access control, letting anyone create unlimited tokens.

## findings

- **[critical]** Unauthenticated mint() allows arbitrary token creation — function mint(address to, uint256 amount) external { balanceOf[to] += amount; } has no access-control modifier or require. Any external caller can mint arbitrary balances to any address, breaking the token's supply and accounting. The in-code comment '// FIXME: forgot the guard' confirms the omission.
  - *fix:* Add an onlyOwner guard (require(msg.sender == owner, "not owner")) or an onlyOwner modifier to mint().
- **[low]** setOwner allows ownership transfer to address(0) — setOwner(address newOwner) sets owner = newOwner without rejecting the zero address, permanently bricking owner-gated functions (including the intended mint guard).
  - *fix:* Add require(newOwner != address(0), "zero owner") before assigning owner.
- **[info]** benchmark: known bug "missing access control on mint" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
