---
name: change-planning
description: Use before implementing any non-trivial codebase change, including new features, bug fixes, refactors, API changes, database changes, UI behavior changes, import/export changes, or permission changes. First inspect the current project and data context, then present a concise implementation scheme and change list for user confirmation before editing code.
---

# Change Planning

Use this skill before making a non-trivial project change. The goal is to keep implementation deliberate: understand the current state, define the change boundary, list affected files/modules, get confirmation, then implement.

## When To Use

Use this when the user asks to:

- add a feature
- change behavior
- fix a bug that may affect multiple files
- adjust APIs, database tables, permissions, imports/exports, or UI flows
- refactor structure or split components
- continue a feature that was discussed previously

Skip only for tiny, direct tasks where the user clearly asks for an immediate command or one-line answer.

## Workflow

1. Inspect current state before proposing.
   - Read relevant code first.
   - Check current API contracts, database schema, config, docs, or existing data when they affect the decision.
   - Prefer `rg`/targeted file reads.

2. Produce a concise plan before implementation.
   Use Chinese section titles for Chinese requests. For all non-Chinese requests, use English section titles by default. For Chinese requests, include the following sections as relevant:
   - 功能逻辑
   - 后端改动
   - 前端改动
   - 数据库/初始化脚本改动
   - 文档改动
   - 验证方式
   For English requests, use:
   - Feature Logic
   - Backend Changes
   - Frontend Changes
   - Database / Init Script Changes
   - Documentation Changes
   - Verification

3. Wait for confirmation.
   - Do not implement until the user confirms, unless the user already explicitly said to implement a previously agreed plan.
   - If the user says “开始/可以/改一下/实现吧”, proceed.

4. Implement with conservative project structure.
   - Follow existing module boundaries and naming.
   - Keep files and abstractions minimal.
   - Extract shared utilities only when there is real reuse or clear necessity.
   - Do not duplicate existing helpers.
   - Do not add broad compatibility for fields/contracts controlled by this project.
   - Keep frontend and backend contracts explicit.

5. Verify before reporting completion.
   - Run focused tests/builds for touched areas.
   - Report commands and outcomes.
   - If verification cannot be run, say why.

## Project Defaults To Preserve

- Normal business queries should operate on valid data, usually `status = 1`.
- Soft delete uses `status = 0`.
- Frontend and backend are developed together, so mismatched response fields are errors, not compatibility cases.
- Avoid over-engineering: fewer files and simpler logic are preferred unless the codebase shape requires separation.
- For frontend components, split only when the current file has a real responsibility or readability problem.
- For backend business modules, prefer module-local `types.go`, `repository.go`, and service logic when it matches existing structure.

## Output Style

Keep the pre-implementation plan direct and scannable. Use a directory-style change list when useful:

```text
iot-platform-backend/
├── internal/app/example/
│   ├── types.go（改）
│   └── repository.go（改）

iot-platform-frontend/
└── src/pages/ExamplePage.tsx（改）
```

For English plans, use English file status labels:

```text
iot-platform-backend/
├── internal/app/example/
│   ├── types.go (modify)
│   └── repository.go (modify)

iot-platform-frontend/
└── src/pages/ExamplePage.tsx (modify)
```

Do not make the plan longer than needed. The plan should help the user confirm scope, not become a full design document unless requested.
