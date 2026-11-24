# Introduction

Airdrop work is mostly waiting. Waiting for a faucet, waiting for a page to stop talking to
Cloudflare, waiting for a quest to unlock, waiting to see whether the project is even real. An
agent is useful here only if it can hold that waiting loop without you, and stop at exactly the
right moments.

This is the fourth generation of that agent. It is a **local operator**, not a cloud service: a
CLI that a person or another agent drives, a policy engine that decides what may happen without
asking, and a browser that carries the session between runs.

## What it is

- A quest runner with a declarative YAML format. You describe steps once; it executes them with
  proof and parks when a human is genuinely needed.
- A custody tool. Keys live encrypted on disk, signing happens locally, and every value transfer
  passes a spend policy before broadcast.
- A session keeper. One durable browser profile per identity, encrypted state mirror, warm-ups,
  and Cloudflare clearance that is reused instead of re-solved.
- A radar. It watches Telegram, X and the web, scores what it finds, and queues drafts.
- A shell for other agents. MCP tools and a router file so Hermes, Muse, OpenClaw or Claude can
  drive it without learning the internals.

## What it is not

- Not a captcha farm. Without a provider key it parks and hands you a screenshot.
- Not a guarantee. Selectors rot, projects change their flow, and points are not tokens.
- Not a bank. Keep only gas on farming wallets.

## The two design rules

**One path.** Every action goes through the CLI (`aa …`) or MCP, which returns one JSON envelope.
Shell scripts, cron, Telegram and LLM hosts all take the same road, so the policy cannot be
sidestepped by a caller that "knows better".

**Ask once, then move.** Risk is a decision, not a dialog. Modes decide how much runs unattended.
Anything above the line produces a payload hash and waits for a short code from the operator's
Telegram — the code is bound to the exact chain, recipient, amount and calldata.

## Where to start

1. [README](../README.md) — install and first run.
2. [GUIDE](GUIDE.md) — the full manual, from identity model to recovery.
3. [TARGETS](TARGETS.md) — the sites this build was tested against.
4. [SAFETY](SAFETY.md) and [SYBIL](SYBIL.md) — what the guard does and how to look human.
5. [CLOUDFLARE](CLOUDFLARE.md) and [SESSIONS](SESSIONS.md) — the two durability layers.

If you only read one section before touching a funded wallet, read the approval chapter in
[GUIDE](GUIDE.md#approvals).
