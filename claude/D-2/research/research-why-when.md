# D-2 API/Tool 연계 데이터 — Why / When 리서치
> 담당 클러스터: 개념·구조 중심 필요성 논증 + 적용 시점·조건
> 작성일: 2026-06-05

---

## 1. 개념·구조로 보는 필요성

### 1-1. LLM은 텍스트 생성기다 — 행동(Action)이 불가능하다

LLM(Large Language Model)은 근본적으로 **확률적 텍스트 생성기(probabilistic text generator)**다. 입력 토큰 시퀀스를 받아 다음 토큰의 확률 분포를 계산하는 것이 전부이며, 자체적으로 외부 시스템에 직접 접근하거나 실행할 수 없다. 데이터베이스 조회, API 호출, 파일 시스템 조작 — 이 모두는 모델 바깥의 실제 시스템 호출이다.

AI Agent가 "행동(Action)"을 수행하려면 반드시 **Tool/Function 명세(Specification)**가 중간에 개입해야 한다. 이 명세가 LLM의 "손과 발"이 된다.

### 1-2. Agent가 Tool을 쓸 수 있으려면 3요소가 필요하다

MCP(Model Context Protocol) 공식 문서와 LLM Function Calling 구조 분석에 따르면, AI Agent가 외부 시스템을 올바르게 호출하려면 세 가지 요소가 반드시 명세(data)로 정의되어 있어야 한다:

| 요소 | 역할 | 없을 때 발생하는 문제 |
|---|---|---|
| **① Description** (설명) | Agent가 "어떤 Tool이 있는지" 알 수 있게 한다. Tool 이름, 목적, 언제 써야 하는지를 자연어로 기술 | Tool 자체를 모르거나, 잘못된 Tool을 선택하는 **Tool Selection Hallucination** 발생 |
| **② Schema** (입출력 명세) | Agent가 "올바른 형식으로 호출"할 수 있게 한다. 파라미터 이름·타입·필수 여부, 반환 값의 구조를 JSON Schema로 정의 | 잘못된 파라미터 타입, 누락된 필수 값, 파싱 불가 응답 → **Tool Usage Hallucination** |
| **③ Risk/권한 (Constraint)** | Agent가 "안전하게 실행"될 수 있게 한다. 허용 범위, 실행 위험도(read-only vs. write), 인증 범위를 사전 정의 | 권한 초과 실행, 의도하지 않은 데이터 변경·삭제 → **Excessive Agency** |

MCP 공식 문서의 Tool 정의 구조는 이 세 요소를 그대로 반영한다:

```typescript
{
  name: string;          // Tool 식별자 (Discovery)
  description?: string;  // LLM이 읽는 목적 설명 (Description)
  inputSchema: {         // 파라미터 JSON Schema (Schema)
    type: "object",
    properties: { ... }
  }
}
```

### 1-3. 명세가 부정확하면 무엇이 원리적으로 불가능해지는가

**명세 부재 또는 부정확 시의 인과 구조:**

```
명세 부재/부정확
    ↓
LLM이 Tool 선택·호출 형식을 "추정"
    ↓
Hallucinated Tool Call (없는 Tool 호출)
    또는
Wrong Parameter (잘못된 파라미터 타입/값)
    또는
Excessive Permission (과도한 권한 실행)
    ↓
시스템 장애 / 데이터 손상 / 보안 침해
```

실제 사례로 확인된 위험: Agent에 광범위한 권한이 주어진 상태에서 "불필요한 파일 정리" 요청이 들어왔을 때, Agent가 어떤 파일이 "불필요한지"를 추정(hallucinate)하여 프로덕션 설정 파일이 담긴 디렉터리를 삭제하는 사고가 보고된다. Tool 호출은 **머신 속도**로 수천 건이 이루어질 수 있으므로, 명세 부재의 위험은 사람이 직접 실행하는 경우보다 훨씬 빠르게 증폭된다.

---

## 2. 왜 "데이터"로 다루어야 하는가

### 2-1. Tool 명세 자체가 관리·버전관리·재사용되어야 하는 데이터 자산이다

Tool Description, Input/Output Schema, Call Log는 단순한 "코드 설정값"이 아니라 **데이터 자산(data asset)**으로 다루어야 한다. 그 이유:

- **버전 관리(Versioning)**: 외부 API가 바뀌면 Schema도 함께 바뀌어야 하며, 이전 버전과의 호환성을 추적해야 한다. 코드 안에 하드코딩되어 있으면 변경 추적이 불가능하다.
- **재사용(Reusability)**: 여러 Agent·LLM이 같은 Tool을 쓸 때 동일한 명세를 참조해야 일관성이 보장된다. 명세가 각 코드베이스에 흩어져 있으면 중복·불일치가 발생한다.
- **감사 가능성(Auditability)**: 어떤 Tool이 어떤 파라미터로 언제 호출되었는지의 Call Log는 규정 준수(compliance)와 장애 분석을 위한 핵심 데이터다. 이를 구조화된 데이터로 저장·조회할 수 없으면 사후 추적이 불가능하다.
- **발견 가능성(Discoverability)**: Agent가 런타임에 "어떤 Tool이 사용 가능한가"를 동적으로 조회(Discovery)하는 구조에서, Tool 명세 목록은 카탈로그로서의 데이터가 된다.

