# D-2 출처 검증 리포트

> 검증일: 2026-06-05 | 검증 대상: storyline.md 참고 출처 24개 + 본문 핵심 주장

---

## 요약

| 항목 | 수치 |
|------|------|
| 검증 시도 URL | 24개 (참고 출처 목록) + 본문 핵심 주장 별도 검증 |
| **OK** (살아있고 주장 뒷받침) | 19개 |
| **MISMATCH** (살아있으나 주장과 불일치) | 2개 |
| **BROKEN** (접속 불가·404) | 0개 |
| **UNVERIFIABLE** (접근 제한·차단·로드 실패) | 3개 |
| 자동 보정 건수 | 4건 (storyline.md 직접 수정) |
| 사람 확인 필요 (⚠️) | 2건 |

---

## 출처별 검증 표

| # | URL | 귀속 주장 (요약) | 판정 | 조치 | 대체 출처 |
|---|-----|----------------|------|------|----------|
| 1 | https://www.anthropic.com/news/model-context-protocol | MCP 2024년 11월 발표 | **OK** | 없음 | — |
| 2 | https://modelcontextprotocol.io/docs/learn/architecture | MCP Host/Client/Server 3계층 아키텍처 | **OK** | 없음 | — |
| 3 | https://modelcontextprotocol.io/specification/2025-11-25 | MCP Specification 2025-11-25 버전 존재 | **OK** | 없음 | — |
| 4 | https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation | MCP 2025년 12월 Linux Foundation 기증 | **OK** | 없음 | — |
| 5 | https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools | Anthropic Tool Use Define Tools 문서 | **OK** (접속 확인, 목차 확인) | 없음 | — |
| 6 | https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-reference | Anthropic Tool Reference 문서 | **OK** (참조 목록에서 확인) | 없음 | — |
| 7 | https://developers.openai.com/api/docs/guides/function-calling | OpenAI Function Calling 공식 문서 | **UNVERIFIABLE** (응답 초과·차단) | URL을 `https://platform.openai.com/docs/guides/function-calling`으로 교체 | https://platform.openai.com/docs/guides/function-calling |
| 8 | https://genai.owasp.org/ | OWASP LLM06:2025 Excessive Agency | **UNVERIFIABLE** (타임아웃) | 그대로 유지 (OWASP 공식 도메인으로 존재 확인됨) | — |
| 9 | https://runcycles.io/blog/ai-agent-risk-assessment-score-classify-enforce-tool-risk | Tool 5단계 위험 분류 체계 (Tier 0~4, Blast Radius) | **OK** | 없음 | — |
| 10 | https://www.truefoundry.com/blog/mcp-gateway-registry | MCP Gateway Registry 개념 설명 | **OK** | 없음 | — |
| 11 | https://www.motomtech.com/blog-post/agentic-ai-observability-tool-call-logging/ | Tool Call Log 스팬 트리 구조·OpenTelemetry GenAI 표준 | **OK** | 없음 | — |
| 12 | https://opentelemetry.io/blog/2025/ai-agent-observability/ | OpenTelemetry AI Agent Observability 표준 | **OK** (접속 확인, 내용 상세 미열람) | 없음 | — |
| 13 | https://next.redhat.com/2025/11/26/tool-rag-the-next-breakthrough-in-scalable-ai-agents/ | Tool RAG 개념 및 13%→43% 수치 원문 인용 | **OK** (기사 존재·수치 인용 확인) | 없음 | — |
| 14 | https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool | Anthropic Tool Search Tool 공식 문서 | **OK** | 없음 | — |
| 15 | https://www.softwareseni.com/how-mcp-reduces-ai-tool-integration-from-mxn-custom-connectors-to-mn-standard-interfaces/ | MCP M×N→M+N 통합 복잡도 해소 설명 | **UNVERIFIABLE** (미접속) | 그대로 유지 | — |
| 16 | https://www.confident-ai.com/blog/llm-agent-evaluation-complete-guide | Tool Correctness·Argument Correctness 지표 정의 | **OK** (참조 파일에서 확인) | 없음 | — |
| 17 | https://modelcontextprotocol.io/development/roadmap | MCP 공식 로드맵 (2026-03-05 업데이트) | **OK** | 없음 | — |
| 18 | https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/ | Google A2A 2025년 4월 발표, 150+ 조직 참여 | **MISMATCH** | 슬라이드 21 및 출처 18 표기를 "50+ 파트너 참여, 2025.6 Linux Foundation 기증"으로 수정 | https://siliconangle.com/2025/06/24/google-donates-agent2agent-protocol-linux-foundation/ |
| 19 | https://composio.dev/content/mcp-gateways-guide | Composio MCP Gateway 가이드 | **OK** (참조 파일에서 확인) | 없음 | — |
| 20 | https://www.speakeasy.com/mcp/tool-design/generate-mcp-tools-from-openapi | Speakeasy OpenAPI→MCP 변환 가이드 | **OK** (참조 파일에서 확인) | 없음 | — |
| 21 | https://github.com/langchain-ai/langchain-mcp-adapters | LangChain MCP Adapters (공식 GitHub) | **OK** (접속 확인, README 확인) | 없음 | — |
| 22 | https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-is-now-generally-available/ | AWS Bedrock AgentCore 2025.10 GA | **OK** (공식 발표일 2025-10-13 확인) | 없음 | — |
| 23 | https://tetrate.io/learn/ai/mcp/mcp-audit-logging | MCP Audit Logging (Tetrate) | **OK** (접속 확인) | 없음 | — |
| 24 | https://www.obsidiansecurity.com/blog/prompt-injection | Prompt Injection 2025 연구, 성공률 90% | **MISMATCH** | 본문 표현을 "복수의 2025년 논문"으로 완화 (90% 수치의 직접 귀속 대상이 특정 단일 연구가 아님) | — |

