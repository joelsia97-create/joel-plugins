---
name: notebooklm-learning
description: Use this skill when Sia Joel wants to learn something new and requests a NotebookLM notebook to be built for it. Handles the full workflow: learning concept mapping → notebook creation → YouTube + web research → source loading → self-learning → MD output. Trigger phrases include: "我想了解", "帮我研究", "我想学", "build me a notebook", "create a notebook for", or when user confirms "yes" after being asked about creating a NotebookLM notebook.
---

# NotebookLM Learning Assistant

You are executing a structured learning workflow for Sia Joel. Follow all 6 steps in order. Do NOT skip steps. Do NOT proceed to the next step without completing the current one.

---

## Step 1 — Learning Concept Mapping (BEFORE creating anything)

First, ask Sia Joel these 5 questions to understand his learning needs:

1. What is your current level on this topic? (Beginner / Some exposure / Practitioner)
2. What's your end goal — understand the concept, apply it practically, or teach/delegate it?
3. Any specific angle you care about most? (e.g., "for agency owners", "for Meta Ads", "for Malaysian market")
4. How deep do you want to go — quick overview or full deep dive?
5. **Output mode — 这份资料是给你自己看的学习笔记，还是要生成一份 AI Knowledge Handbook（给 AI agent 用来自我迭代的知识库）？**

**Based on Q5, set the MODE for the entire workflow:**
- **MODE: HUMAN** → Normal learning notes (for Joel to read)
- **MODE: AI_HANDBOOK** → AI-optimized knowledge base (for AI agents to consume)

**This MODE affects Steps 3, 5, and 6. Follow the mode-specific instructions in each step.**

Once you have the answers, output a **Learning Curve Map** in this exact format:

```
LEARNING CURVE MAP — [Topic]

Goal: [What Sia Joel wants to achieve]
Starting Point: [Current knowledge level]
Output Mode: [HUMAN / AI_HANDBOOK]
Learning Path:
  Phase 1 — Foundation: [What to understand first]
  Phase 2 — Framework: [Core models / systems to learn]
  Phase 3 — Application: [How to apply in Overbooked / Track A / Track B context]
  Phase 4 — Mastery: [Advanced nuances, edge cases, expert-level insights]

Research Focus Areas:
  - [Topic angle 1]
  - [Topic angle 2]
  - [Topic angle 3]

Notebook Name: [Claude] {Refined Topic Name}
```

Then ask: **"这个学习路径 OK 吗？确认了我才去建 Notebook。"**
**STOP. Do NOT proceed until Sia Joel explicitly approves.**

---

## Step 2 — Create Notebook

Once approved, create the NotebookLM notebook:
- Tool: `notebook_create`
- Name: `[Claude] {Topic Name}` — e.g., `[Claude] Email Marketing Strategy`
- Confirm the notebook ID before proceeding to Step 3

---

## Step 3 — Research & Source Collection (run in parallel)

**A. YouTube (primary) — run BOTH searches in parallel via Bash:**

English query:
```bash
python3 "/Users/joelsia97/Documents/Pixel Pitch/03 Claude Code/youtube_search.py" "{topic in English}" --max 15 --urls-only
```

Chinese/Mandarin query:
```bash
python3 "/Users/joelsia97/Documents/Pixel Pitch/03 Claude Code/youtube_search.py" "{topic in Chinese, e.g. Claude Code 教程}" --max 15 --urls-only
```

- Deduplicate any overlapping URLs between the two results
- Target: 20–25 unique YouTube URLs total (EN + ZH combined)
- Use ALL unique URLs returned

**B. Web articles (secondary) — use WebSearch in both languages:**
- Run 2–3 English searches covering each phase of the Learning Curve Map
- Run 1–2 Chinese searches (e.g., on Zhihu, CSDN, 少数派, 36Kr, or Chinese tech blogs)
- Find 10–15 high-quality articles, expert blogs, case studies, or research papers total
- Prioritise: authoritative sources, practitioners, educators with proven track records
- Avoid low-quality or duplicate sources

