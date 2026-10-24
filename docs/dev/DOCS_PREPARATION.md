# Main documentation preparation record

## Current control

- Mode: autodoc/autoloop, explicitly requested on 2026-10-24 for the six cumulative scopes below.
- Multipass delivery mode: strict, the package default; flexible integration has not been authorized. Protect earlier delivery phase/task IDs, scope, milestone membership, order, dependencies and exits. No implementation ranges have yet been defined.
- Current scope: Pass 1, playable classic baseline; DOCS-001 initialization checkpoint.
- Approval basis: first PROJECT approval remains active. Later PROJECT approvals are waived; ARCHITECTURE through TASKS checkpoints are preapproved by the selected mode. Approval of the first brief authorizes the agreed sequence and resets. Newly discovered eligible material choices are delegated to agent judgment, with visible rationale and retained evidence.
- Tracking: root AGENTS.md selects GitHub preparation and implementation tracking by default. Milestone 1 and issues 1–9 were created from this repository's bundled definitions and individually read back before target writes. Git publication remains mandatory after every owned commit.
- Next boundary: publish/read back initialization, close/read back DOCS-001, then prepare and publish the first PROJECT proposal for human approval. Final loop stop: Pass 6 DOCS-008; DOCS-009/package QC, documentation integration and implementation remain outside that stop.
- Reporting: Explicit bootstrap completed; Standard thereafter. Minimal and Explicit remain available on request.

## Scope sequence

1. **Playable classic baseline.** Falling tetrominoes, movement, rotation, gravity, line clearing, scoring, game over and restart. Keep a minimal coherent playable product; subsequent scopes are not requirements of this brief.
2. **Gameplay enhancements.** Add hold, ghost, 7-bag randomization, wall kicks, hard drop and default full-gravity-interval locking from grounding, including after hard drop.
3. **Conditional ghost visibility.** Hide the projected ghost above the midline and show it again in the lower half; resolve straddling using the exploration proposal below.
4. **Penetrator mode.** Hotkey activation costs one bonus; pass through occupied cells without destruction until locking, with an entirely in-board non-overlapping final placement. Hard drop chooses the lowest valid downward landing and visible ghost matches it. Four-line clear earns one bonus, capped at three; white penetrators, no white regular pieces; side-panel symbol per bonus updates on earning/spending. Resolve hold and transit/locking interactions as proposed below.
5. **Session-only game configuration.** Side-panel controls for ghost hidden/dynamic/shown, hard drop on/off, next preview on/off, hard-drop locking follow-general/immediate, general failed-down/full-interval locking, hold on/off. Add next preview. Settings survive restart but reset on reload; no persistence in this scope.
6. **Configuration persistence.** All six settings use independent URL → cookie → default resolution. A player change updates its cookie and URL parameter. Explicit valid URL values seed cookies even when equal to defaults. Omitted parameters preserve valid saved cookies. Invalid inputs and storage failures use the exploration proposals below.

## Per-pass decisions

These are initial exploration proposals by the agent, pending human acceptance with the first brief and this companion record; they are not human-supplied requirements. Future-scope proposals stay here until their scope becomes current. Revisit on steering or contradictory evidence.

