# Source Hierarchy

## Purpose

This skill merges five source clusters:

- `general-plugin-guide_v1.1.0.md`
  Backend architecture, DAL, caching, Store API, storefront, administration, testing, and store-readiness guidance.
- `payment-plugin-guide_v1.1.0.md`
  Payment-specific correctness, security, webhook, refund, and observability guidance.
- `shopware-code-review-prompt_v1.1.0.md`
  Audit workflow, severity model, big-instance reasoning, and review output rules.
- `skills-main.zip` `skills/shopware6`
  Broad taxonomy of Shopware topics and prior rule coverage.
- `shopware-agents-pack.zip`
  Path-scoped instruction layering for root, backend, storefront, views, and administration work.

## Authority Order

Use this order whenever guidance differs:

1. Official Shopware docs for the exact target version.
2. Stable Shopware core behavior matching the project version.
3. Official Symfony docs.
4. This skill's distilled references.

Everything else is context until verified.

For storefront accessibility questions, use this variant:

1. Official Shopware docs for the exact target version.
2. Stable Shopware core storefront behavior matching the project version.
3. WCAG 2.2, WAI-ARIA APG, ADA.gov, and U.S. Access Board material when the question is accessibility-specific.
4. Official Symfony docs when framework behavior matters to the implementation.
5. This skill's distilled references.

## Clustering Rules

Keep each concern in one place:

- Workflow and loading rules belong in `SKILL.md` and `03-implementation-workflow.md`.
- Version splits belong in `02-version-targeting.md`.
- General plugin and project backend rules belong in `04-plugin-backend.md`.
- Compact storefront, Twig, JS, SCSS, theme, SEO, and implementation-level accessibility rules belong in `05-storefront-and-themes.md`.
- Deep accessibility standards, WCAG-oriented audit heuristics, and storefront compliance review patterns belong in `17-accessibility-and-template-best-practices.md`.
- Administration modules, app extensions, and admin migration guidance belong in `06-administration-and-apps.md`.
- Payment-only rules belong in `07-payments.md`.
- Review/audit rules belong in `08-analysis-and-reviews.md`.
- Symfony/PHP baseline rules belong in `09-symfony-and-php.md`.
- Official links belong in `10-official-docs-map.md`.
- Security baseline, observability, testing, store-readiness, and common plugin failure patterns belong in `11-quality-and-operations.md`.
- Configuration, Flow Builder triggers, CMS patterns, import/export, media, and Rule Builder belong in `12-extension-patterns.md`.
- Context handling, multichannel behavior, pricing, and sales-channel-aware logic belong in `13-context-and-commerce.md`.
- Console commands and development tooling belong in `14-cli-and-dev-tooling.md`.
- Subscription lifecycle and recurring-commerce orchestration belong in `15-subscriptions-and-recurring-payments.md`.
- Headless or composable frontend payment and trust-boundary rules belong in `16-headless-and-composable-frontends.md`.
- Deep storefront accessibility heuristics and review patterns belong in `17-accessibility-and-template-best-practices.md`.
- Cart, checkout, delivery, line item, and promotion pipeline rules belong in `18-cart-and-checkout-pipeline.md`.
- Shopware-specific testing patterns belong in `19-testing-patterns.md`.
- Search, Elasticsearch, and OpenSearch rules belong in `20-search-and-indexing.md`.
- Snippet, translation, and localization rules belong in `21-internationalization-and-snippets.md`.

Do not repeat the same rule in multiple files unless the split would otherwise hide a critical safety boundary. Use short cross-references instead.

## Ownership Examples

Use these examples to decide where a new learning belongs:

| New learning | Primary owner | Keep elsewhere |
| --- | --- | --- |
| `searchIds()` versus `search()` in DAL loops | `04-plugin-backend.md` | short cross-reference from `18` when the loop is inside cart logic |
| collector versus processor responsibilities | `18-cart-and-checkout-pipeline.md` | short cross-reference from `04` or `07` |
| Store API functional test with `SalesChannelApiTestBehaviour` | `19-testing-patterns.md` | short cross-reference from `11` |
| extending Elasticsearch product mapping and fetch logic together | `20-search-and-indexing.md` | short cross-reference from `04` |
| `messages.en.base.json` and admin/storefront snippet ownership | `21-internationalization-and-snippets.md` | short cross-reference from `05` or `06` |
| detailed PR template or findings template | `03-implementation-workflow.md` or `08-analysis-and-reviews.md` | no duplicate second owner |

## Attached Source Coverage

This skill intentionally preserves the strongest points from the attached material:

- Hot-path protection, cache discipline, async-first integration, and upgrade safety from the general plugin guide.
- DAL sharp edges from the general plugin guide, including N+1 avoidance, `searchIds()`, pagination, and big-instance query discipline.
- Payment trust boundaries, tokenization rules, server-side verification, webhook idempotency, and refund handling from the payment guide.
- Findings-first review output, large-instance stress reasoning, and version-pinned source hierarchy from the audit prompt.
- Wide topic coverage from the earlier `shopware6` skill, but split into smaller files so only relevant context is loaded.
- Coverage of the earlier `shopware6` skill categories that were not in the plugin and payment bibles, including config, app surfaces, CLI, multichannel context, CMS, import/export, media, and Rule Builder.
- Directory-scoped guardrails from the AGENTS pack, but treated as optional repo-level operational guidance rather than core skill content.

## Repo-Local AGENTS Guidance

Only apply this when the user explicitly wants persistent repo-local guardrails or the repo already uses `AGENTS.md` files.

Recommended split:

- repo root `AGENTS.md`
  High-signal priorities, version targeting, output style.
- `src/AGENTS.md`
  Backend, PHP, DAL, migrations, APIs, queues, payments.
- `src/Resources/views/AGENTS.md`
  Twig, email templates, SEO, accessibility, escaping.
- `src/Resources/app/storefront/AGENTS.md`
  Storefront JS and SCSS rules.
- `src/Resources/app/administration/AGENTS.md`
  Admin modules, ACL, state management, no secrets in client code.

Keep each file short and path-specific. The goal is signal, not a second copy of the whole skill.
