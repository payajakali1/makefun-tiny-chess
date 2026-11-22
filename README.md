[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/payajakali1/makefun-tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/payajakali1/makefun-tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-01 — a chess engine in 4 KB of Python

An original chess engine by **payaja**, written for the make.fun
`tiny-chess-01` bounty: the strongest engine that fits in a single
`main.py` of at most 4,096 bytes.

## What it is

- `main.py` is the whole engine. It unpacks and runs an LZMA-packed
  payload (the packing the rules expressly allow; the payload and the
  unpacker together are 4,058 bytes).
- The engine inside: a 0x88 board with full legal move generation
  (castling, en passant, promotions), a negamax search with alpha-beta
  pruning, a transposition table, null-move pruning and a quiescence
  search over captures, and a material + piece-square evaluation with a
  bishop-pair bonus. Repetitions found in the search, and positions
  already seen in the game, score as draws.
- It searches to a fixed budget of 80,000 nodes per move and never reads
  a clock, so the same position always gets the same move and a replayed
  trial comes out move for move identical.
- It plays the house gauntlet through the env at `MAKEFUN_ENV_URL`:
  `POST /game` serves the game in play, `POST /move` plays a move, and
  the run ends when the env answers `{"status": "done", ...}`.

## Layout

- `main.py` — the engine (packed; see above)
- `LICENSE` — MIT
- `.makefun/owner.json` — the entrant claim file

## Running it

The engine needs only Python 3.13's standard library and the env:

    MAKEFUN_ENV_URL=http://store:8080 python3 -P -s main.py
