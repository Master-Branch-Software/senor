# Stack and Architecture

The stack is a decision about trade-offs, not taste. The best stack for a content site is rarely the best stack for a dashboard, and the best stack today will not be the best stack in three years. Decisions should be explicit.

## Picking a framework

Ask, in order:

1. Is this primarily content (marketing, docs, blogs)? → Astro is the default.
2. Is this a full web app with meaningful interactivity and shared data across routes? → Next.js (React) or SvelteKit.
3. Must this be deeply progressive-enhanced and prefer web standards? → Remix / React Router v7, SvelteKit.
4. Does the project require the smallest possible JavaScript payload and the strongest compiler output? → SvelteKit or Qwik.
5. Is this a static, no-framework site? → Plain HTML, CSS, and JS. It is still the fastest option, and modern CSS/JS cover more than they used to.

### Astro

Zero client-side JavaScript by default. Content-first. Islands architecture allows adding any component framework (React, Vue, Svelte, Solid, Lit, preact) only where interactivity is genuinely needed.

- Best for marketing sites, blogs, documentation, portfolios, e-commerce storefronts.
- Content Collections with Zod schemas provide type-safe content authoring.
- Deploy to static hosts (Netlify, Cloudflare Pages, Vercel) or edge runtimes.
- Image, font, and asset optimization are built in.

### Next.js

The pragmatic default for React apps that need server rendering, routing, and a large ecosystem.

- App Router with React Server Components for fine-grained server/client split.
- `next/image`, `next/font`, `next/script` handle the hard parts of media loading.
- Built-in ISR, streaming SSR, Middleware, Edge Runtime.
- Large hiring pool, extensive documentation, broad vendor support.

### SvelteKit

Compiler-first framework that ships minimal runtime. Produces very small bundles for similar feature surface.

- Strong progressive enhancement story (forms work without JS, then upgrade).
- Svelte 5 runes align with signal-style reactivity.
- Remote functions provide type-safe server/client boundaries.
- Excellent developer experience.

### Remix / React Router v7

Web-standards-first React framework. Loaders, actions, nested routes, and progressive enhancement by default.

- Great for apps with complex data mutations and forms.
- Embraces HTML-native paradigms (forms, redirects, cookies).
- Now unified under React Router v7.

### Nuxt (Vue)

The Vue equivalent of Next.js. Mature ecosystem, strong SSR and SSG support, good defaults for image and asset handling.

### SolidStart, Qwik City

Niche but compelling choices for teams prioritizing raw performance and fine-grained reactivity. Qwik's resumability uniquely avoids most hydration cost. Solid's reactivity is closest to signals by default.

### No framework

Static HTML, CSS, and JavaScript still ship the fastest and last the longest. Consider them explicitly for small sites, landing pages, or documentation; add frameworks only when they solve a problem that the platform cannot.

## Styling strategy

### Utility-first: Tailwind CSS v4

Tailwind v4 is the current default for most greenfield projects.

- CSS-first configuration via the `@theme` directive; no JavaScript config file is required.
- OKLCH-based default color palette.
- Oxide Rust-based engine for significantly faster builds.
- `@apply` still supported, though best reserved for component-level extraction.
- Designed to work with the native cascade layer pattern.
- Requires Safari 16.4+, Chrome 111+, Firefox 128+ because of native `@property`, `color-mix()`, and cascade-layer usage. Projects with older browser targets should stay on v3.4.

Tailwind pros:

- No naming fatigue. Classes reflect intent.
- Purges unused styles automatically; bundle stays small.
- Design tokens are co-located in classes, easy to refactor.
- Plays well with component libraries (shadcn/ui, Radix, Headless UI).

Tailwind cons:

- Requires discipline to avoid class soup. Extract components when classes become long or repeated.
- Onboarding can feel steep without an IDE plugin or class-sort plugin.

Mitigations: use `class-variance-authority` (cva) or `tailwind-variants` (tv) to type variants; use `prettier-plugin-tailwindcss` to sort classes; extract a `<Button>` component rather than repeating identical class lists.

### CSS Modules

Scoped CSS authored in plain CSS files. Good when Tailwind is not desired or when a team prefers classic CSS authoring with scope safety.

### Vanilla Extract

