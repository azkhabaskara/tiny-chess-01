# tiny-chess-01

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/c/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/azkhabaskara/tiny-chess-01/rank.svg)](https://make.fun/c/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/azkhabaskara/tiny-chess-01/reward.svg)](https://make.fun/c/tiny-chess-01)

A 4 KB chess engine in Python (stdlib only) for the make.fun
tiny-chess-01 competition. `main.py` is the kit `makefun compress`
stub (LZMA) around the readable source `engine.py`.

## Design

- 0x88 board, negamax + alpha-beta, iterative deepening to a fixed
  80,000-node budget (deterministic: no clock, no randomness).
- Principal variation search, transposition table (persistent across
  moves, capped), killers, history, MVV-LVA + promotion ordering.
- Null-move pruning (R=2), late-move reductions, quiescence
  (captures/promotions, check evasions). Depth-1 node cap.
- Eval: material + piece-square tables + bishop pair + king MG/EG
  interpolation + pawn structure (doubled, isolated, passed) +
  king pawn-shield + tempo. Zobrist hashing.

## Local check

Perft passes on six standard positions (depths 1-4).
Practice trials (subprocess, as graded): see
BUILD_REPORT.md. MIT license.
