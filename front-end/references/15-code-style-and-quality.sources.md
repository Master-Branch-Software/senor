# Sources — 15-code-style-and-quality.md

Citations backing claims in [`15-code-style-and-quality.md`](15-code-style-and-quality.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `AGENTS.md`:

- Human-authored sources only (specs, MDN, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## Prettier

- [Prettier — Options](https://prettier.io/docs/options) — authoritative reference for `htmlWhitespaceSensitivity`, `bracketSameLine`, `singleAttributePerLine`, `printWidth`, etc.
- [Prettier 1.15 release notes — htmlWhitespaceSensitivity rationale](https://prettier.io/blog/2018/11/07/1.15.0.html) — explains why `"css"` is the default: HTML whitespace inside inline elements (`<b>`, `<a>`) is significant; reformatting under `"ignore"` would alter rendered output.
- [Prettier 2.4 — bracketSameLine introduced for HTML](https://prettier.io/blog/2021/09/09/2.4.0.html) — renamed from `jsxBracketSameLine` and extended to HTML/Vue/Angular.
- [Prettier issue #5246 — option to skip trailing slashes on void elements](https://github.com/prettier/prettier/issues/5246) — open since 2018; the philosophy is "resists adding options."
- [Prettier issue #15612 — trailing slash on void elements](https://github.com/prettier/prettier/issues/15612)

## Biome — alternative formatter

- [Biome — Formatter](https://biomejs.dev/formatter/)
- [Biome — Differences with Prettier](https://biomejs.dev/formatter/differences-with-prettier/)
- [Biome — Language support matrix](https://biomejs.dev/internals/language-support/) — HTML stable in v2.4 (Feb 2026); Vue/Svelte/Astro experimental; Markdown not yet supported.
- [Biome 2026 Roadmap](https://biomejs.dev/blog/roadmap-2026/)
- [Biome — HTML parsing and formatting issue #4726](https://github.com/biomejs/biome/issues/4726)

## ESLint and typescript-eslint

- [ESLint — Configuration files (flat config)](https://eslint.org/docs/latest/use/configure/configuration-files)
- [typescript-eslint](https://typescript-eslint.io/)

## Stylelint

- [Stylelint](https://stylelint.io/)

## EditorConfig

- [EditorConfig specification](https://spec.editorconfig.org/)

## Conventional Commits and commitlint

- [Conventional Commits](https://www.conventionalcommits.org/)
- [commitlint](https://commitlint.js.org/)

## Hooks and Git tooling

- [Husky](https://typicode.github.io/husky/)
- [Lefthook](https://lefthook.dev/)
- [Renovate configuration](https://docs.renovatebot.com/)

## Logging

- [Pino](https://getpino.io/)

## Error handling

- [neverthrow (Result types)](https://github.com/supermacro/neverthrow)

## Monorepo and build

- [pnpm workspaces](https://pnpm.io/workspaces)
- [Turborepo](https://turbo.build/)
- [Vite](https://vite.dev/)
- [tsdown](https://tsdown.dev/)

## CODEOWNERS

- [GitHub — About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)

## Tooling and lint plugins

- [eslint-plugin-compat](https://github.com/amilajack/eslint-plugin-compat) — flag APIs unsupported in target browsers.
- [stylelint-no-unsupported-browser-features](https://github.com/RJWadley/stylelint-no-unsupported-browser-features)
- [stylelint-browser-compat](https://www.npmjs.com/package/stylelint-browser-compat)
- [browserslist update-db](https://github.com/browserslist/update-db) — keep `caniuse-lite` current.
- [PostCSS](https://postcss.org/)
- [Autoprefixer](https://github.com/postcss/autoprefixer)
- [Babel](https://babeljs.io/)

## Error tracking and observability

- [Sentry](https://sentry.io/)
- [Datadog](https://www.datadoghq.com/)
- [New Relic](https://newrelic.com/)
- [Grafana](https://grafana.com/)
- [OpenTelemetry](https://opentelemetry.io/)
- [Honeycomb](https://www.honeycomb.io/)
- [BetterStack](https://betterstack.com/)
- [Axiom](https://axiom.co/)
- [LogRocket](https://docs.logrocket.com/)

## HTML formatting conventions referenced in chapter 7

The "HTML code style" section in chapter 7 — vertical-rhythm rule and void-element trailing-slash rule — is sourced in `07-javascript-and-html.sources.md`. This sidecar covers the toolchain (Prettier, ESLint, Stylelint, commitlint, hooks) only.

## (Backfill in progress for naming conventions, dependency management, and CI enforcement details.)
