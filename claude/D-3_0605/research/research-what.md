# D-3 Prompt/Harness 자산화 — What 클러스터 리서치

> 작성일: 2026-06-05  
> 담당 클러스터: What(핵심) — 정의·구성요소·구조·표준·동작원리·라이프사이클·성숙도·용어집  
> 관점: "AI를 위한 데이터 준비" — Prompt/Harness를 **정비해야 할 데이터 자산**으로 보고 어떻게 구조화·표준화·메타데이터화하는지에 초점

---

## 1. 개념 정의: "Prompt/Harness 자산화"란 무엇인가

### 1-1. 핵심 관점 전환

| 기존 관점 | 자산화 관점 |
|---|---|
| 프롬프트 = 일회성 문자열 | 프롬프트 = 조직의 지식 자산 |
| 개인 노트북/슬랙 메시지에 산재 | 중앙 레지스트리에 버전 관리 |
| "잘 동작하면 그만" | 버전·메타데이터·평가기준 포함한 패키지 |
| 엔지니어만 관리 | 업무 도메인 전문가 + 엔지니어 공동 소유 |

Prompt/Harness **자산화(Assetization)**는 사람의 업무 절차와 판단 기준을 AI가 실행 가능한 Prompt와 Harness 형태로 구조화하고, 이를 조직이 재사용·버전관리·거버넌스할 수 있는 **관리 가능한 데이터 자산**으로 등록·유지하는 체계를 말한다.

쉽게 말하면: "우리 현장에서 숙련 작업자가 매일 하는 판단과 절차를 종이 작업표준서(SOP)처럼 AI가 읽고 실행할 수 있는 구조화된 파일로 바꾸고, 그 파일을 잘 정리해서 보관·관리하는 것."

> 두산 제조 현업 비유: 설비 예지보전 담당자가 "이 진동값이면 베어링 교체 시점"이라는 판단 기준을 갖고 있다. 이 판단 기준을 Prompt로 구조화해 저장해 두면, AI가 MES 데이터를 받아 같은 판단을 내릴 수 있다. 이 Prompt 파일이 "자산"이 된다.

### 1-2. PromptOps와 LLMOps에서의 위치

**PromptOps** (= DevOps for Prompt Engineering)는 프롬프트의 설계·테스트·배포·개선·저장을 체계화하는 방법론으로, 2025~2026년 급속히 부상 중이다. Dataversity는 "By end of 2026, PromptOps will be as standard as DevOps"라고 전망했다.

**LLMOps**의 계층 구조에서 Prompt/Harness 자산화는 "데이터 레이어"에 해당한다:

```
LLMOps 계층
├── 인프라 레이어 (컴퓨팅, API 비용)
├── 모델 레이어 (모델 선택, 파인튜닝)
├── 오케스트레이션 레이어 (LangChain, LlamaIndex)
└── [Prompt/Harness 자산 레이어] ← D-3 주제
    ├── Prompt 템플릿 레지스트리
    ├── Harness 패키지 저장소
    └── 버전·메타데이터·평가기준
```

---

## 2. 무엇을 "자산"으로 보는가 — Prompt 자산 목록

### 2-1. 자산 분류 체계

기업 포트폴리오 관점에서 Prompt 자산을 3차원으로 분류한다 (Segev Shmueli, 2025):

**[1] 비즈니스 임팩트 기준**

| 분류 | 설명 | 두산 제조 예시 |
|---|---|---|
| Revenue-Critical (매출 직결) | 고객 대면, 영업 자동화 | 수주 견적 자동화 Prompt |
| Operational (운영) | 프로세스 자동화, 효율화 | MES 이상 감지 판정 Prompt |
| Strategic (전략) | 의사결정 지원, 경쟁 분석 | 품질 트렌드 분석 Harness |
| Compliance (컴플라이언스) | 규제 요건, 감사 지원 | 안전 기준 준수 판정 Prompt |

**[2] 기술 복잡도 기준**

| 분류 | 설명 |
|---|---|
| Atomic Prompt (원자 프롬프트) | 단일 목적, 결정론적 출력 (예: 데이터 분류, 추출) |
| Composite Workflow (복합 워크플로우) | 조건 로직 포함 다단계 Prompt 시퀀스 |
| Adaptive System (적응형 시스템) | 상호작용 패턴에서 학습하는 컨텍스트 인식 Prompt |
| Integration Layer (통합 레이어) | 기업 시스템에 내장된 API 스타일 Prompt |

