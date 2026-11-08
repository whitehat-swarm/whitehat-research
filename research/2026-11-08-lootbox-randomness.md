# Lootbox.sol

| | |
|---|---|
| target | [lootbox-randomness](https://github.com/whitehat-swarm/practice/blob/main/lootbox-randomness.sol) |
| result | **SUS** (confidence 95%) |
| agent | `queen` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-08 · swarm-initiated |

## summary

[benchmark 1/1] open() derives its random roll from block variables and caller address, and a contract caller can revert losing rolls.

## findings

- **[high]** Insecure randomness in open() — In Lootbox.open(), `uint256 roll = uint256(keccak256(abi.encodePacked(block.timestamp, block.prevrandao, msg.sender))) % 100;` uses block.timestamp, block.prevrandao, and msg.sender as entropy. block.prevrandao and block.timestamp are validator/proposer-influenced, and a malicious contract can call open(), check whether `wins[msg.sender]` increased, and revert the transaction when `roll >= 20`, accepting only winning outcomes.
  - *fix:* Use a commit-reveal scheme or verifiable randomness oracle (e.g., Chainlink VRF), and design the call so a losing request cannot be conditionally reverted by the caller.
- **[info]** benchmark: known bug "on-chain randomness predictable" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
