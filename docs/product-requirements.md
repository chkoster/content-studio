# Content Studio Product Requirements Document

- **Document version:** 0.1.0
- **Status:** Draft for review
- **Last updated:** 2026-10-27
- **Repository:** `chkoster/content-studio`
- **Product status:** New build; implementation status beyond this document is unverified

## 1. Purpose and product principles

Content Studio is a private web application that turns enterprise security architecture book material, personal ideas, and meaningful news into high-quality social-content packages for LinkedIn, X, and future platforms.

The product is a social-content production system, not a replacement for the book-writing application.

The system must:

- Prefer substantive connections to specific book concepts over generic commentary.
- Preserve source references supporting each claim.
- Keep a human in control of angle selection, final approval, GitHub export, and publication.
- Automate intake and preparation without granting permission for automatic public posting.
- Use the same approved brief and claim set for text and graphics.
- Treat provenance, confidentiality, and revision history as first-class product data.

## 2. Scope and status vocabulary

Requirement labels:

- **AGREED** — established product decision.
- **RECOMMENDED** — proposed default pending review.
- **OPEN** — material decision still required.
- **UNVERIFIED** — dependent on repository, service, or credential inspection.

This document is the source of intended behavior. Future changes follow:

1. Update this specification and affected acceptance tests.
2. Identify affected code, migrations, and integrations.
3. Implement and test.
4. Review the change.
5. Separately authorize merge and deployment where applicable.

A specification edit is not deployment approval.

## 3. Goals

### G-001 — Content production

**AGREED:** Help one author produce distinctive, source-supported social content efficiently.

### G-002 — Source-grounded reasoning

**AGREED:** Connect news and ideas to relevant passages or concepts from the enterprise security architecture book.

### G-003 — Human approval

**AGREED:** Require explicit human review and approval of the exact final content package.

### G-004 — Retained automation

**AGREED:** Automate scheduled source intake, filtering, and preparation around human approval gates.

### G-005 — Durable provenance

**AGREED:** Preserve source-to-claim-to-draft-to-asset traceability.

### G-006 — GitHub authority

**AGREED:** GitHub is authoritative for the manuscript and selected drafts, posts, images, and other content assets.

## 4. Non-goals

The MVP will not:

- Publish automatically to social platforms.
- Replace the manuscript repository or book-writing workflow.
- Use an unrestricted autonomous-agent swarm.
- Provision Supabase Storage.
- Promise factual correctness without human review.
- Optimize heavily for analytics before useful publication feedback exists.
- Require multi-user collaboration beyond private authentication and authorization.
- Treat the existing `agentic-writing-studio` D1/R2 architecture as the target architecture.

## 5. Users and permissions

### Primary user

**AGREED:** A single author/operator who selects angles, reviews drafts, approves content, authorizes exports, manually publishes, and records feedback.

### Recommended permission roles

- **Viewer:** Read sources, packages, checks, and publication history.
- **Editor:** Create and revise ideas, angles, briefs, drafts, and graphics.
- **Approver:** Approve a specific revision after checks pass.
- **Exporter:** Separately authorize export of an approved revision to GitHub.
- **Administrator:** Manage sources, schedules, model settings, and integration configuration.

The initial implementation may map all roles to one authenticated user, but the authorization model should not make future separation difficult.

## 6. MVP workflow

### 6.1 Workflow states

A content package progresses through:

`captured` → `normalized` → `candidate` → `angle_proposed` → `angle_selected` → `brief_drafted` → `drafted` → `checks_pending` → `ready_for_review` → `approved` → `export_authorized` → `exported` → `published`

Additional terminal or recovery states:

- `needs_revision`
- `archived`
- `failed`

### 6.2 Required transitions

