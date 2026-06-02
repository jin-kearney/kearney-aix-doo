# C-3 데이터 계통(Data Lineage) — 스토리라인

> 장표 원천 문서 | 청중: 제조업 현업 실무자 | 작성일: 2026-06-02

---

## 1. Executive Summary

### [슬라이드 1] 데이터의 이력서 — "이 숫자, 어디서 왔고 어디로 가는가"

**핵심 메시지**: C-3 데이터 계통(Data Lineage)은 데이터의 출처·이동·변환·활용 이력을 기록해, AI 결과의 근거를 역추적할 수 있게 하는 체계다.

- 리니지 = 데이터의 이력서. 마트 고기의 "○○농장 → △△도축장 → □□유통" 라벨처럼, 우리 데이터에도 같은 추적표를 붙이는 일
- 현업이 즉시 답할 수 있게 되는 세 질문:
  1. **"이 대시보드 숫자, 왜 이렇게 나왔지?"** → 상류 추적(근본원인 분석)
  2. **"이 센서를 교체하면 어떤 보고서·모델이 깨지지?"** → 하류 추적(영향 분석)
  3. **"이 AI가 왜 이렇게 답했지? 근거가 뭐지?"** → AI 답변 근거 역추적(Citation)
- 범위: **연결성·추적성**만 담당. 실시간 이상 감지(C-1), 사용 가능 판정(C-2)은 별도
- 핵심 원칙: 리니지는 "한 번 그려 붙이는 그림"이 아니라 **데이터가 흐를 때마다 자동 갱신되는 지도**
- 5개 Key Question: (1) 대상 흐름 선정 (2) 변환 이력 기록 (3) AI 답변 근거 역추적 (4) 영향도 분석 (5) 감사 Lineage Report

---

## 2. Why

> **이 섹션의 관점**: Why는 "리니지가 무엇이고 어떻게·왜 그렇게 구성되는가"라는 개념·구조로 필요성을 설명한다. 우리 조직의 정량 현황(인시던트 빈도, 추적 소요 시간, 비용 등)은 발표 본편의 **현황(as-is) 분석**에서 채운다. → **[현황에서 보강]**

### [슬라이드 2] 리니지는 데이터의 "출처→이동→변환→활용"을 잇는 지도다

**핵심 메시지**: 리니지는 데이터가 어디서 와서, 어떻게 바뀌고, 어디에 쓰이는지를 점(노드)과 화살표(엣지)로 이은 지도다. 이 **연결 구조 자체가 추적 가능성을 만든다**.

- **구성**: 노드(데이터·변환 단계) + 엣지(흐름 방향). 데이터는 사이클 없이 한 방향으로 흐르는 DAG(방향성 비순환 그래프) 형태
- **4요소를 빠짐없이 연결**: 출처(Origin) → 이동(Movement) → 변환(Transformation) → 활용(Usage)
- **왜 이렇게 구성하나**: 현장 데이터 한 칸은 한 번 쓰고 끝나는 게 아니라, 여러 단계를 거쳐 보고서·AI 답변이 된다. 그 사슬을 통째로 이어둬야 *사슬 어느 지점이든* 짚을 수 있다 — 중간이 끊기면 추적도 끊긴다
- **이 구조가 없으면**: "이 숫자 어디서 왔나"를 사람의 기억과 수소문으로 찾게 되고, 원리적으로 추적을 보장할 수 없다

### [슬라이드 3] 같은 지도를 두 방향으로 읽는다 — upstream과 downstream

**핵심 메시지**: 리니지 지도는 위로 읽으면 **근본원인**(upstream), 아래로 읽으면 **영향범위**(downstream)를 답한다. 이 두 방향이 리니지가 작동하는 핵심 원리다.

| 방향 | 뜻 | 답하는 질문 | 쓰는 시점 |
|---|---|---|---|
| **Upstream (상류)** | 현재 값에서 출처로 거슬러 올라감 | "이 값/판정은 어디서 왔나?" | 문제가 **난 뒤** — 근본원인 분석 |
| **Downstream (하류)** | 현재 노드에서 따라 내려감 | "이걸 바꾸면 뭐가 깨지나?" | 변경 **하기 전** — 영향 분석 |

- **왜 두 방향인가**: 문제는 "이미 난 뒤"(원인 규명)와 "내기 전"(영향 예방) 양쪽에서 생긴다. 같은 지도 하나로 두 상황을 모두 처리한다
- **제조 예시**: 진동 센서 단위를 바꿨을 때 → upstream으로 이상 판정의 발원 센서를 추적, downstream으로 영향받는 PdM 모델·대시보드·Prompt를 사전 파악
- **이 구조가 없으면 원리적으로 불가능해지는 것**: 근본원인 분석, 변경 영향 분석, 그리고 AI 답변의 근거 역추적 — 셋 다 "사슬을 따라가는" 동작이라 리니지 없이는 성립하지 않는다

### [슬라이드 4] C-3가 달성하는 5가지 핵심 목표

**핵심 메시지**: 데이터 리니지를 구축하면 추적성·영향 분석·AI 설명가능성·감사 대응·데이터 신뢰 5가지를 모두 얻는다.

| #   | 목표                            | 핵심 질문           | 제조 맥락 효익                                |
| --- | ----------------------------- | --------------- | --------------------------------------- |
| 1   | **추적성** (Traceability)        | 이 숫자 어디서 왔나?    | "베어링 위험 73%" 판정의 발원 센서까지 즉시 역추적         |
| 2   | **영향 분석** (Impact Analysis)   | 이걸 바꾸면 뭐가 깨지나?  | 센서 교체 전, 하류 PdM 모델·대시보드·Prompt 목록 사전 파악 |
| 3   | **AI 설명가능성** (Explainability) | AI 답변 근거가 뭔가?   | 품질 RAG 답변마다 참조 문서·청크·테이블 역추적            |
| 4   | **감사 대응** (Audit Readiness)   | 증거를 즉시 뽑을 수 있나? | EU AI Act·내부 감사에 "의도" 아닌 "증거"로 대응       |
| 5   | **데이터 신뢰** (Data Trust)       | 이 데이터를 믿어도 되나?  | 데이터 소비자(현업·분석가·AI)의 신뢰 기반 확보            |

---

## 3. When

### [슬라이드 5] 이 신호가 보이면 즉시 시작해야 한다

**핵심 메시지**: 리니지 구축의 최적 시점은 "준비가 완료된 시점"이 아니라 "이 트리거 중 하나가 터진 시점"이다.

