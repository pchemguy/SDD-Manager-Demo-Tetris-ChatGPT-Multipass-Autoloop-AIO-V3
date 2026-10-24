# Implementation tasks

## Execution contract

This list derives bounded work from [PLAN](PLAN.md), [SPEC](SPEC.md), [DECOMPOSITION](DECOMPOSITION.md) and [layout](layout.md). All implementation work is unchecked. Each task owns its result, relevant tests, documentation and evidence; completion and the owning checkbox travel in one commit followed by verified publication. A review task performs both code review and tests, repairs blockers and commits its report. Required browser evidence follows [BROWSER_DEV_POLICY](../../BROWSER_DEV_POLICY.md).

Dependencies include the named task results and required publication/issue closure. A final phase review waits for delivery milestones only, excluding its own open milestone. Implementation phase projection/activation occurs separately at the eligible phase, not merely because this list exists. Complete verified phases integrate and publish before the next phase; partial requested ranges pause. Release packaging/version/notes preparation does not claim publication. After complete authorized phase integration, the coordinator creates and verifies the default-branch release commit/tag and publishes the release under the accepted release protocol.

## Phase 1 — Playable classic baseline

- [ ] Phase 1 — Playable classic baseline
    - [ ] Milestone 1.1 — Deterministic rules foundation
        - [ ] T-001 — Establish isolated TypeScript and browser verification tooling
            Scope: Root package/lockfile, TypeScript/Vite/Vitest/Playwright configuration and setup guide; physical owners are mapped in layout.md.
            Depends on: none. Work: Pin the build/test runner and configure project-owned browser/state paths. Exercise fresh isolated provisioning and repeated headless context/page/interaction/screenshot/teardown for Chromium and managed Firefox using central policy diagnostics; report actual versions and capability limits. No global browser changes.
            Evidence: Build/test entry points and a representative browser preflight run reproducibly; preserve evidence of each browser independently. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-002 — Implement board and tetromino geometry
            Scope: Domain geometry/board modules and focused tests; physical owners are mapped in layout.md.
            Depends on: T-001. Work: Define seven spawn footprints, clockwise transforms, immutable grid state and strict collision/placement including hidden spawn rows.
            Evidence: R-BOARD/R-PIECE fixtures cover each kind, boundary and occupied-cell rejection without partial state changes. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-003 — Implement deterministic falling-piece lifecycle
            Scope: Session and piece-source modules with fake-time tests; physical owners are mapped in layout.md.
            Depends on: T-002. Work: Independent uniform source, spawn/move/soft-down, fixed gravity, failed-down lock, loss and clean restart; inject time/source.
            Evidence: R-TIME/R-LOCK/R-LIFE replay verifies partitioned elapsed time, blocked spawn/top-out, atomic legal placement and restart resets. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-004 — Review, test and report milestone 1.1
            Depends on: T-001, T-002, T-003 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/1/1.1.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 1.2 — Playable browser session
        - [ ] T-005 — Compose Canvas playfield and readable session status
            Scope: Renderer, HTML view and application composition; physical owners are mapped in layout.md.
            Depends on: T-004. Work: Wire one session snapshot to board/active cells, score, controls, restart and game-over/suspended text.
            Evidence: Actual browser draw and HTML state agree; regular colors exclude white; mount owns one loop/listener set. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-006 — Wire keyboard, focus suspension and restart
            Scope: Input/loop adapters and integrated browser path; physical owners are mapped in layout.md.
            Depends on: T-005. Work: Translate native play keys, preserve form/button behavior, own focus and active-time suspension, implement restart without duplicate handlers.
            Evidence: R-INPUT/R-TIME/R-LIFE native key, blur/visibility and repeated restart checks establish predictable play. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-007 — Integrate simultaneous line clears and score
            Scope: Board/session score transition and HTML status; physical owners are mapped in layout.md.
            Depends on: T-006. Work: Clear completed rows atomically after lock, compact survivors, apply 0/100/300/500/800 deltas and spawn the next piece.
            Evidence: R-CLEAR/R-SCORE fixtures for 0..4 rows plus browser score/board agreement; game-over and restart remain correct. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-008 — Review, test and report milestone 1.2
            Depends on: T-005, T-006, T-007 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/1/1.2.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 1.3 — Verified static delivery
        - [ ] T-009 — Verify lifecycle and board integration invariants
            Scope: Cross-component domain integration tests and fixtures; physical owners are mapped in layout.md.
            Depends on: T-008. Work: Exercise reproducible command sequences spanning lock/clear/spawn, invalid commands and time partitions; repair failures and document rule boundaries.
            Evidence: AC-CORE-1..4 hold across integrated session/geometry/source seams with no out-of-bounds or overlapping settled state. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-010 — Verify the classic game in both browser targets
            Scope: Browser acceptance suite and owned screenshot artifacts; physical owners are mapped in layout.md.
            Depends on: T-009. Work: Use actual app server/build path for keyboard play, line clearing, score, game over, focus suspension, restart and 1024x768 layout.
            Evidence: AC-CORE-5 passes in Chromium and managed Firefox; inspect visible cell/status output, with no unexplained skip or substituted browser. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-011 — Prepare documented static build and version metadata
            Scope: Build artifact configuration, README, version file/package and release notes; physical owners are mapped in layout.md.
            Depends on: T-010. Work: Produce the static distribution, reproducible setup/play/test instructions and 0.1.0 release notes; distinguish checks actually run.
            Evidence: Clean build loads static assets; controls match documentation, version/notes agree and no server-side runtime is required. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-012 — Review, test and report milestone 1.3
            Depends on: T-009, T-010, T-011 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/1/1.3.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 1.4 — Phase review
        - [ ] T-013 — Review, test and report phase 1
            Depends on: Milestone 1.1, Milestone 1.2, Milestone 1.3 completion/closure.
            Scope: R-BOARD, R-PIECE, R-INPUT, R-TIME, R-LOCK (failed-down baseline), R-CLEAR, R-SCORE, R-LIFE, R-VIEW, R-QUALITY; integrated playable outcome and prior behavior.
            Evidence: independent code review, phase exits, required browser/visual/build/documentation checks and blocker repairs. Commit `docs/dev/reports/phases/1/PHASE-REPORT.md` and refresh HANDOFF with completion. Aggregate unresolved findings; when this is the final defined phase, also commit `docs/dev/reports/IMPLEMENTATION-REPORT.md` with final TODO provenance/options. Verify phase review issue/milestone closure before main integration and release preparation.

