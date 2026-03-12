# Storefront and Themes

## Rendering Strategy

- Keep Twig templates cheap.
- Precompute data in controllers, page loaders, Store API routes, or dedicated services.
- Pass minimal context to templates.
- Extend templates with blocks before copying files.

Official docs:

- customize templates: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/customize-templates.html`

## Twig Rules

- Use `sw_extends` and block overrides for upgrade safety.
- Do not copy full core templates unless block overrides or smaller partial overrides are insufficient.
- Keep autoescaping enabled.
- Avoid `|raw` on untrusted data.
- Do not hide business logic, authorization logic, or ownership checks in templates.
- Do not traverse heavy entity graphs or large collections in templates when the data can be prepared once upstream.
- Do not leave `dump()` or debug helpers in shipped templates.
- Use snippets for translatable text.
- Do not fetch database-backed data from Twig in loops or CMS slots via helpers such as `searchMedia()`. Resolve data once in a page loader, CMS resolver, or service.
- Do not use hard-coded static IDs in reusable product, listing, CMS, or account components.
- Do not use `href="#"` or similar pseudo-controls for button behavior.
- Do not use positive `tabindex`.
- Do not embed inline script or style blocks inside repeated product-card, slider, or CMS loops.

## Storefront JS

- Use the Shopware storefront plugin system and data attributes for progressive enhancement.
- Keep client code thin and avoid duplicating server-side business rules.
- Prefer Store API or purpose-built endpoints for dynamic data.
- Do not embed secrets or privileged logic in storefront JS.
- Do not load third-party libraries globally when the feature only exists on one route or widget.
- Do not write untrusted HTML with `innerHTML`, `insertAdjacentHTML`, or equivalent sinks. Treat `v-html` with the same suspicion.
- Prefer `textContent` for status and label updates.
- Use `role="status"` or `role="alert"` for async UI feedback where users need an announcement.
- Manage focus explicitly for modals, flyouts, offcanvas panels, and other custom widgets that move the user through a stateful flow.
- Avoid browser `alert()` for recoverable failures or normal UI state changes.
- Scope selectors and event handling to the component root rather than broad document queries.
- Validate `postMessage` origins before trusting cross-window data.
- Escape data for JavaScript context explicitly; HTML escaping alone does not make inline JS safe.
- Do not inject the same external script, inline script, or inline style block inside product-card or CMS loops. Load once per page or route.
- Keep third-party widgets route-scoped and consent-aware.
- Throttle scroll handlers, constrain MutationObservers, and avoid polling loops that run across the whole storefront without a hard need.
- Do not use body-wide MutationObservers as the default way to repair labels, alt text, or other accessibility issues after render.

## SCSS and Theme Work

- Respect theme inheritance.
- Prefer variables, configuration, and stable CSS hooks over brittle selectors.
- Keep overrides local to the feature.
- Do not mix a large redesign with unrelated backend refactors.

Official docs:

- custom styling: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-custom-styling.html`
- SCSS variables: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-scss-variables.html`
- theme inheritance: `https://developer.shopware.com/docs/guides/plugins/themes/add-theme-inheritance.html`

## Performance and Cache

- Treat storefront performance as a cache and payload problem first.
- Avoid adding cookies or headers on cacheable responses without a strong reason.
- Move user-specific fragments to AJAX, Store API, or non-cacheable routes rather than disabling cache broadly.
- Protect LCP by keeping hero media, server response time, and render-blocking CSS under control.
- Protect CLS by avoiding late banners, async style jumps, and unstable placeholders.
- Protect INP by keeping interaction handlers and storefront JS bundles small.
- If navigation, catalog visibility, or shipping/payment presentation varies by customer segment, rule, or sales role, enforce that variation server-side and ensure the cache key varies accordingly. Hiding it only in Twig is not a security boundary.

## 6.7 Storefront Structure

- Review header and footer overrides carefully on 6.7 because shared fragment rendering changed.
- Re-check cache and template-target implications before copying older 6.6 storefront assumptions into 6.7 projects.

## SEO and Accessibility

- Preserve canonical and SEO URL behavior when adding pages or custom entities.
- Keep landmarks, headings, labels, control relationships, and focus handling correct on checkout and interactive UI.
- Use buttons for actions and links for navigation.
- Prefer semantic HTML over ARIA patching. Only add advanced ARIA patterns when the full behavior is implemented.
- Use snippets and semantic HTML instead of hardcoded strings or div-heavy widgets.
- If the change adds tracking, embeds, or third-party storefront scripts, verify consent and store-readiness impact explicitly.
- Do not set global noindex or similar SEO-breaking directives from a theme override without scoping and intent.
- For deeper storefront accessibility requirements, U.S. ecommerce baseline guidance, and WCAG-oriented review output, also load `17-accessibility-and-template-best-practices.md`.

Official docs:

- SEO guides: `https://developer.shopware.com/docs/guides/plugins/plugins/content/seo/`
- add custom SEO URL: `https://developer.shopware.com/docs/guides/plugins/plugins/content/seo/add-custom-seo-url.html`
- custom fields in storefront: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/using-custom-fields-storefront.html`
- add custom JavaScript: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-custom-javascript.html`
- using a modal window: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/using-a-modal-window.html`
- add cookie to manager: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-cookie-to-manager.html`
- reacting to cookie consent changes: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/reacting-to-cookie-consent-changes.html`
- working with media and thumbnails: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/use-media-thumbnails.html`

## Cross-References

- For cart or checkout behavior, also load `04-plugin-backend.md`.
- For payment UI, tokenization, or redirect/finalize flows, also load `07-payments.md`.
- For headless or composable frontend packages, also load `16-headless-and-composable-frontends.md`.
- For cookie consent, store readiness, or release quality checks, also load `11-quality-and-operations.md`.
- For deeper storefront accessibility review or remediation planning, also load `17-accessibility-and-template-best-practices.md`.
