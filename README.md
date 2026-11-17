# Token Radar

[![token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/competitions/token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/mariyam002/token-radar-01/rank.svg)](https://make.fun/competitions/token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/mariyam002/token-radar-01/reward.svg)](https://make.fun/competitions/token-radar-01)

A token launch radar for the [make.fun token-radar-01](https://make.fun/competitions/token-radar-01) bounty. It reads a sealed DefiLlama snapshot of every protocol, finds the ones with no token yet, ranks them by what their launch would be worth, and hands you a dashboard with the board, a detail view per protocol, and the steps to farm it.

## Run it

Node 22, standard library only, no network:

```sh
MAKEFUN_OUTPUT_DIR=out node main.ts --input /path/to/snapshot.json
```

That writes `out/board.json` (178 ranked rows), `out/data.js`, copies `dashboard/` and `screenshots/` to `out/`, and writes `out/dossier.json` with `buildDossier` from `vendor/makefun-board.ts`.

## How the ranking works

`implied = TVL × categoryMultiple × 0.50 (launch discount) × growthMult × ageMult`

- `categoryMultiple` is the median market-cap/TVL of token-bearing peers in the same category (the global median 0.223 when a category has fewer than 3 peers), clamped to [0.05, 1.50].
- `growthMult` follows the 7-day TVL change, clamped to [0.70, 1.60].
- `ageMult` rewards protocols that have been listed longer, in [0.75, 1.00].

A protocol counts as pre-token when it records no token (`gecko_id`, `symbol` and `mcap_usd` all null), its category is not a bridge, liquid-staking/restaking or CEX product, and its name carries no wrapped/staked marker. The board covers the whole 40-protocol watchlist plus 138 further pre-token protocols below the watchlist floor — 178 rows, total implied value $1.61B. Top of the board: Steakhouse Financial ($283.9M), Gauntlet ($169.1M), Veda ($161.3M).

## Layout

- `main.ts` — the program (Node 22, standard library only)
- `lib/model.ts` — the ranking model
- `dashboard/` — the static dashboard; renders with no network
- `screenshots/` — the board, a protocol detail view, and a filtered view
- `vendor/makefun-board.ts` — the kit's dossier builder, byte for byte

## License

MIT
