# Content Studio Confirmed Intent

- **Status:** Confirmed
- **Confirmed:** 2026-10-27
- **Repository:** `chkoster/content-studio`

## Outcome

Content Studio should help produce one or two high-quality, source-supported social posts per week with little rework.

## Quality bar

The system should prioritize:

- Strong, non-forced connections to the enterprise security architecture book.
- Supported claims with preserved source provenance.
- Writing that sounds like the author.
- A high-quality first serious draft that requires minimal revision.

## Editorial filtering

The system should suppress weak or forced opportunities by default.

Critical news—such as zero-day vulnerabilities, major government action, or similarly consequential events—should be surfaced as clearly labeled urgent topics even when no strong book connection exists. The system must never fabricate, overstate, or imply a book connection that is not supported.

## Voice hierarchy

1. The author-provided writer's guide in JSON is authoritative.
2. The book provides subject-matter depth.
3. Selected published posts provide social tone.
4. A supplemental voice guide may address gaps not covered by the JSON guide or source examples.

The writer's guide overrides older or conflicting published posts. Older posts are historical context and must not cause the system to imitate outdated habits or one-off phrasing.

## Human control and automation

The system should automate research and preparation. The author chooses the angle and approves the exact final content package.

Approval must remain separate from GitHub export authorization and public posting authorization. The MVP must not publish automatically.

## Constraints and non-goals

Content Studio should not:

- Optimize for content volume at the expense of quality.
- Fill a content queue with weak opportunities.
- Invent claims, data, source support, or book relevance.
- Imitate outdated writing habits.
- Publish automatically.
