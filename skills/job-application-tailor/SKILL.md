---
name: job-application-tailor
description: Use when tailoring a resume PDF and two interview-preparation Markdown documents for a specific job description, especially when ATS keyword alignment, evidence-based rewriting, anonymization, or project-code inspection is required.
---

# Job Application Tailor

## Overview

Use this skill to turn a job description, the user's existing resume, and local project code paths into exactly three targeted job-search artifacts:

1. A role-targeted resume PDF.
2. A job interview preparation Markdown document.
3. A project technical-detail interview Markdown document.

Do not change this three-artifact contract unless the user explicitly requests a subset. Ask concise follow-up questions when required inputs are missing. Do not invent work history, companies, education, dates, project ownership, skills, or metrics. When the user asks for anonymization, replace personal identity, schools, companies, project names, and exact dates with neutral descriptions.

## Required Inputs

Before producing artifacts, confirm or infer these inputs:

- Job information: target role, business direction, technical stack, responsibilities, preferred qualifications, and seniority.
- Existing resume file: PDF/DOCX/Markdown/plain text path or attached file.
- Project code paths: local repositories or specific subdirectories for each relevant project.
- Output needs: resume PDF, interview-prep Markdown, technical-detail Markdown, or any subset.
- Privacy level: original identity, partially anonymized, or fully anonymized.

If both the job description and resume are missing, ask exactly: “请粘贴职位描述和简历。” If only one is missing, ask only for that item. Treat attached files and readable file paths as supplied inputs; do not ask the user to paste content that can already be read. If a missing input only affects quality, state the assumption and continue.

## Workflow

1. Gather context.
   - Read the job description and extract the role title, required skills, preferred skills, responsibilities, tools/technologies, soft skills, industry keywords, and domain terminology.
   - Extract the existing resume content using the appropriate document/PDF skill when needed.
   - Inspect local project code paths with `rg`, `rg --files`, targeted `cat`/`sed`, package manifests, routes, state stores, key components, API files, build config, and docs.
   - Read any user-provided reference documents, such as prior interview-prep Markdown files.

2. Map experience to the job.
   - Compare every required or relevant job keyword with the resume and code evidence.
   - Classify each item as supported and strong, supported but weak, supported by adjacent experience, or unsupported.
   - Emphasize and move supported evidence forward; express adjacent experience as transferable capability; omit unsupported claims.
   - Rank projects by job relevance.
   - For each relevant project, identify matching keywords, business scenarios, technical stack, measurable impact, and transferable skills.
   - Separate true project experience from technical reserve or learning demos.

3. Produce the requested artifacts.
   - For the ATS-friendly targeted resume PDF, follow [resume-pdf.md](references/resume-pdf.md).
   - For job interview preparation Markdown, follow [job-interview-prep.md](references/job-interview-prep.md).
   - For project technical-detail Markdown, follow [technical-detail-prep.md](references/technical-detail-prep.md).

4. Verify outputs.
   - For the PDF, verify ATS structure, keyword coverage, factual support, rendering, readability, and file existence.
   - For Markdown, check the file path, heading structure, and that required sections are present.
   - Report output file paths and any assumptions or missing verification.

## Output Defaults

- Write generated Markdown files into the current workspace unless the user specifies another path.
- Write generated PDF files into the current workspace unless the user specifies another path.
- Use Chinese output when the user writes Chinese or the target job is described in Chinese.
- Prefer concise, interview-ready language over long theoretical explanations.
- Keep the resume concise, professional, naturally keyword-rich, and free of keyword stuffing.
- Keep project statements defensible from the resume and code inspection.

## Codebase Reading Guidance

For each project path, inspect at least:

- `package.json` or equivalent dependency/build files.
- Entry points, routes, major pages/components, stores/state modules, API clients, utils, and config.
- Files matching key terms from the job and project focus, such as `video`, `player`, `hybrid`, `bridge`, `share`, `feed`, `list`, `schema`, `editor`, `render`, `agent`, `stream`, `map`, `canvas`, `track`, `performance`.

Prefer evidence-based summaries. If a project is old or incomplete, distinguish implemented code from reasonable improvement ideas.

## Privacy and Truthfulness

- Do not fabricate direct experience with a technology merely because the job asks for it.
- Reframe adjacent experience honestly, e.g. “Agent 前端接入和交互工程化” instead of “独立实现 Agent 后端框架”.
- For anonymized resumes, replace exact identity with generic labels such as “大型互联网公司”, “内容社区项目”, “本硕 985”, and fuzzy dates like “多年/近期/早期”.
- Preserve important job keywords when they are genuinely supported by experience, especially recruiter-matched terms like JavaScript, TypeScript, React, Vue, Vite, webpack, H5, WebView, Hybrid, CI/CD, monitoring, and performance optimization.
