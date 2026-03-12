---
name: shopware-development
description: Plan, implement, refactor, or review Shopware 6 changes with Shopware 6.7 as the default target and explicit Shopware 6.6 compatibility handling. Use for plugin development, theme and template work, storefront Twig/JS/SCSS changes, accessibility or storefront compliance reviews, headless or composable frontend integrations, administration modules, app extensions, payment integrations, subscriptions and recurring-commerce flows, saved-payment-method ownership issues, CMS elements, import/export work, media handling, Rule Builder conditions, CLI and tooling tasks, multichannel context issues, project implementation planning, migrations, performance/security hardening, and Shopware code reviews or multi-plugin architecture analysis.
---

# Shopware Development

## Overview

Use this skill as base context and guardrails for Shopware work. It must never constrain Codex from using full model capacity for end-to-end review, deep investigation, or broader solution search when the codebase or request requires it. It distills the attached Shopware guides and agent pack into a small workflow layer plus clustered references, with official Shopware docs as the authority when guidance conflicts. Use it for recurring commerce, saved-payment-method flows, headless payment frontends, accessibility or storefront compliance reviews, and multi-plugin audits where backend plugins and separate frontend packages share one customer journey.

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
   - accessibility or storefront compliance review
   - headless or composable frontend work
   - administration or app extension work
   - payment work
   - subscription or recurring-commerce flow
   - integration or multi-repo review
   - review or architecture analysis
3. Load only the relevant reference files for that task. Treat them as starting context, not a hard boundary. Keep following code, evidence, configs, adjacent repos, and official docs when the task requires it.
4. If the task mentions accessibility, ADA, WCAG, VPAT, screen readers, keyboard support, forms, focus, modals, storefront UX, or theme/frontend audits, always load:
   - `references/05-storefront-and-themes.md`
   - `references/10-official-docs-map.md`
   - `references/17-accessibility-and-template-best-practices.md`
5. Inventory mutation surfaces, trust boundaries, ownership boundaries, browser-authored data, cross-plugin contracts, and accessibility-sensitive UI patterns before proposing fixes.
6. Plan the smallest safe change set. One concern per chunk: versioning, data model, API, UI, payment flow, recurring flow, accessibility remediation, or review findings. If the codebase or user request requires a broader change, expand intentionally instead of forcing an artificial micro-change.
7. Preserve extension points and existing project patterns unless the task explicitly includes a migration.
8. For multi-repo reviews, do not finalize findings until a triangulation pass checks how identifiers, saved methods, browser-stored values, custom fields, webhook state, recurring references, and shared frontend accessibility contracts move across repos.
9. Validate after each chunk with the narrowest useful checks.
10. Report version assumptions, what changed, what was verified, and any remaining regression risk.

## Execution Rules

- Default to Shopware 6.7 guidance. Load 6.6 compatibility notes only when the project or request requires them.
- Keep changes small and reversible. Avoid multi-concern diffs unless the code is tightly coupled.
- Protect hot paths first: cart, checkout, login, account, renewal, and large admin lists.
- Customer-facing mutations must be login-protected, ownership-checked, and `POST`-only unless they are truly read-only.
- Admin write routes must enforce backend `_acl`, not only frontend UI permissions.
- Prefer official extension points over overrides: events, decoration, DAL extensions, Store API/Admin API routes, Twig blocks.
- Keep controllers, subscribers, and handlers thin. Move behavior into services.
- This skill is a base layer, not a reasoning cap. For full reviews, full audits, or end-to-end bug hunts, scan the full relevant surface even when it extends beyond the initially loaded references.
- Prefer semantic HTML over ARIA patching. Use ARIA only when the native element cannot express the behavior.
- Use buttons for actions and links for navigation. Do not use `href="#"` pseudo-controls.
- Reusable storefront components must keep IDs unique and `aria-controls`, `aria-labelledby`, and `aria-describedby` relationships valid.
- Custom storefront JS widgets must handle focus entry, focus return, and live status updates where the interaction changes visible state asynchronously.
- Do not treat filename-derived alt text, browser `alert()`, or body-wide DOM repair scripts as acceptable default accessibility fixes.
- Do not use heavy Twig helpers such as `searchMedia()` inside loops or CMS slots when the data can be prepared once upstream.
- Scope storefront plugin selectors to `this.el` or the component root. Avoid broad `document.querySelector(...)` patterns that break repeated components.
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
- `references/17-accessibility-and-template-best-practices.md`
  Use for U.S. ecommerce accessibility baselines, WCAG mapping, Shopware 6.7 storefront alignment, custom theme and storefront-plugin accessibility patterns, and developer-ready accessibility review output.
- `references/16-headless-and-composable-frontends.md`
  Use for headless or composable frontend packages, browser-storage trust boundaries, frontend/backend payment contracts, and cross-repo payment UX audits.
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
- When the user asks for a full review or end-to-end analysis, use this skill as context only and keep the investigation open until the relevant surfaces are exhausted.
- For review work, present findings first, ordered by severity and blast radius.
- When accessibility is a primary scope, put accessibility findings first, map them to WCAG, name the impacted flow, and distinguish confirmed code defects from runtime verification gaps or vendor limitations.
- Every material finding should include the failure mode, blast radius, smallest safe fix, regression note, and validation note.
- For implementation-ready accessibility reviews, include code samples when the user asks for them.
- When the flow spans multiple plugins or repos, add a separate cross-flow section for contract drift instead of hiding it inside one plugin review.
- When backend and frontend repos share one payment flow, make it explicit which values are browser-authored hints and where server authority is re-established.
- When the user asks for a PR description, merge request description, or release-ready implementation summary, default to the detailed structure from `references/03-implementation-workflow.md` unless the user requested another format.
- For uncertain versioned behavior, say that verification against the exact docs/core tag is required.