## Phase 2 — Modern gameplay

- [ ] Phase 2 — Modern gameplay
    - [ ] Milestone 2.1 — Bag source and hold
        - [ ] T-014 — Implement the 7-bag queue
            Scope: Piece-source module and source tests; physical owners are mapped in layout.md.
            Depends on: T-013. Work: Shuffle each bag independently with injected randomness; own one queued successor and restart reset.
            Evidence: AC-MODERN-1 bag permutations and draw counts hold across bag boundaries. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-015 — Implement one-exchange hold lifecycle
            Scope: Session hold state and lifecycle tests; physical owners are mapped in layout.md.
            Depends on: T-014. Work: Store/exchange kinds, reset orientation/clocks, consume next only for empty hold, reject second exchange and handle blocked spawn.
            Evidence: R-HOLD source/score/board invariants and eligibility reset are verified. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-016 — Expose hold input and view
            Scope: Keyboard adapter, hold view and browser checks; physical owners are mapped in layout.md.
            Depends on: T-015. Work: Wire C and readable held-kind/availability status without a second queue or private state.
            Evidence: Native hold and repeated rejection agree with source snapshots; restart clears hold. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-017 — Review, test and report milestone 2.1
            Depends on: T-014, T-015, T-016 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/2/2.1.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 2.2 — Rotation and shared landing
        - [ ] T-018 — Implement ordered clockwise wall kicks
            Scope: Geometry rotation and focused fixtures; physical owners are mapped in layout.md.
            Depends on: T-017. Work: Apply both R-ROTATE families and O geometry; retain original state if all candidates fail.
            Evidence: All four transitions per family choose the first legal candidate; floor/wall/stack/ceiling rejection verified. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-019 — Implement shared landing and delayed hard drop
            Scope: Landing query, session drop command and input D; physical owners are mapped in layout.md.
            Depends on: T-018. Work: Compute ordinary landing without mutation and move active geometry to it; preserve Space rotation and define top-out.
            Evidence: R-LANDING/R-DROP destination and no-partial-write fixtures pass; D repeat is ignored. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-020 — Render the projected ghost
            Scope: Snapshot and Canvas ghost view; physical owners are mapped in layout.md.
            Depends on: T-019. Work: Draw domain landing behind active geometry with distinguishable opacity/outline, without consuming source or editing board.
            Evidence: AC-MODERN-3 matches ghost coordinates to hard drop, including wall/cavity/top-out cases. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-021 — Review, test and report milestone 2.2
            Depends on: T-018, T-019, T-020 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/2/2.2.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 2.3 — Grounding clock and integrated modern delivery
        - [ ] T-022 — Implement full-interval grounding time
            Scope: Session grounding clock and fake-time tests; physical owners are mapped in layout.md.
            Depends on: T-021. Work: Make interval locking default; grounded moves/failed down preserve time, support loss clears it, repeated drop cannot extend it.
            Evidence: AC-MODERN-4 covers 999/1000 ms, regrounding, equal-timestamp expiry-before-input and drop behavior. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-023 — Integrate hold, kicks and focus with grounding
            Scope: Session/controller interaction tests; physical owners are mapped in layout.md.
            Depends on: T-022. Work: Reset new-piece clocks on hold/spawn, retain grounded deadlines on valid kicks and freeze time on focus loss.
            Evidence: Cross-feature sequences preserve legal placement, one-hold eligibility and exact deadlines without extra loops. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-024 — Verify and package modern gameplay
            Scope: Dual-browser suite, README, static version metadata and release notes; physical owners are mapped in layout.md.
            Depends on: T-023. Work: Exercise modern controls and baseline regressions in both targets, inspect ghost/hold render, prepare 0.2.0 artifact/notes.
            Evidence: AC-MODERN-1..4 and prior playable flow pass; visible preview and timing evidence match the documented release. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-025 — Review, test and report milestone 2.3
            Depends on: T-022, T-023, T-024 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/2/2.3.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 2.4 — Phase review
        - [ ] T-026 — Review, test and report phase 2
            Depends on: Milestone 2.1, Milestone 2.2, Milestone 2.3 completion/closure.
            Scope: R-BAG, R-HOLD, R-ROTATE, R-LANDING, R-DROP, R-GHOST, R-DELAY; integrated playable outcome and prior behavior.
            Evidence: independent code review, phase exits, required browser/visual/build/documentation checks and blocker repairs. Commit `docs/dev/reports/phases/2/PHASE-REPORT.md` and refresh HANDOFF with completion. Aggregate unresolved findings; when this is the final defined phase, also commit `docs/dev/reports/IMPLEMENTATION-REPORT.md` with final TODO provenance/options. Verify phase review issue/milestone closure before main integration and release preparation.

