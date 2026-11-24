# Safety model

## Assets in scope

Agent wallets (burners), social sessions, email inboxes, and the vault passphrase. The operator's
main seed is never imported by default.

## Vault

- AES-256-GCM, scrypt KDF, per-entry random nonce, AAD = `kind/name`.
- Renaming or moving a blob between kinds fails authentication (no silent decrypt of the wrong key).
- `VAULT_PASSWORD` lives in `.env` (0600) or `VAULT_PASSWORD_FILE`. Lose it and the keys are gone —
  export a bundle and store it offline.
- Export is ciphertext only; import writes atomically.

## Approval gates

- 6-char code, hash-bound to the payload, TTL 12h, single use.
- Delivered only to the operator's Telegram bot. The page cannot choose the code.
- A gate covers: chain, recipient, value, calldata, selector, decoded call, balance delta, domain.
- Unlimited approvals always gate, in every mode.
- Permit/Permit2/Seaport orders gate unless turbo explicitly sets `allow_permits`.

## Hard blocks (no mode overrides)

| block | why |
|---|---|
| KILL file | operator stop |
| `eth_sign` blind hash | unreadable payload |
| unlimited approval | drainer pattern |
| blocklisted address | known bad |
| scam verdict `bad` | drainer domain |
| value over per-tx cap | runaway amount |
| daily cap | runaway day |
| contract not allowlisted on mainnet | unknown target |

## Spend caps

Per chain: `max_tx_native`, `max_daily_native`, `max_gas_native`, in native units, converted with the
chain's `native_decimals` (Solana = 9, not 18). Gas is checked against the gas cap before signing.
Turbo widens caps per chain but never removes the hard blocks.

## Scam heuristics

`scam_check` inspects scheme, registrable domain, punycode, suspicious TLD, hyphen count, brand
embedding/typosquat (Levenshtein 1), drain-keywords in page text, and optional RDAP age. Verdict:
`bad` never auto-connects, `warn` gates, `ok` may auto-connect only when the domain is allowlisted
(or auto-allowlist is on in turbo).

## Redaction

Every envelope line passes `log.redact`: 64-hex keys, Telegram bot tokens, Discord/X/Telethon tokens,
proxies, mnemonics, and literal values of sensitive env vars. Vault paths and addresses stay visible
on purpose.

## Operational

- Dedicated social accounts, never the main identity.
- Dedicated email inbox; the agent reads its own IMAP, not yours.
- One residential proxy per identity in `BROWSER_PROXY` for anything funded.
- Autopilot `chat` and `campaign` off by default: both cost (ToS, gas).
- `aa doctor` before mainnet; `aa mode safe` while auditing a new site.