| 트리거 | 구체적 신호 | 왜 지금인가 |
|---|---|---|
| **AI 과제 착수** | 예지보전(PdM), 품질 RAG, 공정 최적화 AI 첫 프로젝트 | AI 답변 근거 로그는 처음부터 설계해야 함. 나중에 붙이기 거의 불가 |
| **데이터 스택 복잡화** | 5개 이상 소스·테이블 연결, 도메인팀 간 데이터 재사용 시작 | 5개 테이블 수작업 추적과 100개 추적은 차원이 다름 |
| **반복 인시던트** | "이 숫자 왜 이렇게 나왔지?" 파악에 며칠 소요 | 인시던트 해소 지연은 리니지 부재의 가장 명확한 징후 |
| **규제·감사 대응** | ISO, 품질 인증, EU AI Act, 고객사 감사 | Article 10 전면 시행(2026.8) 이전 파이프라인 증거 필수 |
| **원천 변경 빈발** | 센서 교체·단위 변경·MES 업그레이드 잦을 때 | 변경 전 영향도 파악 없이 진행 시 연쇄 장애 리스크 |

> **OT→IT 경계 특이사항**: 센서 태그(historian tag) ID와 웨어하우스 컬럼 간 매핑이 누군가의 기억에만 있다면, 이미 늦었다. 지금 당장 수동 매핑표 작성을 시작해야 한다.

### [슬라이드 6] 성숙도 단계에 따라 시작점과 목표가 다르다

**핵심 메시지**: 리니지 적용 수준은 조직 성숙도에 맞춰 단계적으로 올라간다. 처음부터 완벽할 필요 없다.

| 성숙도 단계 | 조직 특성 | 리니지 수준 | 제조업 시작점 |
|---|---|---|---|
| **Level 1 — 반응적** | 인시던트 후 수작업 대응, 경로 기억에 의존 | 없거나 산발적 | 핵심 AI 과제 1개 ①~⑦ 사슬을 화이트보드로 그리기 |
| **Level 2 — 카탈로그** | 데이터 카탈로그 도입, 흐름 체계적 추적 시작 | 테이블 단위 자동 캡처 | OpenLineage 기반 파이프라인 런타임 캡처 도입 |
| **Level 3 — 관리** | 컬럼 단위 리니지, 거버넌스 정책 조율 | 컬럼 단위 + AI 답변 근거 로그 | AI 서비스 전체 Citation 구조 적용 |
| **Level 4 — 최적화** | 리니지가 일상 워크플로에 내재화 | 실시간 갱신, 자동 알림, 감사 리포트 버튼 | 리니지 리포트 즉시 출력, 변경 전 영향도 조회 의무화 |

---

## 4. What

### [슬라이드 7] 리니지는 DAG(방향성 비순환 그래프) — 노드와 엣지로 구성된다

**핵심 메시지**: 데이터 리니지는 본질적으로 DAG(방향성 비순환 그래프)다. 데이터 자산을 노드로, 데이터 흐름을 엣지로 표현한다.

| 요소 | 정의 | 제조 예시 |
|---|---|---|
| **데이터 노드** | 하나의 데이터 자산 (테이블, 파일, 컬럼, 청크, AI 답변 등) | `raw_vibration` 테이블, `avg_temp` 컬럼, chunk_8842 |
| **변환 노드** | 데이터를 변환하는 처리 단계 (ETL/ELT, SQL, Python, AI 추론) | 1분 RMS 집계 SQL, PdM 모델 추론 |
| **엣지(Edge)** | 노드 간 데이터 흐름의 방향과 관계 | `raw_vibration` → (RMS 집계) → `feat_vib_rms_x` |

- 방향성: 상류(Upstream) ← 역추적 ← 현재 노드 → 추적 → 하류(Downstream)
- 비순환: 데이터는 사이클 없이 한 방향으로만 흐름 (단방향 인과 관계)
- 그래프 형태: 여러 노드가 연결·분기·합류하며 복잡한 의존 관계를 표현

```
진동 센서(PLC-3, 태그 AI_0207)
   → raw_vibration (MES 수집 테이블)
   → feat_vib_rms_x (1분 RMS 집계, 이상치 제거 v2.3)
   → PdM 모델 입력
   → "베어링 결함 위험 73%" 판정
```

### [슬라이드 8] 테이블 단위 vs. 컬럼 단위 — 목적에 따라 세분성이 다르다

**핵심 메시지**: 테이블 단위는 "전체 그림", 컬럼 단위는 "정밀 외과". 영향 분석과 AI 근거 추적에는 컬럼 단위가 필수다.

| 구분 | 테이블/데이터셋 단위 | 컬럼/필드 단위 |
|---|---|---|
| **의미** | 어떤 테이블이 어떤 테이블로 흘러가는가 | 어떤 컬럼이 어떤 컬럼에서 어떤 수식으로 파생됐는가 |
| **세분성** | 낮음 (Big Picture) | 높음 (Granular) |
| **주요 용도** | 전체 흐름 파악, 빠른 의존성 확인 | 정밀 디버깅, 영향 분석, 규제 감사, AI 근거 추적 |
| **영향 분석** | "5개 테이블이 연결됨" | "이 특정 컬럼에 의존하는 2개 리포트만 영향받음" |
| **도구 지원** | 대부분 도구 지원 | 지원 도구 한정적, 구현 복잡도 높음 |
| **예시** | MES_RAW → DW_QUALITY | `avg_temp = (sensor_temp℉−32)×5/9`의 5분 평균 |

- **핵심 격언**: "테이블 단위 리니지는 5개 테이블이 연결됐다고 알려준다. 컬럼 단위 리니지는 그 중 특정 컬럼에 실제로 의존하는 2개 테이블만을 알려준다"
- 실무 권장: 처음에는 테이블 단위로 넓게 깔고, CDE(핵심 데이터 요소)는 컬럼 단위까지 드릴다운

### [슬라이드 9] 같은 지도, 두 방향 — 근본원인 분석과 영향 분석

**핵심 메시지**: 리니지 지도는 읽는 방향에 따라 근본원인 분석(상류) 또는 영향 분석(하류) 도구가 된다.

| 방향 | 질문 | 활용 시점 | 제조 예시 |
|---|---|---|---|
| **상류 추적 (Upstream)** | 이 숫자/판정이 어디서 왔는가? | 문제 발생 후 원인 분석 | "베어링 위험 73%"가 이상할 때 → 역추적 → 센서 AI_0207의 교정 오류 발견 |
| **하류 추적 (Downstream)** | 이것을 바꾸면 뭐가 깨지는가? | 변경 전 영향도 확인 | 센서 교체(단위 변경) 전 → 하류 PdM 모델·대시보드·Prompt 목록 자동 추출 |

- 동일 노드라도 문맥에 따라 상류/하류가 결정됨: dbt 모델은 소스 테이블의 하류지만, Looker 대시보드의 상류
- **DataHub** Lineage Impact Analysis가 지원하는 기능:
  - 스키마 변경 파급 범위 사전 파악
  - 실패한 파이프라인의 하류 의존성 파악
  - 예상치 못한 데이터 품질 문제의 상류 원인 추적

### [슬라이드 10] AI 활용 사슬 전체 — ①원천에서 ⑦AI 답변까지

**핵심 메시지**: AI-Ready 리니지는 기존의 "원천→보고서"를 넘어, RAG 인덱스~AI 답변까지 사슬 전체를 대상으로 한다. ⑤~⑦ 구간이 AI 시대의 新 리니지 영역이다.

