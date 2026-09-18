# DE-TETRIS — Hermes Agent Autonomous Build & Deployment Specification

## 1. Objective

Build and deploy a polished Tetris-style browser game at **https://de-tetris.ansellweb.com** using **Cloudflare Workers**. The game is a Data Engineering themed internal/fun experience based on the supplied visual reference and packaged design assets.

The finished app must feel like a modern neon data-engineering control room: dark navy background, cyan grid, bright tetrominoes, glass-like panels, animated glow and subtle datacentre ambience.

The agent is authorised to create, modify, test, deploy and iterate the application autonomously, including Cloudflare Worker code, static assets, DNS/route configuration for the subdomain, build tooling, storage bindings and deployment configuration required for this project.

## 2. Deployment Boundary

The autonomous agent may operate only within the resources needed for **de-tetris.ansellweb.com** and its associated Cloudflare Worker/project. It must not alter unrelated DNS records, Workers, websites or zones.

Use a Cloudflare API token or `wrangler login`; never hard-code credentials into source control.

Recommended Cloudflare token permissions:
- Account / Workers Scripts: Edit
- Account / Workers KV Storage: Edit if KV is used
- Account / Durable Objects: Edit if Durable Objects are used
- Zone / DNS: Edit for `ansellweb.com`
- Zone / Workers Routes: Edit
- Zone / Zone: Read

## 3. Technology

Preferred implementation:
- Cloudflare Workers + Workers Static Assets
- TypeScript
- Vite for frontend build
- HTML5 Canvas for gameplay
- CSS for surrounding UI/HUD
- Wrangler for local dev and deployment
- Optional Durable Object or KV for leaderboard persistence

Keep the game playable without a backend dependency. Gameplay, scoring and controls must work fully client-side.

## 4. Core Gameplay

Implement authentic falling-block gameplay inspired by classic tetromino games:
- 10 × 20 visible playfield
- 7 tetrominoes: I, O, T, S, Z, J, L
- 7-bag randomisation
- clockwise and anti-clockwise rotation
- wall-kick handling
- soft drop
- hard drop
- hold piece
- next-piece queue (minimum 3)
- ghost piece
- lock delay
- increasing gravity by level
- line clear animations
- scoring for singles, doubles, triples and four-line clears
- combo bonus
- pause/restart
- game-over state
- responsive sizing

Keyboard:
- Left/Right arrows: move
- Down: soft drop
- Space: hard drop
- Up or X: rotate clockwise
- Z: rotate anticlockwise
- C or Shift: hold
- P or Escape: pause
- R: restart

Touch:
- left/right buttons
- rotate
- soft drop
- hard drop
- hold
- pause

## 5. Data Engineering Theme

Each tetromino uses one of the supplied themed tile identities. Use the packaged SVG icons and team avatars within individual block cells.

Recommended themes:
- Databricks — red
- Azure Data Factory — cyan
- VS Code — blue
- SQL Server — electric blue
- Azure / cloud — blue/cyan
- Database / SQL — purple
- Team / people — green

Every tetromino should use a consistent icon/category across all four cells rather than mixing icons inside one piece.

Occasionally spawn a **Team Piece** using a team avatar texture. Avatar pieces are cosmetic only and must not change gameplay rules.

## 6. Team References

Supplied avatar assets:
- Trevor
- Harry
- Shane
- Steve

Use the portraits in:
- rotating “Team” panel
- rare team-avatar tetromino skins
- game-over/high-score celebratory UI
- optional attract/demo screen

Do not distort faces. Keep portrait tiles square/rounded-square.

## 7. Visual Language

Follow the packaged concept board:
- deep navy base
- neon cyan grid lines
- luminous borders and subtle bloom
- panels with translucent dark surfaces
- white headings with cyan highlights
- multi-colour tetrominoes
- restrained animations, not excessive flashing

Primary game title:
**DE-TETRIS**

Primary strapline:
**STACK DATA • BUILD SOLUTIONS • CLEAR TOMORROW**

Secondary phrases may include:
- Same data. Brighter outcomes.
- People + Data = Progress.
- From data to impact.
- Plan together. Deliver together.

Avoid copying the supplied NXT Planning reference literally. DE-TETRIS should be its own game while sharing the visual family.

## 8. UI Layout

