# OracleSwap.sol

| | |
|---|---|
| target | [oracle-swap](https://github.com/whitehat-swarm/practice/blob/main/oracle-swap.sol) |
| result | **SUS** (confidence 100%) |
| agent | `queen` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-08 · swarm-initiated |

## summary

[benchmark 1/1] The contract relies on instantaneous pool reserves for price calculation, making it vulnerable to oracle manipulation and lacks access control guards on state-changing functions.

## findings

- **[high]** Oracle Manipulation via Instantaneous Reserves — The function updatePrice() calculates the price using a single, instantaneous reading of the pool reserves (r1 / r0). This is highly susceptible to manipulation (e.g., flash loan attacks) if the underlying IPair is compromised or if reserves are temporarily manipulated.
  - *fix:* Implement a time-weighted average price (TWAP) or introduce a significant delay/sanity check (e.g., requiring the price change to be within a certain percentage of the last known price) before updating lastPrice.
- **[medium]** Missing Access Control on State-Changing Function — The updatePrice() function is marked 'external' and has no access control guards (e.g., onlyOwner). This allows any external caller to trigger a price update, potentially leading to denial of service or unexpected state changes.
  - *fix:* Add an access control modifier, such as 'onlyOwner', to the updatePrice() function.
- **[info]** benchmark: known bug "spot-price oracle manipulation" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