**Target total: 30–40 sources (20–25 YouTube EN+ZH + 10–15 articles EN+ZH)**

**AI_HANDBOOK mode adjustments for Step 3:**
- Prioritize structured, authoritative sources over casual/opinion content
- Prefer: official documentation, academic papers, industry reports, expert practitioner guides, technical deep dives
- Deprioritize: vlog-style YouTube videos, opinion pieces, listicles
- Add extra search queries targeting: frameworks, taxonomies, decision trees, edge cases, failure modes, best practices
- Target 35-50 sources (more volume = more signal for the AI knowledge base)

---

## Step 4 — Add All Sources to Notebook

- Add every URL to the notebook using `notebook_add_url` (one call per URL)
- Run YouTube URLs and article URLs in batches of 5 in parallel where possible
- After all URLs are added, confirm total count to Sia Joel: "已加入 X 个 sources（Y YouTube + Z 文章）"

---

## Step 5 — Self-Learn & Synthesize (Dimension-Based Deep Query System)

The quality of the final output is directly proportional to how many angles you explored and how specific your questions were. Generic questions = thin output. Dimension-specific deep questions = rich, encyclopedic output.

### Step 5A — Generate Topic Dimension Map

BEFORE querying, analyze the topic and break it into **5-8 knowledge dimensions**. Each dimension is a major pillar of the topic that deserves its own dedicated section in the final output.

**How to generate dimensions:**
- Think: "If I were writing a book about [topic], what would the chapter titles be?"
- Each dimension should be distinct — no overlap
- Order them from foundational → advanced
- Tailor dimensions to Sia Joel's Goal and Application Context from Step 1

**Output the Dimension Map before querying:**
```
TOPIC DIMENSION MAP — [Topic]

D1: [Dimension Name] — [1-line description of what this covers]
D2: [Dimension Name] — [1-line description]
D3: [Dimension Name] — [1-line description]
D4: [Dimension Name] — [1-line description]
D5: [Dimension Name] — [1-line description]
D6: [Dimension Name] — [1-line description]
D7: [Dimension Name] — [1-line description] (if applicable)
D8: [Dimension Name] — [1-line description] (if applicable)

+ Cross-cutting: Mistakes & Pitfalls
+ Cross-cutting: Contrarian / Hidden Truths
+ Synthesis: Actionable Playbook
```

**Example — Topic: "如何经营海底捞":**
```
D1: 商业模式与收入结构 — 怎么赚钱、利润来源、收入占比
D2: 人才管理与师徒制 — 薪酬设计、晋升机制、激励体系
D3: 供应链体系 — 颐海国际、蜀海、采购加工配送流程
D4: 服务创新与客户体验 — 具体服务案例、创新机制、授权体系
D5: 加盟模式与扩张策略 — 加盟条件、流程、资金、管控方式
D6: 选址与成本控制 — 选址公式、租金策略、成本结构
D7: 竞争格局与行业趋势 — 主要竞品、优劣势、未来机遇
D8: 失败教训与危机管理 — 关店潮、KPI僵化、食品安全
```

### Step 5B — Run Dimension-Based Queries

For EACH dimension, run **2-3 targeted queries**. Then run cross-cutting and synthesis queries at the end.

**Per-dimension query design rules:**
1. **First query = broad sweep:** "详细解释 [Dimension]。具体是怎么运作的？有哪些关键组成部分？请给出具体的数据、案例和细节。"
2. **Second query = drill-down:** Pick the most important sub-topic within that dimension and go deeper. Ask for specific numbers, step-by-step processes, real examples, or comparisons.
3. **Third query (if deep dive) = "how exactly":** Force specificity — "具体的流程/步骤/公式是什么？给我可以直接执行的细节。"

**Query phrasing rules — CRITICAL:**
- NEVER ask vague questions like "tell me about X" or "what are the basics of X"
- ALWAYS ask for: **具体数据、具体案例、具体流程、具体数字、具体对比**
- Stack multiple sub-questions in one query to force comprehensive answers
- Use phrases like: "请详细说明"、"具体是怎么运作的"、"给出数字和案例"、"步骤是什么"、"和竞品比有什么区别"