**[3] 거버넌스 요건 기준**

| 분류 | 해당 영역 |
|---|---|
| 고도 규제 | 금융, 의료, 법무 컴플라이언스 |
| 중간 감독 | 내부 운영, HR, 공급망 |
| 저위험 | 창의적 콘텐츠, 비핵심 자동화 |

### 2-2. 조직 계층 구조 (제안 형태)

```
Enterprise AI Assets (기업 AI 자산)
├── Business Domain Collections (비즈니스 도메인 컬렉션)
│   예: 생산, 품질, 설비관리, 구매
│   ├── Process-Specific Clusters (프로세스별 클러스터)
│   │   예: 설비 예지보전, 품질 판정, 작업지시 생성
│   │   ├── Versioned Prompt Assets (버전 관리된 Prompt 자산)
│   │   │   예: predictive_maintenance_bearing_v1.2.3
│   │   ├── Performance Baselines (성능 기준선)
│   │   │   예: 정확도 92%, 지연 시간 <2s, 비용 $0.003/call
│   │   └── Usage Analytics (사용 분석)
│   │       예: 일 500회 호출, 담당자 만족도 4.2/5
```

---

## 3. Prompt 자산의 구조 — 공식 스키마와 필드

### 3-1. 공통 표준 필드

여러 플랫폼(Microsoft Semantic Kernel, IBM watsonx, LangSmith, MLflow)에서 공통으로 정의하는 Prompt 자산의 필드:

| 필드 카테고리 | 필드명 | 설명 | 예시 |
|---|---|---|---|
| **식별** | `name` | 프롬프트 고유 이름 | `bearing_failure_detection_v2` |
| **식별** | `version` | 시맨틱 버전 (MAJOR.MINOR.PATCH) | `2.1.3` |
| **식별** | `id` / `asset_id` | 시스템 발급 고유 ID | `pmt_0xA3B2...` |
| **설명** | `description` | 이 Prompt가 하는 일의 설명 | "베어링 진동 데이터로 교체 시점 판정" |
| **분류** | `tags` | 조직, 도메인, 용도 태그 | `["설비관리", "예지보전", "production"]` |
| **입력** | `input_variables` | 런타임에 채워질 변수 목록 | `[{name: "vibration_data", required: true}]` |
| **실행** | `execution_settings` | 모델 ID, temperature, max_tokens | `{model_id: "claude-sonnet-4", temperature: 0}` |
| **출력** | `output_variable` | 예상 출력 형식·스키마 | `{description: "판정결과 JSON", json_schema: {...}}` |
| **메타데이터** | `author` | 작성자 | `"김설비_정비팀"` |
| **메타데이터** | `department` | 담당 부서 | `"생산기술팀"` |
| **메타데이터** | `created_at` | 생성일 | `"2025-04-01"` |
| **메타데이터** | `last_updated_at` | 최종 수정일 | `"2025-11-20"` |
| **거버넌스** | `business_owner` | 업무 책임자 | `"설비기술팀장"` |
| **거버넌스** | `review_cycle` | 검토 주기 | `"분기별"` |
| **품질** | `test_cases` | 테스트 케이스 목록 | `[{input: {...}, expected_output: {...}}]` |
| **품질** | `evaluation_criteria` | 평가 기준 | `["정확도 ≥ 90%", "허용 오류 유형 목록"]` |
| **변경이력** | `change_log` | 변경 내역 | `[{version: "2.0.0", reason: "출력 형식 변경"}]` |

### 3-2. Microsoft Semantic Kernel YAML 스키마 (공식 표준)

Microsoft Semantic Kernel은 프롬프트를 YAML 파일로 정의하는 공식 스키마를 제공한다:

