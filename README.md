# paulwing-codex-skills

[English](README.md) | [中文](README.zh-CN.md)

Personal Codex skills maintained by Paul Wing. This is not an official OpenAI
repository.

These skills capture practical agent workflows I use while building software
and investigating technical problems. The current focus is simple: keep Codex
deliberate before non-trivial changes, keep AI-written code readable enough for
real maintainers to own, and turn repeatable investigation workflows into
clear, evidence-aware outputs.

## Available Skills

### `change-planning`

`change-planning` is a planning-first workflow for Codex.

Use it when a request may touch more than one file, contract, module, database
table, permission rule, or UI flow. The skill guides Codex to:

- inspect the current codebase before proposing changes
- identify the affected backend, frontend, database, documentation, and test areas
- present a concise implementation scheme for confirmation
- wait before editing code unless the user has already approved the plan
- verify the touched area before reporting completion

It is designed for everyday engineering work where a little planning avoids
messy edits, accidental API drift, and broad refactors that were never asked for.

### `readable-development`

`readable-development` is a readability-first workflow for Codex.

Use it when writing, modifying, refactoring, or reviewing code where the user
cares about maintainability, learning value, low cognitive load, or avoiding
over-engineering. The skill guides Codex to:

- optimize for future maintainers
- prefer direct implementation before abstraction
- introduce abstractions only when there is concrete evidence
- explain the code path after changes
- match verification effort to the risk of the change

It is designed for users who want AI assistance without letting the codebase
become harder to understand than the problem it solves.

### `tron-usdt-flow-tracing`

`tron-usdt-flow-tracing` is a public-chain investigation workflow for TRON
USDT/TRC20 fund flows.

Use it when a user provides a TRON transaction hash, Tronscan URL, or address
and wants to understand where USDT moved next. The skill guides Codex to:

- verify the starting transaction and token details
- follow downstream TRC20 transfers through Tronscan data
- preserve transaction hashes, timestamps, amounts, addresses, and public labels
- classify direct transfers, mixed-account downstream flows, and service endpoints
- produce evidence tables, Mermaid diagrams, and exchange/law-enforcement action notes

It is designed for scam-loss triage, compliance-style evidence organization, and
repeatable blockchain investigation reports. It does not promise recovery,
request wallet secrets, or claim identity beyond public chain data and explorer
labels.

## Why This Exists

AI coding agents are fast, but speed can make small assumptions expensive. This
repository adds lightweight workflow guardrails before and during meaningful
changes:

1. Understand the current state.
2. Define the change boundary.
3. Show the affected files and behavior.
4. Get confirmation.
5. Implement with maintainers in mind.
6. Verify and explain the resulting code path.

The goal is not to create long design documents. The goal is a short, readable
plan and a simple implementation style that lets the user keep ownership of the
code.

## Invocation Guidance

For the most predictable behavior, invoke skills explicitly when you care about
which workflow Codex follows. In Codex CLI or IDE, use `/skills` to pick a skill
or mention the skill directly in the prompt, such as `$change-planning`,
`$readable-development`, or `$tron-usdt-flow-tracing`.

Recommended usage:

- Keep `change-planning` available for implicit invocation because it acts like
  a safety gate before non-trivial code changes.
- Prefer explicit invocation for `readable-development` when readability,
  maintainability, or avoiding over-engineering is the main concern.
- Use both when needed: `$change-planning $readable-development` to plan first,
  then implement with a readability-first style after confirmation.
- Use `$tron-usdt-flow-tracing` explicitly for crypto fund-flow tracing because
  investigation tasks should preserve careful wording and evidence boundaries.
- If only one workflow should apply, say so directly in the prompt, for example:
  `Use only $change-planning for this request.`

`readable-development` is configured for explicit invocation only:

```yaml
policy:
  allow_implicit_invocation: false
```

This keeps general coding requests from picking up the readability workflow
unless the user asks for it with `/skills` or `$readable-development`.

`tron-usdt-flow-tracing` should also be invoked explicitly. This avoids
accidentally treating ordinary crypto questions as scam investigations.

## Example Output Styles

