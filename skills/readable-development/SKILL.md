---
name: readable-development
description: Use when writing, modifying, refactoring, or reviewing code where readability, maintainability, learning value, low cognitive load, avoiding over-engineering, simple implementation, or explaining code paths and tradeoffs matters.
---

# Readable Development

## Overview

Prioritize code that future maintainers can understand, modify, and verify.
Prefer direct, local clarity before adding abstractions.

## Core Rules

1. Optimize for maintainers.
   - Choose names, file placement, and control flow that make the change easy to read later.
   - Keep the code path short when the behavior is still small or new.
   - Treat cognitive load as a real maintenance cost.

2. Prefer direct implementation first.
   - Start with the simplest implementation that clearly shows the business flow.
   - Use existing project patterns before inventing new layers.
   - Keep the first version close to the caller unless reuse is already clear.

3. Abstract only with evidence.
   - Extract shared code when duplication is real, responsibilities are stable, or the project already has the same pattern.
   - Avoid speculative interfaces, factories, managers, generic helpers, or configuration systems.
   - Allow small duplication when it keeps behavior easier to follow.

4. Explain the code path after changes.
   - Name the entry point, core function, important data reads/writes, and error path.
   - Point to the files a maintainer should inspect first for future changes.
   - Explain any non-obvious tradeoff or abstraction.

5. Match verification to risk.
   - Use lightweight checks for small, local edits.
   - Use stronger tests for shared logic, security, concurrency, data correctness, caching, queues, migrations, and public APIs.
   - Report what was verified and what remains unverified.

## Implementation Style

Default to:

- Clear names over clever names.
- Fewer files when the behavior is small.
- Straight-line business logic where practical.
- Local helpers over broad utility packages.
- Comments that explain why, not obvious syntax.
- Small, focused changes that respect the current codebase.

Avoid by default:

- New framework-style layers for one feature.
- Interfaces with only one implementation.
- Factories, managers, registries, or rule engines without current need.
- Generic abstractions that hide simple business flow.
- Broad refactors bundled with feature work.
- Compatibility branches for contracts controlled inside the same project.

## Abstraction Test

Before introducing an abstraction, answer at least one of these with concrete evidence:

- Is the same behavior already repeated in multiple places?
- Is the responsibility stable enough to name clearly?
- Does the project already use this exact pattern nearby?
- Will the abstraction reduce a real maintenance problem now?
- Does it make testing or correctness materially easier?

If the answer is no, keep the implementation direct.

## Output Style

When reporting completed code changes, include a maintainer-oriented summary:

- What changed.
- Where the main code path starts.
- Where the core logic lives.
- What data stores, external services, or shared helpers are involved.
- What verification was run.
- Any deliberate simplicity or deferred abstraction.

Keep the explanation concise. The goal is to help the maintainer re-enter the code, not to restate every diff.
