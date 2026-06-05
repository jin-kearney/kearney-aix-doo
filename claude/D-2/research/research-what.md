# D-2 API/Tool 연계 데이터 — What(핵심) 리서치

> 작성일: 2026-06-05 | 담당 클러스터: 정의/구성요소/표준/동작원리/핵심개념/용어

---

## 1. 개념 정의와 범위

### 1.1 "API/Tool 연계 데이터"란 무엇인가

"API/Tool 연계 데이터"는 **AI Agent가 외부 시스템·서비스·함수(Function)를 안전하고 일관되게 호출할 수 있도록, 해당 Tool(도구)의 기능·입력·출력·제약 조건을 명세한 체계**를 가리킨다. 쉽게 말해 "AI가 읽을 수 있는 API 설명서(Specification) 전체"다.

이 주제가 다루는 것:
- Tool/Function의 이름, 목적, 입력 스키마(Input Schema), 출력 형식, 오류 처리
- Tool을 AI가 어떻게 선택·호출·검증하는지 동작 원리
- Tool을 조직 수준에서 등록·버전 관리·거버넌스하는 구조
- Tool 호출 이력(Log)과 Observability

이 주제가 다루지 않는 것:
- Tool이 처리하는 비즈니스 데이터 자체(예: 고객 DB의 레코드 내용)
- AI 모델 자체의 훈련 데이터 또는 파인튜닝
- 데이터 파이프라인, ETL 로직

**핵심 경계**: "API/Tool 연계 데이터"는 Tool *인터페이스 명세(Interface Specification)* 이며, Tool이 반환하는 비즈니스 데이터 자체와 구분된다. 예를 들어 `get_customer_info(customer_id)` Tool의 호출 명세가 이 주제이며, 호출 결과로 돌아오는 고객 정보 레코드는 별도 데이터 주제(예: D-1 데이터 카탈로그)에 속한다.

### 1.2 AI Agent가 외부 세계와 상호작용하는 방식

LLM(Large Language Model) 단독으로는 텍스트 생성만 가능하다. 외부 세계와 상호작용하려면 "Tool"이 필요하다.

```
사용자 질문 → LLM → [Tool 호출 결정] → Tool 실행 → 결과 반환 → LLM → 최종 응답
```

LLM은 어떤 Tool이 있는지, 어떤 인자(Argument)가 필요한지를 **Tool Description(명세)**에서 읽어 판단한다. 따라서 Tool Description의 품질이 Agent 동작의 정확성을 직접 결정한다.

---

## 2. LLM Function Calling / Tool Use 표준

### 2.1 OpenAI Function Calling

OpenAI는 2023년 Function Calling을 도입하며, LLM이 API를 자동으로 호출할 수 있는 표준 방식을 제시했다. 2025년 기준 "Tool Calling"으로 명칭이 정착됐다.

**Tool 정의 구조 (JSON Schema 기반)**:

```json
{
  "type": "function",
  "function": {
    "name": "get_weather",
    "description": "주어진 위치의 현재 날씨 정보를 반환한다.",
    "parameters": {
      "type": "object",
      "properties": {
        "location": {
          "type": "string",
          "description": "도시명 및 국가코드, 예: Seoul, KR"
        },
        "unit": {
          "type": "string",
          "enum": ["celsius", "fahrenheit"],
          "description": "온도 단위"
        }
      },
      "required": ["location"]
    }
  }
}
```

**Strict Mode (2025)**: OpenAI는 `strict: true` 옵션을 도입해, Tool 호출 시 스키마를 완전히 준수하도록 강제할 수 있다. 이 모드에서는 `additionalProperties`가 반드시 `false`이어야 하고, 모든 `properties`가 `required`에 포함되어야 한다.

**동작 원리**:
1. API 요청에 `tools` 파라미터로 Tool 목록 전달
2. LLM이 사용자 메시지를 분석해 적절한 Tool 선택
3. LLM이 `tool_use` 블록으로 Tool 이름 + 인자를 JSON으로 반환
4. 애플리케이션이 실제 Tool 실행
5. 결과를 `tool_result`로 LLM에 반환 → 최종 응답 생성

