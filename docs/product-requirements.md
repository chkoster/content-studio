# Content Studio Product Requirements Document

- **Document version:** 0.2.0
- **Status:** Draft for review
- **Last updated:** 2026-10-27
- **Repository:** `chkoster/content-studio`
- **Product status:** New build; implementation status is unverified

## 1. Product definition

Content Studio is a private web application that turns enterprise security architecture book material, personal ideas, and meaningful news into high-quality social-content packages for LinkedIn, X, and future platforms.

The target outcome is one or two strong, source-supported posts per week that require little rework.

The product is a social-content production system, not a replacement for the book-writing application.

### Editorial principles

- Prefer substantive, non-forced connections to specific book concepts.
- Preserve source references supporting each claim.
- Suppress weak opportunities by default.
- Surface critical news as clearly labeled urgent topics even when no strong book connection exists.
- Never fabricate claims, data, source support, or book relevance.
- Use the same approved brief and claim set for text and graphics.
- Keep the author in control of angle selection, final approval, GitHub export, and publication.

### Requirement status labels

- **AGREED** — established product decision.
- **RECOMMENDED** — proposed default pending review.
- **OPEN** — material decision still required.
- **UNVERIFIED** — dependent on repository, service, or credential inspection.

## 2. Users, scope, and non-goals

### Primary user

**AGREED:** A single author/operator who selects angles, reviews drafts, approves content, authorizes exports, manually publishes, and records feedback.

The initial implementation may map all permissions to one authenticated user, while retaining a role-aware authorization model.

### MVP scope

- Ingest book material, ideas, RSS items, and Readwise items.
- Normalize and deduplicate source material.
- Identify relevant book passages and propose content angles.
- Surface exceptional urgent topics for critical news.
- Generate an editorial brief, platform-specific drafts, and supported graphics.
- Run factual-support, provenance, voice, confidentiality, platform, and visual checks.
- Maintain immutable revisions and revision-specific approvals.
- Export approved content to GitHub only after separate authorization.
- Record manual publication details and feedback.
- Automate intake and preparation around human approval gates.

### Explicit non-goals

- Automatic public posting.
- Replacing the manuscript repository or book-writing workflow.
- Unrestricted autonomous-agent swarms.
- Supabase Storage in the MVP.
- Factual correctness without human review.
- Optimizing for volume or filling a queue with weak opportunities.
- Multi-user collaboration beyond the initial private user model.
- Treating `agentic-writing-studio` or its D1/R2 architecture as the target system.

## 3. Functional requirements

### 3.1 Authentication and authorization

**FR-001 — Private access**  
The system shall require authentication before exposing source material, drafts, approvals, jobs, settings, or publication records.

**FR-002 — Role-aware authorization**  
The system shall model at least Viewer, Editor, Approver, Exporter, and Administrator capabilities, even if the MVP uses one user.

**FR-003 — Separate approval capabilities**  
The system shall distinguish final content approval from GitHub export authorization. Approval shall never authorize public posting.

### 3.2 Source ingestion

**FR-010 — Book ingestion**  
The system shall import book documents and preserve repository, revision, document, chapter, section, passage, path, and location references where available.

**FR-011 — RSS ingestion**  
The system shall ingest configured RSS feeds and preserve feed URL, canonical item URL, title, author where available, publication time, retrieval time, normalized content, and fingerprint.

**FR-012 — Readwise ingestion**  
The system shall ingest Readwise items or an import fixture and preserve provider identifier, source URL, highlights, notes, retrieval time, and fingerprint.

**FR-013 — Idea capture**  
The user shall be able to create an idea with title, body, tags, optional references, and creation/update metadata.

**FR-014 — Scheduled intake**  
The system shall support scheduled source-intake jobs for configured RSS and Readwise sources.

**FR-015 — Ingestion reporting**  
Each intake run shall report new items, duplicates, skipped items, failures, and retry status.

### 3.3 Normalization and discovery

**FR-020 — Deduplication**  
The system shall deduplicate using provider identifiers, canonical URLs, normalized title/source, content fingerprints, and configurable similarity detection where available.