## Phase 3 — Dynamic ghost visibility

- [ ] Phase 3 — Dynamic ghost visibility
    - [ ] Milestone 3.1 — Projected-footprint visibility
        - [ ] T-027 — Implement all-cells midline visibility
            Scope: Pure visibility selector, snapshot/view wiring and focused fixtures; physical owners are mapped in layout.md.
            Depends on: T-026. Work: Apply y>=10 to every projected cell; hidden/straddling footprints draw no ghost while preserving geometry/timing.
            Evidence: Upper/lower/exact-boundary/straddling cases satisfy R-VISIBILITY and leave hard-drop destination unchanged. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-028 — Verify visibility transitions and static delivery
            Scope: Dual-browser acceptance, README and version/release metadata; physical owners are mapped in layout.md.
            Depends on: T-027. Work: Exercise lower→upper→lower landing changes using real commands; inspect ghost restoration and preserve ordinary modern play. Prepare 0.3.0 artifact/notes.
            Evidence: AC-VISIBILITY passes in both targets with meaningful screenshots; prior hold/drop/lock flow stays valid. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-029 — Review, test and report milestone 3.1
            Depends on: T-027, T-028 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/3/3.1.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 3.2 — Phase review
        - [ ] T-030 — Review, test and report phase 3
            Depends on: Milestone 3.1 completion/closure.
            Scope: R-VISIBILITY; integrated playable outcome and prior behavior.
            Evidence: independent code review, phase exits, required browser/visual/build/documentation checks and blocker repairs. Commit `docs/dev/reports/phases/3/PHASE-REPORT.md` and refresh HANDOFF with completion. Aggregate unresolved findings; when this is the final defined phase, also commit `docs/dev/reports/IMPLEMENTATION-REPORT.md` with final TODO provenance/options. Verify phase review issue/milestone closure before main integration and release preparation.

