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

Spawn at orientation 0 with no inherited movement/grounding state. If any visible spawn cell is occupied, game over occurs before accepting another play command. Rotation tests ordered SRS candidates from R-ROTATE, retaining the original state when every candidate collides. A successful move translates one column or row; rejected movement never partly changes the piece.

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

The page starts one playing session. A blocked spawn or invalid above-board lock ends it; movement, rotation and elapsed play cannot change a game-over board or score. Restart clears board, score, game-over state and all per-game state, creates a fresh source sequence and spawns one piece. Repeated restart does not multiply listeners/loops. No play-state persistence is required. Source randomness is injected for deterministic tests; production draws from the 7-bag source in R-BAG.

## R-VIEW / R-QUALITY — Presentation and validation

Show settled and active cells, score, keyboard instructions, Restart and readable playing/suspended/game-over status. Regular pieces never use white; white text/grid/status is permitted. Provide visible boundaries and distinguishable cell outlines. Canvas and HTML status must represent the same committed snapshot. HTML controls have labels and work with keyboard focus. At desktop viewport 1024x768 the board, status and required controls are usable without overlap; smaller windows may scroll. Render failures are readable outside Canvas.

Deliver static browser assets without a backend. No account, network leaderboard, multiplayer, touch or audio is required. Validate the domain with injected source/time and the actual browser path with real keys, focus changes, restart and screenshots. Chromium primary and Playwright-managed Firefox secondary must satisfy applicable interaction/rendering checks under [BROWSER_DEV_POLICY](../../BROWSER_DEV_POLICY.md). That policy owns isolated provisioning, proactive diagnostics and recovery; an unavailable required browser is a verification gap, not an automatic target waiver.

## Core acceptance

- AC-CORE-1: Given known spawn cells and board fixtures, movements/rotations at walls, floor, ceiling and occupied cells either preserve a valid placement or leave state unchanged.
- AC-CORE-2: At 999 active ms no gravity step occurs; at 1000 ms exactly one due step occurs. Equivalent elapsed-time partitions produce equivalent state. Focus suspension adds no active time.
- AC-CORE-3: Fixtures clearing 0..4 rows produce the exact compacted grid and score increments; survivors preserve order and color.
- AC-CORE-4: Blocked spawn and above-board lock end play without partial writes. Restart restores the defined clean state and retains one loop/listener set.
- AC-CORE-5: Both target browsers support the complete keyboard-to-lock-to-clear-to-game-over-to-restart path and matching visible/status evidence. Static build loads without a server-side application.

## Enhanced play

### R-BAG / R-HOLD — Source queue and hold

Each bag is a uniformly shuffled permutation of all seven kinds; concatenate independently shuffled bags. Maintain one queued next kind without consuming it on display. Spawn and empty hold each consume exactly one queued kind. Restart clears bag/queue and starts a fresh bag. An injected sequence replaces random draws in tests only.

C holds the active kind or exchanges it with the stored kind at normal orientation/origin. At most one hold succeeds per piece cycle; holding spawns a replacement but does not itself re-enable hold. A successful lock re-enables it. An unavailable or repeated hold leaves source, board and stored kind unchanged; blocked replacement spawn ends play after the exchange is accounted for. Held/next kinds contain no previous rotation, position or clock. Show the stored kind and availability in HTML/Canvas status. Next-preview visibility follows R-CONFIG.

### R-ROTATE — Clockwise SRS candidates

Use local transforms in R-PIECE; offsets below are tried from left to right after transforming. Coordinates are x right, y down. O has only (0,0), retaining its geometry. For each other kind, use its named offset table; first legal candidate wins. At spawn margin, preserve the ceiling bound. Failed rotation leaves position, orientation and clock unchanged.

| Transition | J/L/S/T/Z offsets | I offsets |
| --- | --- | --- |
| 0→R | (0,0), (-1,0), (-1,-1), (0,2), (-1,2) | (0,0), (-2,0), (1,0), (-2,1), (1,-2) |
| R→2 | (0,0), (1,0), (1,1), (0,-2), (1,-2) | (0,0), (-1,0), (2,0), (-1,-2), (2,1) |
| 2→L | (0,0), (1,0), (1,-1), (0,2), (1,2) | (0,0), (2,0), (-1,0), (2,-1), (-1,2) |
| L→0 | (0,0), (-1,0), (-1,1), (0,-2), (-1,-2) | (0,0), (1,0), (-2,0), (1,2), (-2,-1) |

Counterclockwise/180-degree commands are not required. The tables are the canonical project offsets; tests use their declared screen-coordinate convention.

### R-LANDING / R-DROP / R-GHOST — Shared projection

For an ordinary piece, landing is the last strict valid position reached by downward steps before the first blocking row. Space remains rotation; D, ignoring key repeat, performs hard drop to that landing without modifying settled cells. If no fully visible valid landing exists, hard drop results in top-out rather than locking invisible cells. A visible ghost is a translucent/outlined footprint of the exact projected destination, drawn behind the active piece without mutation or source consumption. Its color/opacity is distinct from settled cells.

