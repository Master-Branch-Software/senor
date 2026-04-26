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

## Project type to stack

Stack decisions cascade: framework, backend, database, CMS, auth, email, hosting. The worst outcome is picking each piece independently and discovering six months in that they do not compose. Pick the shape of the project first, then apply a known-good recipe. Deviate only for a specific, documented reason.

### Marketing site or blog (content-first, SEO-critical)

- Rendering: SSG with selective ISR.
- Framework: Astro (islands, minimal JS) or Next.js when React is already the team standard.
- Styling: Tailwind v4 with a design-token layer, or plain CSS with `@layer`.
- Content: headless CMS for non-technical editors (Sanity, Payload, Storyblok); MDX in the repo when only developers publish.
- Database: none, or content-only in the CMS.
- Auth: none.
- Hosting: Vercel, Netlify, Cloudflare Pages.
- Analytics: Plausible, Fathom, or another privacy-first alternative.
- Budget tier: $0–$50/month at meaningful traffic.

### Documentation site

- Rendering: SSG.
- Framework: Astro with Starlight, Nextra, Docusaurus, or VitePress.
- Content: MDX in the repo, component imports, versioning via Git.
- Hosting: Vercel, Netlify, Cloudflare Pages, or GitHub Pages for public OSS docs.
- Must-haves: full-text search (Pagefind or Algolia), version/branch switching if shipping against multiple product versions.

### Small SaaS / B2B app (up to ~10k users)

- Framework: Next.js App Router with React Server Components.
- Database: PostgreSQL (Neon, Supabase, or self-hosted).
- ORM: Drizzle for serverless-friendly projects, Prisma for a mature DX at the cost of bundle size.
- Auth: Clerk for fastest setup, Auth.js for self-hosted, Better Auth for self-hosted with a modern API.
- Email: Resend + React Email for transactional.
- Payments: Stripe Checkout or Stripe Billing for subscriptions.
- Hosting: Vercel for the app, Neon/Supabase for the database, optional Upstash Redis for cache and rate-limiting.
- Monitoring: Sentry for errors, PostHog or Plausible for product analytics.
- Budget tier: $50–$500/month depending on traffic.

### Dashboard / internal tool / data-heavy admin

- Framework: Next.js or Remix / React Router v7.
- Data layer: TanStack Query for server state; TanStack Table plus `@tanstack/virtual` for large tables.
- Database: PostgreSQL; read replicas when report queries start to slow writes.
- API: tRPC for Next.js monorepos, REST with OpenAPI when multiple consumers exist, GraphQL only if the data graph justifies the operational cost (authorization, query-cost limits, caching strategy).
- Auth: Clerk, Auth.js, or the organization's SSO/OIDC provider.
- Charts: Recharts or Visx for interactive, ECharts for large/high-frequency series, D3 only when nothing pre-built fits.
- Hosting: Vercel, Railway, Fly.io, or a managed container platform when long-running jobs are involved.

### E-commerce

- Platform-first: Shopify (fastest to launch), BigCommerce, or a composable stack (Medusa, Commerce Layer).
- Storefront framework: Next.js (Next Commerce) or Astro.
- Headless Shopify: recommended when design freedom and performance matter; accept the operational trade-off of a separate catalog source.
- Payments: Shopify Payments, Stripe, or Adyen depending on the platform.
- Search: Algolia, Meilisearch, or Typesense.
- Hosting: Vercel/Netlify for the storefront, platform-managed for the commerce backend.

### Real-time or collaborative

- Transport: WebSockets via socket.io, Ably, Pusher, or managed platforms (Liveblocks, PartyKit).
- CRDTs: Yjs or Automerge for collaborative editing.
- Backend: Node.js, Bun, Deno, or Go depending on connection counts; long-lived processes require containers, not short-lived serverless functions.
- Database: PostgreSQL plus Redis; consider Redis Streams or a message broker for high fan-out.
- Hosting: Fly.io, Railway, Render, or a container platform. Stateless serverless is rarely the right fit for stateful real-time.

### Mobile-first consumer web app / PWA

- Framework: Next.js with careful bundle budgets, or SvelteKit / Qwik for smaller payloads.
- Offline: Workbox-based service worker with explicit cache strategies per route.
- Auth: passkeys-first where possible, social providers as fallback.
- Performance budget: tighter than a marketing site; initial JS ≤ 100 KB gzipped for the shell.

### Internal-use app (no SEO, no anonymous access)