| 단계 | 무엇인가 | 제조 예시 | 리니지 역할 |
|---|---|---|---|
| ① 원천 데이터 | 센서·설비·시스템 원본 | 진동 센서 AI_0207, MES 생산실적, C/S Report 원문 | 출처 기록 |
| ② 전처리 결과 | 정제·구조화된 중간 산출물 | 1분 RMS 시계열, PDF에서 추출한 표·청크 | 변환 이력 |
| ③ 분석 데이터셋 | 모델·분석에 쓰는 가공 테이블 | `feat_vib_rms_x`, 결함 원인 학습셋 | 파생 로직 |
| ④ 보고서·대시보드 | 사람이 보는 산출물 | 설비 건전성 대시보드, 품질 일일 리포트 | 활용 추적 |
| ⑤ RAG 인덱스 | AI 검색용으로 적재된 문서 | SOP·PFMEA·C/S Report 벡터 인덱스 | 인덱스 버전 |
| ⑥ Prompt 입력 | AI에 실제로 들어간 데이터 | "이 결함 원인 분석해줘" + 첨부 데이터 | 프롬프트 버전 |
| ⑦ AI 답변 | 최종 생성 결과와 그 근거 | "소재 결함 가능성 높음 + 근거 3건" | **답변 근거 로그** |

- ①~④: 기존 전통 데이터 리니지 영역
- **⑤~⑦: AI 시대에 새로 추가된 구간** — 이 뒷부분이 Citation(KQ3)의 핵심

### [슬라이드 11] OpenLineage — 업계 사실상 표준이 된 리니지 메타데이터 공통 언어

**핵심 메시지**: OpenLineage는 도구가 아니라 "리니지를 주고받는 공통 언어(JSON Schema 스펙)"다. 어떤 파이프라인 도구를 써도 같은 형식으로 이력을 수집할 수 있게 해준다.

- 2020년 시작된 오픈소스 표준. 현재 업계 사실상 표준(De Facto Standard)
- **3개 기본 엔티티**:

| 엔티티 | 정의 | 예시 |
|---|---|---|
| **Job** | 데이터를 소비하고 생성하는 프로세스 정의 | Airflow DAG 태스크, Spark Job, dbt 모델 |
| **Run** | Job의 특정 시점 실행 인스턴스 | run_id로 추적되는 개별 실행 |
| **Dataset** | Job의 입력/출력 데이터 자산 | PostgreSQL 테이블, S3 버킷, Kafka 토픽 |

- **Facet(패싯)**: Job·Run·Dataset에 붙이는 원자 단위 확장 메타데이터. 모듈식으로 자유 확장
- **ColumnLineageDatasetFacet**: 출력 컬럼이 어느 입력 컬럼에서 어떻게 만들어졌는지 기록
  - DIRECT: 출력 컬럼 값이 입력 컬럼에서 직접 파생
  - INDIRECT: 입력 컬럼의 영향을 받지만 직접 파생은 아님
- **Producer(생성)**: Apache Airflow, Spark, Flink, dbt, Dagster, Snowflake, BigQuery
- **Consumer(소비)**: Marquez, Datadog, Atlan, OpenMetadata, DataHub

### [슬라이드 12] 관련 표준·프레임워크 — OpenLineage 생태계와 거버넌스 기반

**핵심 메시지**: 리니지 표준은 OpenLineage를 중심으로, Marquez(참조 구현체)·DataHub·OpenMetadata·Egeria 등 다양한 계층으로 구성된다.

| 표준/프레임워크 | 역할 | 특징 |
|---|---|---|
| **OpenLineage** | 리니지 메타데이터 교환 표준 (스펙) | JSON Schema 기반. 벤더 중립. 업계 De Facto |
| **Marquez** | OpenLineage 공식 참조 구현체 (Reference Implementation) | LF AI & DATA 인큐베이션. PostgreSQL 기반. REST API + 기본 UI |
| **DataHub** | 그래프 기반 메타데이터 플랫폼 | LinkedIn 개발. JanusGraph + Elasticsearch. Lineage Impact Analysis 워크플로 |
| **OpenMetadata** | 엔티티 우선(entity-first) 통합 리니지 | API-first. No-Code 수동 편집기 병행 |
| **Apache Atlas** | Hadoop 생태계 gold standard | Hive·Kafka·Spark 네이티브 후크. Ranger와 통합 |
| **Egeria (Linux Foundation)** | 개방형 메타데이터 거버넌스 표준 | Apache 2.0 라이선스. 벤더 중립 상호운용성 |
| **DAMA-DMBOK2** | 데이터 관리 글로벌 지식 체계 | 리니지는 8장(통합·상호운용성)의 Plan & Analyze 핵심 활동 |

- **2024~2025 주요 동향**: Kafka Summit 2024에서 스트림 처리 리니지 핵심 주제 부상. IBM이 구조화·비구조화 데이터 통합 리니지 지원을 위해 OpenLineage 스펙 확장 기여

### [슬라이드 13] AI 시대의 새로운 리니지 — RAG 파이프라인 프로베넌스(Provenance)

**핵심 메시지**: LLM/RAG는 확률적 생성을 하므로 동일 입력도 다른 출력을 낸다. 이를 추적하는 AI 리니지(AI Lineage)가 전통 리니지와 구별되는 AI 시대의 핵심 요구사항이다.

**RAG 파이프라인 리니지 체인:**

```
원천 문서/데이터
  → (전처리) 청크 생성 + 메타데이터 태깅
  → (인덱싱) 벡터 임베딩 → 벡터 스토어 (index_version 기록)
  → (검색) 사용자 질문 → 유사도 검색 → 관련 청크 N개 선택
  → (생성) [청크 + 프롬프트 + LLM] → AI 답변
  → (기록) 답변 근거 로그(answer_id에 귀속)
```

- 2025년 실무 검증된 RAG 프로베넌스 패턴:
  - **저장 단계**: doc_id, 섹션 경로, 라인 범위, 버전을 함께 저장
  - **생성 단계**: 답변 본문에 섹션 이름과 버전을 인용해 모든 주장을 역추적 가능하게 함
  - **컬럼 단위 연계**: LLM이 검색하는 모든 정보에 추적 가능한 프로베넌스를 컬럼 단위로 제공

- **IBM watsonx.data**: OpenLineage를 구조화 + 비구조화 데이터 통합 리니지로 확장 — 신뢰할 수 있는 메타데이터·리니지가 AI를 설명가능(Explainable)하게 만든다는 접근

### [슬라이드 14] 핵심 용어 정리 — 현업 눈높이에서

**핵심 메시지**: 리니지 실무를 위해 반드시 알아야 할 핵심 용어 정리.

