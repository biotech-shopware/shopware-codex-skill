# Context and Commerce

## Context Types

- Use `SalesChannelContext` for storefront and Store API behavior.
- Use `Context` with `AdminApiSource` for Admin API work.
- Use system context for CLI, workers, and background processes only when that scope is appropriate.
- Do not create `Context::createDefaultContext()` in storefront flows and assume pricing, visibility, currency, or customer state will still be correct.

Example:
- storefront request
  Use `SalesChannelContext`, then extract `getContext()` only for DAL operations.
- admin write
  Verify admin source and ACL expectations instead of reusing storefront context logic.

## Sales Channel Awareness

- Respect `salesChannelId`, language, currency, customer group, and tax state in customer-facing logic.
- Enforce ownership and channel boundaries server-side for saved customer data, tokens, and account operations.
- Do not assume one storefront, one language, or one price context.
- If shipping methods, catalogs, or prices vary for roles such as sales agents or B2B segments, enforce that in backend resolution and route validation, not only in Twig visibility.

## Pricing and Tax

- Keep pricing logic aware of currency, tax state, rule evaluation, and customer group.
- Avoid computing or caching prices outside the correct sales-channel scope.
- Treat external tax integrations as hot-path risks and apply the same timeout and fallback rules used elsewhere in checkout.

## B2B and Organization-Sensitive Logic

- When a project has company accounts, roles, approval flows, or budget logic, keep those boundaries explicit instead of overloading standard customer assumptions.
- Do not let a single customer-level shortcut bypass channel, role, or organization checks.

## Cross-References

- Load `04-plugin-backend.md` for DAL, Store API, and caching behavior.
- Load `11-quality-and-operations.md` for shipping, tax, and external dependency resilience.