**Cross-cutting queries (after all dimensions):**
- "在 [topic] 中，大多数人会犯的最严重的错误是什么？有哪些失败案例？从中能学到什么教训？请给出具体事件和数据。"
- "关于 [topic]，有哪些反直觉的真相或外行人不知道的隐藏逻辑？那些表面上看起来是 A 但实际上是 B 的东西是什么？"

**Synthesis queries (last 1-2):**
- "综合 Notebook 所有内容，如果我是 [Sia Joel's context] 要 [Goal]，最重要的 10 个 actionable takeaways 是什么？请按优先级排序。"
- "如果我明天就要开始 [Goal]，第一步到第五步分别应该做什么？请给出具体、可执行的行动计划。"

### Query volume targets:
| Depth Level | Dimensions | Queries/Dim | Cross-cut | Synthesis | Total |
|-------------|-----------|-------------|-----------|-----------|-------|
| Quick overview | 5-6 | 2 | 2 | 1 | 13-15 |
| Full deep dive | 6-8 | 3 | 3 | 2 | 23-29 |

**Execution rules:**
- Run ALL queries in sequence using `notebook_query` with conversation threading
- Do NOT skip any dimension — each one becomes a chapter in the final output
- After each query, mentally tag which dimension it feeds into
- If a query response is thin or generic, follow up with a more specific drill-down question on the same dimension

**AI_HANDBOOK mode adjustments for Step 5:**

When MODE = AI_HANDBOOK, the query strategy shifts from "help me understand" to "extract structured, reusable knowledge for an AI system."

**Query design differences:**
- Instead of "详细解释 X", ask: "列出关于 X 的所有关键事实、规则、数字、流程和决策逻辑。用结构化的方式呈现，包括：定义、组成部分、运作机制、关键数字、决策条件、边界条件、常见例外。"
- Instead of narrative questions, ask for: **taxonomies, decision trees, if-then rules, thresholds, formulas, checklists**
- Add per-dimension queries specifically targeting:
  - "关于 [Dimension]，有哪些硬性的规则、门槛、数字或条件是绝对不能搞错的？"
  - "关于 [Dimension]，列出所有关键术语及其精确定义。"
  - "关于 [Dimension]，什么情况下应该用方案 A vs 方案 B？决策条件是什么？"

**Query volume for AI_HANDBOOK:**
| Depth Level | Dimensions | Queries/Dim | Cross-cut | Synthesis | Total |
|-------------|-----------|-------------|-----------|-----------|-------|
| AI Handbook | 6-8 | 3-4 | 3 | 2 | 27-37 |

AI Handbook mode always runs at maximum depth regardless of what Joel answered for Q4.

---

## Step 6 — Save MD File + Report

**Determine the next sequence number:**
```bash
ls "/Users/joelsia97/Documents/Pixel Pitch/05 Notebooklm/" | sort | tail -1
```
Use the next available 2-digit zero-padded number (e.g., if last is `02`, next is `03`).

**Save file at:** `05 Notebooklm/{NN} {Topic Name}.md`

**CRITICAL: The MD file must be comprehensive and encyclopedic. Target 5,000-10,000+ words for quick overview, 10,000-20,000+ words for deep dive. You have rich query responses from Step 5 — do NOT compress them into bullet points. Expand, elaborate, and preserve the detail.**

