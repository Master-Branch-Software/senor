# Code Style and Quality
Code that works today is not code that survives. What separates a codebase that compounds into velocity from one that compounds into debt is the discipline applied to everything around the logic — formatting, naming, error handling, logging, imports, dependencies, commits, reviews. This chapter is the cross-cutting reference for those disciplines. Language-specific rules live in `04-css-and-layout.md` (CSS), `07-javascript-and-html.md` (JavaScript, TypeScript, HTML), and `08-stack-and-architecture.md` (architecture, naming within stacks); this chapter covers everything that spans them.
## Philosophy
- Boring consistency beats clever variation. The goal is a codebase that reads as if one person wrote it.
- Automate every rule that can be automated. Humans review intent; machines review form.
- Fail fast, fail loud, fail specific. Silent fallbacks and empty `catch` blocks are how production incidents happen.
- Every configuration file is code. Check it in, version it, review it.
- Delete dead code. Commented-out code is dead code with pretensions.
## Formatting toolchain
Formatting is a solved problem. Pick one formatter, commit its config, run it on save and in CI. Never debate style in a pull request.
### Prettier
Prettier is the default for JavaScript, TypeScript, CSS, JSON, Markdown, YAML, and HTML. Intentionally near-zero configuration.
Canonical `.prettierrc.json`:
```json path=null start=null
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "always",
  "bracketSpacing": true,
  "endOfLine": "lf",
  "plugins": ["prettier-plugin-tailwindcss"]
}
```
- `trailingComma: "all"` is the Prettier 3 default and produces cleaner diffs when items are added to arrays, objects, or function signatures.
- `printWidth: 100` balances legibility against diff noise. 80 is still defensible for prose-heavy codebases; above 120 is a code smell.
- `endOfLine: "lf"` combined with a `.gitattributes` entry (see below) prevents line-ending wars between macOS, Linux, and Windows contributors.
- `prettier-plugin-tailwindcss` sorts Tailwind utility classes into Tailwind's recommended order. Install it whenever Tailwind is in use.
Canonical `.prettierignore`:
```text path=null start=null
node_modules
dist
build
coverage
.next
.nuxt
.svelte-kit
*.min.js
*.min.css
pnpm-lock.yaml
package-lock.json
yarn.lock
bun.lockb
```
Package.json scripts:
```json path=null start=null
{
  "scripts": {
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  }
}
```
CI must run `format:check` — never `format:write`. CI never modifies code.
### Biome — the alternative
Biome is a Rust-based formatter and linter that replaces Prettier + ESLint with a single binary. It is 20–30× faster and has a single config (`biome.json`). Adopt it when:
- Starting a greenfield project with no dependency on ESLint plugins Biome does not yet cover (`eslint-plugin-react-hooks/exhaustive-deps` is the most common gap).
- Pre-commit and CI lint time is a measurable developer-experience problem.
- The stack is TypeScript-first without exotic framework plugins.
Keep ESLint + Prettier when:
- `react-hooks/exhaustive-deps` is enforced (only `eslint-plugin-react-hooks` catches this).
- The project depends on Storybook, testing-library, or accessibility plugins that Biome does not yet fully replicate.
- An existing ESLint config encodes custom organizational rules.
A hybrid is possible — Biome for formatting and most linting, a minimal ESLint config for hook rules — but every additional tool is a maintenance cost.
### EditorConfig
`.editorconfig` at the repository root normalizes indentation, line endings, final newlines, and whitespace across editors. Every editor either reads it natively or has a plugin. It is the floor below formatter configs.
Canonical `.editorconfig`:
```ini path=null start=null
root = true
[*]
charset = utf-8
end_of_line = lf
indent_style = space
indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true
[*.md]
trim_trailing_whitespace = false
[Makefile]
indent_style = tab
```
- `root = true` prevents EditorConfig from climbing parent directories.
- Markdown preserves trailing whitespace because two trailing spaces create a hard line break.
- Makefiles require tabs; the rule is syntactic, not stylistic.
## Linting
Linting catches bugs, not style. Formatter handles spaces and semicolons; linter handles `==` vs `===`, unused variables, floating promises, accessibility violations, dependency-array omissions.
### ESLint flat config (v9+)
ESLint 9 made flat config (`eslint.config.mjs`) the default. ESLint 10 (shipped 2026) removed the legacy `.eslintrc.*` formats entirely. All new projects must use flat config.
Canonical `eslint.config.mjs` for a TypeScript + React project:
```js path=null start=null
import js from '@eslint/js';
import tseslint from 'typescript-eslint';
import react from 'eslint-plugin-react';
import reactHooks from 'eslint-plugin-react-hooks';
import jsxA11y from 'eslint-plugin-jsx-a11y';
import importPlugin from 'eslint-plugin-import';
import simpleImportSort from 'eslint-plugin-simple-import-sort';
import prettier from 'eslint-config-prettier/flat';
import globals from 'globals';
export default tseslint.config(
  { ignores: ['dist/**', 'build/**', 'coverage/**', '.next/**', 'node_modules/**'] },
  js.configs.recommended,
  ...tseslint.configs.recommendedTypeChecked,
  {
    languageOptions: {
      parserOptions: {
        projectService: true,
        tsconfigRootDir: import.meta.dirname,
      },
      globals: { ...globals.browser, ...globals.node },
    },
    rules: {
      '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_', varsIgnorePattern: '^_' }],
      '@typescript-eslint/consistent-type-imports': ['error', { prefer: 'type-imports' }],
      '@typescript-eslint/no-floating-promises': 'error',
      '@typescript-eslint/no-misused-promises': 'error',
      '@typescript-eslint/await-thenable': 'error',
      '@typescript-eslint/no-explicit-any': 'warn',
      'eqeqeq': ['error', 'always'],
      'no-console': ['warn', { allow: ['warn', 'error'] }],
      'no-var': 'error',
      'prefer-const': 'error',
    },
  },
  {
    files: ['**/*.{jsx,tsx}'],
    plugins: { react, 'react-hooks': reactHooks, 'jsx-a11y': jsxA11y },
    rules: {
      ...react.configs.recommended.rules,
      ...react.configs['jsx-runtime'].rules,
      ...reactHooks.configs.recommended.rules,
      ...jsxA11y.configs.recommended.rules,
      'react/prop-types': 'off',
      'react/react-in-jsx-scope': 'off',
      'react-hooks/exhaustive-deps': 'error',
    },
    settings: { react: { version: 'detect' } },
  },
  {
    plugins: { import: importPlugin, 'simple-import-sort': simpleImportSort },
    rules: {
      'simple-import-sort/imports': 'error',
      'simple-import-sort/exports': 'error',
      'import/first': 'error',
      'import/newline-after-import': 'error',
      'import/no-duplicates': 'error',
    },
  },
  { files: ['**/*.{test,spec}.{ts,tsx}'], rules: { '@typescript-eslint/no-explicit-any': 'off' } },
  prettier,
);
```
Rules of engagement:
- `eslint-config-prettier` must be last. It disables every ESLint formatting rule, leaving Prettier as the only formatting authority.
- Type-aware rules (`recommendedTypeChecked`) require `projectService: true`. They are 3–10× slower but catch classes of bug that syntactic rules cannot (floating promises, unsafe `any` flow, mis-used promises). Enable them in CI; optionally run them less aggressively locally via `--cache`.
- Treat `react-hooks/exhaustive-deps` as `error`, not `warn`. It catches stale-closure bugs that are otherwise invisible until production.
- ESM-first: `eslint.config.mjs` (or `.js` with `"type": "module"`). Use `import.meta.dirname` for the TypeScript project root (requires Node 20.11+).
### typescript-eslint
Install the unified package, not the legacy split packages:
```text path=null start=null
@typescript-eslint/eslint-plugin  →  deprecated
@typescript-eslint/parser         →  deprecated
typescript-eslint                 →  use this
```
Prefer `recommendedTypeChecked` over `recommended`. The performance cost is real; the bug-catching power is larger. If CI time becomes a problem, split lint into two jobs: syntactic (fast) and type-aware (slower, runs in parallel).
### Stylelint
CSS, SCSS, and CSS-in-JS also need linting. Canonical `stylelint.config.js`:
```js path=null start=null
/** @type {import('stylelint').Config} */
export default {
  extends: [
    'stylelint-config-standard',
    'stylelint-config-clean-order',
  ],
  plugins: ['stylelint-order'],
  rules: {
    'selector-class-pattern': '^[a-z][a-z0-9-]*(__[a-z0-9-]+)?(--[a-z0-9-]+)?$',
    'max-nesting-depth': 3,
    'selector-max-specificity': '0,3,0',
    'declaration-no-important': true,
    'no-duplicate-selectors': true,
  },
};
```
- `stylelint-config-clean-order` sorts properties into logical groups (positioning → layout → box model → typography → appearance → transition). `stylelint-config-recess-order` is an alternative.
- `selector-class-pattern` enforces kebab-case, optionally with BEM `__element` and `--modifier` segments.
- `max-nesting-depth: 3` is the ceiling, not the target. Beyond three levels, extract a component.
- `selector-max-specificity: '0,3,0'` caps at three classes. ID selectors and `!important` are flagged as errors.
### Accessibility linting
`eslint-plugin-jsx-a11y` catches the most common JSX accessibility mistakes (missing `alt`, non-interactive elements with click handlers, labels without `htmlFor`). It does not replace manual audits — see `05-accessibility.md` — but it prevents an entire class of regression.
## Repository hygiene
Every repository should start with the same baseline files. Add them before the first feature commit.
### `.gitignore`
Language-agnostic baseline:
```text path=null start=null
# Dependencies
node_modules/
.pnp.*
.yarn/*
!.yarn/releases
# Build output
dist/
build/
out/
.next/
.nuxt/
.svelte-kit/
.astro/
.output/
# Tests and coverage
coverage/
.nyc_output/
# Caches
.cache/
.eslintcache
.stylelintcache
.turbo/
.parcel-cache/
# Environment and secrets
.env
.env.local
.env.*.local
*.pem
# Editor and OS
.vscode/*
!.vscode/settings.json
!.vscode/extensions.json
!.vscode/launch.json
.idea/
.DS_Store
Thumbs.db
# Logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
```
- `.DS_Store` must be ignored. macOS creates these files in every directory the Finder visits.
- Keep `.vscode/settings.json`, `.vscode/extensions.json`, and `.vscode/launch.json` checked in. Ignore the rest.
- `.env` is ignored; `.env.example` is committed.
### `.gitattributes`
Normalizes line endings across operating systems and marks binary files so git does not attempt diffs on them:
```text path=null start=null
* text=auto eol=lf
*.sh text eol=lf
*.bat text eol=crlf
*.png binary
*.jpg binary
*.jpeg binary
*.gif binary
*.ico binary
*.pdf binary
*.woff2 binary
# Exclude from GitHub linguist stats
/docs/** linguist-documentation
/**/*.test.* linguist-generated=false
```
### `.nvmrc` (or `.node-version`)
Pins the Node.js version for anyone using nvm, fnm, Volta, or asdf. One file, one line:
```text path=null start=null
22.13.0
```
Commit `pnpm-lock.yaml`, `package-lock.json`, `yarn.lock`, or `bun.lockb` (whichever the project uses). Pin the package manager in `package.json`:
```json path=null start=null
{
  "packageManager": "pnpm@10.4.0",
  "engines": {
    "node": ">=22.13.0",
    "pnpm": ">=10.0.0"
  }
}
```
Corepack (Node 16.9+) reads `packageManager` and uses the correct package manager automatically. No global installs required.
### `.vscode/settings.json`
Commit editor settings that affect the code shape. Keep personal preferences (fonts, themes) out.
```json path=null start=null
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit",
    "source.fixAll.stylelint": "explicit"
  },
  "eslint.useFlatConfig": true,
  "eslint.validate": ["javascript", "javascriptreact", "typescript", "typescriptreact"],
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,
  "files.eol": "\n",
  "files.insertFinalNewline": true,
  "files.trimTrailingWhitespace": true,
  "[markdown]": {
    "files.trimTrailingWhitespace": false
  }
}
```
### `.vscode/extensions.json`
Recommend the extensions the project relies on:
```json path=null start=null
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "stylelint.vscode-stylelint",
    "bradlc.vscode-tailwindcss",
    "editorconfig.editorconfig"
  ]
}
```
VS Code prompts new contributors to install them when they open the project.
### `.github/pull_request_template.md`
```markdown path=null start=null
## Summary
<!-- One paragraph describing the change and why it is needed. -->
## Changes
<!-- Bullet list of the concrete changes. -->
## Testing
<!-- How was this verified? Tests added? Manual steps? -->
## Checklist
- [ ] Tests added or updated
- [ ] Documentation updated (README, ARCHITECTURE, inline comments)
- [ ] Accessibility checked where UI changed
- [ ] No new ESLint, TypeScript, or Stylelint errors
- [ ] Linked to the tracking issue or ticket
## Screenshots / recordings
<!-- Before/after for UI changes. -->
```
### `CODEOWNERS`
`.github/CODEOWNERS` routes pull-request reviews automatically. Ownership is by file path; the last matching pattern wins.
```text path=null start=null
# Default owners for everything
*                           @org/platform
# Frontend
/apps/web/**                @org/frontend
/packages/ui/**             @org/frontend @org/design
# Backend
/apps/api/**                @org/backend
/packages/db/**             @org/backend @org/data
# Security-sensitive
/apps/api/src/auth/**       @org/security
/.github/workflows/**       @org/platform @org/security
SECURITY.md                 @org/security
# Infra
/infra/**                   @org/platform
```
Enable "Require review from Code Owners" in branch protection. Combined with required reviews, this prevents merges to sensitive paths without the right eyes.
## Naming conventions
Language-specific details live in `07-javascript-and-html.md` and `08-stack-and-architecture.md`. The cross-cutting rules:
- Variables, functions, methods — `camelCase`.
- Classes, types, interfaces, enums, React components — `PascalCase`.
- Constants (module-scope literals that never change) — `SCREAMING_SNAKE_CASE`. Regular immutable bindings are `camelCase`.
- File names — `kebab-case.ts` for modules, `PascalCase.tsx` for React components only if the project convention demands it; otherwise `kebab-case.tsx`. Pick one and stick to it.
- Directories — `kebab-case`.
- CSS classes — `kebab-case`, optionally BEM (`block__element--modifier`).
- Boolean variables — drop the `is` prefix: `visible`, `active`, `disabled`, `loading`, `dirty`.
- Setters — `setX`: `setVisible`, `setActive`.
- Getters — no `get` prefix: `id()`, `name()`, `width()`.
- Hooks — `use` prefix: `useAuth`, `useDebounce`.
- Event handlers — `handle*` for internal, `on*` for prop names: `handleClick` / `onClick`.
- Async functions — no `Async` suffix. A function returning `Promise<T>` is obvious from its signature.
- Types and interfaces — no `I`, `T`, or `Type` prefix. `User`, not `IUser` or `TUser`. Enums — no `E` prefix.
- Test files — `foo.test.ts` or `foo.spec.ts`. Pick one per project.
- Private members — leading underscore only when the language lacks a real private keyword. TypeScript has `private`; use it.
## Imports and module organization
### Path aliases
Replace `../../../` with `@/`:
```json path=null start=null
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```
Bundlers need the same aliases. Vite has `vite-tsconfig-paths`; Next.js reads `tsconfig.json` directly; Vitest needs `vite-tsconfig-paths` as a plugin.
More granular aliases (`@/components/*`, `@/server/*`, `@/lib/*`) make the origin of every import visible and encode architectural boundaries at the import site.
### Import ordering
Let `eslint-plugin-simple-import-sort` handle it. The default ordering produces:
1. Side-effect imports (`import 'styles.css'`).
2. External packages (`react`, `lodash`).
3. Internal absolute imports (`@/components/...`).
4. Relative parent imports (`../utils`).
5. Relative sibling imports (`./types`).
6. Style imports.
A blank line between groups. Alphabetical within each group. No manual intervention.
### Named vs default exports
Prefer named exports. Rules:
- Default exports can be renamed at the import site, which fragments usage and breaks refactors.
- Lazy-loaded components (`React.lazy`, dynamic imports) require default exports. Use them only where the API demands it.
- Framework conventions override: Next.js page files, Remix route files, and similar require default exports. Follow the framework.
Named exports produce clearer auto-imports in IDEs, stable identifiers across the codebase, and fewer merge conflicts.
### Barrel files
A barrel is an `index.ts` that re-exports from a directory. They produce cleaner import paths at the cost of:
- Dead-code elimination. Many bundlers have trouble tree-shaking through deep barrel chains, which bloats production bundles.
- Circular imports. Barrels frequently cause import cycles that manifest as undefined values at runtime.
- Slower TypeScript and ESLint. Both tools must resolve everything a barrel exports, even when only one symbol is imported.
Use barrels sparingly. When used, keep them one level deep and never re-export from other barrels. For internal libraries, a single top-level `index.ts` is acceptable; nested barrels are a smell.
### ESM only
New code ships as ES modules. `package.json` sets `"type": "module"`. CommonJS is acceptable only for tooling files that must remain CommonJS (some config loaders) or for maintaining an older library.
## Comments and documentation
### When to comment
The default answer is "not yet." If the code needs a comment, the code might need to be clearer first.
Write comments when:
- The code does something surprising for a non-obvious reason (link to the ticket, the spec, or the benchmark).
- A workaround exists because of an external constraint (browser bug, vendor API quirk, regulatory requirement).
- A non-trivial algorithm is implemented (reference the paper or pseudocode).
- A public API exposes behavior that matters to callers.
Do not write comments that:
- Restate what the code does.
- Narrate what the author did ("Added this because…").
- Say "for now" or "temporary" without a ticket reference.
- Describe commented-out code. Delete the code; git remembers.
### TODO, FIXME, HACK, NOTE
Use a consistent prefix. Every marker includes an owner and an issue link. Unowned TODOs rot.
```ts path=null start=null
// TODO(alice, #1234): replace with native Temporal once Safari ships it
// FIXME(#891): off-by-one in pagination boundary
// HACK(#2019): rate-limit plugin interface is private; revisit on upgrade
// NOTE: this path is hit 10k rps; keep allocations minimal
```
`eslint-plugin-no-warning-comments` can fail the build on bare `TODO:` lines without an issue reference.
### JSDoc
Use JSDoc for every exported function, type, and constant in a public API or library. Keep it terse; let TypeScript carry the type information.
```ts path=null start=null
/**
 * Returns the first element of `arr` that satisfies `predicate`, or
 * `undefined` if none does. Equivalent to `Array.prototype.find` but
 * narrows the return type when `predicate` is a type guard.
 *
 * @example
 * const u = findUser(users, isAdmin);
 * //    ^? AdminUser | undefined
 */
export function findUser<T, S extends T>(
  arr: readonly T[],
  predicate: (value: T) => value is S,
): S | undefined;
```
Do not write JSDoc that duplicates the type signature. `@param name - The name of the user` adds nothing.
### Banning AI narration comments
AI-assisted coding tools frequently insert comments that narrate the code:
```ts path=null start=null
// Loop over users
for (const user of users) {
  // Increment the counter
  count++;
}
```
These are worse than no comments. Review them out on first pass. Add a review comment rule to the team guide: every comment must add information the code does not.
## Error handling
### A taxonomy
Distinguish four error shapes at API boundaries:
- Domain errors — expected business-level failures: email already in use, insufficient balance, invalid coupon. Not bugs. Part of the normal control flow.
- Validation errors — the input did not parse or conform to the schema. Not bugs. Return to the caller with field-level detail.
- Infrastructure errors — the database is down, the upstream API timed out, the queue is full. Transient. Retry-able.
- Unknown errors — anything the code did not anticipate. Log with full context, alert, and present a generic message to the user.
Each category has different logging, retry, and surfacing policies. A single `catch (err) { console.error(err) }` erases the distinction.
### throw vs Result
Two viable styles:
1. Throw for exceptional conditions, return normally for success. Familiar, works everywhere, plays with `async/await`.
2. Return a `Result<T, E>` or `Either<E, T>` for every fallible operation. Errors appear in the type signature; callers cannot forget to handle them. Popular with `neverthrow`, `effect`, or a hand-rolled type.
A reasonable blend: use `Result` at feature boundaries (API handlers, service methods that have business-meaningful failures), throw inside lower-level utilities. Do not mix the two within a single layer — that yields the worst of both.
Minimal `Result` pattern in TypeScript:
```ts path=null start=null
type Ok<T> = { ok: true; value: T };
type Err<E> = { ok: false; error: E };
export type Result<T, E> = Ok<T> | Err<E>;
export const ok  = <T>(value: T): Ok<T> => ({ ok: true, value });
export const err = <E>(error: E): Err<E> => ({ ok: false, error });
```
Or adopt `neverthrow` for the full API (`map`, `andThen`, `orElse`, `ResultAsync`).
### React error boundaries
Every React application needs an `ErrorBoundary` at the route level and at the top of each logically isolated section. Error boundaries catch render-time errors; they do not catch event-handler errors. Wrap async event handlers with an explicit `try/catch` and surface failures through a toast, banner, or inline error.
### Retry and timeout
Every network call needs a timeout. The platform `fetch` does not timeout by default:
```ts path=null start=null
const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 10_000);
try {
  const res = await fetch(url, { signal: controller.signal });
  // ...
} finally {
  clearTimeout(timeout);
}
```
Retry with exponential backoff and jitter on infrastructure errors only. Never retry on validation errors; never retry on 4xx responses other than 429 (with `Retry-After`) and 408. Bound the number of retries to avoid cascading failures.
### Never silently swallow
Empty `catch` blocks are a security and reliability bug. They hide real failures and produce the worst debugging experience possible.
```ts path=null start=null
// Never:
try { doWork(); } catch {}
// Always:
try {
  doWork();
} catch (error) {
  logger.error({ err: error, op: 'doWork' }, 'doWork failed');
  throw error; // or a typed wrapper
}
```
Enforce with ESLint:
```text path=null start=null
"no-empty": ["error", { "allowEmptyCatch": false }]
```
## Logging
### Structured JSON
Logs are machine-readable or they are useless at scale. Write JSON, not formatted text, and ship to an aggregator (Loki, Datadog, CloudWatch, Elastic, Sumo).
Pino is the default Node.js logger: structured JSON, asynchronous writes, Prettier-compatible development output via `pino-pretty`, first-class OpenTelemetry integration via `pino-opentelemetry-transport`.
Minimum configuration:
```ts path=null start=null
import pino from 'pino';
export const logger = pino({
  level: process.env.LOG_LEVEL ?? 'info',
  base: {
    service: process.env.SERVICE_NAME ?? 'unknown',
    env: process.env.NODE_ENV ?? 'development',
  },
  timestamp: pino.stdTimeFunctions.isoTime,
  redact: {
    paths: [
      'req.headers.authorization',
      'req.headers.cookie',
      '*.password',
      '*.token',
      '*.apiKey',
      '*.creditCard',
    ],
    censor: '[REDACTED]',
  },
  transport: process.env.NODE_ENV !== 'production'
    ? { target: 'pino-pretty', options: { colorize: true } }
    : undefined,
});
```
### Levels
- `fatal` — the process is about to exit.
- `error` — a request failed; user-visible impact.
- `warn` — unexpected but recoverable; rate-limit hit, cache miss, deprecated API used.
- `info` — routine business events: signup, login, payment processed. Aim for one `info` per significant event, not per function call.
- `debug` — only enabled for a specific diagnosis; off in production.
- `trace` — extremely verbose; off everywhere except during an investigation.
Set the production threshold to `info`. Expose a runtime control (admin endpoint, feature flag, env var) to raise it to `debug` during an incident without a redeploy.
### Required fields
Every log line should carry enough context to correlate with a request and a user:
- `service` — which service emitted the log.
- `env` — `production`, `staging`, `development`.
- `request_id` — correlation ID generated at ingress and propagated.
- `trace_id` / `span_id` — OpenTelemetry trace context (injected automatically by `pino-opentelemetry-transport`).
- `user_id` — after authentication.
- `tenant_id` — for multi-tenant systems.
Use child loggers to attach these automatically:
```ts path=null start=null
const requestLogger = logger.child({ request_id: req.id, user_id: req.user?.id });
requestLogger.info({ route: 'POST /orders' }, 'order received');
```
### Never log
- Passwords, hashed or plain.
- Full API tokens, session tokens, or refresh tokens.
- Credit card numbers, CVVs, or bank details.
- Personally identifiable information (PII) beyond what the log-retention policy allows.
- Request or response bodies that might contain any of the above.
Redaction lists in the logger config catch the common cases. A linter rule banning `console.log` in production code catches the rest.
### Sampling
At hundreds of thousands of requests per second, logging every one is expensive. Sample at the edge:
- 100% for errors and warnings.
- 1–10% for infos, depending on volume.
- Tail-based sampling in the OpenTelemetry collector keeps every trace that contains an error, regardless of head-sampling rate.
## Dependency management
### Pinning and lockfiles
- Commit the lockfile. Every installation deterministic by default.
- Pin exact versions for devDependencies that shape CI behavior (`eslint`, `typescript`, `prettier`, `vitest`, `playwright`). A minor bump in a linter can introduce hundreds of new errors.
- Use `^` for application runtime dependencies. Accept minor and patch updates; let Renovate propose majors.
- Use exact pins for security-critical libraries (`argon2`, `jsonwebtoken`, `zod`) when stability matters more than auto-upgrades.
### Renovate vs Dependabot
Both tools open pull requests for outdated dependencies. Defaults differ.
- Dependabot — built into GitHub, zero-config, one PR per package by default. Security alerts wired to the GitHub Advisory Database.
- Renovate — more configurable (400+ options), supports 90+ package managers, grouping, scheduled batches, regex managers for non-standard files (Dockerfiles, Terraform, CI configs), replacement PRs for deprecated packages, and presets shareable across repositories.
For small projects on GitHub: Dependabot is sufficient. For larger or multi-ecosystem repositories: Renovate. A sensible Renovate baseline:
```json path=null start=null
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["config:recommended", ":semanticCommits"],
  "schedule": ["before 9am on monday"],
  "prConcurrentLimit": 5,
  "minimumReleaseAge": "3 days",
  "packageRules": [
    {
      "matchUpdateTypes": ["minor", "patch"],
      "matchDepTypes": ["devDependencies"],
      "groupName": "devDependencies (non-major)",
      "automerge": true
    },
    {
      "matchUpdateTypes": ["patch"],
      "matchDepTypes": ["dependencies"],
      "groupName": "dependencies (patch)"
    },
    {
      "matchDepTypes": ["peerDependencies"],
      "enabled": false
    }
  ],
  "vulnerabilityAlerts": {
    "labels": ["security"],
    "schedule": ["at any time"]
  }
}
```
- `minimumReleaseAge: "3 days"` avoids pulling in broken releases that get patched within 24 hours.
- Auto-merge patch and dev-dependency updates after CI passes. Review minors and majors manually.
- Security updates bypass the schedule.
### Monorepo tooling
For monorepos: `pnpm` workspaces + Turborepo is the industry default in 2026. pnpm's strict isolation prevents phantom dependencies; Turborepo adds task caching and parallel execution.
```yaml path=null start=null
# pnpm-workspace.yaml
packages:
  - "apps/*"
  - "packages/*"
```
```json path=null start=null
// turbo.json
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**"]
    },
    "test": {
      "dependsOn": ["build"],
      "outputs": []
    },
    "lint": { "outputs": [] },
    "type-check": { "dependsOn": ["^build"], "outputs": [] },
    "dev": { "cache": false, "persistent": true }
  }
}
```
Alternatives worth knowing:
- Nx — full monorepo platform with code generators, module-boundary rules, affected-project analysis, and distributed task execution. Worth the learning curve for large enterprise monorepos (>20 packages, multiple teams).
- Moon — Rust-based polyglot runner with strict toolchain version management. Good fit when multiple languages live in the same repository.
- Lerna — legacy; use only for publish workflows in existing Lerna repos. Changesets is the modern replacement.
## Build toolchain
### Bundlers
- Vite — default for new applications (React, Vue, Svelte, Solid, Astro). In Vite 8 the Rolldown (Rust) bundler replaces Rollup for production builds, closing the speed gap with Rspack.
- Next.js — uses Turbopack (Rust) by default in 15+. Ship with what the framework provides.
- Rspack — drop-in webpack replacement in Rust. The migration tool handles 90–95% of a typical webpack config; the Rust core is 5–10× faster. Choose this when migrating a large existing webpack codebase.
- webpack — in maintenance mode; new projects should not start here. Existing projects can stay if they work.
### Library bundling
- tsdown — Rolldown-based, tsup-compatible API, fastest library bundler in 2026. The default for new libraries.
- tsup — still widely used, esbuild-based; tsdown is the spiritual successor.
- unbuild — used by the UnJS ecosystem (Nuxt, Nitro, h3). Rollup-based with a stub-mode that avoids the build step during development.
Every new library ships dual ESM/CJS output with generated `.d.ts` files. Canonical `tsdown.config.ts`:
```ts path=null start=null
import { defineConfig } from 'tsdown';
export default defineConfig({
  entry: ['src/index.ts'],
  format: ['esm', 'cjs'],
  dts: true,
  clean: true,
  sourcemap: true,
  exports: true,
});
```
### ESM only for new code
New libraries: publish ESM only unless a specific consumer requires CJS. Node ≥20 supports ESM natively; most modern tooling prefers it. For dual output, `tsdown` and `tsup` handle both formats automatically.
### Environment variables
- `.env` files for development; never committed.
- `.env.example` committed with keys only, no values.
- Runtime validation at startup with Zod or Valibot. Failing fast on a missing or malformed env var is always better than discovering it at 3am.
```ts path=null start=null
import { z } from 'zod';
const envSchema = z.object({
  DATABASE_URL: z.string().url(),
  NODE_ENV: z.enum(['development', 'test', 'production']),
  LOG_LEVEL: z.enum(['trace', 'debug', 'info', 'warn', 'error', 'fatal']).default('info'),
  PORT: z.coerce.number().int().positive().default(3000),
});
export const env = envSchema.parse(process.env);
```
For Next.js, `@t3-oss/env-nextjs` splits server and client variables and enforces the `NEXT_PUBLIC_` prefix at the type level.
### Source maps
Ship source maps to production. Private, authenticated source maps give error trackers (Sentry, Rollbar) usable stack traces without leaking internal code to the public.
## Git and VCS workflow
### Conventional Commits
Adopt the Conventional Commits specification. A consistent commit history is machine-readable (automatic changelogs, semantic versioning) and human-readable (every message states what and why).
```text path=null start=null
<type>(<optional scope>)<optional !>: <subject>
<blank line>
<optional body>
<blank line>
<optional footer>
```
Canonical types (from `@commitlint/config-conventional`): `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`. `feat` bumps MINOR. `fix` bumps PATCH. A `!` or a `BREAKING CHANGE:` footer bumps MAJOR.
Examples:
```text path=null start=null
feat(auth): add WebAuthn login
fix(api): handle null avatar URL in user serializer
refactor(ui): extract Button variants into CVA
docs: correct example in 02-brand-and-copywriting
chore(deps): bump vitest to 2.1.5
feat(api)!: rename /v1/users endpoint to /v1/accounts
BREAKING CHANGE: /v1/users returns 410; migrate clients to /v1/accounts.
```
Subject rules: imperative mood ("add", not "added"), lowercase, no trailing period, under 72 characters.
Enforce with `commitlint`:
```js path=null start=null
// commitlint.config.js
export default {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'subject-case': [2, 'always', 'lower-case'],
    'subject-max-length': [2, 'always', 72],
    'body-max-line-length': [2, 'always', 100],
  },
};
```
### Branch naming
Short, descriptive, kebab-case, optionally prefixed by type and ticket:
```text path=null start=null
feat/1234-webauthn-login
fix/2019-null-avatar
chore/bump-vitest
docs/branded-types
```
Avoid branches named after people (`alice-branch`) or dates (`fix-2026-04-23`). They carry no information.
### Pull-request template
The PR template above (`.github/pull_request_template.md`) is the enforcement mechanism. Every PR answers the same questions: what changed, why, how it was tested.
### Squash vs merge vs rebase
Pick one per repository and enforce it in branch protection.
- Squash merge — default for feature branches. One commit per PR, clean linear history, easy reverts. Combined with Conventional Commits on the PR title, the `main` history stays readable.
- Merge commit — preserves the branch structure. Use when individual commits in a branch are meaningful (public libraries, long-lived release branches).
- Rebase merge — linear history without a merge commit. Requires discipline; forces everyone to understand rebase conflicts.
Most application teams: squash merge. Most library teams: merge commit on main, rebase on feature branches.
### Signed commits
Enable commit signature verification. GitHub, GitLab, and Bitbucket all support SSH and GPG signing. In branch protection, require signed commits for release branches at minimum.
### Git hooks: Husky, lint-staged, Lefthook
Pre-commit hooks run linters and formatters on staged files before they enter the repository. The combination is non-negotiable; the tooling is a choice.
- `husky` + `lint-staged` — industry default in Node.js, millions of installs. Simple, well-documented, sequential execution.
- `Lefthook` — Go binary, parallel execution, 5–10× faster than Husky for multi-hook setups, first-class monorepo support. No Node.js dependency.
- `simple-git-hooks` — minimal, single-purpose. A good fit for small projects that want hooks without adding a dependency manager concept.
Canonical Husky + lint-staged setup:
```json path=null start=null
// package.json
{
  "scripts": {
    "prepare": "husky"
  },
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": ["eslint --fix", "prettier --write"],
    "*.{css,scss}": ["stylelint --fix", "prettier --write"],
    "*.{json,md,yml,yaml}": ["prettier --write"]
  }
}
```
```sh path=null start=null
# .husky/pre-commit
npx lint-staged
# .husky/commit-msg
npx --no -- commitlint --edit "$1"
```
Canonical Lefthook setup (`lefthook.yml`):
```yaml path=null start=null
pre-commit:
  parallel: true
  commands:
    eslint:
      glob: "*.{ts,tsx,js,jsx}"
      run: npx eslint --fix {staged_files}
      stage_fixed: true
    prettier:
      glob: "*.{ts,tsx,js,jsx,css,scss,json,md,yml,yaml}"
      run: npx prettier --write {staged_files}
      stage_fixed: true
    stylelint:
      glob: "*.{css,scss}"
      run: npx stylelint --fix {staged_files}
      stage_fixed: true
commit-msg:
  commands:
    commitlint:
      run: npx commitlint --edit {1}
```
Hooks are a convenience, not a guarantee. Developers can bypass with `--no-verify`. The same checks must run in CI; CI is the enforcement layer.
### semantic-release and changesets
For libraries, automate versioning and changelog generation from commit messages.
- `semantic-release` — fully automatic. Reads Conventional Commits, determines the next version, generates the changelog, tags the release, publishes to npm, creates a GitHub release. Single-package libraries.
- `changesets` — explicit opt-in. Developers write a changeset file per change describing the bump. Monorepos with independent package versions use this.
## Readability limits
Rules of thumb, not hard gates. When they are consistently exceeded, something is wrong.
- Function length — under 50 lines. Longer functions almost always contain multiple responsibilities.
- File length — under 300 lines. A file over 500 lines is a refactor candidate.
- Cyclomatic complexity — under 10. `eslint-plugin-sonarjs` can enforce this.
- Nesting depth — under 4. Prefer early returns, guard clauses, or extraction over indentation.
- Function parameters — under 4. Beyond that, pass an options object.
Early returns beat nested conditionals:
```ts path=null start=null
// Harder to read:
function process(user: User | null): string {
  if (user) {
    if (user.active) {
      if (user.subscription) {
        return user.subscription.plan;
      }
    }
  }
  return 'none';
}
// Clearer:
function process(user: User | null): string {
  if (!user) return 'none';
  if (!user.active) return 'none';
  if (!user.subscription) return 'none';
  return user.subscription.plan;
}
```
## CI enforcement
Pre-commit hooks and editor integrations are about speed; CI is about enforcement. A correct CI pipeline fails the build on any of the following:
- TypeScript errors (`tsc --noEmit`).
- ESLint errors (`eslint . --max-warnings 0`).
- Stylelint errors.
- Prettier drift (`prettier --check .`).
- Test failures.
- Accessibility violations (axe, `@axe-core/playwright`).
- Bundle-size budget exceeded.
- Commit messages failing commitlint (run on PR title when squash-merging).
- Dependency audit with known high-severity vulnerabilities.
Canonical job skeleton:
```yaml path=null start=null
# .github/workflows/ci.yml
name: ci
on:
  pull_request:
  push:
    branches: [main]
jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v4
      - uses: actions/setup-node@v4
        with:
          node-version-file: .nvmrc
          cache: pnpm
      - run: pnpm install --frozen-lockfile
      - run: pnpm type-check
      - run: pnpm lint
      - run: pnpm format:check
      - run: pnpm test
      - run: pnpm build
```
In a Turborepo monorepo, a single `pnpm turbo run type-check lint test build` runs everything in parallel with cache hits on unchanged packages.
## References
- ESLint flat config: https://eslint.org/docs/latest/use/configure/configuration-files
- typescript-eslint: https://typescript-eslint.io/
- Prettier options: https://prettier.io/docs/options
- Biome: https://biomejs.dev/
- Stylelint: https://stylelint.io/
- EditorConfig specification: https://spec.editorconfig.org/
- Conventional Commits: https://www.conventionalcommits.org/
- commitlint: https://commitlint.js.org/
- Husky: https://typicode.github.io/husky/
- Lefthook: https://lefthook.dev/
- Renovate configuration: https://docs.renovatebot.com/
- Pino: https://getpino.io/
- neverthrow: https://github.com/supermacro/neverthrow
- pnpm workspaces: https://pnpm.io/workspaces
- Turborepo: https://turbo.build/
- Vite: https://vite.dev/
- tsdown: https://tsdown.dev/
- CODEOWNERS: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners
