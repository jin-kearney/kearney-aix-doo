# D-2 API/Tool 연계 데이터 — KPI / Roadmap 리서치

> 주제: AI Agent가 외부 시스템과 Tool을 안전하게 호출할 수 있도록 기능, 입력, 출력, 제약 조건을 정의한 명세 체계 (Tool/MCP 인터페이스)
> 작성일: 2026-06-05

---

## 1. 관리 KPI 후보 (7개) — 근거 및 측정 방법

### 1-1. Tool Call 성공률 (Tool Call Success Rate)

| 항목 | 내용 |
|------|------|
| **지표명** | Tool Call 성공률 |
| **정의(산식)** | (성공적으로 완료된 Tool 호출 수) ÷ (전체 Tool 호출 시도 수) × 100 |
| **측정 목적** | Agent가 외부 시스템과의 연계에서 얼마나 안정적으로 동작하는지 전반적 건전성 파악 |
| **목표 방향** | ↑ (높을수록 좋음, P95 기준 1% 미만 오류율 권장) |
| **기대효과** | 장애·재시도 감소, 운영비용 절감, Agent 신뢰도 향상 |

**근거**: UptimeRobot AI Agent Monitoring 가이드에서는 "P50, P95, P99 latency와 1% 미만 error rate"를 세 기둥 모니터링 지표의 핵심으로 제시. Galileo의 LLM Monitoring Best Practices에서도 tool success를 핵심 signal로 명시.
- 출처: https://uptimerobot.com/knowledge-hub/monitoring/ai-agent-monitoring-best-practices-tools-and-metrics/

---

### 1-2. Tool 선택 정확도 (Tool Correctness / Tool Selection Accuracy)

| 항목 | 내용 |
|------|------|
| **지표명** | Tool 선택 정확도 |
| **정의(산식)** | (Agent가 특정 Task에서 올바른 Tool을 호출한 횟수) ÷ (전체 Tool 호출 판정 횟수) × 100 |
| **측정 목적** | Agent가 문맥에 적합한 Tool을 선택하는지 평가 (eval harness 기반) |
| **목표 방향** | ↑ (벤치마크 목표: 93~97% 이상) |
| **기대효과** | 불필요한 Tool 호출(비용 낭비) 감소, Task 완료율 향상 |

**근거**: Confident AI의 LLM Agent Evaluation Complete Guide (2026)에서 Tool Correctness를 결정론적(Deterministic) 지표로 분류하며, "Agent가 주어진 입력에 대해 필요한 Tool을 올바르게 호출했는지 검증"하는 핵심 컴포넌트 지표로 정의. DeepEval/Galileo의 Agentic Evaluations(2025년 1월 출시)에서 93~97% 정확도 달성 사례 보고.
- 출처: https://www.confident-ai.com/blog/llm-agent-evaluation-complete-guide
- 출처: https://www.getmaxim.ai/articles/top-5-tools-to-evaluate-and-observe-ai-agents-in-2025/

---

### 1-3. 파라미터/스키마 오류율 (Argument Error Rate)

| 항목 | 내용 |
|------|------|
| **지표명** | 파라미터·스키마 오류율 (Argument Error Rate) |
| **정의(산식)** | (잘못된 입력 파라미터로 실패한 Tool 호출 수) ÷ (전체 Tool 호출 시도 수) × 100 |
| **측정 목적** | Tool Schema 명세의 완결성 및 Agent의 파라미터 생성 품질 점검 |
| **목표 방향** | ↓ (낮을수록 좋음) |
| **기대효과** | 스키마 오류로 인한 재시도·장애 감소, Tool Registry 품질 향상 |

**근거**: Confident AI 가이드에서 Argument Correctness를 별도 지표(LLM-as-judge)로 분리 정의: "Did the agent call that tool correctly?". LangSmith 및 Langfuse 양 플랫폼 모두 tool call별 입력 파라미터 오류를 structured trace로 추적하고 P50/P99 latency + error rate 대시보드에서 관리.
- 출처: https://www.confident-ai.com/blog/llm-agent-evaluation-complete-guide
- 출처: https://www.langchain.com/langsmith/observability

---

### 1-4. 평균 Tool 호출 지연 (Tool Call Latency — P95)

