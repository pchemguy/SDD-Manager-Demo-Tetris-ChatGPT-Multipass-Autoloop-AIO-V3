# Browser Tetris

## Purpose and users

Create a small, single-player browser game for desktop keyboard play. Players can open the page, understand the controls, play a complete falling-block game, see a score and restart after losing.

## Intended product

Use a 10-column by 20-row visible board and the seven standard tetrominoes. Provide falling pieces, movement, clockwise rotation, soft drop, simultaneous line clearing, line-based scoring, game over and restart. Keep gravity fixed rather than introducing speed levels. A blocked spawn or above-board lock ends play; restart begins a clean game.

Provide hold, a projected ghost, independently shuffled seven-piece bags, clockwise SRS wall kicks and hard drop. Default locking waits one full gravity interval after valid grounding, including hard drop. Grounded manipulation does not extend the deadline; losing support clears it and regrounding starts a fresh interval. Hold is limited to one exchange per piece cycle. Source, timing and game transitions must be deterministic under injected test inputs.

The dynamic ghost is hidden whenever any projected cell lies above the board's midline, including a straddling footprint, and returns when the whole projected footprint is in the lower half. Landing geometry is independent of visibility.

A bonus permits hotkey activation of the current piece as a penetrator until lock. It can traverse settled cells without destroying them, but its final placement must be fully in-board and non-overlapping. Its hard drop and visible ghost select the lowest legal downward destination, even below obstructions. Hold is disabled during activation. A four-line clear earns one bonus, capped at three. Render active penetrators and their visible ghosts white, reserve non-white kind colors for regular/settled pieces, and display one side-panel symbol per available bonus.

A side panel selects ghost hidden/dynamic/shown, hard drop on/off, next preview on/off, hard-drop locking follow-general/immediate, general locking failed-down/full-interval and hold on/off. Defaults enable dynamic ghost, drop, preview and hold, with interval locking and hard drop following it. Display/input settings apply immediately; locking settings apply at the next spawn with visible pending status. Restart retains selected settings.

Settings last only for the current page session. Reload restores defaults without cookie, URL or other persistent writes.

## Quality and constraints

Run entirely as static browser assets, without a game server. Use TypeScript for the domain, Canvas for the board and ordinary labelled HTML controls/status. Keep rules testable without browser globals, and verify the integrated game with real browser input and visible evidence. Chromium is primary and Playwright-managed Firefox secondary under the central [browser development policy](../../BROWSER_DEV_POLICY.md); headless operation is sufficient. Focus loss suspends active simulation time without a catch-up burst.

## Non-goals

Multiplayer, accounts, online leaderboards, touch controls, audio, saved gameplay sessions, speed-level progression and elaborate visual effects are outside scope. Exact behavior and acceptance are owned by the specification; structure, delivery order and executable work have separate document owners. This brief describes the intended product and does not claim implemented functionality.