Type-safe CSS with build-time extraction. Strong choice for teams that want authored styles plus strict typing and theme variables.

### Plain CSS with `@layer`

Perfectly viable now. Modern CSS (nesting, container queries, cascade layers) removes much of the old pain. Use for projects where simplicity and portability matter more than tooling.

## Design tokens

Tokens are the canonical representation of brand and design decisions. They bridge design tools (Figma variables) and code.

- Store tokens in a single source of truth (JSON, YAML, or Style Dictionary).
- Derive platform-specific outputs (CSS custom properties, Tailwind theme, Figma variables, native platform constants).
- Name tokens by intent, not value: `color-background-primary` not `color-blue-600`.
- Organize tokens in tiers:
  - Primitive tokens — raw values (`color-slate-50` through `color-slate-900`).
  - Semantic tokens — role-based names referencing primitives (`color-foreground-default` → `color-slate-950`).
  - Component tokens — component-specific references (`button-primary-background` → `color-accent-9`).

This tiering lets a brand refresh change primitives without touching every component.

## Component architecture

### shadcn/ui

Not a library. A convention: copy accessible, well-styled component source into the project repo and customize it. Built on Radix UI primitives, Tailwind for styling, `class-variance-authority` for variants.

Benefits:

- No dependency on a package that may go unmaintained.
- Full control over markup and styling.
- Easy to upgrade selectively; there is no "everything" upgrade.

Use shadcn/ui as the default for React projects where a custom design system has not yet been built. Radix UI and React Aria Components are the analogous primitives layer for non-React or when more control is needed.

### Composition over configuration

Component APIs should be composable, not option-laden. Prefer:

```jsx path=null start=null
<Card>
  <CardHeader>
    <CardTitle>Plan</CardTitle>
    <CardDescription>Monthly billing</CardDescription>
  </CardHeader>
  <CardBody>…</CardBody>
  <CardFooter>
    <Button variant="primary">Upgrade</Button>
  </CardFooter>
</Card>
```

Over a `<Card title="Plan" description="Monthly billing" body="…" action="Upgrade" />` anti-pattern. Composition handles every edge case; configuration handles the ones that were anticipated.

### Co-locate component assets

A component owns its template, styles, tests, and types:

```
components/
  button/
    button.tsx
    button.module.css    # or button.css if using CSS module scripts
    button.stories.tsx
    button.test.tsx
    index.ts
```

Co-location makes components easy to move, easy to delete, and easy to understand.

### Web component architecture without a framework

Modern CSS module scripts allow the same pattern without a bundler:

```js path=null start=null
import sheet from "./button.css" with { type: "css" };

export class MyButton extends HTMLElement {
  constructor() {
    super();
    const root = this.attachShadow({ mode: "open" });
    root.adoptedStyleSheets = [sheet];
    root.innerHTML = `<button part="btn"><slot></slot></button>`;
  }
}
customElements.define("my-button", MyButton);
```

Supported in Chromium and Firefox 147+. For cross-browser parity today, wrap with a minimal bundler.

### Naming conventions

- Components — PascalCase (`Button`, `CardHeader`).
- Hooks, utilities, helpers — camelCase (`useHotkey`, `formatCurrency`).
- Files — kebab-case (`card-header.tsx`, `use-hotkey.ts`).
- CSS classes — kebab-case, BEM only when strictly necessary. Tailwind projects avoid BEM entirely.
- Booleans — drop the `is` prefix in variable names: `visible`, `active`, `loading`, `disabled`.
- Setters — `setX` prefix: `setVisible`, `setActive`.

### Folder structure

There is no universal right structure; consistency matters more than any specific shape. Two reasonable defaults:

```
# By feature
src/
  features/
    billing/
      components/
      lib/
      routes/
      tests/
    auth/
      components/
      lib/
      routes/
      tests/
  shared/
    components/
    lib/
```

```
# By type (smaller projects)
src/
  components/
  hooks/
  lib/
  pages/
  styles/
  tests/
```

Pick one, apply it everywhere, and enforce with ESLint import rules.

## Form architecture

Forms are their own category of state management. Complex forms — multi-step wizards, dynamic field arrays, conditional visibility, async validation, file uploads — require a different architecture than general application state.

### Schema-first design

