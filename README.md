# research-lite

멀티앵글 딥 리서치 + 컨설팅 프레임워크 분석 파이프라인.
두 개의 슬래시 커맨드(`/research`, `/analyze`)와 그에 필요한 스킬·에이전트로 구성된 경량 플러그인.

Multi-angle deep research and consulting framework analysis pipeline — a lightweight
plugin packaging two slash commands (`/research`, `/analyze`) with the skills and
agents they need.

## Installation

```
# 1. Add the marketplace
/plugin marketplace add HYUNSUK2/snake-flow

# 2. Install the plugin
/plugin install research-lite@research-lite-marketplace
```

로컬 클론으로 바로 테스트할 때:

```
/plugin marketplace add /path/to/snake-flow
/plugin install research-lite@research-lite-marketplace
```

## Commands

| Command | Description |
|---------|-------------|
| `/research <topic>` | 주제에 대한 멀티앵글 딥 리서치. 3-5개 각도로 web-researcher를 병렬 디스패치 후 report-writer가 종합 리포트 생성. |
| `/analyze <topic>` | 컨설팅 프레임워크(SWOT / PEST / Porter's / TAM-SAM-SOM 등) 기반 분석 리포트 생성. |

결과물은 작업 디렉토리의 `.research/<topic-slug>/` 하위에 저장됩니다.

## Examples

```
/research 2026 AI 에이전트 프레임워크 트렌드
/research Rust vs Go backend performance
/analyze 한국 전기차 시장 경쟁 구도
/analyze Notion vs Obsidian 컨슈머 포지셔닝
```

실행하면 다음과 같은 산출물이 생성됩니다:

```
.research/2026-ai-에이전트-프레임워크-트렌드/
├── angle-1-frameworks-comparison.md   # web-researcher 결과 (인용 포함)
├── angle-2-enterprise-adoption.md
├── angle-3-architecture-patterns.md
├── angle-4-market-investment.md
├── angle-5-challenges-limitations.md
└── report.md                          # report-writer 최종 리포트
```

> 주제 복잡도에 따라 **3-5개 각도**가 자동 결정됩니다 (동시 디스패치는 최대 4개, 5개 이상이면 인접 각도가 묶여서 처리).

`report.md`에는 다음이 포함됩니다:
- Executive Summary
- Mermaid 다이어그램 (architecture / timeline / comparison 등)
- 비교 표
- "Data → Insight → Implication" 구조의 분석 단락
- 인라인 인용(`[citation: Title](URL)`)과 출처 목록

## Skills

| Skill | Used By |
|-------|---------|
| `deep-research` | `/research` 의 파이프라인 (Plan → Dispatch → Validate → Synthesize → Save) |
| `consulting-analysis` | `/analyze` 의 2단계 파이프라인 (Framework Design → Data Collection → Synthesis) |

## Agents

| Agent | Role |
|-------|------|
| `web-researcher` | 3-Phase (Broad → Deep → Diverse) 방법론으로 멀티앵글 웹 리서치 수행. 인용 포함 결과를 파일로 저장. |
| `report-writer` | 여러 에이전트 결과를 통합·중복 제거·모순 해결 후 Mermaid 다이어그램과 인용을 포함한 최종 리포트 작성. |

## Requirements

- [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) — Skill / Agent / Plugin 시스템을 지원하는 버전
- 인터넷 접속 — `WebSearch` / `WebFetch` 도구를 통해 실시간 웹 데이터 수집

## License

MIT