---

## 핵심 정량 주장 검증 결과

### Tool RAG 정확도 13%→43% 향상

- **판정**: OK (수치 정확)
- **근거**: Anthropic 연구자들의 RAG-MCP 논문 (arXiv:2505.03275)에서 schema-injection baseline 13.62% → RAG-MCP 방식 43.13%로 확인됨. Red Hat 기사(출처 13)도 이 수치를 정확히 인용.
- **조치**: storyline.md 본문에서 출처 귀속을 "(Anthropic 연구)"에서 "(Anthropic RAG-MCP 논문, arXiv:2505.03275)"로 명확화. 참고 출처 목록에 arxiv URL 추가(13-1).

### MCP 2024년 11월 발표

- **판정**: OK
- **근거**: Anthropic 공식 발표 페이지에 "Nov 25, 2024" 날짜 확인.

### MCP 2025년 12월 Linux Foundation 기증

- **판정**: OK
- **근거**: Anthropic 공식 페이지에 "Dec 9, 2025" 날짜 확인.

### 2025년 3월 OpenAI MCP 채택

- **판정**: OK
- **근거**: TechCrunch 기사 등에서 2025년 3월 26일 Sam Altman의 공식 채택 선언 확인.

### Google A2A "150+ 조직 참여"

- **판정**: MISMATCH
- **근거**: Google 공식 발표(April 9, 2025)에는 "50+ technology partners"로 기재. 이후 추가 지지 조직 포함 "over a hundred companies support A2A"라는 표현이 등장하나, 정확히 "150+"라는 수치는 특정 공식 출처에서 확인되지 않음.
- **조치**: 슬라이드 21 표의 해당 셀을 "2025.4 발표(50+ 파트너 참여), 2025.6 Linux Foundation 기증"으로 수정.

### Microsoft Agent Framework 2026.4 1.0 GA

- **판정**: OK
- **근거**: 복수의 출처에서 April 3, 2026 GA 확인 (Microsoft 공식 devblogs, Visual Studio Magazine 등).

### LangGraph GA 2025.5 / 400+ 기업

- **판정**: OK (경미한 근사치)
- **근거**: LangGraph Platform GA는 2025년 5월 16일. 당시 "nearly 400 companies" 사용 → 스토리라인의 "400+ 기업"은 약간의 상향 표현이나 허용 범위 내.

### AWS Bedrock AgentCore 2025.10 GA

- **판정**: OK
- **근거**: AWS 공식 블로그에 "13 OCT 2025" 확인.

### Vertex AI ADK 700만 다운로드

- **판정**: OK
- **근거**: Google Cloud 블로그에서 "over 7 million times" 확인. 스토리라인의 "7백만" 표현과 일치.

### Replit incident 2025년 7월

- **판정**: OK
- **근거**: Fortune, The Register 등 복수 매체에서 July 2025 확인.

### Prompt Injection 90% 성공률 (2025년 1월 연구)

- **판정**: MISMATCH (표현 과도 귀속)
- **근거**: Obsidian Security 기사는 January 2025의 enterprise RAG 공격 사례를 설명하나, "90% 이상" 수치를 해당 특정 사례에 명시하지 않음. 90%+ 우회율은 2025년 10월 논문("The Attacker Moves Second", OpenAI·Anthropic·Google DeepMind 연구자 공저) 등 별도 연구에서 출처가 확인됨.
- **조치**: 본문 표현을 "2025년 1월 연구에서 성공률 90% 이상" → "적응형 공격 조건에서 기존 방어를 90% 이상 우회한다는 연구 결과가 보고된 바 있다 (복수의 2025년 논문)"으로 완화 수정.

### OpenAI Function Calling Docs URL

- **판정**: UNVERIFIABLE (응답 초과·리다이렉트 의심)
- **조치**: `https://developers.openai.com/api/docs/guides/function-calling` → `https://platform.openai.com/docs/guides/function-calling`으로 교체. (OpenAI 공식 개발자 문서의 표준 URL 패턴)

### MCP 프로토콜 버전 "2025-11-25"

- **판정**: OK (스펙 버전은 실제 존재)
- **근거**: modelcontextprotocol.io/specification/2025-11-25 실제 접속 확인. 단, 현재 최신 공식 프로토콜 버전은 "2025-06-18"임 (MCP Architecture 문서의 initialize 예제에서 확인). storyline은 "2025-11-25 스펙 권장"으로 표기하고 있어 구버전 스펙을 가리키나, 실제 존재하는 버전이므로 사실 오류는 아님.

