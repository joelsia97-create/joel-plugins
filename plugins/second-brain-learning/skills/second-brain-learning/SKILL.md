---
name: second-brain-learning
description: Use this skill when Sia Joel wants to learn something new and build it into his Second Brain wiki. Handles the full workflow: concept mapping → raw source collection via defuddle → batch ingest into wiki → dimension-based synthesis → output MD. Trigger phrases include: "我想了解", "帮我研究", "我想学", "second brain learning", or any learning request that should compound into the wiki.
---

# Second Brain Learning Assistant

You are executing a structured learning workflow for Sia Joel. All knowledge is ingested into the Second Brain wiki at `/Users/joelsia97/Documents/Obsidian Vault/Second Brain/`. Follow all steps in order. Do NOT skip steps. Do NOT proceed to the next step without completing the current one.

**Second Brain paths (reference throughout):**
- `SECOND_BRAIN` = `/Users/joelsia97/Documents/Obsidian Vault/Second Brain`
- Raw sources: `$SECOND_BRAIN/raw/`
- Wiki: `$SECOND_BRAIN/wiki/`
- Outputs: `$SECOND_BRAIN/wiki/outputs/`
- Index: `$SECOND_BRAIN/index.md`
- Log: `$SECOND_BRAIN/log.md`

---

## Step 1 — Learning Concept Mapping (BEFORE doing anything)

Ask Sia Joel these 6 questions:

1. What is your current level on this topic? (Beginner / Some exposure / Practitioner)
2. What's your end goal — understand the concept, apply it practically, or teach/delegate it?
3. Any specific angle you care about most? (e.g., "for agency owners", "for Malaysian market")
4. How deep do you want to go — quick overview or full deep dive?
5. **Output mode — 这份资料是给你自己看的学习笔记，还是要生成一份 AI Knowledge Handbook（给 AI agent 用来自我迭代的知识库）？**
6. **研究深度？**
   - 🟢 **Light** (~50 sources) — 快速了解 ⏱ ~20-30 min
   - 🟡 **Moderate** (50-80 sources) — 中等覆盖 ⏱ ~35-50 min
   - 🔴 **Heavy** (80-120 sources) — 深度研究 ⏱ ~55-80 min
   - 🏇 **Academic** (~150 sources) — 学术级 ⏱ ~90-120 min

**Based on Q5, set MODE:**
- **MODE: HUMAN** → narrative learning notes for Joel to read
- **MODE: AI_HANDBOOK** → structured knowledge base for AI agents to consume

**Based on Q6, set INTENSITY:**

| Intensity | Total Sources | YouTube (EN + ZH) | Articles | Dimensions | Queries/Dim | Total Queries | Output Words (HUMAN) | Output Words (AI_HANDBOOK) |
|-----------|--------------|-------------------|----------|------------|-------------|--------------|---------------------|---------------------------|
| 🟢 Light | ~50 | ~30 (20 EN + 15 ZH) | ~20 | 4–5 | 2 | 10–13 | 3,000–5,000 | 8,000–12,000 |
| 🟡 Moderate | 50–80 | ~40 (25 EN + 20 ZH) | ~30 | 5–6 | 2–3 | 15–22 | 5,000–10,000 | 15,000–20,000 |
| 🔴 Heavy | 80–120 | ~55 (35 EN + 30 ZH) | ~50 | 6–8 | 3–4 | 25–38 | 10,000–20,000 | 20,000–30,000 |
| 🏇 Academic | ~150 | ~70 (45 EN + 40 ZH) | ~70 | 7–10 | 4–5 | 38–57 | 20,000–30,000+ | 30,000–50,000+ |

Once you have the answers, output a **Learning Curve Map**:

```
LEARNING CURVE MAP — [Topic]

Goal: [What Sia Joel wants to achieve]
Starting Point: [Current knowledge level]
Output Mode: [HUMAN / AI_HANDBOOK]
Research Intensity: [🟢 LIGHT / 🟡 MODERATE / 🔴 HEAVY / 🏇 ACADEMIC]
Learning Path:
  Phase 1 — Foundation: [What to understand first]
  Phase 2 — Framework: [Core models / systems to learn]
  Phase 3 — Application: [How to apply]
  Phase 4 — Mastery: [Advanced nuances, edge cases]

Research Focus Areas:
  - [Topic angle 1]
  - [Topic angle 2]
  - [Topic angle 3]

Raw folder: raw/{topic-slug}/
Wiki output: wiki/outputs/{topic-slug}.md
```

