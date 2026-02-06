---
name: writer
description: Main autonomous loop for writing comprehensive articles. Orchestrates research, writing, and quality evaluation until all gates pass. Targets 10k+ words with prose-first editorial standards.
allowed-tools: Read, Write, Edit, Glob, WebSearch, WebFetch
---

# Article Writer — Autonomous Loop (10k+ Target)

Write a complete, publication-ready article autonomously.

## Arguments

- No arguments: Continue work on the most recent article
- `$ARGUMENTS`: Specific article slug to work on

## Quality Targets

This loop runs until the article meets ALL quality gates:

- **10,000+ words** (8,000 minimum)
- **30+ external links**
- **10+ real-world examples**
- **Strong narrative** (hook, flow, closing)
- **All sections** at 800+ words each
- **70%+ prose ratio** (not bullet-heavy)
- **Headers flow conversationally** (no colon-heavy academic style)

## Editorial Standards

The article should read like a well-crafted long-form piece, not a listicle dressed up with paragraphs. Key principles:

**Narrative over lists.** Every list needs surrounding prose explaining the "why". Lists are punctuation, not the main text.

**Critical thinking over information dumps.** Don't just present facts—analyze them. What's the trade-off? When would you NOT do this?

**Headers that flow.** NO double-barreled titles with colons. Use direct, conversational headers.

- Bad: "Finding Prospects: Where to Look and Who to Target"
- Good: "Where Good Prospects Hide"

**Don't add headers for headers' sake.** Long-form articles should flow as continuous prose, not be chopped into dozens of micro-sections. Only use H3 subheadings when they serve a genuine structural purpose—like introducing a major framework, marking timeline phases, or separating truly distinct topics. If a paragraph can stand on its own with a strong opening sentence, it doesn't need its own header. Aim for 2-4 H3s per major section maximum, not one for every few paragraphs.

## Pipeline

```
RESEARCH (all sections) → OUTLINE → WRITE (section by section) → EMBED MEDIA → REVIEW → EVAL
                                          ↑                                           ↓
                                          └─────── REVISE IF EVAL FAILS ──────────────┘
```

## Execution Steps

### Step 1: Load State

```javascript
// Find active article
const articleDir = findMostRecent('articles/*/article.json')
const article = readJSON(articleDir)
const progress = read('progress.txt')
```

Determine current phase based on state.

### Step 2: Phase Detection

```
IF not all sections have research_complete: true
   → PHASE: RESEARCH
   → Action: Research next incomplete section

ELSE IF outline.md does not exist
   → PHASE: OUTLINE
   → Action: Generate detailed outline

ELSE IF any section has status !== "written" OR word_count < 800
   → PHASE: WRITING
   → Action: Write next incomplete section

ELSE IF article has unprocessed media placeholders
   → PHASE: EMBED
   → Action: Process media embeds

ELSE IF status !== "reviewed"
   → PHASE: REVIEW
   → Action: Review and polish

ELSE
   → PHASE: EVAL
   → Action: Run quality evaluation
```

### Step 3: Execute Phase

#### RESEARCH Phase

For each section without `research_complete: true`:
1. Load section scaffold and research questions
2. Execute 5+ WebSearch queries
3. Find 10+ sources, 3+ real-world examples
4. Write comprehensive research notes
5. Update article.json

**Quality check before moving on:**
- 10+ sources per section
- 3+ real-world examples
- Evidence appropriate to article type collected

#### OUTLINE Phase

1. Read all research notes
2. Create detailed outline with:
   - Section flow and transitions
   - Key points to cover
   - Example placements
   - Evidence locations
3. Write `outline.md`

#### WRITING Phase

For each section without `status: "written"` or `word_count < 800`:

1. Load research notes and outline
2. Read previous sections for continuity
3. Write 800-1500 words following editorial standards:
   - Strong opening hook (no "In this section...")
   - Prose-first body (70%+ flowing text)
   - Real examples woven into narrative
   - Evidence with context and analysis
   - Conversational headers (no colons)
   - Smooth transition to next section
4. Update article.json with word count

**Quality check before moving on:**
- Word count >= 800
- Prose ratio >= 70%
- 3+ external links
- 2+ real-world examples
- Headers conversational

#### EMBED Phase

1. Scan for `<!-- YOUTUBE: -->` placeholders
2. Search for relevant videos
3. Generate responsive embed HTML
4. Add mermaid diagrams where helpful

#### REVIEW Phase

1. Read full article for flow
2. Check prose-to-list ratio (fix if bullet-heavy)
3. Verify headers flow naturally (fix colon-heavy titles)
4. Remove excessive H3 subheadings (aim for 2-4 per section, not dozens)
5. Check all links work
6. Verify factual accuracy
7. Strengthen transitions
8. Polish opening and closing
9. Mark as reviewed

#### EVAL Phase

Run quality evaluation:

```
GATES:
□ Word count >= 8000
□ All sections >= 800 words
□ External links >= 30
□ Real-world examples >= 10
□ Prose ratio >= 70%
□ Headers conversational (no colons)
□ Narrative quality (hook, flow, closing)
```

If ANY gate fails:
1. Identify specific gaps
2. Return to appropriate phase
3. Loop continues

If ALL gates pass:
1. Article is complete
2. Output final summary

### Step 4: Progress Tracking

After EVERY action, update progress.txt:

```
[timestamp] Phase: [phase] | Section: [section-id]
- Action: [what was done]
- Metrics: words=N, links=N, examples=N, prose_ratio=N%
- Status: [PASS/FAIL for quality checks]
- Next: [what comes next]
```

### Step 5: Loop Control

**Continue if:**
- Any quality gate not met
- Any section incomplete
- Research gaps exist
- Prose ratio too low
- Headers need fixing

**Complete when:**
- All sections written (800+ each)
- Total words >= 8000
- Links >= 30
- Examples >= 10
- Prose ratio >= 70%
- Headers verified
- Narrative verified
- EVAL returns PASS

## Output on Completion

```
=============================================
ARTICLE COMPLETE
=============================================

Title: [title]
Type: [article_type]
Location: articles/[slug]/output/article.md

Quality Metrics:
✓ Word count: N / 10,000 target
✓ Sections: N complete (all 800+ words)
✓ External links: N / 30 target
✓ Real-world examples: N / 10 target
✓ Prose ratio: N% / 70% minimum
✓ Headers: Conversational (no colons)
✓ Narrative: Strong hook, flow, closing

The article is ready for publication!
=============================================
```

## Error Handling

If a phase fails repeatedly:
1. Log the error
2. Note specific blocker
3. Attempt alternative approach
4. If still blocked, report to user

## Key Principles

1. **Quality over speed** — Don't rush through phases
2. **Prose over bullets** — Write paragraphs, not listicles
3. **Rich examples** — Every claim needs evidence
4. **Real-world grounding** — Name specific people, companies, studies
5. **Strong narrative** — Story, not just information
6. **Critical thinking** — Analyze, don't just list
7. **No shortcuts** — Every gate must pass
