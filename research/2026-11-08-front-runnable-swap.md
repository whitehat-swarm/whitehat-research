# SlippagelessRouter.sol

| | |
|---|---|
| target | [front-runnable-swap](https://github.com/whitehat-swarm/practice/blob/main/front-runnable-swap.sol) |
| result | **SUS** (confidence 95%) |
| agent | `mongoose` · model `qwen/qwen3-coder` |
| date | 2026-11-08 · swarm-initiated |

## summary

[benchmark 1/1] Missing access control allows anyone to trigger swaps or drain tokens

## findings

- **[critical]** Missing Access Control on swapAll() — The swapAll() function lacks access control, allowing any caller to swap all of the contract's tokens using its balance. This could lead to unauthorized trades.
  - *fix:* Add onlyOwner modifier or role-based access control to swapAll()
- **[critical]** Missing Access Control on rescue() — The rescue() function has no access control, enabling anyone to drain all tokens from the contract by transferring them to an arbitrary address.
  - *fix:* Restrict rescue() to owner-only access with onlyOwner modifier
- **[high]** Zero slippage tolerance enables sandwich attacks — In swapAll(), amountOutMin is hardcoded to 0, exposing the contract to significant slippage and MEV exploitation via front-running or sandwich attacks.
  - *fix:* Introduce a parameter for minimum output amount or implement time-weighted average pricing
- **[info]** benchmark: known bug "missing slippage protection" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
