# Search and Indexing

Use this file when the task touches product search, storefront listings, Elasticsearch or OpenSearch customization, index mapping, indexers, or search-related performance.

## Search Surface Choice

- Prefer Shopware's search and listing routes when the task is storefront search or filtering behavior.
- Use DAL reads for bounded backoffice or support tasks, not as a substitute for storefront search architecture.
- Treat Elasticsearch or OpenSearch extension points as mapping plus fetch plus query responsibilities together. Do not modify only one side.

Example patterns:

```php
// Bad: raw DAL search used as a storefront search system
$criteria = new Criteria();
$criteria->addFilter(new ContainsFilter('name', $term));

return $this->productRepository->search($criteria, $context);
```

```php
// Preferred: use Shopware search or listing routes so the configured search backend stays authoritative
return $this->productSearchRoute->load(
    new Request(['search' => $term]),
    $salesChannelContext,
    (new Criteria())->setLimit(24)
)->getListingResult();
```

## Mapping and Fetch Symmetry

- If you extend indexed fields, update mapping and fetch behavior together.
- Keep mapped fields bounded and purpose-driven. Do not stuff large blobs or low-value debug data into the index.
- Preserve language, sales-channel, and visibility semantics when extending indexed data.
- When decorating existing indexers or Elasticsearch definitions, keep the change additive and compatible with the decorated service.

```php
// Preferred: extend mapping and fetch together through decoration
public function mapping(array $mapping): array
{
    $mapping = $this->inner->mapping($mapping);
    $mapping['properties']['myField'] = AbstractElasticsearchDefinition::KEYWORD_FIELD;

    return $mapping;
}
```

## Query Builder Customization

- Extend product search query builders or search decorators instead of bypassing the configured search backend with raw SQL.
- Keep custom scoring, term expansion, and boosted fields explicit and measurable.
- If the project must support fallback to MySQL, make the degraded behavior explicit and observable.

## Reindexing and Operations

- Mapping changes require a reindex strategy. Do not ship mapping changes without naming the reindex implication.
- Keep index updates and batch jobs bounded.
- If the project uses admin OpenSearch or search-specific flags, document the opt-in or fallback behavior in release notes and QA guidance.
- When search behavior changes, verify both correctness and operational blast radius: index size, rebuild time, and fallback behavior.

## Review Prompts

- Is storefront search using the correct route or backend abstraction?
- If a field is added to the index, is fetch logic updated to populate it?
- Does the custom search logic preserve language, visibility, and sales-channel scope?
- What happens when Elasticsearch or OpenSearch is disabled or stale?
- Is any search change going to force reindexing, and is that captured in rollout guidance?

## Cross-References

- Load `04-plugin-backend.md` for DAL and route design.
- Load `10-official-docs-map.md` for official docs and core-source anchors.
- Load `11-quality-and-operations.md` for rollout, observability, and fallback discipline.
- Load `13-context-and-commerce.md` when search behavior depends on pricing or sales-channel scope.
