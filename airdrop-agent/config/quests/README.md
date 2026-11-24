# Example quests

Shipped, validated, tested against the live sites on 2026-11-19. Import, dry-run, then run with one
identity. Most park at a `manual` step on purpose — that is the honest boundary.

| file | flow | stops at |
|---|---|---|
| dow-points | connect + quiz | quiz answer |
| ammora-checkin | connect + daily | X verify / check-in |
| vapi-earn | earn wall | sign-in |
| qyro-manifest | connect + manifest | manifest tasks |
| syqora-register | form + create | email confirm |
| bullshot-gem | connect + mining | start mining |
| dogeos-heist | tutorial | heist tasks |
| kiedex-login | form + sign-in | Turnstile / code |
| nilly-faucet | connect + faucet | faucet tab |
| stabilizer-faucet | faucet | claim + swap |
| flint-perps | connect + perps | open/close |
| whisk-swap | connect + amount | confirm swap |
| ducat-vault | cookie + connect | deposit |
| polyester-trade | connect + trade | place trade |
| tzer0-mission | heavy page | run mission |
| folester-play | connect + play | play round |
| oripa-raffle | email entry | submit |
| codekitty-whitelist | terminal boot | intro + mint |
| zkunnamed-apply | apply form | fill + send |

```bash
aa quest import config/quests/oripa-raffle.yaml
aa quest run oripa-raffle --identity main --dry
aa quest run oripa-raffle --identity main
```

Report and caveats: [../../docs/TARGETS.md](../../docs/TARGETS.md).