- Rendering: client-heavy is acceptable; a Vite + React or Vue SPA is fine.
- Backend: Node.js/Hono, Python/FastAPI, Go/Chi, or the organization's existing platform.
- Auth: SSO/OIDC; internal apps rarely need their own login.
- Hosting: internal Kubernetes, ECS, Cloud Run, or a PaaS. First-load performance is secondary to features.

### Enterprise B2B (RBAC, multi-tenant, audit, SSO)

- Framework: Next.js or a BFF in front of a Go / Java / C# service.
- Database: PostgreSQL with row-level security or tenant-scoped schemas; Redis for rate limiting.
- Auth: Auth0, Okta, Entra ID (Azure AD), or WorkOS for SAML and SCIM support.
- Audit: append-only event log, separate from the transactional store.
- Compliance: SOC 2, ISO 27001, GDPR — architected upfront, not retrofitted.
- Hosting: customer-preferred cloud; often AWS or Azure with customer-managed keys.

### AI-augmented app (RAG, assistants, agentic features)

- Orchestration: Python (LangGraph, LlamaIndex, pydantic-ai) for complex agentic flows; TypeScript (Vercel AI SDK, Mastra) for UI-adjacent chat and tool-use.
- Vector store: pgvector in PostgreSQL for early stages; Qdrant, Weaviate, or Pinecone when scale or specialized features demand it.
- Streaming: SSE or WebSockets; `ReadableStream` on the server, `useCompletion` / `useChat` on the client.
- Evaluation: datasets plus automated eval runs before shipping any prompt change. Treat prompts and model versions like code.

## Rendering strategy

Where the HTML is built — and when — drives perceived performance, SEO, and the operational shape of the app. Decide per route; most real apps mix strategies rather than picking a single one.

### Static site generation (SSG)

HTML is rendered at build time and served from a CDN. Nothing runs per request.

- Fits: marketing, docs, blogs, product pages with infrequent updates, legal pages.
- Strengths: fastest TTFB anywhere on the globe; cheapest infrastructure; smallest attack surface.
- Weaknesses: build time grows with page count; content updates require a rebuild (mitigated by ISR); personalization cannot happen at build time.
- Frameworks: Astro (the default model), Next.js (`output: 'export'` or per-route `dynamic = 'force-static'`), SvelteKit (`prerender = true`), Nuxt, Eleventy, Hugo, Jekyll.

### Server-side rendering (SSR)

HTML is rendered per request. The browser gets a full page; the server hits the database or CMS on each hit.

- Fits: personalized dashboards, authenticated content, frequently-changing data, pages that depend on the request (geolocation, user agent, cookies).
- Strengths: fresh data on every response; works without client JS for the first paint; SEO-clean for logged-out routes.
- Weaknesses: server latency is on every request; CDN caching must be explicit via `Cache-Control` and `Vary`; cold starts can hurt on serverless platforms.
- Frameworks: Next.js App Router (default for dynamic routes), Remix / React Router v7, SvelteKit, Nuxt; server-first stacks like Rails, Django, Phoenix, Laravel.

### Incremental static regeneration (ISR) and on-demand revalidation

Static HTML cached like SSG but regenerated in the background after a TTL expires or when the CMS fires a webhook.

- Fits: product catalogs, pricing pages, aggregated feeds, semi-dynamic content where minutes of staleness are acceptable.
- Strengths: static performance with near-real-time updates; no manual rebuild for one-off changes; tag-based invalidation (Next.js `revalidateTag`) keeps cache keys reasoning straightforward.
- Weaknesses: stale data between revalidations; time-based TTLs are harder to reason about than tag-based invalidation; not every host supports background regeneration well.
- Frameworks: Next.js (`revalidate`, `revalidateTag`, `revalidatePath`), SvelteKit with ISR adapter, Astro with webhook-triggered rebuilds.

### Streaming SSR

HTML is flushed to the browser in chunks as data resolves. Slow parts of a page do not block fast parts.

- Fits: pages that mix a fast-rendering shell with slow data calls (dashboards, mixed-source feeds, product pages that embed reviews from another service).
- Strengths: good TTFB even when some data is slow; progressive hydration as chunks arrive; a clear Suspense boundary model when combined with React Server Components.
- Weaknesses: requires a framework and host that support streaming end-to-end; debugging is harder; some third-party scripts assume a single-shot DOM.
- Frameworks: Next.js App Router with Suspense, Remix with `defer`, SvelteKit streamed data, React Router v7.

