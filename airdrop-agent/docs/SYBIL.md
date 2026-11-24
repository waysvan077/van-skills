# Sybil, proxy and ToS reality

No one can promise an airdrop won't filter you. This is the honest position.

## What projects usually cluster on

- funding source (same CEX withdrawal wallet)
- first/last activity time
- identical calldata or amounts across wallets
- shared RPC/IP or browser fingerprint
- social graph (same follow order, same join order)
- email domain / phone prefix patterns
- wallet age and activity shape

## What this agent does about it

- One identity = one persona + one proxy + one email + its own HD branch.
- Amounts randomised inside a range (`campaign` steps use `[min, max]`).
- Timing jittered (`humanize`, rate windows, active hours).
- Social actions have per-identity budgets so all accounts don't move in lockstep.
- Proxy applied per browser profile, not globally.
- Chat text is generated per persona with dedupe against that identity's own history.

## What still leaks

- The same operator funds many wallets at once. Fund in batches, hours apart, from different sources.
- Browser and Chromium build are identical across identities. Fingerprint shim is minimal on purpose.
- Telegram account creation is manual; accounts created the same day still cluster.
- Discord user-token traffic looks like a self-bot if it's too regular. `dc grind` is paced, but it is
  still a self-bot.

## Practical rules

1. Separate identities for separate projects. Never reuse a wallet across unrelated farms.
2. Fund from distinct sources; avoid one CEX withdrawal address.
3. Let a new identity age: a few days of light activity before a funded quest.
4. Don't scale past what you can fund and track. Ten funded identities beat fifty empty wallets.
5. Keep one identity clean of obvious sybil markers if a project matters a lot.

## ToS positions

| platform | method | risk |
|---|---|---|
| Telegram | official MTProto user session | low |
| X | browser login + actions | low-medium (automation rules) |
| Discord | user token (self-bot) | medium — explicitly grey |
| Quest platforms | browser automation | low-medium, captcha is the tell |
| Faucets | browser automation | low |
| Testnet tx | normal wallets | none |

Read a project's terms before a big push. If a rule isn't written down, assume it isn't banned but
also isn't guaranteed. That is the grey zone; keep the bankroll small relative to the bet.

## Exit discipline

Airdrops are a lottery with effort. Set a per-project gas budget, stop when it's hit, and don't chase
sunk cost. The radar score exists to help you say no.
