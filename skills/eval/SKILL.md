---
name: eval
description: Evaluate article quality against defined gates including prose ratio and header style. Returns PASS or FAIL with specific issues.
allowed-tools: Read, Glob, Bash
---

# Article Quality Evaluation

Evaluate the article and return a quality verdict.

## Load Article State

1. Read `article.json` for targets and gates
2. Read `output/article.md` for actual content
3. Read `progress.txt` for iteration history

## Quality Gates

### Gate 1: Word Count

**Target**: 8,000+ words minimum, 10,000+ ideal

```bash
# Count words in article.md
wc -w output/article.md
```

| Result | Verdict |
|--------|---------|
| < 6,000 | FAIL - Far below minimum |
| 6,000-7,999 | WARN - Below target |
| 8,000-9,999 | PASS - Meets minimum |
| 10,000+ | PASS - Meets target |

### Gate 2: Section Completeness

All sections in `article.json` must have `status: "written"` with `word_count >= 800`.

**Check each section:**
- [ ] Has content in article.md
- [ ] Meets word minimum (800)
- [ ] Has opening hook
- [ ] Has closing transition

| Result | Verdict |
|--------|---------|
| Any section missing | FAIL |
| Any section < 800 words | FAIL |
| All sections complete | PASS |

### Gate 3: External Links

**Target**: 30+ external links

Count unique external URLs in article.md (not internal anchors).

| Result | Verdict |
|--------|---------|
| < 20 | FAIL - Needs more sources |
| 20-29 | WARN - Below target |
| 30+ | PASS |

### Gate 4: Real-World Examples

**Target**: 10+ real-world examples

Look for concrete, named examples with:
- Named person, company, project, study, or organization
- External URL or verifiable reference
- Description of what happened, what they did, or what was found

These can be case studies, company examples, named projects, research findings, historical events, public figures — whatever fits the article's topic.

| Result | Verdict |
|--------|---------|
| < 5 | FAIL - Needs more examples |
| 5-9 | WARN - Below target |
| 10+ | PASS |

### Gate 5: Prose Ratio

**Target**: 70%+ flowing prose, 30% or less structured elements (lists, tables, code)

Count:
- Lines that are bullet points (`- ` or `* `)
- Lines that are numbered items (`1. `, `2. `, etc.)
- Lines in code blocks
- Lines in tables

Compare to total content lines.

| Result | Verdict |
|--------|---------|
| < 60% prose | FAIL - Too listicle-heavy |
| 60-69% prose | WARN - Below target |
| 70%+ prose | PASS |

**Common issues to flag:**
- Sections that are mostly bullet points
- Lists without surrounding narrative
- Concepts explained as bullets rather than paragraphs

### Gate 6: Header Style

**Target**: Conversational headers, NO double-barreled colon style

Scan all `##` and `###` headers for:
- Colons separating topic from description (BAD)
- Academic/formal phrasing (BAD)

**Patterns to FAIL:**
- "Finding Prospects: Where to Look and Who to Target"
- "Email Deliverability: The Technical Foundation"
- "Follow-Ups: Why They Matter and How to Write Them"

**Patterns that PASS:**
- "Where Good Prospects Hide"
- "The Technical Foundation Nobody Talks About"
- "Why Follow-Ups Close 70% of Deals"

| Result | Verdict |
|--------|---------|
| Any colon-split headers | FAIL |
| All conversational | PASS |

### Gate 7: Narrative Quality (Manual Check)

Review the article for:

**Opening (first 200 words):**
- [ ] Has a hook that creates curiosity
- [ ] Establishes why reader should care
- [ ] Promises specific value

**Flow:**
- [ ] Sections connect logically
- [ ] Transitions feel natural
- [ ] Building complexity (simple → advanced)

**Closing:**
- [ ] Summarizes key insights
- [ ] Provides next steps
- [ ] Leaves reader empowered

### Gate 8: Summary Quality

Check the summary in article.json or article intro:
- [ ] Captures main value proposition
- [ ] Mentions key topics covered
- [ ] Would convince someone to read

## Evaluation Output

Create `eval-result.json`:

```json
{
  "evaluated_at": "timestamp",
  "overall_verdict": "PASS|FAIL|WARN",
  "gates": {
    "word_count": {
      "target": 10000,
      "actual": N,
      "verdict": "PASS|FAIL|WARN"
    },
    "sections_complete": {
      "target": "all",
      "actual": "N of M",
      "verdict": "PASS|FAIL"
    },
    "external_links": {
      "target": 30,
      "actual": N,
      "verdict": "PASS|FAIL|WARN"
    },
    "real_world_examples": {
      "target": 10,
      "actual": N,
      "verdict": "PASS|FAIL|WARN"
    },
    "prose_ratio": {
      "target": "70%",
      "actual": "N%",
      "verdict": "PASS|FAIL|WARN"
    },
    "header_style": {
      "colon_headers_found": N,
      "verdict": "PASS|FAIL"
    },
    "narrative_quality": {
      "hook": "PASS|FAIL",
      "flow": "PASS|FAIL",
      "closing": "PASS|FAIL",
      "verdict": "PASS|FAIL"
    }
  },
  "issues": [
    "Issue 1 description",
    "Issue 2 description"
  ],
  "recommendations": [
    "Add more examples in section X",
    "Strengthen transition between Y and Z"
  ]
}
```

## Verdict Logic

```
IF any gate is FAIL:
  overall_verdict = "FAIL"
  Article is NOT ready for completion

ELSE IF any gate is WARN:
  overall_verdict = "WARN"
  Article can complete but has quality gaps

ELSE:
  overall_verdict = "PASS"
  Article meets all quality standards
```

## Append to progress.txt

```
[timestamp] EVALUATION
======================
Overall: [PASS/FAIL/WARN]

Word Count: N/10000 [verdict]
Sections: N/M complete [verdict]
Links: N/30 [verdict]
Examples: N/10 [verdict]
Prose Ratio: N% [verdict]
Headers: [verdict]
Narrative: [verdict]

Issues:
- [issue 1]
- [issue 2]

Recommendations:
- [rec 1]
- [rec 2]

Decision: [COMPLETE / NEEDS REVISION]
======================
```

## Return Value

For use by Stop hook, output final line:

```
EVAL_RESULT: [PASS|FAIL|WARN]
```

If FAIL, the Stop hook should prevent completion and report issues.
If WARN, allow completion but note quality gaps.
If PASS, article meets all quality standards.
