# ATS-Targeted Resume PDF Workflow

Use this reference when creating or revising a role-targeted resume PDF.

## Steps

1. Extract the original resume.
   - Use the PDF or document skill for structured extraction when the source is PDF/DOCX.
   - Preserve true work history, project ownership, education level, and core skills.

2. Analyze the job description.
   - Extract and group the role title, required skills, preferred skills, responsibilities, tools/technologies, soft skills, industry keywords, domain terminology, business direction, and seniority signals.
   - Build an internal mapping from every required or relevant keyword to resume and project evidence. This mapping is an analysis aid, not a table to include in the final resume.

3. Compare the job description with the resume.
   - **Supported and strong:** rewrite for clarity and emphasize the evidence.
   - **Supported but weak:** strengthen the wording, move it earlier when relevant, and make the impact explicit.
   - **Adjacent experience:** add a truthful sentence that explains the transferable capability without claiming direct experience.
   - **Unsupported:** omit the claim. Never infer a skill only because the job requires it.
   - Preserve factual consistency across role titles, employers, education, dates, project ownership, technologies, scope, and metrics.

4. Rewrite for fit.
   - Lead with the target role title.
   - Add a concise, job-specific professional summary using supported job-description keywords.
   - Add a `技术栈` section containing recruiter-searchable keywords supported by the user's experience.
   - Reorder experience and projects by job relevance while keeping the chronology understandable.
   - Align responsibility wording with the job description without copying sentences verbatim.
   - Use keywords naturally in evidence-bearing bullets; do not create a keyword dump.
   - Convert full-stack wording into front-end-centered wording when the target is front-end, without hiding useful backend/platform depth.

5. Add measurable impact when available.
   - Use user-provided metrics such as UV, conversion, scale, latency, bundle size, or delivery efficiency.
   - Also use metrics that are directly and reliably supported by inspected project evidence.
   - If a metric is not provided or evidenced, do not estimate or manufacture one. Use qualitative scale like “高访问量”, “多端”, “复杂业务”.

6. Apply privacy rules.
   - If anonymized, remove name, phone, email, exact school names, company names, project names, and exact dates.
   - Use labels like “本硕 985”, “大型互联网公司”, “内容社区项目”, “企业级平台项目”.

7. Generate and verify PDF.
   - Use the PDF skill for generation and rendering checks.
   - Use a clean, single-column, ATS-friendly layout with standard section headings and ordinary bullet points.
   - Do not use icons, tables, images, text boxes, charts, decorative skill bars, headers/footers containing essential content, or multi-column reading order.
   - Keep the layout concise, readable, professional, and recruiter-friendly.
   - Place the final PDF in the current workspace unless specified otherwise.

## Final Verification

Before delivery, verify:

- The target role title and genuinely supported required keywords are easy to find.
- Preferred keywords appear only when supported by resume or code evidence.
- Every strengthened statement remains defensible and consistent with the source resume.
- No unsupported technology, responsibility, ownership claim, or metric was added.
- Experience ordering highlights relevance without obscuring chronology.
- The PDF contains no icons, tables, images, text boxes, charts, or multi-column reading order.
- Text extraction preserves headings, employer/title information, dates, and bullet content in a sensible order.
- The rendered PDF has no clipping, overlap, missing glyphs, or unreadably dense sections.

## Resume Content Structure

Recommended sections:

- 求职意向
- 工作经验年限 / 个人简介
- 教育背景
- 技术栈
- 核心能力
- 工作经历
- 代表项目

## Tone

Use concise, concrete engineering language. Prefer “负责/设计/实现/优化/沉淀/推动”, avoid inflated claims, and keep keyword use natural rather than repetitive.
