# Architecture

## Direction

[PROJECT](PROJECT.md) defines a static, keyboard-operated browser game. TypeScript expresses the domain contracts. Canvas owns the playfield, while semantic HTML owns controls and readable status. No backend participates in play.

## Blocks and dependencies

| Block | Responsibility | Dependencies |
| --- | --- | --- |
| Domain | Board, piece geometry, collision, movement, rotation, locking, line clearing, score and game lifecycle | Injected piece source and elapsed simulation time; no DOM, Canvas, cookies or browser globals |
| Browser controller | Translate accepted input into ordered domain commands, own the animation lifecycle and publish snapshots | Domain and browser input/time adapters |
| Presentation | Draw immutable snapshots and update HTML score, instructions, restart and game-over status | Domain view data; never mutates board state |
| Composition | Create one game instance and wire controller, source and presentation | Concrete browser adapters and domain entry points |

Commands flow from controller to domain; snapshots flow from domain to presentation. Presentation cannot call geometry or edit state. The domain owns each transition atomically: lock, remove lines, update score and spawn. No rendering callback observes a partially cleared board. The controller owns exactly one animation loop and input subscription set, disposed together.

## State, timing and failure boundaries

The game session owns settled cells, active piece, score and playing/game-over status. Piece generation is injectable so tests can replay a known sequence without altering production behavior. Only elapsed active simulation time advances gravity; focus loss suspends time and clears held input, avoiding catch-up bursts. Rendering reads snapshots and does not change timing.

A blocked spawn or top-out is a domain outcome, displayed as game over. A rejected move is an unchanged game state, not an exception. Unexpected bootstrap or rendering failure stops the owned loop and presents readable failure status rather than continuing an invisible game. Restart creates clean session state without adding event listeners or animation loops.

## Tradeoffs and verification seams

A pure rules boundary avoids fragile browser-only testing of collision and lifecycle. Canvas keeps cell drawing compact; HTML preserves ordinary keyboard navigation for buttons and status. A UI framework and a generic plugin/event-bus system add unnecessary indirection for this product. Domain tests use injected sequences and time; browser checks exercise actual keys, focus, drawing and restart in the two declared targets under [BROWSER_DEV_POLICY](../../BROWSER_DEV_POLICY.md).

Logical component interfaces and exact behavior remain the responsibilities of the decomposition and specification documents respectively.

## Enhanced piece lifecycle

The domain includes one piece-source queue, hold state and a grounding clock. A single landing query supplies hard drop and ghost rendering; presentation receives only its result. Rotation tests ordered kick candidates in the geometry boundary. Hold exchanges piece kinds through session lifecycle ownership rather than moving objects between independent mutable stores. Grounding time belongs to the session transition, never the render loop. A 7-bag source owns bag order and draws through injected randomness; restart clears its queue.

## Visibility policy

Presentation selects whether the domain-provided landing footprint is visible using one pure visibility policy. It does not recompute landing or use the active piece's height as a substitute. The policy takes the complete projected cell set, so a shape crossing the midline has an unambiguous result.

## Penetration boundary

Movement rules distinguish transit feasibility from legal settled placement. The landing query scans every downward candidate using legal-placement checks; it does not stop at the first obstruction. Candidate transit remains bounded by the walls/floor and the availability of a legal landing at or below it. Locking always uses the strict legal-placement predicate. Session state alone owns activation and bonus spend/award order; hold checks that state before any exchange. Rendering chooses white from the active mode rather than modifying tetromino identity or settled-cell geometry.

## Configuration boundary

A typed configuration model owns six validated selected settings. Controller commands change settings, presentation exposes HTML controls, and domain snapshots distinguish selected locking policy from the current piece's latched policy. Input/display settings are read live; locking policy is latched at spawn. This prevents a control change from resetting or immediately expiring an existing timer. The configuration owner outlives a restarted game and is hydrated from validated browser sources before the first spawn.

## Persistence adapters

URL and cookie adapters implement browser I/O around the same typed configuration owner. A pure codec validates each source independently and resolves one setting at a time. Hydration applies valid explicit URL values to cookies but never fills omitted URL parameters with defaults. User edits update the in-memory setting first, then attempt that setting's cookie and history writes independently. Storage errors cannot corrupt the domain or prevent play. Browser adapters preserve unrelated URL data and expose save status; no server or third-party persistence service exists.
