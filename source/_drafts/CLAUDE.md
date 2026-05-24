# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Purpose

AI-Native Note-Taking System for Professor Xueheng Li (李学恒). Claude Code processes every note input through analysis, filing, and integration using Obsidian features.

Xueheng Li's persona: `3_背景/Persona.md`

## Session Start Protocol

**Before any task**, run `date` to confirm current date/time. This prevents outdated information in searches and ensures temporal accuracy.

## Note Processing Workflow

1. **Analyze** - Topic, keywords, note type
2. **Context** - Read Personal Profile/Research Status; for research notes, read source files first
3. **Search** - Find related notes for linking
4. **Place** - Select subfolder; **ASK USER** if uncertain
5. **Create/Update** - Write with links, tags, formatting
6. **Connect** - Add `links`; suggest MoC for 5+ related notes
7. **Report** - Confirm actions and connections made

## Folder Structure

```text
 NewNote/
├── 0_任务中台/# Cross-project task dispatch (TODO.md driven)
├── 1_导航/           # External file index
├── 2_AI/             # AI notes & ideas
│   ├── 2.1_使用实践/     # CC usage, insights, best practices
│   ├── 2.2_进阶研究/     # Advanced research directions
│   ├── 2.3_Skill开发/    # Skill development & implementation
│   └── 2.4_AI思想/       # AI economics, methodology, impact
├── 3_背景/           # Xueheng Li's persona summary & personal profile (CV) & research status
├── 4_教学/           # Teaching materials
├── 5_会议/           # Meeting notes
├── 6_研究/           # Research notes
│   ├── 6.1_AI技术/       # AI frameworks, tech research
│   ├── 6.2_学术论文/     # Paper revisions, publications
│   ├── 6.3_时事分析/     # Current events analysis
│   ├── 6.4_写作指南/     # Academic writing guides
│   └── zotero/           # Zotero literature notes
├── 7_行政/           # Admin reports
│   ├── 7.1_年度总结/     # Annual work summaries
│   ├── 7.2_讲座报告/     # Academic lecture reports
│   └── 7.3_汇报材料/     # Presentation materials
├── 8_代码/           # Code snippets and remote server information
├── 9_学习笔记/       # Study notes (textbooks, courses)
│   ├── econometrics/     # Econometrics study materials
│   ├── platform_economics/  # Platform economics course notes
│   ├── DiscreteChoiceMethods_Train_20260124/  # Train's discrete choice textbook
│   ├── Doing_Economics_Bellemare_20260124/    # Bellemare's practical economics guide
│   ├── Networks_Goyal_20260124/               # Goyal's network economics textbook
│   ├── digital_economics_20260125/            # Digital economics materials
│   ├── networks_economics_goyal_20260125/     # Network economics supplementary
│   └── platform_economics_20260125/           # Platform economics supplementary
├── 10_xai_assets/    # XAI agent reference docs & version history
│   ├── domain-expertise.md   # Current expertise profile
│   ├── knowledge-index.md    # Vault navigation map
│   └── versions/             # Historical snapshots of xai
├── 11_交互日志/      # Claude Code session logs (auto-generated)
├── 12_小李日记/      # XAI agent personal diary & reflections
├── 附件/             # Attachments
├── 书签/             # Bookmarks
├── canvas/           # Obsidian canvas files
├── keys/             # API keys and credentials
├── agent_tasks/      # Multi-agent task working directories
├── AI助手/           # AI assistant projects (OpenClaw etc.)
├── multi_agent_tasks/# Multi-agent task outputs
├── xai-workspace/    # XAI agent outputs
├── 临时工作区/       # Temp files
├── 每日新闻/         # Daily news digests
└── 系统配置/         # System config
```

**Root Directory Rule**: Only `CLAUDE.md` belongs at root level. All other files MUST go into subfolders.

## Obsidian Skills

Official Obsidian skills from [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) are installed and available:

- **obsidian-markdown**: Full Obsidian Flavored Markdown support with comprehensive documentation for wikilinks, embeds, callouts, properties, and all Obsidian-specific syntax
- **obsidian-bases**: Create and edit Obsidian Bases (.base database files) with views, filters, formulas, and summaries
- **json-canvas**: Create and edit JSON Canvas files (.canvas) for visual knowledge graphs with nodes, edges, and groups