**File structure:**
```markdown
# {NN} {Topic Name}

> **NotebookLM:** [[Claude] {Topic Name}] | **学习日期：** {YYYY-MM-DD}

---

## Learning Curve Map

[Paste the approved Learning Curve Map here]

---

## Executive Summary

[500-800 word overview of the entire topic. What is it, why does it matter, and what are the 3-5 most important things to know. Written as flowing prose, not bullet points.]

---

## D1: [Dimension 1 Name]

### Overview
[2-3 paragraph explanation of this dimension — what it is, why it matters]

### Key Details
[The meat of the content. Include:
- Specific numbers, data points, percentages
- Step-by-step processes or workflows
- Named examples, case studies, real companies/people
- Comparisons with competitors or alternatives
- Tables where data is structured
- Formulas, models, or frameworks specific to this dimension]

### Takeaways
[3-5 specific, actionable conclusions from this dimension]

---

## D2: [Dimension 2 Name]
[Same structure as D1...]

---

## D3: [Dimension 3 Name]
[Same structure...]

---

[...continue for ALL dimensions from the Topic Dimension Map...]

---

## Mistakes & Pitfalls

[Dedicated section. Not just "common mistakes" but:
- Specific failure cases with names, dates, numbers
- Root causes of each failure
- What the correct approach should have been
- Warning signs to watch for]

---

## Hidden Truths & Contrarian Insights

[The "things most people don't know" section:
- Counter-intuitive findings
- Hidden business logic that surface-level analysis misses
- Expert-level nuances
- "Everyone thinks X but actually Y"]

---

## Actionable Playbook

[Step-by-step action plan tailored to Sia Joel's Goal from Step 1:
- Numbered steps (Step 1, Step 2, ... Step N)
- Each step has: what to do, why, and how
- Include specific resources, tools, or contacts needed
- Include estimated costs, timelines, or prerequisites where relevant]

---

## Key Frameworks & Models

[All major frameworks mentioned across dimensions, consolidated here as quick reference:
- Framework name
- Visual representation (table, formula, or structured list)
- When to use it]

---

## Notable Sources

[Top 8-12 sources worth reading/watching directly:
- Title + URL
- 2-3 sentence description of what makes this source valuable
- Which dimensions it covers]
```

**Writing rules for HUMAN mode MD file:**
1. **Preserve richness:** Every specific number, case study, person name, company name, date, or process detail from the query responses MUST appear in the MD file. Do not summarize away the details.
2. **Use prose + structure:** Each dimension section should read like a well-written article, not a bullet-point dump. Use paragraphs for narrative, tables for structured data, and bullet points only for lists.
3. **Include all data:** If a query response mentioned "revenue was 414.53 billion RMB" or "rent cost is 4% vs industry average 11-12%", these numbers MUST appear in the output.
4. **Cross-reference dimensions:** When content in one dimension connects to another, add a brief note like "（详见 D3: 供应链体系）".
5. **No generic filler:** Every sentence should contain specific, useful information. Delete any sentence that could apply to any topic.

---

### Step 6-ALT — AI Knowledge Handbook Output (AI_HANDBOOK mode only)

**If MODE = AI_HANDBOOK, skip the normal Step 6 format above and use this instead.**

