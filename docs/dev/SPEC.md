# Specification

## Context and ownership

The single-player static browser game uses the blocks in [ARCHITECTURE](ARCHITECTURE.md) and components in [DECOMPOSITION](DECOMPOSITION.md). Game session owns atomic state transitions; Geometry and Board enforce placement; browser adapters supply input/time and render snapshots. [PROJECT](PROJECT.md) owns scope and non-goals. Requirement IDs below are stable behavioral groups, not implementation completion claims.

## Core play

### R-BOARD — Coordinates and settled cells

The visible board is 10 columns x 20 rows: x=0..9 left-to-right, y=0..19 top-to-bottom. A cell is empty or one settled tetromino color/kind. The board snapshot contains no active or ghost cells. Negative active-cell rows -4..-1 form a spawn margin; rows below -4, x outside 0..9 and y above 19 are out of bounds. Negative rows contain no settled cells. A legal active placement respects bounds and avoids occupied visible cells. Locking requires every cell in visible bounds and non-overlapping.

### R-PIECE — Shapes, spawning and rotation geometry

Seven kinds I, O, T, J, L, S and Z each have four distinct cells. The following local spawn cells use positive y downward. Spawn origin is (3,-1). J/L/S/Z/T rotate clockwise in a 3x3 frame: (x,y) becomes (2-y,x). I rotates in a 4x4 frame: (x,y) becomes (3-y,x). O does not change its cells or origin on rotation. Orientations cycle 0, R, 2, L.

| Kind | Local cells at orientation 0 |
| --- | --- |
| I | (0,1), (1,1), (2,1), (3,1) |
| O | (1,0), (2,0), (1,1), (2,1) |
| T | (1,0), (0,1), (1,1), (2,1) |
| J | (0,0), (0,1), (1,1), (2,1) |
| L | (2,0), (0,1), (1,1), (2,1) |
| S | (1,0), (2,0), (0,1), (1,1) |
| Z | (0,0), (1,0), (1,1), (2,1) |

Spawn at orientation 0 with no inherited movement/grounding state. If any visible spawn cell is occupied, game over occurs before accepting another play command. Rotation initially tests only the zero-offset target, retaining the original state on collision. A successful move translates one column or row; rejected movement never partly changes the piece.

### R-INPUT — Keyboard and focus

ArrowLeft/ArrowRight move by one cell; ArrowUp or Space rotates clockwise; ArrowDown performs one soft-down attempt; R or the named Restart button starts a fresh game. Browser-delivered repeat keydowns are accepted for horizontal movement and soft down; rotation and restart ignore repeat. No custom key-repeat system is required. Prevent page scrolling only for recognized play keys while the game surface owns focus. Text fields, buttons and form controls retain native behavior; the focused Restart button supports native Enter/Space activation. Conflicting simultaneous play commands are processed in received event order.

The game surface is keyboard focusable, visibly focused, and has an accessible name. A fresh game becomes playable without requiring a mouse. Focus loss/hidden page freezes simulation time, clears pending input and shows a suspended indicator; returning focus establishes a fresh time reference without a catch-up burst.

### R-TIME — Gravity and clock

Gravity interval G is fixed at 1000 ms of active simulation time; there is no speed-level progression. Every accumulated G requests one downward step. Process elapsed events chronologically; do not discard due ticks. Soft-down attempts do not reset the gravity cadence. New spawn resets its gravity accumulator. Browser frame rate must not change the resulting sequence for the same injected active-time intervals and command order. Render/UI failures stop owned processing and show readable failure status.

### R-LOCK — Grounding and locking

A regular piece is grounded when a strict downward step is illegal. Under failed-down locking, a gravity or soft-down attempt that cannot advance locks the current valid piece immediately. Merely moving or rotating onto support does not itself constitute a failed-down attempt. A lock with any cell above row 0 ends the game without inserting partial cells. Valid lock, line clearing, scoring and next spawn are one atomic transition.

### R-CLEAR / R-SCORE — Simultaneous lines and scoring

After a valid lock, remove every full row simultaneously, compact all surviving rows downward in original order and insert empty rows at the top. Score starts at 0; clearing 1/2/3/4 lines in one lock adds 100/300/500/800 respectively. No drop-distance points, time bonuses or combo multipliers apply. Clearing no line changes no score. Four lines are the maximum for one four-cell lock. Settled cells outside cleared rows retain their kinds/colors.

### R-LIFE — Start, loss and restart

The page starts one playing session. A blocked spawn or invalid above-board lock ends it; movement, rotation and elapsed play cannot change a game-over board or score. Restart clears board, score, game-over state and all per-game state, creates a fresh source sequence and spawns one piece. Repeated restart does not multiply listeners/loops. No play-state persistence is required. Source randomness is injected for deterministic tests; production baseline draws independent uniform kinds from the seven choices.

## R-VIEW / R-QUALITY — Presentation and validation

Show settled and active cells, score, keyboard instructions, Restart and readable playing/suspended/game-over status. Regular pieces never use white; white text/grid/status is permitted. Provide visible boundaries and distinguishable cell outlines. Canvas and HTML status must represent the same committed snapshot. HTML controls have labels and work with keyboard focus. At desktop viewport 1024x768 the board, status and required controls are usable without overlap; smaller windows may scroll. Render failures are readable outside Canvas.

Deliver static browser assets without a backend. No account, network leaderboard, multiplayer, touch or audio is required. Validate the domain with injected source/time and the actual browser path with real keys, focus changes, restart and screenshots. Chromium primary and Playwright-managed Firefox secondary must satisfy applicable interaction/rendering checks under [BROWSER_DEV_POLICY](../../BROWSER_DEV_POLICY.md). That policy owns isolated provisioning, proactive diagnostics and recovery; an unavailable required browser is a verification gap, not an automatic target waiver.

## Core acceptance

- AC-CORE-1: Given known spawn cells and board fixtures, movements/rotations at walls, floor, ceiling and occupied cells either preserve a valid placement or leave state unchanged.
- AC-CORE-2: At 999 active ms no gravity step occurs; at 1000 ms exactly one due step occurs. Equivalent elapsed-time partitions produce equivalent state. Focus suspension adds no active time.
- AC-CORE-3: Fixtures clearing 0..4 rows produce the exact compacted grid and score increments; survivors preserve order and color.
- AC-CORE-4: Blocked spawn and above-board lock end play without partial writes. Restart restores the defined clean state and retains one loop/listener set.
- AC-CORE-5: Both target browsers support the complete keyboard-to-lock-to-clear-to-game-over-to-restart path and matching visible/status evidence. Static build loads without a server-side application.
