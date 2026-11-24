# Quest DSL

A quest is YAML. You declare steps once, mount selectors and values, and the runner executes them
with proof. `aa quest probe <url>` prints the fields, buttons and challenge state so you do not
write selectors blind.

```yaml
id: demo-daily
name: Demo daily
url: https://demo.example/quest
platform: custom           # galxe | zealy | layer3 | questn | intract | custom | generic
chain: sepolia             # optional, must exist in policy.yaml
cooldown_hours: 24
gate: false
daily: false
steps:
  - goto: "{{url}}"
  - dismiss: true
  - wallet: {connect: true}
  - form: {values: {email: "{{email}}"}}
  - click: "Daily check-in"
  - assert_text: "Checked in"
  - screenshot: checkin
  - checkin: demo-daily
```

## Placeholders

`{{url}}`, `{{email}}`, `{{handle}}`, `{{first}}`, `{{last}}`, `{{wallet}}`, `{{solwallet}}`,
`{{twitter}}`, `{{discord}}`, `{{telegram}}`, `{{bio}}`. Values come from the identity; missing
values stay literal.

## Actions

| key | value | note |
|---|---|---|
| `goto` | url | navigation, records scam verdict |
| `dismiss` | bool | cookie walls and modals |
| `wallet` | `{connect: true}` | clicks connect, falls back to `eth_requestAccounts` |
| `switch` | chain | chain is pinned per session; records the target |
| `fill` | `{selector, value}` | direct fill |
| `type` | `{selector, value}` | human-speed typing |
| `form` | `{values?, otp_sender?, otp_subject?}` | classifies fields, fills email/wallet/name/password, ticks boxes |
| `select` | `{selector, value}` | native select |
| `press` | key | keyboard |
| `scroll` | pixels | page scroll |
| `wait` | seconds | fixed pause |
| `sleep_rand` | `[min, max]` | jitter |
| `click` | text or selector | text first, case-insensitive, substring |
| `assert_text` | string | hard fail when absent |
| `if_text` | `{text, timeout?}` | records which branch was seen |
| `screenshot` | label | proof file |
| `eval` | JS | escape hatch, returns a value |
| `back` | bool | history back |
| `upload` | `{selector, path}` | file input |
| `api` | `{method, url, json?, headers?}` | non-browser call |
| `faucet` | `{name, chain?}` | catalog claim |
| `tx` | `{to, amount, data?, chain?}` | guarded pipeline, broadcasts |
| `deploy` | `{kind, args?}` | counter/erc20/erc721 |
| `sign` | `{message, label?, origin?}` | personal_sign through the bridge |
| `follow` / `join` | target | X handle or Telegram target |
| `like` / `retweet` / `post` / `react` | url or text | social actions |
| `checkin` | project | streak + daily idempotence |
| `manual` | instruction | parks the run |

## Hard vs soft

`goto`, `assert_text`, `tx`, `deploy`, `sign` abort after the second failure. Everything else is
soft: it records an error and continues. A `manual` step does not fail — it parks the run with a
screenshot so the next run can pick up.

## Login pre-flight

Steps that touch X, Telegram or Discord make the runner check the session first. Logged out →
`parked`, `needs_login`, and a login command in `next`. This happens before a browser step runs, so
you never get half a quest done on a dead cookie.

## Gates inside quests

A `wallet` step that reaches a gated connect (non-allowlisted domain, or safe mode) returns exit 10
with a gate id. Approve it, re-run the identical quest. `gate: true` forces the whole run behind a
gate.

## Outcomes

- all steps ok → `done`, proof written, `checkin` recorded, daily cooldown armed.
- manual/captcha/login → `parked`, screenshot kept, skipped by autopilot for six hours.
- hard failure twice → `failed` with the failing index and screenshot; quest becomes `blocked`.

## Adapters

`platform` biases connect and verify text and pre-navigates. It is a hint, not magic. If a site
changes, the parked screenshot is the diff.

## Maintenance

```bash
aa quest probe https://app.example.com/tasks --identity main
# patch the YAML
aa quest validate example-daily
aa quest run example-daily --identity main --dry
```
