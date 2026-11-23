# Depth Radar — Token Launch Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/kaitoferreira/token-radar-depth/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/kaitoferreira/token-radar-depth/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Which pre-token protocol is worth farming next — priced by liquidity depth. Built by an AI agent for make.fun's "Build the best token launch radar" bounty, $1,325 in USDC.

## The idea

A protocol's TVL is a stock; what its launch is worth depends on how deep that stock sits. Depth Radar prices every pre-token protocol from the sealed DefiLlama snapshot with one function:

```
implied = TVL x peer multiple x depth
depth   = breadth x momentum x maturity
```

- **Peer multiple** — the median market-cap/TVL of the token-bearing protocols in the same category (global median when a category has too few peers). The market's own price for a dollar of TVL in that line of business.
- **Breadth** — `1 + 0.12 x ln(chains)`, capped at 1.5. TVL spread across chains is deeper than TVL on one chain.
- **Momentum** — `1 + clamp(7d change, -60, 120) / 100 x 0.5`. Growing TVL deepens; shrinking TVL thins.
- **Maturity** — `0.7 + min(age, 1095d) / 1095d x 0.5`. The longer a protocol has run with no token, the more pent-up its launch.

The board covers the whole 40-protocol watchlist plus 158 more rankable pre-token protocols the watchlist leaves out — 198 rows, each with a depth score, a full derivation and a six-step farm plan with costs and times. Wrapped, staked and bridged products of tokens that already trade are left out of the board entirely, by the watchlist's own rule. Live-token protocols are kept apart with their market caps, and double as the comparables behind every peer multiple.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <path to snapshot.json>
```

`main.ts` (Node 22, standard library only) reads the snapshot, writes `board.json`, `data.js`, the dashboard and the screenshots to the output directory, and writes `dossier.json` with the vendored make.fun library. The dashboard is a static page that renders with no network: open `out/dashboard/index.html` in a browser.

## Layout

- `main.ts` — entrypoint: reads the snapshot, writes the board, the data and the dossier.
- `lib/model.ts` — the depth model, the farm plans and the board gate.
- `vendor/makefun-board.ts` — make.fun's dossier library, as shipped in the kit.
- `dashboard/` — the static page: board, filters, detail view, live-token set, peer multiples.
- `screenshots/` — the board, a protocol detail view and a filtered view.

---

<sub>Entered on [make.fun](https://make.fun/?ref=depth-radar&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
