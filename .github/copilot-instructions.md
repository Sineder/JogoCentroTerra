<!-- .github/copilot-instructions.md - guidance for AI coding agents working on this repo -->
# Copilot instructions for Viagem ao Centro da Terra

Purpose: help AI coding agents be immediately productive in this small, single-file static game.

- **Big picture:** This is a standalone, static HTML5 educational game. The entire app lives in `index.html` (structure, styles, and JavaScript). The pedagogical rationale and design decisions are in `GDD.md` and summarized in [README.md](README.md).

- **Entry points:**
  - Primary: [index.html](index.html) — modify here for UI, styles, and game logic.
  - Reference: [README.md](README.md) and `GDD.md` — contains learning goals and constraints.

- **Architecture & data flow (what to read first):**
  - Global state object `jogo` (near top of the inline `<script>`) stores the whole game state (selected character, current phase, progress arrays).
  - Static data structures such as `personagens` and `camadasTerra` define all content-driven entities (phases, elements, fossils). Change content by editing these objects.
  - UI rendering is done by imperative DOM updates inside the same `<script>` block. Key DOM anchors: `#game-container`, `#conteudo-fase` (phase content), `#progresso-bar` (progress UI), and `#modal-*` modals.

- **Conventions & patterns to follow (project-specific):**
  - Single-file approach: prefer editing `index.html` rather than splitting into many files unless adding a clear benefit (explain it in PR description).
  - Portuguese variable names and text (e.g., `camadasTerra`, `mostrarSelecaoPersonagem`) — keep naming consistent and user-facing text in pt-BR.
  - CSS variables are defined in `:root`. Theme toggles (alto contraste) are implemented by adding/removing the `.alto-contraste` class on `body`.
  - UI visibility uses `.tela` / `.tela.ativa` classes. Show/hide by toggling the `ativa` class rather than changing inline `display` everywhere.
  - Accessibility: interactive elements include `tabindex` and keyboard handlers (e.g., `onkeypress` using Enter). Maintain these when adding new interactive widgets.
  - Modals: use `.modal` and toggle `.ativo` for visibility; keep `tabindex` and a close button for keyboard users.

- **Persistence & run workflow:**
  - The project uses `localStorage` to save progress (see `carregarJogoSalvo()` and `recomecarJogo()` used in the markup). Search `index.html` for those functions when adjusting save/load logic.
  - No build step: run locally by opening `index.html` in a browser. For iterative development use a static server (e.g., `npx http-server .` or VS Code Live Server) to avoid CORS / file reload quirks.

- **When changing content (practical examples):**
  - To add a new fossil in phase 1, add an object to `camadasTerra[1].elementos`, for example:

    { id: "fossil-novo", nome: "Fóssil X", emoji: "🦴", info: "Breve descrição." }

    Then update the renderer that writes into `#conteudo-fase` (search for the function that iterates `camadasTerra` elements).

- **When adding logic / functions:**
  - Keep new functions in the same `<script>` block near the top-level state for clarity.
  - Prefer small, pure rendering helpers that accept data and update DOM anchors (easier to reason about than many scattered inline handlers).
  - Respect existing inline event handlers used in markup (e.g., `onclick="abrirOpcoes()"`). If refactoring to add event listeners, update the markup and keep behaviour identical.

- **Styling patterns:**
  - Use existing utility classes (`texto-centro`, `mt-20`, `escondido`) and CSS variables rather than adding new hard-coded colors or fonts.
  - Follow the naming used in the file and place additions in the same `<style>` block.

- **Testing & debugging tips:**
  - Open `index.html` in Chrome/Edge and use DevTools console. The app stores runtime state in `jogo` — inspect and mutate it to test flows.
  - Watch for `localStorage` keys (search file) when testing save/load. Clear storage with `localStorage.clear()` if needed.

- **Files to review for context:**
  - [index.html](index.html) — main source of truth
  - [README.md](README.md) — usage, learning goals, and constraints
  - `GDD.md` — pedagogical / content decisions (open to confirm curricular requirements)

If anything here is unclear or you want additional examples (render function snippets, where to place unit-test harnesses, or a small refactor plan to split JS into modules), tell me which part you want expanded.
