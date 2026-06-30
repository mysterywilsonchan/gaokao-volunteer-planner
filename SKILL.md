---
name: gaokao-volunteer-planner
description: Create source-backed Chinese gaokao volunteer application planning webpages and reports. Use when the user asks to plan 高考志愿/院校专业组/冲稳保梯度, compare schools and majors, verify enrollment plans/admission scores/位次/tuition, or build a visual webpage for gaokao application reference.
---

# Gaokao Volunteer Planner

Create a data-backed, visually usable gaokao application planning webpage. Optimize for decisions about 院校专业组, not only school names. Treat official/current data as mandatory whenever the user asks for latest, 2025/2026, official sources, 位次, plans, tuition, or concrete school/program data.

## Intake

Collect or infer the minimum candidate profile before 位次-based recommendations. Ask only for missing information that materially changes the plan; otherwise proceed with explicit assumptions.

Required inputs:

- 考生省份/地区, e.g. 广东考生.
- 高考年份 and batch, e.g. 2026 本科普通批.
- 选科/考试科目, e.g. 物理+化学+生物.
- 高考分数.
- 位次 if known; if missing, derive from official 一分一段表. Use 位次 as the only Chinese term for this concept; avoid alternate Chinese wording.

Strongly recommended inputs:

- 兴趣 and disliked subjects.
- 预期专业方向, e.g. 计算机、人工智能、电气、自动化、电子信息、机械.
- 学校优先 or 专业优先; acceptable tradeoff between 985/211/双一流/强双非 and major fit.
- 风险偏好: 冲稳保比例, whether to accept high-risk schools.
- 地域偏好 and forbidden regions.
- 家庭背景 relevant to decisions: tuition budget, 中外合作 acceptance, future city, family industry resources, whether family expects考公/考研/就业.
- Career plan: direct employment, 考研, 保研, 考公, 国企/电网/互联网/制造业/教师/医生等.
- Tuition/住宿/中外合作 budget ceiling.
- Whether to accept non-target majors inside the same 专业组 because of 调剂.
- Physical/medical constraints: 色盲色弱, 单色识别不全, left-handed restrictions for some medical majors, height/vision constraints where relevant.
- Special constraints: only public universities, only in-province, avoid agriculture/medicine/management, accept campus branch/independent campus.

## Ranking Strategy

Use 位次 before score when official 位次 is available. Convert score to 位次 with the province's official 一分一段表 when needed.

Build clear tiers:

- 冲: prior-year 位次 materially better than the candidate's 位次 is required; use for 985/211/双一流 or unusually strong major fit. Explain admission probability as high risk.
- 稳: prior-year 位次 near the candidate 位次 with reasonable buffer. Prefer strong major fit and acceptable group composition.
- 保: multiple layers with increasing 位次 buffer. Use at least three safety bands when possible, e.g. 保一/保二/保三 around progressively lower prior-year 位次.

For every tier, state the target 位次 window, score/位次 gap, and why a group belongs there. Do not rely on a raw score threshold if 位次 data is available.

## Data Source Priority

Browse or inspect local source files when current data is requested. Prefer sources in this order:

1. Provincial education exam authority: 投档线, 一分一段, 招生专业目录.
2. University official admissions site: annual admissions plan, professional admission scores, group code, tuition, campus, restrictions.
3. 阳光高考 or official ministry/university platform.
4. Reliable education databases only as supplemental/cross-check sources, and label them as such.
5. OCR or image/PDF parsing when official data is image/PDF only.

Never fabricate missing plan counts, professional scores, 位次, tuition, or group composition. If a field cannot be verified, mark it as 待核验, list the exact missing items, and continue searching before final delivery when the user explicitly asks to complete all missing fields.

## Required Data Fields

For each displayed school:

- School name, code, labels (985/211/双一流/强双非/中外合作/校区), province, city.
- Tier: 冲/稳/保 and optional safety sub-tier.
- Official group code/专业组号.
- Group-level plan count, minimum投档 score, minimum投档位次, source URL.
- Score gap and 位次 gap against candidate.
- Recommendation reason and cautions.