MCP 감사 로깅 연구에 따르면, 모든 Tool 호출에 대해 "어떤 파라미터 Schema로, 어떤 인증 범위로, 언제 호출되었는가"를 기록하는 것이 규정 준수와 이상 탐지의 기반이 되며, 이 로그는 **비즈니스 자산(business asset)**으로 장기 보존되어야 한다.

### 2-2. 코드에 흩어진 통합을 표준 명세로 외부화하는 의미

기존 통합 방식에서는 각 LLM-Tool 연결이 코드 내 개별 함수 또는 API 클라이언트로 구현되었다. 이를 표준 명세(데이터)로 외부화하면:

- 특정 프로그래밍 언어·프레임워크 종속성이 사라진다
- Tool 명세 변경이 코드 재배포 없이 명세 업데이트만으로 반영될 수 있다
- 다른 팀·다른 Agent가 같은 명세를 참조하여 일관된 방식으로 Tool을 사용할 수 있다

---

## 3. MCP 같은 표준이 해결하는 M×N 문제

### 3-1. M×N 문제의 구조

표준 프로토콜 없이 N개 LLM과 M개 Tool을 연결하면 **N×M개의 개별 커넥터**가 필요하다:

- 5개 LLM × 10개 Tool = **50개 커넥터**
- Tool 1개 추가 시 → 5개 커넥터를 새로 작성
- LLM 1개 교체 시 → 10개 커넥터를 재작성

각 커넥터는 특정 LLM의 Function Calling 형식과 특정 Tool의 API Schema를 하드코딩하므로, 어느 한쪽이 변경될 때마다 해당 커넥터가 독립적으로 깨진다.

### 3-2. 표준 프로토콜이 M+N으로 줄이는 구조

MCP 같은 표준이 개입하면:
- 각 Tool은 **MCP Server 1개** (Tool 측 구현)
- 각 LLM 호스트는 **MCP Client 1개** (모델 측 구현)
- 총 구현 수: **N + M개**

5 + 10 = **15개**만으로 완성. Tool 1개 추가 → MCP Server 1개만 작성하면 5개 LLM 모두에서 즉시 사용 가능. LLM 교체 → MCP Client 1개 구현만으로 10개 Tool 모두 접근 가능.

이 구조적 필요성은 "편의"가 아니라 **확장 가능성(scalability)의 원리적 전제**다. 기업이 수십 개의 AI Agent와 수백 개의 내부 시스템을 연결해야 하는 시나리오에서, M×N 방식은 유지 불가능해진다.

### 3-3. 런타임 동적 Discovery의 구조적 의미

MCP에서는 세션 초기화 시 Client가 Server에 `tools/list`, `resources/list`, `prompts/list`를 호출하여 **능력 목록(capability manifest)**을 동적으로 조회한다. 이는:
- 새 Tool이 추가되어도 Host 측 코드 변경이 불필요하다
- Agent가 문맥에 따라 런타임에 사용 가능한 Tool을 결정할 수 있다
- Tool 명세가 "코드 안의 정적 설정"이 아니라 "런타임에 조회되는 데이터"가 된다

---

## 4. Key Objectives 후보

아래 목표들은 위의 개념·구조에서 직접 도출된 동사형 목표다:

| # | Key Objective | 근거 |
|---|---|---|
| **KO-1** | Agent의 행동 가능 범위를 명세(Description·Schema)로 정의하고 통제한다 | 명세 없이는 Agent가 올바른 Tool을 선택·호출할 수 없는 원리적 한계 |
| **KO-2** | 잘못된·위험한 호출을 명세 수준에서 사전 차단한다 | Hallucinated call, 권한 초과 실행은 명세의 Risk/Constraint가 없으면 사후 통제만 가능 |
| **KO-3** | Tool 통합을 표준화하여 재사용·확장 가능하게 한다 | M×N 문제 해소, 신규 Tool/LLM 추가 시 전체 재작성 방지 |
| **KO-4** | 모든 Tool 호출을 추적 가능(traceable)하게 기록하고 감사한다 | Call Log는 규정 준수·장애 분석·이상 탐지의 데이터 기반 |
| **KO-5** | Tool 명세를 버전 관리되는 데이터 자산으로 관리한다 | API 변경 시 Schema 업데이트 추적, 다중 Agent 간 일관성 보장 |

---

## 5. When — 적용 시점·조건

### 5-1. 단순 챗봇에는 불필요하다

텍스트 입력 → 텍스트 응답만 하는 단발(one-shot) LLM 챗봇 구조에서는 Tool 명세 체계가 필요하지 않다. 모델이 외부 시스템에 아무 행동도 취하지 않기 때문이다.

