# Project Technical Detail Interview Markdown Workflow

Use this reference when generating a general technical-detail interview document based on project code paths. This document is not tied to only one job; it captures reusable project details.

## Required Structure

Include a table of contents and these sections:

1. 说明
2. One section per project
3. 整体面试定位

For each project, include:

- Code path.
- One-sentence positioning.
- Interview talk track.
- Technical highlights.
- Common follow-up answers.
- Implemented facts vs improvement ideas.
- Transferable capabilities.

## Code Inspection Checklist

For each codebase, inspect:

- Dependency/build files.
- Entry files and routes.
- Major pages/components.
- State/store modules.
- API clients and request wrappers.
- Utilities for hybrid, tracking, performance, rendering, schema, or streaming.
- Config and environment-specific code.

Use targeted searches for terms relevant to the project domain.

## Truth Boundary

Clearly distinguish:

- Implemented in code.
- Reasonable inference from code.
- Improvement idea for future refactor.
- Personal technical reserve, not production experience.

This is especially important for AI Agent, RL, WebRTC, HLS, payment, recommendation, or backend-heavy claims.

## Output Style

Write for interview preparation, not documentation for maintainers. Prefer practical explanations the user can speak aloud.
