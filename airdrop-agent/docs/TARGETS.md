# Target test report

Tested 2026-11-19 from a macOS arm64 host, Chromium 1248, one fresh identity, providers injected.
Every URL was fetched with curl and then opened in the real engine. `HTTP` is the curl status;
`CF` is what the browser saw; `stop` is where the shipped quest intentionally parks.

All nineteen responded `200` with no interstitial in the HTML. No site produced a managed
Cloudflare challenge in this run. Kiedex carries a hidden Turnstile field on login; Tzero's bundle
is heavy and its first DOM load can exceed 90 s.

| target | HTTP | CF | wallet | form surface | quest | stop |
|---|---|---|---|---|---|---|
| app.dowprotocol.com/points | 200 | none | injected | none, quiz buttons | `dow-points` | quiz answer |
| ammora.org/waitlist | 200 | none | injected | code input, check-in | `ammora-checkin` | X verify / check-in |
| api.vapinetwork.ai/earn/quests | 200 | none | injected | sign-in wall | `vapi-earn` | sign-in choice |
| qyrolabs.space | 200 | none | injected | connect + manifest | `qyro-manifest` | manifest tasks |
| airdrop.syqora.app/register | 200 | none | injected | name, email, password | `syqora-register` | email confirm |
| gem.bullshot.io | 200 | none | injected | connect, mining | `bullshot-gem` | start mining |
| kiedex.app/login | 200 | turnstile field | injected | email, password | `kiedex-login` | Turnstile / code |
| quest.dogeos.com | 200 | none | injected | tutorial panels | `dogeos-heist` | tutorial + daily |
| testnet.nilly.fun/#/faucet | 200 | none | injected | connect gate | `nilly-faucet` | faucet tab |
| app.stabilizer.finance/swap | 200 | none | injected | amount, faucet tab | `stabilizer-faucet` | claim + swap |
| devnet.app.flint.trade/perps | 200 | none | injected | trade panel | `flint-perps` | open/close position |
| whisk.exchange | 200 | none | injected | amount, token select | `whisk-swap` | confirm swap |
| app.ducatprotocol.com | 200 | none | injected | cookie wall, vault | `ducat-vault` | deposit |
| testnet.polyester.com | 200 | none | injected | markets / trade | `polyester-trade` | place trade |
| testnet.tzer0.fun | 200 | none | injected | mission UI (slow) | `tzer0-mission` | run mission |
| folester.xyz/play | 200 | none | injected | connect, play | `folester-play` | play round |
| oripa.xyz | 200 | none | injected | email | `oripa-raffle` | submit entry |
| codekitty.xyz | 200 | none | injected | terminal boot | `codekitty-whitelist` | intro + mint |
| zkunnamed.xyz/apply | 200 | none | injected | wallet, X post, handles | `zkunnamed-apply` | fill + send |

## What was actually exercised

- `oripa-raffle`: goto, dismiss, email fill from the identity, CTA click. The `get in first` button
  text won't match with a strict selector; the quest uses substring matching and parks.
- `zkunnamed-apply`: APPLY opens the form (hidden inputs become visible), submit park. The form is
  React-controlled, so `form: {}` fills what it can and the rest is manual.
- `dow-points`: `wallet_requestPermissions` produced a connect gate on a non-allowlisted domain,
  code approved, rerun connected through the injected provider and reached the quiz.
- `kiedex-login`: email/password filled; the hidden `cf-turnstile-response` input appears only when
  the widget renders, so the quest detects the widget and parks for a token or a human.
- `ammora-checkin`, `nilly-faucet`, `stabilizer-faucet`, `whisk-swap`, `ducat-vault`: connect and
  page steps verified; the value-bearing action is manual because a deposit size is judgement.

## Notes and caveats

- Tzero: first navigation can time out at 90 s on a cold profile. The quest is `manual` after the
  goto and the browser timeout is 60 s by default (`BROWSER_NAV_TIMEOUT_MS`).
- Code Kitty opens with a terminal boot animation; `click: "Tap to continue"` is required before
  anything is interactive.
- Nilly shows its faucet link only after a wallet is connected; the quest connects first for that
  reason.
- Whisk and Stabilizer pin `sepolia` / `base-sepolia`. Faucet availability decides whether the
  manual action can be completed.
- None of these sites required a provider token in this run. The Turnstile path is still wired and
  tested on the detector and injection layer.

Re-probe a target before trusting an old quest:

```bash
aa quest probe https://app.example.com/tasks --identity main
```