## Phase 4 — Penetrator gameplay

- [ ] Phase 4 — Penetrator gameplay
    - [ ] Milestone 4.1 — Legal penetration and projection
        - [ ] T-031 — Implement penetrator landing scans
            Scope: Geometry landing query and obstruction fixtures; physical owners are mapped in layout.md.
            Depends on: T-030. Work: Distinguish strict final placement from transit; scan all downward rows for greatest valid landing and reject no-destination candidates.
            Evidence: Upper obstruction/lower pocket and floor/ceiling/wall fixtures establish strict versus transit behavior. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-032 — Integrate penetrating movement and rotations
            Scope: Session move/rotate/drop commands and geometry tests; physical owners are mapped in layout.md.
            Depends on: T-031. Work: Use transit feasibility with a remaining legal destination; use ordered kick offsets; hard drop and ghost share the same full scan.
            Evidence: R-PENETRATOR candidate rejection has no side effects and ghost/drop destinations agree below obstructions. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-033 — Integrate valid grounding and locking
            Scope: Grounding clock, board write and integration fixtures; physical owners are mapped in layout.md.
            Depends on: T-032. Work: Never ground/lock an overlapping transit position; preserve effective delay and revalidate strict placement before atomic lock.
            Evidence: No occupied cell is destroyed; all accepted locks are fully visible/non-overlapping under timed and drop transitions. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-034 — Review, test and report milestone 4.1
            Depends on: T-031, T-032, T-033 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/4/4.1.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 4.2 — Activation and bonus economy
        - [ ] T-035 — Implement activation and hold exclusion
            Scope: Session mode/hold state, input P and focused tests; physical owners are mapped in layout.md.
            Depends on: T-034. Work: Validate available bonus and landing before spending; reject repeat/off toggles; keep hold disabled until lock.
            Evidence: Zero-bonus/invalid/repeated activation changes no count or queue; active hold is a side-effect-free rejection. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-036 — Award and cap four-line bonuses
            Scope: Line-clear/score transaction and bonus tests; physical owners are mapped in layout.md.
            Depends on: T-035. Work: Award one after each simultaneous four-line clear including penetrator clears, cap at three, reset on restart.
            Evidence: Cap, spend-before-award, three-line non-award and earned/spent sequences match R-BONUS. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-037 — Verify complete bonus and piece lifecycle
            Scope: Session integration fixtures; physical owners are mapped in layout.md.
            Depends on: T-036. Work: Run activation→transit→lock→clear→award→spawn and failure sequences, preserving next/hold/timer state.
            Evidence: Mode ends before spawn, counters stay 0..3 and existing scoring/source rules remain correct. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-038 — Review, test and report milestone 4.2
            Depends on: T-035, T-036, T-037 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/4/4.2.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 4.3 — Penetrator feedback and delivery
        - [ ] T-039 — Render penetrator and bonus status
            Scope: Canvas mode color and HTML bonus/hold status; physical owners are mapped in layout.md.
            Depends on: T-038. Work: White active/ghost geometry, non-white regular/settled kind colors and exactly one accessible symbol per bonus.
            Evidence: Spend/earn snapshots immediately update count/symbols; white ghost remains visually distinguishable. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-040 — Verify penetrator play in both targets
            Scope: Browser acceptance and screenshot fixtures; physical owners are mapped in layout.md.
            Depends on: T-039. Work: Exercise native activation, pass-through, deepest drop/ghost, hold rejection, awards and restart with real app wiring.
            Evidence: AC-PENETRATOR and prior modern/visibility regressions pass, with occupied-cell preservation visible. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-041 — Prepare penetrator static release
            Scope: README, build/version metadata and release notes; physical owners are mapped in layout.md.
            Depends on: T-040. Work: Document controls, legal transit and bonus rules; produce 0.4.0 static artifact and matching notes.
            Evidence: Built artifact reproduces the verified behavior; no debug fixture/data or local browser state is shipped. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-042 — Review, test and report milestone 4.3
            Depends on: T-039, T-040, T-041 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/4/4.3.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 4.4 — Phase review
        - [ ] T-043 — Review, test and report phase 4
            Depends on: Milestone 4.1, Milestone 4.2, Milestone 4.3 completion/closure.
            Scope: R-PENETRATOR, R-BONUS, R-PEN-VIEW; integrated playable outcome and prior behavior.
            Evidence: independent code review, phase exits, required browser/visual/build/documentation checks and blocker repairs. Commit `docs/dev/reports/phases/4/PHASE-REPORT.md` and refresh HANDOFF with completion. Aggregate unresolved findings; when this is the final defined phase, also commit `docs/dev/reports/IMPLEMENTATION-REPORT.md` with final TODO provenance/options. Verify phase review issue/milestone closure before main integration and release preparation.