### Client-side rendering (CSR) and single-page apps

The server sends a near-empty shell; the browser downloads JavaScript, calls APIs, and renders everything.

- Fits: authenticated apps with no SEO requirements (admin panels, internal tools, dashboards behind a login, Electron-wrapped products).
- Strengths: simplest server infrastructure; app-like behaviour after the first load; hostable on any static CDN.
- Weaknesses: slow first paint, poor SEO for anonymous content, large JS bundle, strict bundle discipline required to stay fast on low-end devices.
- Frameworks: Vite + React / Vue / Solid, TanStack Router, Nuxt in SPA mode, SvelteKit with the SPA adapter.

### Edge rendering

Code runs at a CDN edge close to the user rather than in a single origin region.

- Fits: globally distributed audiences, personalization at the edge, request-level A/B testing, authentication checks before origin, geolocation-based redirects.
- Strengths: low latency worldwide; cheap when the workload fits the edge runtime (no long-running processes, no heavy native modules); near-instant rewrites and redirects.
- Weaknesses: the edge runtime is a subset of Node (no `fs`, limited npm packages, shorter execution budgets); cold starts exist though smaller; databases must expose HTTP-friendly, connection-pooled clients; some APIs are vendor-specific.
- Platforms: Cloudflare Workers, Vercel Edge, Netlify Edge, Deno Deploy, Fastly Compute.

### Islands architecture / partial hydration

The page ships HTML-first; interactive components are explicitly opted into and hydrated independently.

- Fits: mostly-static pages with a few interactive components (e-commerce, marketing, docs, content sites with a few rich widgets).
- Strengths: minimal JavaScript; the non-interactive majority ships zero client JS; interactive islands load their own JS lazily.
- Weaknesses: cross-island state sharing is awkward and often requires a shared signal library; mental model differs from SPA thinking.
- Frameworks: Astro (islands are the default), Fresh (Deno), Eleventy with `is-land`.

### Resumability

Instead of hydrating (re-executing the component tree on the client), the framework serializes the state of handlers so the client resumes them on first interaction.

- Fits: highly interactive pages that must load instantly on slow networks or low-end devices.
- Strengths: close to zero hydration cost; the app is interactive before JS finishes downloading in many cases.
- Weaknesses: Qwik is the main implementation; smaller ecosystem; requires thinking about serialization boundaries.
- Frameworks: Qwik, QwikCity.

### React Server Components (RSC)

Components render on the server and never ship their JS to the client. Client components are opt-in (`"use client"`).

- Fits: apps where most of the tree is data-fetching and layout, with interactive islands scattered through.
- Strengths: eliminates client-side data-fetching waterfalls; smaller bundles; keeps secrets and database queries on the server by construction; pairs naturally with Suspense and streaming.
- Weaknesses: new mental model around server / client boundaries; ecosystem still catching up on library compatibility; some libraries remain client-only by design.
- Frameworks: Next.js App Router, Waku, Redwood SDK.

### Choosing per route

Most real apps use several strategies at once. A sensible default:

- Home, marketing, pricing, blog, docs — SSG or ISR.
- Public dynamic pages (user profiles, product pages with live stock, search results) — SSR or ISR with a short TTL.
- Authenticated dashboards — SSR with React Server Components, or CSR behind the login when SEO is not a concern.
- Real-time collaboration surfaces — CSR over WebSockets on top of an SSR shell.
- Personalization or geolocation — edge SSR, or edge middleware rewriting to the right origin response.

### Anti-patterns

- SPA for a marketing site with SEO goals. The JS-only shell loses to SSG on every metric.
- SSR for static content "just in case." Rebuild times beat per-request CPU on every axis.
- Hand-rolling a prerender step when the framework already provides SSG or ISR.
- Forcing an entire app into the edge runtime when half the code needs Node built-ins.
- Hydrating a fully-static page — if there is no interactivity, there is no reason to ship the framework runtime.

## Backend and API style

The framework question often hides the backend question. Pick the API style that matches who will call it.

### The short decision

