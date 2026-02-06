# article-writer - Editorial Standards

An autonomous agent for writing comprehensive, long-form articles (10k+ words) with deep research, real-world examples, and quality-gated completion.

## Quality Standards

Every article must meet these gates before completion:

- **10,000+ words** (8,000 minimum)
- **30+ external links** with citations
- **10+ real-world examples** (named people, companies, studies, or projects)
- **70%+ prose ratio** (not bullet-heavy)
- **Conversational headers** (no colon-split academic style)
- **Strong narrative** - hook, flow, closing transitions
- **800+ words per section** minimum

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

Every article follows:

```
HOOK → CONTEXT → FOUNDATION → DEEP DIVES → PRACTICAL → INSPIRATION → ACTION
```

- **Hook**: Why should reader care RIGHT NOW?
- **Context**: What's the landscape?
- **Foundation**: Core concepts
- **Deep Dives**: Specific techniques with examples
- **Practical**: How to actually do this
- **Inspiration**: Real-world examples
- **Action**: What reader should do next

## Writing Style

### Prose First
- 70%+ flowing paragraphs, 30% or less structured elements
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

Per section:
- 10+ quality sources
- 3+ real-world examples with URLs
- Supporting evidence appropriate to the topic
- Expert quotes where available
- Answer all research questions with citations

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
