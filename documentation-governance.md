# Documentation Governance

## Purpose

This repository is a docs-first workspace that will be published as a GitHub Pages site with MkDocs Material. Future AI assistance should treat the documentation structure as the primary source of truth for content placement, navigation, and publishing behavior.

## Primary Rule

Use docs first, content second, and code only when it is needed for site plumbing or a requested implementation task.

Future AI systems working in this workspace should begin with the governance docs and the relevant collection landing page unless:

- the user explicitly asks for code review,
- the user asks for a line-level explanation,
- the docs are clearly stale or incomplete for the task,
- a site-publishing issue requires implementation evidence.

## Required Read Order For AI Sessions

1. Root `README.md`
2. This `documentation-governance.md`
3. The relevant collection landing page, such as `docs/collections/study-materials/index.md`
4. The target page or pages inside that collection
5. Implementation files only when the task requires site configuration, publishing, or layout changes

## Site Organization Rules

The site must be organized by collections rather than by ad hoc folders.

Required top-level collections:

- study materials,
- personal projects,
- reference,
- shared,
- archive.

Each collection must have:

- an `index.md` landing page,
- a stable navigation path,
- a clear ownership statement or description,
- a predictable place for future pages.

Study-material content may use sub-collections such as `aws-ai-practitioner`, but those sub-collections must still follow the same landing-page rule.

## Naming Rules

- Use lowercase, kebab-case folder and file names.
- Use numeric prefixes when ordering pages matters, such as `topic-01-...`.
- Keep landing pages named `index.md` inside each collection folder.
- Use descriptive titles in page headings and navigation labels, even when filenames are terse.

## GitHub Pages Rules

The repository should publish through MkDocs Material on GitHub Pages.

- The root `mkdocs.yml` is the site configuration source.
- The `docs/` folder is the published content tree.
- The root `README.md` remains the repo entry point and should link into the site structure.
- Any page move that affects navigation must update the nav config and the collection landing page in the same change.

## AI Authoring Rules

Future AI assistance should classify content before creating or moving pages.

Suggested classification order:

1. Study materials
2. Personal projects
3. Shared reference
4. General repository guidance
5. Archive

If a page could fit multiple places, choose the collection with the clearest long-term ownership and link to related pages rather than duplicating content.

## Update Rules

Any change that affects any of the following must update the docs in the same change:

- site navigation,
- collection ownership,
- publishing configuration,
- page structure or URLs,
- content classification,
- README entry points,
- authoring guidance for future AI.

## Changelog Rules

Every non-trivial site or content change should append an entry to the nearest relevant changelog.

Use a repo-local changelog when the change is limited to this documentation site.

Each changelog entry should include:

- date,
- short title,
- affected area,
- what changed,
- why it changed,
- docs updated.

## Decision Log Rules

Append to a decision log whenever a change introduces or revises:

- a long-lived content taxonomy,
- a navigation pattern,
- a publishing workflow,
- a compatibility rule for future docs,
- a tradeoff that future contributors will need to understand.

Decision-log entries should include:

- identifier,
- date,
- status,
- context,
- decision,
- consequences.

## Drift Handling

If published pages and governance disagree:

1. confirm the current published behavior,
2. update the stale docs in the same change,
3. add a changelog entry describing the correction,
4. add a decision-log entry if the mismatch revealed a missing architectural record.

## Recommended Minimal Maintenance Checklist

For any non-trivial site change:

1. Update the relevant collection landing page.
2. Update the root README if the public entry point changed.
3. Update `mkdocs.yml` if navigation changed.
4. Append a changelog entry.
5. Append a decision-log entry if the change reflects a durable design decision.

## What Not To Do

- Do not treat README files alone as sufficient system documentation.
- Do not create a second source of truth that conflicts with collection landing pages.
- Do not let file moves break the nav or published URLs without documenting the migration.
- Do not duplicate content across collections unless there is a deliberate cross-linking reason.

## Suggested Log Taxonomy

### Site logs

- `docs/change-log.md`
- `docs/decision-log.md`

### Collection logs

- collection-local changelog or decision-log files when a collection becomes large enough to justify them.

## Review Exception

If the user explicitly asks for review or implementation help, AI systems may go directly to the relevant pages or config, but should still consult the governance docs afterward to identify missing or stale documentation and call that out.