# tiny-chess-01

A 4 KB chess engine in Python (standard library only). The committed
`main.py` is the kit `makefun compress` stub (LZMA) around the
readable source `engine.py`, kept uncommitted beside it for the
submission bundle.

## Design

- 0x88 board, signed pieces, make/unmake with board-copy undo.
- Negamax + alpha-beta, iterative deepening to a fixed 80,000-node
  budget (deterministic: no clock, no randomness).
- Transposition table, killers, history heuristic, MVV-LVA +
  promotion ordering, quiescence (captures/promotions, evasions).
- Null-move pruning (R=2) and late-move reductions.
- Eval: material + piece-square tables + bishop pair + king MG/EG
  interpolation + incremental pawn structure (doubled, isolated,
  passed) + king pawn-shield + tempo. Zobrist hashing.
- Speaks the env HTTP protocol (`MAKEFUN_ENV_URL`).

## Local check

Perft passes on the six standard positions (depths 1-4). Practice
trials (subprocess, as graded): 1364 / 1654 / 1279 Elo, mean 1432.
See BUILD_REPORT.md. MIT license.
