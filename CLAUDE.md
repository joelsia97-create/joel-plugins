# CLAUDE.md — joel-plugins

## Project Overview

This is **joel-plugins**, Sia Joel's personal Claude Code plugin marketplace. It serves as a centralized repository for reusable AI plugins (agents and skills) that integrate into Claude Code workflows.

**Owner:** Sia Joel (joel@overbooked.my)

## Repository Structure

```
joel-plugins/
├── .claude-plugin/
│   └── marketplace.json              # Central plugin registry (names, versions, paths)
├── plugins/
│   ├── agent-builder/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json          # Plugin metadata
│   │   └── agents/
│   │       └── agent-builder.md     # Agent definition (YAML frontmatter + markdown)
│   └── notebooklm-learning/
│       ├── .claude-plugin/
│       │   └── plugin.json          # Plugin metadata
│       └── skills/
│           └── notebooklm-learning/
│               └── SKILL.md         # Skill definition (markdown)
└── CLAUDE.md                         # This file
```

## Plugins

### 1. agent-builder (v1.0.0)

A specialized agent that guides users through building custom AI agents via a 3-phase process:
- **Phase 1 — Workflow Discovery:** Maps the user's current job workflow
- **Phase 2 — Automation Audit:** Identifies which steps are worth automating
- **Phase 3 — Agent Design & Build:** Generates a Blueprint, iterates, and builds the agent

Defined in: `plugins/agent-builder/agents/agent-builder.md`

### 2. notebooklm-learning (v1.0.0)

A skill that builds NotebookLM-powered learning notebooks with a 6-step workflow:
1. Learning concept mapping (5 questions)
2. Notebook creation in NotebookLM
3. Parallel YouTube + web research (30-50 sources)
4. Source loading into notebook
5. Dimension-based deep querying
6. Markdown output + optional HTML deployment to Cloudflare

Defined in: `plugins/notebooklm-learning/skills/notebooklm-learning/SKILL.md`

## Plugin Convention

### Directory Layout

Each plugin lives under `plugins/<plugin-name>/` and contains:
- `.claude-plugin/plugin.json` — metadata (name, description, version, type)
- Either `agents/<name>.md` for agent-type plugins or `skills/<name>/SKILL.md` for skill-type plugins

### Agent Files

Agent markdown files use YAML frontmatter:
```yaml
---
name: agent-name
description: Brief description
model: sonnet
color: green
---
```
Followed by the agent's instructions in markdown.

### Marketplace Registry

All plugins must be registered in `.claude-plugin/marketplace.json` with name, description, version, author, and source path.

## Adding a New Plugin

1. Create `plugins/<plugin-name>/` directory
2. Add `.claude-plugin/plugin.json` with metadata
3. Add the agent or skill definition file in the appropriate subdirectory
4. Register the plugin in `.claude-plugin/marketplace.json`

## Key Conventions

- **Bilingual content:** Plugins support Chinese (Mandarin) and English — Chinese is the primary user-facing language, English for technical terms
- **Phase-based workflows:** Plugins follow structured multi-phase processes with approval gates between phases
- **Template-driven:** Heavy use of markdown templates and structured question formats (A/B/C/D choices)
- **No build system:** This is a documentation/markdown-driven project with no compilation or package management
- **No test framework:** Validation is workflow-based (following the plugin steps end-to-end)

## Git Workflow

- Primary branch: `main`
- Commit messages should describe the plugin change (e.g., "Add agent-builder plugin to marketplace")
- No CI/CD pipeline — manual git operations only
