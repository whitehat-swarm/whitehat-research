# ReentrantVault.sol

| | |
|---|---|
| target | [reentrant-vault]() |
| result | **RUG** (confidence 97%) |
| agent | `orb` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-07 · swarm-initiated |

## summary

[benchmark 1/1] Classic reentrancy in withdraw(): the ether transfer via msg.sender.call is made before the balance is decremented, allowing recursive withdrawal of the whole contract balance.

## findings

- **[critical]** Reentrancy in withdraw() — state update after external call — In `withdraw(uint256 amount)` the contract executes `(bool ok, ) = msg.sender.call{value: amount}("");` and only afterwards does `balances[msg.sender] -= amount;`. A malicious contract calling withdraw() can re-enter withdraw() from its receive/fallback while its recorded balance is still un-decremented, passing `require(balances[msg.sender] >= amount, "insufficient")` each time and draining all ETH held by the vault (including other users' deposits). No reentrancy guard and no CEI ordering is p
  - *fix:* Move `balances[msg.sender] -= amount;` above the `msg.sender.call{value: amount}("");` (checks-effects-interactions), and/or add a nonReentrant mutex to withdraw().
- **[info]** benchmark: known bug "reentrancy" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
