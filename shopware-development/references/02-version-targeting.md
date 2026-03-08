# Version Targeting

## Default Rule

Default to Shopware 6.7 guidance. Switch to explicit 6.6 compatibility mode when:

- the root project or plugin constraints cap Shopware at `6.6.*`
- the user says the work must remain 6.6-compatible
- the repo contains transition-era admin or payment code that should not be silently migrated

## Detection Order

Check in this order:

1. Root `composer.json`
2. Plugin/theme/app `composer.json`
3. `composer.lock`
4. Existing code patterns that imply a versioned extension surface

State the version assumption before changing behavior.

## 6.7-First Deltas

Use these as default assumptions for 6.7 work:

- Payments
  Official 6.7 docs show the single `AbstractPaymentHandler` flow, `validate()` always being called, and `finalize()` being called when `pay()` returns a redirect response.
- Administration
  Use the official Vue 3, Pinia, and Vite migration docs when the task includes admin modernization. Treat these as explicit migrations, not incidental cleanup.
- Translations
  Shopware 6.7.3 introduced the country-independent snippet base layer. Use base files such as `messages.en.base.json` and `messages.de.base.json`, keep country-specific files as small overrides, preserve empty former country-specific files where backward compatibility needs them, and validate with `translation:lint-filenames` plus `snippet:validate`.
- Caching
  Follow the current cache and HTTP cache docs. Do not depend on legacy assumptions like "logged-in users always disable cache."

## 6.6 Compatibility Rules

When the project targets 6.6:

- Open the archived docs under `/docs/v6.6/` before relying on a 6.7-only pattern.
- Keep admin migrations explicit. Do not force Pinia or Vite unless the project is already migrating.
- Treat 6.7 translation-layer improvements as optional, not assumed.
- Re-check payment handler contracts and extension points against 6.6 docs before applying 6.7 examples.

## Compatibility Strategy

Use the least risky path:

- If behavior can stay version-neutral, do that.
- If the implementation must differ, isolate the version-sensitive seam in one service or adapter.
- If the task would require a wide migration, stop and name it as a separate chunk rather than mixing it into a feature change.

## Official Anchors

Start here when versioned behavior matters:

- 6.7 payment guide: `https://developer.shopware.com/docs/guides/plugins/plugins/checkout/payment/add-payment-plugin.html`
- 6.6 payment guide: `https://developer.shopware.com/docs/v6.6/guides/plugins/plugins/checkout/payment/add-payment-plugin.html`
- 6.7 template guide: `https://developer.shopware.com/docs/guides/plugins/plugins/storefront/customize-templates.html`
- 6.6 template guide: `https://developer.shopware.com/docs/v6.6/guides/plugins/plugins/storefront/customize-templates.html`
- 6.7 migrations guide: `https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/database-migrations.html`
- 6.6 migrations guide: `https://developer.shopware.com/docs/v6.6/guides/plugins/plugins/plugin-fundamentals/database-migrations.html`
- Vue 3 upgrade: `https://developer.shopware.com/docs/guides/plugins/plugins/administration/system-updates/vue3.html`
- Pinia upgrade: `https://developer.shopware.com/docs/guides/plugins/plugins/administration/system-updates/pinia.html`
- Vite upgrade: `https://developer.shopware.com/docs/guides/plugins/plugins/administration/system-updates/vite.html`
- Translation extension upgrade: `https://developer.shopware.com/docs/resources/references/upgrades/core/translation/extension-translation.html`