Desktop:
- Header / title strip
- left column: score, lines, level, elapsed time, high score
- centre: game board
- right column: next queue, hold, team panel, controls
- footer: short Data Engineering slogans and build/version text

Mobile:
- title at top
- game board central and dominant
- compact score row
- hold/next beside or above board
- fixed touch controls at bottom
- prevent accidental page scrolling during active play

## 9. Sound

Implement optional subtle sounds:
- move
- rotate
- soft/hard drop
- line clear
- level up
- game over

Default volume low. Include sound on/off toggle and persist choice in localStorage.

## 10. Leaderboard

Implement a local leaderboard first using localStorage.

If deployment credentials allow, add a global top-10 leaderboard using a Durable Object or KV-backed Worker API.

Fields:
- player name (max 20 chars)
- score
- lines
- level
- timestamp

Sanitise all submitted names. Rate-limit global score submission. Never trust the client for privileged operations.

## 11. Easter Eggs

Add lightweight Data Engineering references:
- “Pipeline healthy” on line clears
- “MERGE complete” on four-line clear
- “No schema drift detected” on perfect clear
- “Deploy to PROD?” before starting level 10
- rare “ADF retry succeeded” toast after recovering from a near-top board

Keep these humorous but unobtrusive.

## 12. Accessibility

- WCAG-aware contrast
- reduced-motion support
- keyboard playable end-to-end
- visible focus states
- text alternatives for non-decorative images
- no reliance on colour alone to identify tetrominoes
- optional high-contrast mode

## 13. Performance

Targets:
- first load under 2 MB where practical
- 60 FPS gameplay on normal modern desktop/mobile browsers
- no framework-heavy runtime required
- lazy-load nonessential sounds/large artwork
- cache immutable assets aggressively

## 14. Asset Map

Use these packaged files:
- `assets/design/de-tetris-concept-board.png`
- `assets/design/de-tetris-logo.svg`
- `assets/design/favicon.svg`
- `assets/design/grid-pattern.svg`
- `assets/design/design-tokens.json`
- `assets/icons/*.svg`
- `assets/avatars/*-tile.png`
- `assets/avatars/*.png`

The PNG concept board is reference art only and should not be used as the functional UI background.

## 15. Suggested Source Layout

```text
de-tetris/
  src/
    game/
      board.ts
      pieces.ts
      bag.ts
      scoring.ts
      input.ts
      renderer.ts
      audio.ts
    ui/
      hud.ts
      leaderboard.ts
      settings.ts
    worker/
      api.ts
    main.ts
    styles.css
  public/
    assets/
  index.html
  wrangler.jsonc
  package.json
  tsconfig.json
  vite.config.ts
```

## 16. Cloudflare Deployment

The agent should:
1. Create the Worker project.
2. Copy packaged assets into `public/assets`.
3. Implement and run unit tests for game logic.
4. Run Vite build.
5. Run `wrangler dev` and perform smoke testing.
6. Deploy with Wrangler.
7. Configure `de-tetris.ansellweb.com` as a Worker custom domain/route.
8. Verify HTTPS response.
9. Verify desktop and mobile layouts.
10. Verify keyboard and touch controls.
11. Verify leaderboard persistence.
12. Verify no console errors.
13. Record the deployed Worker name, current version/commit and deployment date in `DEPLOYMENT.md`.

Suggested Worker name: `de-tetris`.

## 17. Acceptance Criteria

The project is complete when:
- `https://de-tetris.ansellweb.com` loads successfully over HTTPS.
- A full game can be played from start to game-over.
- Tetromino movement/rotation/hold/next/ghost are functioning.
- Score, lines and level are accurate.
- Data Engineering iconography is clearly visible in the blocks.
- All four supplied team members appear somewhere in the experience.
- UI visually follows the provided neon Data Engineering concept.
- Desktop and mobile layouts are usable.
- Keyboard and touch controls work.
- Page refresh does not lose user settings or local high scores.
- No secrets are committed.
- Worker deployment is reproducible with a documented `npm run deploy`.

## 18. Autonomous Agent Instruction

Proceed without waiting for confirmation for ordinary implementation choices. Prefer the simplest reliable architecture. If a Cloudflare API permission is missing, report the exact missing permission and continue with all work that can be completed locally.

Do not broaden scope beyond the DE-TETRIS project or modify unrelated Cloudflare resources.