### R-DELAY — Full-interval default lock

Default general locking is delayed: once validly grounded, accumulate G=1000 ms from that grounding event, independently of gravity cadence; lock at exactly G unless support is lost first. Movement/rotation that remains grounded preserves elapsed grounding time. Becoming airborne clears it; a later valid grounding starts a fresh interval. Rejected commands and repeated hard drop do not refresh the timer. A failed downward attempt while grounded does not lock early under this policy. Hard drop onto a new landing starts the full interval; a hard drop that leaves an already grounded position uses the remaining interval. New spawn/hold replacement resets both clocks. Time suspension freezes both clocks. If an input event has the same logical timestamp as a due timer expiry, process the due expiry before that input, so a locked piece cannot be rescued retroactively.

The failed-down rule in R-LOCK remains the alternative general policy. A hard drop under that policy follows ordinary movement semantics: arriving at the landing does not lock until a subsequent failed down attempt. Locking-policy selection follows R-CONFIG. Under the default rule, hard drop never bypasses the delay.

### Enhanced acceptance

- AC-MODERN-1: Every consecutive bag-aligned group of seven contains each kind exactly once; held exchanges consume the queue only when empty, reset orientation/clocks and obey one-hold-per-cycle.
- AC-MODERN-2: Fixtures for all four clockwise transitions of both offset families choose the first legal candidate; fully blocked rotation preserves the original piece.
- AC-MODERN-3: Ghost cells equal hard-drop destination for the same snapshot, including walls, cavities and top-out; reading projection never mutates state.
- AC-MODERN-4: At 999 ms of grounding no lock, at 1000 ms lock. Grounded moves/failed down/repeated drop preserve the deadline; leaving support and regrounding resets as specified. Equivalent active-time partitions and focus suspension preserve outcomes.

## R-VISIBILITY — Dynamic ghost visibility

The dynamic mode shows a ghost only when every projected cell has y>=10. A footprint with any cell at y<10 is hidden, including one spanning rows 9 and 10. Row 10 belongs to the lower half. Evaluate the projected landing, never the active piece's current height or footprint center, after every move/rotation/hold/lock/spawn/board change. A missing/illegal fully visible landing has no ghost. Visibility changes no gameplay geometry or timing.

AC-VISIBILITY: Assert entirely upper, entirely lower, exactly row-10 boundary and straddling footprints. Gameplay that changes landing from lower to upper and back hides and restores the correct footprint in both browsers.

## R-PENETRATOR — Activation, transit and final placement

P, ignoring repeat, activates the current regular piece only while playing, when a bonus exists and a legal fully visible landing exists at or below its current origin. Spend exactly one bonus on success. Invalid/repeated activation is a no-op with no cost. Mode cannot be toggled off, lasts until successful lock and is cleared before the next spawn. Activation recomputes grounding under transit semantics: clear an existing regular grounding timer if a deeper legal landing becomes reachable; otherwise retain elapsed grounded time. Activation cannot refresh a still-grounded deadline. Hold is disabled during active penetration without consuming hold availability or changing the queue.

A penetrator may overlap occupied cells during transit but never changes them. Walls, floor and spawn-margin ceiling still constrain every cell. For a candidate origin, scan all downward origins through the last in-board footprint row: legal landings require every cell visible and unoccupied. Select the greatest legal y even when intervening rows overlap. Do not stop at the first blockage. Activation/move/rotation/downward step is accepted only if this set is nonempty at or below the candidate. For rotation use ordered R-ROTATE offsets with this transit predicate. These rules prevent a stranded invalid floor placement.

Grounding occurs only at a strictly valid placement from which the next downward candidate has no remaining legal landing. Overlap transit never starts or expires a lock. The same landing query feeds penetrator hard drop and ghost. A hard drop moves directly to the greatest legal y and applies the selected lock policy. A normal downward transit step may cross overlaps; if no lower legal landing remains, the current valid placement grounds. A lock always revalidates strict visible non-overlap before writing four cells; failed strict validation cannot partially change the board or consume another bonus.

## R-BONUS / R-PEN-VIEW — Bonus and appearance

Every simultaneous four-line clear awards one bonus after locking/clearing, up to three. This includes a penetrator's own clear. Activation spends before any later award. Start/restart has zero bonuses. Render the active penetrator, its outline and its visible projected footprint in white, with ghost opacity/pattern preserving distinction; settled cells retain their original kind color after lock. Regular active/ghost/settled pieces use non-white kind colors. The side panel displays exactly one accessible lightning symbol per bonus and an accessible numeric count; update in the same committed snapshot as spend/award. Show penetrator activation/hold-unavailable status.

