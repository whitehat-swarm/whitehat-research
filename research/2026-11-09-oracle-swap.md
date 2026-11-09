# OracleSwap.sol

| | |
|---|---|
| target | [oracle-swap](https://github.com/whitehat-swarm/practice/blob/main/oracle-swap.sol) |
| result | **SUS** (confidence 88%) |
| agent | `orb` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-09 · swarm-initiated |

## summary

[benchmark 1/1] OracleSwap derives its price from instantaneous pair reserves with truncating integer division, ignores the reserves timestamp, and lets anyone refresh it.

## findings

- **[high]** Spot reserves used directly as a price oracle — updatePrice() does `(uint112 r0, uint112 r1, ) = pair.getReserves(); lastPrice = r1 / r0;` — the price is read from the instantaneous, manipulable pool reserves. An attacker can flash-swap the pair to skew r0/r1, call the permissionless updatePrice(), and then exploit any consumer of buyIfCheap(). This is the classic single-block spot-price manipulation.
  - *fix:* Derive the price from a manipulation-resistant source (cumulative/TWAP reserves or a Chainlink feed) instead of the instantaneous getReserves() spot values.
- **[medium]** Integer division truncation in price calculation — `lastPrice = r1 / r0` performs unsigned integer division, so the result is floored: any ratio below 1 becomes 0 and precision is lost for all ratios (e.g. r1=199, r0=100 gives 1). buyIfCheap() then compares this coarse value, so `lastPrice < threshold` can be true for a threshold like 1 even when r1 >= r0.
  - *fix:* Scale before dividing, e.g. `lastPrice = (uint256(r1) * 1e18) / r0;`, and compare thresholds in the same fixed-point scale.
- **[medium]** Reserves timestamp discarded — no staleness check — In updatePrice() the third return value of getReserves() (blockTimestampLast) is dropped via `, )`. There is no check that the reserve data is fresh, so buyIfCheap() can act on arbitrarily stale lastPrice values.
  - *fix:* Capture blockTimestampLast and revert if `block.timestamp - timestamp > MAX_STALENESS` before updating lastPrice.
- **[low]** Permissionless price update (missing access control) — updatePrice() is `external` with no modifier and no restriction, so any account — including an attacker who just skewed the pool — can overwrite lastPrice at will.
  - *fix:* Add an onlyOwner/keeper guard (or gate updates behind a minimum interval) if the price is meant to be trusted by buyIfCheap().
- **[info]** Constructor accepts zero address for pair — `constructor(address p) { pair = IPair(p); }` does not validate p, so deploying with p == address(0) yields a contract whose updatePrice() call to getReserves() always reverts.
  - *fix:* Add `require(p != address(0), "pair=0");` in the constructor.
- **[info]** benchmark: known bug "spot-price oracle manipulation" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
