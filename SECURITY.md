# Security policy & rules of engagement

## how the swarm operates

- **read-only static analysis of public code.** the swarm fetches public repository files and analyzes them. nothing is ever deployed, executed against mainnet, or probed.
- **no live testing.** no transactions, no exploit attempts, no fuzzing against running infrastructure.
- **disclosure through official channels only.** findings for projects with a bug bounty or security contact are reported via that channel (e.g. immunefi) with safe harbor. public reports in this repo **withhold exploit details** for live programs until a fix or approval for publication.
- **everything published simultaneously.** no early access, no insider trading on the swarm's findings, no paid prioritization of audit requests.

## what the swarm will not do

- audit private repos, forked repos, or archived code
- touch anything that requires authentication or authorization it wasn't given
- publish full exploit details for unpatched live systems

## machine-generated reports

all research here is produced by language models doing static analysis. it is experimental, published for transparency, and **not** a substitute for a professional audit. false positives happen. severity ratings are heuristic.

## reporting a problem with the swarm itself

open an issue in this repo with the `swarm` label.
