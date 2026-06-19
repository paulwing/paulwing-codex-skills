# paulwing-codex-skills

[English](README.md) | [中文](README.zh-CN.md)

Personal Codex skills maintained by Paul Wing. This is not an official OpenAI
repository.

These skills capture practical agent workflows I use while building software.
The current focus is simple: make Codex pause before non-trivial code changes,
inspect the project first, show a clear change plan, and only then implement.

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

## Why This Exists

AI coding agents are fast, but speed can make small assumptions expensive. This
skill adds a lightweight planning gate before meaningful changes:

1. Understand the current state.
2. Define the change boundary.
3. Show the affected files and behavior.
4. Get confirmation.
5. Implement and verify.

The goal is not to create long design documents. The goal is a short, readable
plan that lets the user confirm scope before Codex starts editing.

## Example Planning Style

For a feature or behavior change, Codex should produce something close to this:

```text
功能逻辑
- Add device tree filtering by product model and online status.
- Keep existing status = 1 filtering for normal business queries.

后端改动
- internal/app/device/types.go（改）: add request fields and response shape.
- internal/app/device/repository.go（改）: extend query conditions.
- internal/app/device/service.go（改）: validate filters and preserve existing defaults.

前端改动
- src/pages/device/DeviceTreePage.tsx（改）: add filter controls.
- src/components/device/DeviceTreeFilters.tsx（增）: isolate filter UI if the page is already crowded.
- src/api/device.ts（改）: align request parameters with backend contract.

数据库/初始化脚本改动
- No schema change.

验证方式
- Run focused backend tests for device queries.
- Run frontend typecheck/build for touched pages.
```

That is the core behavior this skill tries to preserve: direct, scoped,
confirmable planning before implementation.

## Repository Layout

```text
skills/
`-- change-planning/
    `-- SKILL.md
```

## Install In Codex

Install from GitHub with Codex's skill installer:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/change-planning
```

Restart Codex after installing so it can discover the new skill.

## Update An Existing Install

If you already installed the skill and want the latest version:

```bash
rm -rf ~/.codex/skills/change-planning
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/change-planning
```

Restart Codex after updating.

## Compatibility

This repository is optimized for OpenAI Codex. The skill itself uses the standard
`SKILL.md` folder format, so it may also be portable to other agent tools that
support Agent Skills, but Codex is the primary target.