| 항목 | 내용 |
|------|------|
| **지표명** | Tool 호출 평균 지연 (P95 Latency) |
| **정의(산식)** | 상위 95번째 백분위 Tool 호출 응답 시간 (ms) |
| **측정 목적** | 외부 API 연계의 응답성 확보 및 병목 식별 |
| **목표 방향** | ↓ (목표 임계값: 서비스별 설정, 가드레일 판단 LLM은 1~5초 허용) |
| **기대효과** | Agent 응답 속도 향상, 사용자 경험 개선, 가드레일 latency 분리 최적화 |

**근거**: UptimeRobot 가이드에서 P50/P95/P99 latency를 3개 필수 메트릭으로 명시. Galileo의 가이드라인에서 "inline LLM-judge 호출은 1~2초(flash 모델)~3~5초(large 모델) 범위이며 비동기 샘플링 처리 권장"이라고 세분화.
- 출처: https://galileo.ai/blog/ai-agent-guardrails-framework
- 출처: https://uptimerobot.com/knowledge-hub/monitoring/ai-agent-monitoring-best-practices-tools-and-metrics/

---

### 1-5. 위험 Tool 사람 승인 준수율 (High-Risk Tool HITL Compliance Rate)

| 항목 | 내용 |
|------|------|
| **지표명** | 위험 Tool 사람 승인 준수율 (HITL Compliance Rate) |
| **정의(산식)** | (사람 승인을 거쳐 실행된 위험 Tool 호출 수) ÷ (승인 정책 대상 위험 Tool 호출 수) × 100 |
| **측정 목적** | 가드레일·승인 정책이 실제로 적용되는지 거버넌스 관점 확인 |
| **목표 방향** | ↑ (100% 목표; 미준수 건은 즉시 알람) |
| **기대효과** | 오동작·데이터 유출 위험 감소, 감사(Audit) 추적 용이, 규정 준수 |

**근거**: Blaxel/W&B 가이드에서 HITL(Human-in-the-Loop) 트리거 조건(신뢰 점수 임계값 초과·정책 규칙·불확실성 초과)을 정의하고 "사람 개입율(Human intervention rate)은 알려진 패턴에선 감소해야 하나 신규 상황에선 높게 유지"라 명시. Cox Automotive 사례에서 circuit breaker(비용·대화 횟수 임계값)를 핵심 운영 지표로 활용.
- 출처: https://wandb.ai/site/articles/guardrails-for-ai-agents/
- 출처: https://blaxel.ai/blog/guardrails-for-ai-agents

---

### 1-6. Tool Registry 커버리지 (Tool Registry Coverage)

| 항목 | 내용 |
|------|------|
| **지표명** | Tool Registry 커버리지 |
| **정의(산식)** | (표준 description·JSON Schema·버전 메타데이터가 등록된 Tool 수) ÷ (운영 중인 전체 Tool 수) × 100 |
| **측정 목적** | Agent가 안전하게 사용할 수 있는 Tool 명세의 완결성 관리 |
| **목표 방향** | ↑ (단계적 100% 목표) |
| **기대효과** | Agent의 올바른 Tool 선택 가능, 스키마 drift 예방, 거버넌스 투명성 |

**근거**: TrueFoundry의 AI Agent Registry 가이드에서 "중앙화된 레지스트리는 관리자가 승인된 Tool 카탈로그를 정의하고 Agent가 게이트웨이를 통해 검증된 Tool을 발견·활용"한다고 명시. MCP Registry(2025년 9월 InfoQ 발표)도 같은 맥락에서 등장 — verification mechanism, security/quality standards 충족 여부 등록.
- 출처: https://www.truefoundry.com/blog/ai-agent-registry
- 출처: https://www.infoq.com/news/2025/09/introducing-mcp-registry/

---

### 1-7. Tool 재사용률 (Tool Reuse Rate)

| 항목 | 내용 |
|------|------|
| **지표명** | Tool 재사용률 |
| **정의(산식)** | (2개 이상의 서로 다른 Agent에서 호출된 Tool 수) ÷ (Registry에 등록된 전체 Tool 수) × 100 |
| **측정 목적** | 중복 Tool 개발 방지, 플랫폼 Tool의 실질적 공유 정도 파악 |
| **목표 방향** | ↑ (높을수록 투자 대비 효율 향상) |
| **기대효과** | 개발·유지보수 비용 절감, 일관된 Tool 동작 보장, MCP 서버 공용화 촉진 |

**근거**: TrueFoundry 레지스트리 가이드에서 "레지스트리는 기존 Agent(Tool)의 재사용을 촉진하여 팀이 필요한 기능을 이미 가진 Agent를 빠르게 발견"한다고 정의. 게이트웨이를 통해 usage count·응답 시간·성공/실패율 텔레메트리 수집 가능.
- 출처: https://www.truefoundry.com/blog/ai-agent-registry
- 출처: https://www.truefoundry.com/blog/top-agent-gateways