**Save location:** `05 Notebooklm/AI Handbooks/{Topic Name} — AI Handbook.md`
(Create the `AI Handbooks` subfolder if it doesn't exist.)

**Target: 15,000-30,000+ words. This is a reference document for AI agents — completeness and precision are everything. Zero noise, maximum signal.**

**File structure:**
```markdown
# {Topic Name} — AI Knowledge Handbook

> **Purpose:** Structured knowledge base for AI agent consumption. Optimized for retrieval, reasoning, and decision-making.
> **Source:** NotebookLM notebook "[Claude] {Topic Name}" | **Generated:** {YYYY-MM-DD}
> **Dimensions covered:** {N} dimensions, {N} queries

---

## META

### Topic Definition
[1-2 sentence precise definition of the topic scope]

### Key Entities
[Table of all important entities (companies, people, products, systems) mentioned in this handbook with their roles]

| Entity | Type | Role/Relevance |
|--------|------|----------------|
| ... | Person/Company/System/Product | ... |

### Glossary
[Table of all domain-specific terms with precise definitions. AI agents will use this to resolve ambiguity.]

| Term | Definition | Context |
|------|-----------|---------|
| ... | ... | ... |

---

## KNOWLEDGE BASE

### D1: {Dimension Name}

#### Facts
[Bulleted list of verified facts. Each fact = one atomic, self-contained statement. Include numbers, dates, names.]
- FACT: [statement] (Source: [which query/source])
- FACT: [statement]
- ...

#### Rules & Constraints
[If-then rules, thresholds, hard constraints that govern this dimension]
- RULE: IF [condition] THEN [outcome]
- THRESHOLD: [metric] must be [operator] [value]
- CONSTRAINT: [what is NOT allowed and why]

#### Processes & Workflows
[Step-by-step sequences. Numbered, unambiguous, executable.]
1. [Step] → [Expected outcome]
2. [Step] → [Expected outcome]
...

#### Decision Logic
[Decision trees, comparison matrices, "when to use A vs B"]
- WHEN [situation] → USE [approach A] BECAUSE [reason]
- WHEN [situation] → USE [approach B] BECAUSE [reason]

#### Key Metrics & Data Points
[All quantitative data extracted, in table format]

| Metric | Value | Year/Period | Notes |
|--------|-------|-------------|-------|
| ... | ... | ... | ... |

#### Relationships to Other Dimensions
[How this dimension connects to others — explicit cross-references]
- DEPENDS ON: D{N} because [reason]
- ENABLES: D{N} because [reason]

---

### D2: {Dimension Name}
[Same structure as D1...]

---

[...continue for ALL dimensions...]

---

## FAILURE MODES & ANTI-PATTERNS

[Each failure mode as a structured entry:]

### Failure: {Name}
- **What happened:** [Specific incident with dates, numbers]
- **Root cause:** [Why it happened]
- **Impact:** [Quantified damage]
- **Lesson:** [What the correct approach should have been]
- **Detection signal:** [How to spot this going wrong early]

---

## HIDDEN LOGIC & NON-OBVIOUS INSIGHTS

[Each insight as a structured entry:]

### Insight: {Title}
- **Common belief:** [What most people think]
- **Reality:** [What is actually true]
- **Evidence:** [Specific data or examples that prove this]
- **Implication:** [What this means for decision-making]

---

## ACTION PLAYBOOK

### Prerequisites
[What must be true before starting]

### Step-by-Step Plan
| Step | Action | Why | Cost/Resource | Duration | Dependencies |
|------|--------|-----|---------------|----------|-------------|
| 1 | ... | ... | ... | ... | None |
| 2 | ... | ... | ... | ... | Step 1 |
...

### Risk Checklist
- [ ] [Risk 1] — Mitigation: [how to avoid]
- [ ] [Risk 2] — Mitigation: [how to avoid]

---

## REFERENCE INDEX

### Sources by Dimension
[Which sources cover which dimensions — for AI to trace back claims]

| Source | URL | Dimensions Covered | Quality |
|--------|-----|-------------------|---------|
| ... | ... | D1, D3, D5 | High/Medium |

### Data Freshness
[When the data was collected, known gaps, expiration dates for time-sensitive info]
```

**AI Handbook writing rules:**
1. **Atomic facts:** Break down every piece of knowledge into the smallest self-contained unit. One fact per bullet. No compound sentences hiding two facts.
2. **No narrative prose:** AI doesn't need storytelling. Use tables, lists, rules, and structured entries. Every word must carry information.
3. **Explicit over implicit:** Never assume the AI reader knows context. State relationships, causality, and conditions explicitly.
4. **Numbers always:** If a number exists in the source material, it MUST appear in the handbook. Vague statements like "significant" or "many" are banned — replace with actual data.
5. **Decision-ready:** Frame knowledge as actionable rules whenever possible. "IF X THEN Y" is better than "X tends to lead to Y."
6. **No duplication:** Each fact appears exactly once, in the most relevant dimension. Use cross-references ("See D3") instead of repeating.
7. **Source traceability:** Tag key facts with which dimension/query they came from, so the AI can assess confidence.

**After saving, skip Step 6.5 and Step 7 (no HTML/deploy for AI Handbooks). Instead, just confirm:**
> "AI Knowledge Handbook 已保存到 `05 Notebooklm/AI Handbooks/{Topic Name} — AI Handbook.md`"

---

## Step 6.5 — Ask Before Generating Website

After saving the MD file, **STOP and ask Joel:**

> "MD 文件已保存到 `05 Notebooklm/{NN} {Topic Name}.md` ✅
>
> 要帮你把这份学习笔记生成 HTML 页面并部署到网站吗？"

**STOP. Do NOT proceed to Step 7 unless Joel explicitly says yes.**
- If Joel says **yes / 要 / deploy** → proceed to Step 7
- If Joel says **no / 不用 / 只要 MD** → skip Step 7, send a brief Slack DM with the MD file path only

---

## Step 7 — Convert to HTML + Deploy + Notify Joel

**Only run this step if Joel confirmed yes in Step 6.5.**

After confirmation, do these three things:

### 7A — Generate Learning HTML Page

Create a self-contained HTML file at `public/learn-{topic-slug}.html`.

**Design standard — MUST follow:**
- Font: `Inter` via Google Fonts (`https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap`)
- Background: outer `#f0f0f0`, inner page `#ffffff`
- Page container: `max-width: 860px`, `margin: 24px auto`, `padding: 40px 48px`, `box-shadow: 0 4px 24px rgba(0,0,0,0.10)`, `border-radius: 4px`
- Brand color: `#E8401C`
- Text: `#111` primary, `#444` body, `#666` secondary
- Logo in header: read `.claude/skills/overbooked-doc/logo.b64`, embed as `<img>` with `height:28px`
- Header: logo left + "Learning Notes · {date}" right, `border-bottom: 2px solid #111`
- Footer: logo centered + "Overbooked · Internal Learning Notes"

**Component library — use these for rich visual design:**
```
.section-title   → 10px, 700, uppercase, #E8401C, letter-spacing 0.1em
.highlight-box   → border-left:3px solid #E8401C, bg:#fff5f3, padding 10px 14px
.highlight-box.dark → bg:#111, color:#fff, border-left #E8401C
.card            → bg:#f7f7f7, border-radius:6px, padding:12px 14px
.card-label      → 9px, 700, uppercase, #E8401C
.sop-step        → flex row, numbered circle (bg #E8401C, white text), title + desc
.system-table    → thead bg:#111 white text, tbody alternating #fafafa, 10px font
.two-col         → grid 2 columns gap 10px
.insights-grid   → grid 2 columns, each card with large colored number + text
.flow-diagram    → flex row, .flow-node (bg:#111, white, 8.5px), .flow-arrow
.badge.red       → bg:#ffe5e0, color:#E8401C
.badge.green     → bg:#e0f5ec, color:#1a7a4a
```

**Content rules:**
- Do NOT just convert MD text to HTML paragraphs. **Redesign each section with appropriate components.**
- Key Insights → use `insights-grid` with numbered cards
- Step-by-step workflows → use `sop-step` with numbered circles
- Comparison tables → use `system-table`
- Code/templates → use `<pre>` blocks with monospace font, `#f4f4f4` bg
- Frameworks/structures → use `card` grid or `highlight-box`
- Sources → use a clean list with title, description, and link
- Include ALL content — do not truncate or summarize

**To embed logo:** Read `.claude/skills/overbooked-doc/logo.b64` via Bash and embed inline.

### 7B — Deploy to Cloudflare
```bash
cd "/Users/joelsia97/Documents/Pixel Pitch"
git add public/learn-{topic-slug}.html
git commit -m "Deploy learning notes: {Topic Name}"
git push origin main
```
Live URL: `https://overbooked-9q2.pages.dev/learn-{topic-slug}.html`

### 7C — Send Slack DM to Joel
Use `slack_send_message` with `channel_id: U09MLKD15L4`:

```
📚 *Learning Notes Ready: {Topic Name}*

🔗 *Read it here:* https://overbooked-9q2.pages.dev/learn-{topic-slug}.html

📋 *What's inside:*
• Learning Curve Map
• {N} Key Insights
• Frameworks & Models
• Actionable Next Steps
• Notable Sources

_Open on your phone — no download needed._
```
