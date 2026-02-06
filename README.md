# article-writer

A Claude Code plugin for writing comprehensive, long-form articles (10k+ words) with deep research, real-world examples, and quality-gated completion.

Works for any topic — technical, business, creative, opinion, educational, or otherwise. The agent adapts its evidence-gathering strategy to the article type: code samples for technical pieces, case studies for business articles, data for analytical work.

## What You Get

- **10,000+ word** articles with narrative flow
- **30+ external links** with citations baked in
- **10+ real-world examples** (named people, companies, studies)
- **70%+ prose ratio** — reads like a long-form piece, not a listicle
- **Quality gates** that enforce standards before completion

## Installation

```bash
# From GitHub
claude plugin install <repo-url>
```

## Quick Start

```bash
# Generate an article spec
/article-writer:prd realistic cold outreach for developer services

# Run the full autonomous pipeline
/article-writer:writer

# Or run steps individually — see Skills below
```

## Skills

### `/article-writer:prd [topic]`

Generates a detailed article specification. Detects article type, creates 8-12 section scaffolds with narrative beats, word targets, example requirements, and research questions.

**Output**: `articles/[slug]/article.json`

### `/article-writer:research [section-id]`

Deep research for a single section. Runs 5+ search queries, finds 10+ sources, identifies 3+ real-world examples, collects expert quotes and supporting evidence.

**Output**: `articles/[slug]/research/[section-id].md`

### `/article-writer:outline`

Creates a detailed outline from completed research. Maps subsections, plans media placements, identifies the strongest examples and quotes to feature.

**Output**: `articles/[slug]/outline.md`

### `/article-writer:write-section [section-id]`

Writes a single section (800-1500 words) with prose-first editorial standards. Weaves research into narrative, adds inline citations, uses conversational headers.

**Output**: Appended to `articles/[slug]/output/article.md`

### `/article-writer:embed-media`

Finds and embeds YouTube videos and generates mermaid diagrams. Replaces placeholders with responsive embed HTML.

### `/article-writer:review`

Fact-checks sources, fixes editorial issues (bullet-heavy sections, colon headers), polishes transitions, and marks the article complete.

### `/article-writer:eval`

Runs all quality gates and returns PASS/FAIL with specific issues. Gates check word count, section completeness, external links, real-world examples, prose ratio, header style, and narrative quality.

### `/article-writer:writer`

The main autonomous loop. Orchestrates the full pipeline — research, outline, write, embed, review, eval — and loops until all quality gates pass.

## Pipeline

```
/prd → /research (per section) → /outline → /write-section (per section) → /embed-media → /review → /eval
```

The `/writer` skill runs this entire pipeline autonomously, looping back to fix any quality gate failures.

```
RESEARCH → OUTLINE → WRITE → EMBED → REVIEW → EVAL
                       ↑                          ↓
                       └── REVISE IF EVAL FAILS ──┘
```

## Quality Gates

The `/eval` skill checks these gates. The pipeline won't complete until all pass:

| Gate | Target | Minimum |
|------|--------|---------|
| Total word count | 10,000+ | 8,000 |
| Words per section | 1,000+ | 800 |
| External links | 30+ | 20 |
| Real-world examples | 10+ | 5 |
| Prose ratio | 70%+ | 60% |
| Header style | Conversational | No colons |
| Narrative quality | Hook + flow + closing | All three |

## Article Structure

Each article lives in its own directory:

```
articles/[slug]/
├── article.json     # Spec, sections, progress, quality gates
├── progress.txt     # Learnings and iteration log
├── outline.md       # Detailed outline
├── research/        # Research notes per section
│   ├── sources.json
│   └── [section-id].md
└── output/
    └── article.md   # Final article
```

## Editorial Philosophy

**Narrative over lists.** 70%+ flowing prose. Lists are punctuation, not the main text.

**Critical thinking over information dumps.** Every section analyzes trade-offs and nuances, not just presents facts.

**Conversational headers.** No academic double-barreled titles with colons. Headers sound like something a smart friend would say.

**Real examples only.** Named people, companies, studies — with links. No hypotheticals.

## Customization

The `article.json` spec controls everything. You can adjust:

- **Word targets** per section and total
- **Quality gate thresholds** (links, examples, prose ratio)
- **Section count and structure**
- **Article type** (which changes what evidence the research phase gathers)
- **Narrative arc** roles per section

Edit `templates/article-spec.json` for your own defaults.

## Hooks

The plugin includes hooks that provide feedback during writing:

- **Stop hook**: Evaluates article quality before the agent completes
- **PreToolUse (WebSearch)**: Logs search queries during research
- **PostToolUse (Write)**: Reports word count when article.md is updated

## License

MIT
