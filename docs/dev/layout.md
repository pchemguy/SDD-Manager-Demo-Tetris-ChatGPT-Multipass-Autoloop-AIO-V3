# Repository layout

## Ownership and status

This map allocates intended implementation paths from [DECOMPOSITION](DECOMPOSITION.md) and [PLAN](PLAN.md). Existing repository assets include root guidance/policies and the bundled plugin; source/build/test paths below are planned and do not claim existing implementation.

| Physical owner | Component / responsibility | Verification home |
| --- | --- | --- |
| src/domain/types.ts | Domain snapshot, cell, piece, command and result types; no browser imports | tests/unit/domain/ |
| src/domain/geometry.ts | Shapes, transforms and strict collision/placement | tests/unit/domain/geometry.test.ts |
| src/domain/board.ts | Settled grid, atomic writes and simultaneous clear/compaction | tests/unit/domain/board.test.ts |
| src/domain/source.ts | Injected kind source and restart lifecycle | tests/unit/domain/source.test.ts |
| src/domain/session.ts | Authoritative gameplay transitions, score, clocks and lifecycle | tests/unit/domain/session.test.ts; tests/integration/session.test.ts |
| src/browser/input.ts; src/browser/loop.ts | Native commands, focus and owned frame lifecycle | tests/unit/browser/; tests/browser/game.spec.ts |
| src/ui/canvas.ts; src/ui/view.ts; src/ui/styles.css | Canvas cells and semantic status/restart/instructions | tests/browser/game.spec.ts; tests/browser/visual.spec.ts |
| src/main.ts; index.html | Composition and single application root | tests/browser/game.spec.ts; static-build smoke |
| package.json; package-lock.json; tsconfig.json; vite.config.ts; vitest.config.ts; playwright.config.ts | Pinned tooling, build/unit/browser configuration | Task setup evidence; clean build and runner preflight |
| tests/fixtures/ | Deterministic board/source/input fixtures used by checks | Consumers own behavior assertions; production build excludes fixtures |
| README.md; RELEASE-NOTES.md | Supported setup/play/test instructions and complete release message | Build/docs/version checks; release notes verified against delivered scope |
| docs/dev/reports/phases/ | Committed milestone and phase acceptance reports | Owning review task and phase boundary |
| docs/dev/reports/IMPLEMENTATION-REPORT.md | Final delivered-scope report and unresolved TODO aggregation | Final defined phase review |

Domain modules never import browser or UI paths. UI consumes snapshots; composition wires concrete adapters. Unit tests live beside their domain/browser ownership within tests, while integration tests exercise cross-module session behavior. Browser fixtures must use explicit test-only setup, excluded from production; never ship a production debug board setter. Images/state/binaries are generated artifacts, not source fixtures.

## Build and generated artifacts

Use dist/ for static production output; keep node_modules/, dist/, test-results/, playwright-report/, .browser-cache/ and other project-owned profiles/downloads ignored. Set the absolute isolated browser path consistently for setup and execution under the central browser policy. No global cache/profile or /pyenv modification is authorized by this layout. Pin versions and commands during tooling implementation; no unverified executable command is claimed here. Root version/package metadata and complete RELEASE-NOTES.md agree at each release. Do not store tokens in source or generated artifacts; the conventional excluded gh.tkn remains separate.

## Documentation and instruction boundaries

The seven development roots and adjacent QC reports stay in docs/dev/. Preparation tasks/control/checkpoint have distinct owners: DOCS_TASKS, DOCS_PREPARATION and HANDOFF. Manual conversation templates/records stay under docs/ai-logs/ and their scoped AGENTS; they are excluded from routine validation. Bundled .agents/ content is externally supplied workflow tooling and is outside product implementation edits. Root AGENTS and browser/shell policies govern execution; source/test ownership changes update navigation without duplicating these policies.

## Enhanced gameplay allocation

Use src/domain/landing.ts for shared projection, src/domain/rotation.ts for ordered kick tables/selection and src/domain/clock.ts for grounding policy. Their focused tests belong in tests/unit/domain/ with cross-feature checks in tests/integration/session.test.ts. Source.ts owns bag/queue; session.ts owns hold state; Canvas/view own ghost and held-kind presentation. Avoid a separate mutable view queue.
