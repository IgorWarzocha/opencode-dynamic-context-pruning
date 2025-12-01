# AGENTS GUIDE
Repository: TypeScript ESM OpenCode plugin; strict tsconfig.
Dependencies: install with `npm install`.
Build: `npm run build` (clean + tsc + copy prompts).
Typecheck: `npm run typecheck`.
Tests: `npm test` (node --import tsx --test tests/*.test.ts).
Single test file: `node --import tsx --test tests/my.test.ts`.
Single test by name: `node --import tsx --test tests/my.test.ts --test-name-pattern "case"`.
Dev: `npm run dev` to run the plugin locally.
Linting: no lint script; rely on typecheck/tests.
Imports: use ESM, prefer named imports; mark type-only with `import type`.
Formatting: follow existing style (4-space indent, double quotes, no semicolons).
Types: strict mode enforced; avoid `any`; add explicit return types for exports/helpers.
Nullability: guard optional config values and early-return on disabled paths.
Error handling: prefer logging via `Logger` (info/debug/warn/error); swallow errors only when safe.
Config/state: merge config immutably; keep default protected tools unless intentionally overridden.
Prompts/assets: keep prompt files under `lib/prompts`; build copies them into dist.
External IO: use `fs/promises` when possible; ensure directories exist before writing.
Cursor/Copilot rules: none present; follow repo conventions here.
PR hygiene: avoid touching unrelated files; keep changes minimal and tested.
