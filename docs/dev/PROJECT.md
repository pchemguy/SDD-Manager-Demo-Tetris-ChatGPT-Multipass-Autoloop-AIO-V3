# Browser Tetris

## Purpose and users

Create a small, single-player browser game for desktop keyboard play. A player can open the page, understand the controls, play a complete falling-block game, see a score and restart after losing.

## Intended experience

The game uses a 10-column by 20-row visible board and the seven standard tetromino shapes. Pieces spawn above the playfield, fall under gravity and respond to horizontal movement, clockwise rotation and soft drop. Movement respects the board and settled cells. Rotation uses a simple collision check. A piece locks when a downward movement attempt fails; completed horizontal lines clear, remaining cells settle by cleared rows, and the score increases according to the number of lines cleared together.

A blocked spawn or a lock extending above the visible board ends the game. Game over freezes play and clearly offers restart. Restart clears the board and score and begins a fresh game. Controls and score remain visible beside or near the playfield. Regular pieces use distinguishable colors that exclude white.

## Scope and priorities

Deliver a coherent playable game with reliable input, legal piece placement, correct line removal, understandable scoring and a complete start-to-game-over-to-restart flow. Use independent uniform selection among the seven pieces, fixed gravity and straightforward rotation. Exact input bindings, timing, scoring and spawn rules belong in the specification.

Prioritize predictable behavior and a small maintainable implementation. Keep the rules independently testable from browser rendering, and use browser interaction plus visual evidence to check the playable result. No gameplay, build or browser verification has yet been performed.

## Constraints and proposed direction

The product runs entirely in the browser as static client assets, without a game server. The proposed implementation uses TypeScript, Canvas for the board and ordinary HTML for controls and status. This is an agent recommendation for acceptance with this brief; structural details remain open for architecture.

Follow the central [browser development policy](../../BROWSER_DEV_POLICY.md): Chromium is the primary target and managed Firefox the secondary target, with dedicated isolated installations and profiles. Headless interaction and screenshot coverage are sufficient for the intended game. Browser provisioning is an implementation prerequisite, not a requirement for preparing these documents.

## Non-goals

Multiplayer, network services, accounts, online leaderboards, touch controls, audio, saved game sessions and elaborate visual effects are outside the intended product. The game does not require progression through speed levels to provide a complete playable experience.

## Decisions for review

The board dimensions, keyboard-first use, TypeScript/Canvas direction, independent random selection, fixed gravity and simple rotation are proposed defaults. Detailed testable contracts will be established in the specification after the project direction is accepted.
