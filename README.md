# Shopware Codex Skill

Public source for the `shopware-development` Codex skill.

This skill is built for Shopware 6.7-first work with explicit 6.6 handling. It covers plugin development, storefront work, accessibility reviews, administration and apps, payments, subscriptions, headless frontend integrations, testing, search and indexing, localization, and code reviews.

## Design

- One installable skill: `shopware-development`
- Lean trigger file plus focused references
- Official Shopware docs first, matching core behavior second
- Guardrails only, never a cap on Codex investigation depth or solution search

## Repository Layout

```text
shopware-codex-skill/
├── CHANGELOG.md
├── LICENSE
├── README.md
└── shopware-development/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        ├── 01-source-hierarchy.md
        ├── 02-version-targeting.md
        ├── 03-implementation-workflow.md
        ├── 04-plugin-backend.md
        ├── 05-storefront-and-themes.md
        ├── 06-administration-and-apps.md
        ├── 07-payments.md
        ├── 08-analysis-and-reviews.md
        ├── 09-symfony-and-php.md
        ├── 10-official-docs-map.md
        ├── 11-quality-and-operations.md
        ├── 12-extension-patterns.md
        ├── 13-context-and-commerce.md
        ├── 14-cli-and-dev-tooling.md
        ├── 15-subscriptions-and-recurring-payments.md
        ├── 16-headless-and-composable-frontends.md
        ├── 17-accessibility-and-template-best-practices.md
        ├── 18-cart-and-checkout-pipeline.md
        ├── 19-testing-patterns.md
        ├── 20-search-and-indexing.md
        └── 21-internationalization-and-snippets.md
```

## Core Files

- [`shopware-development/SKILL.md`](shopware-development/SKILL.md)
  Trigger file, routing rules, source order, and the main operating model.
- [`shopware-development/agents/openai.yaml`](shopware-development/agents/openai.yaml)
  UI metadata for Codex.
- [`CHANGELOG.md`](CHANGELOG.md)
  Canonical release history.
- [`LICENSE`](LICENSE)
  MIT license.

## Reference Guide

- `01-source-hierarchy.md`
  Source authority, ownership boundaries, and where new learnings belong.
- `02-version-targeting.md`
  6.7 default behavior, 6.6 compatibility, and migration handling.
- `03-implementation-workflow.md`
  Small-step delivery, validation rules, PR templates, and QA template.
- `04-plugin-backend.md`
  Backend architecture, DAL, Store API, Admin API, caching, migrations, and async boundaries.
- `05-storefront-and-themes.md`
  Twig, storefront JS, SCSS, theme work, cache-aware storefront behavior, and implementation-level accessibility basics.
- `06-administration-and-apps.md`
  Administration modules, ACL, apps, cloud/app-first tradeoffs, and admin modernization surfaces.
- `07-payments.md`
  Payment handlers, redirects, finalize flow, webhooks, idempotency, vaulting, refunds, and payment trust boundaries.
- `08-analysis-and-reviews.md`
  Findings-first review mode, severity, evidence rules, and triangulation reviews.
- `09-symfony-and-php.md`
  Symfony and PHP rules that matter inside Shopware projects.
- `10-official-docs-map.md`
  Official Shopware and Symfony doc links plus core-source anchors.
- `11-quality-and-operations.md`
  Security baseline, external dependency resilience, observability, testing and CI, and common failure modes.
- `12-extension-patterns.md`
  Config, Flow Builder, CMS, import/export, media, mail/notification patterns, product/catalog extension choices, and Rule Builder.
- `13-context-and-commerce.md`
  `Context` vs `SalesChannelContext`, pricing, tax, multichannel behavior, and B2B-sensitive logic.
- `14-cli-and-dev-tooling.md`
  Console commands, static analysis, CI, and local tooling.
- `15-subscriptions-and-recurring-payments.md`
  Subscription lifecycle, renewals, saved-method ownership, and recurring-flow audits.
- `16-headless-and-composable-frontends.md`
  Browser trust boundaries, separate frontend packages, and backend/frontend payment contracts.
- `17-accessibility-and-template-best-practices.md`
  WCAG-oriented storefront audits, theme/accessibility pitfalls, and accessibility review heuristics.
- `18-cart-and-checkout-pipeline.md`
  Cart collectors, processors, validators, line-item modeling, delivery/promotion interactions, and checkout hot-path rules.
- `19-testing-patterns.md`
  Integration tests, Store API tests, fixtures, repository mocks, and Shopware test traits.
- `20-search-and-indexing.md`
  Search routes, Elasticsearch/OpenSearch extension points, mapping/fetch symmetry, reindexing, and fallback behavior.
- `21-internationalization-and-snippets.md`
  Storefront/admin snippets, translation associations, fallback rules, and 6.7 base-snippet conventions.

## Install

Install locally by copying `shopware-development/` into your Codex skills directory.

```bash
mkdir -p ~/.codex/skills
cp -R shopware-development ~/.codex/skills/
```

If your Codex setup uses `$CODEX_HOME`, install into `$CODEX_HOME/skills/` instead.

Then restart Codex so the skill is reloaded.

## Use in Codex

Invoke it explicitly for reliable activation.

```text
$shopware-development review this Shopware plugin for P0/P1 security, performance, DAL, and compatibility issues
```

```text
$shopware-development implement this feature in small safe chunks and call out version-specific behavior explicitly
```

```text
$shopware-development review this payment flow with focus on validate/finalize, webhook authenticity, idempotency, and ownership
```

```text
$shopware-development review this storefront for WCAG, keyboard, focus, modal, and repeated-component issues
```

```text
$shopware-development analyze this cart or checkout change with focus on collectors, processors, pricing, delivery, and hot-path safety
```

## How the Skill Works

- `SKILL.md` stays lean and routes Codex to the right references.
- References are loaded only as needed.
- The skill is context and guardrails only, not a limit on review breadth or implementation depth.
- The examples inside the references are anchors, not mandatory copy-paste solutions.

## Release Model

- Current release history lives in [`CHANGELOG.md`](CHANGELOG.md).
- The installable root stays `shopware-development/`.
- The trigger name stays `shopware-development`.

## Maintenance Basics

- Keep `SKILL.md` small and routing-focused.
- Put detailed guidance in the owning reference file.
- Keep examples inline with the owning rule instead of creating parallel example files.
- Update `CHANGELOG.md` for every public skill release.
- Do not add machine-specific paths, local workspace links, or personal filesystem references to public files or tag messages.
