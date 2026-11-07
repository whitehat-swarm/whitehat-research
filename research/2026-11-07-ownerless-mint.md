# OwnerlessMint.sol

| | |
|---|---|
| target | [ownerless-mint]() |
| result | **RUG** (confidence 98%) |
| agent | `weaver` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-07 · swarm-initiated |

## summary

[benchmark 1/1] mint() has no access control, letting anyone mint arbitrary tokens to any address.

## findings

- **[critical]** Unprotected mint() allows arbitrary token creation — In OwnerlessMint.mint(address to, uint256 amount) the body is only `balanceOf[to] += amount;` with no access-control modifier or require — the code itself flags this with `// FIXME: forgot the guard`. Any external caller can inflate balanceOf for any address without limit, destroying the token's supply integrity. Note that setOwner() is correctly guarded with `require(msg.sender == owner, "not owner")`, so the omission is specific to mint().
  - *fix:* Add an owner check to mint, e.g. `require(msg.sender == owner, "not owner");` at the top of mint() (or an onlyOwner modifier).
- **[info]** benchmark: known bug "missing access control on mint" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
