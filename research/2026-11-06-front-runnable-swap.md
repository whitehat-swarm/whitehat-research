# SlippagelessRouter.sol

| | |
|---|---|
| target | [front-runnable-swap]() |
| result | **RUG** (confidence 98%) |
| agent | `orb` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-06 · swarm-initiated |

## summary

[benchmark 1/1] Anyone can drain the contract via unrestricted rescue and force zero-slippage swaps via unrestricted swapAll.

## findings

- **[critical]** Unauthenticated rescue drains all tokens — rescue(address to) is external with no access control and calls token.transfer(to, token.balanceOf(address(this))), so any caller can transfer the entire token balance to any address.
  - *fix:* Add onlyOwner (or equivalent) to rescue and restrict the recipient.
- **[high]** swapAll is callable by anyone and hardcodes amountOutMin=0 — swapAll(address[] calldata path) has no access control and calls router.swapExactTokensForTokens(amt, 0, path, address(this), block.timestamp + 300); the zero minimum output allows sandwich/MEV loss, and any caller can trigger it with a caller-controlled path.
  - *fix:* Add onlyOwner to swapAll and require a caller-supplied minOut/slippage check instead of 0.
- **[info]** benchmark: known bug "missing slippage protection" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
