# Iteration 2 — Test Runner Instructions

## What you are testing

The `web-design` skill in `/home/bert/Documents/dev/ai-web-designer/web-design/`.

## What you must do

Run **5 eval prompts**. For each prompt, run it **twice**:
- **WITH skill**: Read `web-design/SKILL.md`, load the primary reference chapter(s) per the task-to-chapter map, then respond to the prompt following the skill's rules. Save the full response.
- **WITHOUT skill**: Respond to the prompt raw, with no skill context. Save the full response.

## Where to save outputs

```
/home/bert/Documents/dev/ai-web-designer/web-design-workspace/iteration-2/
├── eval-new-saas-landing-page/
│   ├── with_skill/outputs/response.md
│   └── without_skill/outputs/response.md
├── eval-pricing-page-a11y-contrast-fix/
│   ├── with_skill/outputs/response.md
│   └── without_skill/outputs/response.md
├── eval-react-component-performance-review/
│   ├── with_skill/outputs/response.md
│   └── without_skill/outputs/response.md
├── eval-hero-copy-anti-ai-audit/
│   ├── with_skill/outputs/response.md
│   └── without_skill/outputs/response.md
└── eval-email-microcopy-brand-voice/
    ├── with_skill/outputs/response.md
    └── without_skill/outputs/response.md
```

Create the directories as needed. Each `response.md` should contain the full agent output for that run.

## The 5 eval prompts

Read them from `/home/bert/Documents/dev/ai-web-designer/web-design/evals/evals.json`. The prompts are:

1. **new-saas-landing-page** — "I need a landing page for my SaaS..." (tests staged workflow, discovery questions, non-negotiables)
2. **pricing-page-a11y-contrast-fix** — "Make my pricing page accessible and fix the color contrast..." (tests targeted chapter loading, contrast analysis)
3. **react-component-performance-review** — "Review this React component for performance issues..." (tests code review lens, anti-patterns)
4. **hero-copy-anti-ai-audit** — "Audit and rewrite the hero copy..." (tests copywriting rules, anti-AI patterns)
5. **email-microcopy-brand-voice** — "Write a transactional email..." (tests tone ladder, brand voice, microcopy)

For each WITH skill run:
- Read SKILL.md first
- Load the primary reference chapter(s) listed in the task-to-chapter map for that task
- Follow the skill's operating principles (ask only what changes output, apply non-negotiables silently)
- Do NOT load all 16 reference chapters

For each WITHOUT skill run:
- Respond to the prompt directly with no skill context
- No need to read any reference files

## After all 10 responses are saved

Write a brief summary comparing WITH vs. WITHOUT skill outputs for each eval. Save to:

```
/home/bert/Documents/dev/ai-web-designer/web-design-workspace/iteration-2/summary.md
```

The summary should note:
- For each eval, what did the WITH skill output do that the WITHOUT skill output missed?
- Any cases where the skill caused worse output (over-constraint, verbosity, wrong chapter loaded)
- Overall: is the skill ready, or does SKILL.md need editing?

## Rules while working

- Do not modify any skill files (SKILL.md, references/, evals/)
- Do not run git commands
- Only create outputs in `web-design-workspace/iteration-2/`