AC-PENETRATOR: Use a board with an upper obstruction and a lower legal pocket to show ordinary landing above and penetrator landing below it; hard drop and ghost agree. Traverse overlap without erasing cells, reject candidates without any legal lower landing, cover wall/floor/ceiling and rotation cases, and assert no overlapping lock. Test zero/one/three-bonus activation, repeated activation, four-line awards at the cap, earning after spending, hold rejection without side effects, restart reset and white/non-white rendering in both browsers.

## R-CONFIG — Session settings and live application

Provide labelled HTML side-panel controls with these exact values and defaults. The view always reflects the selected value, including settings temporarily irrelevant to an active piece.

| Setting / key | Allowed values | Default |
| --- | --- | --- |
| ghost | hidden, dynamic, shown | dynamic |
| hardDrop | on, off | on |
| nextPreview | on, off | on |
| hardDropLock | follow, immediate | follow |
| lockPolicy | failed-down, interval | interval |
| hold | on, off | on |

Ghost hidden draws none; shown draws any valid landing regardless of midline; dynamic applies R-VISIBILITY. Disabling hard drop makes D a no-op; it does not hide the ghost. Disabling preview hides the queued-kind view without changing its source state. Next preview, when on, shows the exact next kind before any spawn/empty hold consumes it. Disabling hold makes C a no-op and preserves the stored kind; re-enabling restores eligibility subject to the per-cycle and penetrator rules.

Display and input toggles apply immediately. The two locking selections latch together on each new spawn, including hold replacement. Until then, show selected values plus a pending-policy indicator whenever they differ from the current piece's effective values. Changes cannot alter its deadline, unlock/relock it or force an immediate lock. With effective hardDropLock=immediate, an accepted enabled hard drop locks its valid destination in that same transition, including a penetrator; follow uses its effective general policy. When hard drop is disabled its lock choice remains selected for later use. All combinations are accepted.

Restart preserves selected settings and applies them to the new piece while clearing gameplay state. Reload resolves settings through R-PERSISTENCE. No local/session storage is used. Form edits do not trigger play shortcuts. Invalid internal setting commands leave the prior valid selection unchanged and surface a readable validation status; no unchecked raw value enters the domain.

AC-CONFIG: Check all six controls and defaults, both general policies combined with both hard-drop policies, immediate versus pending behavior during airborne/grounded/penetrator states, disabling/re-enabling hold and drop, preview without queue consumption, control focus, restart retention and R-PERSISTENCE reload resolution. Session controls themselves remain independent of browser persistence adapters.

## R-PERSISTENCE — Independent source resolution and writes

Before the first spawn resolve each R-CONFIG key independently. URL query names are exactly the setting keys in that table; cookie names are `tetris_v1_` plus the key. Valid values are the table's exact case-sensitive strings. One valid explicit URL occurrence wins over one valid managed cookie, which wins over the default. Missing, empty, invalid or duplicate occurrences of a URL key are not valid explicit input and fall through. Duplicate managed cookie names are invalid for that setting. Decode browser URL/cookie syntax once; malformed cookie decoding invalidates only that cookie. Unknown URL keys/cookies are ignored and preserved.

For every valid explicit URL value, attempt to save its corresponding cookie during hydration, including a value equal to its default and even when other settings are omitted/invalid. Omitted/invalid URL values never overwrite a valid saved cookie with a default. Hydration does not add query parameters or change the URL. Cookies never persist the board, score, held kind, bonuses, clocks or active mode.

On a user change, update the selected in-memory value, attempt that setting's cookie write and replace that setting's query value with one canonical occurrence via history.replaceState, preserving other pairs/order as supported by URLSearchParams, pathname and fragment. Do not reload or add history entries. Keep unrelated parameters including duplicates; canonicalize only the changed key. Perform both writes independently if either fails. Setting a control to its current value may perform the same idempotent save.

Cookie values use encodeURIComponent; attributes are Path equal to the containing application directory (with trailing slash), Max-Age=31536000 and SameSite=Lax, adding Secure on HTTPS. Do not set Domain. Read back managed-cookie values to detect blocked writes. Cookie/history access errors or failed readback leave valid in-memory play active, show a non-blocking save-status message and mark the affected source unsaved. A later successful change retries saving that key and clears its corresponding error; do not claim durable success from merely attempting a write. Unavailable cookie reads fall through to defaults for keys without valid URL input. Failed history writes leave the existing URL unchanged. URL limits/failures do not undo successful cookie writes. No server synchronization is required.

AC-PERSISTENCE: Cover the full precedence matrix independently for six keys; mixed URL/cookie/default sources; default-equal explicit URL seeding; omitted URL retention; malformed/empty/duplicate/unknown values; one invalid setting alongside five valid settings; URL fragment/unrelated query preservation; user edits followed by reload in the same cookie context; a clean context with defaults; separate path scoping; blocked cookie and history writes with continued play and truthful status. Browser tests use isolated contexts for both declared targets. A valid URL wins even when cookie saving fails.
