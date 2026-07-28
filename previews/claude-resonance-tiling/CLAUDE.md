# easy-calculators — instructions for Claude

This repo is a collection of self-contained, static HTML physics calculators
(superconducting-qubit / microwave physics focus) published as a GitHub Page.
**No build step, no framework, no backend.** Every page is a single `.html`
file served as-is.

## Repo layout

- `index.html` — landing page. One `<div class="card"><a href="…">` entry per calculator.
- One `*.html` file per calculator at the repo root. Each file is fully
  self-contained: inline `<style>`, inline `<script>`, and a "Back" card
  linking to `index.html`.
- A few calculators ship with companion `.csv` data files (e.g.
  `qubit-feedline-t1-bem.csv`, `qubit-feedline-t1-fem.csv`) loaded at runtime
  via `fetch`.
- External libraries are pulled via CDN only when actually needed — typically
  `mathjs` and `plotly` for the more complex pages (see `transmon.html`,
  `qubit-feedline-t1-bem.html`). Don't add a CDN dependency if plain JS suffices.
- No `README` content, no `package.json`, no tests, no CI.

## Style conventions (match these in new calculators)

- Light background (`#f0f0f0` or `#f0f4f8`), white rounded card container,
  `max-width` ~500–660 px, simple green (`#4CAF50`) or blue (`#007bff`) button.
- Inputs via `<label>` + `<input type="number" step="any">`. For quantities
  that span many orders of magnitude, add an SI-prefix `<select>` next to the
  input (pattern from `energy.html` / `transmission.html`).
- Results rendered into a `<div class="result">` via `innerHTML`, with
  `toExponential(6)` for numeric values.
- Physical constants hard-coded at the top of the script block
  (`h = 6.62607015e-34`, `c = 2.99792458e8`, `kB = 1.380649e-23`,
  `eV_J = 1.602176634e-19`, …). Use CODATA values.
- More involved calculators may add Plotly charts, inline SVG sketches,
  regime badges, and CSV-backed interpolation — follow `transmon.html` or
  `qubit-feedline-t1-bem.html` as templates when that complexity is needed.
- Every calculator page ends with a "Back" card linking to `index.html`:

  ```html
  <div class="card">
    <a href="index.html">Back</a>
  </div>
  ```

## Recipe for adding a new calculator

1. Create `new-calc.html` at the repo root. Pick a template by complexity:
   - Simple converter → copy the structure of `transmission.html` or `energy.html`.
   - Plot + several inputs → copy the structure of `transmon.html`.
   - CSV-backed interpolation with plots → copy `qubit-feedline-t1-bem.html`.
2. Keep the file self-contained (inline CSS + JS). Only add CDN `<script>`
   tags for libraries you actually use.
3. Add a `<div class="card">` entry linking to the new file in `index.html`.
4. Include the "Back" card at the bottom of the new page.
5. If the calculator needs reference data, ship it as a sibling `.csv`
   loaded via `fetch` (same pattern as the T1 estimators).

## What not to do

- Don't introduce a build step, bundler, framework, or package manager.
- Don't refactor existing calculators into shared CSS/JS files unless the
  user explicitly asks — the self-contained-per-page structure is intentional
  and keeps each calculator independently viewable / hackable.
- Don't remove unlinked legacy pages (e.g. `qubit-feedline-t1.html` is not
  linked from `index.html` but is kept on disk on purpose).
- Don't add emojis to UI or code unless the user asks.
