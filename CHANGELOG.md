# Changelog

All notable changes to this repository are documented here.

## [Unreleased]

- No unreleased changes yet.

## [v0.4.0] - 2026-03-17

### Added

- Added [18-cart-and-checkout-pipeline.md](shopware-development/references/18-cart-and-checkout-pipeline.md) as the primary owner for cart collectors, processors, validators, line-item creation, delivery or promotion interactions, recalculation boundaries, and checkout hot-path rules.
- Added [19-testing-patterns.md](shopware-development/references/19-testing-patterns.md) as the primary owner for Shopware-specific test surfaces, including `IntegrationTestBehaviour`, `SalesChannelApiTestBehaviour`, fixtures, repository mocking, and Store API test guidance.
- Added [20-search-and-indexing.md](shopware-development/references/20-search-and-indexing.md) as the primary owner for Elasticsearch or OpenSearch extension points, mapping plus fetch symmetry, search-route usage, reindexing, and search fallback behavior.
- Added [21-internationalization-and-snippets.md](shopware-development/references/21-internationalization-and-snippets.md) as the primary owner for storefront and admin snippets, translation associations, language-aware DAL behavior, fallback rules, and 6.7 base-snippet conventions.
- Added an explicit routing table to [SKILL.md](shopware-development/SKILL.md) for payments, cart, storefront, admin, review, testing, search, and i18n work.

### Changed

- Made the skill example-driven across the owning references instead of keeping the pack prose-only. Implementation-bearing references now contain concrete bad or good patterns, while workflow and review references now ship literal output templates.
- Strengthened [01-source-hierarchy.md](shopware-development/references/01-source-hierarchy.md) with ownership examples so new learnings route cleanly into one primary reference instead of duplicating across backend, cart, search, testing, or i18n files.
- Strengthened [02-version-targeting.md](shopware-development/references/02-version-targeting.md) with a migration playbook and concrete scan or replacement examples for 6.6 versus 6.7 work.
- Strengthened [03-implementation-workflow.md](shopware-development/references/03-implementation-workflow.md) with literal PR-description and QA-guide templates.
- Strengthened [04-plugin-backend.md](shopware-development/references/04-plugin-backend.md) with an entity-extension decision matrix and concrete DAL batching, partial-loading, and custom-field merge examples.
- Strengthened [05-storefront-and-themes.md](shopware-development/references/05-storefront-and-themes.md), [06-administration-and-apps.md](shopware-development/references/06-administration-and-apps.md), [07-payments.md](shopware-development/references/07-payments.md), [09-symfony-and-php.md](shopware-development/references/09-symfony-and-php.md), [11-quality-and-operations.md](shopware-development/references/11-quality-and-operations.md), [12-extension-patterns.md](shopware-development/references/12-extension-patterns.md), [13-context-and-commerce.md](shopware-development/references/13-context-and-commerce.md), [14-cli-and-dev-tooling.md](shopware-development/references/14-cli-and-dev-tooling.md), [15-subscriptions-and-recurring-payments.md](shopware-development/references/15-subscriptions-and-recurring-payments.md), [16-headless-and-composable-frontends.md](shopware-development/references/16-headless-and-composable-frontends.md), and [17-accessibility-and-template-best-practices.md](shopware-development/references/17-accessibility-and-template-best-practices.md) with concise Shopware-shaped examples or stronger routing and review support.
- Strengthened [08-analysis-and-reviews.md](shopware-development/references/08-analysis-and-reviews.md) with literal findings templates while keeping findings-first review output intact.
- Strengthened [10-official-docs-map.md](shopware-development/references/10-official-docs-map.md) with testing links, translation links, core-source anchors, and stale-link maintenance guidance.
- Updated [README.md](README.md) and [openai.yaml](shopware-development/agents/openai.yaml) to reflect the broader example-driven scope and new reference files.

### Validation

- Validated markdown frontmatter across the skill pack.
- Validated [openai.yaml](shopware-development/agents/openai.yaml).
- Audited the main ownership boundaries between backend, cart, payments, review, accessibility, search, testing, and i18n references so the new examples expand the skill without creating contradictory owners.

## [v0.3.0] - 2026-03-12

### Added

- Added the MIT [LICENSE](LICENSE).
- Added this canonical `CHANGELOG.md` and established `Unreleased` as the default place for new skill-affecting changes.
- Added [17-accessibility-and-template-best-practices.md](shopware-development/references/17-accessibility-and-template-best-practices.md) as the primary owner for storefront accessibility standards, WCAG-oriented audits, theme and template accessibility pitfalls, and developer-ready accessibility review output.
- Added README prompt patterns for storefront accessibility reviews and full end-to-end reviews that explicitly keep the model unconstrained.

### Changed