Then ask: **"这个学习路径 OK 吗？确认了我才开始抓资料。"**
**STOP. Do NOT proceed until Sia Joel explicitly approves.**

---

## Step 2 — Setup Raw Folder

**Find or create the raw/ subfolder for this topic:**

1. List all directories inside `$SECOND_BRAIN/raw/` (excluding `assets/`):
   ```bash
   ls -d "/Users/joelsia97/Documents/Obsidian Vault/Second Brain/raw/"/*/  2>/dev/null
   ```

2. Compare the topic slug with existing folder names. If a semantically matching folder exists (e.g., learning about "LLM agents" and folder `llm/` exists), use it. Otherwise create a new one:
   ```bash
   mkdir -p "/Users/joelsia97/Documents/Obsidian Vault/Second Brain/raw/{topic-slug}/"
   ```

3. Confirm to Joel: "已找到/建立 raw/{topic-slug}/ — 准备开始抓资料"

---

## Step 3 — Research & URL Collection (run in parallel)

**A. YouTube — run BOTH in parallel via Bash:**

```bash
python3 "/Users/joelsia97/Documents/Pixel Pitch/03 Claude Code/youtube_search.py" "{topic in English}" --max {EN_MAX} --urls-only
```
```bash
python3 "/Users/joelsia97/Documents/Pixel Pitch/03 Claude Code/youtube_search.py" "{topic in Chinese}" --max {ZH_MAX} --urls-only
```

Deduplicate overlapping URLs. Keep ALL unique results.

**B. Web articles — use WebSearch in both languages:**
- Cover each phase of the Learning Curve Map
- Search in English and Chinese (Zhihu, 少数派, 36Kr, CSDN, tech blogs)
- Prioritize: practitioners, educators, authoritative sources
- Avoid low-quality or duplicate content

Collect all URLs into two lists: YouTube URLs and Article URLs.

Report to Joel: "已收集 X 个 URLs（Y YouTube + Z 文章）— 开始抓取"

---

## Step 4 — Batch Fetch with Defuddle → Save to raw/

**For every URL collected in Step 3:**

1. Use the `defuddle` skill to extract clean markdown from the URL (works for both web pages and YouTube transcripts)
2. Generate a filename: `{n:02d}-{slug}.md` where slug is derived from the page title (lowercase, hyphens)
3. Save the extracted markdown to `$SECOND_BRAIN/raw/{topic-folder}/{filename}`

**Execution rules:**
- Process URLs in batches — do not wait for all before starting
- If defuddle fails for a URL (paywall, 404, etc.), skip it and note the failure
- YouTube URLs → defuddle extracts the transcript as markdown
- Web articles → defuddle extracts the article text as clean markdown
- Prepend each saved file with a source header:
  ```markdown
  > Source: {original URL}
  > Fetched: {YYYY-MM-DD}

  ---

  {defuddle content}
  ```

After all URLs processed, report: "已抓取并保存 X 个文件到 raw/{topic-folder}/ （跳过 Y 个失败）"

---

## Step 5 — Batch Ingest into Second Brain Wiki

**Run the Second Brain INGEST operation on every file saved in Step 4.**

For each file in `raw/{topic-folder}/`:
1. Read the file
2. Write `wiki/sources/{slug}.md` — structured source summary
3. Identify all entities and concepts mentioned. Update or create their wiki pages in `wiki/entities/` and `wiki/concepts/`
4. Check for contradictions with existing wiki pages. Flag inline with `> ⚠️ Contradicts [[page]]`
5. Update `index.md` to add/update the source row and any new entity/concept rows

After all files are ingested:
- Report: "已 ingest X 个文件 → 新建 A 页、更新 B 页、发现 C 个矛盾"
- Append a single batch entry to `log.md`:
  ```markdown
  ## [{YYYY-MM-DD}] ingest | Batch: {Topic Name} ({X} sources)

  Ingested {X} sources from raw/{topic-folder}/. Created {A} new wiki pages, updated {B} existing pages. {C} contradictions flagged. Sources: {comma-separated source slugs}.
  ```