**tool_choice 옵션**:
- `auto`: LLM이 Tool 사용 여부 자체 결정 (기본값)
- `any`: 반드시 Tool 중 하나를 사용
- `tool`: 특정 Tool 강제 호출
- `none`: Tool 사용 금지

출처: [OpenAI Function Calling Docs](https://developers.openai.com/api/docs/guides/function-calling)

### 2.2 Anthropic Claude Tool Use

Claude API에서 Tool은 다음 구조로 정의한다:

```json
{
  "name": "get_stock_price",
  "description": "주식 시세를 실시간으로 조회한다. 종목코드(ticker)와 거래소를 입력하면 현재가, 등락률, 거래량을 반환한다. 비상장 종목이나 지수(index)에는 사용하지 않는다.",
  "input_schema": {
    "type": "object",
    "properties": {
      "ticker": {
        "type": "string",
        "description": "종목코드 (예: AAPL, 005930.KS)"
      },
      "exchange": {
        "type": "string",
        "enum": ["NYSE", "NASDAQ", "KRX", "LSE"],
        "description": "거래소 코드"
      }
    },
    "required": ["ticker"]
  }
}
```

OpenAI와 달리 Claude는 `parameters` 대신 `input_schema`라는 키를 사용한다. 내용은 동일하게 JSON Schema 표준을 따른다.

**Tool Use 베스트 프랙티스 (Anthropic 공식 권고)**:
- description은 최소 3~4문장 이상 상세하게 작성
- 언제 써야 하고, 언제 쓰지 말아야 하는지 명시
- 관련 작업은 하나의 Tool로 통합 (`action` 파라미터 활용)
- 서비스 접두어 사용: `github_list_prs`, `slack_send_message`
- 응답은 LLM에게 필요한 정보만 담아 간결하게 설계

**input_examples 필드 (2025 신규)**: 복잡한 Tool은 `input_examples` 배열로 예시 입력 제공 가능 (약 20~200 토큰 추가 비용).

출처: [Anthropic Claude Tool Use - Define Tools](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools)

### 2.3 Tool 호출의 전체 흐름

```
1. 개발자가 Tool 정의 (name + description + input_schema)
2. API 요청 시 tools 파라미터로 Tool 목록 전달
3. LLM이 사용자 의도 분석 → 적절한 Tool + 인자 선택
4. LLM이 tool_use 블록 반환
5. 애플리케이션이 실제 함수/API 실행
6. 결과를 tool_result로 LLM에 전달
7. LLM이 최종 응답 생성
```

---

## 3. MCP (Model Context Protocol)

### 3.1 정의와 목적

MCP(Model Context Protocol)는 **Anthropic이 2024년 11월에 발표한 개방형 프로토콜**로, LLM 애플리케이션과 외부 데이터 소스·도구 간의 표준화된 통합을 가능하게 한다.

> "MCP는 AI를 위한 USB-C 포트와 같다. USB-C가 기기와 주변기기를 연결하는 표준 방식을 제공하듯, MCP는 AI 모델을 다양한 데이터 소스와 도구에 연결하는 표준 방식을 제공한다."
> — Anthropic 공식 발표

**해결하는 문제: M×N 통합 문제**

MCP 이전에는 M개의 AI 모델 각각이 N개의 데이터 소스/도구와 개별 통합을 유지해야 했다 → M×N 통합 복잡도.

MCP는 이를 M+N으로 축소한다. 각 도구는 MCP 서버를 한 번만 구현하고, 각 AI 앱은 MCP 클라이언트를 한 번만 구현하면, 어떤 조합도 연결 가능해진다.

**거버넌스 이관**: 2025년 12월, Anthropic은 MCP를 Linux Foundation 산하 Agentic AI Foundation에 기증했다. 2025년 3월, OpenAI도 MCP를 채택하며 업계 표준으로 자리잡았다. 2025년 기준 월 9,700만 건 이상의 SDK 다운로드, 10,000개 이상의 활성 MCP 서버.

출처: [Anthropic MCP 발표](https://www.anthropic.com/news/model-context-protocol), [MCP 거버넌스 이관](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation)

### 3.2 MCP 아키텍처: Host / Client / Server

```
┌─────────────────────────────────────────┐
│          MCP Host (AI Application)       │
│  (예: Claude Desktop, VS Code, IDEs)    │
│                                          │
│   ┌──────────┐  ┌──────────┐            │
│   │MCP Client│  │MCP Client│  ...        │
│   │    1     │  │    2     │            │
│   └────┬─────┘  └─────┬────┘            │
└────────┼──────────────┼─────────────────┘
         │              │
  전용 연결(Dedicated)   │
         │              │
┌────────▼──┐   ┌───────▼────┐
│MCP Server A│  │MCP Server B │
│(Local:    │  │(Remote:     │
│ Filesystem│  │ Sentry API) │
│ DB 등)    │  │             │
└───────────┘  └─────────────┘
```

**MCP Host**: LLM 애플리케이션 (예: Claude Desktop, VS Code). 하나 이상의 MCP Client를 생성·관리하며 MCP 기능을 조율한다.

**MCP Client**: MCP Host가 MCP Server 하나당 생성하는 컴포넌트. 전용 연결(1:1)을 유지하며 JSON-RPC 메시지를 처리한다.

**MCP Server**: 외부 데이터·기능을 AI 애플리케이션에 노출하는 프로그램. 로컬(stdio 사용)이거나 원격(Streamable HTTP 사용)일 수 있다.

출처: [MCP Architecture Overview](https://modelcontextprotocol.io/docs/learn/architecture)

### 3.3 MCP 핵심 Primitive (원시 구성요소)

**서버가 노출하는 Primitive**:

| Primitive | 정의 | 예시 |
|-----------|------|------|
| **Tools** | AI가 호출할 수 있는 실행 가능한 함수 | DB 쿼리, 파일 쓰기, API 호출 |
| **Resources** | AI에게 맥락 정보를 제공하는 읽기 전용 데이터 소스 | 파일 내용, DB 스키마, API 응답 |
| **Prompts** | LLM과의 상호작용을 구조화하는 재사용 가능한 템플릿 | 시스템 프롬프트, Few-shot 예시 |

**클라이언트가 노출하는 Primitive**:

| Primitive | 정의 |
|-----------|------|
| **Sampling** | 서버가 클라이언트의 LLM에 완성(Completion) 요청 |
| **Elicitation** | 서버가 사용자로부터 추가 정보 요청 |
| **Logging** | 서버가 디버깅용 로그 메시지 전송 |

**실험적 Primitive**:
- **Tasks**: 장시간 실행 작업의 비동기 추적 및 결과 조회

출처: [MCP Architecture - Primitives](https://modelcontextprotocol.io/docs/learn/architecture#primitives)

### 3.4 MCP 데이터 레이어 프로토콜

MCP는 **두 개의 레이어**로 구성된다:

**데이터 레이어 (Data Layer)**:
- JSON-RPC 2.0 기반의 메시지 교환 프로토콜
- 라이프사이클 관리: 초기화(initialize), 기능 협상(capability negotiation), 종료
- 핵심 Primitive 정의 및 메서드 규격

**전송 레이어 (Transport Layer)**:
- **stdio 전송**: 로컬 프로세스 간 표준 입출력 사용. 네트워크 오버헤드 없음.
- **Streamable HTTP 전송**: HTTP POST + Server-Sent Events(SSE). 원격 서버 지원. OAuth 등 표준 HTTP 인증 사용.

### 3.5 MCP 메시지 교환 예시 (JSON-RPC 2.0)

**초기화 (Initialization)**:
```json
// 클라이언트 → 서버
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "initialize",
  "params": {
    "protocolVersion": "2025-06-18",
    "capabilities": { "elicitation": {} },
    "clientInfo": { "name": "example-client", "version": "1.0.0" }
  }
}

// 서버 → 클라이언트 응답
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "protocolVersion": "2025-06-18",
    "capabilities": {
      "tools": { "listChanged": true },
      "resources": {}
    },
    "serverInfo": { "name": "example-server", "version": "1.0.0" }
  }
}
```

**Tool 목록 조회 (tools/list)**:
```json
// 요청
{ "jsonrpc": "2.0", "id": 2, "method": "tools/list" }

// 응답
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": {
    "tools": [
      {
        "name": "weather_current",
        "title": "Weather Information",
        "description": "Get current weather information for any location worldwide",
        "inputSchema": {
          "type": "object",
          "properties": {
            "location": { "type": "string", "description": "City name or coordinates" },
            "units": { "type": "string", "enum": ["metric", "imperial", "kelvin"], "default": "metric" }
          },
          "required": ["location"]
        }
      }
    ]
  }
}
```

**Tool 실행 (tools/call)**:
```json
// 요청
{
  "jsonrpc": "2.0",
  "id": 3,
  "method": "tools/call",
  "params": {
    "name": "weather_current",
    "arguments": { "location": "Seoul", "units": "metric" }
  }
}

// 응답
{
  "jsonrpc": "2.0",
  "id": 3,
  "result": {
    "content": [
      { "type": "text", "text": "Current weather in Seoul: 22°C, partly cloudy" }
    ]
  }
}
```

출처: [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)

---

## 4. Tool Description / Schema 표준

### 4.1 Tool 정의의 필수 항목

Tool을 AI가 이해하고 올바르게 호출하려면 다음 항목이 표준으로 포함되어야 한다:

| 항목 | 설명 | 예시 |
|------|------|------|
| **name** | 고유 식별자. 정규식 `^[a-zA-Z0-9_-]{1,64}$` 준수 | `get_customer_order` |
| **description** | 상세한 자연어 설명. 기능·사용 시점·제약·반환값 포함 | "고객 ID로 주문 이력 조회. 최근 90일 데이터만 제공." |
| **input_schema** | JSON Schema 객체. 파라미터 타입·설명·필수여부·허용값 | `{"type":"object","properties":{...},"required":[...]}` |
| **input_examples** | (선택) 유효한 입력 예시 배열. 복잡한 Tool에 권장 | `[{"customer_id":"C001"},{"customer_id":"C002","days":30}]` |

**Claude API Tool 정의 전체 파라미터**:
- `name`: 필수
- `description`: 필수 (성능에 가장 중요한 항목)
- `input_schema`: 필수 (JSON Schema)
- `input_examples`: 선택
- `cache_control`: 캐시 제어
- `strict`: 스키마 강제 준수 여부
- `defer_loading`: 지연 로딩
- `allowed_callers`: 허용된 호출자 제한

출처: [Anthropic Tool Reference](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-reference)

### 4.2 JSON Schema와의 관계

Tool의 `input_schema` / `parameters`는 **JSON Schema (IETF 표준)** 를 기반으로 한다. 주요 JSON Schema 키워드:

```json
{
  "type": "object",                 // 타입: string, number, integer, boolean, array, object, null
  "properties": {                   // 속성 정의
    "start_date": {
      "type": "string",
      "format": "date",             // 형식 힌트 (date, date-time, email, uri 등)
      "description": "조회 시작일 (YYYY-MM-DD)"
    },
    "status": {
      "type": "string",
      "enum": ["active", "inactive", "pending"],  // 허용값 제한
      "description": "계정 상태 필터"
    },
    "limit": {
      "type": "integer",
      "minimum": 1,                 // 최솟값
      "maximum": 100,               // 최댓값
      "default": 20                 // 기본값
    }
  },
  "required": ["start_date"],       // 필수 항목
  "additionalProperties": false     // 정의되지 않은 항목 불허 (Strict Mode에서 필수)
}
```

**OpenAPI 3.1과의 관계**: OpenAPI 3.1은 JSON Schema Draft 2020-12와 완전히 호환된다. 많은 엔터프라이즈 환경에서 기존 OpenAPI 명세를 MCP Tool 또는 LLM Function Calling 정의로 변환하는 방식을 채택한다.

출처: [Using JSON Schema with OpenAPI - Tyk](https://tyk.io/learning-center/openapi-json-schema/)

### 4.3 좋은 Tool Description의 조건

Anthropic이 공식 문서에서 권고하는 기준:

**좋은 예시**:
```
"주식 종목의 실시간 가격 정보를 조회한다. 
ticker 파라미터에는 NYSE/NASDAQ 표준 종목코드를 입력한다 (예: AAPL, MSFT).
현재가, 등락률(%), 당일 최고/최저가, 거래량을 반환한다.
비상장 종목, 펀드, 지수(예: ^KS11)는 지원하지 않는다."
```

**나쁜 예시**:
```
"주가를 가져온다."
```

핵심 원칙:
1. 무엇을 하는지, 언제 써야 하는지, 언제 쓰지 말아야 하는지 명시
2. 각 파라미터의 의미와 허용 형식 설명
3. 반환값의 구조와 의미 설명
4. 주요 제약·한계 명시

---

## 5. Tool 유형 분류 (위험도 기반)

Tool은 부작용(Side Effect)과 되돌림 가능성(Reversibility)에 따라 분류한다. 이는 Agent의 안전한 자율 실행 범위를 결정하는 핵심 기준이다.

### 5.1 5단계 위험 분류 체계

| Tier | 유형 | 설명 | 예시 Tool | 되돌림 | 부작용 |
|------|------|------|-----------|--------|--------|
| **0** | **Read-only (조회형)** | 상태 변경 없음 | 검색, 조회, 요약, 벡터 검색 | 해당 없음 | 없음 |
| **1** | **Write-local (로컬 쓰기형)** | 내부 상태만 변경 | 임시파일 저장, 캐시 업데이트, 초안 저장 | 쉬움 | 내부만 |
| **2** | **Write-external (외부 쓰기형)** | 시스템 경계 이탈 | 외부 API 호출, 웹훅 트리거 | 가능 (조율 필요) | 파트너 시스템 |
| **3** | **Mutation (변형형)** | 되돌리기 어려운 변경 | 이메일 발송, DB 업데이트, Slack 전송, 티켓 생성 | 어려움/불가 | 고객 노출 |
| **4** | **Execution (실행형)** | 실질적으로 불가역 | 결제 처리, 배포 실행, 권한 변경 | 불가역 | 운영/재무 |

### 5.2 분류 판단 기준 (3가지 질문)

1. **이 행동이 시스템 경계를 벗어나는가?** → Yes: Tier 2 이상
2. **사람이 5분 안에 되돌릴 수 있는가?** → No: Tier 3 이상
3. **요청하지 않은 제3자에게 영향을 미치는가?** → Yes: Tier 3 이상

### 5.3 위험도와 승인 게이트

- **Tier 0~1**: 자동 실행 허용
- **Tier 2**: Reserve-Commit 패턴 적용
- **Tier 3**: Reserve-Commit + 수량 제한
- **Tier 4**: 명시적 승인(Approval Gate) 필수

출처: [AI Agent Risk Assessment - Cycles](https://runcycles.io/blog/ai-agent-risk-assessment-score-classify-enforce-tool-risk), [OWASP LLM06:2025 Excessive Agency](https://genai.owasp.org/)

---

## 6. Tool Registry (레지스트리) / 버전 관리 개념

### 6.1 Tool Registry란

Tool Registry(도구 레지스트리)는 **조직에서 AI Agent가 사용할 수 있는 모든 승인된 Tool을 중앙에서 등록·관리·배포하는 카탈로그 시스템**이다. API 개발자 포털(Developer Portal)이 사람 개발자를 위한 것이라면, Tool Registry는 AI Agent를 위한 것이다.

**Tool Registry의 핵심 기능**:

| 기능 | 설명 |
|------|------|
| **중앙 등록** | Tool을 한 번만 등록하면 권한 있는 모든 Agent가 사용 가능 |
| **동적 발견(Discovery)** | Agent가 런타임에 사용 가능한 Tool 목록 조회 |
| **버전 관리** | 여러 버전의 Tool을 동시 운영 가능 |
| **접근 제어(RBAC)** | Agent별·역할별 Tool 접근 권한 관리 |
| **자격증명 관리** | API 키·OAuth 토큰을 중앙에서 보관·주입 |
| **감사 로그** | 모든 Tool 호출 이력 기록 |

### 6.2 MCP Gateway Registry

MCP 아키텍처에서 Gateway Registry는 MCP 게이트웨이 내부의 중앙 카탈로그다:

```
AI Agent
    │
    ▼
MCP Gateway (인증/인가/라우팅)
    │
    ▼
Tool Registry (카탈로그·버전·RBAC)
    │
    ├─→ MCP Server A (내부 DB)
    ├─→ MCP Server B (외부 API)
    └─→ MCP Server C (사내 서비스)
```

**API Developer Portal과의 차이**:
- Developer Portal: 사람 개발자가 읽는 문서 중심
- Tool Registry: AI Agent가 런타임에 동적으로 조회하는 기계 가독형 카탈로그
- Registry는 per-call 인가, 런타임 자격증명 주입, 구조화된 감사 로그가 필수

### 6.3 버전 관리 패턴

```
Tool Registry
  ├── weather_current v1.0 (생산 중, 기존 Agent 사용)
  ├── weather_current v2.0 (테스트 중, 개발 Agent만 사용)
  └── weather_current v1.0 (deprecated 예정, 유예기간 적용)
```

버전 변경 시 기존 Agent는 자동 영향 없이 v1.0 계속 사용. 새 Agent는 v2.0 명시적 지정. 안정성 확인 후 마이그레이션.

출처: [What Is MCP Gateway Registry - TrueFoundry](https://www.truefoundry.com/blog/mcp-gateway-registry), [AWS Agent Registry - InfoQ](https://www.infoq.com/news/2026/04/aws-agent-registry-preview/)

---

## 7. Tool Call Log / Observability 개념

### 7.1 Agent Observability의 특수성

LLM 단독 Observability(입력-출력 쌍 기록)와 달리, **Agent Observability는 스팬 트리(Span Tree)** 구조를 필요로 한다. 하나의 사용자 요청이 다수의 Tool 호출·재시도·폴백·LLM 재계획으로 이어지기 때문이다.

```
turn:req_abc123  (root, 6.8s, 3,418 tokens)
  plan:claude-opus  (840ms, 1,210 tokens)
  tool:lookup_customer  (320ms, ok)
  tool:fetch_history  (1.1s, ok, retries=1)
    retry:fetch_history.attempt_2  (520ms, ok)
  tool:check_inventory  (180ms, ok)
  compose:claude-opus  (1.4s, 2,208 tokens)
```

### 7.2 Tool Call Log의 필수 필드

**OpenTelemetry GenAI Semantic Conventions** 기반 표준 구조:

```json
{
  "trace_id": "tr_2026-06-05_a91f08b",
  "span_id": "sp_tool_0001",
  "parent_span_id": "sp_root_turn_0000",
  "operation": "execute_tool",
  "tool_name": "lookup_customer",
  "agent_version": "support-agent-v2.1.0",
  "tenant_id": "acme-corp",
  "user_id": "u_88431",
  "start_ts": "2026-06-05T09:32:08.412Z",
  "end_ts": "2026-06-05T09:32:08.732Z",
  "latency_ms": 320,
  "status": "ok",
  "retries": 0,
  "tool_args": {"customer_id": "[REDACTED:pii.id]"},
  "tool_output_hash": "sha256:9b4e3f...",
  "tool_output_size_bytes": 1842,
  "cost_usd_cents": 0
}
```

| 필드 | 설명 |
|------|------|
| `trace_id` | 하나의 사용자 요청(Turn) 전체를 추적하는 ID |
| `span_id` | 개별 Tool 호출 단위 ID |
| `parent_span_id` | 상위 스팬(루트 Turn 또는 Plan 스팬) |
| `operation` | `execute_tool` / `chat` / `embeddings` (OpenTelemetry GenAI 표준) |
| `tool_name` | 호출된 Tool 이름 |
| `agent_version` | Agent 소프트웨어 버전 |
| `tenant_id` | 멀티테넌트 환경에서의 테넌트 식별자 |
| `user_id` | 실제 요청을 한 사용자 |
| `start_ts` / `end_ts` | 호출 시작/종료 시각 (ISO 8601) |
| `latency_ms` | 응답 소요 시간 (밀리초) |
| `status` | `ok` / `error` / `timeout` |
| `retries` | 재시도 횟수 |
| `tool_args` | 호출 인자 (PII/민감정보 Redacted) |
| `tool_output_hash` | 출력 해시 (전체 내용은 별도 저장소) |
| `tool_output_size_bytes` | 출력 크기 |
| `cost_usd_cents` | 해당 호출의 비용 (토큰 기반 모델 호출의 경우) |

### 7.3 OpenTelemetry GenAI 표준

**OpenTelemetry Semantic Conventions for Generative AI**는 LLM·Agent 시스템의 표준 로깅 스키마를 정의한다:

- `gen_ai.operation.name`: `chat`, `execute_tool`, `embeddings`, `generate_content`
- `gen_ai.system`: `anthropic`, `openai`, `google` 등 AI 시스템 식별
- `gen_ai.request.model`: 사용된 모델명
- `gen_ai.usage.input_tokens`: 입력 토큰 수
- `gen_ai.usage.output_tokens`: 출력 토큰 수

Datadog, Honeycomb, New Relic, Langfuse, Arize 등 주요 Observability 벤더가 이 표준으로 수렴 중이다.

**기본 정책**: 개인정보·민감정보 보호를 위해 **기본적으로 프롬프트 내용과 Tool 인자는 로깅하지 않는다**. 디버그 시에만 선택적으로 활성화. 민감 필드는 `[REDACTED:pii.필드명]` 형태로 치환해 디버그 맥락을 유지한다.

출처: [Agentic AI Observability - Motomtech](https://www.motomtech.com/blog-post/agentic-ai-observability-tool-call-logging/), [OpenTelemetry GenAI Conventions](https://opentelemetry.io/blog/2025/ai-agent-observability/), [How OpenTelemetry Traces LLM Calls - Greptime](https://greptime.com/blogs/2026-05-09-opentelemetry-genai-semantic-conventions)

### 7.4 Latency 추적의 3가지 지표

| 지표 | 정의 | 활용 |
|------|------|------|
| **TTFT** (Time to First Token) | 사용자 요청 → 첫 응답 토큰 | 체감 반응 속도 |
| **TBT** (Time Between Tokens) | 토큰 간 간격 | 스트리밍 품질 |
| **Total Turn Time** | 전체 Turn의 경과 시간 | 임계 경로 분석 |

---

## 8. 핵심 용어집

| 용어 | 정의 |
|------|------|
| **Function Calling** | LLM이 미리 정의된 함수(Tool)를 호출하도록 구조화된 출력을 생성하는 메커니즘. OpenAI가 2023년 도입한 명칭. |
| **Tool Use** | Anthropic Claude에서 Function Calling에 해당하는 기능 명칭. 기술적으로 동일한 개념. |
| **MCP (Model Context Protocol)** | Anthropic이 2024년 발표한 개방형 프로토콜. AI 앱과 외부 도구·데이터를 표준화된 방식으로 연결. JSON-RPC 2.0 기반. |
| **Tool** (MCP Primitive) | MCP 서버가 AI에게 노출하는 실행 가능한 함수. AI가 `tools/call`로 호출한다. |
| **Resource** (MCP Primitive) | MCP 서버가 AI에게 제공하는 읽기 전용 데이터 소스 (파일, DB 스키마 등). |
| **Prompt** (MCP Primitive) | MCP 서버가 제공하는 재사용 가능한 프롬프트 템플릿. |
| **JSON-RPC 2.0** | MCP가 채택한 원격 프로시저 호출 프로토콜. 요청·응답·알림을 JSON 형식으로 교환. |
| **Tool Schema** | Tool의 입력 파라미터를 JSON Schema 형식으로 기술한 명세. LLM이 올바른 인자를 생성하는 기준. |
| **Tool Registry** | 조직 내 AI Agent가 사용할 수 있는 모든 Tool을 중앙에서 등록·발견·버전관리·접근제어하는 시스템. |
| **Agent** | LLM을 핵심으로 하며, Tool을 통해 외부와 상호작용하고 다단계 작업을 자율적으로 수행하는 AI 시스템. |
| **Orchestration** | 여러 Agent·Tool·LLM 호출의 순서, 조건, 병렬화를 관리하는 로직 또는 시스템. |
| **Structured Output** | LLM이 자유 형식 텍스트 대신 JSON Schema에 맞는 정형화된 데이터를 반환하는 기능. Tool Use의 기반. |
| **Tool Call Log** | Agent가 Tool을 호출한 시점·입력·출력·소요시간·성공실패를 기록한 이력 데이터. Observability의 핵심 단위. |
| **Blast Radius** | Tool 오작동 시 영향을 받는 범위. 위험도 분류의 핵심 개념. |

---

## 출처

1. **Anthropic MCP 발표** — https://www.anthropic.com/news/model-context-protocol
2. **MCP 공식 아키텍처 문서** — https://modelcontextprotocol.io/docs/learn/architecture
3. **MCP Specification 2025-11-25** — https://modelcontextprotocol.io/specification/2025-11-25
4. **Anthropic Claude Tool Use - Define Tools** — https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools
5. **OpenAI Function Calling Docs** — https://developers.openai.com/api/docs/guides/function-calling
6. **OpenAI Structured Outputs** — https://developers.openai.com/api/docs/guides/structured-outputs
7. **MCP 거버넌스 이관 (Linux Foundation)** — https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation
8. **One Year of MCP: November 2025 Spec Release** — https://blog.modelcontextprotocol.io/posts/2025-11-25-first-mcp-anniversary/
9. **Anthropic MCP Docs (Claude)** — https://docs.anthropic.com/en/docs/agents-and-tools/mcp
10. **MCP Official Registry** — https://registry.modelcontextprotocol.io/
11. **AI Agent Risk Assessment: Score, Classify, Enforce** — https://runcycles.io/blog/ai-agent-risk-assessment-score-classify-enforce-tool-risk
12. **What Is MCP Gateway Registry (TrueFoundry)** — https://www.truefoundry.com/blog/mcp-gateway-registry
13. **AWS Agent Registry Preview (InfoQ)** — https://www.infoq.com/news/2026/04/aws-agent-registry-preview/
14. **Agentic AI Observability: What to Log Per Tool Call (Motomtech)** — https://www.motomtech.com/blog-post/agentic-ai-observability-tool-call-logging/
15. **OpenTelemetry AI Agent Observability Standards (2025)** — https://opentelemetry.io/blog/2025/ai-agent-observability/
16. **How OpenTelemetry Traces LLM Calls, Agent Reasoning, and MCP Tools (Greptime)** — https://greptime.com/blogs/2026-05-09-opentelemetry-genai-semantic-conventions
17. **OpenAPI Specification with JSON Schema (Tyk)** — https://tyk.io/learning-center/openapi-json-schema/
18. **OWASP Top 10 for LLM 2025: LLM06 Excessive Agency** — https://genai.owasp.org/
19. **MCP "USB-C for AI" - The Pragmatic AI Engineer (Medium)** — https://medium.com/@ThePragmaticAIEngineer/mcp-just-became-the-usb-c-for-ai-and-youre-already-behind-if-you-don-t-know-what-it-is-dd17e864987a
20. **AI Agent Index 2025 (MIT)** — https://aiagentindex.mit.edu/data/2025-AI-Agent-Index.pdf
21. **OpenTelemetry Semantic Conventions for GenAI** — https://opentelemetry.io/docs/specs/semconv/gen-ai/
22. **Anthropic: Building Effective Agents** — https://www.anthropic.com/engineering/building-effective-agents