### 5-2. 다음 조건 중 하나라도 충족되면 즉시 필요하다

**트리거 조건 (적용 시작 기준):**

```
[ ] AI Agent가 외부 시스템을 조회(read)해야 한다
    → DB 조회, API GET, 파일 읽기 포함. Read-only라도 명세가 없으면 잘못된 파라미터로 오류 발생
    
[ ] AI Agent가 외부 시스템에 실행/변경 작업을 수행해야 한다
    → API POST/PUT/DELETE, 파일 쓰기, 메시지 발송 등
    → 실행형 Tool의 존재 = 위험 관리(Risk/권한) 명세가 필수
    
[ ] 여러 Tool을 순서대로 또는 조건부로 사용해야 한다 (Multi-step Agent)
    → Tool Chaining: 이전 Tool의 출력이 다음 Tool의 입력이 되는 구조
    → 각 Tool의 Output Schema가 다음 Tool의 Input Schema와 호환되는지 사전 정의 필요
    
[ ] 여러 LLM 또는 여러 Agent가 같은 Tool을 공유해야 한다
    → M×N 문제 발생 → 표준 명세 없이는 각자 별도 구현, 불일치 필연
    
[ ] Tool 호출 결과에 대한 감사·규정 준수가 요구된다
    → 어떤 데이터에 접근했는지, 어떤 작업을 수행했는지의 Call Log가 규제 증거
```

### 5-3. 적용 범위 확장 시점

| 단계 | 상황 | 필요 수준 |
|---|---|---|
| **단순 조회** | Agent가 read-only API 1~2개 사용 | Tool Description + Input Schema 최소 필요 |
| **실행형 Agent** | Agent가 데이터 변경·전송 작업 수행 | Description + Schema + Risk/권한 명세 필수 |
| **Multi-tool Agent** | Agent가 여러 Tool을 연계 사용 | Tool 간 Input/Output Schema 호환성 검증 필요 |
| **Multi-agent 시스템** | 여러 Agent·LLM이 Tool 공유 | 표준 프로토콜(MCP 등) + Tool Registry 필요 |
| **엔터프라이즈 배포** | 규정 준수·감사 요건 존재 | Call Log + 버전 관리 + 접근 제어 전체 필요 |

---

## 출처

| 문서 | URL |
|---|---|
| MCP 공식 문서 — Tools 개념 | https://modelcontextprotocol.info/docs/concepts/tools/ |
| MCP 공식 스펙 (최신) | https://modelcontextprotocol.io/specification/2025-11-25 |
| MCP M×N 문제 해결 구조 분석 (SoftwareSeni) | https://www.softwareseni.com/how-mcp-reduces-ai-tool-integration-from-mxn-custom-connectors-to-mn-standard-interfaces/ |
| Klavis — MCP N×M Integration Problem | https://www.klavis.ai/blog/mcp-solving-n-x-m-integration-problem |
| Humanloop — MCP Explained | https://humanloop.com/blog/mcp |
| The Anatomy of Tool Calling (Amit Chaudhary) | https://amitness.com/posts/function-calling-schema/ |
| LLM Function Calling 구조 (ML4Devs) | https://www.ml4devs.com/what-is/llm-function-calling-and-tools/ |
| LLM Function Calling — Agenta 가이드 | https://agenta.ai/blog/the-guide-to-structured-outputs-and-function-calling-with-llms |
| AI Agent Hallucination — Tool 선택·사용 오류 | https://manveerc.substack.com/p/ai-agent-hallucinations-prevention |
| AI Agent 위험 — Excessive Agency | https://auth0.com/blog/mitigate-excessive-agency-ai-agents/ |
| Rafter — Hallucination & Unsafe Autonomy | https://rafter.so/blog/ai-agent-hallucinations-unsafe-autonomy |
| Atlan — AI Agent Hallucination 원인·위험 | https://atlan.com/know/ai-agent-hallucination/ |
| MCP Audit Logging — Tetrate | https://tetrate.io/learn/ai/mcp/mcp-audit-logging |
| MCP 감사 추적 구축 — MintMCP | https://www.mintmcp.com/blog/build-audit-trails-ai-coding-agents |
| AI Agent 감사 로그 — Dynatrace | https://www.dynatrace.com/news/blog/the-rise-of-agentic-ai-part-7-introducing-data-governance-and-audit-trails-for-ai-services/ |
| AWS 보안 MCP 패턴 | https://aws.amazon.com/blogs/security/secure-ai-agent-access-patterns-to-aws-resources-using-model-context-protocol/ |
| MCP Wikipedia | https://en.wikipedia.org/wiki/Model_Context_Protocol |
| MCP November 2025 스펙 업데이트 (Medium) | https://medium.com/@dave-patten/mcps-next-phase-inside-the-november-2025-specification-49f298502b03 |