These skills provide deep understanding of Obsidian's proprietary formats. Use them when working with .md, .base, or .canvas files.

## Obsidian Features

### Wiki Links `[[]]`

- **只链接已存在的笔记** - 不要为机构名、学科概念、期刊名等创建空链接，除非该笔记确实存在或用户明确要求创建
- **完整相对路径** - 嵌入附件时使用从 vault 根目录开始的完整路径，如 `![[附件/folder/file.mp3]]`，避免仅用文件名
- **Aliases** - `BE research` displays as "BE research"
- **Headings** - `note name` for precise references

### Tags `#`

Use sparingly. Primary tags in frontmatter (type, status); contextual tags inline.

```text
#type/idea    #type/reference    #status/draft    #project/nsfc
```

### Maps of Content

For topics with 5+ related notes, create a MoC note (e.g., `Game Theory MOC.md`) as a navigation hub.

### Folder Index Files

Each folder contains an `_index.md` file with one-sentence descriptions for each note.

### External File Index

The `1_导航/` folder indexes external SynologyDrive resources. See `1_导航/CLAUDE.md` for the complete architecture.

## Note Format Standard

```markdown
---
created: YYYY-MM-DD
tags:
  - type/idea
  - status/draft
aliases: [alternative name, 别名]
---

# Title

[Content with natural wiki links to related concepts]

## Related

- Related Note 1

## Source

[If applicable: URL, paper citation, origin]
```

## Writing Style Guidelines

**避免翻译腔和AI味表达**：

- 不要有 AI 味
- 写自然流畅的中文，像人说话一样
- 避免："值得注意的是"、"需要强调的是"、"总而言之"、"综上所述"、'这不是科幻'
- 避免：过度使用"的"字、生硬的被动语态、冗长的定语从句
- 避免：空洞的套话和过度礼貌的表达
- 创建新笔记后立即检查是否有翻译腔或AI味表达，发现即改
- 如非必要，勿用引号

### Chinese-English Mixed Formatting
- Add half-width space between Chinese and English/numbers: `Claude Code 是`, `第 1 章`
- Use full-width punctuation for Chinese, half-width for English/code
- Keep proper nouns in English: Claude Code, MCP, Skills, GitHub, Vibe Coding

## Quick Commands

- **Raw text**: Process and file automatically
- **`@待整理`**: Force placement in pending folder for manual review
- **`@merge note name`**: Integrate content into existing note
- **`@type:[category]`**: Override automatic categorization
- **`@link note`**: Explicitly request linking to specific note
- **`/import <内容或路径>`**: Universal import for notes, URLs, files

## Safety Rules

- Confirm before merging/modifying existing notes
- Never delete content without permission
- Preserve original input
- Show proposed links/tags before applying
- Only read (don't modify) files outside this vault

## Temporary Workspace (临时工作区)

For AI-generated scripts, pipeline processing, and intermediate results. Use prefixes: `temp_`, `pipeline_`, `script_`, `draft_`, `test_`. Review and clean regularly; move important results to proper folders.

## Use subagents for different tasks

- **xai**: Operates as Professor Xueheng Li (`ultrathink` mode). Use for drafting communications, evaluating research ideas, and making decisions from his perspective. Outputs go to `xai-workspace/`.
- **x-writer**: Handles long-form academic writing/revision (papers, grant proposals, literature reviews). Use via Task tool for extended writing tasks.
- **web-researcher**: Conducts comprehensive web research, fact-checking, and information gathering. Use for research tasks requiring thorough investigation, cross-verification of sources, and structured presentation of findings.

## Claude Code insights analysis

- Use the cc-insights skill
- 设置 archive_chats.py 的输出目录为 `~/SynologyDrive/ClaudeCodeArchive/`
- Save analysis reports to `2_AI/2.1_使用实践/` of current vault

## Continuous Improvement

Capture reusable insights in `系统配置/lessons_learned.md`. Read it before applying learned preferences.

## Documentation Maintenance

Update this file whenever the vault's directory structure changes. You should automatically update this @CLAUDE.md file whenever the folder structure changes.