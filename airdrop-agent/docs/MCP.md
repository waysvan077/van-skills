# MCP server

`aa mcp serve` speaks JSON-RPC 2.0 over stdio. Protocol frames on stdout, logs on stderr.

## Methods

`initialize`, `notifications/initialized`, `ping`, `tools/list`, `tools/call`.

## Tools

21 operations, all prefixed `aa_`: status, identity/wallet create/list/balance, tx, sign, telegram join,
x follow, discord grind, email otp, quest list/run, radar scan, autopilot tick, gate list/approve,
daily run, session status/warm, cf status.

## Errors

Tool failures come back as `result.isError = true` with the message; the transport stays alive.
Policy denials and gate prompts are not protocol errors — that's how a client distinguishes "ask the
human" from "server broke".

## Security

- No network listener; stdio only.
- Same policy/gate/KILL path as the CLI.
- `AIRDROP_HOME` must be passed in the client env, or the server uses `~/.airdrop-agent`.
- Tools never return private keys.

## Test by hand

```bash
printf '%s\n' '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{}}' \
  '{"jsonrpc":"2.0","id":2,"method":"tools/list"}' | aa mcp serve
```

## Not exposed

Browser-only flows (manual login, headful inspection), vault export/import, mode switches and
`kill/unkill`. Use the CLI for those; they are rare and benefit from a terminal.