Define the form schema before writing any UI. The schema is the single source of truth for field types, validation rules, error messages, and TypeScript types:

```ts path=null start=null
import { z } from 'zod';

const userSchema = z.object({
  email: z.string().email('Enter a valid email'),
  password: z
    .string()
    .min(8, 'At least 8 characters')
    .regex(/[A-Z]/, 'Needs uppercase letter')
    .regex(/[0-9]/, 'Needs a number'),
  confirmPassword: z.string(),
  address: z.object({
    street: z.string().min(1, 'Required'),
    city: z.string().min(1, 'Required'),
    postalCode: z.string().regex(/^\d{5}$/, 'Invalid postal code'),
  }),
}).refine((d) => d.password === d.confirmPassword, {
  message: "Passwords don't match",
  path: ['confirmPassword'],
});

type UserFormData = z.infer<typeof userSchema>;
```

### Multi-step forms

Break complex forms into steps when there are 8+ fields or natural groupings. Each step reduces cognitive load. Validate before advancing; never wipe data when going back:

```tsx path=null start=null
function MultiStepForm() {
  const [step, setStep] = useState(1);
  const methods = useForm<FormData>();

  const onSubmit = async (data: FormData) => {
    if (step < 3) { setStep(step + 1); return; }
    await createUser(data);
  };

  return (
    <FormProvider {...methods}>
      <form onSubmit={methods.handleSubmit(onSubmit)}>
        {/* Progress indicator */}
        <p>Step {step} of 3</p>

        {step === 1 && <PersonalInfoStep />}
        {step === 2 && <AddressStep />}
        {step === 3 && <ReviewStep />}

        <div>
          {step > 1 && <button type="button" onClick={() => setStep(s => s - 1)}>Back</button>}
          <button type="submit">{step < 3 ? 'Next' : 'Submit'}</button>
        </div>
      </form>
    </FormProvider>
  );
}
```

### Validation timing

Show errors at the right moment — not on every keystroke:

```tsx path=null start=null
const { register, formState: { errors, touchedFields, isSubmitted } } = useForm({
  mode: 'onBlur', // validate on blur, not on change
});

// Show error only after user has touched the field OR tried to submit
const showError = error && (touchedFields[name] || isSubmitted);
```

Validating on every keystroke produces red error messages while a user is still typing. Validate on blur for individual fields, and on all fields at submission.

### Dynamic field arrays

```tsx path=null start=null
import { useFieldArray } from 'react-hook-form';

function SkillsList() {
  const { fields, append, remove } = useFieldArray({ control, name: 'skills' });

  return (
    <>
      {fields.map((field, index) => (
        <div key={field.id}>
          <input {...register(`skills.${index}.name`)} />
          <button type="button" onClick={() => remove(index)}>Remove</button>
        </div>
      ))}
      <button type="button" onClick={() => append({ name: '' })}>Add Skill</button>
    </>
  );
}
```

### File uploads

For simple uploads, use FormData with multipart:

```tsx path=null start=null
const FileSchema = z
  .instanceof(File)
  .refine(f => f.size <= 5 * 1024 * 1024, 'Max 5 MB')
  .refine(f => ['image/jpeg', 'image/png', 'image/webp'].includes(f.type), 'JPEG, PNG, or WebP only');

// After form submission:
async function uploadFile(file: File) {
  const formData = new FormData();
  formData.append('file', file);
  await fetch('/api/upload', { method: 'POST', body: formData });
}
```

For large files, use chunked uploads with resume capability. Show progress per file. Handle all states: queued, uploading, success, error. Validate MIME type on the server from actual file bytes, not the extension.

### Server-side error propagation

Preserve user input on server errors. Map server field errors back into the form:

```ts path=null start=null
const onSubmit = async (data: FormData) => {
  const res = await fetch('/api/signup', { method: 'POST', body: JSON.stringify(data) });
  if (!res.ok) {
    const { fieldErrors } = await res.json();
    // Map server errors to fields without wiping values
    Object.entries(fieldErrors).forEach(([field, message]) => {
      setError(field as keyof FormData, { message: message as string });
    });
  }
};
```

### Form stack selection

- React Hook Form + Zod — default for React. Uncontrolled forms with minimal re-renders.
- Valibot — smaller bundle than Zod, compatible API.
- TanStack Form — framework-agnostic, type-safe, supports async validation out of the box.

