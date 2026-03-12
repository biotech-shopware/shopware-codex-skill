---
name: shopware-development
description: Plan, implement, refactor, or review Shopware 6 changes with Shopware 6.7 as the default target and explicit Shopware 6.6 compatibility handling. Use for plugin development, theme and template work, storefront Twig/JS/SCSS changes, administration modules, app extensions, payment integrations, subscriptions and recurring-commerce flows, saved-payment-method ownership issues, CMS elements, import/export work, media handling, Rule Builder conditions, CLI and tooling tasks, multichannel context issues, project implementation planning, migrations, performance/security hardening, and Shopware code reviews or multi-plugin architecture analysis.
---

# Shopware Development

## Overview

Use this skill to keep Shopware work version-aware, upgrade-safe, and incremental. It distills the attached Shopware guides and agent pack into a small workflow layer plus clustered references, with official Shopware docs as the authority when guidance conflicts. Use it for recurring commerce, saved-payment-method flows, and multi-plugin audits where subscription and PSP plugins share one customer journey.

## Source Discipline

Use sources in this order:
1. Shopware official docs for the exact target version.
2. Stable Shopware core behavior matching `composer.lock`.
3. Symfony official docs.
4. The clustered guidance in this skill.

Treat attached source material, ADRs, issues, forums, and blog posts as context only unless they match the official docs or stable core behavior.

## Workflow

1. Detect the target Shopware version from root `composer.json`, plugin/theme `composer.json`, and `composer.lock`.
2. Classify the task before reading references:
   - implementation or refactor
   - storefront or theme work
   - administration or app extension work
   - payment work
   - subscription or recurring-commerce flow
   - integration or multi-repo review
   - review or architecture analysis
3. Load only the relevant reference files for that task. Do not bulk-load the whole `references/` directory.
4. Inventory mutation surfaces, trust boundaries, ownership boundaries, and cross-plugin contracts before proposing fixes.
5. Plan the smallest safe change set. One concern per chunk: versioning, data model, API, UI, payment flow, recurring flow, or review findings.
6. Preserve extension points and existing project patterns unless the task explicitly includes a migration.
7. For multi-repo reviews, do not finalize findings until a triangulation pass checks how identifiers, saved methods, custom fields, webhook state, and recurring references move across repos.
8. Validate after each chunk with the narrowest useful checks.
9. Report version assumptions, what changed, what was verified, and any remaining regression risk.

## Execution Rules

- Default to Shopware 6.7 guidance. Load 6.6 compatibility notes only when the project or request requires them.
- Keep changes small and reversible. Avoid multi-concern diffs unless the code is tightly coupled.
- Protect hot paths first: cart, checkout, login, account, renewal, and large admin lists.
- Customer-facing mutations must be login-protected, ownership-checked, and `POST`-only unless they are truly read-only.
- Admin write routes must enforce backend `_acl`, not only frontend UI permissions.
- Prefer official extension points over overrides: events, decoration, DAL extensions, Store API/Admin API routes, Twig blocks.
- Keep controllers, subscribers, and handlers thin. Move behavior into services.
- Never trust client-reported payment state, amount, or provider references. Verify amount, currency, state, and recurring references server-side.
- Ownership-check saved methods, provider card-choice records, and subscription payment-method changes by customer and sales-channel context.
- Make webhook, finalize, and callback flows signature-verified or provider-verified and idempotent.
- Review `convertedOrder`, cart snapshots, `customFields`, and `ApiAware` fields as data-exposure surfaces.
- Keep scheduled tasks, reminders, and renewal scans batched, bounded, and keyed off technical names or immutable state, not translated labels.
- Log correlation IDs and redacted metadata, not provider payloads, tokens, signatures, or customer PII.
- Keep template logic cheap, admin UIs thin, and background work asynchronous when possible.
- If the repo uses local `AGENTS.md` files or the user asks for them, apply the layered guidance from `references/01-source-hierarchy.md`.

## Reference Map

Read only the files needed for the current task:

- `references/01-source-hierarchy.md`
  Use for source-of-truth rules, attached-source clustering, and repo-local `AGENTS.md` layering.
- `references/02-version-targeting.md`
  Use first when the target version is unclear or the task may differ between 6.7 and 6.6.
- `references/03-implementation-workflow.md`
  Use for step-by-step delivery, chunking, validation, regression control, and default PR description structure.
- `references/04-plugin-backend.md`
  Use for plugin architecture, DI, DAL, Store API/Admin API, caching, migrations, queues, and project-level backend work.
- `references/05-storefront-and-themes.md`
  Use for Twig, storefront JS, SCSS, themes, page loaders, SEO, accessibility, and performance-sensitive template work.
- `references/06-administration-and-apps.md`
  Use for admin modules, services, permissions, apps, Vue 3 migration, and 6.7 Pinia/Vite migration work.
- `references/07-payments.md`
  Use for payment handlers, redirects, tokenization, webhooks, saved methods, refunds, recurring PSP behavior, and payment reviews.
- `references/08-analysis-and-reviews.md`
  Use for audit mode, severity ordering, triangulation reviews, big-instance reasoning, and concise review output.
- `references/09-symfony-and-php.md`
  Use for Symfony and PHP implementation rules that matter inside Shopware plugins and projects.
- `references/10-official-docs-map.md`
  Use when you need exact official docs to open for the current task.
- `references/11-quality-and-operations.md`
  Use for security baseline, external API resilience, observability, testing, recurring-batch safety, store readiness, and the common failure modes called out in the general plugin guide.
- `references/12-extension-patterns.md`
  Use for plugin configuration, Flow Builder and business events, CMS elements, import/export profiles, media handling, and Rule Builder extensions.
- `references/13-context-and-commerce.md`
  Use for `Context` vs `SalesChannelContext`, multichannel behavior, recurring totals, pricing, customer groups, tax state, and sales-channel-aware logic.
- `references/14-cli-and-dev-tooling.md`
  Use for Symfony console commands, static analysis, code quality tooling, and local developer workflows.
- `references/15-subscriptions-and-recurring-payments.md`
  Use for subscription lifecycle reviews, recurring-order correctness, saved-payment-method ownership, provider contract drift, and multi-plugin recurring audits.

## Output Expectations

- State the target Shopware version explicitly when it affects the answer.
- For implementation work, explain the next smallest safe step rather than proposing a broad rewrite.
- For review work, present findings first, ordered by severity and blast radius.
- Every material finding should include the failure mode, blast radius, smallest safe fix, regression note, and validation note.
- When the flow spans multiple plugins or repos, add a separate cross-flow section for contract drift instead of hiding it inside one plugin review.
- When the user asks for a PR description, merge request description, or release-ready implementation summary, default to the detailed structure from `references/03-implementation-workflow.md` unless the user requested another format.
- For uncertain versioned behavior, say that verification against the exact docs/core tag is required.
