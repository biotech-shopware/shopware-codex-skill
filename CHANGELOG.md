# Changelog

All notable changes to this repository are documented here.

## [Unreleased]

- No unreleased changes.

## [v0.4.1] - 2026-03-17

### Changed

- Sanitized public documentation and removed machine-specific absolute paths from the current tree.
- Compressed `README.md` into a public operator guide focused on structure, install, usage, and maintenance.
- Compressed `CHANGELOG.md` into concise public release notes.
- Rewrote public history and recreated release tags to remove leaked local paths from tagged file contents and annotated tag messages.

### Validation

- Verified the current tree for path leaks and machine-specific references.
- Revalidated markdown frontmatter and `shopware-development/agents/openai.yaml`.
- Rechecked repo and global installed skill alignment after cleanup.

## [v0.4.0] - 2026-03-17

### Added

- Added `shopware-development/references/18-cart-and-checkout-pipeline.md`.
- Added `shopware-development/references/19-testing-patterns.md`.
- Added `shopware-development/references/20-search-and-indexing.md`.
- Added `shopware-development/references/21-internationalization-and-snippets.md`.
- Added explicit routing rules in `shopware-development/SKILL.md` for cart, testing, search, and localization work.

### Changed

- Made the skill example-driven across the owning references.
- Added structured PR-description and findings templates.
- Expanded backend, storefront, admin, payment, review, tooling, and accessibility references with concise Shopware-shaped examples.

## [v0.3.0] - 2026-03-12

### Added

- Added `LICENSE`.
- Added `CHANGELOG.md`.
- Added `shopware-development/references/17-accessibility-and-template-best-practices.md`.

### Changed

- Strengthened the “guardrails, not a cap” rule in `shopware-development/SKILL.md`.
- Added stronger accessibility routing and storefront compliance guidance.

## [v0.2.0] - 2026-03-12

### Added

- Added `shopware-development/references/16-headless-and-composable-frontends.md`.

### Changed

- Expanded payment, backend, review, and operations guidance for browser trust boundaries and backend/frontend payment contracts.

## [v0.1.0] - 2026-03-12

### Added

- Added `shopware-development/references/15-subscriptions-and-recurring-payments.md`.

### Changed

- Expanded recurring/subscription audit coverage, saved-method ownership rules, and cross-plugin triangulation guidance.

## [v0.0.1] - 2026-03-12

### Added

- Initial public release of `shopware-development`.
- Introduced the clustered reference structure and Shopware 6.7-first / 6.6-compatible operating model.