| 용어 | 뜻 | 현업 활용 맥락 |
|---|---|---|
| **리니지 (Data Lineage)** | 데이터의 출처·이동·변환·활용 이력을 점·화살표로 이은 추적 지도 | "이 컬럼 어디서 왔어요?" 대답 가능 |
| **상류 (Upstream)** | "어디서 왔나" — 근본원인 분석 방향 | 이상 값 발원지 역추적 |
| **하류 (Downstream)** | "무엇이 영향받나" — 영향 분석 방향 | 변경 전 파급 범위 확인 |
| **테이블/컬럼 단위** | 큰 흐름 지도 / 개별 칸의 파생식까지 추적 | 컬럼 단위가 AI 근거 추적에 필수 |
| **CDE** | 핵심 데이터 요소. 리니지를 우선·정밀 적용하는 대상 | AI 과제·안전·품질·규제 관련 데이터 |
| **스튜어드 (Steward)** | 기술과 비즈니스를 잇고 리니지를 검증·보강하는 책임자 | "왜 이렇게 했는가"를 아는 사람 |
| **변환 노드/엣지** | 리니지 지도의 점(데이터)과 화살표(변환) | DAG의 기본 구성 요소 |
| **Citation** | AI 답변·보고서에 근거 출처를 함께 표기하는 구조 | "이 답변 근거는 ①②③" 표시 |
| **답변 근거 로그** | AI 답변 한 건이 어떤 입력에서 나왔는지 기록한 이력 | answer_id로 retrieved_chunks 역추적 |
| **OpenLineage** | 도구 간 리니지를 주고받는 개방형 공통 양식(표준) | Airflow·Spark·dbt 어디서도 같은 형식 |
| **OT→IT 경계** | 센서·SCADA(OT)에서 웨어하우스·AI(IT)로 넘어가는 구간 | 제조 리니지의 최대 난관 |
| **프로베넌스 (Provenance)** | 데이터가 어떻게 생성됐는지를 기록하는 메타데이터 (리니지와 상호 보완) | 과학 데이터·AI 결과의 재현성 검증 |

---

## 5. How

### [슬라이드 15] 리니지 구축 6단계 — CDE 선정에서 전사 자동화까지

**핵심 메시지**: 리니지는 0단계(준비)에서 5단계(자동화)까지 순서대로 밟는다. 처음부터 전부 하려다 포기하지 말고, AI 과제 1개의 끝-끝(End-to-End) 파일럿으로 시작한다.

| 단계           | 이름                  | 핵심 활동                                                                           | 완료 조건                                 |
| ------------ | ------------------- | ------------------------------------------------------------------------------- | ------------------------------------- |
| **0. 준비**    | Scoping & Ownership | CDE 목록 확정, 스튜어드 지정, 도구 선택                                                       | CDE 목록 문서화, 스튜어드 책임 배정 완료             |
| **1. 파일럿**   | 단일 AI 과제 End-to-End | AI 과제 1개 선정 → 원천~AI 답변 ①~⑦ 전 구간 리니지 적용                                          | 파이프라인 변경 후에도 리니지가 자동 갱신되는지 검증         |
| **2. 교차 연결** | 시스템 경계 연결 우선        | 원천→전처리→분석셋→보고서 구간을 시스템 경계 횡단 지점 중심 테이블 단위 연결. OT→IT 경계 수동 매핑표 포함                | 경계 횡단 흐름 90% 이상 노드 연결 완료              |
| **3. 컬럼 심화** | 컬럼 단위 리니지           | CDE와 영향도 분석 대상 자산을 컬럼 단위까지 드릴다운                                                 | 핵심 파생 컬럼의 "입력 컬럼 + 변환 로직" 기록 비율 ≥ 80% |
| **4. AI 구간** | AI 답변 근거 Logging    | RAG 답변 근거 로그 스키마 적용, Citation 화면 표기, retrieved_chunks·tables_used·tool_calls 기록 | 임의 AI 답변 선택 시 answer_id로 근거 역추적 가능    |
| **5. 자동화**   | 운영 자동화              | 영향도 조회 의무화(변경 전 승인 프로세스), Lineage Report 버튼화, 리니지 갱신 자동화                        | 사람의 수작업 없이 영향도 목록이 생성되는지 확인           |

- **핵심 원칙 1**: 한 시스템만 깊게 파지 말고, 시스템 경계를 먼저 이어라. 최악의 사고는 경계에서 난다
- **핵심 원칙 2**: 리니지는 고장났을 때만이 아니라 카탈로그 검색·자산 화면에 평소에도 상시 노출

### [슬라이드 16] CDE(핵심 데이터 요소) — 어디에 먼저 리니지를 걸 것인가

**핵심 메시지**: 모든 데이터에 리니지를 걸 수는 없다. CDE 선정 기준으로 "먼저 걸 대상"을 구별하는 것이 현실적인 시작점이다.

**CDE 선정 4가지 기준:**
- AI 과제(PdM, 품질 RAG, 공정 최적화 등)에 **실제로 입력**되는 피처·문서
- 오류 시 **안전·품질·규제**에 타격이 큰 데이터 (공정 한계값, 검사 판정)
- 여러 팀이 **중복 참조**하는 공용 마스터 (설비 코드, 자재 코드, 거래처 코드)
- 주기적인 **감사·인증 대상** 데이터

**스튜어드(Steward) 지정:**
- CDE마다 단일 스튜어드 배정. 각 CDE에 비즈니스 책임자(Business Accountable) + 운영 책임자(Operational Responsible) 구분
- 스튜어드의 역할: 자동 캡처가 잡지 못하는 "비즈니스 의미(왜 이렇게 했는가)"와 오너십 큐레이션
- 기술(IT)과 업무(OT·현업) 사이에서 리니지 정확성 검증

> **제조 맥락 CDE 예시**: `feat_vib_rms_x`(PdM 피처), `defect_rate_daily`(품질 KPI), `AI_0207_tag_mapping`(OT→IT 매핑표), `PFMEA_2호기.pdf`(품질 RAG 원천)

### [슬라이드 17] 캡처 방식 — 기술 흐름은 자동, 비즈니스 의미는 사람이

**핵심 메시지**: 하이브리드가 정답이다. 기술 흐름(어떻게 흐르나)은 자동 캡처, 비즈니스 의미(왜 그런가)와 OT→IT 경계는 스튜어드가 보완한다.

| 캡처 방식           | 동작 원리                                                               | 강점                                     | 약점                            |
| --------------- | ------------------------------------------------------------------- | -------------------------------------- | ----------------------------- |
| **코드/SQL 파싱**   | SQL·dbt·저장 프로시저를 AST(추상 구문 트리)로 분석해 컬럼 단위 흐름 추출. DataHub 97~99% 정확도 | 가장 정밀, 변환 로직 완전 추적                     | 코드 없는 GUI ETL·수작업 Excel에는 미적용 |
| **런타임/실행 로그**   | Airflow·Spark·dbt 실행 시점에 OpenLineage 이벤트 방출. "계획"이 아닌 "실제로 돈" 흐름 기록 | 조건 분기·스킵된 작업도 반영                       | 파이프라인 에이전트 설치 필요              |
| **BI/Tool API** | Tableau·Power BI·Looker 의존성 API 수집해 보고서→데이터셋 연결                     | 보고서 계층까지 연결 완성                         | BI 벤더마다 API 커버리지 상이           |
| **수동 입력**       | 스튜어드가 카탈로그 UI에서 직접 노드·엣지 추가                                         | 자동으로 잡히지 않는 비즈니스 의미, OT→IT 경계 매핑 기록 가능 | 시간 비용, 변경 추적 누락 위험            |
| **하이브리드 (권고)**  | 기술 흐름 → 자동 캡처 + 비즈니스 의미·OT 경계 → 수동 보완                               | 지속 가능성 + 정확성 균형                        | 역할 구분과 프로세스 정립 필요             |

