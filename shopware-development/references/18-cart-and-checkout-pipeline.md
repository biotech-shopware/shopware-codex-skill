# Cart and Checkout Pipeline

Use this file when the task touches cart collectors, processors, validators, line items, delivery calculation, promotion interactions, checkout recalculation, or anything that runs on the customer hot path.

## Core Pipeline Mental Model

The stable Shopware core pattern is:

1. collectors enrich `CartDataCollection`
2. the cart amount is calculated
3. processors run in order and the amount is recalculated after each processor
4. validators add cart errors
5. transaction data is built from the processed cart

Treat that order as a correctness boundary, not just an implementation detail.

## Collector vs Processor

- Collectors fetch or precompute data once and write it into `CartDataCollection`.
- Processors read prepared data and mutate the calculated cart.
- Validators add errors or block invalid states; they should not hide heavy I/O or business side effects.
- If you need database or API data for many line items, collect it once, keyed by stable IDs, and let the processor read it.

Example patterns:

```php
// Bad: repository calls inside process() for every line item
public function process(CartDataCollection $data, Cart $original, Cart $toCalculate, SalesChannelContext $context, CartBehavior $behavior): void
{
    foreach ($original->getLineItems() as $lineItem) {
        $product = $this->productRepository->search(
            new Criteria([$lineItem->getReferencedId()]),
            $context->getContext()
        )->first();

        $lineItem->setLabel($product?->getName() ?? $lineItem->getLabel());
        $toCalculate->add($lineItem);
    }
}
```

```php
// Preferred: batch in collect(), read in process()
public function collect(CartDataCollection $data, Cart $original, SalesChannelContext $context, CartBehavior $behavior): void
{
    $ids = array_filter($original->getLineItems()->getReferenceIds());
    if ($ids === []) {
        return;
    }

    $criteria = new Criteria($ids);
    $criteria->addFields(['id', 'name']);

    $products = $this->productRepository->search($criteria, $context->getContext());
    $data->set('my-plugin.products', $products->getEntities());
}

public function process(CartDataCollection $data, Cart $original, Cart $toCalculate, SalesChannelContext $context, CartBehavior $behavior): void
{
    /** @var EntityCollection<ProductEntity> $products */
    $products = $data->get('my-plugin.products') ?? new ProductCollection();

    foreach ($original->getLineItems() as $lineItem) {
        $product = $products->get($lineItem->getReferencedId());
        $lineItem->setLabel($product?->getName() ?? $lineItem->getLabel());
        $toCalculate->add($lineItem);
    }
}
```

## Processor Ordering and Recalculation

- Processor order matters because price and delivery totals are recalculated after each processor.
- Do not assume your processor runs last unless you control the tag priority and have verified the interaction.
- Keep shipping, promotion, and custom-fee interactions explicit. If your processor changes line item price definitions, check delivery and promotion fallout.
- If the processor depends on data from another processor, document the priority dependency instead of relying on container order luck.

## Line Item Creation and Enrichment

- Prefer `LineItemFactoryRegistry` or existing line item types over ad hoc line item construction from controllers.
- Use dedicated line item types for plugin-owned fees, credits, bundles, or markers.
- Keep payloads minimal and deterministic. Do not hide operational state in large mutable payload blobs.
- If the storefront creates line items, validate referenced IDs, customer scope, and allowed payload shape before adding them.

Example patterns:

```php
// Bad: ad hoc line item with generic type and mutable blob payload
$lineItem = new LineItem(Uuid::randomHex(), 'custom');
$lineItem->setPayload($request->request->all());
```

```php
// Preferred: dedicated line item type and whitelisted payload
$lineItem = $this->lineItemFactoryRegistry->create([
    'type' => 'my_plugin_fee',
    'id' => 'my-plugin-fee',
    'referencedId' => $feeConfigId,
    'payload' => ['source' => 'checkout-config'],
], $context);
```

## Validators

- Use cart validators for cart-state validation, not as a hidden place for outbound HTTP or database-heavy work.
- Add actionable cart errors and keep them scoped to the exact line item or checkout state that failed.
- Payment, shipping, and recurring-method validators must not widen customer access or trust browser-authored identifiers.

## Delivery, Shipping, and Promotion Interactions

- Delivery calculation and promotion calculation are part of the same checkout truth surface.
- If you add discounts, credits, or fees, verify shipping costs, free-shipping thresholds, promotion stacking, and tax behavior.
- Do not branch shipping or promotion logic on translated labels or storefront-only presentation state.
- Keep shipping-method technical names stable across config and migration changes.

## Order Conversion and Recalculation

- Do not assume a cart snapshot is authoritative forever. Recalculate from the current sales-channel context when payment, shipping, currency, or rules may have changed.
- Keep the boundary between cart calculation and order persistence explicit. Do not mutate finished orders from casual cart helpers.
- When a renewal or post-order edit reconstructs a cart, re-run the real calculation path instead of patching totals manually.

## Hot-Path Red Lines

- No slow external API calls on the customer checkout path unless the business flow truly requires it and the timeout is tight.
- No repository `search()` inside loops over line items.
- No broad association trees or cart snapshot hydration just to render one badge, label, or fee.
- No writes or recalculations hidden in read routes that only load confirm, cart, or offcanvas pages.

## Review Prompts

Minimum questions to answer in cart or checkout reviews:

- Is expensive data fetched once in a collector or repeatedly in a processor?
- Does any processor priority assumption break shipping, promotions, or tax state?
- Are custom line item types explicit and stable?
- Are cart validators cheap and deterministic?
- Does any cart, confirm, or checkout page loader trigger writes, remote I/O, or heavy recalculation?
- If payment is involved, where does payment authority rejoin the cart or order flow?

## Official Anchors

- cart concept: `https://developer.shopware.com/docs/concepts/commerce/checkout-concept/cart.html`
- add cart collector/processor: `https://developer.shopware.com/docs/guides/plugins/plugins/checkout/cart/add-cart-processor-collector.html`

## Cross-References

- Load `04-plugin-backend.md` for DAL, Store API, migrations, and async boundaries.
- Load `07-payments.md` when payment handlers or finalize flows intersect the checkout path.
- Load `13-context-and-commerce.md` for pricing, tax, and sales-channel context rules.
- Load `19-testing-patterns.md` for cart integration and Store API test guidance.