```yaml
name: GenerateMaintenanceReport        # 함수명
description: 설비 점검 결과를 바탕으로 정비 보고서를 생성
template_format: handlebars            # 템플릿 형식
template: |
  당신은 두산의 설비 정비 전문가입니다.
  다음 점검 데이터를 기반으로 정비 보고서를 작성하세요.
  
  설비명: {{equipment_name}}
  점검일: {{inspection_date}}
  이상 항목: {{anomaly_list}}
  
input_variables:
  - name: equipment_name
    description: 점검 대상 설비명
    is_required: true
  - name: inspection_date
    description: 점검 수행 날짜
    is_required: true
  - name: anomaly_list
    description: 발견된 이상 항목 목록
    is_required: false
    default: "이상 없음"
output_variable:
  description: 구조화된 정비 보고서 텍스트
  json_schema:
    type: object
    properties:
      summary: {type: string}
      recommendations: {type: array}
execution_settings:
  default:
    model_id: claude-sonnet-4
    temperature: 0.2
    max_tokens: 1000
```

출처: Microsoft Learn - YAML Schema Reference for Semantic Kernel Prompts

### 3-3. Prompt 내부 구조 (시스템 지시문 7개 섹션)

Anthropic의 공식 가이드(Context Engineering, 2025)와 AWS Bedrock 권고사항을 종합하면, 시스템 프롬프트의 내부 구조는 다음과 같이 구성된다:

| 섹션 | XML 태그 예시 | 내용 | 두산 예시 |
|---|---|---|---|
| 1. 역할·페르소나 | `<background_information>` | AI의 역할 정의, 전문성 수준 | "두산밥캣 장비의 유압계통 전문 진단 엔지니어" |
| 2. 지시문 | `<instructions>` | 수행할 작업의 구체적 규칙 | "진동 RMS값이 기준치 120%를 초과하면 즉시 교체 판정" |
| 3. 판단 기준 | `<decision_criteria>` | 경계값, 예외 처리, 우선순위 | "임박 경보 > 주의 > 정상 3단계 판정 기준표" |
| 4. 도구 안내 | `## Tool guidance` | 사용 가능한 도구와 사용 순서 | "먼저 get_sensor_data, 다음 check_maintenance_history" |
| 5. 출력 형식 | `## Output description` | 예상 출력의 구조·형식 | JSON 형식, 필수 필드 목록 |
| 6. 예외 처리 | `<exception_handling>` | 데이터 누락, 비정상 입력 처리 | "센서 값이 null이면 '데이터 부족'으로 판정 보류" |
| 7. 예시 | `<examples>` | 3~5개 Few-shot 예시 | 실제 과거 판정 사례 |

Anthropic 권고: "시스템 프롬프트는 '너무 구체적인 if-else 로직 하드코딩'과 '너무 추상적인 고수준 지시' 사이의 골디락스 존에 있어야 한다."

---

## 4. Harness의 정의와 구성

### 4-1. Harness란 무엇인가

**Harness (하네스)** = AI 에이전트에서 모델(LLM) 자체를 제외한 모든 실행 인프라. 즉 모델이 "어떻게 실행되는가"를 결정하는 실행 레이어.

- HuggingFace 공식 Agent 용어집(2026): "Harness: The execution layer inside the agent — it calls the model, handles its tool calls, decides when to stop."
- Parallel.ai 정의: "An agent harness is the software infrastructure that wraps around a large language model (LLM) or AI agent, handling everything except the model itself."
- Anthropic Claude Agent SDK 공식 설명: Claude Code(Claude의 코딩 에이전트)를 "a general-purpose agent harness"로 설명.

**쉬운 비유**: 사람이 공장 현장에서 작업할 때 필요한 것 = 작업복 + 공구함 + 절차서 + 체크리스트 + 통신장비. 이 모든 것이 "Harness"다. LLM은 그 사람 자신의 두뇌.

### 4-2. Scaffold와 Harness의 구분

HuggingFace의 공식 용어집이 명확하게 정의:

| 개념 | 정의 | 비유 |
|---|---|---|
| **Scaffold (스캐폴드)** | 모델의 행동을 결정하는 레이어: 시스템 프롬프트, 도구 설명, 응답 파싱 방식, 컨텍스트 관리. 모델이 세상을 어떻게 보는지를 결정. | 작업 지시서, 절차서 |
| **Harness (하네스)** | 실행 레이어: 모델을 호출하고, 도구 호출을 처리하고, 언제 멈출지 결정. 에이전트를 실제로 동작시키는 것. | 작업 도구함, 통신 시스템 |
| **Agent = Model + Harness** | 모델 자체가 아닌 모든 것이 Harness | 사람 + 작업 환경 전체 |

