# BUILD_REPORT — tiny-chess-01 v5

0x88 negamax + alpha-beta, iterative deepening, fixed
80,000-node budget, PVS, persistent TT (capped 300k),
killers + history, NMP (R=2) + LMR, quiescence with
check evasions, depth-1 node cap. Eval: material + PSTs
+ bishop pair + king MG/EG + pawn structure (doubled,
isolated, passed) + king pawn-shield + tempo.
Deterministic. Kit `makefun compress`: source
`engine.py` (8,649 B, uncommitted) -> `main.py` (3,906 B).

## A/B (10k nodes, 12 games, alternating colours)

PVS vs v2: 8.5 KEEP. LMR legal>2: 5.5 DROP. NMP R=3:
6.0 DROP. Aspiration: 6.5 DROP. Persistent TT: 8.0 KEEP.
Means: v2 1432, v3 (PVS) 1482, v5 (PVS+PTT) 1770.

## Gates

Perft: 6 positions, depths 1-4, exact. Tactics 5/5.
No illegal move in any trial (all env calls status 200).

## Trials (as graded, 80k nodes)

T0 clean: 1654, 5W/0D/1L, all checkmates, 1071 s.
T1: 2112 (cap), 6W/0D/0L, all checkmates, 901 s.
T2: 1545, 4W/1D/1L, 870 s. Clean mean 1770.
Sunfish mean 1365; target 1500. No timeout in clean
runs. First T0 run 1364 with 1 time loss from a host
CPU spike (house also slow) — discarded; polluted
mean 1674, still above target.

## Timing (T0 clean, proxy, 370 moves)

Mean 2.78 s, max 4.21 s, 0 moves over 5 s. A
capture-dense position once used 95k nodes in the
uncapped depth-1 QS; v5 caps every depth at the
budget and pre-seeds the best move.

## Determinism

T0 repeated twice clean: 6 games, 786 plies, move
for move identical. The polluted first run diverged
only after its timeout changed the persistent-TT
history — history dependence, not nondeterminism.
