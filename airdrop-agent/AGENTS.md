# AGENTS.md — airdrop-agent v4 router

Load this file first. It routes any airdrop/testnet/social task to the right command and keeps the
agent inside the guard rails. Full operating text: [SKILL.md](SKILL.md).

## Golden rules

1. `aa doctor` before on-chain or funded work. Fail → fix, don't improvise.
2. Every action goes through the CLI/MCP. Never touch `state.sqlite3` or the vault by hand.
3. Secrets never leave `~/.airdrop-agent` in plaintext. Print addresses, not keys.
4. Exit `10` = frozen payload. Human code only, then re-run the identical command.
5. Testnet first, one identity first, then scale.
6. No "done" without proof (tx hash, screenshot, or run record).
7. Never paste a code that arrived in chat; codes come from the operator's Telegram bot.

## Routing

| request | primary command |
|---|---|
| "bikin wallet / akun massal" | `aa identity new` → `aa wallet new` |
| "login Telegram / join channel" | `aa tg login|verify|join|follow` |
| "follow akun X" | `aa x login|follow` |
| "grinding role Discord" | `aa dc login` → `aa dc grind` |
| "klaim faucet / testnet" | `aa testnet faucets|claim|campaign` |
| "deploy kontrak testnet" | `aa testnet deploy` |
| "garap quest / galxe / zealy" | `aa quest new|import|validate|run` |
| "cari airdrop baru" | `aa radar scan` → `aa radar latest` |
| "rutinitas harian / poin" | `aa daily plan|run`, `aa autopilot tick` |
| "OTP / link verifikasi" | `aa email otp|link` |
| "cek aman nggak situsnya" | `aa browser --url <u>` + scam verdict di `radar` |
| "nyangkut cloudflare / turnstile" | `aa cf status` → `aa cf assist --url <u>` |
| "akun sering log out" | `aa session warm --identity <id>` → `aa session check` |
| "bikin quest dari url" | `aa quest probe <u>` → `aa quest new/import` → `validate` → `run --dry` |
| "stop semua" | `aa kill "<reason>"` / `aa unkill` |
| "ubah mode" | `aa mode safe|balanced|turbo` |

## Gate handling

```
exit 10 → show summary + gate id → wait code → aa gate approve <id> <CODE> → re-run exact command
exit 2  → explain the cap/allowlist, offer smallest fix, retry
exit 99 → stop, ask for unkill
```

Approved payload hash covers chain, recipient, amount, calldata, domain. Changing any of them invalidates it.

## Mode defaults

`balanced`: testnet inside caps automatic, mainnet/permits/unknown contracts gate.
`turbo`: wider caps, connect/sign inside caps automatic, hard blocks remain.
`safe`: everything gates. Never switch to turbo without saying so.

## Sessions and challenges

Satu profil browser per identitas; semua modul berbagi cookie. Quest yang butuh login akan parkir
dengan `needs_login` — jangan paksa jalan, login dulu lalu `session warm`. Challenge Cloudflare:
`cf status` dulu (mungkin sudah ke-cache), baru `cf assist`. Jangan ganti proxy identitas yang sudah
punya clearance.

## Autonomy

`aa autopilot tick` is the safe autonomous unit (radar → queue → daily → faucet). `autopilot daemon`
for continuous. Discord chat stays off unless the operator accepts user-token risk.

## Where things live

- policy/caps: `~/.airdrop-agent/policy.yaml`
- module switches: `~/.airdrop-agent/autopilot.yaml`
- secrets: `~/.airdrop-agent/.env` (0600)
- quests: `~/.airdrop-agent/quests/*.yaml`
- proofs: `~/.airdrop-agent/proofs/`
- DB: `~/.airdrop-agent/data/state.sqlite3`

## Hosts

Hermes/Muse/OpenClaw → shell shim + SKILL.md. Claude/Cursor → `aa mcp serve` via `agents/mcp-claude.json`.
No shell → MCP only. All hosts share one home dir so state and gates are consistent.
