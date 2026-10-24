# Decomposition

## Component ownership

Components refine the directed blocks in [ARCHITECTURE](ARCHITECTURE.md). Interfaces below describe responsibilities; exact observable rules belong in SPEC. State has one owner and crosses the presentation boundary as snapshots.

| Component | Provides / owns | Requires and excludes | Verification seam |
| --- | --- | --- | --- |
| Geometry | Tetromino cell transforms, legal board placement, rotation candidate selection | Board occupancy and piece descriptor; no score or browser state | Exact cell coordinates, walls, ceiling, floor and occupied-cell boundaries |
| Board | Settled cell grid, atomic placement and simultaneous row removal | Strict legal placement; no active-piece movement or draw calls | Clear/compaction fixtures, immutable snapshots |
| Piece source | Kind generation and restartable injected randomness | Random-number source; no knowledge of rendering or key events | Deterministic draw sequences and reset |
| Game session | Active piece, board, score, status; command and elapsed-time transitions | Geometry, board and piece source; no global clock | Command replay, fake elapsed time, spawn/top-out and restart |
| Input adapter | Key-to-command translation and focus ownership | Focused game surface and restart button; no direct cell mutation | Native keyboard delivery, form-control exclusions |
| Loop controller | One owned requestAnimationFrame loop, focus suspension and disposal | Session commands and snapshot reader; no scoring decisions | Synthetic clock tests plus real browser lifecycle checks |
| Canvas renderer | Grid, settled and active cells, visible game-over indication | Read-only snapshot; no rules decisions | Screenshot/shape assertions with matching state |
| HTML view | Score, keyboard instructions, restart and status | Read-only snapshot and controller callbacks | Accessible names, tab order and state-to-text updates |
| Composition root | Construct/dispose one application and wire concrete dependencies | Concrete components; no duplicated gameplay rules | Repeated restart/mount lifecycle and static build smoke |

## Collaboration and lifecycle

Input commands enter Game session through Loop controller in browser event order. For a lock, Game session requests strict Board placement, clears complete rows, computes the score delta and requests the next kind. One final snapshot includes the resulting settled cells, active piece and status. Canvas and HTML consume that same snapshot. An illegal move returns no state change. Unexpected exceptions propagate to one controller failure boundary that stops input/time processing and reports a readable error.

Restart replaces session-owned state and resets the piece source while preserving the application wiring. Focus loss drops outstanding input and freezes the elapsed simulation clock; focus return establishes a fresh wall-time reference. Testing does not reach into private browser state to make the rules deterministic: source and elapsed time are explicit dependencies.

## Enhanced components and state

| Component | Responsibility and interface seam | Collaboration / verification |
| --- | --- | --- |
| Landing query | Return deepest reachable ordinary landing cells from active piece and settled board | Shared by ghost snapshot and hard-drop command; compare coordinates directly |
| Bag source | Produce successive shuffled permutations of seven kinds and maintain one queued successor | Piece source supplies spawn/hold draws; test two independent bags and seeded replay |
| Grounding clock | Track a piece's accumulated grounded time under the effective locking rule | Game session resets only on leaving support/new piece; fake-clock boundary checks |
| Hold state | Stored kind plus per-piece exchange eligibility | Session performs atomic exchange and blocked-spawn handling; renderer reads state |

Geometry owns clockwise SRS transforms and ordered kick tests; O remains unchanged. Session invokes landing before hard drop, applies the same grounding transition as any move, and does not let repeated drops refresh time. The renderer never creates a second queue or recomputes the held piece's spawn.

## Ghost visibility

A pure visibility selector accepts projected cells and board midline. Its only output is visible/hidden; geometry and clock state are unchanged. Canvas receives the approved footprint or no ghost. Unit fixtures cover upper/lower/straddling shapes and browser checks verify visibility follows projected position after play commands.
