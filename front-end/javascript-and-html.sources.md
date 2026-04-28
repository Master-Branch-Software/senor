# Sources — javascript-and-html.md

Citations backing claims in [`javascript-and-html.md`](javascript-and-html.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `AGENTS.md`:

- Human-authored sources only (specs, MDN, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## HTML — primary specification

- [HTML Living Standard (WHATWG)](https://html.spec.whatwg.org/) — normative.
- [HTML Living Standard — Syntax](https://html.spec.whatwg.org/multipage/syntax.html) — void elements, self-closing rules.
- [MDN Web Docs — HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)

## HTML code style

### Void elements and trailing slashes

The chapter rule "void elements have no trailing slash in HTML" is supported by:

- [WHATWG: Start tags (void-element behavior)](https://html.spec.whatwg.org/multipage/syntax.html#start-tags) — verbatim: the slash on void elements "does not mark the start tag as self-closing but instead is unnecessary and has no effect of any kind." It also "should be used only with caution — especially since, if directly preceded by an unquoted attribute value, it becomes part of the attribute value rather than being discarded by the parser."
- [MDN — Void element](https://developer.mozilla.org/en-US/docs/Glossary/Void_element)
- [Rocket Validator: Trailing slash on void elements has no effect](https://rocketvalidator.com/html-validation/trailing-slash-on-void-elements-has-no-effect-and-interacts-badly-with-unquoted-attribute-values)
- [Web Experience Toolkit — Decision 15: Removal of trailing slash for void elements](https://wet-boew.github.io/wet-boew-documentation/decision/15.html)
- Style-guide consensus: Google HTML/CSS Style Guide, Code Guide (mdo), Idiomatic-HTML (Necolas), GoCardless, WordPress core, WET — all strip the slash.

Counter-position: Prettier 3 hardcodes the slash and provides no opt-out ([prettier#5246](https://github.com/prettier/prettier/issues/5246), [prettier#15612](https://github.com/prettier/prettier/issues/15612), open since 2018). The trade-off is documented in `web-design/index.html` being added to `.prettierignore`.

### Vertical rhythm — blank lines between elements

The HTML spec is silent beyond [WHATWG: a newline is suggested after DOCTYPE and around `<html>`](https://html.spec.whatwg.org/multipage/syntax.html). Codified rules in `javascript-and-html.md` are derived from the empirical practice in:

- [Idiomatic-HTML (Necolas Gallagher)](https://github.com/necolas/idiomatic-html) — "Use whitespace to improve readability."
- [Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html) — "Use a new line for every block, list, or table element."
- Prettier's documented behavior: collapses runs of blank lines to a single blank between block siblings under default `htmlWhitespaceSensitivity: "css"`.

## ECMAScript — primary specification

- [ECMA-262 (TC39)](https://tc39.es/ecma262/)
- [TC39 process document](https://tc39.es/process-document/)

## TC39 proposals tracked in chapter

- [Signals proposal](https://github.com/tc39/proposal-signals)
- [Temporal proposal](https://github.com/tc39/proposal-temporal)
- [Iterator helpers](https://github.com/tc39/proposal-iterator-helpers)
- [Set methods](https://github.com/tc39/proposal-set-methods)
- [Regex escaping](https://github.com/tc39/proposal-regex-escaping)
- [Promise.try](https://github.com/tc39/proposal-promise-try)
- [Import attributes](https://github.com/tc39/proposal-import-attributes)
- [JSON modules](https://github.com/tc39/proposal-json-modules)
- [Float16Array](https://github.com/tc39/proposal-float16array)
- [Duplicate named capturing groups](https://github.com/tc39/proposal-duplicate-named-capturing-groups)
- [RegExp modifiers](https://github.com/tc39/proposal-regexp-modifiers)
- [Signal polyfill](https://github.com/proposal-signals/signal-polyfill)

## Web components

- [Lit](https://lit.dev/) — minimal web-components library used in chapter examples.
- [Web Components Guide](https://webcomponents.guide/)
- [Custom Elements Everywhere](https://custom-elements-everywhere.com/) — framework integration matrix.
- [Custom Elements Manifest](https://github.com/webcomponents/custom-elements-manifest)

## Articles referenced

- [Frontend Masters — What to know in JavaScript 2026 edition](https://frontendmasters.com/blog/what-to-know-in-javascript-2026-edition/)
- [Frontend Masters — Architecture through component colocation](https://frontendmasters.com/blog/architecture-through-component-colocation/)
- [Frontend Masters — Post-mortem rewriting AgnosticUI with Lit](https://frontendmasters.com/blog/post-mortem-rewriting-agnosticui-with-lit-web-components/)
- [Frontend Masters — Form-associated custom elements in practice](https://frontendmasters.com/blog/form-associated-custom-elements-in-practice/)
- [Frontend Masters — Light DOM only](https://frontendmasters.com/blog/light-dom-only/)
- [2ality — ECMAScript 2025](https://2ality.com/2025/06/ecmascript-2025.html)
- [Pawel Grzybek — What's new in ECMAScript 2025](https://pawelgrzybek.com/whats-new-in-ecmascript-2025/)
- [Socket — ECMAScript 2025 finalized](https://socket.dev/blog/ecmascript-2025-finalized)

## (Backfill in progress for Date/Temporal, async patterns, progressive enhancement details.)