---

## 2. KPI 선정 요약표 (7개 후보 → 최종 5개 권장)

| # | 지표명 | 산식 요약 | 목표 방향 | 우선순위 | 연계 주제 |
|---|--------|-----------|-----------|----------|-----------|
| 1 | Tool Call 성공률 | 성공 호출 / 전체 호출 | ↑ | ★★★ 필수 | C-2 품질/권한 |
| 2 | Tool 선택 정확도 | 올바른 Tool 선택 / 전체 판정 | ↑ | ★★★ 필수 | E-3 평가데이터 |
| 3 | 파라미터 오류율 | 파라미터 오류 호출 / 전체 호출 | ↓ | ★★★ 필수 | D-3 Prompt/Harness |
| 4 | Tool 호출 지연(P95) | 95th percentile latency (ms) | ↓ | ★★☆ 권장 | - |
| 5 | 위험 Tool HITL 준수율 | 승인 실행 / 승인 대상 | ↑ | ★★★ 필수 | C-2 품질/권한 |
| 6 | Tool Registry 커버리지 | 등록 완결 Tool / 전체 Tool | ↑ | ★★☆ 권장 | E-4 Feedback Loop |
| 7 | Tool 재사용률 | 다중 Agent 공유 Tool / 전체 Tool | ↑ | ★☆☆ 선택 | - |

> **최종 5개 권장**: 1, 2, 3, 5, 6 (Tool 호출 지연은 SLA 설정 이후 단계적 도입 권장)

---

## 3. 확산 로드맵 (성숙도 단계별)

### Phase 1 — 단기 (0~6개월): 기초 표준화·가시성 확보

**목표**: 핵심 read-only Tool 명세 표준화, Registry 구축, 기본 로깅

| 과제 | 세부 내용 |
|------|-----------|
| Tool 명세 표준화 | 모든 Tool에 name / description / JSON Schema / 버전 태그 의무화. OpenAPI 또는 MCP Tool Definition 형식 채택 |
| Tool Registry MVP | 내부 Tool 카탈로그 구축. 등록·검색·버전 이력 관리. 최소한 read-only(조회) Tool부터 시작 |
| 기본 로깅·추적 | LangSmith 또는 Langfuse 연동. Tool 호출마다 tool name, input params, output, latency, error code 구조화 기록 |
| KPI 대시보드 초기 | Tool Call 성공률·파라미터 오류율·지연 P95 측정 시작 |

**연계 주제**: D-3 Prompt/Harness (Tool description 품질이 Agent prompt에 직결), C-2 품질/권한 (Tool 접근 권한 초기 정책 수립)

**근거**: LangSmith는 "모든 LLM 호출, Tool 호출, 중간 추론 단계"를 추적하며 custom 대시보드로 latency P50/P99, error rate, cost를 관리. Langfuse도 동일 구조(traces → observations → tool calls).
- 출처: https://www.langchain.com/langsmith/observability
- 출처: https://langfuse.com/integrations/frameworks/langchain

---

### Phase 2 — 중기 (6~18개월): 실행형 Tool + 평가 체계 + MCP 서버화

**목표**: 쓰기·실행형(write/action) Tool 가드레일 적용, Tool 평가 자동화, MCP 서버 표준화

| 과제 | 세부 내용 |
|------|-----------|
| 실행형 Tool 가드레일 | 위험도 분류(read/write/critical) 정책 수립. write/critical Tool에 HITL 승인 흐름 적용. Circuit breaker(비용·실행 횟수 임계값) 구현 |
| MCP 서버화 | 기존 내부 API를 MCP Server로 래핑. Tool schema를 MCP Tool Definition으로 통일. MCP Registry에 등록 |
| Tool Retrieval 도입 | Tool 수가 수십 개 이상이면 semantic search 기반 tool retrieval(BM25/vector) 도입. Anthropic Tool Search Tool 방식 참조 |
| 평가 Harness 연계 (E-3) | DeepEval / Confident AI 등 eval framework로 Tool Correctness·Argument Correctness 자동 측정. 회귀 테스트 파이프라인 구축 |
| Tool 선택 정확도 추적 | CI/CD에 Tool eval 단계 삽입. PR마다 Tool 정확도 리포트 자동 생성 |