### [슬라이드 18] 수작업 리니지의 4대 Pain Point와 AI·자동화 해소 기술

**핵심 메시지**: 스프레드시트·위키 기반 수동 리니지는 파이프라인 변경 즉시 낡는다. "낡은 리니지는 거짓 확신을 주기 때문에 없는 것보다 위험하다."

**수작업의 4대 Pain Point:**
1. **노후화(Staleness)**: 파이프라인 변경 한 번이면 수작업 문서 즉시 낡음 — "거짓 지도"는 없느니만 못함
2. **커버리지 공백**: 가장 복잡한 변환(여러 시스템 교차)이 빠짐
3. **컬럼 단위 불가**: "어느 컬럼이 어느 컬럼에서 왔는가"를 손으로 추적하는 것은 현실적으로 불가능
4. **확장 비용**: 자산 수천 개 초과 시 수작업 유지 인력 비용이 구축 효익을 초과

**AI·자동화 해소 기술:**

| 기술 | 기능 | 대표 적용 도구 |
|---|---|---|
| **SQL/코드 AST 파싱** | 쿼리·저장 프로시저 파싱해 컬럼 단위 리니지 자동 추출. DataHub 97~99% 정확도 | DataHub, IBM MANTA, Collibra, Informatica |
| **런타임 이벤트(OpenLineage)** | 실행 시점 이벤트로 "실제로 돈" 흐름 기록, 조건 분기 반영 | Airflow, Spark, dbt + Marquez/DataHub |
| **LLM 기반 리니지 추론** | 자연어 설명·주석·코드에서 LLM이 리니지 추론. CoT 프롬프트로 오픈소스 모델이 GPT-4 수준 달성 | 연구/실험 단계, Ataccama AI 일부 반영 |
| **AI 변환 설명** | 복잡한 SQL·파이프라인 로직을 자연어로 자동 설명 → 비기술 사용자도 이해 | Ataccama ONE AI, Atlan AI |
| **ML 기반 오너 추천** | 메타데이터 패턴으로 자산 오너·스튜어드 자동 추천 | DataHub (LinkedIn ML 기반) |
| **AI 답변 Citation 자동 기록** | RAG 답변 시 retrieved_chunks·tables_used·tool_calls를 자동 로깅 → answer_id로 역추적 | LangChain + Langfuse, LlamaIndex + OpenTelemetry |

### [슬라이드 19] Tech Spec — 상용 솔루션 비교 (제조업 기준)

**핵심 메시지**: 시장에는 다양한 리니지 플랫폼이 있다. 스택 환경과 OT 적합성을 기준으로 선택해야 한다.

| 도구                          | 유형            | 리니지 방식                                                                                  | 컬럼 레벨             | AI/RAG 리니지               | Gartner 포지션(2025)                | 제조/OT 적합성               |
| --------------------------- | ------------- | --------------------------------------------------------------------------------------- | ----------------- | ------------------------ | -------------------------------- | ----------------------- |
| **Collibra**                | 상용(엔터프라이즈)    | QueryFlow 기술 — SQL 파싱 + OpenLineage(2025.06 추가). 테이블·컬럼 단위                              | ✅                 | 부분(거버넌스 연계)              | **Leader**                       | 중간(IT 스택 중심)            |
| **Alation**                 | 상용(엔터프라이즈)    | Active Metadata 카탈로그 기반 + Manta 파트너십으로 컬럼 단위 크로스-시스템 리니지                                | ✅(Manta 통합 시)     | 부분(카탈로그 연계)              | **Leader**                       | 중간                      |
| **Atlan**                   | 상용(모던 스택)     | SQL 파싱(Snowflake/BigQuery/Redshift/Databricks) + OpenLineage 런타임 이벤트 네이티브 수신. G2 9.1/10 | ✅(네이티브, BI까지)     | ✅ AI 리니지 및 MCP 연동        | **Leader**(D&A Governance, 2026) | 낮음(클라우드 특화)             |
| **Informatica IDMC**        | 상용(엔터프라이즈)    | EDC + Axon: 자동화 스캐너로 SQL·저장 프로시저·AI/ML 코드 파싱. 멀티클라우드                                    | ✅(고급 스캐너)         | 부분(CLAIRE AI)            | **Leader**(최고 비전 완성도)            | 중간(SAP·Oracle 연동 강점)    |
| **IBM watsonx.data(MANTA)** | 상용(엔터프라이즈)    | 구 MANTA 기술 통합: DB·ETL·BI 전방위 자동 리니지. 컬럼 단위 step-by-step 분석                              | ✅(가장 광범위한 소스)     | 부분(거버넌스 연계)              | **Leader**                       | **중간~높음**(레거시·메인프레임 강점) |
| **Microsoft Purview**       | 상용(Azure 생태계) | Azure Fabric·ADF·Synapse 네이티브 스캔. 비Azure는 커넥터 개발 필요                                     | △(Azure는 지원, 프리뷰) | 부분(Fabric AI Foundry 예정) | Challenger                       | 낮음(Azure 전제)            |
| **Ataccama ONE**            | 상용(DQ 중심)     | 자동 리니지 + 품질 오버레이를 리니지 다이어그램에 직접 표시(업계 유일)                                               | ✅(기술·비즈니스 뷰)      | 부분(AI 설명·agentic DQ)     | **Leader**(Augmented DQ)         | **중간**(제조 레퍼런스 있음)      |

### [슬라이드 20] Tech Spec — 오픈소스 솔루션 비교

**핵심 메시지**: 오픈소스는 데이터 엔지니어링 역량이 있는 조직에 적합하다. DataHub가 가장 성숙하고, OpenMetadata는 단순 아키텍처로 진입 장벽이 낮다.