**FR-021 — Duplicate provenance**  
Deduplicated records shall retain links to all observed source occurrences.

**FR-022 — Relevance evaluation**  
The system shall evaluate source items against book concepts and configured editorial criteria.

**FR-023 — Explainable relevance**  
Each relevance result shall include a score or category, human-readable reasons, relevant book passages, and source references.

**FR-024 — Strict default filtering**  
The system shall suppress or deprioritize items with weak or forced book connections by default.

**FR-025 — Critical-news exception**  
The system shall surface critical news—such as zero-day vulnerabilities or major government action—as clearly labeled urgent topics even without a strong book connection.

**FR-026 — No fabricated relevance**  
The system shall explicitly indicate when an urgent topic lacks a strong book connection and shall not invent or overstate one.

**FR-027 — Angle proposals**  
The system shall propose one or more distinctive angles from selected source material, with supporting source items, book passages, and expected claims.

**FR-028 — Human angle selection**  
The user shall select an angle before production proceeds.

### 3.4 Voice and content production

**FR-030 — Voice hierarchy**  
The system shall use the author-provided writer's-guide JSON as the authoritative voice source.

**FR-031 — Voice fallbacks**  
The system may use the book for subject-matter depth and selected published posts for social tone. A supplemental voice guide may address gaps.

**FR-032 — Voice conflict resolution**  
The writer's guide shall override conflicting or older published posts.

**FR-033 — Editorial brief**  
The system shall create a brief containing audience, purpose, selected angle, thesis, key claims, supporting sources, relevant book passages, platform notes, confidentiality notes, and graphic direction.

**FR-034 — Platform drafts**  
The system shall create separate LinkedIn and X drafts from the same brief and claim set.

**FR-035 — Claim preservation**  
Platform adaptation shall not silently introduce unsupported claims.

**FR-036 — Claim provenance**  
Every factual claim shall link to one or more source references, or be marked as opinion/editorial framing.

**FR-037 — Graphics**  
The system shall support diagrams, carousels, charts, and illustrations derived from the same brief and claim set as the text.

**FR-038 — Graphic claim integrity**  
Graphics shall use only supported claims or clearly labeled editorial decoration. Invented data is prohibited.

**FR-039 — Graphic accessibility**  
Each graphic shall include appropriate alternative text, labels, legible typography, and sufficient contrast.

### 3.5 Quality checks and review

**FR-040 — Independent checks**  
The system shall evaluate a revision without silently modifying it.

**FR-041 — Required check categories**  
Checks shall cover factual support, source validity, citation completeness, voice, confidentiality, platform constraints, repetitive angles, graphic-to-claim consistency, accessibility, and visual quality.

**FR-042 — Blocking findings**  
Each check shall identify whether a finding blocks review readiness or approval.

**FR-043 — Review workspace**  
The user shall be able to inspect the exact revision, source references, claims, graphics, check findings, and revision differences.

**FR-044 — Revision creation**  
Material changes to text, claims, references, graphics, or platform configuration shall create a new immutable revision.

**FR-045 — Approval invalidation**  
A new revision shall invalidate approvals for earlier revisions.

**FR-046 — Final approval**  
The user shall be able to approve only a specific revision after required blocking checks pass.

### 3.6 Workflow and operations

**FR-050 — Workflow states**  
The system shall support: `captured`, `normalized`, `candidate`, `angle_proposed`, `angle_selected`, `brief_drafted`, `drafted`, `checks_pending`, `ready_for_review`, `needs_revision`, `approved`, `export_authorized`, `exported`, `published`, `archived`, and `failed`.

**FR-051 — State integrity**  
The system shall reject invalid state transitions and record the actor, timestamp, and reason for manual transitions.

**FR-052 — Durable jobs**  
Jobs shall persist state, attempts, timestamps, input/output references, error category, and idempotency key.

**FR-053 — Retries**  
Transient failures shall retry with bounded exponential backoff. Permanent failures shall become reviewable failed jobs.