| Origin / decision | Resolution and rationale | Basis / source | Applicability / status |
| --- | --- | --- | --- |
| Pass 1 / product | Single-player desktop keyboard game, 10×20 visible board, seven tetrominoes, score and restart; no backend or accounts. | User browser-game seed; agent proposes platform details and exclusions. | Passes 1–6; proposed. |
| Pass 1 / implementation direction | TypeScript, Canvas board and semantic HTML controls, static client delivery. Plain JavaScript would reduce tooling but weaken state contracts; a UI framework adds little value for this scale. Detailed architecture remains for DOCS-003. | Agent recommendation for review; no claim of human technology selection. | Passes 1–6; proposed. |
| Pass 1 / minimal rules | Uniform independent piece selection; simple clockwise rotation without kicks; left/right, rotate and soft drop; fixed gravity; lock on failed down; score by simultaneous cleared lines. Specify exact keys, timing and scoring during SPEC. | Agent minimal-baseline proposal; later enhancements remain separate. | Pass 1; replaced only by corresponding later scope. |
| Pass 1 / browser validation | Chromium primary, Playwright-managed Firefox secondary; headless UI interaction and screenshot checks in dedicated isolated environments. Central BROWSER_DEV_POLICY.md owns provisioning and proactive recovery. Document-only work needs no browser installation. | Existing root AGENTS.md and BROWSER_DEV_POLICY.md; runner recommendation by agent. | All delivery phases; proposed runner, existing browser policy active. |
| Pass 1 / versioning | First verified playable phase targets 0.1.0; subsequent coherent added-capability phases increment the minor version, corrective compatible releases the patch. Publish a release/tag/notes after each completed, verified and integrated implementation phase, only once implementation is authorized. Preparation creates no product release. | Explicit user release request; agent SemVer proposal. | All implementation phases; pending planning allocation. |
| Pass 2 / lock clock | Start at valid first grounding; consume one full gravity interval. Movement/rotation do not extend a still-grounded timer. Losing support clears it; regrounding starts a full new interval. Hard drop uses remaining existing grounded time, or a new interval if newly grounded. Ignore repeated hard drops as a timer reset. Freeze simulation timing while unfocused; no catch-up burst. | Agent proposal to prevent indefinite grounded resets and unify drop behavior. | Passes 2–6; proposed. |
| Pass 2 / hold | One hold action per spawned piece, reset by successful lock/spawn; stored piece re-enters at normal spawn orientation. Empty hold consumes the next piece; blocked spawn is game over. | Agent proposal for predictable hold. | Passes 2–6; proposed. |
| Pass 3 / midline | Show only if every projected ghost cell is at or below visible row 10 in a zero-based 20-row board. A ghost with any cell above that boundary, including a straddling ghost, is hidden. Evaluate projected geometry after every change. | Agent conservative all-cells interpretation. | Passes 3–6; proposed. |
| Pass 4 / activation and hold | Activation spends one available bonus immediately, persists until lock and cannot be toggled off; repeat activation has no effect/cost. Hold is disabled for an active penetrator. Restart clears bonuses and activation. | Agent proposal; consistent with the established penetrator clarification supplied in project context, presented here for this repository's acceptance. | Passes 4–6; proposed. |
| Pass 4 / legal transit and lock | Occupied cells may be traversed without destruction; side walls and floor remain hard bounds. A penetrator may ground/lock only in a fully in-board non-overlapping placement. Scan downward for the deepest valid landing, even below intervening obstructions; ghost and hard drop share that result. Accept activation/move/rotation only when at least one valid landing remains at or below the candidate, preventing an overlapping piece stranded at the floor. Transit overlap never starts a lock timer. | Agent proposal to reconcile penetration with legal final placement; invalid commands are no-ops and rejected activation costs no bonus. | Passes 4–6; proposed. |
| Pass 4 / bonuses | Award after a single four-line clear, capped at three, including clears made by a penetrator; spend before any later award. White styling applies to active penetrator and its visible ghost; regular piece colors exclude white. | User award/cap/rendering rules; agent event-order and ghost proposal. | Passes 4–6; proposed refinements. |
| Pass 5 / defaults | Dynamic ghost, hard drop enabled, next preview enabled, follow-general hard-drop lock, full-interval general lock and hold enabled. | Agent sensible defaults maintaining cumulative behavior. | Passes 5–6; proposed. |
| Pass 5 / live changes | Display/input toggles apply immediately; disabling hold retains its stored piece. General and hard-drop lock-policy changes take effect at next spawn and display pending status. They do not alter a current grounded timer. Restart retains selected settings, clears board/score/hold/bonuses, and applies pending policies. | Agent proposal to avoid unexpected locking from a setting change. | Passes 5–6; proposed. |
| Pass 6 / invalid inputs | Validate every source independently against exact allowed values. Invalid/empty/duplicate URL values fall back to a valid cookie, then default, without overwriting a valid cookie. Unknown parameters are retained; never copy raw invalid data into the DOM. | Agent proposal; user precedence applies to valid supplied values. | Pass 6; proposed. |
| Pass 6 / persistence failures | Use versioned setting names, path-scoped cookies and finite lifetime; precise schema belongs in SPEC. Explicit valid URL values persist even at defaults. Omitted parameters are not added during hydration; each user edit changes only its own parameter/cookie via history replacement. Preserve other query parameters and fragment. Cookie/history failures retain in-memory play and expose a non-blocking save-status message. | User persistence requirements; agent encoding/failure proposal. | Pass 6; proposed. |

