# article-writer - Editorial Standards

An autonomous agent for writing articles with deep research, real-world examples, and quality-gated completion. Supports multiple content types and custom word lengths.

## Content Types

The agent supports different content formats. Each has its own default targets, but word length can always be overridden:

| Type | Default Words | Sections | Links | Examples | Prose Ratio |
|------|--------------|----------|-------|----------|-------------|
| long-form | 10,000 | 8-12 | 30+ | 10+ | 70% |
| research | 8,000 | 6-10 | 40+ | 8+ | 75% |
| news | 1,500 | 3-5 | 8+ | 3+ | 80% |
| opinion | 3,000 | 4-6 | 10+ | 5+ | 80% |
| listicle | 3,000 | 5-15 | 15+ | 8+ | 40% |
| email | 1,000 | 2-4 | 3+ | 2+ | 85% |
| tutorial | 5,000 | 5-8 | 15+ | 5+ | 55% |
| case-study | 4,000 | 4-6 | 10+ | 3+ | 75% |

**Usage examples:**
- `/prd Topic Here` — long-form article (default)
- `/prd news: Breaking coverage of X` — news format with preset targets
- `/prd 4000 words: Deep dive into Y` — long-form at custom length
- `/prd tutorial 2000 words: How to build Z` — tutorial at custom length

Content type controls **format** (length, structure, quality gates). Article type controls **evidence** (technical, business, etc.) and is auto-detected from the topic.

## Quality Standards

Quality gates are set per-article in `article.json` based on content type and custom word count. The defaults for long-form are:

- **10,000+ words** (8,000 minimum)
- **30+ external links** with citations
- **10+ real-world examples** (named people, companies, studies, or projects)
- **70%+ prose ratio** (not bullet-heavy)
- **Conversational headers** (no colon-split academic style)
- **Strong narrative** - hook, flow, closing transitions
- **800+ words per section** minimum

All downstream skills read their targets from `article.json`, not hardcoded values.

## Article Type Awareness

The agent adapts to the topic. A technical article naturally includes code samples. A business strategy piece includes case studies. A cultural essay includes quotes and references. The PRD phase detects article type and sets appropriate per-section requirements.

## Editorial Philosophy

**Narrative over lists.** Readers come for insight, not bullet points. Every list needs surrounding prose that explains the "why" and connects ideas. Lists are punctuation, not the main text.

**Critical thinking over information dumps.** Don't just present facts—analyze them. Ask: Why does this matter? What's the trade-off? When would you NOT do this?

**Headers that flow.** NO double-barreled titles with colons.

- Bad: "Finding Prospects: Where to Look and Who to Target"
- Good: "Where Good Prospects Hide"

Headers should sound like something a smart friend would say, not academic paper sections.

## Narrative Arc

The narrative arc adapts to the content type. Long-form articles follow:

```
HOOK -> CONTEXT -> FOUNDATION -> DEEP DIVES -> PRACTICAL -> INSPIRATION -> ACTION
```

Shorter formats use condensed arcs (news: hook/context/details/impact, email: hook/value/cta, etc.). The PRD reads the appropriate arc from `templates/presets.json`.

## Writing Style

### Prose First
- Prose ratio target varies by content type (40% for listicles, 85% for emails)
- Weave information into narrative, don't just list it
- Every list needs setup prose and synthesis prose

### Voice
- Active voice, second person ("you")
- Specific over vague ("13% response rate" not "good results")
- Show then tell (example before explanation)
- Every claim needs a citation
- Real examples with links, not hypotheticals

### Headers
- Short, punchy, 3-7 words
- NO colons separating topic from description
- Conversational tone, not academic

## Research Standards

Research depth scales with content type:
- Long-form/Research: 10+ sources per section, 5+ search queries
- Tutorial/Case-study: 5-8 sources per section, 4+ search queries
- News/Opinion: 3-5 sources per section, 3+ search queries
- Email: 3 sources per section, 2+ search queries

## Project Structure

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