- Same-team React / Next frontend with no other clients: Next.js Route Handlers or Server Actions. Add tRPC when typed contracts across packages are valuable.
- Same-team frontend plus one mobile app: tRPC or typed REST, with types generated from a single source of truth.
- Multiple clients with different teams, or long-lived partner integrations: REST with OpenAPI. Generate clients. Version explicitly (URL `/v1`, `/v2`, or header-based).
- Deeply-nested data graphs queried by varied clients with different field needs: GraphQL, and budget for the operational work it requires.
- Service-to-service with tight coupling inside one organisation: gRPC or Connect.
- Real-time state: WebSockets, SSE, or a managed real-time platform layered on top of the main API.
- Third parties calling in: webhooks with signed payloads, idempotency keys, and fast 2xx responses.

### REST with OpenAPI

The boring default. Still the right call for most public and partner APIs.

- Strengths: every client language speaks it; HTTP caching works out of the box; `curl` debugging is trivial; versioning and deprecation stories are well-understood.
- Weaknesses: over-fetching and under-fetching on complex data; `n+1` calls from the client unless composite endpoints are designed; type safety must be layered on via OpenAPI plus codegen.
- Discipline: define the contract first in OpenAPI; generate server stubs and client SDKs from it; document error shapes explicitly (RFC 7807 / Problem Details is a good baseline); use ETags and `Cache-Control` deliberately; version in the URL for major-breaking changes and in headers for minor additions.
- Libraries: Hono, Fastify, NestJS, Express, FastAPI, Django REST Framework, Chi, Axum. Client codegen via openapi-typescript, orval, or hey-api.

### tRPC

End-to-end TypeScript contracts with no schema file and no codegen step. Wins when the frontend and backend live in the same repo.

- Strengths: rename a procedure on the server, get a type error on the client immediately; zero boilerplate; integrates with Zod for runtime validation; works over HTTP, WebSockets, or React Server Components.
- Weaknesses: TypeScript-only on both ends; not an option for partner APIs or language-diverse clients; opaque to tools that expect OpenAPI or GraphQL introspection.
- Use when: the frontend team is the only caller, the monorepo ships both ends together, and the team values type safety over an open protocol.

### GraphQL

A query language and runtime for data graphs. Powerful and expensive. The main reason to pick it is many clients with different field needs; the main reason to avoid it is underestimating the operational cost.

- Strengths: clients request only the fields they need; one endpoint for many shapes; subscriptions for real-time; a schema-first workflow that maps well to design reviews; federation lets multiple teams own parts of the graph.
- Weaknesses: authorization is required on every resolver, not every route; unbounded query cost is a denial-of-service vector unless cost limits and persisted queries are in place; HTTP caching does not work out of the box; debugging is harder than REST; tooling investment is real.
- Required discipline: persisted queries or trusted documents in production; query-cost analysis plus depth and complexity limits; field-level authorization; careful N+1 handling via DataLoader; response caching via a GraphQL-aware layer (Apollo Router, Stellate) rather than CDN defaults; a clear deprecation policy (`@deprecated` plus analytics).
- Pick it when: the data is genuinely a graph, varied clients need varied slices, and the team has capacity to operate it well. Do not pick it as a default for a single-frontend app.
- Libraries: Apollo Server, Pothos, GraphQL Yoga, Mercurius, graphql-ruby, strawberry (Python). Client: Apollo Client, urql, Relay.

### Server Actions and framework-native RPC

Functions declared on the server and called from the client as if local. Modern frameworks hide the HTTP plumbing.

- Strengths: the least boilerplate path from "I need to save this form" to shipped; typed end-to-end; pairs naturally with Server Components; progressive enhancement is free when forms submit to the same endpoint.
- Weaknesses: framework-specific (Next.js Server Actions, SvelteKit form actions, Remix actions); not an API for external clients; magical behaviour can surprise teams that expect explicit routes.
- Use when: the caller is the same-framework frontend and nothing else. Expose a REST, tRPC, or GraphQL layer alongside if any other consumer appears.

### gRPC, Connect, and binary RPC

Schema-first RPC with generated clients in many languages. The right call for high-throughput service-to-service traffic.

- Strengths: compact binary protocol; bidirectional streaming; strict typed contracts via Protobuf; mature clients in Go, Java, Rust, C#, Node, Python.
- Weaknesses: browsers cannot speak native gRPC; browser clients need gRPC-Web or Connect; more operational overhead (proto toolchain, load balancers, tracing); overkill for most public APIs.
- Use when: services call services inside one organisation and latency or throughput matters, or when polyglot teams need a strict contract.

### WebSockets and Server-Sent Events

WebSockets for bidirectional state; SSE for server-to-client push with automatic reconnection.