실무에서는 혼용되며, Claude Code·Codex 같은 제품들은 전체를 "harness"로 부른다. 둘을 구분할 필요가 있을 때만 scaffold/harness를 분리해서 사용.

### 4-3. 자산으로서의 Harness 구성 요소

Prompt/Harness 자산화 맥락에서 Harness는 다음 5가지 요소를 묶은 **재사용 가능한 업무 실행 패키지**다:

```
Harness 자산 패키지
├── [1] Prompt 집합 (System Prompt + User Prompt 템플릿)
│       예: 설비 판정 시스템 프롬프트, 사용자 요청 템플릿
├── [2] 입력 데이터 정의 (Input Data Schema)
│       예: {sensor_id, timestamp, vibration_rms, temperature}
├── [3] Tool 호출 순서 (Tool Call Sequence)
│       예: 1.get_sensor_data → 2.check_threshold → 3.query_history → 4.generate_report
├── [4] 평가 기준 (Evaluation Criteria)
│       예: {정확도 ≥ 90%, false_positive_rate ≤ 5%, 응답시간 ≤ 3s}
└── [5] 출력 포맷 (Output Format Specification)
        예: JSON {status: "교체필요|주의|정상", confidence: float, reason: string}
```

### 4-4. 실제 Harness 구조 예시 (AWS Bedrock 스타일)

```
prompts > predictive_maintenance_agent / v2
Agent:
  Instructions: "두산의 설비 예지보전 전문 진단 AI. 센서 데이터를 분석해 교체 시점을 판정한다."
  Tools:
    - get_real_time_sensor_data     # MES에서 현재 센서 데이터 조회
    - query_maintenance_history     # CMMS에서 과거 정비 이력 조회
    - check_specification_standard  # 설비 사양 기준치 조회
    - create_work_order            # ERP에 작업 지시 생성
  KnowledgeBase: EquipmentMaintenanceManuals
  EvaluationCriteria:
    accuracy: ">= 0.90"
    false_positive_rate: "<= 0.05"
  OutputFormat:
    type: JSON
    schema: {status, confidence, reason, recommended_action}
```

AWS Prescriptive Guidance는 이 구조를 "Prompt acts like the logic layer in traditional applications"으로 설명한다.

### 4-5. Anthropic의 장기 실행 Harness 구조 (2-에이전트 패턴)

Anthropic Engineering(2025. 11)이 공개한 장기 실행 Harness 패턴:

| 에이전트 유형 | 역할 | 프롬프트 특징 |
|---|---|---|
| **Initializer Agent** (초기화 에이전트) | 첫 번째 세션에서 환경 설정: feature_list.json, init.sh, progress.txt 생성, 초기 git commit | 환경 구조화·문서화에 초점 |
| **Coding Agent** (실행 에이전트) | 이후 세션마다 하나의 기능씩 점진적 구현, 세션 종료시 진행상황 기록 | 점진적 실행·상태 기록에 초점 |

핵심 통찰: "다음 세션이 완전히 새 컨텍스트로 시작할 때, 이전 진행상황을 어떻게 전달할 것인가" → 구조화된 artifacts(feature_list.json, progress.txt, git history)를 Harness 자산으로 관리.

---

## 5. 관련 표준·프레임워크·도구

### 5-1. 주요 Prompt 관리 플랫폼 비교표

