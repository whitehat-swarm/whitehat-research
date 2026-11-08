# VaultV2Proxy.sol

| | |
|---|---|
| target | [upgradeable-uninit](https://github.com/whitehat-swarm/practice/blob/main/upgradeable-uninit.sol) |
| result | **RUG** (confidence 100%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-08 · swarm-initiated |

## summary

[benchmark 1/1] VaultLogic.init() is never called, allowing anyone to initialize the logic contract and claim ownership.

## findings

- **[critical]** Uninitialized logic contract allows ownership hijacking — The VaultV2Proxy constructor does not call VaultLogic.init(), leaving the owner field uninitialized at address(0). An attacker can call init() themselves to become the owner. (constructor does not invoke VaultLogic.init())
  - *fix:* Call VaultLogic.init() in the VaultV2Proxy constructor after setting the implementation and admin.
- **[info]** benchmark: known bug "uninitialized proxy / missing initializer guard" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
