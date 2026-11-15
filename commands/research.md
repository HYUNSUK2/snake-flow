---
description: 주제에 대한 멀티앵글 딥 리서치 실행. 여러 에이전트가 병렬로 웹 리서치 후 종합 리포트 생성.
argument-hint: <research topic>
---

Use the deep-research skill to conduct comprehensive multi-angle research on the following topic:

$ARGUMENTS

Follow the deep-research skill's 4-phase pipeline:
1. Plan — analyze the topic and plan research angles (3-5 dimensions)
2. Dispatch — dispatch web-researcher agents in parallel (one per angle) using the Agent tool in a single message
3. Validate — validate research quality against the synthesis checklist, fill gaps if needed (max 1 supplementary round)
4. Synthesize — synthesize all results via report-writer agent into final report with Mermaid diagrams and citations

Save all results to `.research/<topic-slug>/` directory.
Present the final report path with a brief summary of key findings.
