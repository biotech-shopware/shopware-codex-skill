# Changelog

All notable changes to this repository are documented here.

## [Unreleased]

- No unreleased changes.

## [v0.5.0] - 2026-03-18

### Changed

- Expanded `shopware-development/SKILL.md` routing to cover HTTP cache, Varnish, reverse proxy, Messenger, RabbitMQ, worker, async, and indexer tasks explicitly.
- Added concrete 6.7 cache and reverse-proxy guidance to `shopware-development/references/04-plugin-backend.md`, `05-storefront-and-themes.md`, and `11-quality-and-operations.md`, including `_httpCache`, `sw-cache-hash`, `sw-invalidation-states`, and cache-safe personalization patterns.
- Added concrete Messenger and RabbitMQ guidance to `shopware-development/references/04-plugin-backend.md`, `11-quality-and-operations.md`, and `14-cli-and-dev-tooling.md`, including CLI-first workers, failure transport, delayed dispatch, and transport separation.
- Expanded `shopware-development/references/20-search-and-indexing.md` with the `EntityIndexer` lifecycle, `IteratorFactory`, `EntityIndexingMessage`, and bounded reindex guidance.
- Expanded `shopware-development/references/18-cart-and-checkout-pipeline.md` with checkout hot-path async boundaries and post-order queue handoff patterns.
- Expanded `shopware-development/references/05-storefront-and-themes.md` and `17-accessibility-and-template-best-practices.md` with more Shopware-shaped examples for cache-safe Twig, consent-aware widgets, modal handling, live status, and repeated-component accessibility.
- Expanded `shopware-development/references/10-official-docs-map.md` with exact Shopware cache, storefront, message queue, and indexer docs plus additional core-source anchors.
- Updated `README.md` and `shopware-development/agents/openai.yaml` to reflect the new cache, queue, indexing, and storefront hardening coverage.

### Validation

- Revalidated markdown frontmatter and `shopware-development/agents/openai.yaml`.
- Rechecked the main ownership boundaries for cache, queue, cart, indexing, and accessibility guidance.
- Rechecked repo and global installed skill alignment after sync.

## [v0.4.2] - 2026-03-18

### Changed

- Tightened `shopware-development/SKILL.md` into routing, source discipline, execution rules, and output expectations only.
- Removed provenance, repeated file-intro text, disclaimer text, and generic cross-reference tails from the skill references.
- Centralized duplicated implementation-doc links into `shopware-development/references/10-official-docs-map.md`.
- Trimmed low-value wording without changing the trigger name, file layout, topic ownership, or example coverage.

### Validation

- Grepped the skill pack for removed boilerplate patterns such as `Use this file`, `Official docs:`, and `## Cross-References`.
- Revalidated markdown frontmatter and `shopware-development/agents/openai.yaml`.
- Rechecked the cleaned repo copy against the installed global skill after sync.

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
