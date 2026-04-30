---
description: 컨설팅 프레임워크 기반 종합 분석 (SWOT, PEST, Porter's 등). 시장/산업/경쟁/전략 분석 리포트 생성.
argument-hint: <analysis topic>
---

Use the consulting-analysis skill to produce a consulting-grade analytical report.

Topic: $ARGUMENTS

Follow the consulting-analysis skill's pipeline:
1. Design analysis framework (select 2-4 professional frameworks, create chapter skeleton with data requirements)
2. Dispatch web-researcher agents in parallel to collect data per chapter requirements
3. Optionally dispatch data-analyst for quantitative analysis
4. Synthesize via report-writer into consulting-grade report with:
   - Mermaid diagrams and comparison tables
   - "Data → Insight → Strategic Implication" analysis chain
   - Professional consulting tone (McKinsey/BCG style)
   - 200+ word analytical paragraphs per section
5. Save to `.research/<slug>/` and present report path with executive summary
