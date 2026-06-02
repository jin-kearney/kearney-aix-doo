# C-3 데이터 계통(Data Lineage) — How 섹션 리서치 원자료

> 작성일: 2026-06-02 | 담당 클러스터: How(핵심) | 청중: 제조업 현업 실무자
> 핸드북 원문 참조: C-3_데이터_리니지_가이드.md (v2.0)

---

## 1. 구축 방법론: 미준비 상태에서 Lineage 세우는 단계

### 1.1 단계별 로드맵 (핸드북 7.2 확장)

핸드북 7.2에서 정의한 5단계를 각 단계별 구체적 작업 수준으로 확장한다.

| 단계 | 이름 | 핵심 활동 | 주요 판단기준 / 완료 조건 |
|---|---|---|---|
| **0. 준비** | Scoping & Ownership | CDE(핵심 데이터 요소) 목록 확정, 스튜어드(Steward) 지정, 도구 선택 | CDE 목록 문서화, 스튜어드 책임 배정 완료 |
| **1. 파일럿** | 단일 AI 과제 End-to-End | AI 과제 1개 선정 → 원천~AI 답변까지 ①~⑦ 전 구간 리니지 수동+자동 혼합 적용 | 파이프라인 변경 1회 후에도 리니지가 자동 갱신되는지 검증 |
| **2. 교차 연결** | 시스템 경계 연결 우선 | 원천→전처리→분석셋→보고서 구간을 *시스템 경계 횡단* 지점 중심으로 테이블 단위 연결. OT→IT 경계 수동 매핑표 작성 포함 | 경계 횡단 흐름 90% 이상 노드 연결 완료 |
| **3. 컬럼 심화** | 컬럼 단위 리니지 | CDE에 해당하는 자산과 영향도 분석 대상 자산을 컬럼 단위까지 drill-down | 핵심 파생 컬럼의 "입력 컬럼 + 변환 로직"이 기록된 비율 ≥ 80% |
| **4. AI 구간** | AI 답변 근거 Logging | RAG 답변 근거 로그 스키마 적용, Citation 화면 표기, retrieved_chunks·tables_used·tool_calls 기록 | 임의 AI 답변 선택 시 answer_id로 근거 역추적 가능 여부 확인 |
| **5. 자동화** | 운영 자동화 | 영향도 조회 의무화(변경 전 승인 프로세스), Lineage Report 버튼화, 리니지 갱신 자동화 | 사람의 수작업 없이 영향도 목록이 생성되는지 확인 |

### 1.2 CDE(핵심 데이터 요소) 선정 기준