- Strengthened [SKILL.md](shopware-development/SKILL.md) with a red-line policy that the skill is context and guardrails only, never a cap on Codex investigation depth, review breadth, or solution search.
- Expanded the skill scope, routing, and output rules for accessibility or storefront compliance work, including mandatory loading of `05`, `10`, and `17` when accessibility is primary scope.
- Updated [01-source-hierarchy.md](shopware-development/references/01-source-hierarchy.md) so accessibility questions use Shopware docs and matching core behavior first, then WCAG/WAI-ARIA/ADA/U.S. Access Board material, then the distilled skill references.
- Strengthened [05-storefront-and-themes.md](shopware-development/references/05-storefront-and-themes.md) with template-copy discipline, semantic-control rules, repeated-component ID safety, focus and status guidance, consent-aware widget handling, and stronger accessibility-oriented storefront JS guidance.
- Strengthened [08-analysis-and-reviews.md](shopware-development/references/08-analysis-and-reviews.md) with accessibility-first review behavior, WCAG mapping, runtime-gap versus code-defect separation, and vendor-limitation labeling.
- Strengthened [10-official-docs-map.md](shopware-development/references/10-official-docs-map.md) with Shopware storefront JS, consent, modal, and media docs plus the accessibility standards and pattern links used by `17`.
- Strengthened [11-quality-and-operations.md](shopware-development/references/11-quality-and-operations.md) with third-party storefront widget readiness guidance, including consent, keyboard/focus/status checks, and VPAT/vendor accessibility statement expectations when appropriate.
- Updated [README.md](README.md) so it summarizes releases, documents the new license and changelog policy, and states clearly that the skill is a base layer only.
- Updated [openai.yaml](shopware-development/agents/openai.yaml) to reflect accessibility/compliance scope and the "guardrails, not a cap" posture.

### Validation

- Validated markdown frontmatter across the skill pack.
- Validated [openai.yaml](shopware-development/agents/openai.yaml).
- Audited the ownership boundaries between `05`, `08`, `10`, `17`, and the main skill so the accessibility additions do not duplicate or contradict existing review, workflow, or source-order rules.

## [v0.2.0] - 2026-03-12

### Added

- Added [16-headless-and-composable-frontends.md](shopware-development/references/16-headless-and-composable-frontends.md) as the primary owner for browser trust boundaries, frontend/backend payment contracts, composable checkout flow review, callback-target hardening, and browser-side payment anti-patterns.
- Added a dedicated prompt pattern for paired backend and frontend payment reviews so headless checkout audits are triggered intentionally instead of being treated as generic storefront work.

### Changed

- Expanded [SKILL.md](shopware-development/SKILL.md) so the skill routes headless frontend integrations, browser-storage trust boundaries, and backend plus frontend payment audits explicitly while staying lean.
- Strengthened [04-plugin-backend.md](shopware-development/references/04-plugin-backend.md) with server-authoritative headless payment rules, including cart or order recalculation before payment-state writes and backend ownership resolution for saved-method references.
- Strengthened [07-payments.md](shopware-development/references/07-payments.md) with hosted-checkout callback-target hardening, explicit authority boundaries for browser-authored payment identifiers, and timeout-bound provider verification helpers.
- Strengthened [08-analysis-and-reviews.md](shopware-development/references/08-analysis-and-reviews.md) with separate frontend repo scope coverage, backend plus frontend payment triangulation, and browser-authored versus server-resolved authority mapping.
- Strengthened [11-quality-and-operations.md](shopware-development/references/11-quality-and-operations.md) with bans on browser-console logging of tokens or payment payloads and on treating browser storage as authoritative payment or vault state.
- Preserved the existing findings-first review posture, recurring-payment structure, and detailed PR-description default while extending headless coverage.

## [v0.1.0] - 2026-03-12

### Added

- Added explicit recurring/subscription and multi-plugin recurring audit support.
- Added [15-subscriptions-and-recurring-payments.md](shopware-development/references/15-subscriptions-and-recurring-payments.md) as the primary owner for subscription lifecycle safety, recurring-order correctness, saved-method ownership, provider-choice handling, snapshot exposure, and recurring-flow triangulation.

### Changed

- Expanded [SKILL.md](shopware-development/SKILL.md) to route subscription, recurring-commerce, and multi-repo review work without turning the trigger file into a monolith.
- Strengthened [07-payments.md](shopware-development/references/07-payments.md) for PSP-side recurring boundaries, recurring authority, provider recurring references, and support-safe recurring diagnostics.
- Strengthened [08-analysis-and-reviews.md](shopware-development/references/08-analysis-and-reviews.md) with multi-repo triangulation reviews, cross-flow contract-drift handling, ownership and IDOR prominence, and richer evidence requirements.
- Strengthened [11-quality-and-operations.md](shopware-development/references/11-quality-and-operations.md) with recurring-log redaction, snapshot minimization, renewal-batch and scheduler safety, translated-label hazards, and heavy recurring-batch hydration checks.
- Strengthened [13-context-and-commerce.md](shopware-development/references/13-context-and-commerce.md) with subscription/saved-method ownership scoping and recurring total/payment-context recalculation rules.
- Retained the default detailed PR and merge-request description structure with `QA notes` as a compact test guide.
- Consolidated local learnings from the installed skill into primary owners instead of scattering repeated rules across multiple references.

## [v0.0.1] - 2026-03-12

### Added

- Introduced the installable `shopware-development` skill structure.
- Split Shopware guidance across 14 focused references to keep the trigger file lean.
- Established Shopware 6.7-first behavior with explicit 6.6 fallback handling.
- Added implementation, storefront, admin, payment, review, PHP/Symfony, and operational hardening guidance.
