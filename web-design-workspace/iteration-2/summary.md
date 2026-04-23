# Iteration 2 — Eval Summary: WITH skill vs. WITHOUT skill

## Eval 1: new-saas-landing-page

**WITH skill:**
The response followed the 11-stage running order from SKILL.md. It asked exactly one targeted question (primary action) rather than a bulk intake form, stated assumptions before acting on them, and produced named artifacts (`brief.md`, `constraints.md`, `brand.md`, `sitemap.md`) with stage boundary pauses. Non-negotiables (`img` attributes, WCAG contrast, `font-display`, `prefers-reduced-motion`, `visible` not `isVisible`, `.DS_Store`) were listed in `constraints.md` without needing to be requested. The archetype recommendation (Sage/Creator) was derived from the brief and offered as a default to confirm. The scope was capped to 3 pages appropriate for a 3-week deadline with deferred items named explicitly.

**WITHOUT skill:**
The response jumped directly into a layout prescription and a 3-week timeline. It produced a reasonable structure but skipped the staged artifact workflow entirely — no `brief.md`, no `brand.md`, no discovery questions. The non-negotiables (contrast, focus, motion, `font-display`, image attributes) were not mentioned. The "design direction" section gave vague color guidance ("muted blues or slate grays") rather than a principled token system. The timeline recommended Figma mockups, which the skill's chapter 15 would flag as the wrong order (artifacts before visual work).

**Delta:**
WITH skill asked before assuming; WITHOUT skill assumed and prescribed. WITH skill produced durable artifacts that upstream-protect downstream stages. WITHOUT skill produced a usable but shallow response that would require substantial rework if the primary action or voice turned out to be wrong.

---

## Eval 2: pricing-page-a11y-contrast-fix

