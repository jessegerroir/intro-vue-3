## Quick orientation for AI coding agents

This is a small Vue 3 app scaffolded for Vite. Keep guidance short and specific so contributors can make quick, correct edits.

- Entry point: `index.html` mounts the app to `<div id="app">`. App bootstrap is in `src/main.js` which imports `src/App.vue` and `src/assets/main.css`.
- Build/dev: see `package.json` scripts. Use `npm run dev` to start Vite, `npm run build` to produce a production build, and `npm run preview` which runs `vite preview --port 4173`.
- Vite alias: `vite.config.js` defines `@` -> `src`. Prefer imports like `@/components/ReviewForm.vue` and `@/assets/images/...`.

Key patterns to follow (copy examples from these files):

- Single-file components use the Composition API with `<script setup>` (see `src/components/productDisplay.vue`, `reviewForm.vue`, `reviewList.vue`). Use `ref`, `reactive`, `computed`, `defineProps`, and `defineEmits` consistently.
- Props and emits: components declare props with `defineProps({ ... })` and emits with `defineEmits(['event-name'])`. Example: `productDisplay.vue` defines a `premium` prop and emits `add-to-cart`.
- Parent-child communication is event-based: `ReviewForm` emits `review-submitted` and `ProductDisplay` listens and pushes reviews into a reactive `reviews` array (see `productDisplay.vue`). Follow this pattern rather than introducing a global store.
- Template binding: use `v-model.number` for numeric input conversion (example in `reviewForm.vue`), `v-for` with `:key`, and `v-bind:src` / `:style` for dynamic attributes.

Project-specific gotchas

- Filename case: components in `src/components/` are named with lowercase filenames (e.g., `productDisplay.vue`, `reviewForm.vue`) but imports in code use PascalCase filenames (e.g., `@/components/ProductDisplay.vue`). This works on Windows (case-insensitive filesystem) but will break on case-sensitive systems (macOS/Linux). Prefer matching the file name exactly when adding or renaming files.
- No router / state management libs: there's no Vue Router or Vuex/Pinia configured. If you need cross-page or global state, add explicit instructions and install dependencies — do not assume a store exists.
- Minimal dev tooling: there is no linter, test runner, or CI config present. Avoid adding infra changes without opening a PR with rationale.

Where to look when changing behavior

- UI and data flow: `src/App.vue` (holds `cart` state) -> `src/components/productDisplay.vue` (product UI, reviews array, emits events) -> `src/components/{reviewForm,reviewList}.vue`.
- Styling: `src/assets/main.css` and images under `src/assets/images/`.
- Vite config / alias: `vite.config.js`.

Practical editing rules for agents

- Keep changes small and focused. Update components using the same `<script setup>` style and Composition API.
- When adding imports, use the `@/` alias.
- Match filename casing to the import on non-Windows systems; prefer PascalCase component filenames (e.g., `ProductDisplay.vue`) to align with imports.
- When adding a new event or prop, mirror the existing event names and payload shapes (see `add-to-cart` which emits a single `id`, and `review-submitted` which sends an object `{ name, content, rating }`).

If unsure, run locally:

```bash
# start dev server
npm run dev
# build
npm run build
# preview build on port 4173
npm run preview
```

Ask the repo owner before adding linters, test frameworks, or changing the project layout.

---
If any section is unclear or you want more examples from specific files, ask and I'll expand the instructions.