**FR-054 — Resumption**  
A failed job shall be resumable from the failed stage without repeating successful prior stages.

**FR-055 — Cost controls**  
Model stages shall support configurable limits for frequency, batch size, requests/tokens, and estimated cost.

**FR-056 — Automation boundary**  
Automation may perform intake, normalization, relevance filtering, and preparation. It shall not approve content, authorize export, or publish publicly.

### 3.7 GitHub export and publication

**FR-060 — Explicit export authorization**  
The user shall explicitly authorize export of an approved revision.

**FR-061 — Export traceability**  
An export shall record repository, branch, paths, content revision, authorization, and resulting commit.

**FR-062 — Manual publication**  
The MVP shall not publish automatically to social platforms.

**FR-063 — Publication record**  
The user shall be able to record platform, URL, publication timestamp, content revision, and feedback metrics or notes.

## 4. Non-functional requirements

### 4.1 Quality and editorial integrity

**NFR-001 — Low-rework quality**  
The system shall optimize for a high-quality first serious draft, prioritizing book relevance, claim support, and voice over volume or cosmetic polish.

**NFR-002 — Provenance completeness**  
The system shall preserve source-to-claim-to-draft-to-asset traceability through review and export.

**NFR-003 — Deterministic approval scope**  
An approval shall identify the exact content revision and associated checks.

**NFR-004 — Accessibility**  
Generated graphics and review interfaces shall provide required labels, alternative text, legibility, and usable contrast.

### 4.2 Security and confidentiality

**NFR-010 — Server-side secrets**  
Provider credentials and tokens shall remain server-side and shall not appear in browser bundles, committed files, generated content, or logs.

**NFR-011 — Least privilege**  
GitHub and source integrations shall use the minimum permissions required.

**NFR-012 — Auditability**  
The system shall record approvals, export authorizations, configuration changes, overrides, state transitions, and job outcomes.

**NFR-013 — Redaction**  
Logs shall redact credentials and minimize sensitive source content.

**NFR-014 — Confidentiality gate**  
Confidentiality findings shall be evaluated before review readiness.

### 4.3 Reliability and data integrity

**NFR-020 — Idempotency**  
Repeating an operation with the same source revision and operation key shall not duplicate source records, revisions, exports, or publication records.

**NFR-021 — Failure visibility**  
Failures shall expose stage, category, attempt history, retry state, and recommended recovery action.

**NFR-022 — Immutable revisions**  
Approved revisions and their approval records shall remain immutable.

**NFR-023 — Portable migrations**  
Database migrations shall use portable PostgreSQL SQL so future movement to local PostgreSQL remains feasible.

**NFR-024 — Durable audit trail**  
Operational events required to reconstruct workflow history shall be persisted in the database.

### 4.4 Cost and operational control

**NFR-030 — Bounded model usage**  
Model calls shall be bounded by configurable rate, batch, token/request, and cost limits.

**NFR-031 — No hidden mutation**  
Quality checks and previews shall not modify content without an explicit user action.

**NFR-032 — Safe defaults**  
The default system behavior shall favor suppression, review, and non-public actions when uncertain.

### 4.5 Development and portability

**NFR-040 — Local development**  
The project shall provide a practical local development server, browser URL, migration/seed commands, fixture mode, and visible workflow updates.

**NFR-041 — Secret separation**  
Browser and server/controller configuration shall be separate.

**NFR-042 — Portability**  
The system shall run locally on the author's Mac and retain a practical path to Mac Studio operation. Containerization is optional.

**NFR-043 — Stack validation**  
React/TypeScript with Vite and a Node/TypeScript controller remain recommended defaults until repository and environment validation confirms them.

## 5. Data ownership and boundaries

### Operational data in Supabase PostgreSQL

Sources, source items, provenance, workflow state, jobs, attempts, schedules, claims, revisions, checks, approvals, audit events, publication records, and references.

### Authoritative content in GitHub

The manuscript and selected approved drafts, posts, images, graphics, and other content assets.

### Temporary local files