- A source item becomes `candidate` only after normalization and deduplication.
- An angle must identify its supporting source items and relevant book passages.
- A user must select an angle before production.
- Draft generation creates a new immutable content revision.
- Quality checks evaluate a revision without silently modifying it.
- A failed or blocking check prevents `ready_for_review`.
- Human approval applies only to the exact revision reviewed.
- Any material change to text, claims, references, graphics, or platform configuration creates a new revision and invalidates prior approval.
- GitHub export requires a separate explicit authorization after approval.
- Approval never authorizes public posting.
- Publication is recorded manually in the MVP.

### 6.3 Human approval gates

Required gates:

1. **Angle selection**
2. **Editorial review**
3. **Final package approval**
4. **Separate GitHub export authorization**

The system must display the revision identifier at each approval action.

## 7. Product screens

### SCR-001 — Dashboard

Show pipeline counts, pending reviews, failed jobs, recent packages, and scheduled intake status.

### SCR-002 — Sources

Show book documents and passages, ideas, RSS items, Readwise items, provenance, retrieval metadata, and deduplication status.

### SCR-003 — Discovery queue

Show candidate items, relevance explanations, book connections, proposed angles, source references, and selection actions.

### SCR-004 — Content package

Show selected angle, editorial brief, claims, citations, platform drafts, graphics, and revision history.

### SCR-005 — Review workspace

Show exact revision comparison, check results, source references, confidentiality findings, accessibility text, and approval/export actions.

### SCR-006 — Jobs and schedules

Show schedules, job attempts, idempotency keys, retries, failures, cost estimates, and recovery actions.

### SCR-007 — Publication log

Allow manual recording of platform, publication URL, publication timestamp, content revision, and feedback metrics or notes.

### SCR-008 — Settings

Manage source configuration, GitHub destination, model configuration, schedules, and authorization settings without exposing secrets to the browser.

## 8. End-to-end journeys

### J-001 — Scheduled source intake

1. A configured schedule starts an intake job.
2. The job retrieves RSS and Readwise items.
3. Items are normalized and fingerprinted.
4. Duplicates are linked to their existing source record.
5. New items are persisted with retrieval metadata.
6. The job reports its result and any failures.

### J-002 — Book ingestion

1. The system imports book documents from the configured book repository or fixture.
2. It preserves document, chapter, section, passage, and repository revision references.
3. It stores passages suitable for search and source citation.
4. Re-importing the same revision is idempotent.

### J-003 — Discovery and angle selection

1. The controller evaluates source items against book concepts and configured relevance criteria.
2. It proposes one or more angles with explanations.
3. Each angle lists source items, book passages, and expected claims.
4. The user selects one angle.
5. The selected angle becomes the input to production.

### J-004 — Production and review

1. The controller creates an editorial brief.
2. It creates platform-specific drafts for LinkedIn and X.
3. It creates a graphic concept or asset using the same brief and claims.
4. Independent checks evaluate support, provenance, voice, confidentiality, platform fit, and visual accessibility.
5. The user revises or approves the exact package revision.

### J-005 — Export and publication

1. The user approves a revision.
2. The user separately authorizes GitHub export.
3. The system records repository, branch, paths, revision, and resulting commit.
4. The user manually publishes.
5. The user records publication details and feedback.

## 9. Sources, ingestion, and provenance

### SRC-001 — Book sources

The book repository is the authoritative manuscript source. Passage references must preserve repository, revision, path or document identifier, and location where available.

### SRC-002 — RSS sources

Each RSS item must preserve feed URL, canonical item URL, title, author where available, publication time, retrieval time, normalized content, and content fingerprint.

### SRC-003 — Readwise sources

Each Readwise item must preserve provider identifier, source URL, highlights, notes, retrieval time, and content fingerprint.

### SRC-004 — Ideas

The user must be able to create an idea with title, body, tags, optional references, and creation/update metadata.

### SRC-005 — Deduplication

Deduplication must use, where available:

1. Provider identifier.
2. Canonical URL.
3. Normalized title and source.
4. Content fingerprint.
5. Configurable similarity detection.

Deduplicated records must retain links to all observed source occurrences.

### SRC-006 — Explainable relevance

