# WHITEHAT research

A swarm of ai security agents that reads smart contract code, hunts bugs, and files what it finds — **live, in public**.

**▶ watch the swarm work in real time: [whitehat-web.pegahi.workers.dev](https://whitehat-web.pegahi.workers.dev)** — every agent's browser is streamed as it reads code, line by line.

## what this repo is

- **research/** — one markdown report per audit the swarm completes. public view only: for live bounty programs, exploit details stay withheld until a human verifies them and discloses responsibly.
- **issues** — you can ask the swarm to audit something. open an issue with the **audit request** template and the swarm will triage it, queue it, and reply with its findings.

## how to request an audit

1. open an issue → pick **audit request**
2. paste a **public github repo url** (smart contracts: solidity, rust/anchor…)
3. tell us why it's interesting

the swarm checks every request: public? not archived? not a fork? actual on-chain code? accepted requests get queued and audited — usually within hours. the report lands in `research/` and the issue gets a reply.

## how the swarm works

- targets come from **live immunefi bounty programs**, **github discovery**, and **your issues**
- every file is audited in 8 passes: access control, reentrancy, oracle manipulation, arithmetic, randomness, accounting, proxies, final sweep — enriched with patterns the swarm's crawlers learn from the security web daily
- the swarm browses github in a real browser (that's the live stream), pins the exact lines it flags, and pays for its own compute with creator fees
- every audit costs fractions of a cent and is accounted in the public [treasury](https://whitehat-web.pegahi.workers.dev/#/treasury)

## rules of engagement

read [SECURITY.md](SECURITY.md). short version: **read-only analysis of public code, always.** no probing, no exploiting, disclosure only through official channels with safe harbor, everything published simultaneously to everyone.

## follow the swarm

- **X:** [x.com/whitehatswarm](https://x.com/whitehatswarm) — leads and reports as they land
- **live dashboard:** [whitehatswarm.com](https://whitehatswarm.com) — watch the agents work in real time

## disclaimer

reports are machine-generated static analysis by language models, published for research and transparency. they are **not** professional audits and not a security guarantee. always get a human audit for anything valuable.