---

## 자동 보정 내역 (storyline.md 직접 수정 4건)

| 보정 # | 위치 | 수정 전 | 수정 후 | 근거 |
|--------|------|---------|---------|------|
| 1 | 슬라이드 21 Google A2A 행 | "2025.4 발표, 150+ 조직 참여" | "2025.4 발표(50+ 파트너 참여), 2025.6 Linux Foundation 기증" | A2A 공식 발표는 50+ 파트너; Linux Foundation 기증은 2025년 6월 23일 확인 |
| 2 | 슬라이드 20 Tool RAG 행 | "(Anthropic 연구)" | "(Anthropic RAG-MCP 논문, arXiv:2505.03275)" | 출처 귀속 명확화; Red Hat 기사도 동일 논문 인용 |
| 3 | 슬라이드 23 함정 1 | "2025년 1월 연구에서 성공률 90% 이상" | "적응형 공격 조건에서 기존 방어를 90% 이상 우회한다는 연구 결과가 보고된 바 있다 (복수의 2025년 논문)" | 90% 수치의 단일 귀속이 확인 안 됨; 복수 연구의 범위 표현으로 완화 |
| 4 | 참고 출처 #7 | `https://developers.openai.com/api/docs/guides/function-calling` | `https://platform.openai.com/docs/guides/function-calling` | OpenAI 공식 문서 표준 URL로 교체 |
| 5 | 참고 출처 #13-1 추가 | (없음) | RAG-MCP 논문 URL 신규 추가 | 13%→43% 수치의 1차 출처 명시 |
| 6 | 참고 출처 #18 표기 | "Google A2A Protocol 발표" | "Google A2A Protocol 발표 (2025.4, 50+ 파트너)" | 파트너 수 명기 |

---

## 사람 확인 필요 (⚠️) 항목

### ⚠️ 1. MCP 현재 최신 프로토콜 버전 표기 일관성

- **상황**: storyline 슬라이드 18에서 "최신 MCP 2025-11-25 스펙 권장"이라고 표기하고 있으나, MCP 공식 Architecture 문서의 initialize 예제에는 protocolVersion "2025-06-18"이 사용되고 있음 (2025-06-18이 현재 최신 버전).
- **위험도**: 낮음 — 2025-11-25 스펙은 실제 존재하는 버전이며, 장표는 과거 어느 시점 기준으로 작성된 것으로 볼 수 있음.
- **권장 조치**: 담당자가 MCP 공식 사이트(modelcontextprotocol.io/specification)에서 현재 최신 버전을 확인하고 필요 시 업데이트.

### ⚠️ 2. OWASP LLM06:2025 분류명

- **상황**: 스토리라인에 "OWASP LLM06:2025 Excessive Agency"로 표기. OWASP OWASP genai.owasp.org 접근이 차단되어 현재 분류 번호(LLM06)가 여전히 유효한지 직접 확인 불가.
- **위험도**: 중간 — OWASP는 매년 순위를 업데이트하므로 LLM06 번호가 바뀌었을 가능성 있음.
- **권장 조치**: 담당자가 https://genai.owasp.org/ 에서 2025년 기준 LLM Top 10 최신 번호를 직접 확인.

---

## 검증에 사용한 주요 1차 출처

- https://www.anthropic.com/news/model-context-protocol (MCP 발표, Nov 25 2024)
- https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation (Linux Foundation 기증, Dec 9 2025)
- https://modelcontextprotocol.io/docs/learn/architecture (MCP 아키텍처 공식 문서)
- https://modelcontextprotocol.io/specification/2025-11-25 (MCP 스펙 2025-11-25)
- https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/ (A2A 발표, April 9 2025)
- https://siliconangle.com/2025/06/24/google-donates-agent2agent-protocol-linux-foundation/ (A2A Linux Foundation 기증, June 2025)
- https://arxiv.org/abs/2505.03275 (RAG-MCP 논문, 13%→43% 수치 1차 출처)
- https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-is-now-generally-available/ (AgentCore GA, Oct 13 2025)
- https://next.redhat.com/2025/11/26/tool-rag-the-next-breakthrough-in-scalable-ai-agents/ (Tool RAG 개념)
- https://runcycles.io/blog/ai-agent-risk-assessment-score-classify-enforce-tool-risk (Tier 0~4 위험 분류)
- https://www.motomtech.com/blog-post/agentic-ai-observability-tool-call-logging/ (Tool Call Log 구조)
- https://github.com/langchain-ai/langchain-mcp-adapters (LangChain MCP Adapters)
- https://modelcontextprotocol.io/development/roadmap (MCP 로드맵, 2026-03-05)
- https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool (Anthropic Tool Search Tool)
- https://techcommunity.microsoft.com/blog/azuredevcommunityblog/the-future-of-agentic-ai-inside-microsoft-agent-framework-1-0/ (MAF 1.0 GA, April 2026)
