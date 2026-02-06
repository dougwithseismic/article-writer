---
name: research
description: Deep research for an article section. Gathers 10+ sources, finds real-world examples, collects supporting evidence, and identifies case studies. Produces comprehensive research notes.
allowed-tools: Read, Write, WebSearch, WebFetch, Glob
---

# Deep Section Research Agent

Research the section: **$ARGUMENTS**

## Research Standards
- **10+ quality sources** per section
- **3+ real-world examples** with links
- **Supporting evidence** appropriate to the article type
- **Expert quotes** where available
- **Contrasting viewpoints** if they exist

## Setup

1. **Load Article Context**
   - Read `article.json` to find section matching `$ARGUMENTS`
   - Note the section's `scaffold`, `required_elements`, and `research_questions`
   - Understand the `narrative_role` (hook, foundation, deep-dive, etc.)
   - Check `article_type` to know what kinds of evidence to prioritize

2. **Review What's Needed**
   - Word target for this section
   - Required examples and links
   - Specific research questions to answer

## Research Process

### Phase 1: Broad Search (5+ queries)

Search for current, authoritative information:

```
Query patterns:
- "[topic] 2025 2026" - Recent content
- "[topic] guide" - Educational content
- "[topic] examples" - Real implementations
- "[topic] best practices" - Expert recommendations
- "[topic] case study" - Real-world applications
- "[topic] vs [alternative]" - Comparisons
- "[topic] research data statistics" - Hard evidence
- "[topic] expert opinion" - Authority voices
```

### Phase 2: Find Real-World Examples

This is critical. Search specifically for:
- **Named companies** that succeeded or failed with this approach
- **Named people** (founders, practitioners, experts) with relevant stories
- **Research studies** with concrete findings and data
- **Case studies** with measurable outcomes
- **Historical examples** or precedents
- **Conference talks or interviews** with practitioners

For each example found, note:
- Name (person, company, study) and URL
- What makes it notable
- Specific details, numbers, or outcomes
- Why it matters for the article's argument

### Phase 3: Deep Dive (WebFetch)

For the 5 most promising sources:
- Fetch full content
- Extract key insights, not just summaries
- Pull exact quotes with attribution
- Note any data/statistics
- Gather evidence appropriate to the article type

### Phase 4: Topic-Specific Evidence

Based on `article_type` in article.json, prioritize gathering:

| Article Type | Evidence to Prioritize |
|-------------|----------------------|
| Technical | Code samples, documentation, benchmarks, repo examples |
| Business | Revenue figures, growth metrics, strategy breakdowns |
| Marketing | Campaign results, conversion data, channel comparisons |
| Opinion | Research papers, expert positions, counterarguments |
| Tutorial | Step-by-step processes, tool comparisons, common pitfalls |
| Cultural | Quotes, surveys, trend data, cultural artifacts |

### Phase 5: YouTube/Video Content

Search for relevant videos:
- Conference talks (often have unique insights)
- Interviews with practitioners
- Documentary-style explorations of the topic

Note: Title, URL, timestamp of key moments, why it's valuable

## Output Files

### 1. Research Notes: `research/[section-id].md`

Structure your notes for easy writing:

```markdown
# Research: [Section Title]

## Executive Summary
3-4 sentences capturing the most important findings for this section.

## Key Insights (for narrative)
1. **Insight with hook potential**: [explanation with source]
2. **Surprising finding**: [explanation with source]
3. **Common misconception**: [what people get wrong]

## Real-World Examples

### [Example Name 1]
- **URL**: [link]
- **What happened**: [description]
- **Key detail**: [specific numbers, outcomes, or quotes]
- **Why it's notable**: [what makes this a good example]

### [Example Name 2]
... continue for 3+ examples ...

## Supporting Evidence

### [Evidence piece 1]
- **Source**: [url]
- **Type**: [data/quote/case study/code sample]
- **Content**: [the actual evidence]
- **Context**: [how to use this in the article]

## Expert Quotes
> "Quote that captures key insight"
> — [Name], [Role] at [Company] ([Source](url))

## Statistics & Data
- [Stat 1 with source]
- [Stat 2 with source]

## Video Resources
- **[Video Title](url)** - [Why it's valuable, key timestamp]

## Contrasting Views (if any)
- View A: [perspective with source]
- View B: [alternative perspective with source]

## Questions Answered

### Q: [Research question 1]
**A:** [Detailed answer with citations]

### Q: [Research question 2]
**A:** [Detailed answer with citations]

## Raw Sources
| Title | URL | Type | Key Value |
|-------|-----|------|-----------|
| ... | ... | doc/blog/video | ... |
```

### 2. Update sources.json

Append comprehensive source data:

```json
{
  "section": "section-id",
  "researched_at": "timestamp",
  "sources": [
    {
      "title": "Source Title",
      "url": "https://...",
      "type": "documentation|blog|video|research|case-study|interview",
      "authority": "high|medium",
      "key_quotes": ["quote 1", "quote 2"],
      "accessed": "YYYY-MM-DD"
    }
  ],
  "examples": [
    {
      "name": "Example Name",
      "url": "https://...",
      "description": "What it is",
      "relevance": "Why include in article"
    }
  ]
}
```

### 3. Update article.json
- Set `research_complete: true`
- Add discovered sources to main sources array

### 4. Append to progress.txt
```
[timestamp] Research: [section-id]
- Sources: N (target: 10+)
- Examples found: N (target: 3+)
- Key insight: [one sentence]
- Research quality: [PASS/NEEDS MORE]
```

## Quality Check

Before marking complete, verify:
- [ ] 10+ quality sources gathered
- [ ] 3+ real-world examples with URLs
- [ ] Evidence appropriate to article type collected
- [ ] Research questions answered with citations
- [ ] Enough material to write 800+ words

If any check fails, do more research before completing.

## Completion

Summarize:
- Sources gathered (should be 10+)
- Examples found (should be 3+)
- Evidence collected
- Research quality assessment
- Next step: `/research [next-section]` or `/outline` when all done
