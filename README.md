# DE-TETRIS Design & Build Pack

This ZIP contains the visual concept, reusable UI assets, team avatars, design tokens and the full autonomous Hermes Agent build/deployment specification.

## Key files
- `HERMES_AGENT_SPEC.md` — primary build instruction
- `assets/design/de-tetris-concept-board.png` — visual direction
- `assets/design/de-tetris-logo.svg` — title artwork
- `assets/design/favicon.svg` — app icon
- `assets/design/design-tokens.json` — colours/fonts/tile settings
- `assets/icons/` — Data Engineering themed block icons
- `assets/avatars/` — team images prepared for UI/game tiles

The supplied Data Engineering icons are deliberately lightweight themed artwork suitable for the game UI rather than exact reproductions of vendor brand assets. If exact corporate/vendor logos are required for public release, replace them with approved official assets under the applicable brand guidelines.

## Running the game

```bash
npm install
npm test
npm run check
npm run dev
```

Production: https://de-tetris.ansellweb.com

Deploy with `npm run deploy`. Gameplay is client-side and the best score/settings are stored locally in the browser. Keyboard controls: arrows move/drop, Space hard-drops, Up/X rotates clockwise, Z rotates anticlockwise, C/Shift holds, P/Escape pauses, R restarts.
