# Plugin Backend

## Architecture

Shape plugin backend code like a disciplined Symfony application:

- thin glue layer
  Controllers, subscribers, message handlers, scheduled tasks, Store API/Admin API routes.
- application services
  Business rules, orchestration, state transitions, idempotency, retries.
- infrastructure adapters
  DAL repositories, HTTP clients, SDK wrappers, filesystem access.

Keep orchestration out of controllers and subscribers.

## First Principles

- Treat every feature as either request-path code or background code. Request-path code must stay cheap.
- Protect hot paths first: cart, checkout, login, account, and any large admin listing.
- Treat cache hit rate, invalidation frequency, indexer backlog, queue backlog, and DB query count as the primary performance levers.
- Keep an uninstall and cleanup story for every custom entity row, custom field, config key, media asset, and scheduled operational artifact.

## Plugin Structure

Baseline structure:

- `composer.json` with PSR-4 autoloading
- `src/<Plugin>.php` kept thin
- `src/Service/`
- `src/Subscriber/`
- `src/MessageHandler/`
- `src/Migration/`
- `src/Resources/config/services.xml`

Use install/update lifecycle code only for plugin lifecycle concerns, not general business behavior.

## Compatibility and Upgrade Safety

- Pin supported Shopware packages conservatively. Avoid broad wildcard constraints.
- Prefer documented extension points and stable core patterns over `@internal` behavior.
- Do not copy core-only metadata such as `#[Package(...)]` or `@internal` tags into plugin public code.
- If a design intentionally couples to a fragile event payload or core behavior, document that coupling explicitly.

## DI and Service Design

- Use constructor injection only.
- Avoid `$container->get()` in runtime code.
- Prefer decoration or events over replacing core behavior by inheritance.
- Wrap external clients in services that centralize timeouts, retries, and logging policy.

Open official docs when the task touches service wiring:

- add custom service: `https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/add-custom-service.html`
- adjust service: `https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/adjusting-service.html`

## DAL and Data Modeling

- Use DAL first; fall back to DBAL only when necessary.
- Load only required associations. Do not hydrate full entity graphs by default.
- Prefer `searchIds()` when only identifiers are needed, then fetch a second time only if full entities are required.
- Never call repository `search()` inside loops over entities or line items. Treat that as a classic N+1 failure.
- Paginate admin lists, exports, and sync jobs rather than assuming bounded result sizes.
- Treat unbounded criteria queries as production incidents waiting to happen on large shops.
- Avoid pulling large serialized blobs such as cart payloads unless absolutely necessary; batch and cap memory if you must.
- Use custom fields for merchant-managed metadata, not write-heavy operational data.
- Use entity extensions or custom entities for structured domain state.
- Do not use mutable JSON payload fields on line items, orders, or logs as primary operational query state. If you need to query or deduplicate by that state, model it in indexed columns or a custom entity instead.
- Merge existing custom fields instead of overwriting the full array unless replacing everything is intentional and safe.

Entity extension decision matrix:

| Need | Prefer | Avoid when |
| --- | --- | --- |
| a few merchant-managed values on an existing entity | custom fields | the data drives high-volume writes, deduplication, or indexed operational queries |
| structured fields attached to an existing entity with code ownership | entity extension | the state really has its own lifecycle or needs standalone queries |
| standalone operational state, dedupe keys, sync history, or batch processing | custom entity | the data is just editorial decoration on an existing entity |
| merchant-defined structure in 6.7+ where dynamic entities truly fit the requirement | dynamic custom entities | the requirement needs plugin-owned behavior, strict invariants, or broad 6.6 support |

Example patterns:

```php
// Bad: full entity hydration just to collect IDs inside a loop
foreach ($customers as $customer) {
    $criteria = new Criteria();
    $criteria->addFilter(new EqualsFilter('customerId', $customer->getId()));

    $orders = $this->orderRepository->search($criteria, $context);
    $this->syncOrderIds($orders->getIds());
}
```

```php
// Preferred: batch IDs first, then fetch only what is needed
$criteria = new Criteria();
$criteria->addFilter(new EqualsAnyFilter('customerId', $customerIds));

$orderIds = $this->orderRepository->searchIds($criteria, $context)->getIds();
$this->syncOrderIds($orderIds);
```

```php
// Bad: loading full associations and blobs for an admin list
$criteria = new Criteria();
$criteria->addAssociation('lineItems');
$criteria->addAssociation('transactions');
$criteria->addAssociation('addresses');
```

```php
// Preferred: bounded list query plus targeted fields
$criteria = new Criteria();
$criteria->setLimit(50);
$criteria->setOffset($offset);
$criteria->addAssociation('transactions.stateMachineState');
$criteria->addFields(['id', 'orderNumber', 'createdAt']);
```

```php
// Preferred: merge custom fields instead of replacing unrelated state
$payload = [
    'id' => $orderId,
    'customFields' => [
        ...($order->getCustomFields() ?? []),
        'my_plugin_sync_state' => 'queued',
    ],
];
```

