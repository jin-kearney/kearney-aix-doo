# C-3 데이터 계통(Data Lineage) — What 섹션 리서치 원자료

> 작성일: 2026-06-02
> 담당: What 클러스터 리서치 에이전트
> 선행 읽기: C-3_데이터_리니지_가이드.md (핵심 소스로 참조, 용어·예시 일관성 유지)

---

## 1. 정확한 정의 — 권위 출처별 비교

### 1.1 DAMA-DMBOK2 정의

DAMA-DMBOK2는 데이터 리니지를 **"데이터 소스에서 현재 위치까지의 경로, 그리고 그 경로를 따라 데이터에 가해진 변경의 설명(a description of the pathway from the data source to their current location and the alterations made to the data along the pathway)"** 으로 정의한다. 이를 *데이터 체인(data chain)* 이라고도 부른다. [source: https://datacrossroads.nl/2019/03/13/data-lineage-102/]

DMBOK2에서 리니지는 주로 **8장 데이터 통합 및 상호운용성(Data Integration and Interoperability)** 의 Plan & Analyze 단계 활동으로 위치한다. "데이터 흐름(data flow)을 문서화하는 것"이 이 단계의 핵심 활동 중 하나이며, 엔드-투-엔드 데이터 흐름은 데이터가 어디서 시작해 어디에 저장·사용되며 다양한 프로세스와 시스템 내부·사이에서 어떻게 변환되는지를 보여준다. [source: https://datacrossroads.nl/2017/09/10/new-vision-on-data-lineage-flow-in-dama-dm-bok-2/]

> 참고: DAMA-DMBOK2는 리니지(lineage)·데이터 흐름(data flow)·통합 아키텍처(integration architecture)·데이터 가치 체인(data & information value chain)이 혼용되는 경우가 많으며, 명시적 경계 정의가 필요하다고 스스로 밝힌다.

### 1.2 DataHub(LinkedIn/Acryl) 정의

DataHub는 리니지를 **"데이터의 전체 흐름을 나타내는 그래프 — 어디서 왔고, 어디로 갔으며, 어떤 변환을 거쳤는가"** 로 설명한다. 핵심 가치를 두 가지로 요약한다.

- **근본원인 분석(Root Cause Analysis)**: 상류(upstream)를 역추적해 문제 발원지 식별
- **영향 분석(Impact Analysis)**: 하류(downstream)를 추적해 변경의 파급 범위 식별

[source: https://datahub.com/blog/data-lineage-what-it-is-and-why-it-matters/]

### 1.3 Monte Carlo 정의

Monte Carlo(데이터 옵저버빌리티 플랫폼)는 데이터 리니지를 **"데이터가 생성되는 곳부터 소비되는 곳까지 데이터가 이동하는 방식을 추적하는 것"** 으로 정의하며, 자동화된 리니지 캡처를 현대 데이터 스택의 필수 요소로 강조한다. SQL 쿼리 로그와 오픈소스 ANTLR 파서를 활용해 리니지 데이터를 자동 생성하는 방식을 사용한다. [source: https://montecarlo.ai/blog-data-lineage/]

---

## 2. 구성요소 — 노드(Node)와 엣지(Edge)

### 2.1 그래프 모델

데이터 리니지는 본질적으로 **방향성 비순환 그래프(DAG, Directed Acyclic Graph)** 로 표현된다. [source: https://datacrossroads.nl/2019/03/13/data-lineage-102/]

| 요소 | 정의 | 예시 |
|---|---|---|
| **데이터 노드(Data Node)** | 하나의 데이터 자산(테이블, 파일, 컬럼, 청크, AI 답변 등) | `raw_vibration` 테이블, `avg_temp` 컬럼, chunk_8842 |
| **변환 노드(Transformation Node)** | 데이터를 변환하는 처리 단계(ETL/ELT 작업, SQL, Python 스크립트, AI 추론 등) | 1분 RMS 집계 SQL, PdM 모델 추론 |
| **엣지(Edge)** | 노드 간 데이터 흐름의 방향과 관계 | `raw_vibration` → (RMS 집계) → `feat_vib_rms_x` |

특허 문헌(USPTO)에서도 동일한 구조를 채택한다: "하나 이상의 데이터 노드와 그 사이의 방향성 링크(directed links)로 구성된 방향성 그래프" [source: https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/10917283]

### 2.2 테이블 단위(Table-Level) vs. 컬럼 단위(Column-Level) 리니지

| 구분 | 테이블/데이터셋 단위 | 컬럼/필드 단위 |
|---|---|---|
| **개념** | 어떤 테이블이 어떤 테이블로 흘러가는가 | 어떤 컬럼이 어떤 컬럼에서 어떤 수식으로 파생됐는가 |
| **세분성** | 낮음 (Big Picture) | 높음 (Granular) |
| **주요 용도** | 전체 흐름 파악, 빠른 의존성 확인 | 정밀 디버깅, 영향 분석, 규제 감사, AI 근거 추적 |
| **영향 분석 정밀도** | "5개 테이블이 연결됨" | "이 특정 컬럼을 사용하는 2개 리포트만 영향받음" |
| **도구 지원** | 대부분 도구가 지원 | 지원 도구가 한정적, 구현 복잡도 높음 |

[source: https://www.montecarlodata.com/blog-table-level-vs-field-level-data-lineage-whats-the-difference/]
[source: https://datahub.com/blog/column-level-lineage-comes-to-datahub/]

**핵심 격언**: "테이블 단위 리니지는 5개 테이블이 연결됨을 말해준다. 컬럼 단위 리니지는 그 중 특정 컬럼에 실제로 의존하는 2개 테이블만을 알려준다." [source: https://www.montecarlodata.com/blog-table-level-vs-field-level-data-lineage-whats-the-difference/]

---

## 3. 동작 원리 — Upstream/Downstream 추적

### 3.1 상류 추적(Upstream Lineage) — 근본원인 분석(Root Cause Analysis)

상류 추적은 "이 데이터가 어디서 왔는가?"를 거슬러 올라가는 방향이다. 데이터 품질 문제나 잘못된 AI 판정이 발생했을 때, **발원지(root cause)** 를 찾는 데 사용한다.

- dbt 모델은 소스 테이블의 하류이지만, Looker 대시보드에서 보면 상류다 — 문맥에 따라 상류/하류가 결정된다. [source: https://docs.datahub.com/docs/features/feature-guides/lineage]
- 컬럼 단위 리니지는 ETL 스크립트의 날짜 형식 오처리처럼 파이프라인의 어느 단계가 문제인지 정확히 특정할 수 있게 한다. [source: https://euno.ai/glossary/column-level-lineage]

### 3.2 하류 추적(Downstream Lineage) — 영향 분석(Impact Analysis)

하류 추적은 "이것을 바꾸면 무엇이 영향받는가?"를 아래로 따라가는 방향이다. **변경 전** 영향 범위를 파악하는 데 사용한다.

DataHub의 Lineage Impact Analysis 기능은 다음을 지원한다:
- 스키마 변경 파급 범위 사전 파악
- 실패한 파이프라인의 하류 의존성 파악
- 예상치 못한 데이터 품질 문제의 상류 원인 추적

[source: https://docs.datahub.com/docs/act-on-metadata/impact-analysis]

### 3.3 근본원인 분석 vs. 영향 분석 — 같은 지도, 두 방향

| 방향 | 질문 | 활용 시점 |
|---|---|---|
| 상류(Upstream) | 이 숫자/판정이 어디서 왔는가? | 문제 발생 후 원인 분석 |
| 하류(Downstream) | 이것을 바꾸면 뭐가 깨지는가? | 변경 전 영향도 확인 |

---

## 4. 표준 및 프레임워크

### 4.1 OpenLineage — 업계 사실상 표준(De Facto Standard)

**OpenLineage**는 2020년 시작된 오픈소스 리니지 메타데이터 수집 표준으로, 현재 업계 사실상 표준으로 자리잡았다. 도구가 아니라 **"리니지를 주고받는 공통 언어(JSON Schema 기반 스펙)"** 다. [source: https://openlineage.io/docs/]

**핵심 데이터 모델 — 3개 기본 엔티티:**

| 엔티티 | 정의 | 예시 |
|---|---|---|
| **Job** | 데이터를 소비하고 생성하는 프로세스 정의. 네임스페이스 내 고유 이름으로 식별 | Airflow DAG 태스크, Spark Job, dbt 모델 |
| **Run** | Job의 특정 시점 실행 인스턴스 ("지금 무엇이 실행되고 있나") | job_run_id로 추적되는 개별 실행 |
| **Dataset** | Job의 입력/출력. 물리적 위치(db.schema.table 등)에서 파생된 고유 이름 | PostgreSQL 테이블, S3 버킷, Kafka 토픽 |

[source: https://github.com/OpenLineage/OpenLineage/blob/main/spec/OpenLineage.md]

**Facet(패싯) — 확장성의 핵심:**

Facet은 Job·Run·Dataset에 붙이는 원자 단위 메타데이터다. 모듈식으로 확장 가능하다.

- **Job Facet**: 소스코드 위치, 오너십 등 정적 정보
- **Run Facet**: 스케줄 시간, 배치 ID 등 런타임 정보
- **Dataset Facet**: 스키마 필드, 컬럼 통계 등 데이터 정보

[source: https://apxml.com/courses/data-governance-quality-observability-production/chapter-4-data-lineage-metadata-management/openlineage-standard]

**컬럼 단위 리니지 Facet (ColumnLineageDatasetFacet):**

각 출력 컬럼(outputField)에 대해 어느 입력 컬럼(inputField)에서 어떤 변환을 거쳐 만들어졌는지를 기록한다. 변환 타입은 두 가지다:
- **DIRECT**: 출력 컬럼 값이 입력 컬럼 값에서 직접 파생됨
- **INDIRECT**: 출력 컬럼 값이 입력 컬럼의 영향을 받지만 직접 파생되지는 않음

[source: https://openlineage.io/docs/spec/facets/dataset-facets/column_lineage_facet/]

**지원 플랫폼 (Producer/Consumer):**
- Producer(생성): Apache Airflow, Apache Spark, Apache Flink, dbt, Dagster, Snowflake, BigQuery
- Consumer(소비): Marquez, Datadog, Atlan, OpenMetadata, DataHub

[source: https://www.kai-waehner.de/blog/2024/05/13/open-standards-for-data-lineage-openlineage-for-batch-and-streaming/]

**2024~2025 주요 동향:**
- Kafka Summit 2024에서 스트림 처리 리니지가 핵심 주제로 부상
- IBM이 구조화·비구조화 데이터 통합 리니지 지원을 위해 OpenLineage 스펙 확장 기여 [source: https://www.ibm.com/new/announcements/openlineage-for-a-unified-lineage-view-across-structured-and-unstructured-data-to-enable-explainable-ai]

### 4.2 Marquez — OpenLineage 참조 구현체(Reference Implementation)

**Marquez**는 OpenLineage 표준의 공식 참조 구현체(Reference Implementation)이자 LF AI & DATA 인큐베이션 프로젝트다. PostgreSQL을 기본 저장소로 사용하며, 리니지 이벤트를 수신·저장·조회하는 API와 기본 UI를 제공한다. [source: https://marquezproject.ai/]

- OpenLineage 이벤트를 받아 중앙 메타데이터 서비스로 저장
- 데이터 에코시스템의 복잡한 상호의존성을 시각적 맵으로 제공
- Airflow, Spark, Flink, dbt, Dagster 통합 지원

[source: https://github.com/MarquezProject/marquez]
[source: https://dataengineeracademy.com/blog/openlineage-and-marquez-data-lineage-for-modern-pipelines/]

### 4.3 OpenMetadata — 엔티티 중심 통합 리니지

OpenMetadata는 **엔티티 우선(entity-first) 관계형 모델**을 채택해 데이터셋·대시보드·파이프라인·용어집을 구조화된 방식으로 연결한다. OpenLineage 이벤트를 소비해 탐색 가능한 리니지 그래프를 카탈로그 안에 구축한다.

[source: https://atlan.com/openmetadata-vs-datahub/]

### 4.4 DataHub — 그래프 기반 메타데이터 플랫폼

LinkedIn에서 개발해 오픈소스화된 DataHub는 데이터셋, 대시보드, 차트, 기타 엔티티의 완전한 상류/하류 의존성을 이해하기 위한 강력한 **Lineage Impact Analysis** 워크플로우를 제공한다. [source: https://docs.datahub.com/docs/act-on-metadata/impact-analysis]

컬럼 단위 리니지 지원: 원본 테이블의 개별 필드에서 최종 보고서/모델의 목적지까지 모든 변환을 추적한다. [source: https://datahub.com/blog/column-level-lineage-comes-to-datahub/]

### 4.5 Egeria — 개방형 메타데이터 거버넌스 표준 (Linux Foundation)

**Egeria**는 ODPi(Open Data Platform Initiative, Linux Foundation) 프로젝트로, 기업 전반의 메타데이터 저장소 간 **공개 API·타입·교환 프로토콜**을 제공하는 벤더 중립 프레임워크다. IBM과 ING가 초기 공동 개발했으며, Apache Atlas 프로젝트에서 인큐베이션됐다. [source: https://www.linuxfoundation.org/press/press-release/odpi-announces-egeria-for-open-sharing-exchange-and-governance-of-metadata]

Egeria의 리니지 관련 핵심 기능:
- 데이터가 시스템을 통해 이동하는 방식을 매핑해 추적성 및 영향 분석 지원
- 거버넌스 엔진이 분류·검증·리니지 추적 거버넌스 액션을 자동 실행
- Apache 2.0 라이선스, 개방 표준 기반 상호운용성 보장

[source: https://opendatascience.com/introducing-odpi-egeria-the-industrys-first-open-metadata-standard/]

---

## 5. AI 시대의 새로운 개념 — RAG/LLM 리니지

### 5.1 왜 AI가 리니지를 더 복잡하게 만드는가

전통적 리니지는 "원천 → ETL → 보고서" 구조로 비교적 결정론적(deterministic)이었다. 그러나 LLM/RAG 시스템은 **확률적(probabilistic) 생성** 과정을 거치므로, 동일 입력도 다른 출력을 낼 수 있다. 이로 인해 "이 AI 답변은 어디서 왔는가"를 추적하는 **AI 리니지(AI Lineage)** 가 새로운 요구사항이 됐다.

현대 LLM은 출력의 출처를 기본적으로 공개하지 않아 **환각(hallucination)** 문제가 발생한다. 이를 해결하기 위해 AI 인용 프레임워크(AI citation frameworks)가 개발됐으며, 크게 두 범주로 나뉜다: (1) RAG 기법 통합, (2) 모델 학습/출력에 소스 귀속 메커니즘 내장 [source: https://rankstudio.net/articles/en/ai-citation-frameworks]

### 5.2 RAG 파이프라인 리니지 체인

RAG 기반 AI의 리니지 체인은 다음 단계로 구성된다:

```
원천 문서/데이터
  → (전처리) 청크(Chunk) 생성 + 메타데이터 태깅
  → (인덱싱) 벡터 임베딩 → 벡터 스토어
  → (검색) 사용자 질문 → 유사도 검색 → 관련 청크 N개 선택
  → (생성) [청크 + 프롬프트 + LLM] → AI 답변
  → (기록) 답변 근거 로그(answer provenance log)
```

리니지가 있으면 이 체인 전체를 역추적할 수 있다. [source: https://rankstudio.net/articles/en/ai-citation-frameworks]

### 5.3 프로베넌스(Provenance) — 데이터 리니지와의 관계

**데이터 프로베넌스(Data Provenance)** 는 데이터가 어떻게 생성됐는지를 기록하는 메타데이터이며, 데이터 리니지와 동의어로 쓰이기도 한다. 워크플로우 프로베넌스는 계산 워크플로우의 구조·제어 로직·실행 세부사항을 설명한다.

**차이점:**
- Data Provenance: "이 데이터가 어떻게 만들어졌는가" (생성 관점)
- Data Lineage: "이 데이터가 어디서 와서 어디로 갔는가" (흐름 관점)
- 실무에서는 혼용됨

[source: https://arxiv.org/html/2509.13978v2]

### 5.4 LLM 에이전트와 워크플로우 프로베넌스 (2025 최신 연구)

SC'25(Supercomputing 2025) 워크숍 논문에서는 LLM 에이전트를 활용한 인터랙티브 워크플로우 프로베넌스 참조 아키텍처를 제안했다. 핵심은:
- **Model Context Protocol(MCP)** 기반 느슨한 결합 아키텍처
- 워크플로우 실행 중 사용자와 데이터 간 실시간 상호작용 지원
- 자연어로 "이 결과가 어디서 왔는가"를 대화형으로 탐색

[source: https://arxiv.org/html/2509.13978v2]
[source: https://dl.acm.org/doi/10.1145/3731599.3767582]

### 5.5 엔터프라이즈 RAG 리니지 패턴 (2025)

2025년 실무에서 검증된 RAG 프로베넌스 패턴:
- **저장 단계**: doc_id, 섹션 경로, 라인 범위, 버전을 함께 저장
- **생성 단계**: 답변 본문에 섹션 이름과 버전을 인용해 모든 주장을 역추적 가능하게 함
- **컬럼 단위 리니지 연계**: LLM이 검색하는 모든 정보에 대해 추적 가능한 프로베넌스를 컬럼 단위로 제공

[source: https://medium.com/@bhagyarana80/top-10-rag-patterns-that-actually-fix-llm-answers-1141ed29d6cc]

### 5.6 IBM의 OpenLineage 확장 — 비구조화 데이터 + AI 설명가능성

IBM은 watsonx.data 플랫폼을 통해 OpenLineage를 **구조화 + 비구조화 데이터 통합 리니지**로 확장했다. 핵심 주장: "신뢰할 수 있는 메타데이터가 AI를 설명 가능(explainable)하고 책임 가능(accountable)하고 확장 가능(scalable)하게 만든다. 데이터가 어디서 왔고, 어떻게 변환됐으며, 어떻게 사용됐는지를 투명하게 기록함으로써 책임 있는 AI 원칙을 운용 가능하게 한다." [source: https://www.ibm.com/new/announcements/openlineage-for-a-unified-lineage-view-across-structured-and-unstructured-data-to-enable-explainable-ai]

---

## 6. 핵심 개념 정의 (용어집)

### 6.1 상류(Upstream) / 하류(Downstream)

- **상류(Upstream)**: 현재 데이터의 입력·원천 방향. "어디서 왔는가"를 역추적할 때의 방향. 근본원인 분석에 사용.
- **하류(Downstream)**: 현재 데이터가 흘러가는 목적지 방향. "이것을 바꾸면 무엇이 영향받는가"를 파악할 때의 방향. 영향 분석에 사용.

동일한 노드도 문맥에 따라 상류 또는 하류가 된다: dbt 모델은 소스 테이블의 하류지만 Looker 대시보드의 상류다. [source: https://docs.datahub.com/docs/features/feature-guides/lineage]

### 6.2 CDE (Critical Data Element, 핵심 데이터 요소)

CDE는 **품질·정의·보호·리니지가 비즈니스 성과나 위험에 불균형적으로 큰 영향을 미치는 데이터 필드·속성·코드·날짜·측정값·식별자**다.

"핵심 데이터"의 개념은 2010년 DAMA-DMBOK 2판에서 처음 등장했다.

CDE 거버넌스의 필수 요소:
- 각 CDE에 대한 비즈니스 및 기술 리니지 포착
- 데이터가 조직에 유입되는 방식, 기록 시스템(System of Record), 생애주기 변환 이력 이해
- 출처 추적(provenance tracing), 품질 입증, 통제 감사 증빙

[source: https://www.alation.com/blog/critical-data-elements-definition-governance-automation/]
[source: https://dgx.group/2024/05/14/mastering-critical-data-elements-part-1-foundations-of-modern-data-governance/]

### 6.3 스튜어드(Data Steward)

데이터 스튜어드는 **기술과 비즈니스 측면을 연결하고 데이터 거버넌스 활동의 운영을 담당하는 책임자**다. CDE 식별은 데이터 거버넌스 스튜어드에게 할당돼야 하며, 각 CDE마다 비즈니스 책임자(Business Accountable)와 운영 책임자(Operational Responsible) 역할이 배정돼야 한다. [source: https://dgx.group/2024/05/14/mastering-critical-data-elements-part-1-foundations-of-modern-data-governance/]

리니지 맥락에서 스튜어드의 역할: 자동 캡처가 잡지 못하는 **비즈니스 의미("왜 이렇게 했는가")와 오너십**을 큐레이션하는 사람.

### 6.4 프로베넌스(Data Provenance)

데이터 프로베넌스는 데이터가 어떻게 생성됐는지를 기록하는 메타데이터다. 리니지(흐름 관점)와 상호 보완적인 개념이며, 특히 과학 데이터·AI 결과의 재현성·신뢰성 검증에 중요하다. [source: https://arxiv.org/html/2509.13978v2]

### 6.5 OpenLineage 핵심 엔티티 재정리

| 엔티티 | 역할 | 식별 방식 |
|---|---|---|
| **Job** | "무엇을 해야 하는가" — 프로세스 정의 | 네임스페이스 + 고유 이름 |
| **Run** | "지금 무엇이 실행되고 있는가" — Job의 실행 인스턴스 | run_id |
| **Dataset** | Job의 입력/출력 데이터 자산 | 데이터소스 네임스페이스 + 물리적 위치 |
| **Facet** | 엔티티에 붙는 원자 단위 확장 메타데이터 | 자유 확장 |

---

## 7. DAMA-DMBOK 체계에서 리니지의 위치

DAMA-DMBOK2의 **데이터 관리 지식 영역(DMBOK Knowledge Areas)** 에서 데이터 리니지는 주로 다음 영역들과 교차한다:

| DMBOK 영역 | 리니지와의 관계 |
|---|---|
| **8장. 데이터 통합 및 상호운용성** | 리니지 문서화가 Plan & Analyze 핵심 활동 |
| **11장. 데이터 보안** | 접근 이력과 연계 |
| **13장. 메타데이터 관리** | 카탈로그에 리니지 정보 저장 |
| **14장. 데이터 거버넌스** | CDE 관리, 스튜어드십, 리니지 정책 |

DAMA-DMBOK Framework는 DAMA International(비영리 전문가 협회)이 발행하며 데이터를 전략적 엔터프라이즈 자산으로 관리하는 모범 사례를 정의하는 글로벌 인정 프레임워크다. [source: https://www.ovaledge.com/blog/dama-dmbok-data-governance-framework]

---

## 8. 자동 캡처 방법론

### 8.1 세 가지 자동 캡처 경로

| 방법 | 메커니즘 | 강점 | 한계 |
|---|---|---|---|
| **코드/SQL 파싱** | 쿼리·dbt 모델·스크립트를 정적 분석해 컬럼 흐름 추출 | 정밀, 변환 로직 추적 | 동적 SQL 처리 어려움 |
| **런타임 이벤트** | Airflow·Spark 등 파이프라인이 실행 시 OpenLineage 이벤트 방출 | 실제 실행 반영, 실시간 | 도구가 OpenLineage 지원해야 |
| **BI/Tool API** | 대시보드·도구 제공 의존성 정보 수집 | 보고서 계층 연결 | 도구별 API 상이 |

[source: https://montecarlo.ai/blog-data-lineage/]

### 8.2 수동 관리의 한계

스프레드시트·위키 기반 수동 리니지는 파이프라인 변경 시 즉시 낡는다. "낡은 리니지는 거짓 확신을 주기 때문에 없는 것보다 위험하다." 자동 캡처가 기본 원칙이며, 사람(스튜어드)이 추가해야 할 부분은 자동으로 잡히지 않는 **비즈니스 의미와 오너십** 뿐이다.

---

## 9. 제조업 맥락 특이사항 — OT/IT 경계

### 9.1 OT→IT 경계의 리니지 공백

제조업(센서·PLC·SCADA·Historian → 웨어하우스·BI·AI) 구간은 일반 SQL 파서나 표준 OpenLineage 통합으로는 자동 추적이 어렵다. 주요 이유:
- 산업 프로토콜(OPC-UA, MQTT, Modbus)이 표준 데이터 파이프라인 도구와 다른 레이어
- Historian 시스템의 폐쇄적 API
- PLC 태그 → 웨어하우스 컬럼 매핑이 암묵적 지식으로만 존재하는 경우 다수

### 9.2 현실적 대응 방안

1. **Historian 태그 ID ↔ 웨어하우스 컬럼 매핑표** 수동 관리 (오너·갱신 주기 지정)
2. **센서 교정·단위·스케일 변경 이력**을 메타데이터로 명시적 기록
3. OT→IT 핸드오프 지점을 처음부터 명시적 리니지 대상으로 설계

---

## 참고 출처

- [About OpenLineage | OpenLineage](https://openlineage.io/docs/)
- [OpenLineage GitHub Repository](https://github.com/OpenLineage/OpenLineage)
- [OpenLineage Spec — main](https://github.com/OpenLineage/OpenLineage/blob/main/spec/OpenLineage.md)
- [Column Level Lineage Dataset Facet | OpenLineage](https://openlineage.io/docs/spec/facets/dataset-facets/column_lineage_facet/)
- [IBM: OpenLineage for unified lineage view across structured and unstructured data to enable explainable AI](https://www.ibm.com/new/announcements/openlineage-for-a-unified-lineage-view-across-structured-and-unstructured-data-to-enable-explainable-ai)
- [Open Standards for Data Lineage: OpenLineage for Batch AND Streaming — Kai Waehner](https://www.kai-waehner.de/blog/2024/05/13/open-standards-for-data-lineage-openlineage-for-batch-and-streaming/)
- [Marquez Project](https://marquezproject.ai/)
- [Marquez GitHub Repository](https://github.com/MarquezProject/marquez)
- [OpenLineage and Marquez: Data Lineage for Modern Pipelines — Data Engineer Academy](https://dataengineeracademy.com/blog/openlineage-and-marquez-data-lineage-for-modern-pipelines/)
- [Data Lineage: What It Is and Why It Matters | DataHub](https://datahub.com/blog/data-lineage-what-it-is-and-why-it-matters/)
- [Lineage Impact Analysis | DataHub](https://docs.datahub.com/docs/act-on-metadata/impact-analysis)
- [About DataHub Lineage | DataHub](https://docs.datahub.com/docs/features/feature-guides/lineage)
- [Column-Level Lineage Comes to DataHub](https://datahub.com/blog/column-level-lineage-comes-to-datahub/)
- [OpenMetadata vs. DataHub — Atlan](https://atlan.com/openmetadata-vs-datahub/)
- [New Vision on Data Lineage/Flow in DAMA-DMBOK2 — Data Crossroads](https://datacrossroads.nl/2017/09/10/new-vision-on-data-lineage-flow-in-dama-dm-bok-2/)
- [Data Lineage 102: Definition and Key Components — Data Crossroads](https://datacrossroads.nl/2019/03/13/data-lineage-102/)
- [DAMA-DMBOK Framework | OvalEdge](https://www.ovaledge.com/blog/dama-dmbok-data-governance-framework)
- [The Ultimate Guide To Data Lineage — Monte Carlo](https://montecarlo.ai/blog-data-lineage/)
- [Table-Level vs. Field-Level Data Lineage: What's the Difference? — Monte Carlo](https://www.montecarlodata.com/blog-table-level-vs-field-level-data-lineage-whats-the-difference/)
- [Understanding Column-Level Lineage | Solix Technologies](https://www.solix.com/kb/understanding-column-level-lineage/)
- [Column-Level Lineage: What It Is and How It Works | Euno](https://euno.ai/glossary/column-level-lineage)
- [Critical Data Elements (CDE) Best Practices | Alation](https://www.alation.com/blog/critical-data-elements-best-practices-data-governance/)
- [Critical Data Elements Explained: Defining, Governing, and Automating CDEs | Alation](https://www.alation.com/blog/critical-data-elements-definition-governance-automation/)
- [Mastering Critical Data Elements: Part 1 — DGX](https://dgx.group/2024/05/14/mastering-critical-data-elements-part-1-foundations-of-modern-data-governance/)
- [ODPi Announces Egeria — Linux Foundation](https://www.linuxfoundation.org/press/press-release/odpi-announces-egeria-for-open-sharing-exchange-and-governance-of-metadata)
- [Introducing ODPi Egeria — Open Data Science](https://opendatascience.com/introducing-odpi-egeria-the-industrys-first-open-metadata-standard/)
- [LLM Citations Explained: RAG & Source Attribution Methods | RankStudio](https://rankstudio.net/articles/en/ai-citation-frameworks)
- [LLM Agents for Interactive Workflow Provenance: Reference Architecture — arXiv](https://arxiv.org/html/2509.13978v2)
- [LLM Agents for Interactive Workflow Provenance — ACM DL](https://dl.acm.org/doi/10.1145/3731599.3767582)
- [Top 10 RAG Patterns That Actually Fix LLM Answers — Medium](https://medium.com/@bhagyarana80/top-10-rag-patterns-that-actually-fix-llm-answers-1141ed29d6cc)
- [OpenLineage as the Spine of Data Observability — Data Lakehouse Hub](https://datalakehousehub.com/blog/2026-05-openlineage-observability/)
- [The OpenLineage Standard — apxml.com](https://apxml.com/courses/data-governance-quality-observability-production/chapter-4-data-lineage-metadata-management/openlineage-standard)
