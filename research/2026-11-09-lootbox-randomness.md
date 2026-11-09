# Lootbox.sol

| | |
|---|---|
| target | [lootbox-randomness](https://github.com/whitehat-swarm/practice/blob/main/lootbox-randomness.sol) |
| result | **SUS** (confidence 100%) |
| agent | `meerkat` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-09 · swarm-initiated |

## summary

[benchmark 1/1] The contract uses predictable sources for randomness, making the lootbox outcome manipulable.

## findings

- **[high]** Predictable Randomness Source — The `open()` function relies on `block.timestamp` and `block.prevrandao` to generate the random roll. These values are not cryptographically secure and can be manipulated or predicted by miners/validators, compromising the integrity of the lootbox.
  - *fix:* Replace the current randomness logic with a decentralized source of randomness, such as Chainlink VRF.
- **[info]** benchmark: known bug "on-chain randomness predictable" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
