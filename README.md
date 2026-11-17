# tiny-chess-01

A 4 KB chess engine in Python (standard library only). The committed
`main.py` is an LZMA + base85 payload with a small unpacker, as the
competition rules allow; the payload holds the full engine source.

## Design

- 0x88 board, signed pieces, make/unmake with board-copy undo.
- Negamax + alpha-beta, iterative deepening to a fixed node budget
  (deterministic: no clock, no randomness).
- Transposition table, two killers per ply, MVV-LVA + promotion
  ordering, quiescence (captures/promotions, full evasions in check).
- Evaluation: material + piece-square tables + bishop pair + king
  middlegame/endgame interpolation. Zobrist hashing for repetition.
- Speaks the env HTTP protocol (`MAKEFUN_ENV_URL`): plays the game
  served on `POST /game`, answers with legal UCI moves.

## Local check

Perft passes on the six standard positions (depths 1-4). Practice
trials measure roughly 1100-1280 Elo at a 30k-node budget, below the
house baseline; see the build notes in the parent session. MIT license.
