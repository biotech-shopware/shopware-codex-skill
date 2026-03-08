# Analysis and Reviews

## Review Mode

Use review mode when the user asks for:

- a code review
- a security or performance audit
- architecture analysis
- regression risk assessment
- upgrade-safety review

## Source Rules

- Pin the Shopware version from project files before judging best practices.
- Use official Shopware docs, matching core behavior, and Symfony docs as the basis for recommendations.
- When linking core behavior, pin links to the exact Shopware tag or commit from `composer.lock` when possible.
- Treat ADRs, issues, forum posts, and blog posts as labeled context unless verified.

## Scope Coverage

Scan the full affected scope rather than just the first suspicious file:

- backend services, subscribers, controllers, routes, DAL definitions, migrations, commands, handlers, and DI config
- storefront Twig, JS, SCSS, theme overrides, and SEO-sensitive surfaces
- administration modules, permissions, build compatibility, and app surfaces
- config, queue, logging, environment-sensitive code, and CI hooks
- translations and snippet filename conventions when localization is touched

## Scan Order

Prioritize the highest-risk surfaces first:

1. security boundaries
2. payment correctness
3. cart, checkout, login, account hot paths
4. cache, invalidation, indexer, and queue behavior
5. DAL query shape, migrations, and large-instance impact
6. storefront performance and SEO
7. admin ACL and operational safety
8. extensibility and upgrade safety

## Severity Model

- P0
  Security vulnerabilities, payment trust-boundary failures, hot-path regressions, cache-collapse risks, broken idempotency.
- P1
  Large performance problems outside the hottest flows, migration/index risks, broad admin or storefront regressions.
- P2
  Maintainability, lower-risk upgrade issues, or testing gaps that still matter.

If the repo is clean, say so directly and mention residual testing or verification gaps instead of inventing findings.

## Big-Instance Stress Model

Assume production scale unless the repo proves otherwise:

- large catalogs and customer tables
- many sales channels and languages
- heavy imports and queue usage
- Redis, reverse proxy, CDN, and background workers

For each important finding, explain:

- what breaks first at scale
- the complexity shape or unbounded behavior
- the cache, queue, or DB blast radius

## Evidence Rules

Every material finding should include:

- exact file path and symbol, with line references when practical
- a realistic failure mode or regression scenario
- the smallest fix direction that matches Shopware patterns
- copyable inline references as raw URLs in backticks when a source is cited

## Review Output

Default format:

- findings first
- ordered by severity and impact
- each finding names the file or surface, the risk, and the smallest safe fix direction

Avoid checklist spam. Only report material issues that are actually present.

## Writing Style

- Write like a senior engineer, not a pasted system checklist.
- Keep sentences short and decisive.
- Avoid filler and speculative noise.
- If there are no findings, say so clearly and mention remaining verification gaps instead of padding the report.

## Cross-References

- Load `02-version-targeting.md` when version-specific behavior is relevant.
- Load `07-payments.md` for payment reviews.
- Load `05-storefront-and-themes.md` for storefront-heavy reviews.
- Load `11-quality-and-operations.md` for security, observability, store-readiness, and third-party resilience checks.
- Load `13-context-and-commerce.md` when sales-channel, currency, tax, or permission scope matters.