| 도구 | 유형 | 리니지 방식 | 컬럼 레벨 | 특징 | 제조 적합성 |
|---|---|---|---|---|---|
| **DataHub** (LinkedIn) | 오픈소스/상용(DataHub Cloud) | SQL 파싱(BigQuery·Snowflake·dbt 등 20+) + OpenLineage REST 엔드포인트. ML 기반 오너 추천 | ✅(자동, 네이티브) | LinkedIn 내부 생산 검증. JanusGraph + Elasticsearch | 중간(IT 스택 전제) |
| **OpenMetadata** | 오픈소스 | API-first. dbt·BigQuery·Snowflake 자동 수집 + No-Code 수동 편집기 | ✅+수동 편집 | MySQL/PostgreSQL 기반 단순 아키텍처. 그래프 DB 없음 | 중간(단순 환경 적합) |
| **Apache Atlas** | 오픈소스(Hadoop) | Hive·HBase·Kafka·Spark 네이티브 후크. REST API | ✅(Hadoop 내) | JanusGraph + Solr. Ranger 통합 PII 분류 자동 전파 | 낮음(Hadoop 전제) |
| **Marquez** | 오픈소스(OpenLineage Ref.) | OpenLineage 이벤트 수신 전용 저장소 + 시각화 UI | 부분(이벤트 의존) | 설정·운영에 데이터 엔지니어 전문성 요구 | 낮음(최소 구성용) |

### [슬라이드 21] 스택별 솔루션 선택 가이드

**핵심 메시지**: "모든 상황에 최고인 솔루션"은 없다. 현재 데이터 스택과 OT 환경을 기준으로 선택하라.

| 현재 환경 | 권장 선택 | 이유 |
|---|---|---|
| **Azure/Fabric 중심** | Microsoft Purview + IBM MANTA(복잡 SQL 보강) | 네이티브 통합 + 레거시 커버리지 보완 |
| **SAP/Oracle ERP + 레거시 DW** | Informatica IDMC 또는 IBM watsonx.data Intelligence | 레거시 커버리지 최강 |
| **Snowflake/dbt/Tableau 모던 스택** | Atlan | G2 최고점, 컬럼 리니지 + AI 기능 |
| **오픈소스 선호 + 엔지니어링 역량 보유** | DataHub (가장 성숙) 또는 OpenMetadata (단순 아키텍처) | 커뮤니티 활성, OpenLineage 지원 |
| **Airflow/Spark 파이프라인 + 최소 도구** | OpenLineage 표준 + Marquez | 가장 경량한 구성 |
| **품질(C-1·C-2)과 통합된 리니지 필요** | Ataccama ONE | 품질 오버레이 리니지. 제조 레퍼런스 |

### [슬라이드 22] RAG/LLM 답변 근거 로그 — 스키마 설계

**핵심 메시지**: AI 답변 근거 로그는 answer_id 하나로 해당 답변의 모든 근거(청크·테이블·Tool 호출·프롬프트 버전)를 역추적할 수 있어야 한다.

**AI 답변 근거 로그 핵심 스키마:**

```json
{
  "answer_id": "ans_20260602_001",
  "timestamp": "2026-06-02T09:14:00Z",
  "question": "2호기 압축기 베어링 결함 원인은?",
  "prompt_template_id": "defect-rca-v1.4",
  "model_id": "claude-sonnet-4-6",

  "retrieved_chunks": [
    {
      "doc_id": "CS_2231", "chunk_id": "c_8842",
      "source_display": "C/S Report #2231, p.3",
      "retrieval_score": 0.91,
      "index_version": "v20260601"
    }
  ],
  "tables_used": [
    {
      "table_ref": "dw.feat_vib_rms_x",
      "lineage_node_id": "node_vib_rms_x_v3"
    }
  ],
  "tool_calls": [
    {"tool_name": "get_sensor_history", "input_params": {"tag": "AI_0207", "days": 30}}
  ],

  "citation_display": [
    "① C/S Report #2231 3p",
    "② PFMEA 2호기 12p",
    "③ 진동 이력 AI_0207 (최근 30일)"
  ]
}
```

**3가지 핵심 설계 원칙:**
- **answer_id**: retrieved_chunks, tables_used, tool_calls 역추적 가능 — 리니지의 시작점
- **index_version 기록**: "어느 버전의 RAG 인덱스로 답했는가" 추적 (인덱스 업데이트 전후 답변 차이 비교 가능)
- **lineage_node_id**: 기존 데이터 리니지 그래프의 노드와 연결 — AI 구간과 전통 데이터 리니지를 이음

### [슬라이드 23] Citation 구현 패턴과 구현 도구

**핵심 메시지**: Citation은 3가지 패턴 중 상황에 맞게 선택한다. 제조업 RAG는 "출처 카드 + 하이라이트 링크" 조합이 현업 수용성과 추적성을 모두 만족한다.

**Citation 구현 3가지 패턴:**

| 패턴 | 방식 | 적합 상황 |
|---|---|---|
| **인라인 Citation** | 답변 문장 내 `[1]`, `[2]` 번호 삽입, 참고문헌 목록 연결 | 학술·기술 문서 수준 엄밀성 필요 시 |
| **출처 카드** | 답변 하단에 참조 문서 카드(제목·페이지·신뢰도 점수) 나열 | 현업 RAG 챗봇 UI 일반적 방식 |
| **하이라이트 링크** | 답변 문장 클릭 시 원문 PDF 해당 위치로 이동 | 안전·품질 도메인 등 원문 확인 필요성 높을 때 |

**구현 도구·프레임워크:**

| 도구 | 역할 |
|---|---|
| **Langfuse** | LangChain·LlamaIndex 연동 RAG 파이프라인 전체 트레이싱. retrieved_chunks·tool_calls 자동 기록. 오픈소스 + 상용 SaaS |
| **OpenTelemetry GenAI** | LLM 호출을 span으로 감싸 model명·token수·finish reason 등 표준 속성 기록. OpenAI·Anthropic·LangChain 자동 계측 가능 |
| **OpenLLMetry (Traceloop)** | OpenTelemetry 기반 LLM 앱 트레이싱 SDK. LangChain·LlamaIndex·Haystack 자동 계측 |
| **LlamaIndex Observability** | `set_global_handler` 한 줄로 Langfuse·Arize·Weights&Biases 연동. 컨텍스트 증강·LLM 쿼리 상세 트레이스 |

- **설계 핵심**: AI 답변 근거 로그는 **처음부터 요구사항에 포함**해야 한다. LangChain/LlamaIndex 기반 시스템은 프로젝트 초기에 Langfuse 또는 OpenLLMetry를 붙이면 retrieved_chunks·tool_calls가 자동 기록됨. 나중에 추가하려면 파이프라인 재설계 필요

### [슬라이드 24] OT→IT 경계 — 제조 리니지의 최대 난관

**핵심 메시지**: 제조업의 OT→IT 경계(센서/SCADA → 웨어하우스/AI)는 시중 리니지 도구의 자동 커버 범위 밖이다. 별도 전략이 필요하다.

**왜 어려운가:**
- Historian은 태그 ID(예: `AI_0207`)를 키로 사용 — IT의 테이블명/컬럼명과 매핑 문서가 없으면 자동 추적 불가
- OPC-UA·Modbus·MQTT 레이어는 SQL 파서 미적용 — 표준 리니지 도구의 자동 캡처 대상 밖
- 단위·스케일 변환이 암묵적 — Historian 적재 시 공학 단위 변환(예: raw 카운트 → mm/s)이 별도 문서 없이 발생
- 2025년 현재 시장 도구(Collibra, DataHub 등) 모두 IT/클라우드 스택 전제 — OT Historian 직접 커넥터를 제공하는 도구는 매우 드묾