Relevance results must include a score or category plus human-readable reasons, relevant book passages, and source references.

### SRC-007 — Traceability

Every claim in a brief or draft must link to one or more source references or be marked as an unsupported opinion or editorial framing. Unsupported factual claims must block approval unless explicitly overridden and recorded.

## 10. Content and graphic production

### CNT-001 — Editorial brief

A brief must contain audience, purpose, selected angle, thesis, key claims, supporting sources, relevant book passages, platform notes, confidentiality notes, and graphic direction.

### CNT-002 — Platform drafts

The system must create separate platform drafts from the same brief and approved claim set. Platform adaptation must not silently introduce unsupported claims.

### CNT-003 — Revision history

Drafts, briefs, claims, references, and graphics must be versioned. Revisions must be immutable after approval.

### GFX-001 — Graphic capability

The MVP must support a graphic-designer capability for diagrams, carousels, charts, and illustrations.

### GFX-002 — Claim support

Graphics must use only claims in the package claim set or clearly labeled editorial decoration. Invented data is prohibited.

### GFX-003 — Accessibility

Each graphic must include appropriate alternative text, labels, legible typography, and sufficient contrast for its intended use.

### GFX-004 — Shared brief

Text and graphics must reference the same approved brief and claim set.

## 11. Quality checks

Checks are independent evaluations and must not silently rewrite content.

Required check categories:

- Factual support coverage.
- Source URL and passage validity.
- Citation completeness.
- Voice and audience fit.
- Confidentiality and sensitive-information detection.
- Platform constraints.
- Duplicate or repetitive angle detection.
- Graphic-to-claim consistency.
- Accessibility text and label completeness.
- Visual legibility and quality.

Each check must record status, revision, checker type, timestamp, findings, and whether the finding blocks approval.

## 12. Data model

Recommended operational tables:

- `users`
- `sources`
- `source_items`
- `source_references`
- `book_documents`
- `book_passages`
- `ideas`
- `angles`
- `content_packages`
- `content_revisions`
- `claims`
- `claim_support`
- `platform_drafts`
- `graphics`
- `quality_checks`
- `approvals`
- `jobs`
- `job_attempts`
- `schedules`
- `github_exports`
- `publication_records`
- `feedback_events`
- `audit_events`

### Ownership boundaries

- **Supabase PostgreSQL:** Operational state, sources, workflow, jobs, approvals, provenance, revisions, and references.
- **GitHub:** Authoritative approved manuscript-related content and selected assets.
- **Local temporary files:** Rendering intermediates only; disposable.
- **Supabase Storage:** Excluded from the MVP.

Database migrations should use portable PostgreSQL SQL so future migration to local PostgreSQL remains feasible.

## 13. Jobs, scheduling, retries, and cost controls

### JOB-001 — Durable jobs

Jobs must persist state, attempts, timestamps, input references, output references, error category, and idempotency key.

### JOB-002 — Idempotency

Repeating a job with the same source revision and operation key must not create duplicate source records, revisions, exports, or publication records.

### JOB-003 — Retries

Transient failures may retry with bounded exponential backoff. Permanent failures must move to a reviewable failed state.

### JOB-004 — Recovery

The system must preserve enough context to resume from the failed stage without repeating successful prior stages.

### JOB-005 — Cost controls

Model stages must support configurable limits for frequency, batch size, token or request budgets, and estimated cost. Cost-related failures must be visible before or during execution.

### SCH-001 — Automation boundaries

Automation may perform scheduled intake, normalization, relevance filtering, and preparation. It must not approve content, authorize GitHub export, or publish publicly.

## 14. Interfaces and architecture

### Recommended baseline

**RECOMMENDED:** React/TypeScript with Vite for the browser, a Node/TypeScript controller, and Supabase PostgreSQL.

This recommendation is subject to repository inspection and stack validation. The browser must never receive provider secrets.

### Controller stages

