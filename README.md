# Shopware Codex Skill

Single-skill repository for `shopware-development`, a Codex skill for Shopware 6 plugin work, project implementation, code review, payment integrations, subscriptions and recurring-commerce flows, storefront work, administration work, and architecture analysis.

The repository is intentionally small:

- one installable skill
- one root README
- one `.gitignore`
- no extra packaging layer

That shape keeps GitHub installation simple later and makes the repo usable immediately as a local skill source.

## Contents

- [What this repo is](#what-this-repo-is)
- [Repository structure](#repository-structure)
- [What the skill does](#what-the-skill-does)
- [Skill architecture](#skill-architecture)
- [Reference file guide](#reference-file-guide)
- [How the skill works with the current model](#how-the-skill-works-with-the-current-model)
- [When to use the skill](#when-to-use-the-skill)
- [When not to use the skill](#when-not-to-use-the-skill)
- [How to install locally in Codex](#how-to-install-locally-in-codex)
- [How to use it in Codex chats](#how-to-use-it-in-codex-chats)
- [How to use it in real Shopware projects](#how-to-use-it-in-real-shopware-projects)
- [Prompt patterns](#prompt-patterns)
- [Versioning and release](#versioning-and-release)
- [Future GitHub installation](#future-github-installation)
- [Maintenance notes](#maintenance-notes)

## What This Repo Is

This repository is the source for the installable `shopware-development` skill.

The skill is designed to make Codex better at Shopware-specific work without turning the skill into a large monolith. The core file stays lean, while domain-specific rules live in reference files that are loaded only when relevant.

This repo exists for four reasons:

1. Keep the skill versioned independently from any single Shopware project.
2. Preserve hard-won Shopware review and implementation rules across projects.
3. Make local installation simple now.
4. Make GitHub-based installation simple later.

## Repository Structure

```text
shopware-skills/
├── .gitignore
├── README.md
└── shopware-development/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        ├── 01-source-hierarchy.md
        ├── 02-version-targeting.md
        ├── 03-implementation-workflow.md
        ├── 04-plugin-backend.md
        ├── 05-storefront-and-themes.md
        ├── 06-administration-and-apps.md
        ├── 07-payments.md
        ├── 08-analysis-and-reviews.md
        ├── 09-symfony-and-php.md
        ├── 10-official-docs-map.md
        ├── 11-quality-and-operations.md
        ├── 12-extension-patterns.md
        ├── 13-context-and-commerce.md
        ├── 14-cli-and-dev-tooling.md
        └── 15-subscriptions-and-recurring-payments.md
```

### Root files

- [README.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/README.md)
  Operator guide for installing, using, maintaining, and releasing the skill.
- [.gitignore](/Users/vi.khliupko/Documents/Codex/shopware-skills/.gitignore)
  Excludes local scratch analysis artifacts and OS clutter. It should stay minimal.

### Skill root

- [SKILL.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/SKILL.md)
  Trigger file and high-level operating rules. This is the only file Codex needs to discover the skill and understand when it applies.
- [openai.yaml](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/agents/openai.yaml)
  UI metadata used by Codex to display the skill in lists and chips.
- `references/`
  Focused domain files. These are not meant to be bulk-loaded. The skill tells Codex to load only the relevant ones for the active task.

## What The Skill Does

`shopware-development` is built for:

- Shopware 6.7-first work with explicit 6.6 compatibility handling
- plugin implementation and refactoring
- storefront Twig, JavaScript, and SCSS work
- administration modules and app extensions
- payment handlers, redirects, webhooks, vaulting, refunds, and payment reviews
- subscriptions, renewals, saved-payment-method ownership, and recurring-commerce flows
- Store API and Admin API extension work
- DAL design, migrations, service architecture, and queue patterns
- project-level Shopware implementation planning
- code review, architecture analysis, and multi-plugin recurring audits
- security, performance, upgrade-safety, and operational hardening

The skill encodes repeatable Shopware-specific constraints that generic coding guidance often misses:

- exact Shopware version targeting
- official-doc precedence
- extension-point preference over brittle overrides
- DAL hot-path rules
- webhook and payment hardening
- admin ACL and permissions discipline
- storefront performance traps
- recurring P0/P1 failure modes from real plugin reviews

## Skill Architecture

The skill is split deliberately.

### Why `SKILL.md` is small

[SKILL.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/SKILL.md) is short on purpose. It does four jobs:

1. tells Codex when the skill should trigger
2. sets source priority and version discipline
3. defines the delivery workflow
4. routes Codex to the right reference file

It does not try to contain every Shopware rule. That would waste context and make the skill heavier than necessary.

### Why the references are split

Each reference file owns one cluster of concerns. That keeps the skill open-ended:

- Codex still reasons broadly
- only relevant guardrails are loaded
- the model is less likely to drown in irrelevant domain rules

This is the main mechanism that keeps the skill from limiting the model unnecessarily.

## Reference File Guide

### [01-source-hierarchy.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/01-source-hierarchy.md)

Defines authority order:

- exact-version Shopware docs first
- matching stable core behavior second
- Symfony docs third
- skill guidance after that

Also covers how to treat attached guides, forums, issues, ADRs, and local `AGENTS.md` files.

### [02-version-targeting.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/02-version-targeting.md)

Owns version-sensitive behavior:

- Shopware 6.7 default assumptions
- explicit Shopware 6.6 fallback handling
- known admin and payment-flow differences
- how to state version assumptions in output

Use this first when the target version is unclear.

### [03-implementation-workflow.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/03-implementation-workflow.md)

Defines the delivery shape:

- small-step changes
- one concern per chunk
- narrow validation after each chunk
- regression control
- conservative refactoring
- default detailed PR and merge-request description structure

This is the operational backbone for implementation work.

### [04-plugin-backend.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/04-plugin-backend.md)

Owns backend plugin architecture and common backend failure modes:

- DI and service structure
- DAL usage, criteria discipline, and N+1 avoidance
- Store API/Admin API extension rules
- caching and cache key discipline
- migrations and schema evolution
- event/subscriber design
- queue and async boundaries
- avoiding internal HTTP self-calls
- avoiding writes in read flows and render-time logic

This is the primary reference for backend implementation and backend reviews.

### [05-storefront-and-themes.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/05-storefront-and-themes.md)

Owns storefront and theme rules:

- Twig block extension patterns
- JS behavior in the storefront
- SCSS/theme considerations
- template performance
- SEO and accessibility basics
- XSS sinks, JS-context escaping, and `postMessage` origin validation
- segmented catalog behavior and cache interaction

Use this for storefront templates, plugins, CMS output, and theme work.

### [06-administration-and-apps.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/06-administration-and-apps.md)

Owns administration and app-extension guidance:

- admin module design
- permissions and ACL
- repository usage in admin UI
- app manifest and least privilege
- Vue 3, Pinia, and Vite migration considerations
- keeping privileged credentials out of storefront code
- correct separation between storefront routes and custom Admin API routes

Use this for admin UI changes, app integrations, or admin security review.

### [07-payments.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/07-payments.md)

Owns payment-specific behavior:

- payment handler flow
- redirect/finalize/validate lifecycle
- prepared payment invariants
- refunds, disputes, and PSP-side recurring behavior
- webhook handling and idempotency
- vaulting and saved methods
- operation-row design and state synchronization
- payment review sharp edges and recurring authority boundaries

Use this for any payment plugin work or payment audit.

### [08-analysis-and-reviews.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/08-analysis-and-reviews.md)

Owns review mode:

- finding-first output
- severity ordering
- evidence requirements
- triangulation review for multi-repo or multi-plugin flows
- blast-radius thinking
- large-instance review posture
- version-aware review wording

Use this when the task is analysis, code review, or architecture critique.

### [09-symfony-and-php.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/09-symfony-and-php.md)

Owns general implementation rules that matter inside Shopware:

- Symfony best practices
- PHP correctness baseline
- service orientation
- configuration discipline

Use this when a question is more framework-level than Shopware-specific.

### [10-official-docs-map.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/10-official-docs-map.md)

Index of official docs used most often by the skill:

- payment guides
- Store API route guides
- template guides
- migrations
- admin migration docs
- rate limiter
- CMS elements
- Flow Builder triggers
- custom rules

Use this when Codex needs exact official links quickly.

### [11-quality-and-operations.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/11-quality-and-operations.md)

Owns cross-cutting security and operational rules:

- secrets handling
- TLS verification
- external API resilience
- logging and observability
- recurring batch and scheduler safety
- testing and CI expectations
- store-readiness thinking
- recurring P0/P1 mistakes seen in plugin reviews

Use this for hardening, readiness, and review work.

### [12-extension-patterns.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/12-extension-patterns.md)

Owns common extension patterns:

- plugin configuration
- Flow Builder and business events
- CMS elements
- import/export hooks
- media handling
- Rule Builder
- business-specific line item modeling
- custom field merge discipline

Use this when the feature is built on top of a known Shopware extension mechanism.

### [13-context-and-commerce.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/13-context-and-commerce.md)

Owns commerce-context reasoning:

- `Context` vs `SalesChannelContext`
- multichannel behavior
- recurring total and payment-context recalculation
- customer group and pricing effects
- tax-aware logic
- B2B and role-sensitive behavior
- backend enforcement for segmented commerce behavior

Use this for logic that changes by customer, sales channel, or commerce context.

### [14-cli-and-dev-tooling.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/14-cli-and-dev-tooling.md)

Owns CLI and toolchain guidance:

- Symfony command design
- static analysis expectations
- quality checks
- safe local tooling patterns
- avoiding command orchestration via `Symfony Process` in application code

Use this for command work, tooling work, and CI-oriented implementation.

### [15-subscriptions-and-recurring-payments.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/15-subscriptions-and-recurring-payments.md)

Owns recurring and subscription-specific rules:

- subscription lifecycle mutation safety
- recurring-order correctness
- saved-payment-method ownership in recurring flows
- provider choice and recurring reference handling
- snapshot exposure in subscription or renewal flows
- cross-plugin triangulation when subscriptions and PSP plugins share one journey

Use this when renewals, subscriptions, saved methods, or multi-plugin recurring reviews are in scope.

## How The Skill Works With The Current Model

This skill is meant to improve the model, not narrow it.

### What the skill changes

It adds Shopware-specific guardrails:

- which sources matter most
- which version assumptions are dangerous
- which Shopware patterns are safe
- which recurring bugs and security issues to look for

### What the skill does not change

It does not replace the model's reasoning, planning, code reading, or implementation ability.

The current model still decides:

- how to investigate the repo
- which files matter
- which fix is best
- how broad or narrow the final solution should be
- how to explain tradeoffs

### Why this does not materially limit model power

The protection against over-constraining the model is structural:

- the trigger file is short
- references are split by concern
- Codex is instructed to load only relevant reference files
- the skill uses guidance, not rigid templates

In practice, the skill acts like a Shopware senior engineer sitting next to the model and saying:

- verify the target version
- do not trust unsafe assumptions
- do not miss common Shopware traps
- keep the change safe unless the user clearly wants a migration or redesign

### The main tradeoff

The skill does bias the model toward:

- safer changes
- more explicit version handling
- more conservative extension patterns
- stricter security and performance review

That is usually what you want for production Shopware work. If you want wider exploration, keep the skill and add one sentence telling Codex to explore more broadly before converging.

## When To Use The Skill

Use `shopware-development` by default for:

- plugin analysis
- plugin fixes
- feature development in a Shopware plugin
- payment integration work
- subscription lifecycle and renewal work
- saved-payment-method or recurring-payment ownership reviews
- storefront Twig/JS/SCSS changes
- admin module work
- Store API/Admin API extensions
- project implementation planning
- Shopware code review
- multi-plugin or multi-repo recurring/payment audits
- security and performance hardening

## When Not To Use The Skill

Skip the skill when the task is not meaningfully Shopware-specific, for example:

- pure PHP work with no Shopware context
- general Symfony questions unrelated to Shopware behavior
- generic Git, shell, or repository management work
- broad product brainstorming with no implementation angle

If the task is partially Shopware-specific, use the skill and scope the prompt accordingly.

## How To Install Locally In Codex

Copy the skill directory into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R shopware-development ~/.codex/skills/
```

If a previous version exists:

```bash
rm -rf ~/.codex/skills/shopware-development
cp -R shopware-development ~/.codex/skills/
```

Then restart Codex so the skill is reloaded.

Installed location should be:

```text
~/.codex/skills/shopware-development
```

The required file for discovery is:

```text
~/.codex/skills/shopware-development/SKILL.md
```

## How To Use It In Codex Chats

The most reliable way is to invoke it explicitly:

```text
$shopware-development review this Shopware 6.7 payment plugin for P0/P1 issues
```

```text
$shopware-development implement a Shopware 6.6-compatible Store API extension in the smallest safe chunk
```

```text
$shopware-development analyze this plugin architecture and recommend the safest fix path
```

Codex may also trigger it implicitly from the task description, but explicit invocation is more reliable and makes the context obvious.

## How To Use It In Real Shopware Projects

Recommended workflow:

1. Open the Shopware project in Codex.
2. Point Codex at the exact plugin or project path.
3. Invoke `$shopware-development`.
4. State whether the task is analysis, fix, development, refactor, payment, subscription, storefront, admin, multi-repo review, or audit.
5. If known, state the exact Shopware version or ask Codex to detect it from `composer.json` and `composer.lock`.
6. Ask for narrow validation after each chunk.

Good project prompt ingredients:

- exact business goal
- plugin or project path
- target Shopware version if known
- whether backward compatibility matters
- whether the task is a fix, review, or feature
- whether one plugin or multiple repos/plugins are part of the flow
- whether you want the safest path or broader alternatives first

## Prompt Patterns

### Strict plugin review

```text
$shopware-development review this plugin for P0/P1 security, performance, DAL, webhook, and Shopware compatibility issues. Findings first, with concrete file references.
```

### Small-step bug fix

```text
$shopware-development analyze this bug, identify the smallest safe fix, implement it, and validate only the affected flow first.
```

### Feature implementation

```text
$shopware-development implement this feature in small safe chunks, preserve extension points, and call out version-specific behavior explicitly.
```

### Payment work

```text
$shopware-development review and fix this payment integration with focus on validate/finalize flow, webhook authenticity, idempotency, refunds, and state sync.
```

### Recurring and subscription work

```text
$shopware-development review these subscription and PSP plugins together with focus on ownership, recurring references, renewal authority, saved methods, and cross-plugin contract drift.
```

### Storefront work

```text
$shopware-development implement this storefront change with attention to Twig block safety, JS escaping, cache behavior, and template performance.
```

### Architecture exploration without losing the skill

```text
$shopware-development think broadly first, compare 2-3 viable approaches, then recommend the best one. Use the skill as Shopware guardrails, not as a cap on solution space.
```

### Using the current model without over-constraining it

```text
$shopware-development analyze and fix this plugin. Keep the Shopware guardrails from the skill, but choose the best approach from the actual codebase, exact Shopware version, and official docs.
```

This last pattern is the default recommendation for most real work.

### PR description default

If you ask for a PR or merge-request description and do not specify another format, the skill should default to a detailed structure with:

- `Description`
- `Problem`
- `Why this is critical` or `Why this matters`
- `Implementation`
- `Affected plugin areas`
- `Risk and regression assessment`
- `Validation performed`
- `QA notes`

`QA notes` should read like a compact test guide, not a vague reminder.

## Versioning And Release

This repository keeps release history in the README and in annotated git tags.

### `v0.1.0`

First capability expansion release after the initial skill bootstrap.

Detailed changelog:

- Added explicit recurring/subscription and multi-plugin recurring audit support.
- Added [15-subscriptions-and-recurring-payments.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/15-subscriptions-and-recurring-payments.md) as the primary owner for subscription lifecycle safety, recurring-order correctness, saved-method ownership, provider-choice handling, snapshot exposure, and recurring-flow triangulation.
- Expanded [SKILL.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/SKILL.md) to route subscription, recurring-commerce, and multi-repo review work without turning the trigger file into a monolith.
- Strengthened [07-payments.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/07-payments.md) for PSP-side recurring boundaries, recurring authority, provider recurring references, and support-safe recurring diagnostics.
- Strengthened [08-analysis-and-reviews.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/08-analysis-and-reviews.md) with multi-repo triangulation reviews, cross-flow contract-drift handling, ownership and IDOR prominence, and richer evidence requirements.
- Strengthened [11-quality-and-operations.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/11-quality-and-operations.md) with recurring-log redaction, snapshot minimization, renewal-batch and scheduler safety, translated-label hazards, and heavy recurring-batch hydration checks.
- Strengthened [13-context-and-commerce.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/13-context-and-commerce.md) with subscription/saved-method ownership scoping and recurring total/payment-context recalculation rules.
- Retained the default detailed PR / merge-request description structure with `QA notes` as a compact test guide.
- Consolidated local learnings from the installed skill into primary owners instead of scattering repeated rules across multiple references.
- Kept the skill reference-driven and findings-first for review mode; the recurring additions expand coverage without replacing the concise review posture.

### `v0.0.1`

Initial release.

- Introduced the installable `shopware-development` skill structure.
- Split Shopware guidance across 14 focused references to keep the trigger file lean.
- Established Shopware 6.7-first behavior with explicit 6.6 fallback handling.
- Added implementation, storefront, admin, payment, review, PHP/Symfony, and operational hardening guidance.

## Future GitHub Installation

The repository is already installable by path because the skill root is correct:

```bash
install-skill-from-github.py --repo biotech-shopware/shopware-codex-skill --path shopware-development
```

The important invariant is:

- the repo root can contain anything
- the installable skill root must remain `shopware-development/`
- that directory must contain `SKILL.md`

## Maintenance Notes

When updating this repo:

- keep [SKILL.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/SKILL.md) lean
- put detailed domain rules into the correct reference file
- do not duplicate rules across reference files unless the duplication prevents a real operational mistake
- update [openai.yaml](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/agents/openai.yaml) when the skill description materially changes
- prefer adding rules only when they are repeatable, high-signal, and specific to Shopware work
- validate frontmatter after changes

If a new rule is discovered from real plugin reviews:

1. decide which reference file owns it
2. add it there once
3. avoid repeating it elsewhere
4. only update [SKILL.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/SKILL.md) if the top-level routing or trigger behavior needs to change

If the learning belongs primarily to renewals, subscriptions, saved methods, or recurring multi-plugin flows, prefer [15-subscriptions-and-recurring-payments.md](/Users/vi.khliupko/Documents/Codex/shopware-skills/shopware-development/references/15-subscriptions-and-recurring-payments.md) as the owner and leave only the shortest necessary cross-reference elsewhere.

This keeps the skill open-ended, specific, and maintainable.
