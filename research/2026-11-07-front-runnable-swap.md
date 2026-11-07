# SlippagelessRouter.sol

| | |
|---|---|
| target | [front-runnable-swap]() |
| result | **RUG** (confidence 97%) |
| agent | `orb` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-07 · swarm-initiated |

## summary

[benchmark 1/1] No access control on rescue() lets anyone drain the entire token balance, and swapAll() has zero slippage protection and is callable by anyone.

## findings

- **[critical]** Missing access control on rescue() allows anyone to drain funds — rescue(address to) is external with no modifier and executes token.transfer(to, token.balanceOf(address(this))). Any address can call rescue(attacker) and transfer the entire token balance of the contract to themselves.
  - *fix:* Add an onlyOwner (Ownable) guard to rescue() and/or restrict the recipient.
- **[high]** swapAll() is permissionless and hardcodes amountOutMin = 0 — swapAll(address[] calldata path) is external with no access control, and calls router.swapExactTokensForTokens(amt, 0, path, address(this), block.timestamp + 300) with amountOutMin = 0. Any caller can trigger the swap and it is fully sandwichable / MEV-vulnerable, returning arbitrarily little output.
  - *fix:* Add onlyOwner to swapAll() and pass a caller-supplied amountOutMin with a require check instead of 0.
- **[low]** Ignored return value of token.approve — token.approve(address(router), amt) return value is not checked in swapAll(); a non-standard/false-returning token would silently leave allowance unset.
  - *fix:* Use require(token.approve(address(router), amt), "approve failed") or SafeERC20.safeApprove.
- **[low]** Ignored return value of token.transfer in rescue — rescue() calls token.transfer(...) without checking its boolean return, so a failed transfer would be silently swallowed.
  - *fix:* require(token.transfer(to, ...), "transfer failed") or use SafeERC20.safeTransfer.
- **[info]** benchmark: known bug "missing slippage protection" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
