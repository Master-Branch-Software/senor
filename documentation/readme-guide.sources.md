# Sources — readme-guide.md

Citations backing claims in [`readme-guide.md`](readme-guide.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `CONTRIBUTING.md`:

- Human-authored sources only (specs, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## Canonical references

- [Make a README](https://www.makeareadme.com/) — interactive guide and template, source of the canonical section list (Name, Description, Badges, Visuals, Installation, Usage, Support, Roadmap, Contributing, Authors, License, Project status). Also the source of "too long is better than too short" and "use examples liberally, and show the expected output if you can."
- [Standard-readme specification (Richard Litt)](https://github.com/RichardLitt/standard-readme/blob/main/spec.md) — formal spec for required vs optional sections, file-naming rules (`README.<BCP-47>.md`), 119-character short-description cap, "License must be last," and the rule that the table of contents is required for READMEs over ~100 lines.
- [Art of Readme — Stephen Whitmore](https://github.com/hackergrrl/art-of-readme) — cognitive-funneling section order (broadest to most specific), "the ideal README is as short as it can be without being any shorter," the bus-factor / six-month-stranger framing, and "care about people's time."
- [How to Write a Great README — Caleb Thompson, thoughtbot](https://thoughtbot.com/blog/how-to-write-a-great-readme) — voice argument ("technical writing is still writing"), defense of code samples over prose ("a few lines of syntax-highlighted source are worth a thousand words"), known-issues section.
- [Readme Driven Development — Tom Preston-Werner](https://tom.preston-werner.com/2010/08/23/readme-driven-development.html) — argument that the README is the act of creation, and that writing it first forces design clarity.
- [How to write a good README — banesullivan](https://github.com/banesullivan/README) — the four reader questions ("does this solve my problem, can I use this code, who made this, how can I learn more"), the "highlights at the top" rule, and the anti-pattern of burying overview behind API doc links.
- [How to write good README and why should you care — Kamen Mladenov](https://thejunkland.com/blog/how-to-write-good-readme.html) — empirical observation that "only 1 in 10" surveyed game projects had a basic project description; reframes audience to include future-self and future employer.
- [How to Write a Good README File for Your GitHub Project — freeCodeCamp](https://www.freecodecamp.org/news/how-to-write-a-good-readme-file/) — section list, audience triad (end-user, technical user, contributor), and the "no README is the only wrong way" framing.

## Tool catalog

- [shields.io](https://shields.io/) — canonical badge host. Cited for the recommendation to use cached badge images rather than hot-linking from CI vendors.
- [choosealicense.com](https://choosealicense.com/) — referenced when telling readers to name the license. Provides license text and one-line summaries.
- [vhs (charmbracelet)](https://github.com/charmbracelet/vhs) — terminal-recording tool for the "GIF or `vhs`-recorded terminal session" recommendation in the visuals section.

## Curated examples

The following project READMEs were repeatedly cited across the canonical references above as exemplars worth studying. Read them before writing or auditing a README — the patterns are easier to internalize from real artifacts than from rules.

- [fatiando/pooch](https://github.com/fatiando/pooch) — cited by banesullivan as a model README.
- [Kitware/ITK](https://github.com/InsightSoftwareConsortium/ITK) — cited by banesullivan.
- [gruns/furl](https://github.com/gruns/furl) — cited by banesullivan; tight one-liner and immediate usage example.
- [marcomusy/vedo](https://github.com/marcomusy/vedo), [nschloe/meshio](https://github.com/nschloe/meshio) — cited by banesullivan for visuals + concise structure.
- [ai/size-limit](https://github.com/ai/size-limit), [gofiber/fiber](https://github.com/gofiber/fiber), [Day8/re-frame](https://github.com/Day8/re-frame), [iterative/dvc](https://github.com/iterative/dvc) — examples curated in [matiassingers/awesome-readme](https://github.com/matiassingers/awesome-readme).

## AI-fingerprint vocabulary

The list of AI-fingerprint words ("delve," "leverage," "robust," "seamless," "tapestry," etc.) is cross-referenced with `front-end/brand-and-copywriting.md` and its sources file. Treat that chapter as the canonical list; keep this chapter's anti-patterns aligned but do not duplicate the full vocabulary here.

## Uncited / heuristic claims to verify

- The ~400-line README ceiling and ~100-line TOC threshold are heuristics drawn from the Standard-readme spec (which sets ~100 for the TOC) and from observed practice in the curated examples above. Mark as a heuristic in any eval that depends on it.
- The "five or fewer badges" cap is editorial — Make a README and Standard-readme allow badges without bounding the count. Verify against curated examples before treating as a hard rule.
- The 500 KB image budget is a performance heuristic that aligns with `front-end/performance.md`; cross-link if that chapter sets a different number.
