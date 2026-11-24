# Capability map

Requirement → command → honest limit. Everything below exists in this build.

| # | requirement | commands | limit |
|---|---|---|---|
| 1 | Telegram login + follow | `aa tg login/verify/join/follow/read/react` | dedicated SIM recommended; flood waits are respected |
| 2 | X login + follow | `aa social login x`, `aa x follow/like/repost/post` | a challenge needs one headful solve or a provider |
| 3 | Discord login + human chat for roles | `aa social login discord`, `aa dc grind/progress` | user token is self-bot, ToS-grey; captcha parks |
| 4 | EVM + Solana, extensions, mass wallets | `aa identity new --wallets N`, `aa wallet new --count`, `aa ext install` | extensions need headful |
| 5 | Run testnets | `aa testnet faucets/claim/deploy/campaign`, `aa wallet send` | needs gas; faucet selectors rot |
| 6 | Own email, Gmail login, form filling | `aa email add/otp/link`, `aa social login gmail`, quest `form` | Google signup usually ends in phone check |
| 7 | Complete airdrop quests | `aa quest probe/import/validate/run` | unknown flows stop at `manual` by design |
| 8 | Monitor TG/X and auto-execute drops | `aa radar scan/latest`, autopilot `radar` + `quests` | score is a filter; start with one identity |
| 9 | Daily points quests | `aa daily plan/run`, cooldowns + check-in ledger | idempotent per day |
| + | Cloudflare that holds | `aa cf assist/status/clear`, cache + provider + FlareSolverr | IP-bound; proxy change invalidates |
| + | Logins that survive | one profile, `aa session warm/save/restore`, pre-flight park | single writer per identity |
| + | 19 tested targets | `config/quests/*.yaml`, [TARGETS](TARGETS.md) | maintenance via `quest probe` |
| + | Guard, vault, MCP, services | modes, gates, KILL, MCP, systemd/launchd | — |

## What "auto" means

Auto = proceeds without a prompt **inside the policy**. It never pretends a captcha was solved, a
login happened, or a claim landed. Those park with a screenshot plus one `next` command.

## First hour

```bash
aa init && aa doctor
aa identity new me --wallets 2
aa email add --identity me --address you@domain.tld --password '…'
aa social login x --identity me --user … --password …
aa session warm --identity me
aa quest probe https://app.dowprotocol.com/points --identity me
aa testnet faucets --chain base-sepolia
aa quest run dow-points --identity me --dry
```

## First week

- one identity end-to-end: email → social → faucet → quest
- tune radar sources, threshold 60
- then more identities and `aa wallet new --count 10`
- leave Discord chat off until you have watched one grind

## Scale

Ten identities is one browser at a time, shared nothing. Fifty needs one proxy per identity, a
provider key, and autopilot budget caps. A thousand needs a second host and a queue in front of the
CLI.
