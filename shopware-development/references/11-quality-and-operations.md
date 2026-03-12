# Quality and Operations

## Security Baseline

- Escape output by default and treat rich HTML as hostile unless it is sanitized.
- Prefer DAL to reduce injection risk. If raw SQL is necessary, bind every parameter.
- Treat any URL ingestion, remote file import, or webhook callback target as SSRF-sensitive: validate scheme and host, block private IP ranges, apply timeouts, and cap payload size.
- Use the official CSRF or request-authenticity protections for storefront or admin-initiated write flows where the platform expects them.
- Enforce ACL for admin operations and ownership checks for storefront or Store API operations.
- Never commit secrets. Treat system configuration values, API keys, and provider tokens as sensitive and keep them out of logs.
- Never set `verify => false` or equivalent TLS-disable flags in production HTTP clients.
- Distinguish browser-write routes from server-to-server endpoints. Browser-initiated POSTs need CSRF or equivalent request protection; webhooks and signed callbacks usually disable CSRF and replace it with sender authentication.
- State-changing customer-facing endpoints should be POST-only and protected against IDOR or ownership bypass. Do not expose mutations over GET.
- Do not log tokens, context tokens, provider responses, or payment payloads to the browser console or client-side telemetry by default.
- Do not treat `localStorage`, `sessionStorage`, or other browser-persisted values as authoritative payment, vault, or ownership state.

Official docs:

- security reference: `https://developer.shopware.com/docs/resources/references/security.html`

## External API Resilience

- Timeouts are mandatory.
- Use circuit-breaker or graceful-degradation behavior when a dependency can affect storefront, checkout, or admin usability.
- Favor queue-first synchronization and retries for non-interactive work.
- A provider outage must not crash the storefront, cart, checkout, or core admin workflows.
- Public callback or authorization endpoints need explicit hardening: signed tokens, bounded input, rate limiting, and narrow purpose.

### Shipping and Tax Notes

- Do not block checkout on shipping-rate lookups; cache or decouple them where possible.
- Keep shipping method technical names and config migrations stable.
- Bound external tax calls in cart and checkout with caching, timeouts, and fallback behavior.
- Push reporting and other tax side effects into async processing.

## Observability

- Use structured logging with correlation IDs and actionable context.
- Log failures, retries, and state changes without dumping full request or response payloads.
- Redact tokens, signatures, secrets, and PII by default.
- Keep a lightweight audit trail for merchant support, but do not turn application tables into an unbounded log sink.
- If the domain stores order snapshots, cart snapshots, or provider payloads, keep only the minimum fields needed for support and reconciliation.
- Review recurring-payment and webhook logs specifically for leaked provider tokens, card identifiers, customer identifiers, signatures, and raw payload bodies.

Official docs:

- Shopware logging: `https://developer.shopware.com/docs/guides/hosting/configurations/observability/logging.html`
- Symfony logging: `https://symfony.com/doc/current/logging.html`

## Testing and CI

- Unit-test core services.
- Integration-test DAL definitions, migrations, and critical flows.
- Use Store API or HTTP-level tests for custom routes and customer-facing API behavior.
- Use reusable fixtures or builders when repeated setup would otherwise hide intent.
- For frontend-heavy extensions, add smoke checks and compare Lighthouse results where performance risk is real.
- Improve PHPStan incrementally instead of freezing a huge baseline forever.
- Keep coding standards aligned with Shopware conventions.
- Do not ship or leave behind `die`, `exit`, or `var_dump`.
- Validate and package distributable extensions with Shopware CLI when the project uses it.
- For 6.7.3+ translation work, add `translation:lint-filenames` and `snippet:validate` to CI when possible.
- Run at least one container-compile or boot-path check when service wiring, decorators, handlers, or scheduled tasks changed.

## Store Readiness

- Test against the highest supported Shopware version before release.
- Keep the binary clean: do not ship unnecessary tests, CI config, editor settings, lock files, or other forbidden packaging artifacts.
- Register storefront cookies with the cookie consent manager when the feature sets them.
- Use store quality guidelines and store review errors as release gates, not as cleanup after the implementation is done.

Official docs:

- quality guidelines: `https://developer.shopware.com/docs/resources/guidelines/testing/store/quality-guidelines-plugins/`
- store review errors: `https://developer.shopware.com/docs/guides/development/monetization/store-review-errors.html`
- Shopware CLI: `https://developer.shopware.com/docs/products/cli/`
- Shopware CLI validation: `https://developer.shopware.com/docs/products/cli/validation.html`

## Common Failure Modes

Treat these as explicit regression checks:

- external HTTP calls in cart, checkout, or login without tight timeouts and fallback
- rendering user-specific HTML into cacheable responses
- DAL `search()` inside loops or other N+1 query shapes
- unbounded criteria queries or large blob hydration without batching
- broad cache invalidations on high-frequency events
- missing ACL or ownership checks
- logging secrets, tokens, or raw payloads
- browser console or client telemetry logging of tokens, context tokens, or payment payloads
- exposing Admin API credentials or private third-party keys to the storefront
- browser storage used as an authoritative source for saved-method, vault, or payment identity
- browser routes that disable CSRF without replacing it with the right security model
- GET endpoints that mutate customer data or order state
- internal plugins calling the shop's own HTTP API instead of shared services
- writes or recalculations triggered from storefront render events or read routes
- route duplication that drops core ACL or validation
- scheduled tasks with tiny intervals, unbounded scans, or duplicate handler registration
- scheduled tasks, reminders, or renewal workers that branch on translated state names instead of technical names
- invalid DAL association usage such as treating `customFields` like a normal association
- heavy blob hydration or full snapshot loading on large recurring batches without a clear need
- cache entries with unbounded growth because keys are serialized payloads and TTL is missing
- mutable operational flags stored in JSON payloads or custom fields and later queried at scale
- deep template copying instead of block extension
- copying core-only metadata into plugin code
- silent API contract changes without versioning or compatibility planning
- shipping dev artifacts or duplicated translation files in the plugin package

## Scheduled Tasks and Recurring Batches

- Batch due-item scans explicitly. Do not assume production data sets stay small.
- Keep loop control safe when external failures or partial processing happen. Do not advance business-critical schedule fields before success is known.
- Prefer technical names, stable IDs, or persisted state markers over translated labels in task logic.
- Avoid broad association trees in scheduled-task hydration unless every field is required for the batch decision.
- If a task stores snapshots or support data, put a bound on retention and growth.
