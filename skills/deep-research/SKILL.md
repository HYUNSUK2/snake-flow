---
name: deep-research
description: >
  Use this skill for ANY question requiring thorough web research. Trigger on "research X",
  "investigate X", "what is X", "explain X", "compare X and Y", or before content generation tasks.
  Orchestrates multiple web-researcher agents in parallel for multi-angle deep research, then
  synthesizes results via report-writer agent. Produces comprehensive reports with citations,
  diagrams, and data. Far superior to single WebSearch — use this PROACTIVELY when the user's
  question needs online information from multiple angles.
---

# Deep Research Orchestrator Skill

## Overview

This skill orchestrates a team of research agents to conduct systematic, multi-angle deep research on any topic. It replaces single superficial searches with a rigorous 4-phase pipeline that dispatches specialized agents in parallel.

## When to Use

**Always use this skill when:**
- User asks to research, investigate, or deeply understand a topic
- User wants current, comprehensive information from multiple sources
- A single web search would be insufficient to answer properly
- Content generation requires real-world data as foundation
- User says "research", "investigate", "analyze", "compare", "what is", "explain"

## Pipeline Overview

```
Phase 1: Plan → Phase 2: Dispatch → Phase 3: Validate → Phase 4: Synthesize
```

## Execution

### Phase 1: Research Planning

Analyze the topic and create a research plan:

1. **Extract dimensions** — identify 3-5 key angles/dimensions of the topic

   Example for "AI in healthcare":
   - Diagnostic AI (radiology, pathology)
   - Treatment recommendation systems
   - Administrative automation
   - Regulatory landscape
   - Market trends and investment

2. **Design search strategy** — for each angle, define:
   - Primary keywords (2-3 per angle)
   - Information types needed (data, cases, opinions, trends)
   - Source priorities (academic, industry, news, community)

3. **Determine agent team**:

   | Topic Signal | Agent Team |
   |---|---|
   | General topic | web-researcher × (number of angles, min 2, max 4) |
   | Mentions GitHub repo or open source | + github-researcher |
   | Needs quantitative analysis | + data-analyst |
   | Always | + report-writer (synthesis phase) |

4. **Set up output directory**:
   - Create `.research/<topic-slug>/` directory
   - Assign each agent a result file: `.research/<topic-slug>/angle-<N>-<name>.md`
   - Final report path: `.research/<topic-slug>/report.md`

### Phase 2: Parallel Agent Dispatch

Launch all research agents in a **single message** using the Agent tool for parallel execution.

For each angle, dispatch a `web-researcher` agent:

```
Agent tool:
  subagent_type: "web-researcher"
  description: "Research angle: <ANGLE_NAME>"
  prompt: |
    Research the following angle of the topic "<TOPIC>":

    **Angle**: <ANGLE_NAME>
    **Keywords to start with**: <KEYWORD_1>, <KEYWORD_2>, <KEYWORD_3>
    **Information types needed**: <data/cases/opinions/trends/comparisons/challenges>
    **Current date**: <TODAY'S DATE>

    Save your findings to: <FILE_PATH>

    Follow your 3-phase research methodology (Broad → Deep → Diverse).
    Ensure all claims have inline citations [citation: Title](URL).
    Include concrete data points and real-world examples.
    Minimum 5 web searches, minimum 2 WebFetch for full content.
```

**Important**: Dispatch ALL research agents in a single message block for true parallel execution.

### Phase 3: Quality Validation

After all agents complete, validate results:

**Synthesis Checklist:**
- [ ] Results from 3-5 different angles?
- [ ] Concrete data points and statistics found?
- [ ] Real-world examples and case studies included?
- [ ] Both positive aspects and challenges covered?
- [ ] Sources are authoritative and current?
- [ ] All results have inline citations?

**If gaps found** — dispatch 1-2 additional web-researcher agents for missing angles.

**Maximum one supplementary round** — do not loop infinitely. If gaps remain after supplementary round, note them in the report.

### Phase 4: Report Synthesis

Dispatch the `report-writer` agent:

```
Agent tool:
  subagent_type: "report-writer"
  description: "Synthesize research: <TOPIC>"
  prompt: |
    Synthesize the following research results into a comprehensive report.

    **Topic**: <TOPIC>
    **Research files to read**:
    - .research/<topic-slug>/angle-1-<name>.md
    - .research/<topic-slug>/angle-2-<name>.md
    - .research/<topic-slug>/angle-3-<name>.md
    [... all angle files]

    **Save final report to**: .research/<topic-slug>/report.md

    Read all files, synthesize findings, resolve contradictions,
    and produce a well-structured report with:
    - Executive summary
    - Mermaid diagrams (at least 1)
    - Comparison tables where appropriate
    - Inline citations preserved from sources
    - "Data → Insight → Implication" analysis chain
    - Conclusion with forward-looking perspective
    - Numbered source list
```

After report-writer completes:
1. Read the generated report
2. Present to user:
   - The report file path
   - A brief summary (3-5 key findings)
   - Offer to dive deeper into any section

## Topic Slug Generation

Convert topic to filesystem-safe slug:
- Lowercase
- Replace spaces and special characters with hyphens
- Collapse multiple hyphens
- Truncate to 50 characters
- Examples:
  - "AI 에이전트 프레임워크 2026 트렌드" → `ai-에이전트-프레임워크-2026-트렌드`
  - "Rust vs Go backend performance" → `rust-vs-go-backend-performance`
  - "Claude Code vs Cursor AI" → `claude-code-vs-cursor-ai`

## Limits & Error Handling

- **Max concurrent research agents**: 4 (plus report-writer in synthesis phase)
- **Max supplementary rounds**: 1 (no infinite loops)
- **Search failure**: Agent retries with reformulated keywords (up to 2 retries per query). If non-English queries fail, agent should fall back to English equivalents.
- **Fetch failure (403/timeout)**: Do NOT retry the same URL. Agent uses search snippets as fallback and tries alternative sources. Note unfetchable URLs in gaps.
- **Agent timeout**: Skip that angle, note the gap in report
- **Empty results**: If an agent returns no useful findings, note the gap and proceed

### Dispatch Best Practices for Reliability

When writing the prompt for each web-researcher agent:

1. **Always provide both English AND non-English search queries** — this ensures fallback if one language fails
2. **Provide 5-8 search queries per agent** (not just 2-3) — more queries = more resilience to individual failures
3. **Include varied query formulations** — mix short keyword queries with longer natural language queries
4. **Remind agents to handle errors gracefully** — include this in the dispatch prompt:
   ```
   If any WebSearch or WebFetch call fails, retry with different keywords or skip and move on.
   Do not stop researching due to individual tool errors. Use search snippets as fallback when
   full page fetch fails.
   ```

## Example Usage

User: `/research 2026 AI 에이전트 프레임워크 트렌드`

Phase 1 output:
```
Angles identified:
1. Major frameworks comparison (LangGraph, CrewAI, AutoGen, etc.)
2. Enterprise adoption trends and use cases
3. Technical architecture patterns (multi-agent, tool-use, memory)
4. Market size and investment landscape
5. Challenges and limitations

Agent team: web-researcher × 4 (angles 1-2 combined, 3, 4, 5)
Output: .research/2026-ai-에이전트-프레임워크-트렌드/
```
