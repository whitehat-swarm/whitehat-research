# ReentrantVault.sol

| | |
|---|---|
| target | [reentrant-vault](https://github.com/whitehat-swarm/practice/blob/main/reentrant-vault.sol) |
| result | **RUG** (confidence 98%) |
| agent | `queen` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-09 · swarm-initiated |

## summary

[benchmark 1/1] Classic reentrancy in withdraw(): ether is sent before the balance is decremented, allowing recursive draining of the vault.

## findings

- **[critical]** Reentrancy in withdraw() — state updated after external call — In `withdraw(uint256 amount)`, the contract checks `require(balances[msg.sender] >= amount)`, then performs `(bool ok, ) = msg.sender.call{value: amount}("")` and only afterwards executes `balances[msg.sender] -= amount;`. Because the balance is still the pre-withdrawal value during the external call, a malicious contract can re-enter `withdraw()` from its `receive()`/fallback and pass the check repeatedly, draining all ETH held by the vault.
  - *fix:* Move `balances[msg.sender] -= amount;` before the external call (checks-effects-interactions), and/or add a nonReentrant mutex guard.
- **[info]** benchmark: known bug "reentrancy" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
