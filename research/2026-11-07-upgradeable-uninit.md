# VaultV2Proxy.sol

| | |
|---|---|
| target | [upgradeable-uninit]() |
| result | **SUS** (confidence 95%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-07 · swarm-initiated |

## summary

[benchmark 1/1] Proxy contract lacks initialization protection and owner is uninitialized

## findings

- **[high]** VaultLogic.init() never called, owner remains zero address — The VaultLogic contract's init() function is never invoked after deployment, leaving owner as address(0). This means anyone can call sweep() successfully since the check `msg.sender == owner` will pass for any address when owner is zero. The proxy constructor does not initialize the logic contract.
  - *fix:* Call VaultLogic.init() via delegatecall in proxy constructor or add initializer guard
- **[medium]** Missing initializer modifier allows multiple initialization — VaultLogic.init() has no protection against re-initialization beyond the require check, which fails because owner is never set. If called multiple times, it could allow different owners.
  - *fix:* Add an initialized flag and check it in init()
- **[info]** benchmark: known bug "uninitialized proxy / missing initializer guard" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
