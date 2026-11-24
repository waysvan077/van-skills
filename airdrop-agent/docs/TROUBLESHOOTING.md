# Troubleshooting

## Boot

**`browser executable doesn't exist`** → `.venv/bin/playwright install chromium`. Set
`PLAYWRIGHT_BROWSERS_PATH` if your browsers live elsewhere. On Linux CI add `--with-deps`.

**`VAULT_PASSWORD is not set`** → `aa vault password` or edit `.env`. `aa init` generates one; back it up.

**`policy.yaml` missing / empty chains** → `aa init` re-copies templates, then `aa doctor`.

**import errors for `solders`, `discord`, `telethon`** → `pip install -e ".[full]"`. Minimal install
works for watch-only but not for Solana/Discord/Telegram.

## Guard

**exit 10, `approval #N required`** → `aa gate list`, read summary, code from Telegram, `aa gate approve N CODE`,
re-run the identical command. Changing amount/recipient/domain needs a new gate.

**exit 2, `value ... > per-tx cap`** → lower the amount, or raise `max_tx_native` in policy.yaml, or
`aa mode turbo` for testnet. Cap is per payload, not cumulative.

**exit 2, `contract not allowlisted`** → verify the address from official docs, add under
`contracts_allowlist.<chain>` in policy.yaml.

**exit 2, `daily cap reached`** → wait until UTC midnight or raise the daily cap deliberately.

**exit 99, `KILL switch is set`** → `aa unkill`. Check `aa events` for why it tripped.

**exit 3, `simulation reverted`** → usually unfunded wallet or a stale calldata. `aa wallet balance`,
faucet, re-run.

## Cloudflare / sessions

**"Just a moment" loop** → `aa cf status`; if the host is cached the cookie is stale, `aa cf clear
--host host`. Then `aa cf assist --url <u> --identity <id>`. On servers set `FLARESOLVERR_URL` and a
provider key. Do not retry in a tight loop; each retry burns score.

**clearance stopped working after a proxy change** → expected, it is IP-bound. Keep one proxy per
identity and re-assist once on the new IP.

**`ProcessSingleton` on launch** → the engine clears the stale lock and retries three times; if it
persists, `rm ~/.airdrop-agent/browser/<id>/main/Singleton*` and retry.

**quest returns `needs_login`** → `aa social login <platform> --identity <id>`, then `aa session
warm --identity <id>`, then re-run. Do not force the quest; the parked row is not a failure.

**`session restore` restores nothing** → the mirror expired (`SESSION_STATE_DAYS`) or the profile was
used with `BROWSER_SEPARATE_PROFILES=1`. Log in once.

## Browser / quests

**captcha** → set `CAPTCHA_API_KEY` (2captcha/anticaptcha), or run `--headful` and finish by hand.
Blind retries just burn quota.

**login wall** → `aa social login <platform>` (or `aa browser --login`), then warm. Cookies persist in
the identity profile; quests pre-flight this and park instead of failing.

**selector miss** → `aa browser --identity <id> --url <url> --login`, inspect, patch the quest YAML, re-validate.
Proof screenshots are in `~/.airdrop-agent/proofs/`.

**quest parked forever** → `aa quest show <id>`; a `manual` step always parks. Clear with `aa quest remove`.

## Social

**Telegram flood wait** → already respected up to `TG_MAX_FLOOD_WAIT`; if you hit longer, stop for the day.

**Telegram `SESSION_REVOKED`** → `aa tg login` again; kill old sessions in Telegram settings.

**X challenge / suspicious login** → headful login once, solve it, then reuse cookies.

**Discord token not captured** → `aa dc login` headful; if it still fails, put `DISCORD_USER_TOKEN` in `.env`.

## Data

**wrong mode winning** → `aa mode` reads the DB override first, then `AA_MODE`. Delete `mode` row via
`aa mode balanced` if confused.

**DB locked** → autopilot daemon and a manual run collide; stop the daemon for bulk work.

**`no such table`** → `aa init`; schema is created on every connection, so this is usually an empty/corrupt
home — check `AIRDROP_HOME`.

## Diagnostics

```bash
aa doctor
aa status
aa events --limit 50
AA_DEBUG=1 aa --debug <command>
```

Paste the envelope's `error` and the matching event rows when reporting a bug. Never paste a private key.
