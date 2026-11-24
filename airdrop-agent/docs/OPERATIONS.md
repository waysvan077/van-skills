# Operations

## Daily

```bash
aa doctor
aa autopilot plan          # what is due
aa autopilot tick          # one safe pulse
aa daily run --identity main
aa radar latest --min-score 60
```

## Weekly

- `aa doctor` full (RPC, sessions, IMAP, browser)
- review `aa radar latest` and promote at most a few quests
- `aa daily status` streaks; drop dead projects
- `aa vault export ~/aa-$(date +%F).json` and move it offline
- rotate anything that leaked into a chat screenshot

## Modes

Start `balanced`. Use `safe` when auditing an unknown site or when a bug is suspected. Use `turbo`
only for testnet bursts, and say so out loud. `aa kill` first when something looks wrong.

## Gates

1. `aa gate list`
2. read the summary (chain, recipient, amount, call, balance delta, domain verdict)
3. code from the Telegram bot only
4. `aa gate approve <id> <CODE>`
5. re-run the identical command

A gate pending for >12h is dead: `aa gate expire`, then re-run to get a fresh one.

## Services

```bash
sudo systemctl --user enable --now airdrop-agent   # after install.sh --service=systemd
launchctl load -w ~/Library/LaunchAgents/com.airdrop-agent.plist
journalctl --user -u airdrop-agent -f
tail -f ~/.airdrop-agent/logs/*.log
```

## Backups

- `~/.airdrop-agent/.env` (vault passphrase!) — offline, two copies.
- `aa vault export` periodically.
- `state.sqlite3` is reproducible from the vault + config; back it up for convenience, not survival.
- Session mirror lives in the vault, so `vault export` also carries logins. Profile folders are not
  backed up; rebuild with `aa session restore` after a reinstall.
- Clearance secrets (`cf-*`) are in the vault too. Restoring on a different IP invalidates them;
  re-assist instead of debugging.

## Health signals

`aa events` should show runs, not a wall of retries. A growing `radar_items` with zero runs means the
quest step is broken. Empty `proofs/` after a "done" is a lie — check the run row.

## Cron (if no service manager)

```cron
*/20 * * * * cd /path/to/agent && AIRDROP_HOME=$HOME/.airdrop-agent .venv/bin/aa autopilot tick >> $HOME/.airdrop-agent/logs/cron.log 2>&1
0 9 * * * cd /path/to/agent && AIRDROP_HOME=$HOME/.airdrop-agent .venv/bin/aa daily run --all >> $HOME/.airdrop-agent/logs/cron.log 2>&1
```