## Migrations and Schema Safety

- Keep schema changes idempotent where possible.
- Keep large data backfills out of `update()`. Ship schema first, backfill asynchronously in batches, and add final constraints or indexes only after the data is ready.
- Separate destructive operations into `updateDestructive()`.
- Add indexes that match real filters, joins, and sort keys.
- Add composite unique indexes where they enforce idempotency or natural uniqueness for operational tables.
- Use parameter binding for manual SQL.

Official docs:

- migrations: `https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/database-migrations.html`

## Store API and Admin API

- Keep routes thin and decoratable.
- Apply the correct route scope and permissions.
- Expose storefront-relevant behavior through Store API when headless use is plausible.
- Prefer cacheable reads where it is safe and make writes clearly non-cacheable.
- Enforce ownership server-side on Store API routes. Never expose privileged operations there.
- Keep privileged operations behind Admin API plus ACL.
- Keep API error shapes consistent and avoid leaking internal exception details through public responses.
- Version or isolate externally consumed contracts when breaking changes would affect headless or third-party clients.
- Apply rate limiting deliberately on abuse-prone or public endpoints, not blindly across trusted internal flows.
- Never accept raw `Criteria`, arbitrary associations, or client-selected query complexity directly from request payloads. Whitelist filters, pagination, sorting, and include depth explicitly.
- Keep headless endpoints bounded. Do not return "all snippets", "all entities", or deep graphs without a pagination and payload-size strategy.
- Public Store API or headless payment routes must never create authoritative local paid transaction rows or decide payment success from browser-authored inputs.
- For headless payment flows, derive amount, currency, and order context from server-side cart or order recalculation before any provider charge or local payment-state write.
- Resolve current-customer vaulted or saved-payment records server-side. Do not accept raw browser-stored vault identifiers as authoritative without ownership resolution.
- Prefer decorating services or inner routes over redefining existing public routes. Route duplication or controller copy-paste often drops ACL, validation, or behavior guarantees.
- Do not call your own shop's Admin API or Store API over HTTP from backend code in the same project. Inject the underlying services or repositories directly.

Official docs:

- add Store API route: `https://developer.shopware.com/docs/guides/plugins/plugins/framework/store-api/add-store-api-route.html`
- add cart collector/processor: `https://developer.shopware.com/docs/guides/plugins/plugins/checkout/cart/add-cart-processor-collector.html`

Keep detailed collector, processor, validator, delivery, and promotion pipeline rules in `18-cart-and-checkout-pipeline.md` instead of expanding this file with cart internals.

## Performance, Cache, and Async

- Treat cart, checkout, login, account, and high-volume queries as hot paths.
- Do not block request paths on external I/O unless absolutely required.
- Push expensive derived data or sync work into Messenger, indexers, or scheduled tasks.
- Separate low-priority or bulk background work from checkout-sensitive consumers when queue backlog could affect customer-facing flows.
- Plan for reverse proxy, CDN, and Varnish-style caching behavior.
- Avoid cache-key explosions from extra cookies, headers, or highly dynamic fragments.
- Do not assume "logged-in users disable HTTP cache" as a permanent invariant.
- If user-specific data is required, make it explicit with AJAX, ESI fragments, or non-cacheable routes rather than globally disabling cache.
- Before increasing cache variation or invalidation frequency, estimate the hit-rate impact and state the least destructive alternative.
- Do not perform DAL writes, entity updates, or expensive recalculations inside storefront read routes or generic render events just to decorate response data. Use runtime extensions, response extensions, or cacheable precomputation.
- Avoid `order.written` or state-transition subscribers that write the same entity again or call external systems synchronously. That pattern creates recursion, duplicate processing, and checkout latency.
- Normalize cache keys and set TTLs for external-quote or derived-data caches. Never cache entire serialized request payloads forever.

Official docs:

- cache concept: `https://developer.shopware.com/docs/concepts/framework/cache.html`
- HTTP cache: `https://developer.shopware.com/docs/concepts/framework/http_cache.html`
- plugin caching guide: `https://developer.shopware.com/docs/guides/plugins/plugins/framework/caching/`
- hosting caches: `https://developer.shopware.com/docs/guides/hosting/performance/caches.html`
- data indexer: `https://developer.shopware.com/docs/guides/plugins/plugins/framework/data-handling/add-data-indexer.html`
- message queue: `https://developer.shopware.com/docs/guides/hosting/infrastructure/message-queue.html`
- scheduled task: `https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/add-scheduled-task.html`

## External Integrations

- Centralize HTTP clients.
- Set explicit timeouts.
- Prefer retries only for safe, idempotent operations.
- Add circuit-breaker or fallback logic when the dependency can degrade the shop.
- Queue non-interactive sync work.
- Never disable TLS verification in production clients.
- Do not build callback or return URLs from `HTTP_HOST` or other untrusted request host headers. Use configured sales-channel domains or explicit trusted configuration.

If the integration is a payment provider, stop here and load `07-payments.md`.