For each displayed 专业组:

- List every major in the group, including non-preferred or risky majors. Do not delete majors just because they are less attractive; group completeness is required to judge 调剂 risk.
- For each major: major name, detail/campus/remarks, professional enrollment count or admitted count, professional minimum score, professional minimum位次, tuition, source URL, and verification status.
- If the group is a大类招生 group, preserve contained majors from the official remark.
- Highlight unacceptable majors, restrictions, high tuition, campus issues, 中外合作, and second-year/third-year分流 rules.

For each major, add career planning:

- 3-5 热门就业方向.
- 3-5 推荐考研方向 using recognized一级学科 or专业学位 categories where possible.
- A short note about undergraduate employment, skills to build, or whether考研 is strongly recommended.

## Webpage Requirements

Build the actual usable webpage, not a landing page. The first screen should show candidate profile, tier navigation, filters, and summary metrics.

The page must include:

- Candidate profile: province, subject track, score, 位次, batch lines, data date.
- Tier dashboard: 冲/稳/保 counts, target 位次 windows, average 位次, score/位次 gaps.
- Search/filter controls: school, city/province, label, major direction, tier.
- Cards or tables for schools and professional groups.
- Default-expanded professional group details, or a UI that preserves layout correctly when expanded.
- Per-major rows with plan/admission/score/位次/tuition/source/career/考研 information.
- Source panel with official URLs and notes explaining which fields each source supports.
- Audit summary: number of groups, programs, verified fields, and remaining 待核验 items.
- Clear final caveat that final submission must be checked against the official provincial志愿填报辅助系统 and the latest招生专业目录.

Use a quiet data-heavy UI: dense but readable cards/tables, stable row widths, responsive layout, no decorative landing hero. Ensure mobile and desktop text does not overlap.

Desktop and mobile adaptation is mandatory:

- Design first-class layouts for desktop and phone screens, not just a shrunken desktop page.
- On desktop, use the available width for side navigation, filters, summary metrics, and dense school/program-group cards or tables.
- On mobile, collapse navigation and filters into a single-column flow; professional rows, score/位次 fields, tuition, sources, employment, and postgraduate suggestions must wrap cleanly.
- Use responsive CSS breakpoints, flexible grids, and bounded table/card widths so long school names, major names, group details, and source links do not overflow.
- Avoid hover-only interactions; all expanded details, filters, links, and source controls must work on touch screens.
- Verify at least one desktop viewport and one mobile viewport before final delivery; if browser tools are available, inspect screenshots or rendered layout rather than relying only on code review.

## Implementation Workflow

1. Inspect existing project/data if working in a repo. Do not assume prior generated data is current.
2. Normalize candidate profile and derive 位次 if needed.
3. Define 位次-based tier windows and selection rules.
4. Gather candidate school/group list from official投档 data and user preferences.
5. For every chosen group, verify complete group composition: all majors, group code, plan count, group line/位次.
6. For every major, fill professional-level admission count, minimum score, minimum位次, tuition, campus/detail, and source.
7. Add employment and考研 suggestions for every major.
8. Build or update the webpage and source/audit panels.
9. Run validation:
   - No displayed major lacks tuition.
   - No displayed major lacks professional score/位次/admission metadata unless explicitly marked and listed.
   - No displayed major lacks employment and考研 suggestions.
   - Every group has a group code and complete major list.
   - Desktop and mobile layouts both render without overlapping text, clipped controls, unusable filters, or hidden professional data.
   - Generated static/deploy files match source data.
10. If deploying or exporting, rebuild artifacts after data changes and verify the artifact contains the new data.

## Quality Bar

- Prefer fewer fully verified groups over many partially verified groups unless the user explicitly asks for broad exploration.
- Never hide 调剂-unfriendly majors.
- Separate facts from recommendations.
- Keep source attribution close to the data it supports.
- When using supplemental databases, mention whether official data was unavailable, image-only, or cross-checked.
- If the user says data is incomplete, first list all missing fields, then fill them; do not merely attach links.
