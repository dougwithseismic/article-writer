---
name: prd
description: Generate a comprehensive article specification. Creates detailed scaffolding with narrative beats, word targets, and example requirements for a 10k+ word article.
allowed-tools: Read, Write, Glob, WebSearch
---

# Article PRD Generator (10k+ Word Target)

Generate a comprehensive article specification for: **$ARGUMENTS**

## Target Output

- **10,000+ words** total article
- **8-12 sections** with 800-1500 words each
- **Rich with examples** — real people, companies, case studies, data
- **Narrative flow** — story arc, not information dump
- **Prose-first** — 70% flowing text, 30% structured elements

## Article Type Detection

Before scaffolding, determine the article type. This shapes what "evidence" looks like:

| Type | Primary Evidence |
|------|-----------------|
| Technical | Code samples, documentation, repos, benchmarks |
| Business/Strategy | Case studies, revenue data, company examples |
| Marketing/Growth | Campaign results, conversion data, tool comparisons |
| Opinion/Analysis | Research papers, expert quotes, historical precedent |
| Tutorial/How-To | Step-by-step walkthroughs, screenshots, before/after |
| Cultural/Trend | Quotes, surveys, cultural artifacts, timelines |

Set `article_type` in article.json. This tells downstream skills what kinds of evidence to prioritize.

## Editorial Philosophy

The PRD sets the tone. If the scaffolding encourages bullet-heavy writing, the article will be bullet-heavy. Build narrative thinking into the structure from the start.

**Headers that flow:** Avoid double-barreled academic titles like "Finding Prospects: Strategies for Success". Use direct, conversational headers: "Where Good Prospects Hide" or "Building Your First 100 Contacts".

**Critical thinking prompts:** Each section scaffold should include a "why it matters" question, not just "what to cover".

## Your Task

### Step 1: Topic Research

Quick WebSearch to understand:
- Current state of the topic (2025-2026)
- Key players, people, companies, and examples
- Common pain points and solutions
- What makes this topic interesting NOW

### Step 2: Define the Narrative Arc

Every great article tells a story:

```
HOOK → CONTEXT → FOUNDATION → DEEP DIVES → PRACTICAL APPLICATION → INSPIRATION → ACTION
```

Map sections to this arc:
1. **Hook** (intro) — Why should reader care RIGHT NOW?
2. **Context** — What's the landscape? What changed?
3. **Foundation** — Core concepts needed
4. **Deep Dives** (2-4 sections) — Specific techniques with examples
5. **Practical** — How to actually do this
6. **Inspiration** — Real-world examples, case studies
7. **Action** — What reader should do next

### Step 3: Create Section Scaffolding

For EACH section, define:

```json
{
  "id": "section-slug",
  "title": "Short Punchy Title",
  "priority": 1,
  "status": "pending",
  "research_complete": false,
  "word_target": 1200,
  "word_minimum": 800,
  "narrative_role": "hook|context|foundation|deep-dive|practical|inspiration|action",
  "required_elements": {
    "examples_minimum": 2,
    "external_links_minimum": 3,
    "evidence_types": ["case study", "data point", "expert quote"]
  },
  "scaffold": {
    "opening_hook": "One compelling sentence that pulls reader in",
    "central_argument": "The main insight this section delivers",
    "key_questions": [
      "Why does this matter?",
      "What's the trade-off or nuance?",
      "When would you NOT do this?"
    ],
    "example_types": ["company case study", "named person", "research finding"],
    "closing_bridge": "Natural transition to next section"
  },
  "research_questions": [
    "Specific question 1?",
    "Specific question 2?",
    "What are the best real-world examples?"
  ]
}
```

**Title Guidelines:**
- NO colons separating concept from description
- 3-7 words, conversational tone
- Should sound like something you'd say to a colleague

Good: "Where Good Prospects Hide"
Bad: "Finding Prospects: Strategies and Techniques"

Good: "The Foundation Nobody Talks About"
Bad: "Email Deliverability: Technical Setup and Best Practices"

### Step 4: Identify Example Requirements

For each deep-dive section, specify:
- **Real-world examples** to feature (actual companies, people, events)
- **Evidence** appropriate to the article type (code for technical, data for analytical, case studies for business)
- **Comparisons** (before/after, old vs new approach)

### Step 5: Create Article Directory

```bash
mkdir -p articles/[slug]/{research,output,drafts}
```

### Step 6: Write article.json

```json
{
  "topic": "Original topic",
  "slug": "url-slug",
  "article_type": "technical|business|marketing|opinion|tutorial|cultural",
  "title": "Compelling Title That Promises Value",
  "subtitle": "More specific description of what reader learns",
  "summary": "3-4 sentence executive summary covering: the problem, the solution, what reader will learn, why it matters now",
  "status": "draft",
  "created": "YYYY-MM-DD",
  "targets": {
    "total_words": 10000,
    "minimum_words": 8000,
    "sections": 10,
    "external_links": 30,
    "real_world_examples": 10
  },
  "editorial_standards": {
    "prose_ratio_minimum": 0.7,
    "max_consecutive_bullets": 5,
    "header_style": "conversational, no colons"
  },
  "narrative_arc": {
    "hook": "Why this matters now",
    "tension": "The problem/challenge",
    "resolution": "How to solve it",
    "transformation": "What reader becomes capable of"
  },
  "sections": [],
  "quality_gates": {
    "min_words_per_section": 800,
    "min_links_per_section": 3,
    "min_examples_total": 10,
    "narrative_flow_check": true,
    "prose_ratio_check": true
  },
  "sources": [],
  "metadata": {
    "target_audience": "Description of who this is for",
    "reading_time": "40-50 minutes",
    "depth": "high",
    "tags": []
  }
}
```

### Step 7: Initialize progress.txt

```
# Article Progress Log
Topic: [topic]
Type: [article_type]
Target: 10,000+ words
Created: [date]

## Quality Gates
- [ ] All sections meet word minimums (800+)
- [ ] 30+ external links
- [ ] 10+ real-world examples
- [ ] Narrative flow verified
- [ ] Prose ratio 70%+ (not bullet-heavy)
- [ ] Headers conversational (no colons)

## Iterations
```

## Output Summary

After creating the PRD, report:
- Article title and subtitle
- Article type detected
- Number of sections with word targets
- Total target word count
- Key examples to feature
- Narrative arc summary
- Next step: `/research [first-section-id]`

## Quality Standard

This PRD sets the bar. Thin scaffolding produces thin sections. Invest time here to define:
- Compelling hooks and natural bridges
- Critical thinking questions (not just "what to cover")
- Concrete example requirements appropriate to the topic
- Headers that flow conversationally

A good PRD makes writing 10x easier.