- WebSockets fit real-time collaboration, chat, presence, live dashboards, multiplayer. They require stateful hosting (not short-lived serverless functions).
- SSE fits AI token streaming, notifications, and live feeds where the client only needs to receive. SSE is simpler than WebSockets, works over plain HTTP, and is supported by every browser.
- Libraries: socket.io, the native `WebSocket`, `ws`, `hono/streaming`, `@hono/node-ws`. Managed platforms: Ably, Pusher, Liveblocks, PartyKit, Supabase Realtime.

### Webhooks (when third parties call in)

- Always verify the signature (HMAC with a shared secret, or public-key).
- Always treat delivery as at-least-once; require idempotency keys on handlers.
- Always return a 2xx response quickly; offload processing to a queue. Blocking a webhook endpoint on slow work guarantees duplicate deliveries and provider-side retries.
- Log every incoming webhook with its provider-side ID for replay and debugging.
- Expose a replay endpoint or tooling so support can re-run a missed event without asking the provider.

### Backend languages and runtimes

For standalone backend services, pick by workload profile, not trend:

- Node.js (TypeScript) with Hono, Fastify, or NestJS — default when the frontend is TypeScript and the team is small.
- Python (FastAPI) — default when AI/ML, data pipelines, or scientific computing are core.
- Go (Chi, Fiber) — default when latency, memory, and concurrency dominate (real-time, infrastructure, edge).
- Elixir (Phoenix) — strong pick for real-time-heavy workloads when team familiarity exists.
- Rust (Axum) — justified when raw performance or strict memory control is the product requirement.
- Ruby (Rails), Java (Spring), C# (.NET) — correct when the organisation already runs them at scale; do not adopt new.

### API-style anti-patterns

- Shipping REST, GraphQL, and tRPC endpoints for the same data. Pick one primary contract and, if needed, expose a thin adapter to a second.
- Building a GraphQL layer as "future-proofing" before any client needs the flexibility. It is operational debt until then.
- Treating Server Actions as a public API. They are framework glue, not an external contract.
- Running WebSockets on a stateless serverless runtime. Use containers or a managed real-time platform.
- Versioning REST silently by changing field behaviour. Breaking changes require a new version and a deprecation window.
- REST endpoints that return 200 with an `error` field in the body. Use HTTP status codes; clients rely on them.

## Database selection

- Relational default: PostgreSQL. Handles JSONB, full-text search, geospatial, and partitioning in a single engine. Use it unless a concrete requirement rules it out.
- Embedded or single-machine: SQLite, or libSQL/Turso for distributed read replicas. Excellent for desktop, CLI, prototypes, and apps with one primary writer.
- Document-oriented: MongoDB only when the data model is genuinely schema-flexible and relationships are shallow. "We might add fields later" is PostgreSQL+JSONB territory, not Mongo.
- Analytical / time-series: ClickHouse, TimescaleDB, or DuckDB. Do not force OLAP shapes onto OLTP Postgres.
- Key-value / cache / queue: Redis for caching, sessions, rate limiting, and lightweight pub/sub; Valkey as an open-source drop-in.
- Search: Postgres full-text at small scale; Meilisearch or Typesense for self-hosted; Algolia or Elasticsearch for scale.
- Vector: pgvector until query patterns or scale justify a dedicated store (Qdrant, Weaviate, Pinecone).

Managed Postgres options include Neon (serverless, branching), Supabase (Postgres plus auth, storage, and edge functions), Railway, Fly Postgres, AWS RDS/Aurora, and Google Cloud SQL. Choose based on hosting locality and scaling model rather than a feature-checklist match.

## Content management

"Should the project use a CMS" is three questions: who edits content, how often, and whether the same content needs to appear in more than one surface.

- Developer edits only, rare changes: Markdown or MDX in the repo. Git is the CMS. Preview via PR.
- Non-technical editor, web only, small team: Sanity (real-time, customizable Studio) or Payload (TypeScript-native, lives inside a Next.js app).
- Marketing team with visual expectations: Storyblok (visual editor, component blocks).
- Multi-channel content (web + mobile + IoT + email): Sanity or Contentful.
- Enterprise governance, localization at scale, approval workflows: Contentful.
- Existing SQL database as source of truth: Directus (CMS layer over the database that already exists).
- Self-hosting is a hard requirement: Strapi v5 (SQL-only), Payload (Postgres or MongoDB), or Directus.

Rule of thumb: Markdown for docs. Headless CMS the moment a non-developer needs to publish without a deploy.

