# Philosophy and Psychology

Before syntax and pixels, a website is a decision about what to communicate, to whom, and how it should feel. Skip this chapter and the rest will produce a fast, accessible, generic product.

## The one-sentence premise

A website's job is to help a specific person accomplish a specific goal while reinforcing a specific brand. Every choice — color, word, spacing, animation — is evaluated against those three variables. If a choice does not clarify the audience, advance the goal, or reinforce the brand, it is decoration.

## Craft as input, beauty as output

Beauty is a consequence of care applied at every level: the alignment of one label, the radius of nested corners, the timing of a single transition. Stripe, Linear, Figma, and Apple do not look beautiful because a visual designer made them so at the end. They look beautiful because every small decision was made correctly, and the accumulation reads as beauty. Treat craft as the input. Audit the boring details first.

## Intent

Every project needs a written answer to four questions before any UI is produced:

1. Who is this for? Describe the user in a sentence, naming a job-to-be-done, not a demographic.
2. What is the primary action they should take? One sentence, one verb.
3. Why would they trust this over the alternative? One sentence, grounded in something true.
4. What feeling should they leave with? One adjective, chosen deliberately.

If any answer is vague, the design will be vague.

## Laws of UX

A short list of empirically supported heuristics. Each is a lens, not a law of physics; when they conflict, preserve the one most aligned with user intent.

- Jakob's Law — Users spend most of their time on other sites. A novel pattern costs trust. Use conventional layouts for conventional flows (login, checkout, search). Reserve novelty for moments that carry brand weight.
- Hick's Law — Decision time grows with the number of choices. Collapse lists over seven items. Hide advanced options behind progressive disclosure.
- Fitts's Law — Time to acquire a target is a function of distance and size. Primary actions belong where the pointer already is and at a size that cannot be missed. Mobile tap targets should be at least 44 × 44 CSS pixels.
- Miller's Law — Working memory holds around 7 ± 2 items, and often fewer on the web. Chunk related content; avoid long uninterrupted lists.
- Aesthetic–Usability Effect — Users perceive attractive designs as easier to use. This rewards polish but does not excuse bad usability; pretty but broken still fails.
- Doherty Threshold — Productivity soars when a system responds in under 400 ms. Use optimistic UI, skeletons, and streaming to keep perceived response times short.
- Peak–End Rule — People judge an experience by its peak moment and its ending. Invest disproportionately in hero moments and final states (empty states, success screens, confirmations).
- Zeigarnik Effect — Uncompleted tasks occupy mental space. Progress indicators reduce abandonment because they close an open loop.
- Serial-Position Effect — First and last items in a list are remembered best. Lead with the most important, end with the second most important, and bury the filler in the middle.
- Goal-Gradient Effect — Motivation increases as users near completion. Show progress early, especially in onboarding and checkout.
- Law of Proximity — Related items belong close together; unrelated items belong far apart. Spacing is information.
- Law of Similarity — Items that look alike are perceived as related. Consistent styling is a promise; inconsistent styling is a bug report.

## Nielsen's 10 Usability Heuristics

Apply this checklist to every non-trivial interaction.

1. Visibility of system status — Tell the user what is happening through timely, appropriate feedback.
2. Match between system and the real world — Speak the user's language. Use familiar words, phrases, and concepts; follow real-world conventions.
3. User control and freedom — Provide a clearly marked undo, back, and cancel for every action. Never trap users.
4. Consistency and standards — Same word, same icon, same outcome. Honor platform conventions.
5. Error prevention — Remove conditions that lead to errors. Confirm destructive actions.
6. Recognition rather than recall — Show options; do not require the user to remember them from one screen to the next.
7. Flexibility and efficiency of use — Provide shortcuts for experts that do not impede novices.
8. Aesthetic and minimalist design — Every extra unit of information competes with the relevant units.
9. Help users recognize, diagnose, and recover from errors — Use plain language, identify the problem, and suggest a constructive next step.
10. Help and documentation — Keep it concise, searchable, task-focused, and close to the point of need.

## Gestalt principles

How humans group visual information. Use deliberately; most layout bugs are Gestalt violations.

- Proximity — Close elements are perceived as a group.
- Similarity — Shared color, shape, size, or orientation imply relationship.
- Continuity — The eye follows the smoothest path. Align elements along shared axes.
- Closure — The mind completes missing parts; suggest a shape rather than drawing it fully.
- Figure / ground — Every element is either figure or ground; maintain clear separation.
- Common region — A shared background or border is a strong grouping cue. Cards exploit this.
- Common fate — Items that move together are perceived as related.

## Cialdini's 7 principles of persuasion

Use to explain why designs work or do not. Apply ethically; manipulation backfires.