## Phase 5 — Session game configuration

- [ ] Phase 5 — Session game configuration
    - [ ] Milestone 5.1 — Selected settings and visible controls
        - [ ] T-044 — Implement validated session configuration
            Scope: Typed configuration model, selectors and tests; physical owners are mapped in layout.md.
            Depends on: T-043. Work: Six exact key/value sets/defaults; reject invalid changes; keep selected state across restart only.
            Evidence: R-CONFIG defaults, invalid commands and all selector values remain typed and memory-only. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-045 — Build labelled keyboard-accessible controls
            Scope: HTML configuration view and input-focus integration; physical owners are mapped in layout.md.
            Depends on: T-044. Work: Expose all six controls and current selected values; native editing must not invoke gameplay shortcuts.
            Evidence: Control labels, focus, immediate display/input effects and disabled hold/drop retention pass actual browser checks. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-046 — Implement next preview and live display selectors
            Scope: Queued-kind presentation and ghost/hold/drop selector wiring; physical owners are mapped in layout.md.
            Depends on: T-045. Work: Show next without consumption; support hidden/dynamic/shown ghost and immediate input toggles independent of lock choices.
            Evidence: Preview matches next spawn/empty hold; toggles preserve source, stored kind and legal landing. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-047 — Review, test and report milestone 5.1
            Depends on: T-044, T-045, T-046 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/5/5.1.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 5.2 — Locking policies and session delivery
        - [ ] T-048 — Latch locking selections per piece
            Scope: Session spawn/hold policy snapshot and clock tests; physical owners are mapped in layout.md.
            Depends on: T-047. Work: Latch both general and hard-drop rules on spawn; implement failed-down/interval and follow/immediate combinations.
            Evidence: All four combinations satisfy R-CONFIG; current timer is unaffected by selected changes and hold replacement adopts settings. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-049 — Integrate pending status and restart semantics
            Scope: HTML pending view, composition and interaction tests; physical owners are mapped in layout.md.
            Depends on: T-048. Work: Distinguish selected/effective policies; apply pending values on spawn/restart, retain settings on restart and reset on reload.
            Evidence: Airborne/grounded/penetrator changes are predictable; no cookie/storage/URL writes occur. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-050 — Verify and package session configuration
            Scope: Dual-browser tests, README, version metadata and release notes; physical owners are mapped in layout.md.
            Depends on: T-049. Work: Exercise all controls and combinations during live play, reload and focus; produce 0.5.0 artifact/notes.
            Evidence: AC-CONFIG and prior gameplay pass in both targets, including no persistence and next-preview invariants. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-051 — Review, test and report milestone 5.2
            Depends on: T-048, T-049, T-050 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/5/5.2.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 5.3 — Phase review
        - [ ] T-052 — Review, test and report phase 5
            Depends on: Milestone 5.1, Milestone 5.2 completion/closure.
            Scope: R-CONFIG; integrated playable outcome and prior behavior.
            Evidence: independent code review, phase exits, required browser/visual/build/documentation checks and blocker repairs. Commit `docs/dev/reports/phases/5/PHASE-REPORT.md` and refresh HANDOFF with completion. Aggregate unresolved findings; when this is the final defined phase, also commit `docs/dev/reports/IMPLEMENTATION-REPORT.md` with final TODO provenance/options. Verify phase review issue/milestone closure before main integration and release preparation.

