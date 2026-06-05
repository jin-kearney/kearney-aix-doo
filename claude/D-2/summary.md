# D-2 API/Tool 연계 데이터 — 스토리라인 요약 (콘텐츠 순서표)

> storyline.md를 펼치기 전 "무엇을, 어떤 순서로" 담는지 한눈에 보는 목차.
> slide 제목·핵심 메시지는 storyline.md와 일치.

## 한 줄 정의

AI Agent가 외부 시스템·Tool을 안전하고 일관되게 호출할 수 있도록 Tool의 기능·입력·출력·제약 조건을 정의한 명세 체계(Tool/MCP 인터페이스).

## 전체 흐름

LLM은 텍스트 생성기일 뿐 외부 행동을 못 한다는 본질에서 출발해, Tool 명세 3요소(Description·Schema·Risk/권한)가 없으면 무엇이 원리적으로 깨지는지로 필요성을 논증한다(Why) → Agent가 외부 시스템과 상호작용하는 순간 즉시 필요함을 짚고(When) → Function Calling·MCP·Tool 명세·위험도 분류·Registry·Call Log로 "정확히 무엇인지"를 정의한 뒤(What) → Phase 1~6 구축 로드맵·Pain Point AI 해결·시장 솔루션 맵으로 "어떻게 구축하는지"를 제시하고(How) → 5개 KPI로 체계 건전성을 측정하며(KPI) → 단기·중기·장기 확산과 타 주제 연계로 마무리한다(Roadmap).

## 슬라이드 순서표

