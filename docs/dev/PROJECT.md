# Browser Tetris

## Purpose and users

Create a small, single-player browser game for desktop keyboard play. Players can open the page, understand the controls, play a complete falling-block game, see a score and restart after losing.

## Intended product

Use a 10-column by 20-row visible board and the seven standard tetrominoes. Provide falling pieces, movement, clockwise rotation, soft drop, simultaneous line clearing, line-based scoring, game over and restart. Keep gravity fixed rather than introducing speed levels. A blocked spawn or above-board lock ends play; restart begins a clean game.

Provide hold, a projected ghost, independently shuffled seven-piece bags, clockwise SRS wall kicks and hard drop. Default locking waits one full gravity interval after valid grounding, including hard drop. Grounded manipulation does not extend the deadline; losing support clears it and regrounding starts a fresh interval. Hold is limited to one exchange per piece cycle. Source, timing and game transitions must be deterministic under injected test inputs.

## Quality and constraints

Run entirely as static browser assets, without a game server. Use TypeScript for the domain, Canvas for the board and ordinary labelled HTML controls/status. Keep rules testable without browser globals, and verify the integrated game with real browser input and visible evidence. Chromium is primary and Playwright-managed Firefox secondary under the central [browser development policy](../../BROWSER_DEV_POLICY.md); headless operation is sufficient. Focus loss suspends active simulation time without a catch-up burst.

## Non-goals

Multiplayer, accounts, online leaderboards, touch controls, audio, saved gameplay sessions, speed-level progression and elaborate visual effects are outside scope. Exact behavior and acceptance are owned by the specification; structure, delivery order and executable work have separate document owners. This brief describes the intended product and does not claim implemented functionality.