| 플랫폼 | 유형 | 핵심 기능 | Prompt 자산 관리 특징 |
|---|---|---|---|
| **LangSmith / LangChain Hub** | 상용 + 오픈소스 | 커밋 기반 버전관리, 환경 태그(staging/production), 비교(Diff) | Git 스타일 commit hash + 태그로 버전 관리, 코드 변경 없이 프롬프트 업데이트 |
| **PromptLayer** | 상용 | 시각적 프롬프트 관리, 모델 불가지론 Blueprint, A/B 테스트 | 키-값 메타데이터(session_id, user_id 등), 비용/지연/피드백 추적 |
| **MLflow Prompt Registry (Databricks)** | 오픈소스 + 상용 | Git-like 버전관리, Unity Catalog 통합, aliases(production/staging) | 버전 불변성(immutable versions), alias로 무중단 배포, 거버넌스(접근제어) |
| **Amazon Bedrock Prompt Management** | 상용(AWS) | 버전 비교, 커스텀 메타데이터(author, department), 환경 분리 | Enterprise 메타데이터 태깅, 모델 교체 시 프롬프트 재사용 |
| **Langfuse** | 오픈소스 | 오픈소스 Prompt CMS, 추적/관찰 | 프롬프트-실행결과 연계 추적 |
| **Agenta** | 오픈소스 | LLM 프레임워크 호환성, 구조화 출력 | 프롬프트 A/B 테스트, 평가 파이프라인 |
| **IBM watsonx Prompt Lab** | 상용 | 프롬프트 템플릿 자산 관리, AI Factsheets 연동 | Develop/Evaluate/Operate 3단계 생애주기 추적 |
| **Microsoft Prompt Flow** | 상용 + 오픈소스 | YAML 스키마 정의, Semantic Kernel 통합 | 표준화된 YAML 스키마로 프롬프트 정의 |

### 5-2. 버전 관리 표준: Semantic Versioning (SemVer) 적용

Prompt 자산에 소프트웨어와 동일한 Semantic Versioning(MAJOR.MINOR.PATCH) 적용:

| 버전 구분 | 의미 | Prompt 적용 예시 | 두산 현업 예시 |
|---|---|---|---|
| **MAJOR (X.0.0)** | 하위 호환 깨지는 근본 변경 | 출력 형식 변경, 역할 변경, 구조적 재작성 | "판정 결과를 3단계에서 5단계로 변경" |
| **MINOR (x.Y.0)** | 기존 호환 유지하며 기능 추가 | 새 예시 추가, 지시문 확장, 새 변수 추가 | "계절적 온도 보정 로직 추가" |
| **PATCH (x.y.Z)** | 오탈자·소폭 개선, 동작 변화 없음 | 표현 명확화, 오타 수정 | "단위 표기 mm/s → mm/s(RMS)로 수정" |

MLflow Prompt Registry는 이를 "버전 불변성(immutable versions)" + "alias(production/staging)"로 구현. alias를 코드 변경 없이 이동시켜 배포.

### 5-3. ISO/IEC 42001과 Prompt 거버넌스

ISO/IEC 42001 (2023): AI 관리 시스템을 위한 최초의 국제 표준. Prompt를 포함한 AI 생애주기 전반의 위험 관리, 투명성, 윤리적 사용을 다룬다. Automation Anywhere 등 100개 기업이 인증을 취득 (2025).

2025년 3월, 국제표준화기구(ISO)는 기업 환경의 Prompt 생애주기 관리를 위한 프로토콜을 발표했다고 PromptFluent 등 업계 보도가 전한다.

---

## 6. 동작 원리와 라이프사이클

### 6-1. Prompt 자산 라이프사이클 (8단계)

```
1. 발굴(Discovery)
   └── 현장 업무 인터뷰·관찰로 AI 적용 가능 업무 절차·판단 기준 식별
       두산 예: "베어링 교체 시점 판단"은 숙련 기술자만 아는 암묵지

2. 변환(Conversion)
   └── 업무 절차 → Prompt 구조화 (역할·지시·판단기준·도구·출력 형식 작성)
       도구: Anthropic Console 프롬프트 생성기, Semantic Kernel 템플릿

3. 등록(Registration)
   └── 레지스트리에 v1.0.0으로 등록, 메타데이터 태깅
       예: MLflow register_prompt(), LangSmith 저장

4. 테스트(Testing)
   └── 다양한 입력값으로 검증, 경계 조건 테스트, 성능 기준선 수립
       A/B 테스트, 자동화 회귀 테스트

5. 배포(Deployment)
   └── staging → production 환경으로 승격
       alias 방식 (코드 변경 없이 버전 전환)

6. 모니터링(Monitoring)
   └── Prompt Drift 감지 (같은 Prompt라도 모델 업데이트로 출력이 달라질 수 있음)
       지연시간, 정확도, 비용 추적

7. 개선(Iteration)
   └── 피드백 루프 → 새 버전 작성 → 회귀 테스트 통과 → 배포
       Canary 배포 (5~10% 트래픽으로 먼저 검증)

8. 폐기/아카이브(Deprecation)
   └── 하위 호환 버전 유지 + 더 이상 사용되지 않는 버전 아카이브
```

