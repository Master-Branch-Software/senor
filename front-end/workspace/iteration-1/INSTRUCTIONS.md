# Testing Instructions for the Next Agent

## What You Are Testing

The `web-design` skill in `/home/bert/Documents/dev/ai-web-designer/web-design/`.

This skill encodes professional-grade web design, frontend engineering, and copywriting guidelines. It has 16 reference chapters in `references/` and a `SKILL.md` navigation index. The skill uses progressive disclosure: read SKILL.md first, then load only the primary reference chapter(s) for the current task.

## Test Goal

Run 3 eval prompts. For each prompt, run it **twice**:
- **WITH skill**: Read `web-design/SKILL.md`, then load the primary reference chapter(s) per the task-to-chapter map, then respond to the prompt following the skill's rules.
- **WITHOUT skill**: Respond to the prompt raw, with no skill context.

Save both outputs so the user can compare them.

## Where to Save Outputs

```
/home/bert/Documents/dev/ai-web-designer/web-design-workspace/iteration-1/
├── eval-new-saas-landing-page/
│   ├── with_skill/outputs/response.md
│   └── without_skill/outputs/response.md
├── eval-pricing-page-a11y-contrast-fix/
│   ├── with_skill/outputs/response.md
│   └── without_skill/outputs/response.md
└── eval-react-component-performance-review/
    ├── with_skill/outputs/response.md
    └── without_skill/outputs/response.md
```

Each `response.md` should contain the full agent output for that run.

## The 3 Eval Prompts

### Eval 1 — New SaaS Landing Page
**Tests**: staged workflow, discovery questions, non-negotiables, no chapter dump

**Prompt**:
```
I need a landing page for my SaaS. It is a time-tracking app for remote engineering teams. We integrate with Jira, GitHub, and Slack. Our differentiator is that we automatically categorize time by project and client using AI, so managers get reports without anyone filling timesheets. Target audience: engineering managers at 50-500 person remote-first companies. I want it to feel calm and professional, not another loud productivity tool. I need it live in 3 weeks. No compliance requirements. English only for now. I have a logo and 3 product screenshots.
```

**With skill**: Load `web-design/SKILL.md`, then `web-design/references/15-discovery-and-communication.md` and `web-design/references/02-brand-and-copywriting.md`. Follow the 11-stage running order. Ask discovery questions one at a time. Apply non-negotiables silently.

**Assertions to check in output**:
- Does it propose or follow a multi-stage workflow (discovery → voice → architecture → copy → tokens → stack → scaffold → pages → tests → audits → launch)?
- Does it ask audience, primary action, differentiator, feel, metric, deadline, browser matrix, locales, compliance, or assets before producing artifacts?
- Does it reference img width/height/alt, WCAG contrast, font-display, prefers-reduced-motion, native HTML first, no positive tabindex, or two-space indent without being asked?
- Does it NOT paste large chunks of the 16 reference files verbatim?

---

### Eval 2 — Pricing Page Accessibility Fix
**Tests**: targeted chapter loading, contrast analysis, semantic HTML, keyboard focus, no scope creep

**Prompt**:
```
Make my pricing page accessible and fix the color contrast. The page has three tiers: Starter ($29/mo), Pro ($79/mo), Enterprise (custom). The Pro card has a 'Most Popular' badge. Currently the badge text is white on a light orange (#FFA500) background, and the disabled 'Current Plan' button uses a light gray (#CCCCCC) background with white text. The page uses a serif font for headings and a sans-serif for body. There is a comparison table with 12 features. The page is built in Next.js with Tailwind.
```

**With skill**: Load `web-design/SKILL.md`, then `web-design/references/05-accessibility.md` and `web-design/references/03-visual-design.md`. Focus only on a11y and color fixes. Do not scaffold a full repo.

**Assertions to check in output**:
- Does it identify the specific contrast failures (white on #FFA500, white on #CCCCCC)?
- Does it propose specific color values that meet WCAG 2.2 AA (4.5:1 for body, 3:1 for large text/UI)?
- Does it mention scope, headers, captions, or proper semantic HTML for the comparison table?
- Does it mention keyboard navigation, focus-visible, or focus management for pricing cards?
- Does it NOT spontaneously scaffold a full Next.js repo or add unnecessary dependencies?

---

### Eval 3 — React Component Performance Review
**Tests**: code review lens, anti-patterns, performance metrics, concrete fixes

**Prompt**:
```
Review this React component for performance issues.

export function ProductGrid({ products }) {
  const [filtered, setFiltered] = useState(products);
  const [sort, setSort] = useState('price');

  useEffect(() => {
    const sorted = [...products].sort((a, b) => a[sort] - b[sort]);
    setFiltered(sorted);
  }, [products, sort]);

  return (
    <div className="grid grid-cols-3 gap-4">
      {filtered.map((p) => (
        <ProductCard
          key={p.id}
          product={p}
          onAddToCart={() => addToCart(p)}
        />
      ))}
    </div>
  );
}

It renders up to 500 products. The parent re-renders every 2 seconds because of a polling hook. Users complain the page feels sluggish on mobile.
```

**With skill**: Load `web-design/SKILL.md`, then `web-design/references/06-performance.md`, `web-design/references/07-javascript-and-html.md`, and `web-design/references/09-anti-patterns-and-process.md`. Give concrete fixes in order of impact.

**Assertions to check in output**:
- Does it identify missing memoization (useMemo for sorting, React.memo for ProductCard, or useCallback for handler)?
- Does it identify inline onAddToCart causing re-render on every parent render?
- Does it note grid-cols-3 without responsive breakpoints hurts mobile?
- Does it suggest windowing/virtualization for 500 items?
- Does it mention INP, CLS, or web-vitals measurement?

---

## Reporting

After all 6 responses are saved, produce a brief summary:
1. For each eval, which assertions passed for the WITH skill run vs the WITHOUT skill run?
2. Were there any cases where the skill caused worse output (over-constraint, irrelevant chapter loading, verbosity)?
3. Overall assessment: is the skill ready, or does SKILL.md need editing?

Save this summary to:
```
/home/bert/Documents/dev/ai-web-designer/web-design-workspace/iteration-1/summary.md
```

Then tell the user you're done and they can review the outputs.