## Phase 6 — Persistent configuration

- [ ] Phase 6 — Persistent configuration
    - [ ] Milestone 6.1 — Validated hydration sources
        - [ ] T-053 — Implement per-setting codec and precedence
            Scope: Pure configuration codec and source-matrix tests; physical owners are mapped in layout.md.
            Depends on: T-052. Work: Validate exact key/value strings, duplicate/invalid/empty inputs and independent URL→cookie→default resolution.
            Evidence: AC-PERSISTENCE mixed-source and invalid-one/valid-five fixtures produce exact settings and explicit-valid provenance. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-054 — Implement scoped cookie adapter
            Scope: Cookie browser adapter and isolated-context tests; physical owners are mapped in layout.md.
            Depends on: T-053. Work: Read/write managed encoded cookies with app-directory path, max age and HTTPS attributes; detect failed readback.
            Evidence: Malformed/duplicate/blocked cookie cases are isolated per key; default-equal values save and reload correctly. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-055 — Implement URL hydration and startup coordination
            Scope: URL adapter, coordinator and bootstrap; physical owners are mapped in layout.md.
            Depends on: T-054. Work: Read without rewriting URL; hydrate before spawn; seed only valid explicit URL values into corresponding cookies.
            Evidence: Omitted/invalid URL settings retain saved cookies; explicit defaults seed cookies; unknown query/fragment remain intact. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-056 — Review, test and report milestone 6.1
            Depends on: T-053, T-054, T-055 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/6/6.1.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 6.2 — Resilient edits and persistent delivery
        - [ ] T-057 — Persist individual user edits
            Scope: Configuration coordinator and URL replacement path; physical owners are mapped in layout.md.
            Depends on: T-056. Work: Update in-memory selection then independently attempt one cookie and one history replacement; canonicalize only the edited key.
            Evidence: No reload/history growth, unrelated parameters and fragment survive, and same-value edits can retry idempotently. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-058 — Handle persistence failures truthfully
            Scope: Save-status view and browser adapter failure tests; physical owners are mapped in layout.md.
            Depends on: T-057. Work: Catch read/write/history errors, keep play active, expose per-source unsaved state and clear it after successful retry.
            Evidence: Failed URL save does not suppress cookie save and vice versa; blocked cookies do not defeat a valid URL. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-059 — Verify reload behavior and prepare persistent release
            Scope: Dual-browser acceptance, README, static version metadata and release notes; physical owners are mapped in layout.md.
            Depends on: T-058. Work: Exercise clean/saved/mixed contexts, exact defaults, invalid values, path scope, failure/retry and live policy latching. Produce 0.6.0 artifact/notes.
            Evidence: AC-PERSISTENCE, AC-CONFIG and all prior game flows pass in both targets with current screenshots and documented limits. Include relevant tests/docs with this task; do not postpone its own acceptance to the review.
        - [ ] T-060 — Review, test and report milestone 6.2
            Depends on: T-057, T-058, T-059 and their verified issue closures. Scope: the complete milestone capability, its changed code and relevant dependencies.
            Evidence: separate code review and focused/regression execution, repair all blockers, demonstrate the milestone outcome and retain justified TODOs with impact/options/owner. Commit report `docs/dev/reports/phases/6/6.2.md` with completion; close the review issue and then the milestone only after verified publication.
    - [ ] Milestone 6.3 — Phase review
        - [ ] T-061 — Review, test and report phase 6
            Depends on: Milestone 6.1, Milestone 6.2 completion/closure.
            Scope: R-PERSISTENCE; integrated playable outcome and prior behavior.
            Evidence: independent code review, phase exits, required browser/visual/build/documentation checks and blocker repairs. Commit `docs/dev/reports/phases/6/PHASE-REPORT.md` and refresh HANDOFF with completion. Aggregate unresolved findings; when this is the final defined phase, also commit `docs/dev/reports/IMPLEMENTATION-REPORT.md` with final TODO provenance/options. Verify phase review issue/milestone closure before main integration and release preparation.
