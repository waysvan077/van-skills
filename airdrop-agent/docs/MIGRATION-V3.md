# Migration from v3

v3 was a pile of `scripts/*/*.py` with per-script policy. v4 is one package, one CLI, one policy file.
Old paths still exist as thin shims, but run the new commands.

## Mapping

| v3 | v4 |
|---|---|
| `python aa_agent.py --init` | `aa init` |
| `aa_doctor.py` | `aa doctor` |
| `wallet_create.py --label main` | `aa identity new main --wallets 3` |
| `wallet_list.py` | `aa wallet list --identity <id>` |
| `wallet_balance.py` | `aa wallet balance --identity <id>` |
| `send_tx.py --to .. --amount ..` | `aa wallet send --chain <c> --identity <id> --to .. --amount ..` |
| `sign_message.py --msg ..` | `aa sign message --identity <id> --message ..` |
| `telegram_login.py` | `aa tg login` + `aa tg verify` |
| `telegram_join.py --target ..` | `aa tg join @target --identity <id>` |
| `x_login.py` / `x_follow.py` | `aa x login` / `aa x follow` |
| `discord_login.py` / `discord_chat.py` | `aa dc login` / `aa dc grind` |
| `quest_run.py --file q.yaml` | `aa quest import q.yaml` + `aa quest run <id>` |
| `faucet_claim.py --chain ..` | `aa testnet claim --chain .. --identity <id>` |
| `deploy_contract.py` | `aa testnet deploy --kind counter` |
| `radar_scan.py` | `aa radar scan --identity <id>` |
| `autopilot.py` | `aa autopilot tick|run|daemon` |
| `daily_quest.py` | `aa daily run --identity <id>` |
| `state.sqlite3` | same schema name, new tables (auto-created) |

## Config changes

- `policy.yaml` is now strict: chains need `rpc_env`, `chain_id`, `native_decimals`, caps, `testnet`.
- `null` caps used to mean unlimited; now they are `0`. Set them explicitly.
- Gas caps are enforced pre-sign. Old scripts did not check gas at all.
- Domain allowlist is separate from contract allowlist.
- `VAULT_PASSWORD` is required (v3 defaulted to a weak file key).

## State

Same DB file, additive schema. Wallet rows are compatible; identities gained `notes/status`.
Old encrypted blobs are not compatible if the KDF changed — re-import keys, so keep the v3 `keys/`
directory until `aa wallet balance` matches.

## Regressions to know

- v3 happily sent with no simulation; v4 blocks on revert.
- v3 allowed unlimited approvals silently; v4 gates them.
- v3 treated `null` policy as allow; v4 treats it as deny.
- Turborequired manual edits in v3; now it is a mode, but hard blocks stay.