- **Ordinary code:** ingestion, normalization, deduplication, persistence, state transitions, scheduling, retries, audit logging, exports.
- **Model calls:** relevance assessment, angle generation, brief drafting, platform adaptation, claim extraction, voice review, graphic concepts.
- **Human actions:** angle selection, revision, approval, export authorization, publication, feedback.

### Local development

The project should provide:

- a local development server and browser URL;
- database migration and seed commands;
- fixture mode;
- visible job and workflow updates;
- separate server/controller and browser configuration;
- clear setup documentation.

Containerization may support later Mac Studio portability but is not required before the local workflow is functional.

## 15. Security and confidentiality

- Use private authentication.
- Keep all secrets server-side.
- Never commit secrets or include them in browser bundles or logs.
- Use least-privilege GitHub access.
- Record audit events for approvals, exports, configuration changes, and overrides.
- Redact credentials and sensitive source data from logs.
- Run confidentiality checks before review readiness.
- Separate approval from export authorization.
- Do not add public-posting credentials or APIs to the MVP.

## 16. Acceptance criteria

The MVP is acceptable when all of the following are demonstrated:

1. A fixture book, RSS feed, Readwise export, and user idea can be ingested.
2. Duplicate source items are detected without duplicate operational records.
3. A candidate displays explainable relevance and linked book passages.
4. The user can select an angle.
5. The system creates a brief and LinkedIn/X drafts from that angle.
6. The drafts retain claim-level source references.
7. A graphic can be created from supported claims with accessibility text.
8. Unsupported claims and missing references are visible and block approval.
9. Editing creates a new revision and invalidates earlier approval.
10. GitHub export requires separate explicit authorization.
11. Export records repository, branch, paths, revision, and commit.
12. A failed job retries safely without duplication.
13. Publication can be recorded manually.
14. No automatic social posting occurs.
15. Secrets are absent from browser bundles, Git, logs, and generated assets.
16. The complete fixture workflow can be demonstrated locally.
17. Live integrations are labeled separately from fixture-based behavior.

## 17. Dependency-aware implementation backlog

1. Inspect repository and establish baseline.
2. Confirm authentication and secret-handling approach.
3. Validate the local stack and development workflow.
4. Define portable migrations and audit events.
5. Build book, RSS, Readwise, and idea ingestion.
6. Build normalization, fingerprinting, and deduplication.
7. Build passage indexing and provenance.
8. Build relevance filtering and angle selection.
9. Build package revisions, claims, and approvals.
10. Build quality-check framework.
11. Build platform draft generation.
12. Build graphic production and accessibility validation.
13. Build durable jobs, retries, schedules, and cost controls.
14. Build GitHub export with explicit authorization.
15. Build publication log and feedback capture.
16. Add fixture demonstration and integration tests.
17. Verify live integrations individually.
18. Conduct security and failure-recovery review.

## 18. Open decisions

### OPEN-001 — GitHub destination

**Recommended:** Use `chkoster/content-studio` on `main` for the product and documentation, with a clearly defined content-assets directory for exported material.

### OPEN-002 — Readwise integration

**Recommended:** Start with an import adapter or export fixture, then add API synchronization after the core workflow is proven.

### OPEN-003 — Deployment target

**Recommended:** Local Mac development first, with a documented path to Mac Studio. Defer hosted deployment until the local workflow is reliable.

### OPEN-004 — Model provider

**Recommended:** Define a provider interface and configure one provider initially. Provider credentials remain server-side.

### OPEN-005 — Graphics implementation

**Recommended:** Begin with SVG/HTML-first diagrams and carousels before adding raster-heavy rendering workflows.

### OPEN-006 — Authentication model

**Recommended:** Single-user private authentication initially, with role-aware authorization in the data model.

## 19. Change control

A proposed change must identify:

- affected requirement identifiers;
- affected acceptance tests;
- affected tables or migrations;
- affected integrations;
- compatibility and data-preservation considerations;
- whether separate merge or deployment authorization is required.

Existing behavior must be preserved unless the approved change explicitly replaces it.