CDE는 모든 데이터에 리니지를 거는 것이 불가능하기 때문에 "먼저 걸 대상"을 구별하는 기준이다 [source: https://www.collibra.com/blog/what-is-data-lineage].

- AI 과제(PdM, 품질 RAG, 공정 최적화 등)에 **실제로 입력**되는 피처·문서
- 오류 시 타격이 큰 **안전·품질·규제** 관련 데이터 (예: 공정 한계값, 검사 판정)
- 여러 팀이 **중복 참조**하는 공용 마스터(설비 코드, 자재 코드, 거래처 코드)
- 주기적인 **감사·인증 대상** 데이터

선정된 CDE마다 단일 **데이터 스튜어드(Steward)** 를 지정한다. 스튜어드는 기술(IT)과 업무(OT·현업) 사이에서 리니지 정확성을 검증하고, "왜 이 변환을 했는지"를 보강하는 역할이다.

---

## 2. 캡처 방식: 자동·수동·하이브리드

### 2.1 캡처 방식 분류

| 캡처 방식 | 동작 원리 | 강점 | 약점 |
|---|---|---|---|
| **코드/SQL 파싱** | SQL·dbt·Stored Procedure·스크립트를 AST(추상 구문 트리)로 분석해 컬럼 단위 흐름 추출 | 가장 정밀, 변환 로직 완전 추적. DataHub SQL Parser는 97–99% 정확도 보고 [source: https://docs.datahub.com/docs/generated/lineage/automatic-lineage-extraction] | 코드가 없는 GUI ETL·수작업 Excel에는 미적용 |
| **런타임/실행 로그** | Airflow·Spark·dbt 실행 시점에 OpenLineage 이벤트를 방출, 실제로 돈 흐름을 기록 | "계획"이 아닌 "실제 실행" 기반 — 조건 분기·스킵된 작업도 반영 | 파이프라인 에이전트 설치 필요, 실행 빈도 낮으면 이력 희소 |
| **BI/Tool API** | Tableau, Power BI, Looker 등이 제공하는 의존성 API를 수집해 보고서→데이터셋 연결 | 보고서 계층까지 연결 완성 | BI 벤더마다 API 커버리지 상이 |
| **수동 입력** | 스튜어드가 카탈로그 UI에서 직접 노드·엣지 추가 | 자동으로 잡히지 않는 비즈니스 의미, OT→IT 경계 매핑 기록 가능 | 시간 비용, 변경 추적 누락 위험 |
| **하이브리드** | 기술 흐름은 자동 캡처 + 비즈니스 의미/OT 경계는 수동 | 지속 가능성 + 정확성 균형 | 역할 구분과 프로세스 정립 필요 |

**핸드북 7.1과의 일관성**: 핸드북이 제시한 하이브리드 원칙(기술 흐름 → 자동, 비즈니스 의미·오너십 → 스튜어드)이 시장 베스트 프랙티스와 일치한다 [source: https://openlineage.io/].

### 2.2 OpenLineage 표준

OpenLineage는 도구 간 리니지 메타데이터를 주고받는 **개방형 JSON 이벤트 표준**이다. 도구 종속 없이 Airflow·Spark·dbt·Flink 등 어느 오케스트레이터에서도 동일한 형식으로 이력을 수집한다 [source: https://github.com/OpenLineage/OpenLineage].

- **Airflow**: `apache-airflow-providers-openlineage` 패키지로 Task 단위 리니지 이벤트 자동 방출. Spark 작업과 parent-child 관계 자동 생성 [source: https://openlineage.io/docs/integrations/spark/configuration/airflow/]
- **Spark**: OpenLineage Spark Listener가 컬럼 단위 lineage를 포함한 이벤트 방출. DataHub의 Spark Event Listener와 연동 시 PathSpec 지원, 컬럼 단위 lineage, 패치 지원 추가 [source: https://docs.datahub.com/docs/lineage/openlineage]
- **dbt**: `openlineage.yml` 설정 파일로 dbt 모델 실행 시 자동 이벤트 방출. 컬럼 단위 lineage 지원 [source: https://medium.com/@sendoamoronta/open-lineage-in-modern-data-platforms-advanced-pipelines-with-dbt-and-airflow-59a4172ac52e]
- **Marquez**: OpenLineage 공식 Reference Implementation — 메타데이터 저장소 + 시각화 UI + REST API 제공 [source: https://blog.skyvia.com/top-data-lineage-tools/]

**Collibra**는 2025년 6월 릴리스에서 OpenLineage 통합을 추가, AWS Glue 및 Apache Airflow로부터의 lineage 추출이 개선됐다 [source: https://promethium.ai/guides/data-governance-tools-comparison-collibra-alation-atlan-purview/].

---

## 3. 수작업 Pain Point와 AI·자동화 해소 기술

### 3.1 수작업 리니지의 4대 Pain Point

1. **노후화(Staleness)**: 파이프라인 변경 한 번이면 수작업 문서가 즉시 낡음. "거짓 지도"는 없느니만 못함
2. **커버리지 공백**: 사람이 기억하고 기록해야 하므로 가장 복잡한 변환(여러 시스템 교차)이 빠짐
3. **컬럼 단위 불가**: 테이블 단위는 그릴 수 있어도 "어느 컬럼이 어느 컬럼에서 왔는가"를 손으로 추적하는 것은 현실적으로 불가능
4. **확장 비용**: 데이터 자산이 수천 개를 넘으면 수작업 유지 인력 비용이 구축 효익을 초과

### 3.2 AI·자동화가 해소하는 방식

| 기술 | 기능 | 대표 적용 도구 |
|---|---|---|
| **SQL/코드 AST 파싱** | 쿼리·저장 프로시저를 파싱해 컬럼 단위 lineage 자동 추출. DataHub 97–99% 정확도 [source: https://docs.datahub.com/docs/generated/lineage/automatic-lineage-extraction] | DataHub, IBM MANTA, Collibra, Informatica |
| **런타임 이벤트(OpenLineage)** | 실행 시점 이벤트로 "실제로 돈" 흐름 기록, 조건 분기 반영 | Airflow, Spark, dbt + Marquez/DataHub |
| **LLM 기반 Lineage 추론** | 자연어 설명·주석·코드에서 LLM/SLM이 lineage를 추론. CoT 프롬프트로 오픈소스 모델이 GPT-4 수준 달성 [source: https://www.mdpi.com/2079-9292/14/9/1762] | 연구/실험 단계, Ataccama AI 설명 기능 일부 반영 |
| **AI 변환 설명** | 복잡한 SQL·파이프라인 로직을 자연어로 자동 설명 → 비기술 사용자도 이해 | Ataccama ONE AI, Atlan AI |
| **ML 기반 오너 추천** | 메타데이터 패턴으로 자산 오너·스튜어드 자동 추천 | DataHub (LinkedIn ML 기반) [source: https://atlan.com/linkedin-datahub-metadata-management-open-source/] |
| **AI 답변 Citation 자동 기록** | RAG 답변 시 retrieved_chunks·tables_used·tool_calls를 자동 로깅 → answer_id로 역추적 | LangChain + Langfuse, LlamaIndex + OpenTelemetry, 자체 구현 |

---

## 4. Tech Spec — 시장 솔루션/플레이어 정리

### 4.1 상용 솔루션 비교표

| 도구 | 유형 | Lineage 방식 | 컬럼 레벨 | AI/RAG Lineage 지원 | Gartner 포지션 (2025) | 제조/OT 적합성 |
|---|---|---|---|---|---|---|
| **Collibra** | 상용 (엔터프라이즈) | QueryFlow 기술 — SQL 파싱 + OpenLineage(2025.06 추가). 테이블·컬럼 단위 [source: https://productresources.collibra.com/docs/collibra/latest/Content/ReleaseNotes/Archive/ref_release-202506.htm] | ✅ (테이블·컬럼·리포트 레벨) | 부분 (거버넌스 거버넌스 연계) | **Leader** [source: https://www.collibra.com/blog/collibra-named-a-leader-in-the-gartner-magic-quadrant-for-data-and-analytics-governance-platforms] | 중간 (IT 스택 중심, OT 커버리지 추가 구성 필요) |
| **Alation** | 상용 (엔터프라이즈) | Active Metadata 카탈로그 기반 + Manta 파트너십으로 컬럼 단위 크로스-시스템 lineage [source: https://www.alation.com/blog/alation-manta-automate-advanced-data-lineage/] | ✅ (Manta 통합 시) | 부분 (카탈로그 연계) | **Leader** [source: https://www.alation.com/gartner-magic-quadrant-data-analytics-governance-2026/] | 중간 |
| **Atlan** | 상용 (모던 스택) | SQL 파싱(Snowflake/BigQuery/Redshift/Databricks) + OpenLineage 런타임 이벤트 네이티브 수신. G2 9.1/10 [source: https://promethium.ai/guides/data-governance-tools-comparison-collibra-alation-atlan-purview/] | ✅ (네이티브, BI까지 연결) | ✅ AI lineage 및 MCP 연동, AI 답변 맥락 제공 [source: https://atlan.com/data-lineage/] | **Visionary** (2025 D&A Governance), **Leader** (Metadata Mgmt) [source: https://atlan.com/gartner-magic-quadrant-data-governance-2025/] | 낮음 (클라우드 모던 스택 특화, OT 비지원) |
| **Informatica IDMC** | 상용 (엔터프라이즈) | EDC(Enterprise Data Catalog) + Axon: 자동화 스캐너로 SQL·저장 프로시저·AI/ML 코드 파싱. 멀티클라우드 [source: https://www.ataccama.com/blog/top-data-lineage-tools-in-2025] | ✅ (고급 스캐너) | 부분 (CLAIRE AI 자동화) | **Leader** (최고 비전 완성도 · 실행 능력) [source: https://www.informatica.com/about-us/news/news-releases/2025/01/20250113-informatica-named-a-leader-in-2025-gartner-magic-quadrant-for-data-and-analytics-governance-platforms.html] | 중간 (SAP·Oracle 연동 강점, OT 직접 지원 제한) |
| **IBM watsonx.data Intelligence (MANTA)** | 상용 (엔터프라이즈) | 구 MANTA 기술 통합: 데이터베이스·ETL·BI 전방위 자동 lineage. 컬럼 단위 step-by-step flow analysis [source: https://www.ibm.com/products/watsonx-data-intelligence/data-lineage] | ✅ (기술 분석 특화, 가장 광범위한 소스 커버) | 부분 (거버넌스 연계) | **Leader** [source: https://www.ibm.com/new/announcements/ibm-recognized-as-a-leader-in-the-gartner-magic-quadrant-for-data-analytics-and-governance-platforms] | 중간-높음 (레거시·메인프레임 커버 강점) |
| **Microsoft Purview** | 상용 (Azure 생태계) | Azure Fabric·ADF·Synapse 네이티브 스캔. 비Azure 소스는 커넥터 개발 필요. 컬럼 단위는 일부 프리뷰 [source: https://learn.microsoft.com/en-us/purview/data-map-lineage-fabric] | △ (Azure 자산은 지원, Fabric 서브아이템은 프리뷰) | 부분 (Fabric AI Foundry 연계 예정) | Challenger | 낮음 (Azure 생태계 전제, OT 비지원) |
| **Ataccama ONE** | 상용 (Data Quality 중심) | 자동 lineage + 품질 오버레이를 lineage 다이어그램에 직접 표시(업계 유일 기능). AI 변환 설명 [source: https://www.ataccama.com/blog/top-data-lineage-tools-in-2025] | ✅ (기술·비즈니스 뷰 선택) | 부분 (AI 설명·agentic DQ) | Leader (Augmented DQ, 2026 Gartner) | 중간 (제조 레퍼런스 있음) |
| **Cloudera Data Lineage (구 Octopai)** | 상용 (BI·ETL 특화) | SaaS 기반 자동 수집. ETL·BI 40+ 도구 네이티브 통합. 비네이티브는 Universal Connector [source: https://www.octopai.com/data-lineage-xd/] | ✅ (BI/ETL 구간 전문) | 제한적 | Niche Player | 낮음 (IT/BI 전용) |

### 4.2 오픈소스 솔루션 비교표

| 도구 | 유형 | Lineage 방식 | 컬럼 레벨 | AI/RAG Lineage | 비고 |
|---|---|---|---|---|---|
| **DataHub** (LinkedIn) | 오픈소스 / DataHub Cloud(상용) | SQL 파싱(BigQuery·Snowflake·Redshift·dbt·Looker·Tableau 등 20+) + OpenLineage REST 엔드포인트. ML 기반 오너 추천 [source: https://docs.datahub.com/docs/generated/lineage/automatic-lineage-extraction] | ✅ (자동, 네이티브 커넥터) | 부분 (Graph DB 기반 컨텍스트) | LinkedIn 내부 생산 검증. JanusGraph + Elasticsearch + Kafka 아키텍처 |
| **OpenMetadata** | 오픈소스 | API-first. dbt·BigQuery·Snowflake·Redshift 자동 수집 + No-Code 수동 편집기(자동 탐지 보완용) [source: https://thedataguy.pro/blog/2025/08/open-source-data-governance-frameworks/] | ✅ + 수동 편집 | 부분 (v1.8 데이터 계약, 품질 프레임워크) | MySQL/PostgreSQL + Elasticsearch (그래프 DB 없음, 단순 아키텍처) |
| **Apache Atlas** | 오픈소스 (Hadoop 생태계) | Hive·HBase·Kafka·Spark 네이티브 후크. Hadoop 생태계 gold standard. REST API [source: https://thedataguy.pro/blog/2025/08/open-source-data-governance-frameworks/] | ✅ (Hadoop 내) | 제한적 | JanusGraph + Solr. Ranger와 통합해 PII 분류 자동 전파. Hadoop 외 연동은 약함 |
| **Marquez** | 오픈소스 (OpenLineage Ref.) | OpenLineage 이벤트 수신 전용 메타데이터 저장소 + 시각화 UI. Airflow·Spark 직접 통합 [source: https://blog.skyvia.com/top-data-lineage-tools/] | 부분 (OpenLineage 이벤트 의존) | 제한적 | 설정·운영에 데이터 엔지니어 전문성 요구. 학습 곡선 높음 |
| **Select Star** | 상용 (자동 발견 특화) | SQL 쿼리 로그 자동 분석 + Monte Carlo 통합(품질 인사이트) [source: https://www.selectstar.com/resources/monte-carlo-data] | ✅ | 제한적 | 사용량 기반 lineage(실제로 쓰인 컬럼 추적), 비기술 사용자 친화 |
| **Monte Carlo** | 상용 (Data Observability 특화) | Pipeline·Metrics·Validation·Performance 4종 모니터 + 통합 lineage [source: https://www.montecarlodata.com/blog-select-star-integration] | 부분 | 제한적 | 주력은 이상탐지(C-1 영역). Lineage는 품질 alert 맥락 제공용 |

### 4.3 선택 가이드 (제조업 AI 과제 기준)

```
[현재 스택이 Azure/Fabric 중심]
  → Microsoft Purview (네이티브 통합) + IBM MANTA (복잡 SQL 보강)

[SAP/Oracle ERP + 레거시 DW 환경]
  → Informatica IDMC 또는 IBM watsonx.data Intelligence
  (레거시 커버리지 최강)

[Snowflake/dbt/Tableau 모던 스택]
  → Atlan (G2 최고점, 컬럼 lineage + AI 기능)

[오픈소스 선호 + 데이터 엔지니어링 역량 보유]
  → DataHub (가장 성숙, OpenLineage 지원) 또는
    OpenMetadata (단순 아키텍처, No-Code 편집)

[Airflow/Spark 파이프라인 표준화 + 최소 도구]
  → OpenLineage 표준 + Marquez (가장 경량)

[품질·관찰성(C-1·C-2)과 통합된 lineage 필요]
  → Ataccama ONE (품질 오버레이 lineage, 제조 레퍼런스)
```

---

## 5. OT→IT 경계 Lineage — 제조 특유 난관 (핸드북 7.3 보강)

### 5.1 왜 어려운가

제조 현장 데이터는 OPC-UA·Modbus·MQTT·proprietary historian 프로토콜을 통해 PLC → SCADA → Historian(예: OSIsoft PI, Aspentech IP.21, Aveva) → MES/ERP → DW/AI 경로로 이동한다. 이 경로에서:

- **Historian은 태그 ID(예: `AI_0207`)를 키로 사용** — IT의 테이블명/컬럼명과 매핑이 없으면 lineage 자동 추적 불가 [source: https://www.iotforall.com/data-historians-industry-4]
- **OPC-UA·Modbus 레이어는 SQL 파서 미적용** — 표준 lineage 도구의 자동 캡처 대상 밖
- **단위·스케일 변환이 암묵적** — Historian 적재 시 공학 단위 변환(예: raw 카운트 → mm/s)이 별도 문서 없이 발생
- **2025년 현재 시장 도구(Collibra, DataHub 등) 모두 IT/클라우드 스택 전제** — OT historian 직접 커넥터를 제공하는 도구는 매우 드묾 [source: https://www.iot83.com/blog-posts/what-is-data-lineage-a-practical-implementation-guide]

### 5.2 현실적 대응 방안

1. **OT→IT 매핑표를 "첫 번째 수동 lineage 산출물"로 작성**
   - 태그 ID ↔ Historian 테이블 ↔ DW 컬럼 매핑
   - 단위·스케일 변환 규칙 명기
   - 이 매핑표 자체를 CDE로 지정하고 스튜어드 배정

2. **센서 교정·교체 이력을 메타데이터로 등록**
   - 변경 날짜, 변경 내용, 영향받는 DW 컬럼 목록을 변경 통제 프로세스에 포함

3. **Unified Namespace(UNS) + MQTT/Kafka 도입 시 lineage 연동 고려**
   - MQTT·Kafka 기반 UNS 구조는 OT 데이터를 스트림 이벤트로 IT에 노출 → OpenLineage 이벤트 방출 가능성 열림 [source: https://www.kai-waehner.de/blog/2025/07/01/unified-namespace-vs-data-product-in-it-ot-for-industrial-iot/]

4. **단기 현실적 목표**: OT→IT 경계의 *태그→DW 컬럼 매핑*과 *단위 변환 이력*을 카탈로그에 등록하는 것만으로도, 핵심 리니지 단절 구간을 "연결 가능한 노드"로 만들 수 있다.

---

## 6. RAG/LLM 답변 근거 로그·Citation 구현 방법 (핸드북 4장 확장)

### 6.1 AI 답변 근거 로그 — 상세 스키마

핸드북 4.2의 JSON 스키마를 구현 관점에서 확장한다.

```json
{
  "answer_id": "ans_20260602_001",
  "session_id": "sess_abc123",
  "timestamp": "2026-06-02T09:14:00Z",
  "user_id": "user@domain.com",
  "question": "2호기 압축기 베어링 결함 원인은?",
  "prompt_template_id": "defect-rca-v1.4",
  "model_id": "claude-sonnet-4-6",
  "model_version_tag": "2026.05",

  "retrieved_chunks": [
    {
      "doc_id": "CS_2231",
      "chunk_id": "c_8842",
      "source_path": "s3://rag-index/cs-reports/CS_2231.pdf",
      "source_display": "C/S Report #2231, p.3",
      "chunk_text_preview": "베어링 외륜 마모 패턴 관찰…",
      "retrieval_score": 0.91,
      "embedding_model": "text-embedding-3-large",
      "index_version": "v20260601"
    },
    {
      "doc_id": "PFMEA_2H",
      "chunk_id": "c_0157",
      "source_path": "s3://rag-index/pfmea/PFMEA_2호기.pdf",
      "source_display": "PFMEA 2호기, p.12",
      "retrieval_score": 0.88,
      "embedding_model": "text-embedding-3-large",
      "index_version": "v20260601"
    }
  ],

  "tables_used": [
    {
      "table_ref": "dw.feat_vib_rms_x",
      "time_range": "2026-05-01/2026-05-31",
      "rows_read": 44640,
      "lineage_node_id": "node_vib_rms_x_v3"
    }
  ],

  "tool_calls": [
    {
      "tool_name": "get_sensor_history",
      "input_params": {"tag": "AI_0207", "days": 30},
      "output_ref": "log_55021",
      "execution_timestamp": "2026-06-02T09:13:58Z"
    }
  ],

  "answer_text": "소재 결함 가능성 높음. 근거: ①②③",
  "citation_display": [
    "① C/S Report #2231 3p",
    "② PFMEA 2호기 12p",
    "③ 진동 이력 AI_0207 (최근 30일)"
  ]
}
```

이 스키마의 핵심 설계 원칙:
- **answer_id** → retrieved_chunks, tables_used, tool_calls 역추적 가능
- **index_version** 기록 → "어느 버전의 RAG 인덱스로 답했는가" 추적 (인덱스 업데이트 전후 답변 차이 비교 가능)
- **lineage_node_id** → 기존 데이터 lineage 그래프의 노드와 연결 (AI 구간과 전통 데이터 lineage를 이음)

### 6.2 Citation 구조 — 사용자 화면 표기

RAG 시스템에서 Citation을 구현하는 3가지 패턴 [source: https://www.tensorlake.ai/blog/rag-citations]:

| 패턴 | 방식 | 적합 상황 |
|---|---|---|
| **인라인 Citation** | 답변 문장 내 `[1]`, `[2]` 번호 삽입, 참고문헌 목록 연결 | 학술·기술 문서 수준 엄밀성 필요 시 |
| **출처 카드** | 답변 하단에 참조 문서 카드(제목·페이지·신뢰도 점수) 나열 | 현업 RAG 챗봇 UI 일반적 방식 |
| **하이라이트 링크** | 답변 문장 클릭 시 원문 PDF 해당 위치로 이동 | 원문 확인 필요성이 높은 안전·품질 도메인 |

제조업 RAG 과제에서는 **출처 카드 + 하이라이트 링크** 조합이 현업 수용성과 추적성을 모두 만족한다.

### 6.3 구현 도구·프레임워크

| 도구 | 역할 |
|---|---|
| **Langfuse** | LangChain·LlamaIndex 연동 RAG 파이프라인 전체 트레이싱. retrieved_chunks·tool_calls 자동 기록. 오픈소스 + 상용 SaaS [source: https://medium.com/@thepracticaldeveloper/top-open-source-llm-observability-tools-in-2025-d2d5cbf4b932] |
| **OpenTelemetry GenAI** | LLM 호출을 span으로 감싸 model명·token수·finish reason 등 표준 속성 기록. OpenAI·Anthropic·LangChain 자동 계측 가능 [source: https://uptrace.dev/blog/opentelemetry-ai-systems] |
| **OpenLLMetry (Traceloop)** | OpenTelemetry 기반 LLM 앱 트레이싱 SDK. LangChain·LlamaIndex·Haystack 자동 계측 [source: https://github.com/traceloop/openllmetry] |
| **LlamaIndex Observability** | `set_global_handler` 한 줄로 Langfuse·Arize·Weights&Biases 연동. 컨텍스트 증강·LLM 쿼리 상세 트레이스 [source: https://developers.llamaindex.ai/python/framework/module_guides/observability/] |

**설계 핵심**: AI 답변 근거 로그는 **처음부터 요구사항에 포함**해야 한다. LangChain·LlamaIndex 기반 RAG 시스템은 Langfuse 또는 OpenLLMetry를 프로젝트 초기에 붙이면 retrieved_chunks·tool_calls가 자동 기록된다. 나중에 추가하려면 파이프라인 재설계가 필요하다 (핸드북 4.3의 "이런 일을 하면 됩니다 ③" 강조 사항과 일치).

---

## 7. 수동 vs 자동 vs 하이브리드 — 최종 권고

| 구분 | 수동 | 자동 | 하이브리드 (권고) |
|---|---|---|---|
| **커버리지** | 아는 것만 | 자동 인식 가능한 것만 | 자동 + 수동 보완으로 최대 |
| **정확성** | 변경 즉시 낡음 | 실행 기반으로 최신 | 자동 기반 + 스튜어드 검증 |
| **비즈니스 의미** | 가능 | 불가 | 스튜어드가 담당 |
| **OT→IT 경계** | 가능(시간 비용 큼) | 거의 불가 | 수동 매핑표 + 자동 캡처 연결 |
| **초기 비용** | 낮음 | 도구 도입 비용 있음 | 중간 |
| **유지 비용** | 매우 높음 (변경 추적 수작업) | 낮음 | 낮음 |
| **적합 단계** | 0. 준비(CDE 선정) | 2. 교차 연결 이후 | 1. 파일럿부터 |

---

## 8. 핵심 요약 — How 섹션 스토리라인

1. **아무것도 없는 상태에서 시작**: CDE 선정 → 스튜어드 지정 → 파일럿(AI 과제 1개 End-to-End) → 시스템 경계 연결 → 컬럼 심화 → AI 구간 → 자동화 순서로 단계 밟기.

2. **캡처는 하이브리드**: 기술 흐름(SQL 파싱·런타임 로그·OpenLineage)은 자동, "왜"라는 비즈니스 의미와 OT→IT 경계는 스튜어드가 수동 보완.

3. **도구 선택**: 스택에 맞게 선택. 제조업 레거시 환경에는 IBM MANTA/Informatica, 모던 클라우드 스택에는 Atlan, 오픈소스 선호 시 DataHub + OpenLineage, 최소 구성은 OpenLineage + Marquez.

4. **AI 답변 근거 로그는 처음부터**: LangChain/LlamaIndex + Langfuse 조합으로 retrieved_chunks·tables_used·tool_calls를 answer_id에 귀속해 자동 기록. Citation 화면 표기(출처 카드 + 원문 링크)도 초기 요구사항으로 확정.

5. **OT→IT는 별도 전략 필요**: 시중 도구의 자동 커버 범위 밖이므로, historian 태그 ↔ DW 컬럼 수동 매핑표를 첫 번째 deliverable로 작성하고 CDE로 관리.

---

## 참고 출처

1. OpenLineage 공식 문서: https://openlineage.io/
2. OpenLineage GitHub: https://github.com/OpenLineage/OpenLineage
3. OpenLineage Airflow-Spark 통합: https://openlineage.io/docs/integrations/spark/configuration/airflow/
4. DataHub 자동 Lineage 추출: https://docs.datahub.com/docs/generated/lineage/automatic-lineage-extraction
5. DataHub OpenLineage 통합: https://docs.datahub.com/docs/lineage/openlineage
6. Collibra Data Lineage 제품 페이지: https://www.collibra.com/products/data-lineage
7. Collibra 2025.06 릴리스 노트: https://productresources.collibra.com/docs/collibra/latest/Content/ReleaseNotes/Archive/ref_release-202506.htm
8. Alation + Manta 파트너십: https://www.alation.com/blog/alation-manta-automate-advanced-data-lineage/
9. Atlan Data Lineage: https://atlan.com/data-lineage/
10. Atlan AI-Ready Data Lineage: https://atlan.com/know/ai-readiness/ai-ready-data-lineage/
11. Atlan Gartner MQ 2025: https://atlan.com/gartner-magic-quadrant-data-governance-2025/
12. Informatica Gartner Leader 2025: https://www.informatica.com/about-us/news/news-releases/2025/01/20250113-informatica-named-a-leader-in-2025-gartner-magic-quadrant-for-data-and-analytics-governance-platforms.html
13. IBM watsonx.data Intelligence Lineage: https://www.ibm.com/products/watsonx-data-intelligence/data-lineage
14. IBM Manta 문서: https://www.ibm.com/docs/en/watsonxdata/standard/2.1.x?topic=components-manta-data-lineage
15. IBM Gartner Leader 인정: https://www.ibm.com/new/announcements/ibm-recognized-as-a-leader-in-the-gartner-magic-quadrant-for-data-analytics-and-governance-platforms
16. Microsoft Purview Fabric Lineage: https://learn.microsoft.com/en-us/purview/data-map-lineage-fabric
17. Ataccama 2025 상위 Lineage 도구: https://www.ataccama.com/blog/top-data-lineage-tools-in-2025
18. OpenMetadata vs DataHub 비교: https://atlan.com/openmetadata-vs-datahub/
19. 오픈소스 거버넌스 프레임워크 비교: https://thedataguy.pro/blog/2025/08/open-source-data-governance-frameworks/
20. Octopai/Cloudera Data Lineage XD: https://www.octopai.com/data-lineage-xd/
21. Select Star + Monte Carlo 통합: https://www.selectstar.com/resources/monte-carlo-data
22. Monte Carlo 아키텍처: https://docs.getmontecarlo.com/docs/architecture
23. Marquez + OpenLineage: https://blog.skyvia.com/top-data-lineage-tools/
24. Gartner MQ 데이터 거버넌스 플랫폼: https://www.gartner.com/en/documents/6059363
25. Alation Gartner Leader 2026: https://www.alation.com/gartner-magic-quadrant-data-analytics-governance-2026/
26. Collibra Gartner Leader: https://www.collibra.com/blog/collibra-named-a-leader-in-the-gartner-magic-quadrant-for-data-and-analytics-governance-platforms
27. RAG Citation 구현: https://www.tensorlake.ai/blog/rag-citations
28. LLM Lineage 파싱 (LLM 기반 추론 논문): https://www.mdpi.com/2079-9292/14/9/1762
29. OpenTelemetry AI/LLM 관찰성: https://uptrace.dev/blog/opentelemetry-ai-systems
30. OpenLLMetry (Traceloop): https://github.com/traceloop/openllmetry
31. LlamaIndex 관찰성 통합: https://developers.llamaindex.ai/python/framework/module_guides/observability/
32. LLM 관찰성 도구 2025 (Langfuse 포함): https://medium.com/@thepracticaldeveloper/top-open-source-llm-observability-tools-in-2025-d2d5cbf4b932
33. 데이터 Lineage in dbt + Airflow (2026): https://medium.com/@sendoamoronta/open-lineage-in-modern-data-platforms-advanced-pipelines-with-dbt-and-airflow-59a4172ac52e
34. 제조업 OT→IT Lineage 도전: https://www.iot83.com/blog-posts/what-is-data-lineage-a-practical-implementation-guide
35. Unified Namespace vs Data Product (OT/IT): https://www.kai-waehner.de/blog/2025/07/01/unified-namespace-vs-data-product-in-it-ot-for-industrial-iot/
36. 프롬테우스 AI 거버넌스 도구 비교: https://promethium.ai/guides/data-governance-tools-comparison-collibra-alation-atlan-purview/
37. AWS SageMaker + OpenLineage (dbt·Airflow·Spark): https://aws.amazon.com/blogs/big-data/capture-data-lineage-from-dbt-apache-airflow-and-apache-spark-with-amazon-sagemaker/