### 6-2. Prompt Drift — 자산 관리가 없으면 생기는 문제

**Prompt Drift (프롬프트 드리프트)**: 프롬프트 자체를 수정하지 않아도, 기반 LLM이 업데이트되거나 입력 데이터 분포가 변하면 출력 품질이 서서히 저하되는 현상.

이것이 바로 "Prompt를 데이터 자산으로 관리해야 하는 이유"다:
- 버전 관리 없이는 드리프트 발생 시점 추적 불가
- 기준선(baseline) 없이는 "언제부터 나빠졌는지" 모름
- 황금 테스트 케이스(golden test cases) 없이는 자동 감지 불가

AWS Prescriptive Guidance: "Without proper lifecycle management, enterprises face: drift in behavior due to model or prompt changes, data leakage, undetected degradation in accuracy, lack of reproducibility or traceability."

### 6-3. 컨텍스트 엔지니어링 (Context Engineering) — Prompt 설계의 진화

Anthropic Engineering(2025. 9): "Building with language models is becoming less about finding the right words and phrases for your prompts, and more about answering the broader question of 'what configuration of context is most likely to generate our model's desired behavior?'"

컨텍스트 = 시스템 프롬프트 + 도구 정의 + MCP + 외부 데이터 + 메시지 이력 전체.

**Context Engineering 핵심 원칙**: "가장 작은 고신호(high-signal) 토큰 집합으로 원하는 결과를 내는 것."

이는 Prompt 자산화 관점에서 중요한 함의를 갖는다: Prompt를 단독 텍스트가 아니라, 컨텍스트 전체 설계의 핵심 구성요소로 봐야 한다.

---

## 7. 자산화가 없을 때 현장에서 생기는 문제 — 인과 관계

### 7-1. "Prompt Spaghetti" 상태

자산 관리 없이 AI를 도입한 기업의 전형적 증상 (Martin Rodek, 2025):

| 증상 | 원인 | 결과 |
|---|---|---|
| 프롬프트가 개인 노트/슬랙에 산재 | 중앙 레지스트리 없음 | 같은 업무를 팀마다 다르게 AI 지시 → 일관성 없음 |
| 버전 추적 불가 | 버전 관리 없음 | 언제 왜 AI 출력이 바뀌었는지 모름 |
| 누가 어떤 프롬프트 쓰는지 모름 | 소유권 불명확 | 담당자 퇴사하면 AI 업무 지식 사라짐 |
| 모델 업데이트 후 조용히 성능 저하 | Prompt Drift 모니터링 없음 | 현장에서 이상 결과 발견 후에야 인지 |
| 컴플라이언스 감사 대응 불가 | 감사 추적 없음 | "AI가 왜 이 판단을 했는가" 설명 불가 |

두산 제조 현업 가상 시나리오:
> 테스나 공정 품질 검사 팀이 AI 불량 판정 Prompt를 엔지니어 A가 만들어 사용 중. 엔지니어 A가 퇴사하자 Prompt 소재 파악 불가. 신입이 처음부터 다시 만들었지만 기준이 달라져 불량율이 갑자기 튀었음. 품질 감사에서 "과거 판정 기준이 무엇이었냐"는 질문에 답 못함.

### 7-2. 자산화 도입 후 기대 효과

| 지표 | 목표 수준 (업계 기준) |
|---|---|
| Prompt 재사용률 (부서 간) | 40~60% |
| 새 AI 기능 프로덕션 배포 기간 | 주 단위 → 일 단위 |
| AI 지원 업무 비율 | 24개월 내 80% |
| 운영 효율화 생산성 향상 | 20~30% |
| 보안 사고 (취약 Prompt 기인) | 제로 목표 |

---

## 8. 성숙도 단계 (Prompt/Harness 자산화 성숙도 모델)

업계 PromptOps 문헌과 조직 성숙도 모델을 종합하여 5단계로 정리:

