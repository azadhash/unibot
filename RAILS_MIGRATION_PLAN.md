# UniBot → Ruby on Rails Migration Plan

## 1) Current Project Snapshot (What exists today)

Based on the current repository:

- **Current app type:** Astro + Tailwind landing website (marketing site).
- **Primary runtime/build:** Node.js (`npm run dev/build/preview`).
- **Backend today:** No full Rails-like app backend yet; only a lightweight Supabase client setup for lead capture.
- **Deployment today:** Cloudflare Pages.

## 2) Existing Features to Preserve in Rails

## A. Public Marketing Website
- Hero + product positioning section.
- “How it works” section.
- Feature highlights section.
- Demo preview section.
- Use-case section.
- Pricing section.
- Security section.
- FAQ section.
- Footer/header navigation and CTA flow.

## B. Demo Booking Experience
- Embedded Cal.com iframe in the “Book Demo” section.
- “Contact us directly” mailto fallback.

## C. Product Scope Documented in the Repo
The product feature spec already targets a SaaS chatbot platform with:
- Real-time sync expectations.
- Secure/private handling.
- Data upload and training flow.
- Embed widget distribution.
- Pricing tiers and enterprise capabilities.
- Backend and frontend implementation checklist.

## 3) Target Rails Architecture (Recommended)

## A. Rails Stack
- **Ruby on Rails 8** (or latest stable Rails 7.1+ if team preference).
- **PostgreSQL** as primary database.
- **Hotwire (Turbo + Stimulus)** for server-rendered interactivity.
- **Tailwind CSS** in Rails for continuity with current visual style.
- **Devise** (or Sorcery/Auth0) for authentication.
- **Pundit** for authorization (roles/permissions).
- **Sidekiq + Redis** for async processing (documents, embeddings, sync jobs).
- **Active Storage** (S3/R2-compatible) for document upload pipeline.
- **Stripe** for billing/subscription plans.

## B. Data & AI Components
- Multi-tenant schema (account/workspace scoped tables).
- `pgvector` in Postgres (or external Pinecone if scaling requires).
- Service objects for LLM inference and RAG retrieval.
- Background jobs for ingestion/chunking/embedding.

## C. Deploy/Infra
- App hosting: Render/Fly/Heroku/AWS (team choice).
- Postgres managed service.
- Redis managed service.
- Object storage (Cloudflare R2 or S3).
- Environment secrets manager.

## 4) Feature Mapping: Astro → Rails

- Astro page composition (`src/pages/index.astro` + components) becomes Rails views/partials.
- Global styling stays Tailwind-based via Rails asset pipeline.
- Supabase lead capture can become:
  - Rails-native model (`Lead`) + controller + admin list, OR
  - Keep Supabase and call via API from Rails.
- Cal.com iframe can be retained in Rails view partial with the same embed URL.

## 5) Rails Migration Task List (Execution Backlog)

## Phase 0 — Discovery & Decisions
- [ ] Confirm migration scope:
  - [ ] “Marketing site only” first, or
  - [ ] Full SaaS app (auth, chatbots, docs, billing).
- [ ] Confirm Ruby/Rails versions.
- [ ] Choose auth approach (Devise vs external auth).
- [ ] Choose vector strategy (`pgvector` vs Pinecone).
- [ ] Choose deployment target and environments (dev/stage/prod).
- [ ] Define acceptance criteria for “migration done”.

## Phase 1 — Rails Foundation
- [ ] Initialize Rails app with PostgreSQL.
- [ ] Add Tailwind setup and base layout.
- [ ] Configure environment variables and credentials.
- [ ] Add CI (lint, test, security scan).
- [ ] Add error tracking/logging baseline.

## Phase 2 — Marketing Site Port
- [ ] Create Rails home controller/view.
- [ ] Port sections as partials:
  - [ ] Header
  - [ ] Hero
  - [ ] How It Works
  - [ ] Features
  - [ ] Demo Preview
  - [ ] Use Cases
  - [ ] Pricing
  - [ ] Security
  - [ ] Book Demo
  - [ ] FAQ
  - [ ] Footer
- [ ] Port global styles and design tokens.
- [ ] Ensure responsive parity with current Astro site.
- [ ] Add SEO metadata, sitemap, robots.

## Phase 3 — Lead Capture & Contact Workflow
- [ ] Create `Lead` model + migration.
- [ ] Build validated lead form endpoint.
- [ ] Add spam protection (honeypot + rate limiting + optional captcha).
- [ ] Create admin view/export for captured leads.
- [ ] Keep/replace existing Supabase integration by decision.

## Phase 4 — SaaS Core (MVP Product)
- [ ] Authentication:
  - [ ] User signup/login/logout
  - [ ] Password reset
- [ ] Multi-tenancy:
  - [ ] Workspace/account model
  - [ ] Membership roles
- [ ] Chatbot management:
  - [ ] CRUD chatbots
  - [ ] Bot settings (name, tone, model)
- [ ] Document ingestion:
  - [ ] Upload PDFs (MVP)
  - [ ] Parse/chunk pipeline
  - [ ] Embedding creation jobs
- [ ] Retrieval + chat pipeline:
  - [ ] Query embeddings
  - [ ] Build prompt context
  - [ ] Call LLM provider
  - [ ] Save conversation logs
- [ ] Embeddable widget:
  - [ ] Snippet generator
  - [ ] Public tokenized endpoint
  - [ ] Basic customization

## Phase 5 — Billing, Plans, Limits
- [ ] Stripe customer + subscription setup.
- [ ] Plan catalog (Starter/Growth/Pro/Enterprise).
- [ ] Usage tracking (messages/conversations/docs).
- [ ] Enforce quotas and upgrade prompts.
- [ ] Billing portal + invoices access.

## Phase 6 — Security & Compliance Foundations
- [ ] Role-based authorization enforcement.
- [ ] Audit logs for sensitive actions.
- [ ] Data encryption review (at rest/in transit).
- [ ] API key storage/rotation pattern.
- [ ] GDPR workflows (data export/delete).

## Phase 7 — Analytics & Operations
- [ ] Dashboard metrics (usage, response quality, volume).
- [ ] Performance and background job monitoring.
- [ ] SLA/uptime checks and alerting.
- [ ] Backup/restore verification.

## Phase 8 — Testing, Cutover & Launch
- [ ] Unit tests (models/services/jobs).
- [ ] Request/system tests for key flows.
- [ ] Load tests for chat endpoints.
- [ ] Migration rehearsal in staging.
- [ ] DNS/domain cutover plan.
- [ ] Rollback plan and post-launch checklist.

## 6) Suggested Repo Structure in Rails

```text
app/
  controllers/
  models/
  services/
  jobs/
  policies/
  views/
    home/
    shared/
    dashboard/
lib/
config/
db/
```

## 7) Immediate Next 10 Tasks (Start Here)

1. [ ] Create a new Rails app skeleton in a separate branch/folder.
2. [ ] Set up PostgreSQL + Tailwind + RSpec/Minitest baseline.
3. [ ] Port the current homepage sections into Rails partials.
4. [ ] Recreate navbar/footer and anchor-link behavior.
5. [ ] Port Book Demo Cal.com embed.
6. [ ] Implement lead capture model/form endpoint.
7. [ ] Add admin page to view leads.
8. [ ] Add SEO artifacts (sitemap/robots/meta tags).
9. [ ] Add CI pipeline and environment configs.
10. [ ] Deploy first Rails marketing version to staging and compare parity.