**연계 주제**: E-3 평가데이터 (Tool eval golden dataset 관리), D-3 Prompt/Harness (Tool description 품질과 선택 정확도 상관관계 분석), C-2 품질/권한 (write Tool 접근 권한 세분화)

**근거**:
- Anthropic Tool Search Tool: BM25 + semantic similarity 방식으로 수백~수천 개 Tool을 동적 발견. context window 오염 없이 필요한 Tool만 로드.
- MCP Registry (2025년 9월 InfoQ): verification mechanism, security/quality standards 기반 등록.
- DeepEval ToolCorrectnessMetric: 순서 독립적 채점, input parameter 비율 점수, output accuracy 지원.
- 출처: https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool
- 출처: https://www.infoq.com/news/2025/09/introducing-mcp-registry/
- 출처: https://www.confident-ai.com/blog/llm-agent-evaluation-complete-guide

---

### Phase 3 — 장기 (18개월+): 전사 MCP 게이트웨이 + 멀티 에이전트 + 동적 발견

**목표**: 전사 MCP Gateway 거버넌스, Agent-to-Agent(A2A) 연계, 동적 Tool Discovery, Feedback Loop 통합

| 과제 | 세부 내용 |
|------|-----------|
| 전사 MCP Gateway | 모든 Tool 호출이 단일 게이트웨이를 경유. 인증(SSO 통합·Cross-App Access), 감사 로그, 정책 집행 중앙화. MCP 2026 Enterprise Readiness 로드맵 항목: audit trails, gateway/proxy patterns, configuration portability 구현 |
| 동적 Tool Discovery | Agent Card / Server Card(.well-known URL) 기반 Tool·Agent 능력 자동 발견. MCP Server Card WG이 표준화 추진 중(2026 로드맵). semantic search로 수천 개 Tool 중 필요한 것만 동적 로드 |
| A2A(Agent-to-Agent) 연계 | Google A2A 프로토콜 또는 MCP Agents WG(SEP-1686 Tasks primitive) 기반으로 Agent 간 Tool 위임·핸드오프 표준화. 계층적 Agent 조직(coordinator → specialist), namespace isolation 구현 |
| Feedback Loop 연계 (E-4) | Tool 호출 결과(성공/실패, 사용자 피드백)를 E-4 Feedback Loop로 연결. Tool description·schema 자동 개선 사이클 구축. 반복 오류율 감소 추세 모니터링 |
| 거버넌스 성숙 | Gartner 전망: 2026년 75% API Gateway 벤더, 50% iPaaS 벤더가 MCP 기능 탑재. 전사 Tool 생애주기(등록→승인→운영→폐기) 관리 프로세스 수립 |

**연계 주제**: E-4 Feedback Loop (Tool 성능 데이터 기반 자동 개선), C-2 품질/권한 (Gateway 수준 권한 정책), D-3 Prompt/Harness (Agent Card의 Tool 설명이 프롬프트에 반영), E-3 평가데이터 (A2A 시나리오 eval 데이터셋 확장)

**근거**:
- MCP 공식 로드맵(2026-03-05): Transport Evolution, Agent Communication(SEP-1686), Enterprise Readiness(audit trails·gateway·auth), Extensions Ecosystem이 4대 우선순위.
- A2A 프로토콜: 50+ launch partners. Agent Card로 능력 광고, JSON-RPC 2.0/HTTP 기반. "완전한 엔터프라이즈 에이전트 스택은 MCP(tool access) + A2A(agent coordination)"으로 수렴.
- Gartner 예측: 75% API gateway vendors + 50% iPaaS vendors에 MCP 기능 탑재(by 2026).
- 출처: https://modelcontextprotocol.io/development/roadmap
- 출처: https://a2a-mcp.org/blog/mcp-2026-roadmap
- 출처: https://www.programming-helper.com/tech/agent-to-agent-protocol-2026-google-a2a-standard

---

## 4. 로드맵 요약 다이어그램 (텍스트)

```
Phase 1 (0~6개월)      Phase 2 (6~18개월)         Phase 3 (18개월+)
──────────────────    ──────────────────────────   ──────────────────────────────
Tool 명세 표준화    → 실행형 Tool + 가드레일     → 전사 MCP Gateway
Tool Registry MVP   → MCP 서버화·Registry 연동  → A2A 연계·동적 발견
기본 로깅/추적      → Tool Retrieval 도입        → Agent Card 기반 자동 발견
KPI 측정 시작       → 평가 Harness 연계 (E-3)   → Feedback Loop 연계 (E-4)
                    → HITL 승인 흐름             → 거버넌스 성숙·감사 추적
```

