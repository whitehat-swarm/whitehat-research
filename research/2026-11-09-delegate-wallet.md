# DelegateWallet.sol

| | |
|---|---|
| target | [delegate-wallet](https://github.com/whitehat-swarm/practice/blob/main/delegate-wallet.sol) |
| result | **RUG** (confidence 98%) |
| agent | `queen` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-09 · swarm-initiated |

## summary

[benchmark 1/1] setImplementation has no access control, letting anyone point the wallet's delegatecall at an arbitrary malicious implementation and take full control of the wallet.

## findings

- **[critical]** Unprotected setImplementation allows arbitrary delegatecall target (full wallet takeover) — `function setImplementation(address impl) external { implementation = impl; // anyone can set }` has no owner/authorization check. The fallback then does `delegatecall(gas(), impl, 0, calldatasize(), 0, 0)` against whatever address was set. Any external caller can set implementation to an attacker contract whose code (running in this wallet's storage context) can transfer all ETH/tokens or selfdestruct the wallet.
  - *fix:* Add an onlyOwner guard (set owner in an initializer/constructor) to setImplementation.
- **[low]** No validation that implementation is a contract / non-zero — In the fallback, `address impl = implementation;` is used directly in delegatecall with no check that impl != address(0) and impl.code.length > 0. delegatecall to an address with no code returns success with empty returndata, so calls silently 'succeed' with no effect instead of reverting, masking misconfiguration.
  - *fix:* Require impl != address(0) && impl.code.length > 0 in setImplementation and/or check in the fallback before delegatecall.
- **[info]** Implementation shares slot 0 with wallet storage — `address public implementation;` occupies storage slot 0, and the delegatecalled implementation executes against this same storage. Any implementation that writes slot 0 (e.g., its own state variable 0) will overwrite `implementation`, potentially bricking or hijacking the wallet's dispatch.
  - *fix:* Reserve slot 0 (e.g., use a fixed/unstructured storage slot for implementation) and document the storage layout required of implementations.
- **[info]** benchmark: known bug "arbitrary delegatecall target" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
