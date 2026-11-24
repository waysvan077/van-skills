# Portability

## Contract

- Python >= 3.10, macOS or Linux.
- State in `AIRDROP_HOME` (default `~/.airdrop-agent`), never in the repo.
- All output is JSON; exit codes are part of the interface.
- No GUI requirement: headless is the default, headful is opt-in.

## Targets

| target | how |
|---|---|
| laptop | `./install.sh` |
| VPS | `./install.sh --no-browser` then `playwright install chromium --with-deps` |
| container | same, mount `~/.airdrop-agent` as a volume |
| cron | `aa autopilot tick` |
| systemd user unit | `templates/systemd/`, `install.sh --service=systemd` |
| launchd | `templates/launchd/`, `install.sh --service=launchd` |
| MCP host | `aa mcp serve` |

## Moving hosts

1. `aa vault export /safe/bundle.json` — stop the daemon first.
2. Copy `.env`, `policy.yaml`, `quests/`, `autopilot.yaml` and the bundle. **Not** `state.sqlite3`
   unless you also want run history.
3. New host: install, `aa init`, `aa vault import /safe/bundle.json`, restore config files.
4. `aa doctor`, then resume.

## Environments

`AIRDROP_HOME` lets several independent agents coexist: `AIRDROP_HOME=~/.aa-alice aa status`.
`VAULT_PASSWORD_FILE` is for injected secrets (Docker/SOPS) instead of a plain `.env`.
`PLAYWRIGHT_BROWSERS_PATH` / `AA_BROWSERS_PATH` for pre-seeded browser installs.
`AA_BROWSER=patchright` to try the stealth fork (default is playwright; see engine notes).

## What is not portable

- Social sessions are device-tied in practice; expect a fresh login on a new host/IP.
- Telegram may demand a fresh code; keep the SIM.
- A vault bundle without its password is useless. Store both separately.
- The browser profile directory is portable but large; copying it can carry fingerprints across hosts.