1. Reciprocity — People return value. Provide something useful (a tool, a guide, a trial) before asking.
2. Commitment and consistency — Small initial actions lead to larger aligned ones. Onboard with a trivial first step.
3. Social proof — People follow the behavior of similar others. Show specific, credible evidence: named customers, quantified outcomes, recent activity.
4. Authority — People defer to credible experts. Display evidence of expertise without boasting.
5. Liking — People comply with those they like. Be warm, specific, and human in copy.
6. Scarcity — Limited availability increases perceived value. Use only when real; false scarcity is corrosive.
7. Unity — Shared identity persuades. Speak to a tribe in language the tribe uses.

## Emotional design (Don Norman)

Three levels operate in parallel, and a complete product satisfies all three:

- Visceral — Immediate sensory impression. Handled by imagery, color, type weight, negative space, first paint.
- Behavioral — How it feels to use. Handled by latency, motion, haptics, clarity of feedback.
- Reflective — What using it says about the user afterwards. Handled by brand story, values, moments of delight, shareable outputs.

## The 5-second test

Show a new user the page for five seconds, then hide it and ask three questions: What is this? Who is it for? What can you do here? If any answer is wrong, the hero section is broken.

## Above the fold

"The fold" is a 1990s artifact; scrolling is universal. What matters is the first contentful region. It must answer the three 5-second-test questions and imply that more exists below. Do not cram every feature into it.

## F-pattern and Z-pattern

- F-pattern applies to dense, text-heavy pages (blog posts, search results, documentation). The eye scans the first lines fully, then the start of subsequent lines. Front-load meaning.
- Z-pattern applies to sparse marketing pages with a clear hierarchy. The eye moves top-left, top-right, down diagonally, then across again. Place the logo top-left, nav or CTA top-right, primary message center, primary CTA bottom-right.

Neither is deterministic; both are defaults. Validate with heatmaps or direct observation when stakes are high.

## Cognitive load budget

Every interface has a fixed budget. Stretch it and users leave. Reduce load by:

- Removing choices rather than adding explanations.
- Collapsing options behind sensible defaults.
- Keeping the visual density low where decisions are required.
- Treating empty space as a feature, not a cost.

## Feel

"Feel" is the sum of motion timing, typographic rhythm, color temperature, copy tone, and sound (if any). A site feels fast when responses are under 400 ms. A site feels warm when color temperature leans amber, type has high x-height, and copy uses contractions. A site feels serious when spacing is generous, type is neutral, and copy is terse. Decide the target feel in writing and audit every decision against it.

## Craft and the quality without a name

Christopher Alexander, in his work on architecture, described a central quality present in things that feel truly alive — a quality that is objective and precise but cannot be named. It is when something feels right, even if you cannot describe why exactly. A door that opens and closes perfectly. A staircase that feels good to walk up. Code that reads naturally on the first pass.

This quality is the result of craft: the deliberate attention put into making something excellent — not because someone is checking, but because it matters to the maker.

Jony Ive described working on a Sunday afternoon worrying about how a cable was coiled in a box. Millions of people would encounter that detail. He could have ignored it. Instead: "I knew that when somebody unwrapped that box and took out that cable, they'll think, 'Somebody gave a shit about me.' I think that's a spiritual thing."

Karri Saarinen, co-founder of Linear, observed the same pattern: "When something is crafted with care and attention, it possesses a certain feeling — a quality that's difficult to define but impossible to miss." Linear became profitable in year two, reached 10,000 paying customers by year four with effectively zero marketing spend — because people who experienced it told others.

Stripe's head of design documented a 20% conversion increase from an email redesign. Not from A/B-tested copy optimization, but from applying craft: improving hierarchy, improving visuals, improving how the journey felt at each step.

The practical implications:

- Craft scales through teams, but it starts with an individual deciding to care.
- Metrics measure what craft produces, not craft itself. "The lie is: because we spend all our time talking about what we can measure, that's all that matters."
- AI makes it faster to build but harder to care. The velocity that AI provides amplifies both careful and careless choices.
- Minimalism as an aesthetic is not craft. Minimalism as clarity — removing everything that obscures the essential — can be.
- The accumulation of small correct decisions reads as beauty. No single decision produces it.

The question for every design and code decision: does this reflect care? Not "does this look polished" — care. The distinction is that polished can be applied to the surface; care runs through the structure.

## Test for bias toward yourself

The designer is rarely the user. Counteract with:

- Five-second tests on people outside the team.
- Guerrilla usability with three to five unfamiliar participants per round — more than enough to surface the top issues.
- Heatmaps and session replays for live products.
- Analytics for unexpected drop-offs; then qualitative follow-up to learn why.
