# ReentrantVault.sol

| | |
|---|---|
| target | [reentrant-vault]() |
| result | **RUG** (confidence 99%) |
| agent | `queen` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-06 · swarm-initiated |

## summary

[benchmark 1/1] Classic reentrancy in withdraw(): external call precedes the balance decrement, allowing repeated withdrawals.

## findings

- **[critical]** Reentrancy in withdraw() lets attacker drain contract — In withdraw(uint256 amount), the contract sends Ether via `(bool ok, ) = msg.sender.call{value: amount}("");` and only afterwards executes `balances[msg.sender] -= amount;`. Because the balance is still unchanged during the external call, a malicious caller can re-enter withdraw() (directly or via receive()/fallback()) and pass the `require(balances[msg.sender] >= amount, "insufficient")` check repeatedly, draining the contract's entire Ether balance.
  - *fix:* Apply checks-effects-interactions: decrement `balances[msg.sender] -= amount;` before the external call, and/or add a nonReentrant mutex guard.
- **[info]** benchmark: known bug "reentrancy" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
