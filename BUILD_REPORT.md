# BUILD_REPORT — tiny-chess-01 v2

0x88 negamax + alpha-beta, iterative deepening, fixed 80,000-node
budget, TT + killers + history, NMP (R=2) + LMR, quiescence with
check evasions. Eval: material + PSTs + bishop pair + king MG/EG
+ incremental pawn structure (doubled/isolated/passed) + king
pawn-shield + tempo. Deterministic. Packed with
kit `makefun compress`: source `engine.py` (8,471 B, uncommitted)
-> `main.py` (3,871 B).

## A/B (book openings, alternating colours)

eval (pawn/shield/tempo) vs v1, 5k, 16g: 10.5, +112, KEEP.
NMP vs v1, 5k, 16g: 6.0, -89; LMR vs v1, 5k, 16g: 5.0, -137.
NMP+LMR+eval vs eval-only, 15k, 12g: 8.0, +120, KEEP.
History vs NMP+LMR+eval, 15k, 12g: 6.5, +29, KEEP.
Rook-file bonus, 5k, 16g: 7.5, -22, DROP.
Check extensions vs finalist, 15k, 12g: 7.5, +89 — but trials
at 80k came out WORSE (mean 1394 vs 1432), so DROPPED. Pruning
hurts at 5k nodes, helps at 15k+.

## Gates (final main.py)

Perft: 6 positions, depths 1-4, exact counts. Eval/hash/pawn
consistency: pass. Mate-in-1: a1a8. Determinism: identical moves
across repeated and subprocess/in-process runs.

## Trials (subprocess, 80k)

0: Elo 1364, 3.5/6, wall 862s, avg 3.35s, max 7.30s.
1: Elo 1654, 5.0/6, wall 835s, avg 2.55s, max 3.51s.
2: Elo 1279, 3.0/6, wall 853s, avg 2.60s, max 3.99s.
Mean 1432 (v1: 1160). No timeouts; walls well under 1800s.

## Budget

100k timed out once (trial 1, 10.011s) with 2.2x wall variance
for identical positions on shared CPU; cut to 80k. One trial-2
run at 80k was discarded (4 time losses under heavy host load,
load avg 8; clean re-run used).

## Assessment

Mean 1432 beats the house baseline mean (~1365) but does NOT
clear the ~1450 gate with confidence and misses the 1500 target;
trials 0 and 2 fall below 1450. No rank-1 claim. Large gain over
v1 (+272), competitive, but prize contention is not assured.