### Planning Before Changes

For a feature or behavior change, `change-planning` should guide Codex to produce
something close to this:

```text
Feature Logic
- Add device tree filtering by product model and online status.
- Keep existing status = 1 filtering for normal business queries.

Backend Changes
- internal/app/device/types.go (modify): add request fields and response shape.
- internal/app/device/repository.go (modify): extend query conditions.
- internal/app/device/service.go (modify): validate filters and preserve existing defaults.

Frontend Changes
- src/pages/device/DeviceTreePage.tsx (modify): add filter controls.
- src/components/device/DeviceTreeFilters.tsx (new): isolate filter UI if the page is already crowded.
- src/api/device.ts (modify): align request parameters with backend contract.

Database / Init Script Changes
- No schema change.

Verification
- Run focused backend tests for device queries.
- Run frontend typecheck/build for touched pages.
```

That is the core behavior `change-planning` tries to preserve: direct, scoped,
confirmable planning before implementation.

### Readable Development Summary

For a completed non-trivial code change, `readable-development` should guide Codex
to report the code path in a maintainer-oriented way:

```text
Changed the device tree filter path to keep the query logic direct and local.

The request enters through src/pages/device/DeviceTreePage.tsx, which passes the
selected filters to src/api/device.ts. Backend validation happens in
internal/app/device/service.go, and the final status/model conditions are built
in internal/app/device/repository.go.

No new abstraction was added because the behavior is still specific to this
page and query. Verification: ran focused backend device tests and frontend
typecheck.
```

That is the core behavior `readable-development` tries to preserve: simple code
with enough explanation for the next maintainer to find the important path.

### TRON USDT Flow Report

For a TRON USDT tracing request, `tron-usdt-flow-tracing` should guide Codex to
produce a concise evidence-first report:

```text
Summary
The initial TRON USDT transfer is confirmed. The funds moved from Binance-Hot 11
to the reported recipient, then through active intermediate addresses. The most
actionable endpoint identified is OKX Hot Wallet 8.

Key Flow
| Step | Time | Amount | From | To | Tx | Confidence |
|---|---:|---:|---|---|---|---|

Mermaid
flowchart LR
    A["Binance-Hot 11"] --> B["Reported recipient"]
    B --> C["Intermediate address"]
    C -.-> D["Mixed downstream flow"]
    D --> E["OKX Hot Wallet 8"]

Recommended Actions
Preserve the exchange withdrawal record, file the listed hashes with law
enforcement, and contact the exchange compliance channel. Do not share seed
phrases or pay recovery scammers.
```

That is the core behavior `tron-usdt-flow-tracing` tries to preserve: useful
fund-flow evidence without overstating what public chain data can prove.

## Repository Layout

```text
skills/
|-- change-planning/
|   |-- agents/
|   |   `-- openai.yaml
|   `-- SKILL.md
|-- readable-development/
|   |-- agents/
|   |   `-- openai.yaml
|   `-- SKILL.md
`-- tron-usdt-flow-tracing/
    |-- agents/
    |   `-- openai.yaml
    |-- assets/
    |   `-- mermaid-template.mmd
    |-- references/
    |   |-- evidence-standards.md
    |   |-- report-template.md
    |   `-- tronscan-workflow.md
    `-- SKILL.md
```

## Install In Codex

Install a skill from GitHub with Codex's skill installer:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/change-planning
```

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/readable-development
```

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/tron-usdt-flow-tracing
```

Restart Codex after installing so it can discover the new skill.

## Update An Existing Install

If you already installed a skill and want the latest version:

```bash
rm -rf ~/.codex/skills/change-planning
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/change-planning
```

```bash
rm -rf ~/.codex/skills/readable-development
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/readable-development
```

```bash
rm -rf ~/.codex/skills/tron-usdt-flow-tracing
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/tron-usdt-flow-tracing
```

Restart Codex after updating.

## Compatibility

This repository is optimized for OpenAI Codex. The skill itself uses the standard
`SKILL.md` folder format, so it may also be portable to other agent tools that
support Agent Skills, but Codex is the primary target.
