# Architecture

## Shape

```
caller (shell | MCP | cron | agent)
        │
        ▼
      aa/cli.py ──► jsonio envelope (ok / denied / gate / kill)
        │
        ├── policy.py ── mode, caps, allowlist, blocklist, domain verdict
        ├── gates.py  ── payload hash + human code + TTL
        ├── guard.py  ── KILL file
        ├── vault.py  ── AES-256-GCM + scrypt, AAD bound
        ├── chains.py ── RPC with chain_id verification
        ├── social/   ── Telethon, X browser, Discord gateway
        ├── browser/  ── stealth engine + provider injection + guards
        │                 ├── cf.py      clearance cache, turnstile, assist
        │                 └── captcha.py provider + frame-safe token injection
        ├── session.py ── profile reuse, warm-up, encrypted state mirror
        ├── wallet/   ── hd, evm, sol, sign, send, extensions
        ├── quest/    ── YAML DSL, runner, site adapters
        ├── testnet/  ── faucets, deploy, campaigns
        ├── radar/    ── sources, extract, score, pipeline
        └── daily.py autopilot.py
```

## Why the CLI is the only surface

Policy has to be unavoidable. If a caller could import `wallet.send` directly, a host could skip a
gate. Every entry point — `aa`, MCP, autopilot, cron — funnels through `cli.py`, and `send()` re-checks
policy itself even then. Two independent locks: caller-level envelope and library-level checks.

## Transaction pipeline

```
decode → simulate → caps (per-tx, gas, daily) → blocklist → gate (payload hash)
      → sign (vault) → broadcast → receipt → proof + spend row
```

Simulation comes before caps because a revert should cost nothing and should not create a gate.
A gate is created for the *exact* payload; if anything changes, the hash changes and the old approval
is useless. Spends are recorded only after a successful broadcast.

## Wallet injection

The Python process is the signer; the page only sees a provider object.

```
page JS  ──►  window.ethereum / window.solana
        ──►  exposed binding  ──►  WalletBridge (Python)
        ──►  scam_check + origin allowlist + policy + gates
        ──►  vault sign / send
```

The bridge rejects any signing request when `document.activeElement` origin is not what the summary
shows. `eth_sign` (blind hash) is always gated; `personal_sign` and typed data get a SIWE domain check.

## State

SQLite holds identities, wallets, socials, emails, quests, runs, gates, spends, check-ins, events,
radar items, chat memory and follows. Vault holds the secrets. Nothing secret is in SQLite or in logs.

## Autopilot

A tick checks `autopilot.yaml` switches, active hours, per-identity rate windows and daily budgets,
then runs at most one instance of each due module. Every module writes a run/event row so a crash
resumes rather than repeats.

## Durability layers

Two problems sit outside policy but decide whether anything runs at all: Cloudflare and session
death. Both are handled structurally rather than by retries.

`cf.py` treats clearance as a credential: it is bound to host and user agent, stored in the vault,
re-injected before navigation, and refreshed by natural interaction, a provider token, FlareSolverr,
or one manual assist. `session.py` treats login as state: one profile per identity, encrypted
`storage_state` mirror, warm-up, and a pre-flight that parks instead of failing mid-quest.

## Failure philosophy

Every browser flow can park. Parking produces a screenshot, an event row, and an envelope with `next`;
it never produces a fake success. Retries are bounded (3) and respect `TG_MAX_FLOOD_WAIT`.
