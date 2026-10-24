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
