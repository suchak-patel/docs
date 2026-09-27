# Repo Guidance

This repository is a documentation site first and should be treated as a GitHub Pages project.

## Read Order

1. `README.md`
2. `documentation-governance.md`
3. The relevant collection landing page under `docs/`
4. The target page or pages for the task

## Operating Rules

- Prefer collection-based organization over flat topic dumps.
- Keep content in `docs/` aligned with MkDocs Material navigation.
- Use lowercase, kebab-case names for folders and files.
- Keep landing pages as `index.md` files within each collection.
- Update governance and README references when content moves.
- Treat the archive as the destination for retired or superseded material.

## When Adding Content

- Classify the content before creating files.
- Choose the least ambiguous home collection.
- Add cross-links instead of duplicating pages when possible.
- Update the site nav and any landing pages in the same change.

## When Changing Structure

- Update the governance doc first if the change affects content rules or navigation rules.
- Keep the root README as a short map to the site.
- Preserve stable URLs when possible.