Treat forms as their own category, not general state. Do not use Zustand or Redux for form state.

## State management

- Local, component-only state — component state hooks or signals.
- Shared across a few siblings — lift state to common parent.
- Shared across many components — context + reducer, or a small state manager (Zustand, Jotai, Nanostores, Valtio).
- Server state — TanStack Query, SWR, or framework-native equivalents. Do not keep server state in local state libraries.
- Forms — React Hook Form (React) or framework-native equivalents. Treat forms as their own category, not general state.
- Global app state (auth, theme) — lightweight store, or context if the churn is low.

Rule of thumb: the smallest scope that works is the right scope. Do not reach for Redux to manage a single boolean.

## Routing

- Use the framework's router. Avoid libraries like `react-router` inside a Next.js or Remix project.
- Prefer nested layouts to repeated page-level structure.
- Enable route-level code splitting (it is the default in every mainstream framework).
- Preserve scroll position across navigation by default; restore it on back/forward.

## Data fetching

- Server-render or statically generate pages whose content does not depend on the user.
- Co-locate data loaders with the routes they serve.
- Cache aggressively with appropriate invalidation.
- Stream when possible; hydrate progressively when supported.
- Never fetch data in a hot re-render path without caching.

## Authentication

- Prefer framework- or platform-native auth (Clerk, Auth.js, Supabase Auth, Firebase Auth) over hand-rolled.
- Store session tokens in `HttpOnly`, `Secure`, `SameSite=Lax` or `Strict` cookies.
- Never store tokens in `localStorage` — vulnerable to XSS.
- Rotate refresh tokens; keep access tokens short-lived.
- Hash passwords with Argon2id or bcrypt (work factor tuned to ~250 ms).

## Deployment

- CI runs type checks, unit tests, accessibility checks, Lighthouse CI, and bundle-size budgets.
- Deploy previews per pull request.
- Canary or staged rollouts for production.
- Monitor Core Web Vitals, error rates, and latency from day one.
- Pick the deployment target to match the framework:
  - Astro, SvelteKit, Next.js — Vercel, Netlify, Cloudflare Pages, or self-host with a Node runtime.
  - Edge-heavy apps — Cloudflare Workers, Vercel Edge, Deno Deploy, Bun serverless.
  - Static-only — any CDN, including GitHub Pages.

## Observability

- Structured logging with a consistent schema (`timestamp`, `level`, `msg`, `request_id`, `user_id`, structured fields).
- Distributed tracing for backend requests (OpenTelemetry).
- Error tracking (Sentry, Rollbar, equivalent) capturing both server and client errors with source maps.
- Front-end Real-User Monitoring for Core Web Vitals and long tasks.
- Dashboards for the three or four metrics that predict user pain: TTFB, LCP, INP, error rate.

## Source control and collaboration

- Conventional commits are optional; clear commits are not. The first line states the change and affected area.
- Pull requests describe the "why," link to the ticket, and include screenshots for UI changes.
- Enforce review for any change touching shared infrastructure, auth, payments, or schemas.
- Protect the main branch; require passing CI.
- Use `.gitignore` to exclude build artifacts, secrets, and OS detritus (`.DS_Store`, `Thumbs.db`, `node_modules`, `.env`, `dist/`).

## Documentation

A project's documentation debt compounds faster than its code debt.

- `README.md` — what the project is, how to run it, how to deploy, how to run tests.
- `ARCHITECTURE.md` — high-level diagram and rationale for major decisions.
- `CONTRIBUTING.md` — expected workflow.
- `DESIGN_TOKENS.md` or equivalent — the source of truth for colors, type, spacing.
- In-code comments for non-obvious decisions, not for what the code does.

## Aging gracefully

Stacks change. Code that depends on a single library deeply will feel that library's decline. Counteract with:

- A thin abstraction layer over third-party primitives where it costs little.
- Standardized data formats (JSON, SQL schemas) over proprietary ones.
- Types derived from schemas, not scattered through the codebase.
- Tests at the behavior level, not the implementation level.
- Dependency audits quarterly; remove unused packages aggressively.

The simplest architecture that solves today's problem is the architecture most likely to survive tomorrow's changes.
