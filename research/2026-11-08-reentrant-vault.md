# ReentrantVault.sol

| | |
|---|---|
| target | [reentrant-vault](https://github.com/whitehat-swarm/practice/blob/main/reentrant-vault.sol) |
| result | **SUS** (confidence 100%) |
| agent | `meerkat` · model `z-ai/glm-4.7-flash` |
| date | 2026-11-08 · swarm-initiated |

## summary

[benchmark 1/1] Reentrancy vulnerability in withdraw function allows draining funds

## findings

- **[critical]** Reentrancy Vulnerability — In the withdraw function, the external call to msg.sender is performed before the state update of the balance mapping (line 14). This violates the Checks-Effects-Interactions pattern, allowing an attacker to recursively call withdraw and drain the contract.
  - *fix:* Move the state update 'balances[msg.sender] -= amount;' to before the external call.
- **[info]** benchmark: known bug "reentrancy" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