**현실적 대응 방안 4가지:**

1. **OT→IT 매핑표를 "첫 번째 수동 리니지 산출물"로 작성**
   - 태그 ID ↔ Historian 테이블 ↔ DW 컬럼 매핑, 단위·스케일 변환 규칙 명기
   - 이 매핑표 자체를 CDE로 지정하고 스튜어드 배정

2. **센서 교정·교체 이력을 메타데이터로 등록**
   - 변경 날짜·내용·영향받는 DW 컬럼 목록을 변경 통제 프로세스에 포함

3. **Unified Namespace(UNS) + MQTT/Kafka 도입 시 리니지 연동 고려**
   - MQTT·Kafka 기반 UNS 구조는 OT 데이터를 스트림 이벤트로 IT에 노출 → OpenLineage 이벤트 방출 가능성

4. **단기 현실적 목표**: OT→IT 경계의 "태그→DW 컬럼 매핑 + 단위 변환 이력"을 카탈로그에 등록하는 것만으로도, 핵심 리니지 단절 구간을 "연결 가능한 노드"로 만들 수 있음

---

## 6. KPI

### [슬라이드 25] 리니지 관리 KPI 5개 — 측정하지 않으면 개선할 수 없다

**핵심 메시지**: 리니지 체계는 5개 KPI로 측정한다. 도입 전 기준선(Baseline)을 먼저 확보하고, 개선 효과를 숫자로 경영진에게 보여라.

| KPI | 정의 | 측정 방법 | 측정 목적 | 기대효과/벤치마크 |
|---|---|---|---|---|
| **K-1. 리니지 커버리지** (Lineage Coverage) | 전체 프로덕션 데이터 자산 중 리니지가 문서화된 비율 | (리니지 등록 자산 수 ÷ 전체 자산 수) × 100 | 리니지 완성도·공백 파악 | Stage 1 <20% → Stage 4(목표) 90%+ |
| **K-2. 컬럼 단위 리니지 비율** (Column-Level Ratio) | CDE 중 컬럼 단위 리니지가 확보된 비율 | (컬럼 단위 등록 CDE 수 ÷ 전체 CDE 수) × 100 | 정밀 영향 분석·AI 학습 데이터 추적 가능 여부 | EU AI Act·GDPR·SOX 등 컴플라이언스 요건 충족에 필수. 컬럼 단위 없이 테이블 단위만으로는 감사 대응 불가 |
| **K-3. 자동 캡처 비율** (Automated Capture Rate) | 전체 리니지 이력 중 자동화 수단으로 기록된 비율 | (자동 캡처 이벤트 수 ÷ 전체 이벤트 수) × 100 | 리니지 지도의 최신성(Freshness) 보장 여부 | 프로덕션 자산 갱신 목표: 24시간 이내. 수동 방식은 수주 내 stale화 |
| **K-4. 데이터 인시던트 MTTR** (Mean Time to Resolution) | 데이터 품질 이상·파이프라인 오류 발생~근본원인 파악 및 해결까지 평균 소요 시간 | 인시던트 티켓 오픈~클로즈 시간 집계. 리니지 도입 전후 비교 | 리니지가 업무 시간을 얼마나 단축하는지 정량적 ROI 측정 | Aliaxis(제조): 95% 빠르게, 1일→1시간. Takealot: 50% 단축. Xometry(제조 플랫폼): 며칠→몇 시간 |
| **K-5. AI 답변 Citation 부착률** (AI Answer Citation Rate) | AI 시스템이 생성한 답변 중 근거 출처가 구조적으로 기록·제공된 비율 | (Citation 로그 포함 답변 수 ÷ 전체 답변 수) × 100 | AI 결과 신뢰성·감사 추적 가능성 확보. 리니지 ⑤~⑦ 구간 커버 여부 | EU AI Act Article 11 및 FINOS AI Governance Framework 출처 추적성 요건. 핵심 AI 과제 목표: **100%** |

**KPI 측정의 3가지 전제:**
1. 기준선(Baseline) 확보: 도입 전 현황 먼저 측정해야 개선 효과 증명 가능
2. 자동 수집 인프라: 카탈로그·옵저버빌리티 툴에서 자동화
3. 비즈니스 언어로 재표현: "MTTR 30% 단축 → 설비 가동률 X% 향상" 형태로 경영진에게 보고

---

## 7. Roadmap

### [슬라이드 26] 확산 로드맵 — 6단계, 12~24개월 성숙도 여정

**핵심 메시지**: 리니지는 Phase 0(준비)부터 Phase 5(전사 자동화)까지 단계적으로 확산한다. 핵심은 Phase 1 파일럿에서 즉시 MTTR 단축 수치를 보여 경영진 지지를 확보하는 것이다.

| 단계 | 명칭 | 성숙도 | 핵심 활동 | 주요 산출물 | 기간 |
|---|---|---|---|---|---|
| **Phase 0** | 준비(Foundation) | Stage 1 (Reactive) | 스튜어드 지정, CDE 선정, KPI Baseline 측정, 도구 선택 | CDE 목록, 스튜어드 배정표, KPI Baseline | 1~2개월 |
| **Phase 1** | 파일럿(Pilot) | Stage 1 | AI 과제 1개의 원천~AI 답변 ①~⑦ 전 사슬 종단 리니지 적용. OT→IT 수동 매핑 포함 | 종단 리니지 지도, 파일럿 MTTR 측정 결과, OT→IT 매핑표 v1 | 2~3개월 |
| **Phase 2** | 핵심 흐름 확장(Core Expansion) | Stage 2 (Passive) | 시스템 경계 우선 연결. 원천→전처리→분석셋→보고서 테이블 단위 확대. BI 연동. Coverage 50% 목표 | 핵심 흐름 테이블 단위 리니지, Lineage Coverage 대시보드 | 3~6개월 |
| **Phase 3** | 컬럼 단위 심화(Column-Level) | Stage 3 (Active) | CDE·컴플라이언스 대상 자산 컬럼 단위 리니지 적용. 영향 분석 자동 알림 시작. Column-Level Ratio KPI 가동 | 컬럼 단위 리니지, 영향 분석 자동 알림 | 3~6개월 |
| **Phase 4** | AI 구간 적용(AI Lineage) | Stage 3→4 | AI 답변 근거 로그 스키마 설계·배포. Citation UI 구현. AI Answer Citation Rate KPI 가동 (핵심 과제 100% 목표) | AI 답변 근거 로그, Citation 화면, Citation Rate 대시보드 | 2~4개월 |
| **Phase 5** | 전사 자동화(Enterprise) | Stage 4 (Governed) | Lineage Report 버튼화(원클릭 증빙 출력). 전사 확대. 리니지를 "제품처럼" 운영(오너·갱신·점검 주기) | 영향 분석·감사 리포트 자동화. Coverage 90%+. 자동 캡처율 80%+ | 6~12개월 |

**성숙도 모델 연결:**