| 단계 | 명칭 | 특징 | 두산 현업 적용 모습 |
|---|---|---|---|
| **Level 1** | 임시방편 (Ad-hoc) | 프롬프트가 개인 노트·슬랙에 산재, 관리 없음 | AI 파일럿 시도는 있으나 표준 없음 |
| **Level 2** | 관리됨 (Managed) | 공유 폴더나 Git에 저장, 기초 버전 관리 시작 | 팀 내 Google Drive에 prompt.txt 저장 |
| **Level 3** | 표준화됨 (Standardized) | 중앙 레지스트리, 메타데이터 스키마, 소유권 정의 | LangSmith/MLflow에 등록, 담당자·태그 관리 |
| **Level 4** | 자동화됨 (Automated) | CI/CD 통합, 자동 회귀 테스트, Drift 모니터링 | Prompt 변경 시 자동 테스트 → 스테이징 배포 |
| **Level 5** | 최적화됨 (Optimizing) | A/B 테스트 상시화, 자동 최적화, 조직 전체 재사용 | 계열사 공통 Prompt 라이브러리, DSPy 자동 최적화 |

---

## 9. 핵심 용어집

| 용어 (English) | 한국어 | 한 줄 풀이 |
|---|---|---|
| **Prompt** | 프롬프트 | AI에게 보내는 지시·질문 텍스트. 업무 맥락에서는 "AI에게 주는 작업 지시서" |
| **System Prompt** | 시스템 프롬프트 | 역할·규칙·출력 형식 등 AI의 행동을 결정하는 초기 설정 지시문 |
| **Prompt Template** | 프롬프트 템플릿 | 런타임에 변수로 채워지는 재사용 가능한 프롬프트 구조. 예: "{{설비명}}의 상태를 분석하라" |
| **Harness** | 하네스 | LLM을 제외한 에이전트 실행 인프라 전체. 도구 호출·메모리·컨텍스트 관리·검증 포함 |
| **Scaffold** | 스캐폴드 | 모델의 행동 방식을 결정하는 레이어: 시스템 프롬프트, 도구 설명, 응답 파싱 방식 |
| **PromptOps** | 프롬프트옵스 | DevOps를 모델로 한 프롬프트 생애주기 관리 방법론. 설계→테스트→배포→모니터링→개선 |
| **Prompt Registry** | 프롬프트 레지스트리 | 조직의 프롬프트를 중앙에서 등록·검색·버전관리하는 저장소 |
| **Prompt Drift** | 프롬프트 드리프트 | 프롬프트 자체를 바꾸지 않아도 LLM 업데이트나 입력 변화로 성능이 서서히 저하되는 현상 |
| **Semantic Versioning** | 시맨틱 버전닝 | MAJOR.MINOR.PATCH 형식으로 변경의 심각도를 표시하는 버전 표기 체계 |
| **Context Engineering** | 컨텍스트 엔지니어링 | LLM 추론 시 컨텍스트 창에 들어가는 내용(프롬프트+도구+데이터+이력)을 최적화하는 기술 |
| **Few-shot Prompting** | 퓨샷 프롬프팅 | 프롬프트 안에 3~5개의 예시를 넣어 AI가 원하는 패턴을 학습하게 하는 기법 |
| **Tool Call** | 도구 호출 | AI가 프롬프트 처리 중 외부 API·함수·데이터베이스를 호출하는 행위 |
| **Prompt Alias** | 프롬프트 별칭 | production, staging 같이 특정 버전을 가리키는 가변 레이블. 코드 변경 없이 버전 전환 가능 |
| **Canary Deployment** | 카나리 배포 | 새 Prompt 버전을 소수(5~10%) 트래픽에 먼저 적용해 안전성 확인 후 전체 배포 |
| **Prompt Hygiene** | 프롬프트 위생 | 조직 전체에 걸쳐 일관된 프롬프트 품질 기준을 유지하는 관행 |
| **LLMOps** | LLM옵스 | LLM 기반 애플리케이션의 개발·배포·운영·모니터링 전체 관리 체계 |
| **MCP (Model Context Protocol)** | 모델 컨텍스트 프로토콜 | Anthropic이 주도하는 AI 에이전트의 도구 호출 표준화 프로토콜 |
| **Context Compaction** | 컨텍스트 압축 | 긴 대화 이력을 요약해 컨텍스트 창 제한을 극복하는 기법 |