Rendering intermediates only; disposable and not authoritative.

### Excluded from MVP

Supabase Storage and automatic social-posting integrations.

## 6. Core screens

- **Dashboard:** pipeline counts, pending reviews, failed jobs, recent packages, schedules.
- **Sources:** book passages, ideas, RSS, Readwise, provenance, retrieval and deduplication status.
- **Discovery queue:** candidates, relevance reasons, book connections, urgent-topic labels, angles.
- **Content package:** selected angle, brief, claims, citations, drafts, graphics, revisions.
- **Review workspace:** exact revision comparison, check results, accessibility, approval/export actions.
- **Jobs and schedules:** schedules, attempts, idempotency, retries, failures, cost estimates.
- **Publication log:** manual publication and feedback records.
- **Settings:** sources, GitHub destination, model settings, schedules, and authorization.

## 7. Acceptance criteria

The MVP shall demonstrate:

1. Fixture book, RSS, Readwise, and idea ingestion.
2. Duplicate detection without duplicate operational records.
3. Explainable relevance with linked book passages.
4. Suppression of weak connections by default.
5. Clearly labeled urgent-topic handling without fabricated relevance.
6. Angle selection before production.
7. Writer's-guide JSON taking precedence over conflicting older posts.
8. Brief plus LinkedIn/X drafts sharing one claim set.
9. Claim-level source references.
10. Graphics using supported claims and accessibility text.
11. Blocking unsupported claims or missing references.
12. Revision creation that invalidates earlier approval.
13. Separate approval and GitHub export authorization.
14. Export records containing repository, branch, paths, revision, and commit.
15. Safe retry of a failed job without duplication.
16. Manual publication recording.
17. No automatic social posting.
18. No secrets in browser bundles, Git, logs, or generated assets.
19. A complete fixture workflow demonstrated locally.
20. Live integrations separately labeled from fixture behavior.

## 8. Recommended implementation order

1. Inspect repository and validate local stack.
2. Confirm authentication and secret handling.
3. Define portable migrations, workflow state, revisions, and audit events.
4. Implement book, RSS, Readwise, and idea ingestion.
5. Implement normalization, deduplication, and provenance.
6. Implement relevance, urgent-topic override, and angle selection.
7. Implement writer's-guide configuration and content revisions.
8. Implement claims, checks, review, and approval invalidation.
9. Implement platform drafts and graphics.
10. Implement durable jobs, retries, schedules, and cost controls.
11. Implement explicit GitHub export.
12. Implement publication logging and feedback.
13. Add fixture demonstrations and integration tests.
14. Verify live integrations individually.
15. Conduct security and failure-recovery review.

## 9. Open decisions

### OPEN-001 — GitHub content destination

**Recommended:** Use `chkoster/content-studio` on `main` for product documentation and define a separate content-assets directory for exported material.

### OPEN-002 — Readwise integration

**Recommended:** Start with an import adapter or fixture, then add API synchronization after the core workflow is proven.

### OPEN-003 — Deployment target

**Recommended:** Local Mac development first, with a documented Mac Studio path. Defer hosted deployment until the local workflow is reliable.

### OPEN-004 — Model provider

**Recommended:** Define a provider interface and configure one provider initially. Credentials remain server-side.

### OPEN-005 — Graphics implementation

**Recommended:** Start with SVG/HTML-first diagrams and carousels before raster-heavy rendering.

### OPEN-006 — Authentication model

**Recommended:** Single-user private authentication initially, with role-aware authorization in the data model.

### OPEN-007 — Critical-news taxonomy

**OPEN:** Define the initial severity rules and sources that qualify for the urgent-topic override.

### OPEN-008 — Writer-guide schema

**OPEN:** Define the required JSON schema, validation rules, and update/versioning workflow for the writer's guide.

## 10. Change control

A proposed change shall identify affected requirement IDs, acceptance tests, tables or migrations, integrations, compatibility considerations, and whether separate merge or deployment authorization is required.

Existing behavior shall be preserved unless an approved change explicitly replaces it.
