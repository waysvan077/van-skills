# Cloudflare and Turnstile

Cloudflare is a session problem disguised as a captcha problem. A challenge solved a minute ago is
usually solved already; hammering it again is what gets an IP flagged. The layer therefore tries
reuse first, interaction second, a solver third, and you last.

## What runs when navigation sees a challenge

```
inject cached clearance (host + UA, TTL)
        │ miss
        ▼
28 s of human behaviour: mouse, scroll, focus, Turnstile click
        │ still challenged
        ▼
provider token (2captcha / anticaptcha) with action + cdata, injected into every frame
        │ no key or failure
        ▼
FlareSolverr request.get, cookies imported
        │ nothing configured
        ▼
screenshot + "blocked" + aa cf assist
```

`goto` never throws for a challenge; it returns `cf: "blocked"` plus a screenshot and a `next`
command. Quest steps that need the page record the block and park.

## Why cache instead of solve every time

`cf_clearance` is bound to the IP and user agent that earned it. Storing it with the UA lets the
next run skip the widget entirely. The cache is per identity and per host in the vault, defaults to
45 minutes (`CF_TTL_MINUTES`), and is dropped when the UA changes. Change proxy → clearance dies,
so keep one residential proxy per identity.

## Commands

```bash
aa cf status                         # cached hosts, age
aa cf assist --identity main --url https://app.example.com/points
aa cf clear --identity main --host app.example.com
```

`assist` opens one visible window on the identity's own profile. Click once, then wait: the cookie
is captured and reused immediately. `CF_ASSIST_SECONDS` (300) is the patience budget.

## Configuration

| key | default | meaning |
|---|---|---|
| `CF_ENABLED` | 1 | master switch |
| `CF_WAIT_SECONDS` | 28 | natural interaction budget |
| `CF_TTL_MINUTES` | 45 | clearance cache life |
| `CF_ASSIST_SECONDS` | 300 | assist window |
| `CAPTCHA_PROVIDER` | — | `2captcha` or `anticaptcha` |
| `CAPTCHA_API_KEY` | — | provider key |
| `FLARESOLVERR_URL` | — | e.g. `http://127.0.0.1:8191` |

Provider tokens cost roughly a cent. Turnstile needs `action` and `cdata` for a good score; the
layer reads `data-action` and `data-cdata` from the widget when present.

## Server / headless reality

Headless Chromium is the weakest signal. On a VPS:

1. `BROWSER_HEADLESS=1`
2. set `FLARESOLVERR_URL` and a provider key
3. keep a residential proxy, one per identity
4. run `cf assist` once from a desktop session and copy the vault if you can

## What it does not do

- It does not rotate dozens of free proxies against one host; that burns clearance constantly.
- It does not solve interactive image grids without a provider.
- It cannot reuse clearance across a changed user agent or IP.
