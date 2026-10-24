# Delivery plan

## Delivery strategy

Deliver a static client in coherent capability increments, preserving an integrated playable game at each phase exit. [SPEC](SPEC.md) owns behavioral contracts; [ARCHITECTURE](ARCHITECTURE.md) and [DECOMPOSITION](DECOMPOSITION.md) own responsibilities. The milestones below own delivery outcomes; TASKS will own executable units. Phase 1 establishes the simplest useful game rather than a scaffold-only release.

Each implementation task includes relevant tests and documentation. Every delivery milestone ends with code review, testing, repair of blockers and a report. A separate final phase review milestone contains exactly one review/testing/report outcome after all delivery milestones complete/close. Code review and tests are distinct obligations. Confirmed contract/exit failures block completion; retain only justified non-critical TODOs with impact, options and owner.

Milestones integrate their capability early; no final integration bucket may defer broken behavior. Browser verification follows the central [BROWSER_DEV_POLICY](../../BROWSER_DEV_POLICY.md); required provisioning and representative execution precede dependent browser acceptance. Use Chromium primary and managed Firefox secondary without weakening either target on setup failure. Preserve usable gameplay during tooling and UI changes.

Each phase is a bounded release candidate: 0.1.0 starts the playable series, subsequent capability phases increment the minor version, and compatible corrective releases increment the patch. Release notes describe only verified delivered behavior. Release/tag publication requires authorized implementation, complete exits and published default-branch integration. Preparation is not implementation authority. Review findings and UI evidence enable human steering within the selected range; routine work uses existing authorization.

## Delivery boundaries

The phase definitions fix capability scope, milestone membership, dependencies and exits. Tests for an early delivery use its included contract subset; later capabilities do not retroactively broaden that exit. A later explicit rule change is verified in its own phase alongside preservation of unaffected earlier behavior. Source/test/documentation placement is allocated by layout, not this strategy.

## Phase 1 — Playable classic baseline

Outcome: An independently playable classic game: fixed gravity, independent kinds and simple rotation; complete scoring/loss/restart flow.

Scope: R-BOARD, R-PIECE, R-INPUT, R-TIME, R-LOCK (failed-down baseline), R-CLEAR, R-SCORE, R-LIFE, R-VIEW, R-QUALITY. Version target: 0.1.0. No predecessor; establish the simplest complete playable baseline.

### Milestone 1.1 — Deterministic rules foundation

Reproducible isolated tooling and independently checked board/session rules; no claim of a playable UI yet.

Includes: establish isolated typescript and browser verification tooling; implement board and tetromino geometry; implement deterministic falling-piece lifecycle.

Dependency: phase entry. Exit: integrated milestone capability, its focused checks and relevant regressions, code review with blockers repaired, and a committed milestone report. Demonstrate the stated outcome at this boundary; evidence supports a human continue/amend/simplify/stop decision without imposing an extra approval on an already authorized range.

### Milestone 1.2 — Playable browser session

A keyboard-playable board with complete line clearing, score and restart; inspect basic control discoverability before broader acceptance.

Includes: compose canvas playfield and readable session status; wire keyboard, focus suspension and restart; integrate simultaneous line clears and score.

Dependency: Milestone 1.1 completion/closure. Exit: integrated milestone capability, its focused checks and relevant regressions, code review with blockers repaired, and a committed milestone report. Demonstrate the stated outcome at this boundary; evidence supports a human continue/amend/simplify/stop decision without imposing an extra approval on an already authorized range.

### Milestone 1.3 — Verified static delivery

A usable, documented static artifact supported by complete baseline acceptance and independent browser evidence.

Includes: verify lifecycle and board integration invariants; verify the classic game in both browser targets; prepare documented static build and version metadata.

Dependency: Milestone 1.2 completion/closure. Exit: integrated milestone capability, its focused checks and relevant regressions, code review with blockers repaired, and a committed milestone report. Demonstrate the stated outcome at this boundary; evidence supports a human continue/amend/simplify/stop decision without imposing an extra approval on an already authorized range.

### Milestone 1.4 — Phase review

Exactly one final code-review/testing/report outcome after all delivery milestones complete and close. Verify the phase contracts and playable integration, applicable browser/visual evidence, prior behavior, build/docs/version consistency and unresolved findings. Repair blockers before completion. Aggregate unresolved findings for the delivered scope; when this is the final defined phase, include the final implementation report/TODO aggregation.

