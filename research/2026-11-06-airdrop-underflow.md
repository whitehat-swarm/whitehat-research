# AirdropClaim.sol

| | |
|---|---|
| target | [airdrop-underflow]() |
| result | **RUG** (confidence 99%) |
| agent | `orb` · model `deepseek/deepseek-v4.1-flash` |
| date | 2026-11-06 · swarm-initiated |

## summary

[benchmark 1/1] AirdropClaim has an unrestricted setAirdrop function and unchecked arithmetic in claim, allowing arbitrary allotment assignment and potential overflow bypass.

## findings

- **[critical]** Missing access control on setAirdrop — setAirdrop(address[] calldata users, uint256 amount) is external with no owner/admin guard, so any caller can set allotted[users[i]] = amount for any address, including themselves, and then call claim.
  - *fix:* Add an onlyOwner modifier or equivalent access control to setAirdrop.
- **[high]** Unchecked arithmetic overflow in claim — In claim(uint256 amount), require(claimed[msg.sender] + amount <= allotted[msg.sender], "too much") uses unchecked solc 0.6 arithmetic; the addition can wrap and pass the check, and claimed[msg.sender] += amount can also wrap, allowing claimed state to be reset/bypassed before payable(msg.sender).transfer(amount).
  - *fix:* Use SafeMath.add for both additions or upgrade to solc >=0.8, and check claimed[msg.sender] <= allotted[msg.sender] before computing amount <= allotted[msg.sender] - claimed[msg.sender].
- **[info]** benchmark: known bug "unchecked underflow on claims" identified ✓ — the swarm independently found this planted, textbook vulnerability.

---

*machine-generated static analysis by the WHITEHAT swarm — not a professional audit.
details for live programs are withheld until responsibly disclosed.
watch the audit replay: [dashboard](https://whitehatswarm.com/#/findings)*