---

## Step 6 — Dimension-Based Synthesis (Query the Wiki)

Now synthesize the ingested knowledge into a structured output by querying the wiki directly.

### Step 6A — Generate Topic Dimension Map

Break the topic into **5-8 knowledge dimensions** based on what was ingested:
- Think: "If I were writing a book about [topic], what would the chapters be?"
- Each dimension should be distinct, ordered foundational → advanced
- Tailor to Sia Joel's Goal and Application Context from Step 1

Output the Dimension Map:
```
TOPIC DIMENSION MAP — [Topic]

D1: [Name] — [1-line description]
D2: [Name] — [1-line description]
...

+ Cross-cutting: Mistakes & Pitfalls
+ Cross-cutting: Contrarian / Hidden Truths
+ Synthesis: Actionable Playbook
```

### Step 6B — Query the Wiki Per Dimension

For each dimension, read the relevant wiki pages (sources/, entities/, concepts/) and synthesize answers. Ask dimension-specific questions against the wiki content:

**Per-dimension query rules:**
1. **Broad sweep:** "What do the ingested sources say about [Dimension]? Key components, data points, examples."
2. **Drill-down:** Go deeper on the most important sub-topic — specific numbers, step-by-step processes, real examples
3. **How exactly (Heavy/Academic):** Force specificity — processes, formulas, comparisons

**Cross-cutting queries (after all dimensions):**
- "Across all ingested sources, what are the most common mistakes and failure cases in [topic]? Specific incidents with numbers."
- "What are the counter-intuitive truths about [topic] that most people miss?"

**Synthesis queries (last):**
- "Given Sia Joel's goal of [Goal], what are the top 10 actionable takeaways from everything ingested? Ranked by priority."
- "Concrete step-by-step action plan — what should Sia Joel do first, second, third?"

**Query volume by INTENSITY:**
- 🟢 Light: 4–5 dims × 2 q/dim + 1 cross-cut + 1 synthesis = 10–13 total
- 🟡 Moderate: 5–6 dims × 2–3 q/dim + 2 cross-cut + 1–2 synthesis = 15–22 total
- 🔴 Heavy: 6–8 dims × 3–4 q/dim + 3 cross-cut + 2 synthesis = 25–38 total
- 🏇 Academic: 7–10 dims × 4–5 q/dim + 4 cross-cut + 3 synthesis = 38–57 total

---

## Step 7 — Generate Output MD

**CRITICAL: The MD file must be comprehensive and encyclopedic. Do NOT compress query responses into bullet points. Expand, elaborate, and preserve detail.**

### For MODE: HUMAN — save to `wiki/outputs/{topic-slug}.md`

```markdown
---
title: {Topic Name} — Learning Notes
type: output
tags: [learning, {topic-tag}]
sources: [{source-slug-1}, {source-slug-2}, ...]
updated: {YYYY-MM-DD}
---

# {Topic Name}

> **学习日期：** {YYYY-MM-DD} | **研究深度：** {INTENSITY} | **Sources ingested:** {N}

---

## Learning Curve Map

{Paste the approved Learning Curve Map}

---

## Executive Summary

{500-800 word overview. What is it, why does it matter, 3-5 most important things. Flowing prose, not bullets.}

---

## D1: {Dimension 1 Name}

### Overview
{2-3 paragraphs — what it is, why it matters}

### Key Details
{Specific numbers, data points, step-by-step processes, named examples, comparisons, tables, formulas}

### Takeaways
{3-5 specific, actionable conclusions}

---

## D2: {Dimension 2 Name}
{Same structure as D1...}

---

{...continue for ALL dimensions...}

---

## Mistakes & Pitfalls

{Specific failure cases with names, dates, numbers. Root causes. Correct approaches. Warning signs.}

---

## Hidden Truths & Contrarian Insights

{Counter-intuitive findings. Hidden logic. "Everyone thinks X but actually Y."}

---

## Actionable Playbook

{Step-by-step action plan for Sia Joel's specific goal. Each step: what to do, why, how. Resources needed.}

---

## Key Frameworks & Models

{All major frameworks consolidated. Name, visual/table/formula, when to use.}

---

## Related Wiki Pages

{Links to entities and concepts created/updated during this ingest}
- [[entities/...]] — one-line note
- [[concepts/...]] — one-line note

---

## Notable Sources

{Top 8-12 sources: title + URL + 2-3 sentence description + which dimensions it covers}
```

