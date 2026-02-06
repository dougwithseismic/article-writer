# article-writer

A Claude Code plugin for writing publication-ready articles with deep research, real-world examples, and quality-gated completion.

Supports 8 content types — from 500-word emails to 10k+ word deep dives — with customizable word lengths. The agent adapts its research depth, narrative arc, and quality gates to match the format you need.

## Quick Start

```bash
# Install
claude plugin install <repo-url>

# Write a long-form article (default)
/article-writer:prd The state of AI agents in 2026

# Write a news article
/article-writer:prd news: OpenAI releases GPT-5

# Write a tutorial at custom length
/article-writer:prd tutorial 3000 words: Building a RAG pipeline with LangChain

# Write an email newsletter
/article-writer:prd email: Weekly AI tools roundup

# Run the full autonomous pipeline
/article-writer:writer
```

## Content Types

Each content type has default targets, but you can always override the word count. Content type controls **format** (length, structure, quality gates). Article type controls **evidence** (technical, business, cultural, etc.) and is auto-detected from the topic.

| Type | Default Words | Sections | Links | Examples | Prose Ratio | Best For |
|------|:------------:|:--------:|:-----:|:--------:|:-----------:|----------|
| `long-form` | 10,000 | 8-12 | 30+ | 10+ | 70% | Deep dives, comprehensive guides |
| `research` | 8,000 | 6-10 | 40+ | 8+ | 75% | Data-driven analysis, whitepapers |
| `tutorial` | 5,000 | 5-8 | 15+ | 5+ | 55% | Step-by-step how-tos, walkthroughs |
| `case-study` | 4,000 | 4-6 | 10+ | 3+ | 75% | Company/project deep dives |
| `opinion` | 3,000 | 4-6 | 10+ | 5+ | 80% | Argument-driven essays, hot takes |
| `listicle` | 3,000 | 5-15 | 15+ | 8+ | 40% | Curated lists with context |
| `news` | 1,500 | 3-5 | 8+ | 3+ | 80% | Timely coverage, announcements |
| `email` | 1,000 | 2-4 | 3+ | 2+ | 85% | Newsletters, email campaigns |

### Custom Word Lengths

Word count is independent from content type. Override any preset:

```bash
/article-writer:prd 4000 words: State of JavaScript testing   # long-form at 4k words
/article-writer:prd news 3000 words: React Server Components   # news format, longer than default
/article-writer:prd email 2000 words: Monthly product update    # email format, longer than default
```

When you set a custom word count, all other targets (links, examples, sections, research depth) scale proportionally from the preset's base ratios.

### How Content Type and Article Type Work Together

These two axes are orthogonal:

- **Content type** = format (how long, how structured, what quality gates)
- **Article type** = evidence (what kind of research and examples to gather)

So "technical tutorial" and "technical long-form" use the same evidence strategy (code samples, docs, benchmarks) but completely different formats. A "business news" article gathers business evidence (revenue, case studies) but in a short, timely news format.

## Skills

### `/article-writer:prd [content-type] [word-count]: [topic]`

Generates a detailed article specification. Reads presets from `templates/presets.json`, detects article type, creates section scaffolds with narrative beats, word targets, example requirements, and research questions.

```bash
/article-writer:prd news: Breaking coverage of new React framework
/article-writer:prd tutorial 8000 words: Complete guide to Kubernetes networking
/article-writer:prd The future of remote work                  # defaults to long-form
```

**Output**: `articles/[slug]/article.json`

### `/article-writer:research [section-id]`

Deep research for a single section. Research depth scales with content type — 2 queries for email, 5+ for long-form. Gathers sources, real-world examples, expert quotes, and topic-specific evidence.

**Output**: `articles/[slug]/research/[section-id].md`

### `/article-writer:outline`

Creates a detailed outline from completed research. Skipped automatically for short formats (fewer than 5 sections).

**Output**: `articles/[slug]/outline.md`

### `/article-writer:write-section [section-id]`

Writes a single section to its word target (from `article.json`). Adapts writing style to content type — inverted pyramid for news, step-by-step for tutorials, CTA-focused for emails.

**Output**: Appended to `articles/[slug]/output/article.md`

### `/article-writer:embed-media`

Finds and embeds YouTube videos and generates mermaid diagrams. Skipped for email and news formats.

### `/article-writer:review`

Fact-checks sources, fixes editorial issues, polishes transitions, and marks the article complete.

### `/article-writer:eval`

Runs all quality gates against the targets in `article.json` (not hardcoded values). Returns PASS/FAIL/WARN with specific issues.

### `/article-writer:writer`

The main autonomous loop. Orchestrates the full pipeline and loops until all quality gates pass. Adapts the pipeline to the content type — shorter formats skip outline and media embedding.

## Pipeline

The pipeline adapts based on content type:

**Full pipeline** (long-form, research, case-study, tutorial):
```
RESEARCH -> OUTLINE -> WRITE -> EMBED MEDIA -> REVIEW -> EVAL
                         ^                                  |
                         +------ REVISE IF EVAL FAILS ------+
```

**Short pipeline** (email, news, opinion, listicle):
```
RESEARCH -> WRITE -> REVIEW -> EVAL
              ^                  |
              +-- REVISE IF FAIL +
```

## Quality Gates

All thresholds are read from `article.json` — set during PRD based on content type and custom word count. Nothing is hardcoded in the eval or stop hooks.

| Gate | What It Checks |
|------|---------------|
| Word count | Total words >= `targets.minimum_words` |
| Section completeness | Each section >= `quality_gates.min_words_per_section` |
| External links | Unique URLs >= `targets.external_links` |
| Real-world examples | Named examples >= `targets.real_world_examples` |
| Prose ratio | Flowing text >= `editorial_standards.prose_ratio_minimum` |
| Header style | No double-barreled colon headers |
| Narrative quality | Hook, flow, and closing all present |

## Article Structure

Each article lives in its own directory:

```
articles/[slug]/
├── article.json     # Spec, targets, sections, quality gates
├── progress.txt     # Iteration log
├── outline.md       # Detailed outline (long-form only)
├── research/
│   ├── sources.json
│   └── [section-id].md
└── output/
    └── article.md   # Final article
```

## Customization

### Presets

Edit `templates/presets.json` to change default targets for any content type, or add new content types entirely. Each preset defines:

- Word targets and minimums
- Section count range
- Link and example requirements
- Prose ratio and editorial rules
- Research depth (sources per section, search queries, video research)
- Narrative arc template

### Per-Article Overrides

The `article.json` spec controls everything for a specific article. The PRD populates it from presets (scaled by custom word count), but you can edit it directly to fine-tune any target.

## Editorial Philosophy

**Narrative over lists.** Prose ratio targets vary by type (40% for listicles, 85% for emails), but the principle holds: lists need surrounding prose that explains the "why".

**Critical thinking over information dumps.** Every section analyzes trade-offs and nuances, not just presents facts.

**Conversational headers.** No academic double-barreled titles with colons. "Where Good Prospects Hide" not "Finding Prospects: Strategies and Techniques".

**Real examples only.** Named people, companies, studies — with links. No hypotheticals.

## Hooks

- **Stop hook**: Evaluates article quality before the agent completes (reads all thresholds from article.json)
- **PreToolUse (WebSearch)**: Logs search queries during research
- **PostToolUse (Write)**: Reports word count when article.md is updated

## License

MIT