Phase exit: all delivery and review outcomes complete, reports committed/published, required issue/milestone closures verified, and a two-parent integration into main verified/published. After implementation is authorized and these exits hold, create the 0.1.0 default-branch release commit, tag that exact commit, then publish matching complete release notes and static artifact. No phase completion or release is claimed by this plan.

## Phase 2 — Modern gameplay

Outcome: A playable modern piece lifecycle with hold, shuffled bags, kicks, shared projection and full-interval default locking.

Scope: R-BAG, R-HOLD, R-ROTATE, R-LANDING, R-DROP, R-GHOST, R-DELAY. Version target: 0.2.0. Depends on verified, published integration of Phase 1; preserve its working behavior except the explicitly scheduled rule changes in this phase.

### Milestone 2.1 — Bag source and hold

A coherent predictable source/hold lifecycle with visible stored kind.

Includes: implement the 7-bag queue; implement one-exchange hold lifecycle; expose hold input and view.

Dependency: phase entry. Exit: integrated milestone capability, its focused checks and relevant regressions, code review with blockers repaired, and a committed milestone report. Demonstrate the stated outcome at this boundary; evidence supports a human continue/amend/simplify/stop decision without imposing an extra approval on an already authorized range.

### Milestone 2.2 — Rotation and shared landing

SRS movement and a ghost that previews the exact hard-drop destination.

Includes: implement ordered clockwise wall kicks; implement shared landing and delayed hard drop; render the projected ghost.

Dependency: Milestone 2.1 completion/closure. Exit: integrated milestone capability, its focused checks and relevant regressions, code review with blockers repaired, and a committed milestone report. Demonstrate the stated outcome at this boundary; evidence supports a human continue/amend/simplify/stop decision without imposing an extra approval on an already authorized range.

### Milestone 2.3 — Grounding clock and integrated modern delivery

One coherent delayed-lock lifecycle, usable in both browsers and documented for the static release.

Includes: implement full-interval grounding time; integrate hold, kicks and focus with grounding; verify and package modern gameplay.

Dependency: Milestone 2.2 completion/closure. Exit: integrated milestone capability, its focused checks and relevant regressions, code review with blockers repaired, and a committed milestone report. Demonstrate the stated outcome at this boundary; evidence supports a human continue/amend/simplify/stop decision without imposing an extra approval on an already authorized range.

### Milestone 2.4 — Phase review

Exactly one final code-review/testing/report outcome after all delivery milestones complete and close. Verify the phase contracts and playable integration, applicable browser/visual evidence, prior behavior, build/docs/version consistency and unresolved findings. Repair blockers before completion. Aggregate unresolved findings for the delivered scope; when this is the final defined phase, include the final implementation report/TODO aggregation.

Phase exit: all delivery and review outcomes complete, reports committed/published, required issue/milestone closures verified, and a two-parent integration into main verified/published. After implementation is authorized and these exits hold, create the 0.2.0 default-branch release commit, tag that exact commit, then publish matching complete release notes and static artifact. No phase completion or release is claimed by this plan.

## Phase 3 — Dynamic ghost visibility

Outcome: A ghost that hides above the midline and restores only for a fully lower-half landing.

Scope: R-VISIBILITY. Version target: 0.3.0. Depends on verified, published integration of Phase 2; preserve its working behavior except the explicitly scheduled rule changes in this phase.

### Milestone 3.1 — Projected-footprint visibility

One small integrated visibility capability; no unrelated gameplay expansion.

Includes: implement all-cells midline visibility; verify visibility transitions and static delivery.

Dependency: phase entry. Exit: integrated milestone capability, its focused checks and relevant regressions, code review with blockers repaired, and a committed milestone report. Demonstrate the stated outcome at this boundary; evidence supports a human continue/amend/simplify/stop decision without imposing an extra approval on an already authorized range.

### Milestone 3.2 — Phase review

Exactly one final code-review/testing/report outcome after all delivery milestones complete and close. Verify the phase contracts and playable integration, applicable browser/visual evidence, prior behavior, build/docs/version consistency and unresolved findings. Repair blockers before completion. Aggregate unresolved findings for the delivered scope; when this is the final defined phase, include the final implementation report/TODO aggregation.

Phase exit: all delivery and review outcomes complete, reports committed/published, required issue/milestone closures verified, and a two-parent integration into main verified/published. After implementation is authorized and these exits hold, create the 0.3.0 default-branch release commit, tag that exact commit, then publish matching complete release notes and static artifact. No phase completion or release is claimed by this plan.