**WITH skill:**
The response correctly identified both contrast failures with measured ratios (#FFA500 badge: ~2.16:1; #CCCCCC button: ~1.6:1) and mapped them to WCAG 2.2 SC numbers (1.4.3, 1.4.1). It introduced a second failure on the badge not mentioned in the prompt: SC 1.4.1 (Use of Color) — the badge relies on color alone to convey "most popular." Color fixes were proposed using OKLCH for perceptual accuracy with verified ratios. It caught the SC 1.4.3 exemption nuance for disabled controls (the exemption has a precondition that the markup must be correct). It restructured the comparison table with `scope`, `<caption>`, and `<th scope="row">` on data rows. Focus-visible and heading hierarchy were included. It explicitly declined to scaffold a full repo. Issues were ordered by severity with WCAG SC citations.

**WITHOUT skill:**
The response identified the same two contrast failures but did not measure ratios or cite SC numbers. The badge fix proposed dark text on `#FFA500` (which the response implies passes without verifying), and one option kept `#CCCCCC` with `#3a3a3a` text — that pair is approximately 3.4:1, which passes for large text (≥3:1) but does not pass for normal body text (needs 4.5:1), and the response did not note that distinction. The SC 1.4.1 "color alone" failure on the badge was missed entirely. The table fix showed `<caption>` and `scope="col"` but omitted `<th scope="row">` for the feature column and left checkmarks as raw `✓` characters with no text equivalent, which fails SC 1.1.1. The disabled button was suggested to use `tabindex="-1"` which is incorrect — a natively `disabled` button is already removed from tab order.

**Delta:**
WITH skill caught an additional SC 1.4.1 failure. It used OKLCH for ratio-accurate color proposals. It caught the SC 1.4.3 exemption nuance. It caught the `<th scope="row">` omission and icon text equivalents. The without-skill response contained one error (recommending `tabindex="-1"` on a disabled button) and one underspecified fix (dark text on #FFA500 not ratio-verified).

---

## Eval 3: react-component-performance-review

**WITH skill:**
The response diagnosed the `useEffect`/`setState` pattern as a React anti-pattern for derived state (per chapter 09) with a measurable performance consequence: a 50 ms long-task budget violation every 2 seconds (per chapter 06's INP guidance). It correctly identified that `React.memo` alone is insufficient without `useCallback`, and explained why neither works without the other. It connected the 500-DOM-node issue to INP degradation and CLS risk (chapter 06), not just render performance. It recommended `@tanstack/virtual` without adding unrelated dependencies. It mentioned `startTransition` for expensive `addToCart` handlers and image `aspect-ratio` for CLS. Issues were ordered by impact with a measurement directive (web-vitals, React DevTools Profiler) at the end.

**WITHOUT skill:**
The response identified the same five issues in roughly the same order. It correctly recommended `useMemo`, `useCallback`, `React.memo`, and virtualization. However, it had a factual error in the `useCallback` fix — the proposed code still wraps `handleAddToCart` in an inline arrow `() => handleAddToCart(p)` inside the JSX, which re-introduces the unstable reference problem it was trying to solve. The response then self-corrected partway through but the correction was awkward. No mention of INP, CLS, 50 ms task budget, or measurement methodology. Virtualization was offered with three library options without a recommendation, and the `react-window` example used `FixedSizeGrid` with hardcoded pixel dimensions (900px wide), which breaks responsiveness.

**Delta:**
WITH skill connected the issues to named performance metrics (INP, CLS) and gave a measurement directive. The without-skill response had a real technical error in the `useCallback` fix and a hardcoded pixel width in the virtualization example. The WITH skill response was more precise about the causal chain between the polling parent and the re-sort cycle.

---

## Eval 4: hero-copy-anti-ai-audit

**WITH skill:**
The audit named each forbidden verb and pattern with the specific rule it violated (abstract verbs, vague collectives, no specific audience, no mechanism, no numbers, wrong archetype register). The rewrites applied named headline formulas from chapter 02 (specific outcome + mechanism, BAB, Ogilvy fact-first) and used brand vocabulary from the prompt (pipeline, deploy, incident, on-call, page). Draft C included a correct caution: "if the stat does not exist, do not use Draft C" — reflecting the chapter 02 rule that social proof claims must be sourced. The response gave a recommendation for which draft to test first and specified the right downstream conversion metric (signup → first pipeline connected) rather than CTA clickthrough.

**WITHOUT skill:**
The audit correctly identified the vague verbs and absence of specificity, and the rewrites were reasonable. However, the audit did not name specific patterns from a reference (e.g., "no mechanism," "no numbers" were touched on but not as a systematic taxonomy). The rewrites in options 1–3 were generally good but still used one potentially vague phrase ("high-growth teams") and in Option 1 "cut mean time to resolution" is a standard MTTR claim that reads better than the original but is still generic without a number. The brand vocabulary (root cause, signal, noise, on-call, ship, incident) was not explicitly drawn from a brand document and one option used "full visibility" which is itself a fairly generic phrase. No measurement recommendation was given.

**Delta:**
WITH skill applied a systematic taxonomy of AI-pattern failures and tied the rewrites to named formulas. It named the right downstream metric for testing. The without-skill rewrites were reasonable but less precise about which patterns were being corrected and why.

---

## Eval 5: email-microcopy-brand-voice

**WITH skill:**
The response cited the tone ladder explicitly (mid formality, warm, low urgency, no play, never guilt) before writing a word, then produced an email that hit every rule: subject states outcome (not sender), under 50 characters, preheader extends without repeating subject, body leads with the reason sent, names the specific change (Pro → Free), scannable "What stays / What ends" structure, zero guilt language, one action, signed from a named human. The annotation section explained each decision by reference to the rule it enacted. The forbidden word list was confirmed clean.

**WITHOUT skill:**
The email included "We're sorry to see you step back from Pro" — a soft guilt phrase, directly contrary to the "never guilt" rule for churn context. It was signed from "The Flint Team" rather than a named human, missing the relationship-sensitive email rule. The subject was 39 characters and acceptable, but the preheader was not specified. The structure had two CTAs ("Upgrade to Pro" and "[Help Center]") where the rule is one action per email. The body did not use Flint's brand vocabulary (root cause, signal, noise) at all — the product was treated as a generic SaaS. The response partially caught itself in the "Notes" section ("we're sorry to see you step back... can feel patronizing") but did not fix the email body.

**Delta:**
WITHOUT skill violated two named rules (never guilt, named human sender) and added a second CTA. The self-correction in the notes section was not applied to the actual email, which is a meaningful failure mode — the system knew the rule and still produced the wrong output. WITH skill had zero violations.

---

## Overall assessment

**Verdict: skill is ready with one minor note.**

Across all five evals, the WITH skill responses:
- Applied correct chapters without loading all 16.
- Named WCAG SC numbers, measured contrast ratios, and caught an SC 1.4.1 failure not in the original prompt.
- Connected React performance issues to INP/CLS metrics and gave measurement guidance.
- Applied tone ladder positions from chapter 02 before writing, then hit every rule.
- Audited copy against named patterns rather than intuition.
- Produced no technical errors.

The WITHOUT skill responses ranged from "reasonable but shallow" (evals 1, 4) to "contains a technical error" (evals 2, 3) to "violates the primary rule of the task" (eval 5).

**One note for SKILL.md:**
The task-to-chapter map entry for "Audit/improve performance" lists chapter 06 as primary and 07 + 09 as secondary, which is correct. For the React performance task (eval 3), loading all three was appropriate and the boundary between them was clean. No change needed.

The chapter 02 churn tone ladder rule ("never guilt") was the single most decisive differentiator in eval 5 — a rule the without-skill response knew about conceptually but failed to apply to its own output. This suggests the skill's operating principle ("apply rules silently; never paraphrase back into output") is working correctly for the with-skill run, and the evaluation surfaces a real failure mode in the without-skill case. The skill is ready for production use.