### 다른 AI-Ready Data 주제와의 연계 요약

| 연계 주제 | D-2와의 접점 |
|-----------|-------------|
| **D-3 Prompt/Harness** | Tool description 품질이 Agent prompt 구성에 직접 영향. Tool schema 변경 시 harness 자동 업데이트 |
| **E-3 평가데이터** | Tool Correctness / Argument Correctness eval을 위한 golden dataset 관리. CI/CD eval 파이프라인 공유 |
| **E-4 Feedback Loop** | Tool 호출 결과(성공/실패율, 사용자 피드백)를 Tool schema 자동 개선에 반영. 반복 오류율 추세 관리 |
| **C-2 품질/권한** | Tool 접근 권한 정책(read/write/critical 분류), 위험 Tool HITL 승인, Gateway 수준 인증·감사 |

---

## 5. 출처

1. Confident AI — LLM Agent Evaluation Metrics in 2026 (Tool Calling, Task Completion, Reasoning, Trace-Based Evals)
   https://www.confident-ai.com/blog/llm-agent-evaluation-complete-guide

2. DeepEval — AI Agent Evaluation Metrics
   https://deepeval.com/guides/guides-ai-agent-evaluation-metrics

3. Galileo — Agent Observability and LLM Monitoring Best Practices
   https://galileo.ai/blog/effective-llm-monitoring

4. Galileo — Essential Framework for AI Agent Guardrails
   https://galileo.ai/blog/ai-agent-guardrails-framework

5. UptimeRobot — AI Agent Monitoring: Best Practices, Tools & Metrics for 2026
   https://uptimerobot.com/knowledge-hub/monitoring/ai-agent-monitoring-best-practices-tools-and-metrics/

6. AWS — Evaluating AI agents: Real-world lessons from building agentic systems at Amazon
   https://aws.amazon.com/blogs/machine-learning/evaluating-ai-agents-real-world-lessons-from-building-agentic-systems-at-amazon/

7. Getmaxim — Top 5 Tools to Evaluate and Observe AI Agents in 2025
   https://www.getmaxim.ai/articles/top-5-tools-to-evaluate-and-observe-ai-agents-in-2025/

8. TrueFoundry — What is AI Agent Registry
   https://www.truefoundry.com/blog/ai-agent-registry

9. TrueFoundry — Top Agent Gateways 2025
   https://www.truefoundry.com/blog/top-agent-gateways

10. LangChain — LangSmith AI Agent & LLM Observability Platform
    https://www.langchain.com/langsmith/observability

11. Langfuse — LangChain Tracing & Callbacks Integration
    https://langfuse.com/integrations/frameworks/langchain

12. InfoQ — Introducing the MCP Registry (September 2025)
    https://www.infoq.com/news/2025/09/introducing-mcp-registry/

13. Model Context Protocol — Official Roadmap (Updated 2026-03-05)
    https://modelcontextprotocol.io/development/roadmap

14. MCP Blog — The 2026 MCP Roadmap
    https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/

15. Anthropic — Tool Search Tool (Claude API Documentation)
    https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool

16. A2A MCP — MCP Roadmap 2026: Official Priorities for Model Context Protocol Scalability & AI Agents
    https://a2a-mcp.org/blog/mcp-2026-roadmap

17. Programming Helper — Agent to Agent Protocol 2026: Google's A2A Standard
    https://www.programming-helper.com/tech/agent-to-agent-protocol-2026-google-a2a-standard

18. Zylos Research — Agent Interoperability Protocols 2026: MCP, A2A, ACP and the Path to Convergence
    https://zylos.ai/research/2026-03-26-agent-interoperability-protocols-mcp-a2a-acp-convergence

19. Wandb — Understanding guardrails for AI agents
    https://wandb.ai/site/articles/guardrails-for-ai-agents/

20. Blaxel — What Are Guardrails for AI Agents?
    https://blaxel.ai/blog/guardrails-for-ai-agents

21. ZenML — What 1,200 Production Deployments Reveal About LLMOps in 2025
    https://www.zenml.io/blog/what-1200-production-deployments-reveal-about-llmops-in-2025

22. Arcade.dev — Agentic AI Adoption Trends & Enterprise ROI Statistics for 2025
    https://www.arcade.dev/blog/agentic-framework-adoption-trends/