---

## 출처

1. Shmueli, S. (2025). "Managing Prompts as Enterprise Assets: A Portfolio Approach." Medium.  
   https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327

2. Juršėnas, J. (2025). "Prompt Engineering Is Dead – Long Live PromptOps." Dataversity.  
   https://www.dataversity.net/articles/prompt-engineering-is-dead-long-live-promptops/

3. Paniego, S. & Gosthipaty, A. R. (2026). "Harness, Scaffold, and the AI Agent Terms Worth Getting Right." HuggingFace Blog.  
   https://huggingface.co/blog/agent-glossary

4. Parallel Web Systems. (2025). "What is an agent harness in the context of large-language models?"  
   https://parallel.ai/articles/what-is-an-agent-harness

5. Anthropic Engineering. (2025). "Effective context engineering for AI agents."  
   https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents

6. Anthropic Engineering. (2025). "Effective harnesses for long-running agents."  
   https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents

7. AWS Prescriptive Guidance. "Prompt, agent, and model lifecycle management."  
   https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html

8. Microsoft Learn. "YAML Schema Reference for Semantic Kernel Prompts."  
   https://learn.microsoft.com/en-us/semantic-kernel/concepts/prompts/yaml-schema

9. LangChain Documentation. "Prompt engineering concepts - LangSmith."  
   https://docs.langchain.com/langsmith/prompt-engineering-concepts

10. MLflow / Databricks. "Prompt Registry."  
    https://docs.databricks.com/aws/en/mlflow3/genai/prompt-version-mgmt/prompt-registry/

11. Amazon Web Services. "Amazon Bedrock Prompt Management."  
    https://aws.amazon.com/bedrock/prompt-management/

12. Rodek, M. (2025). "Why Large Enterprises Need a Prompt Registry for AI Governance." Medium.  
    https://medium.com/@martin_rodek/why-large-enterprises-need-a-prompt-registry-for-ai-governance-04d744039bb4

13. Anthropic. "Prompt engineering overview - Claude API Docs."  
    https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview

14. IBM. "Prompt Template Manager - IBM watsonx.ai."  
    https://ibm.github.io/watsonx-ai-python-sdk/prompt_template_manager.html

15. Databricks Community. "Revolutionizing GenAI Application Management: MLflow 3 Prompt Registry."  
    https://community.databricks.com/t5/technical-blog/revolutionizing-genai-application-management-mlflow-3-prompt/ba-p/139606

16. Arize AI. "Top 5 AI Prompt Management Tools of 2025."  
    https://arize.com/blog/top-5-ai-prompt-management-tools-of-2025/

17. ZenML Blog. "Prompt Engineering & Management in Production: Practical Lessons from the LLMOps Database."  
    https://www.zenml.io/blog/prompt-engineering-management-in-production-practical-lessons-from-the-llmops-database

18. TestRigor. "Why DevOps Needs a 'PromptOps' Layer in 2026."  
    https://testrigor.com/blog/why-devops-needs-a-promptops-layer/

19. McKinsey & Company. "The State of AI: How organizations are rewiring to capture value." (2024).  
    https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai

20. Agenta AI Blog. "Prompt Drift: What It Is and How to Detect It."  
    https://agenta.ai/blog/prompt-drift

21. LangSmith. "Manage prompts - LangChain Documentation."  
    https://docs.langchain.com/langsmith/manage-prompts

22. MindStudio. "What Is an AI Agent Harness? The Architecture Behind Stripe's 1,300 Weekly AI Pull Requests."  
    https://www.mindstudio.ai/blog/what-is-ai-agent-harness-stripe-minions

23. PromptLayer Documentation. "Prompt Blueprints."  
    https://docs.promptlayer.com/running-requests/prompt-blueprints

24. PromptLayer Documentation. "Metadata."  
    https://docs.promptlayer.com/features/prompt-history/metadata

25. Nayak, P. (2025). "Harness Engineering: Building the Operating System for Autonomous Agents." The AI Forum, Medium.  
    https://medium.com/the-ai-forum/harness-engineering-building-the-operating-system-for-autonomous-agents-1e20c105f689