## Cross-scope decisions

| Concern | Applicable decisions | Reconsideration condition |
| --- | --- | --- |
| Scope preservation | Pass 1 minimal rules, six ordered scopes and strict protected delivery ranges; future requirements stay out of current product roots. | Explicit flexible permission or a concrete correctness conflict requiring scoped direction. |
| Landing and visibility | Pass 3 midline plus Pass 4 legal-transit decisions; visible ghost and hard drop share a destination. | Board geometry or penetration semantics change. |
| Locking, hold and configuration | Pass 2 clock/hold, Pass 4 activation and Pass 5 live-change decisions. | New manipulation rules, clock behavior or configuration semantics. |
| Reload versus restart | Pass 5 session semantics; Pass 6 source precedence and failure behavior. | Storage schema or user steering changes. |
| Validation and releases | Pass 1 browser/versioning decisions carry forward without adding later capabilities to earlier phase exits. | Different runtime/browser requirements or changed release authorization. |

## Decisions and document evidence

| Task | Current evidence / decision basis |
| --- | --- |
| DOCS-001 | All four root files retained byte for byte against initial commit 35c594d49dbe3e0483f79585c2553e7323bfb52c; task template copied from bundled 0.15.2 and only DOCS-001 checked. Provenance identities below. Initialization publication and issue 1 closure require subsequent readback. |
| DOCS-002 | Pass 1 PROJECT proposal not yet authored at initialization; first human approval required. |
| DOCS-003–008 | Not started; later checkpoints preapproved by mode after the first PROJECT approval. |
| DOCS-009 | Not selected; keep task, issue 9 and milestone 1 open at the final autoloop stop. |

## Initialization and current reset

- Original root-file presence: all four present. Retained AGENTS.md, AI_DISCLOSURE.md, SDD-MANAGER.md and SKILL_DISCOVERY_PROTOCOL.md. No root guidance files copied or reconciled.
- Verification: each retained file compared byte for byte with the initial Git object. Bundled task definitions preserved; token excluded by appended `*.tkn` rule and verified untracked before storage.
- Continuation: all four present, so no mixed-state pause applies.
- Manual provenance: default enabled; only the two package scaffold files copied unchanged. Published seed a33e466ebd28a47a20e666aabf1159d80156616f from initial main 35c594d49dbe3e0483f79585c2553e7323bfb52c; explicit two-parent merge 1082c5229eb7fb58129b28543e1f3b0c0fdf6e4f on main verified and published before this branch. No conversations recorded.
- Current reset: none; Pass 1.
- Preparation branch creation parent: main; full branch-off 1082c5229eb7fb58129b28543e1f3b0c0fdf6e4f.
- Execution host: Linux; Git 2.51.1, Node 24.19.0, Python 3.12.14. Windows-specific shell policy does not apply to this host. No dependencies installed and no browser provisioned.

## Pending transitions

Publish/read back this initialization commit and close/read back issue 1 before PROJECT work. First PROJECT and exploration proposals remain pending human review. No implementation, release, package QC or project-document integration is authorized at this boundary.
