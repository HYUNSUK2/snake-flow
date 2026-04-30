---
name: consulting-analysis
description: >
  Professional consulting-grade analysis using structured frameworks (SWOT, PEST, Porter's Five Forces,
  TAM-SAM-SOM, BCG Matrix, etc.). Use when user requests market analysis, competitive analysis,
  industry research, brand analysis, consumer insights, investment due diligence, or any
  strategic/analytical report. Operates in two phases: (1) framework design with chapter skeleton
  and data requirements, (2) data collection via parallel agents + final report synthesis.
  Trigger on "analyze X market", "competitive analysis", "industry research", "strategic analysis",
  "market entry", "SWOT", or any consulting-style analysis request.
---

# Consulting Analysis Orchestrator Skill

## Overview

Produces consulting-grade analytical reports by combining professional analysis frameworks with multi-agent deep research. Adapted from DeerFlow's two-phase consulting methodology for Claude Code.

## When to Use

- Market analysis, competitive intelligence, industry research
- Brand strategy, consumer profiling, investment due diligence
- Any request for structured strategic analysis
- User mentions specific frameworks (SWOT, PEST, Porter's, etc.)
- User says "analyze", "market analysis", "competitive landscape", "strategic assessment"

## Pipeline

```
Phase 1: Framework Design → Agent Dispatch (parallel) → Phase 2: Report Generation
```

## Phase 1: Analysis Framework Design

### Step 1.1: Understand the Subject

- Identify **core entity** (market, brand, product, industry, consumer segment)
- Determine **analytical domain** (marketing, finance, industry, competitive, consumer, investment)
- Define **scope** (geography, time range, industry segment)
- Formulate 1-3 **core research questions**

### Step 1.2: Select Frameworks (2-4)

Choose from these categories based on domain:

**Strategic & Environmental:**
| Framework | Best For |
|---|---|
| SWOT | Brand assessment, competitive positioning, strategic planning |
| PEST/PESTEL | Macro-environment scanning, market entry, policy impact |
| Porter's Five Forces | Industry competitive landscape, entry barriers, profit margins |
| VRIO | Core competency assessment, resource advantage |

**Market & Growth:**
| Framework | Best For |
|---|---|
| STP (Segmentation, Targeting, Positioning) | Market segmentation, brand positioning |
| BCG Matrix | Product portfolio management, resource allocation |
| TAM-SAM-SOM | Market sizing, opportunity quantification |
| Product Life Cycle | Product strategy, market timing |

**Consumer & Behavioral:**
| Framework | Best For |
|---|---|
| Consumer Decision Journey | Behavior path mapping, touchpoint optimization |
| AARRR Funnel | User growth analysis, conversion optimization |
| RFM Model | Customer value segmentation |
| Jobs-to-be-Done | Demand insight, product innovation |

**Financial:**
| Framework | Best For |
|---|---|
| DuPont Analysis | Profitability decomposition, financial health |
| Comparable Company Analysis | Relative valuation, peer benchmarking |

**Competitive:**
| Framework | Best For |
|---|---|
| Benchmarking | Competitor gap analysis, best practices |
| Strategic Group Mapping | Competitive landscape visualization |
| Value Chain Analysis | Cost advantage, differentiation opportunities |
| Blue Ocean Strategy | Differentiated innovation, new market creation |

**Selection principles:**
1. Domain-first: pick most relevant frameworks
2. Complementary: macro + micro, not overlapping
3. Depth over breadth: 2 deep > 6 shallow
4. Data-feasible: must be supportable by web research

### Step 1.3: Design Chapter Skeleton

For each chapter:
- **Chapter Title** — professional, concise, subject-based
- **Analysis Objective** — what this chapter aims to reveal
- **Analysis Logic** — which framework, reasoning chain
- **Core Hypothesis** — preliminary hypothesis to validate
- **Data Requirements** — specific metrics, search keywords, priority (P0/P1/P2)

### Step 1.4: Present Framework Briefly

Show the user a brief summary of chosen frameworks and chapters, then proceed to data collection without waiting for approval (unless the user previously asked to review plans first).

## Data Collection: Parallel Agent Dispatch

Based on framework data requirements:

1. **Group data requirements by research angle** — cluster related search keywords
2. **Dispatch agents in parallel** (single message):

```
web-researcher × N → .research/<topic-slug>/angle-<N>-<name>.md
  Each agent gets: angle name, keywords, information types, current date

data-analyst (if quantitative data available) → .research/<topic-slug>/angle-data.md
  Gets: data files/content to analyze, analysis objectives
```

3. **Validate** — check all P0 data requirements have results

## Phase 2: Report Synthesis

Dispatch `report-writer` with specific consulting report instructions:

```
report-writer → .research/<topic-slug>/report.md
  prompt: |
    Synthesize research into a consulting-grade analytical report.

    **Analysis Framework**: [framework summary with chapters]
    **Research files**: [list all angle files]

    Report structure:
    1. Abstract — executive summary with key takeaways
    2. Introduction — background, objectives, methodology
    3-N. Body chapters — per chapter skeleton
      Each sub-chapter follows: Visual Anchor → Data Contrast → Integrated Analysis
      Each sub-chapter ends with 200+ word analytical paragraph
      Every insight follows: Data → User Psychology → Strategy Implication
    N+1. Conclusion — objective synthesis, flowing prose, no bullet points
    N+2. Sources — with URLs

    Requirements:
    - Professional consulting tone (McKinsey/BCG style)
    - Mermaid diagrams and comparison tables in each chapter
    - Inline citations [citation: Title](URL) throughout
    - No hallucination — only use data from research files
    - Bold key numbers and viewpoints
    - "Data → Insight → Strategic Implication" chain for every analytical paragraph
```

## Quality Standards

- **Zero hallucination**: all data traceable to research sources
- **"Data → User Psychology → Strategy Implication"** chain for every insight
- **Visual Anchor** in every section (Mermaid diagram or comparison table)
- **200+ word analytical paragraph** closing each major section
- **Inline citations** throughout
- **Professional tone**: authoritative, objective, data-driven

## Output

Save all files to `.research/<topic-slug>/`:
- `angle-*.md` — individual research results
- `report.md` — final consulting report

Present: report file path + executive summary of key findings

## Limits

- Max 4 concurrent research agents + report-writer
- Max 1 supplementary round for P0 data gaps
- If framework data cannot be collected, note gaps explicitly in report