**Writing rules:**
1. Every specific number, case study, person name, company, date, or process detail from the wiki queries MUST appear in the output. Do not summarize away details.
2. Use prose for narrative, tables for structured data, bullets only for true lists.
3. Cross-reference dimensions: "（详见 D3: ...）"
4. No generic filler — every sentence must contain specific, useful information.

---

### For MODE: AI_HANDBOOK — save to `wiki/outputs/{topic-slug}-ai-handbook.md`

```markdown
---
title: {Topic Name} — AI Knowledge Handbook
type: output
tags: [ai-handbook, {topic-tag}]
sources: [{source-slug-1}, ...]
updated: {YYYY-MM-DD}
---

# {Topic Name} — AI Knowledge Handbook

> **Purpose:** Structured knowledge base for AI agent consumption.
> **Generated:** {YYYY-MM-DD} | **Dimensions:** {N} | **Sources:** {N}

---

## META

### Topic Definition
{1-2 sentence precise definition}

### Key Entities
| Entity | Type | Role/Relevance |
|--------|------|----------------|

### Glossary
| Term | Definition | Context |
|------|-----------|---------|

---

## KNOWLEDGE BASE

### D1: {Dimension Name}

#### Facts
- FACT: {statement}
- FACT: {statement}

#### Rules & Constraints
- RULE: IF {condition} THEN {outcome}
- THRESHOLD: {metric} must be {operator} {value}

#### Processes & Workflows
1. {Step} → {Expected outcome}

#### Decision Logic
- WHEN {situation} → USE {approach A} BECAUSE {reason}

#### Key Metrics & Data Points
| Metric | Value | Period | Notes |
|--------|-------|--------|-------|

#### Relationships to Other Dimensions
- DEPENDS ON: D{N} because {reason}

---

{...continue for ALL dimensions...}

---

## FAILURE MODES & ANTI-PATTERNS

### Failure: {Name}
- **What happened:** {incident with dates, numbers}
- **Root cause:** {why}
- **Impact:** {quantified}
- **Lesson:** {correct approach}
- **Detection signal:** {early warning}

---

## HIDDEN LOGIC & NON-OBVIOUS INSIGHTS

### Insight: {Title}
- **Common belief:** {what most think}
- **Reality:** {what is true}
- **Evidence:** {specific data/examples}
- **Implication:** {decision-making impact}

---

## ACTION PLAYBOOK

| Step | Action | Why | Resource | Dependencies |
|------|--------|-----|----------|-------------|

### Risk Checklist
- [ ] {Risk} — Mitigation: {how to avoid}

---

## REFERENCE INDEX

| Source | URL | Dimensions Covered | Quality |
|--------|-----|--------------------|---------|
```

---

## Step 8 — Update Index + Log + Report

1. Update `index.md` — add the output row to the Outputs table
2. Append to `log.md`:
   ```markdown
   ## [{YYYY-MM-DD}] output | Learning: {Topic Name}

   Generated {INTENSITY} learning output from {N} ingested sources. Output at wiki/outputs/{slug}.md. Dimensions covered: {D1, D2, ...}. Key new concepts: {list}. Key new entities: {list}.
   ```
3. Report to Joel:
   ```
   ✅ Second Brain Learning Complete: {Topic Name}

   📥 Sources ingested: {N} ({Y YouTube + Z articles)
   📄 Wiki pages created: {A}
   📝 Wiki pages updated: {B}
   ⚠️  Contradictions found: {C}
   📖 Output: wiki/outputs/{slug}.md

   Wiki is updated — future queries on this topic will draw from this knowledge.
   ```

---

## Notes

- **defuddle** extracts clean markdown from both web pages and YouTube transcripts — use it for all URL fetching
- **Never modify** `raw/` files after saving — they are immutable source documents
- If a source fails to fetch, log it but continue — don't block the whole batch
- The wiki compounds over time: knowledge from this session will connect with future sessions automatically
