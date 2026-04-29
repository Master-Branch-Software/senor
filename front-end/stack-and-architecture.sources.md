# Sources — stack-and-architecture.md

Citations backing claims in [`stack-and-architecture.md`](stack-and-architecture.md). Developer artifact — not loaded by skill consumers. Update in the same commit when chapter rules change.

Per `CONTRIBUTING.md`:

- Human-authored sources only (specs, MDN, established engineering blogs, production codebases, books). No AI-generated content.
- Cite the specific claim, not just the topic.
- Mark uncited rules inline ("uncited — verify before publishing") rather than fabricating sources.

## Articles referenced

Component / repo organization:

- [Frontend Masters — Architecture through component colocation](https://frontendmasters.com/blog/architecture-through-component-colocation/)
- [Frontend Masters — When Deno or Bun is a better solution than Node.js](https://frontendmasters.com/blog/when-deno-or-bun-is-a-better-solution-than-node-js/)
- [Turborepo — Structuring a repository](https://turborepo.dev/docs/crafting-your-repository/structuring-a-repository) — backs the `apps/` + `packages/` monorepo convention and the rule against importing across package boundaries via relative paths.

Application architecture patterns:

- [Alistair Cockburn — Hexagonal Architecture (2005)](https://alistair.cockburn.us/hexagonal-architecture/) — canonical source for ports/adapters and the goal of "developed and tested in isolation from its eventual run-time devices and databases." (Site occasionally returns expired-cert errors; use the Wikipedia mirror as a stable backup.)
- [Wikipedia — Hexagonal architecture (software)](<https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)>) — secondary mirror for the Cockburn pattern definition.
- [Robert C. Martin — The Clean Architecture (2012)](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) — Dependency Rule ("source code dependencies can only point inwards") and the four concentric layers (entities, use cases, interface adapters, frameworks/drivers).
- [Jimmy Bogard — Vertical Slice Architecture (2018)](https://www.jimmybogard.com/vertical-slice-architecture/) — coining of the term and the principle "minimize coupling between slices, and maximize coupling in a slice."
- [Three Dots Labs — Is Clean Architecture Overengineering?](https://threedots.tech/episode/is-clean-architecture-overengineering/) — backs the "adopt when team has shipped it before, decline for small teams or simple projects" framing.
- [Martin Fowler — DomainDrivenDesign (2020)](https://martinfowler.com/bliki/DomainDrivenDesign.html) — DDD overview and attribution to Eric Evans's _Domain-Driven Design: Tackling Complexity in the Heart of Software_ (Addison-Wesley, 2003); names the Evans Classification (entities, value objects, services), aggregates, bounded contexts, ubiquitous language.
- [Martin Fowler — BoundedContext (2014)](https://martinfowler.com/bliki/BoundedContext.html) — definition of bounded context as "a central pattern in DDD" and the polysemic-term example (a utility company where "meter" meant three different things).

Frontend architecture methodology:

- [Feature-Sliced Design — Overview](https://feature-sliced.design/docs/get-started/overview) — backs the six-layer list (`app`, `pages`, `widgets`, `features`, `entities`, `shared`; `processes` is deprecated in v2.1) and the dependency rule "modules on one layer can only know about and import from modules from the layers strictly below."

## Note on tool catalogs

The catalogs below are vetted recommendations the chapter may surface. When a specific architectural claim depends on one of them ("pnpm + Turborepo for monorepos because…"), cite the engineering blog, benchmark, or post-mortem here in addition to listing the tool.

## Frameworks

- Next.js — `https://nextjs.org/`
- Astro — `https://astro.build/`
- SvelteKit — `https://svelte.dev/docs/kit/introduction`
- React Router v7 / Remix — `https://reactrouter.com/`
- Nuxt — `https://nuxt.com/`
- SolidStart — `https://start.solidjs.com/`
- Qwik — `https://qwik.dev/`

## Runtimes

- Node.js — `https://nodejs.org/`
- Bun — `https://bun.sh/`
- Deno — `https://deno.com/`

## Backend frameworks and APIs

- Hono — `https://hono.dev/`
- Fastify — `https://fastify.dev/`
- NestJS — `https://nestjs.com/`
- Express — `https://expressjs.com/`
- FastAPI — `https://fastapi.tiangolo.com/`
- Django — `https://www.djangoproject.com/`
- Flask — `https://flask.palletsprojects.com/`
- chi (Go) — `https://go-chi.io/`
- Fiber (Go) — `https://gofiber.io/`
- Phoenix (Elixir) — `https://www.phoenixframework.org/`
- Axum (Rust) — `https://docs.rs/axum/`
- tRPC — `https://trpc.io/`
- Apollo GraphQL — `https://www.apollographql.com/`
- Pothos — `https://pothos-graphql.dev/`
- gRPC — `https://grpc.io/`

## Documentation site generators

- Starlight (Astro) — `https://starlight.astro.build/`
- Docusaurus — `https://docusaurus.io/`
- VitePress — `https://vitepress.dev/`
- Nextra — `https://nextra.site/`
- Mintlify — `https://docs.mintlify.com/`
- Pagefind (search) — `https://pagefind.app/`

## Hosting and deployment

- Vercel — `https://vercel.com/`
- Netlify — `https://www.netlify.com/`
- Cloudflare Workers / Pages — `https://workers.cloudflare.com/`
- Fly.io — `https://fly.io/`
- Deno Deploy — `https://deno.com/deploy`
- Railway — `https://railway.com/`
- Render — `https://render.com/`
- GitHub Pages — `https://pages.github.com/`
- DigitalOcean — `https://www.digitalocean.com/`
- Hetzner Cloud — `https://www.hetzner.com/cloud`
- Coolify (self-hosted PaaS) — `https://coolify.io/`
- Dokku (self-hosted PaaS) — `https://dokku.com/`
- SST (IaC for serverless) — `https://sst.dev/`
- Terraform — `https://www.terraform.io/`
- Pulumi — `https://www.pulumi.com/`
- AWS CDK — `https://aws.amazon.com/cdk/`

## Monorepo and build tooling

- Turborepo — `https://turbo.build/`
- Nx — `https://nx.dev/`
- pnpm workspaces — `https://pnpm.io/workspaces`
- moonrepo — `https://moonrepo.dev/`
- Vite — `https://vitejs.dev/`
- Rollup — `https://rollupjs.org/`
- esbuild — `https://esbuild.github.io/`
- Rolldown — `https://rolldown.rs/`
- SWC — `https://swc.rs/`
- Biome — `https://biomejs.dev/`
- unbuild (UnJS) — `https://github.com/unjs/unbuild`

## Module boundary enforcement

- eslint-plugin-boundaries — `https://github.com/javierbrea/eslint-plugin-boundaries` (declarative architectural rules tied to folder patterns; docs at `https://jsboundaries.dev/`)
- eslint-plugin-import — `https://github.com/import-js/eslint-plugin-import` (zone-based restriction via `no-restricted-paths`: `https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-restricted-paths.md`)
- dependency-cruiser — `https://github.com/sverweij/dependency-cruiser` (graph analysis, default-rule cycle detection, CI-friendly text output)

## Databases and ORMs

Postgres-flavor

- PostgreSQL — `https://www.postgresql.org/`
- SQLite — `https://www.sqlite.org/`
- DuckDB (analytics) — `https://duckdb.org/`
- Turso (libSQL) — `https://turso.tech/`
- Neon (serverless Postgres) — `https://neon.tech/`
- Supabase Postgres — `https://supabase.com/database`
- Crunchy Bridge — `https://www.crunchybridge.com/`
- AWS RDS for Postgres — `https://aws.amazon.com/rds/postgresql/`
- Google Cloud SQL — `https://cloud.google.com/sql`
- PlanetScale — `https://planetscale.com/`

KV, cache, streams

- Redis — `https://redis.io/`
- Valkey — `https://valkey.io/`
- Upstash — `https://upstash.com/`

OLAP and time-series

- ClickHouse — `https://clickhouse.com/`
- Timescale — `https://www.timescale.com/`

Other

- MongoDB — `https://www.mongodb.com/`
- MongoDB Atlas — `https://www.mongodb.com/atlas`
- Dgraph (graph) — `https://dgraph.io/`

ORMs and query builders

- Prisma — `https://www.prisma.io/`
- Drizzle — `https://orm.drizzle.team/`
- Kysely — `https://kysely.dev/`
- MikroORM — `https://mikro-orm.io/`
- TypeORM — `https://typeorm.io/`
- SQLAlchemy — `https://www.sqlalchemy.org/`

## Authentication and identity providers

- Clerk — `https://clerk.com/`
- Auth.js — `https://authjs.dev/`
- better-auth — `https://www.better-auth.com/`
- Supabase Auth — `https://supabase.com/auth`
- Firebase Auth — `https://firebase.google.com/docs/auth`
- WorkOS — `https://workos.com/`
- Auth0 — `https://auth0.com/`
- Okta — `https://www.okta.com/`
- Microsoft Entra ID — `https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id`
- Keycloak (self-hosted) — `https://www.keycloak.org/`
- Ory (self-hosted) — `https://www.ory.sh/`

(Protocol-level references — WebAuthn, passkeys — live in the security chapter sidecar.)

## Headless CMS

- Sanity — `https://www.sanity.io/`
- Payload — `https://payloadcms.com/`
- Contentful — `https://www.contentful.com/`
- Storyblok — `https://www.storyblok.com/`
- Strapi — `https://strapi.io/`
- Directus — `https://directus.io/`
- Hygraph — `https://hygraph.com/`
- Prismic — `https://prismic.io/`
- Decap CMS (formerly Netlify CMS) — `https://decapcms.org/`
- Tina — `https://tina.io/`
- Keystone — `https://keystonejs.com/`
- Cosmic — `https://www.cosmicjs.com/`
- Jamstack CMS comparison — `https://jamstack.org/headless-cms/`

## E-commerce

- Shopify — `https://www.shopify.com/`
- Shopify Headless Storefronts — `https://shopify.dev/docs/storefronts/headless`
- Next.js Commerce template — `https://vercel.com/templates/next.js/nextjs-commerce`
- BigCommerce — `https://www.bigcommerce.com/`
- Medusa — `https://medusajs.com/`
- Commerce Layer — `https://commercelayer.io/`
- Saleor — `https://saleor.io/`
- Swell — `https://swell.is/`
- Shopware — `https://www.shopware.com/`
- WooCommerce — `https://woocommerce.com/`

## Payments and billing

- Stripe — `https://stripe.com/`
- Stripe Billing — `https://docs.stripe.com/billing`
- Stripe Checkout — `https://docs.stripe.com/checkout`
- Paddle — `https://paddle.com/`
- Lemon Squeezy — `https://www.lemonsqueezy.com/`
- Adyen — `https://www.adyen.com/`
- Mollie — `https://www.mollie.com/`
- Braintree — `https://www.braintreepayments.com/`
- Polar — `https://polar.sh/`

## Email and transactional

- Resend — `https://resend.com/`
- React Email — `https://react.email/`
- Postmark — `https://postmarkapp.com/`
- SendGrid — `https://sendgrid.com/`
- Loops — `https://loops.so/`
- Mailgun — `https://mailgun.com/`
- AWS SES — `https://aws.amazon.com/ses/`
- MJML — `https://mjml.io/`

## Web and product analytics

- Plausible — `https://plausible.io/`
- Fathom — `https://usefathom.com/`
- PostHog — `https://posthog.com/`
- Umami — `https://umami.is/`
- Mixpanel — `https://mixpanel.com/`
- Amplitude — `https://amplitude.com/`
- Google Analytics — `https://developers.google.com/analytics`
- Simple Analytics — `https://www.simpleanalytics.com/`

## Search

- Algolia — `https://www.algolia.com/`
- Meilisearch — `https://www.meilisearch.com/`
- Typesense — `https://typesense.org/`
- Elasticsearch — `https://www.elastic.co/elasticsearch`
- OpenSearch — `https://opensearch.org/`
- Postgres full-text search — `https://www.postgresql.org/docs/current/textsearch.html`
- Pagefind (static-site search) — `https://pagefind.app/`

## Vector stores and embeddings

- pgvector — `https://github.com/pgvector/pgvector`
- Qdrant — `https://qdrant.tech/`
- Weaviate — `https://weaviate.io/`
- Pinecone — `https://www.pinecone.io/`
- Chroma — `https://www.trychroma.com/`
- LanceDB — `https://lancedb.com/`
- Supabase AI guides — `https://supabase.com/docs/guides/ai`

## Background jobs and queues

- BullMQ — `https://docs.bullmq.io/`
- Inngest — `https://www.inngest.com/`
- Trigger.dev — `https://trigger.dev/`
- Temporal — `https://temporal.io/`
- Defer — `https://www.defer.run/`
- Quirrel — `https://quirrel.dev/`
- Apache Kafka — `https://kafka.apache.org/`
- Redis Streams — `https://redis.io/docs/latest/develop/data-types/streams/`

## AI, agents, and evaluation

- Vercel AI SDK — `https://sdk.vercel.ai/`
- Mastra — `https://mastra.ai/`
- LangGraph — `https://langchain-ai.github.io/langgraph/`
- LlamaIndex — `https://www.llamaindex.ai/`
- Pydantic AI — `https://ai.pydantic.dev/`
- LangChain (Python) — `https://python.langchain.com/`
- LangSmith (eval) — `https://smith.langchain.com/`
- Langfuse (eval) — `https://langfuse.com/`
- Braintrust (eval) — `https://braintrust.dev/`
- OpenAI API — `https://openai.com/api/`
- Anthropic Claude API — `https://docs.anthropic.com/`
- Google Gemini API — `https://ai.google.dev/`
- Model Context Protocol — `https://modelcontextprotocol.io/`

## Real-time and collaboration

- Liveblocks — `https://liveblocks.io/`
- PartyKit — `https://www.partykit.io/`
- Yjs (CRDT) — `https://yjs.dev/`
- Automerge (CRDT) — `https://automerge.org/`
- Ably — `https://ably.com/`
- Pusher — `https://pusher.com/`
- Socket.IO — `https://socket.io/`
- Supabase Realtime — `https://supabase.com/realtime`
- Centrifugo — `https://centrifugal.dev/`

## Component primitives and headless UI

- Radix UI — `https://www.radix-ui.com/`
- React Aria / React Aria Components — `https://react-spectrum.adobe.com/react-aria/`
- Headless UI — `https://headlessui.com/`
- shadcn/ui — `https://ui.shadcn.com/`
- Chakra UI — `https://chakra-ui.com/`
- Ark UI (framework-agnostic) — `https://ark-ui.com/`

## State management

- TanStack Query — `https://tanstack.com/query`
- SWR — `https://swr.vercel.app/`
- Zustand — `https://github.com/pmndrs/zustand`
- Jotai — `https://jotai.org/`
- Nanostores — `https://github.com/nanostores/nanostores`
- Signal polyfill — `https://github.com/proposal-signals/signal-polyfill`

## Forms and validation

- React Hook Form — `https://react-hook-form.com/`
- TanStack Form — `https://tanstack.com/form`
- Zod — `https://zod.dev/`
- Valibot — `https://valibot.dev/`

## Data, schemas, APIs

- JSON Schema — `https://json-schema.org/`
- OpenAPI — `https://www.openapis.org/`
- GraphQL — `https://graphql.org/`
- tRPC — `https://trpc.io/`
