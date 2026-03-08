# Implementation Workflow

## Goal

Deliver Shopware changes in small, reviewable chunks with explicit version handling and low regression risk.

## Step 1: Frame the Task

- Identify the target version and active extension surface.
- Decide whether the task is implementation, refactor, migration, or review.
- Name the hot path if one exists: cart, checkout, login, account, admin list, search, webhook, queue worker.

## Step 2: Inspect Before Editing

Read only what is necessary:

- nearest `AGENTS.md` files if the repo uses them
- the relevant `composer.json` files
- the existing service, route, subscriber, Twig block, admin module, or payment handler that owns the behavior
- the relevant reference file from this skill

Do not jump from a bug report straight to a rewrite.

## Step 3: Choose the Extension Point

Prefer the least invasive mechanism that solves the task:

1. event subscription
2. service decoration
3. DAL entity extension or custom entity
4. Store API/Admin API route or page loader
5. Twig block extension

Avoid direct core overrides, copied templates without a block-based reason, or ad hoc service lookups in runtime code.

## Step 4: Plan One Concern Per Chunk

Good chunks:

- add a migration and repository usage
- add one Store API route and its service
- refactor one payment webhook flow to be idempotent
- move one heavy storefront computation into a page loader
- harden one admin action with ACL and backend validation

Bad chunks:

- migration plus admin rewrite plus caching changes
- payment correctness plus saved methods plus refunds in one diff
- storefront redesign mixed with backend query refactors

## Step 5: Implement Safely

Guardrails:

- keep controllers, handlers, subscribers, and Twig thin
- use constructor injection
- load only the data needed
- avoid repository calls inside loops
- queue heavy work and external I/O when it is not request-critical
- do not trust client-supplied payment or authorization state
- do not add cookies, headers, or invalidations casually on cacheable routes

## Step 6: Validate the Smallest Useful Surface

Pick checks that match the change:

- PHP backend: static analysis, unit/integration tests, targeted console commands
- migrations: migration execution plan, index coverage, destructive update separation
- storefront: template rendering path, JS build or lint if present, accessibility and SEO sanity
- administration: build/test path used by the repo, ACL behavior, API responses
- payments: redirect/finalize flow, webhook verification, idempotency, retry safety
- distributable extensions: translation filename validation, snippet validation, extension quality checks, and any `shopware-cli` validation the project already relies on

If checks cannot run, say so and explain what remains unverified.

## Step 7: Run a Regression Scan

Ask explicitly:

- What existing extension point might this change break?
- What gets slower at scale?
- What changes cache tags, invalidation frequency, queue load, or DB access patterns?
- If the change touches storefront caching, what happens to cache hit rate, cache-key permutations, and `sw-cache-hash` separation?
- What part of the feature remains version-sensitive?

## Step 8: Report the Outcome

Always capture:

- target Shopware version
- files or surfaces changed
- validation performed
- remaining risk or follow-up chunk

For review work, use `08-analysis-and-reviews.md` instead of this file's delivery format.