```
Phase 0+1  →  Stage 1 (Reactive):   리니지 있지만 문제 날 때만 조회. 커버리지 <20%
Phase 2    →  Stage 2 (Passive):    문서화됐지만 일상 워크플로 미통합. 커버리지 20~50%
Phase 3    →  Stage 3 (Active):     일상 변경·알림에 자동 노출. 커버리지 50~85%
Phase 4+5  →  Stage 4 (Governed):  규제 대응·AI 추적·의사결정 추적 가능. 커버리지 90%+
```

### [슬라이드 27] 로드맵 리스크와 성공 원칙

**핵심 메시지**: 리니지 구축 실패의 3대 리스크는 OT→IT 공백, 수동 의존, ROI 미가시화다. 이 셋을 처음부터 대응하라.

**3대 리스크와 대응:**

| 리스크 | 증상 | 대응 |
|---|---|---|
| **OT→IT 경계 공백** | 산업 프로토콜(OPC-UA, historian) 구간 자동 캡처 불가 | Phase 1에서 수동 매핑으로 시작 → Phase 3~5에서 전용 OT 커넥터 또는 historian API 연동으로 자동화 |
| **낡은 리니지** (Stale Lineage) | 수동 관리 의존도 높아 파이프라인 변경 직후 지도가 거짓말 | K-3 자동 캡처율 KPI 주기적 점검. 수동 입력 비율이 증가하면 즉시 자동화 계획 재검토 |
| **ROI 미가시화** | 69%의 데이터·AI 리더가 ROI 증명에 어려움 | Phase 1 파일럿에서 MTTR 단축 수치를 즉시 측정·공유해 경영진 지지 확보 |

**성공을 위한 4가지 원칙:**
1. **시스템 경계 먼저**: 한 시스템 내부만 깊게 파지 말고, 시스템 간 연결을 먼저 확보. 최악의 사고는 경계에서 발생
2. **AI 과제 설계 시 처음부터 포함**: 답변 근거 로그·Citation을 초기 요구사항에 넣어야 함. 나중에 추가하면 파이프라인 재설계 필요
3. **리니지를 제품처럼 운영**: 오너·갱신 주기·점검 주기를 정기 거버넌스 회의에 포함
4. **카탈로그에 평소에도 노출**: 리니지 지도를 자산 화면에 상시 노출. 고장났을 때만 보는 도구가 되면 안 됨

---

## 참고 출처

1. https://openlineage.io/docs/ — OpenLineage 공식 문서
2. https://github.com/OpenLineage/OpenLineage/blob/main/spec/OpenLineage.md — OpenLineage 스펙
3. https://openlineage.io/docs/spec/facets/dataset-facets/column_lineage_facet/ — ColumnLineageDatasetFacet
4. https://www.kai-waehner.de/blog/2024/05/13/open-standards-for-data-lineage-openlineage-for-batch-and-streaming/ — OpenLineage 배치·스트리밍 표준
5. https://marquezproject.ai/ — Marquez Project
6. https://datahub.com/blog/data-lineage-what-it-is-and-why-it-matters/ — DataHub 데이터 리니지 정의
7. https://docs.datahub.com/docs/act-on-metadata/impact-analysis — DataHub Lineage Impact Analysis
8. https://datahub.com/blog/column-level-lineage-comes-to-datahub/ — DataHub 컬럼 단위 리니지
9. https://docs.datahub.com/docs/generated/lineage/automatic-lineage-extraction — DataHub 자동 리니지 추출
10. https://www.montecarlodata.com/blog-table-level-vs-field-level-data-lineage-whats-the-difference/ — 테이블 vs. 컬럼 단위 비교
11. https://montecarlo.ai/blog-data-lineage/ — Monte Carlo 데이터 리니지 가이드
12. https://atlan.com/gartner-data-lineage/ — Atlan Gartner 데이터 리니지 연구·트렌드·도구 선택 가이드 2026 (Atlan: Gartner D&A Governance MQ 2026 Leader)
13. https://atlan.com/data-lineage/ — Atlan 데이터 리니지 제품
14. https://atlan.com/know/data-lineage-best-practices/ — Atlan 데이터 리니지 성숙도 모델
15. https://www.ibm.com/new/announcements/openlineage-for-a-unified-lineage-view-across-structured-and-unstructured-data-to-enable-explainable-ai — IBM OpenLineage 확장 (구조화·비구조화 통합)
16. https://www.ibm.com/products/watsonx-data-intelligence/data-lineage — IBM watsonx.data Intelligence Lineage
17. https://www.ataccama.com/blog/top-data-lineage-tools-in-2025 — Ataccama 2025 상위 리니지 도구
18. https://www.alation.com/blog/alation-manta-automate-advanced-data-lineage/ — Alation + Manta 파트너십
19. https://www.ataccama.com/blog/real-time-data-observability-is-the-missing-layer-in-eu-ai-act-compliance — EU AI Act Article 10 데이터 품질·리니지 요건
20. https://medium.com/@pulsr-io-enrico/gdpr-taught-us-data-governance-the-ai-act-demands-data-lineage-heres-the-difference-eb3c3466f324 — GDPR vs. AI Act와 데이터 리니지
21. https://www.secoda.co/blog/risks-of-neglecting-data-lineage — 리니지 부재의 리스크
22. https://www.decube.io/post/data-lineage-concepts — 데이터 리니지 개념과 비용
23. https://www.tensorlake.ai/blog/rag-citations — RAG Citation 구현 패턴
24. https://langfuse.com/docs — Langfuse 공식 문서 (LLM 트레이싱·관찰성)
25. https://uptrace.dev/blog/opentelemetry-ai-systems — OpenTelemetry AI/LLM 관찰성
26. https://github.com/traceloop/openllmetry — OpenLLMetry (Traceloop)
27. https://humansofdata.atlan.com/2023/12/aliaxis-worldwide-glossary/ — Aliaxis 케이스 스터디 (MTTR 95% 단축)
28. https://atlan.com/data-lineage-explained/ — Takealot MTTR 50% 단축 사례
29. https://air-governance-framework.finos.org/mitigations/mi-13_providing-citations-and-source-traceability-for-ai-generated-information.html — FINOS AI Governance Framework Citation 요건
30. https://arxiv.org/html/2509.13978v2 — LLM 에이전트 워크플로우 프로베넌스 참조 아키텍처
31. https://www.iot83.com/blog-posts/what-is-data-lineage-a-practical-implementation-guide — 제조업 OT→IT 리니지 도전
32. https://www.kai-waehner.de/blog/2025/07/01/unified-namespace-vs-data-product-in-it-ot-for-industrial-iot/ — Unified Namespace와 OT/IT 통합
33. https://datacrossroads.nl/2019/03/13/data-lineage-102/ — DAMA-DMBOK2 리니지 정의
34. https://euno.ai/glossary/column-level-lineage — 컬럼 단위 리니지 개요
35. https://www.alation.com/blog/critical-data-elements-definition-governance-automation/ — CDE 정의·거버넌스·자동화