## Authentication providers

The chapter covers the security rules later; this is about picking the provider.

- Fastest to ship, hosted UI, social plus email: Clerk.
- Self-hosted, full control, modern API: Better Auth or Auth.js v5.
- Enterprise SSO (SAML, SCIM, directory sync): WorkOS, Auth0, Okta, Entra ID.
- Bundled with database and storage: Supabase Auth or Firebase Auth.
- Passkeys-first: every modern provider now supports WebAuthn; password-only auth is a choice that needs a justification, not a default.

Never hand-roll session management, password hashing, or SSO. The failure modes are well-studied and expensive, and the compliance surface is large.

## Hosting selection

Match the runtime to the workload, not to brand loyalty.

- Stateless web and API: Vercel (Next.js), Cloudflare Pages/Workers (global edge), Netlify (Jamstack), Render.
- Containers, long-running processes, WebSockets: Fly.io, Railway, Render, AWS ECS/Fargate, Google Cloud Run.
- Heavy custom infrastructure and regulated workloads: AWS, Google Cloud, or Azure driven by Infrastructure-as-Code (Terraform, Pulumi, SST, CDK).
- Edge-first APIs and low-latency personalization: Cloudflare Workers, Deno Deploy, Vercel Edge.
- Self-hosted on a VPS: Coolify, Dokku, or plain Docker Compose on Hetzner, DigitalOcean, or Linode when predictable billing matters more than autoscaling.

## Reference stacks

Known-good recipes. Pick the closest match and deviate only for a cited reason.

### Content site (marketing + blog)

Astro + Tailwind v4 + Sanity (or MDX) + Vercel or Netlify + Plausible + Resend for form submissions.

### Small SaaS

Next.js + Tailwind v4 + shadcn/ui + Drizzle + Postgres (Neon) + Clerk + Stripe + Resend + Vercel + Sentry + PostHog.

### Dashboard or internal tool

Next.js + Tailwind + shadcn/ui + TanStack Query + TanStack Table + Postgres (Neon or self-hosted) + Prisma or Drizzle + Auth.js + Vercel or Railway.

### Real-time collaborative app

Next.js or SvelteKit UI + Liveblocks, PartyKit, or a custom WebSocket service on Fly.io + Yjs + Postgres + Redis.

### E-commerce storefront

Next.js Commerce or Astro + Shopify headless + Algolia + Vercel + Stripe.

### Content-heavy editorial site

Astro + Sanity + Cloudinary or Sanity Images + Vercel + Plausible.

### AI-augmented SaaS

Next.js + Tailwind + Vercel AI SDK on the client + FastAPI or Next.js API + pgvector in Postgres + Clerk + Stripe + LangSmith or Langfuse for evaluation and observability.

## Scaling decision ladder

Take each step when the current step actually hurts, not before. Most apps never reach the bottom.

1. Single process, single server, single database. Handles tens of thousands of daily users for most apps.
2. Add a read replica when read queries dominate and write latency starts to suffer.
3. Add Redis for session, cache, and rate limiting when hot paths repeat.
4. Split static assets to a CDN. Enable fingerprinted, immutable caching.
5. Queue slow work (email, image processing, webhooks) with BullMQ, Inngest, or Trigger.dev before reaching for Kafka.
6. Move background workers to dedicated containers. Decouple their deploy cadence from the web app.
7. Introduce a search index when `LIKE` queries or full-text search on the primary database become the bottleneck.
8. Add a cache-ahead layer (Redis or a CDN with stale-while-revalidate) for expensive query paths.
9. Partition or shard the database when a single primary cannot hold the working set. Table partitioning in Postgres first, sharding as a last resort.
10. Split the monolith only when team topology or deploy contention demands it. Microservices are an organizational solution, not a performance one.

The anti-pattern: adopting Kubernetes, microservices, event sourcing, or CQRS for imagined scale. Each carries a real, ongoing operational tax. Earn the complexity by outgrowing the simpler choice.

## Maintainability principles

Stacks live for years. Optimize for the engineer reading the code six months after shipping.