| # | 섹션 | 슬라이드 제목 | 한 줄 핵심 메시지 |
|---|------|-------------|-----------------|
| 1 | Executive Summary | 한 장 요약 — AI Agent가 외부 세계와 행동하려면 Tool 명세가 필수다 | 외부 시스템을 안전하게 호출하려면 "AI가 읽는 API 설명서"인 Tool 명세 체계가 먼저 갖춰져야 한다 |
| 2 | Why | LLM은 텍스트 생성기다 — 외부 행동은 Tool 명세 없이 불가능하다 | Tool 명세가 LLM과 외부 세계 사이에 개입해 Agent의 "손과 발"이 된다 |
| 3 | Why | 명세 3요소가 없으면 무엇이 원리적으로 깨지는가 | Description·Schema·Risk 중 하나라도 빠지면 Selection Hallucination·Usage Hallucination·Excessive Agency로 귀결된다 |
| 4 | Why | M×N → M+N: 표준 명세가 통합 복잡도를 구조적으로 줄인다 | MCP로 명세를 외부화하면 N개 LLM × M개 Tool 통합이 N+M으로 수렴한다 |
| 5 | Why | Key Objectives — Tool 명세 체계가 달성하는 5가지 목표 | 행동 범위 정의·위험 호출 차단·표준화 재사용·호출 추적·명세 자산화 |
| 6 | When | 적용 시점과 조건 — Agent가 외부 시스템과 상호작용하는 순간, 즉시 필요하다 | 단발 챗봇엔 불필요하나 외부 시스템을 한 번이라도 읽거나 쓰면 필수 조건이 된다 |
| 7 | What | 정의와 범위 — "AI가 읽는 API 설명서"이며, 데이터 자체가 아닌 인터페이스 명세다 | Tool이 처리하는 비즈니스 데이터가 아니라 Tool 호출 인터페이스 명세 체계다 |
| 8 | What | LLM Function Calling / Tool Use 동작 원리 — OpenAI와 Anthropic 표준 비교 | 둘 다 JSON Schema 기반이며 핵심 차이는 parameters vs input_schema 키 이름이다 |
| 9 | What | MCP 개념과 아키텍처 — AI를 위한 USB-C 표준 | MCP는 JSON-RPC 2.0 기반 개방형 프로토콜로 2025년 업계 사실상 표준이 됐다 |
| 10 | What | MCP 3대 Primitive — Tools·Resources·Prompts | 서버가 노출하는 기능은 Tools(실행)·Resources(데이터)·Prompts(템플릿)이며 핵심은 Tools다 |
| 11 | What | Tool Description·Schema 표준 항목 — LLM이 이해하는 명세의 조건 | name·description·input_schema 3개 필수 항목 중 description 품질이 정확도를 가장 좌우한다 |
| 12 | What | Tool 유형 분류 — 위험도 기반 Tier 0~4 체계 | 부작용·되돌림 가능성으로 5단계 분류하면 자율 실행 범위와 승인 게이트를 원칙적으로 설계할 수 있다 |
| 13 | What | Tool Registry·Call Log 개념 — AI를 위한 API 카탈로그와 감사 이력 | Registry는 Agent용 동적 API 카탈로그, Call Log는 호출을 스팬 트리로 기록하는 Observability 단위다 |
| 14 | What | 핵심 용어집 — D-2를 이해하기 위한 8대 개념 | Function Calling·Tool Use·MCP·Tool Schema·Registry·Call Log·Blast Radius·Excessive Agency |
| 15 | How | 구축 전체 로드맵 — Phase 1~6 순서와 산출물 | 인벤토리→명세 표준→Registry→가드레일→Observability→MCP 서버화, 각 산출물이 다음 단계 선행조건 |
| 16 | How | Phase 1~2 상세 — 인벤토리 수집과 Description/Schema 표준화 | Phase 1은 호출 대상 전체 지도를, Phase 2는 LLM이 정확히 호출할 명세 형식을 확보한다 |
| 17 | How | Phase 3~4 상세 — Registry 구축과 위험도·가드레일 정책 | Phase 3은 중앙 레지스트리를, Phase 4는 Tier별 자동실행·HITL·차단 정책으로 안전 경계를 설정한다 |
| 18 | How | Phase 5~6 상세 — Observability와 MCP 서버화 | Phase 5는 OTel 기반 가시성을, Phase 6은 REST API를 MCP로 표준화해 전 프레임워크 호환성을 확보한다 |
| 19 | How | 인증·권한 및 Sandbox — 최소 권한 원칙과 안전한 테스트 환경 | Least Privilege·JIT 자격증명이 Over-privileged 위험을 차단하고 Sandbox가 프로덕션 사고를 예방한다 |
| 20 | How | Pain Point → AI 기반 해결 매핑 | 수기 Description 작성·Tool 선택 저하·반복 변환을 LLM 자동생성·Tool RAG·자동 변환으로 해소한다 |
| 21 | How | Tech Spec — 시장 솔루션 맵 (프로토콜·프레임워크) | MCP·OpenAI FC·A2A 3대 프로토콜과 LangGraph·LlamaIndex·MAF·CrewAI·Vertex ADK·Bedrock 6대 프레임워크 |
| 22 | How | Tech Spec — 시장 솔루션 맵 (Registry·Gateway·API→Tool 변환) | Composio·Glama·Smithery 게이트웨이와 Speakeasy·Stainless·FastAPI-MCP 변환 도구가 운영 인프라를 구성한다 |
| 23 | How | 구축 함정 3가지 — Prompt Injection·Over-privileged Tool·버전 Drift | 3대 함정을 설계 수준에서 차단하지 않으면 프로덕션 사고로 이어진다 |
| 24 | KPI | KPI 관리 지표 — Tool 명세 체계의 건전성을 측정하는 5개 지표 | 성공률·선택 정확도·파라미터 오류율·위험 Tool HITL 준수율·Registry 커버리지로 품질·안전·완결성 측정 |
| 25 | Roadmap | 확산 로드맵 — 단기·중기·장기 및 연계 주제 | 단기 명세·Registry 기반 → 중기 실행형 가드레일·MCP 서버화 → 장기 전사 Gateway·A2A·동적 Discovery |

## 이 발표의 핵심 메시지 3가지

1. LLM은 스스로 외부 세계에 행동할 수 없으며, Tool 명세(Description·Schema·Risk)가 없으면 잘못된 Tool 선택·파라미터 오류·권한 초과 실행이 원리적으로 차단되지 않는다 — Tool 명세는 Agent의 "손과 발"이다.
2. Tool 명세는 코드에 흩어진 설정이 아니라 버전 관리·재사용·발견·감사가 가능한 데이터 자산이며, MCP 같은 표준으로 외부화하면 통합 복잡도가 M×N에서 M+N으로 줄어든다.
3. 구축은 인벤토리→명세 표준→Registry→위험도 가드레일→Observability→MCP 서버화의 6단계로, 수작업 한계는 LLM 자동 Description 생성·Tool RAG·OpenAPI→MCP 자동 변환으로 해소하고 5개 KPI로 건전성을 관리한다.
