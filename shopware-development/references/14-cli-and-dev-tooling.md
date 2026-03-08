# CLI and Dev Tooling

## Console Commands

- Build commands with Symfony console conventions: clear name, description, arguments, options, and help text.
- Keep commands orchestration-only; business logic belongs in services.
- Support `--dry-run`, bounded execution, and confirmation prompts for expensive operations.
- Use `SymfonyStyle` and progress output for long-running work.
- Chunk large operations instead of loading everything into memory.
- Do not spawn internal sync or maintenance work via `Symfony Process` from controllers or services just to run your own commands. Use Messenger, scheduled tasks, or invoke the service directly.

Example:
- a sync command
  Accept IDs optionally, add `--limit` and `--since`, confirm large runs, and report a compact success/failure summary.

## Static Analysis and Quality Gates

- Use the repo's existing quality toolchain first.
- If the project lacks one, prefer PHPStan with Shopware and Symfony-aware configuration, consistent code style tooling, and targeted tests.
- Keep generated resources, migrations, and build output out of the most aggressive analysis paths when that matches the team's setup.

## Local Development and Debugging

- Prefer reproducible local environments over machine-specific assumptions.
- Use targeted profiling and debugging for hot paths instead of leaving verbose debug tooling enabled by default.
- Keep build artifacts, coverage reports, and local editor state out of distributable extension packages.

## CI Behavior

- Run the narrowest checks that prove the change.
- For extension packages, include packaging and validation checks before release when the project uses them.
- Keep quality gates incremental so teams can improve rather than disable tooling wholesale.
- Add sanity checks for duplicate handler registration, container compilation, and command wiring when background jobs or scheduled tasks are involved.