- Prefer boring technology. Use novel tools only where they solve a specific, acknowledged problem.
- Keep the critical path in one language and runtime when possible. TypeScript across client and server removes whole categories of bugs at the boundary.
- Avoid bleeding edge in the hot path. Let the ecosystem find the sharp edges first.
- Avoid architectures that cannot be explained on a whiteboard in five minutes.
- Prefer platform primitives to libraries when the platform suffices (`fetch`, `IntersectionObserver`, `structuredClone`, `Intl`, `<dialog>`, `popover`). See the Dependency management section for the evaluation checklist.
- Document the "why" for any non-obvious choice. `ARCHITECTURE.md` is where an outsider should start.
- Delete rather than deprecate. Unused code confuses more than it helps.

## The boring-technology budget

Every project has a small budget for novel technology — usually one or two components. Spend it where the novel tool creates a measurable advantage the boring alternative cannot match. Spend it everywhere and the result is ten half-understood components instead of one well-understood system.

Safe boring picks in 2026: Next.js, PostgreSQL, Tailwind, shadcn/ui, Clerk, Resend, Stripe, Vercel, Sentry.

Novel technology worth the budget when it fits:

- SvelteKit for extreme bundle-size constraints.
- Qwik for resumable SSR.
- Bun / Deno for fast CLIs and single-binary deploys.
- Payload for tight Next.js integration when the CMS lives beside the app.
- Turso for edge-replicated SQLite.
- Supabase when "one platform for DB, auth, storage, functions" is genuinely valuable at the prototype stage.

Novel for its own sake (spend budget with care): any framework in v0.x, any database without production deployments at the intended scale, any architecture pattern whose main advocate is a conference talk.

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

### Logging standards

Chapter 16 covers the logger choice (`pino` on Node, `tslog` elsewhere) and the level taxonomy. This subsection covers the runtime contract the platform must enforce:

- Every request generates a correlation ID. Propagate it via `x-request-id` header and include it in every log line the request produces.
- Every log line is a single-line JSON object. No `console.log` statements reach production.
- Every log line carries `service`, `env`, `version`, `request_id`. User-identifying fields (`user_id`, `tenant_id`) are attached at the earliest point they are known.
- Sensitive fields (passwords, tokens, full payment details, personal identifiers) are redacted by the logger configuration, not by the caller. Redaction lives in one place.
- Log levels match severity, not verbosity. `error` means a human must act. `warn` means a potentially degraded state. `info` is the default operational narrative. `debug` is off in production unless a flag is flipped for a short window.
- Sampling is acceptable for high-volume `info` lines. Never sample `error`.
- Ship logs to a queryable backend (Grafana Loki, Datadog, Honeycomb, Axiom, CloudWatch Logs Insights). Local file logging is a development convenience, not a production strategy.

## Source control and collaboration

Chapter 16 details the exact commit format, tooling installation (Husky, Lefthook, lint-staged, commitlint), and release automation. This section captures the workflow and repository conventions every project inherits.

### Commit messages

Conventional Commits is the default on every new project. It enables automated changelogs, semantic versioning, and machine-readable history.

```text path=null start=null
<type>(<scope>): <subject>

<body>

<footer>
```

- Types: `feat`, `fix`, `perf`, `refactor`, `docs`, `test`, `build`, `ci`, `chore`, `revert`. `feat!` or a `BREAKING CHANGE:` footer triggers a major bump.
- Scope is optional but recommended; use package or feature name (`auth`, `billing`, `ui/button`).
- Subject is imperative, lowercase, no trailing period, ≤72 characters.
- Body explains the "why," wrapped at 72 characters. Reference tickets in the footer (`Refs: JIRA-123`).
- Enforce with `commitlint` + `@commitlint/config-conventional` on the `commit-msg` hook.

### Branch naming

- `main` is the only long-lived branch. No `develop`, no `staging` unless a specific deploy strategy requires it.
- Feature branches: `feat/short-description`, `fix/short-description`, `chore/short-description`. Include a ticket number when available: `feat/AUTH-42-passkeys`.
- Delete branches after merge. Never force-push to `main`.

### Merge strategy

Pick one per project and enforce it in branch-protection settings:

- Squash and merge (default): keeps `main` linear, one commit per PR, Conventional Commits applied to the squashed message.
- Rebase and merge: preserves individual commits when the branch history is already meaningful. Requires discipline from every contributor.
- Merge commit: only when preserving the branch topology carries real information (release branches, long-lived integration work).

### Pull request template

Every repository ships `.github/PULL_REQUEST_TEMPLATE.md` with four sections: Summary (what and why), Change details (list of notable changes), Testing (what was run), Checklist (tests, docs, accessibility, screenshots for UI). PRs that skip the template are sent back.

### CODEOWNERS and review

- `.github/CODEOWNERS` assigns default reviewers per path. Critical paths (auth, billing, schemas, infrastructure) require review from the owning team.
- Branch protection requires at least one approving review for every PR touching `main`. Protected paths require review from the CODEOWNERS entry.
- Every PR must pass CI (lint, type-check, test, build, bundle budget) before merge.
- Reviewer checklist: does the change match the ticket, is the test coverage appropriate, does the copy match the voice guide (chapter 02), does the styling honor the design tokens (chapter 04 and the token tiers).

### Git hooks

Chapter 16 covers setup details. Every repository installs hooks for:

- `pre-commit`: `lint-staged` running formatter and linter on changed files only.
- `commit-msg`: `commitlint` validating Conventional Commits.
- `pre-push` (optional): type check and fast unit tests.

Hooks must be installable automatically (`postinstall` script or Lefthook's self-install) so a fresh clone is compliant without manual steps.

### Signed commits

Enforce GPG or SSH-signed commits on any repository handling production code, secrets, or customer data. Require verified signatures in branch protection.

### Repository hygiene

- `.gitignore` excludes build artifacts, secrets, and OS detritus (`.DS_Store`, `Thumbs.db`, `node_modules`, `.env`, `dist/`, coverage reports).
- `.gitattributes` normalizes line endings (`* text=auto eol=lf`) and marks binaries.
- `.editorconfig` aligns indentation, charset, and final newline across editors.
- `.nvmrc` or `.tool-versions` pins the runtime used in development and CI.
- Secrets never enter the repository. Use a secret manager; commit `.env.example` with placeholder values only.

## Dependency management

Every new dependency is a liability. The cost surfaces as bundle size, security advisories, supply-chain risk, type incompatibilities, and eventual deprecation. Manage it like a budget.

### Evaluating a new dependency

Before adding a package, check:

- Last release date; weekly download volume.
- Open issue and PR counts; responsiveness to security reports.
- Bundle impact via `bundlephobia` or `package-size`.
- License compatibility (MIT, Apache-2.0, BSD are safe; GPL, AGPL, custom licenses require approval).
- Alternatives, including "no dependency." Platform APIs often suffice.
- Maintainer footprint (single maintainer is a risk; corporate backing or a steering committee reduces it).

### Package manager

- pnpm is the default. Fast, strict, disk-efficient, workspace-aware.
- npm is acceptable for single-package projects when toolchain mandates it.
- Bun's package manager is acceptable for solo Bun projects; mixing managers inside one repo is not.
- Yarn Classic is end-of-life; Yarn Berry is acceptable when a team already runs it at scale.

### Lockfile policy

- Commit the lockfile. Always. `pnpm-lock.yaml`, `package-lock.json`, or `bun.lockb`.
- Pin runtime and package manager via `packageManager` in `package.json` and enforce in CI.
- Prefer exact versions (`"react": "19.1.0"`) over ranges. Ranges drift silently; exact versions make upgrades an explicit event.
- Use `overrides` / `resolutions` to patch a transitive vulnerability rather than waiting for upstream.

### Upgrade automation

- Renovate is the default. Group patch and minor updates into a single weekly PR; major upgrades ship individually with release notes in the PR body.
- Dependabot is acceptable on GitHub-only workflows with simple graphs. Renovate wins at scale.
- Security advisories (GitHub Dependabot alerts, `npm audit`, `osv-scanner`) must block merges in CI.
- Review the weekly upgrade PR like any other change. Never merge without CI passing.

### Monorepo tooling

- Turborepo is the default task runner for pnpm workspaces. Remote caching on Vercel or a self-hosted store.
- Nx is acceptable when the project needs graph-aware generators, code splitting, and tight framework plugins.
- Moon is a newer contender worth evaluating for Rust-heavy or polyglot repositories.
- Keep the number of root-level tools minimal; chapter 15 covers the exact setup.

### Supply-chain hygiene

- Run `npm audit` / `pnpm audit` in CI and fail on high-severity advisories.
- Scan dependencies with `osv-scanner` or Socket.dev on every PR.
- Limit install-time scripts with `pnpm` `ignoreBuiltDependencies` or `npm` `--ignore-scripts` in CI; allowlist the ones needed.
- Keep `resolutions` / `overrides` documented in `ARCHITECTURE.md` so the next engineer understands why they exist.
- For binary dependencies, verify checksums; for Docker base images, pin digests, not tags.

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
