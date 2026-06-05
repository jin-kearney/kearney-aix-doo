# E-1 Data Product화 (Data as a Product) — How 클러스터 리서치

> 주제코드: E-1 | 담당 클러스터: How | 작성일: 2026-06-03

---

## 1. Data Product 0→1 구축 방법론 / Phase별 로드맵

### 1.1 Data Product란 무엇인가 (구축 전 전제 확인)

Data Product는 "소비자 수요를 충족시키기 위해 만들어진, 최소 하나 이상의 도메인 데이터셋(Domain Dataset)을 담은 자율적·읽기 최적화·표준화된 데이터 단위"(Majchrzak et al., *Data Mesh in Action*)다. Data Mesh 구조에서 각 노드(node)가 Data Product이며, 각 Data Product는 하나의 Bounded Context 안에 존재한다.

구체적인 Data Product 예시(datamesh-architecture.com):
- 데이터베이스 테이블 또는 뷰
- 원천 비정형 파일(이미지·영상 + 메타데이터)
- 트랜잭션 시스템 데이터 스트림
- 변경 이력(Change Data Capture) 이벤트 스트림
- CSV 파일, Parquet 파티셔닝 파일
- 읽기 최적화 REST API
- MDM(Master Data Management) 데이터베이스
- ML 피처(Feature)
- 시각화·대시보드

### 1.2 Data Product 0→1 구축 5단계 로드맵

전체 구현 기간은 일반적으로 12~24개월이며, 도메인 수·레거시 복잡도·인력에 따라 달라진다. 아래는 단계별 정리다.

---

#### Phase 1: 전략 수립 및 현황 진단 (1~2개월)

**목표**: 출발점 파악, 목적지 합의, 이해관계자 정렬

**주요 활동**:
- 기술 성숙도 평가: 도메인 팀이 독립적으로 데이터 파이프라인을 운영할 수 있는가?
- 데이터 도메인 식별 및 경계(Bounded Context) 정의
- Data Product Owner 후보 역할 지정
- 플랫폼 자동화 수준 현황 파악
- 거버넌스 정책 기초 설계

**산출물**:
- 현황 진단 보고서(데이터 플랫폼 성숙도 매핑)
- 도메인 맵(Domain Map)
- 파일럿 도메인·파일럿 Data Product 후보 목록
- 실행 로드맵(Roadmap)

**선행 조건**:
- 경영진의 Data Mesh 전환 의지 및 예산 승인
- 도메인 팀 대표자 Cross-functional 참여

---

#### Phase 2: 파일럿 MVP — Data Product 0→1 (2~4개월)

**목표**: 1개 도메인, 2~3개 Data Product를 실제로 만들어 플랫폼·거버넌스·문화적 공백 발견

**주요 활동**:
1. **후보 도출**: 반복 요청 빈도, 다팀 사용, AI·분석 과제에서의 활용 여부로 후보 스코어링
2. **Data Product Canvas 작성**: 8개 블록(Domain, 이름, Consumer & Use Case, Data Contract, Sources, Architecture, Ubiquitous Language, Classification) 협업 작성
3. **Data Product Spec 명세서 작성**: YAML 기반 ODPS 또는 Agile Lab DPS 형식
4. **Data Contract 초안 정의**: 스키마, SLA, 품질 기준, 접근 정책 초안
5. **Output Port 구현**: 테이블/API/파일 중 가장 단순한 방식 1가지 선택
6. **카탈로그/마켓플레이스 등록**: 조직 내 데이터 카탈로그에 초기 메타데이터 등록
7. **소비자 피드백 루프**: 실제 소비자(분석가·AI 엔지니어)의 사용성 피드백 수집

**산출물**:
- 완성된 Data Product Canvas(2~3개)
- YAML 형식의 Data Product Spec 파일
- Data Contract YAML 파일(datacontract.com 또는 ODCS 형식)
- Output Port 구현 코드(예: BigQuery 뷰 + REST API)
- 카탈로그 등록 완료 레코드
- 파일럿 회고(Retrospective) 보고서

**선행 조건**:
- Self-Serve 플랫폼 최소 환경(스토리지, 쿼리 엔진, CI/CD)
- 데이터 카탈로그 기초 도구 도입

---

#### Phase 3: 스케일 준비 — 표준화·플랫폼 강화 (3~6개월)

**목표**: 파일럿 학습을 반영하여 재사용 가능한 템플릿·플랫폼 구성 고도화

**주요 활동**:
- Data Product Spec 템플릿 표준화(도메인별 확장 허용)
- 데이터 계약(Data Contract) 자동 유효성 검증 파이프라인 구축
- Federated Governance 정책 코드화(OPA, Dataplex Policy 등)
- 카탈로그-파이프라인-버전관리 통합(GitOps 방식)
- Data Product 품질 모니터링 대시보드 구축
- Data Product Owner 교육 프로그램 운영

**산출물**:
- 표준 Data Product 템플릿 라이브러리
- 자동 검증 CI/CD 파이프라인
- 거버넌스 정책 코드 저장소
- 내부 Data Product 마켓플레이스 v1.0

---

#### Phase 4: 확장 및 스케일 (6~12개월)

**목표**: 추가 도메인 온보딩, Data Product 수 증가, 소비자 기반 확대

**주요 활동**:
- 도메인별 순차적 온보딩(한꺼번에 전부 아닌, 단계적 확장)
- AI/ML 과제 연동: Feature Store, RAG 소스로 Data Product 활용
- Data Product 마켓플레이스 고도화(탐색·구독·API 토큰 발급)
- 비용 청구 모델(Chargeback) 도입 검토
- 사용량·가치 측정 KPI 대시보드 운영

**산출물**:
- 다수 도메인에 걸친 Data Product 포트폴리오
- AI 과제 연계 Data Product 목록
- 내부 마켓플레이스 운영 현황 보고서

---

#### Phase 5: 지속 운영·버전관리 (상시)

**목표**: Data Product를 살아있는 제품(Living Product)으로 관리

**주요 활동**:
- SemVer(Semantic Versioning) 기반 버전관리
- Breaking Change 탐지 자동화(Data Contract CLI)
- Deprecation → Sunset 프로세스(공지 → 마이그레이션 지원 → 종료)
- 사용량 감소 제품의 Retirement 판정
- 소비자 피드백 기반 Product 로드맵 갱신

**선행 조건**:
- Git 기반 Data Product Spec 저장소 운영
- 카탈로그에 버전·수명주기 상태 필드 반영

---

### 1.3 Data Product 후보 선정 기준 및 스코어링 프레임워크

수작업으로 수백 개 데이터셋을 검토하는 대신, 아래 스코어링 기준을 사용한다.

| 평가 기준 | 설명 | 가중치 예시 |
|---|---|---|
| 반복 요청 빈도 | 과거 6개월 내 동일 데이터셋에 대한 데이터 요청 티켓 수 | 25% |
| 소비 팀 수 | 이미 해당 데이터를 사용 중인 팀·개인 수 | 20% |
| AI/분석 과제 연동 | 현재 진행 중 또는 계획된 AI·분석 과제의 필수 데이터 여부 | 20% |
| 데이터 품질 가능성 | 원천 데이터 정합성·최신성 수준 | 15% |
| 구현 복잡도(역산) | 파이프라인 복잡도 낮을수록 우선 | 10% |
| 비즈니스 임팩트 | 연결된 비즈니스 지표·의사결정의 중요도 | 10% |

스코어 = Σ(기준별 점수 × 가중치). 상위 5~10개 후보를 파일럿으로 선정한다.

**추가 분류 기준**:
- Source-aligned: 원천 시스템(ERP·MES·QMS)에 1:1 대응 — 초기 구축 용이
- Aggregate: 여러 원천을 통합·변환 — 다팀 소비 니즈 충족
- Consumer-aligned: 특정 소비자 유스케이스 최적화 — BI 리포트·AI 모델 피처

---

## 2. Data Product 명세서(Spec) 실제 템플릿

### 2.1 Open Data Product Specification (ODPS) 3.0/4.x

ODPS는 Linux Foundation 프로젝트로, 벤더 중립적 오픈소스 기계가독형(machine-readable) Data Product 메타데이터 모델이다. 버전 4.1(2025년 10월 기준)까지 출시되었으며, Medallion Architecture 레이어 확장, Product Strategy 객체(KPI↔비즈니스 목표 연결) 추가가 특징이다.

**ODPS 3.0 필수(REQUIRED) 필드:**

```yaml
schema: https://opendataproducts.org/v3.0/schema/odps.yaml
version: "3.0"
product:
  details:
    en:
      name: "공정 품질 지표 데이터셋"          # 필수: Data Product명
      productID: "mfg-quality-001"            # 필수: 고유 식별자
      visibility: "organisation"              # 필수: private|invitation|organisation|dataspace|public
      status: "production"                    # 필수: announcement|draft|development|testing|acceptance|production|sunset|retired
      type: "dataset"                         # 필수: dataset|derived data|algorithm|dashboard 등
```

**ODPS 3.0 주요 선택(OPTIONAL) 필드:**

```yaml
      valueProposition: "MES에서 수집된 공정 품질 지표를 AI 과제 및 분석팀이 재사용할 수 있도록 표준화된 데이터셋"
      description: "SAP MES 공정 데이터를 일별 집계하여 제공하는 품질 지표 Data Product"
      productVersion: "1.2.0"                 # SemVer 기반 버전
      categories: ["manufacturing", "quality"]
      tags: ["MES", "QMS", "defect-rate"]
      outputFileFormats: ["Parquet", "CSV", "JSON"]
      useCases:
        - useCase:
            useCaseTitle: "불량률 예측 모델 피처 소스"
            useCaseDescription: "AI 불량 예측 모델의 학습·추론 피처 소스로 활용"
            useCaseURL: "https://internal.wiki/ai/defect-prediction"
      recommendedDataProducts:
        - "https://catalog.internal/products/equipment-downtime.json"
```

**ODPS SLA 객체 — 11개 표준 차원:**

```yaml
SLA:
  - dimension: uptime
    objective: 99
    unit: percent
    monitoring:
      type: prometheus
      spec: |
        avg_over_time(up{job="data-product-api"}[7d])
  - dimension: updateFrequency
    objective: 30
    unit: minutes
  - dimension: latency
    objective: 100
    unit: milliseconds
  - dimension: errorRate
    objective: 0.1
    unit: percent
  - dimension: timeToDetect
    objective: 60
    unit: minutes
  - dimension: timeToNotify
    objective: 120
    unit: minutes
  - dimension: timeToRepair
    objective: 24
    unit: hours
  - dimension: endOfSupport
    objective: "31/12/2026"
    unit: date
```

**ODPS Data Quality 객체 — 8개 표준 차원:**

```yaml
dataQuality:
  - dimension: accuracy
    objective: 98
    unit: percentage
    monitoring:
      type: SodaCL
      spec:
        - require_range(defect_rate, 0, 100)
  - dimension: completeness
    objective: 99.9
    unit: percentage
  - dimension: timeliness
    objective: 100
    unit: percentage
  - dimension: uniqueness
    objective: 100
    unit: percentage
```

### 2.2 Agile Lab Data Product Specification (오픈소스)

실제 엔터프라이즈 Data Mesh 구현에서 사용 중인 Agile Lab의 Data Product Specification은 다음 요소를 포함한다. (출처: Agile Lab Engineering Medium, github.com/agile-lab-dev/Data-Product-Specification)

**핵심 컴포넌트**:
- `domain`: 소속 도메인(Bounded Context)
- `dataProductOwner`: 오너 식별자(Active Directory 그룹 기반 권장)
- `inputPorts`: 원천 시스템 연결 설명(ERP·MES 등 각 원천별 입력 포트)
- `outputPorts`: 소비 방식별 포트(type: SQL Table / REST API / Files / Streaming)
- `workloads`: 파이프라인·변환 로직 기술
- `stagingAreas`: 내부 임시 저장소
- `observability`: 품질 모니터링 엔드포인트
- `dataSharingAgreements`: 데이터 계약(접근 조건·사용 목적·제약)
- `semanticLinks`: 다른 Data Product와의 의미적 연결(폴리세미 처리)
- `slo`: 서비스 레벨 목표(SLO)

**자동화 포인트**: YAML Spec이 플랫폼에 의해 파싱되어 ①Data Product 생성 부트스트랩, ②거버넌스 정책 자동 검증, ③Change Management(브레이킹 체인지 감지), ④Self-Service 인프라 자동 프로비저닝에 활용된다.

### 2.3 Data Product Canvas (datamesh-architecture.com)

8개 블록으로 구성된 협업 설계 도구. CC BY 4.0 라이선스, PDF 무료 다운로드 및 Miro 템플릿 제공.

| 블록 | 내용 |
|---|---|
| Domain | 어떤 도메인 팀이 소유·운영하는가? |
| Data Product Name | 조직 내 고유 명칭 |
| Consumer & Use Case | 소비자 유스케이스, 분석 목적, 비즈니스 목표 |
| Data Contract | Output Port 형식·프로토콜, 스키마, 사용 조건 |
| Sources | Input Port: 원천 운영 시스템 또는 다른 Data Product |
| Data Product Architecture | 내부 변환·저장·처리 로직 (Input → Output) |
| Ubiquitous Language | 도메인 공통 용어 사전 |
| Classification | Source-aligned / Aggregate / Consumer-aligned |

### 2.4 Data Contract (ODCS / datacontract.com)

Data Contract는 Data Product의 Output Port와 소비자 사이의 인터페이스 합의서다.

**두 주요 표준**:
- **ODCS v3.1.0** (Open Data Contract Standard, Linux Foundation의 Bitol 프로젝트): 인프라, 스키마, 품질 규칙, SLA, 팀 정보, 접근 제어를 포함한 포괄적 구조
- **datacontract.com Spec**: OpenAPI·AsyncAPI 관례를 따르는 YAML 형식. `datacontract.yaml` 파일로 저장 권장. 플랫폼 중립(BigQuery·Snowflake·Databricks·S3 등 모두 지원)

**ODCS v3 핵심 필드 예시**:

```yaml
dataContractSpecification: 2.3.0
id: urn:datacontract:mfg:quality-metrics-v1
info:
  title: "공정 품질 지표 계약"
  version: 1.0.0
  owner: "mfg-data-team@company.com"
  description: "MES 품질 데이터를 AI·분석팀에 제공하는 계약"
servers:
  production:
    type: bigquery
    project: mfg-data-prod
    dataset: quality_metrics
models:
  quality_daily:
    description: "일별 공정 품질 집계"
    fields:
      production_date: {type: date, required: true}
      line_id: {type: string, required: true}
      defect_rate: {type: float, description: "불량률 (0~100%)"}
      throughput: {type: integer}
quality:
  type: SodaCL
  specification:
    checks for quality_daily:
      - row_count > 0
      - missing_count(line_id) = 0
      - min(defect_rate) >= 0
      - max(defect_rate) <= 100
servicelevels:
  freshness:
    description: "데이터 최신성"
    freshness_expression: "MAX(production_date)"
    threshold: 1
    unit: day
  availability: "99%"
```

**Data Contract CLI** (오픈소스): lint, test, import, export 지원. datacontract.com 및 ODCS 양 표준 지원.

---

## 3. 이질적 솔루션 환경 추상화 계층

### 3.1 제조 계열사 환경의 문제

제조 그룹사는 ERP(SAP·Oracle), MES(여러 벤더), QMS(IQS·ETQ), LIMS(STARLIMS·LabWare) 등 이질적 솔루션을 보유하며, IT 시스템과 OT(Operation Technology) 시스템이 서로 다른 프로토콜·데이터 모델·소유 구조로 분리되어 발전했다. 결과적으로 공장 바닥 데이터가 비즈니스 시스템에 보이지 않고, 반대로 비즈니스 데이터가 OT에 보이지 않는 '데이터 사일로'가 형성된다.

### 3.2 추상화 계층 4가지 접근법

#### A. Data Virtualization (데이터 가상화)

데이터를 물리적으로 이동하지 않고 분산된 원천에 대한 통합·실시간 접근 추상화 계층을 생성한다. 가상화 엔진이 사용자 요청을 원천별 쿼리로 변환·실행하여 결과를 통합 뷰로 반환한다.

**장점**:
- 원천 시스템 변경(ERP 업그레이드·클라우드 마이그레이션)이 하위 분석에 영향을 주지 않음
- 실시간 데이터 접근 가능(ETL 배치 지연 없음)
- 중앙 집중식 거버넌스·데이터 마스킹 지원

**제조 활용 예시**: SAP ERP의 자재 마스터, MES의 공정 이력, QMS의 검사 결과를 단일 SQL 인터페이스로 조회하여 "제품 품질 종합 데이터 뷰" Data Product 제공

**주요 솔루션**: Denodo(150+ 커넥터, 25년 이상 경력), Starburst(Trino 기반, 오픈소스), Dremio

#### B. Semantic Layer (시맨틱 레이어)

데이터 가상화 위에 메트릭 정의, 문서화, 조인 경로, 접근 정책을 추가한 비즈니스 의미 레이어다. 소비자는 SQL이나 기술적 스키마 세부 사항 없이 비즈니스 개념으로 데이터에 접근한다.

**기능**:
- 원천 변경을 Semantic Layer가 흡수 → 하위 분석 파이프라인 불변
- 거버넌스 정책을 뷰 레벨에서 집행
- 일관된 메트릭(KPI) 정의를 전사적으로 공유

**주요 솔루션**: dbt Semantic Layer, AtScale, Cube.js, LookML

#### C. Federated Query (연합 쿼리)

단일 쿼리를 여러 이질적 원천(RDBMS·NoSQL·오브젝트 스토리지·API)에 분산 실행하여 결과를 통합한다. Data Product는 이 Federated Query를 Output Port로 노출한다.

**주요 솔루션**: Starburst(Trino), Dremio, AWS Athena Federation, Google BigQuery Omni

#### D. Unified Namespace (UNS) — OT/IT 통합

제조 현장 전용 패턴으로, 공장 내 모든 시스템(PLC·SCADA·MES)이 데이터를 단일 실시간 브로커(MQTT·Kafka)에 발행하고, 필요한 시스템이 구독하는 방식이다. Data Product는 UNS의 토픽을 소비하여 구조화된 Output Port를 생성한다.

**구현 레이어**:
1. 기계 레이어: OPC-UA, MQTT 프로토콜
2. 시스템 레이어: API·ETL로 ERP·MES·QMS 연결
3. 시맨틱 레이어: 데이터 매핑·변환 규칙으로 일관성 보장

### 3.3 Self-Serve Data Platform 개념

Self-Serve 플랫폼은 도메인 팀이 중앙 데이터 엔지니어링 팀의 도움 없이 Data Product를 독립적으로 빌드·배포·운영할 수 있게 하는 인프라다.

**최소 구성 요소**:
- 스토리지 추상화(Data Lake / Lakehouse)
- 쿼리 엔진(SQL 인터페이스)
- 파이프라인 오케스트레이션(Airflow·Prefect·dbt)
- CI/CD 파이프라인 (Data Product Spec → 자동 배포)
- 데이터 카탈로그·마켓플레이스
- 접근 제어 자동화(RBAC·ABAC)
- 품질·SLA 모니터링 대시보드

---

## 4. Output Port 소비 방식별 설계

Data Product의 Output Port는 소비자가 데이터를 사용하는 방식에 따라 다양하게 설계된다.

### 4.1 API (REST / GraphQL) 방식

**적합 케이스**: 실시간 조회, 애플리케이션 연동, 이벤트 기반 소비

**REST API**:
- OpenAPI 스펙(OAS 3.0)으로 계약 문서화
- CRUD 시맨틱 + OData 쿼리 오퍼레이터 지원
- 인증: OAuth2.0, API Key 방식
- 예: Azure Data API Builder — DB를 REST+GraphQL API로 자동 노출

**GraphQL**:
- 소비자가 필요한 필드만 쿼리(오버/언더페칭 방지)
- 단일 요청으로 여러 도메인 데이터 조인 가능
- Data Product의 기본 접근 메커니즘으로 적합 (Hasura, Apollo)
- Microsoft Fabric도 내장 GraphQL API 지원

**설계 포인트**:
- API 버저닝 정책(v1, v2 경로 분리 또는 Accept-Version 헤더)
- Rate Limit 및 SLA 설정(latency 목표값 ODPS에 명시)
- API Gateway를 통한 중앙 보안·로깅

### 4.2 테이블/SQL 방식

**적합 케이스**: BI·분석, 데이터 엔지니어링 파이프라인 연동, 대용량 배치 처리

**구현 방법**:
- 데이터 웨어하우스(BigQuery·Snowflake·Databricks)의 공유 뷰(View) 또는 테이블
- Delta Sharing(Databricks): 오픈 프로토콜로 비Databricks 소비자도 지원
- Iceberg REST Catalog API: Unity Catalog의 테이블을 Trino·Flink·Dremio 등에서 직접 조회

**설계 포인트**:
- 소비자에게 물리 테이블 직접 접근 지양 → 뷰(View) 또는 Shared Schema 추상화
- Row-level 보안 및 Column-level 마스킹 적용
- Data Contract의 스키마 정의와 일치 보장

### 4.3 파일 방식

**적합 케이스**: 대용량 배치 처리, ML 학습 데이터, 시스템 간 데이터 교환

**형식**:
- Parquet (컬럼형, 압축 효율 우수) — ML 학습·대용량 분석
- CSV (범용) — 레거시 시스템 연동, 엑셀 소비
- JSON Lines — 이벤트·로그 데이터
- Delta/Iceberg 포맷 (ACID + 스키마 진화 지원)

**설계 포인트**:
- 파티셔닝 전략(날짜·공장·제품별) 사전 정의
- 파일 경로 네이밍 규칙 표준화(Data Contract에 포함)
- 오브젝트 스토리지(S3·Azure Blob·GCS) 접근 제어

### 4.4 대시보드/BI 방식

**적합 케이스**: 경영진·현업 의사결정자, 비기술 소비자

**구현**:
- Power BI, Tableau, Looker, Grafana 등에서 Data Product의 SQL Output Port 연결
- Semantic Layer(dbt Semantic Layer·AtScale)를 통한 일관된 메트릭 제공
- Embedded Analytics: 내부 포털에 대시보드 임베딩

**설계 포인트**:
- 대시보드는 Output Port의 최종 소비층이지 Data Product 자체가 아님
- 대시보드 갱신 주기 = Data Product updateFrequency SLA와 동기화
- BI 툴이 바뀌어도 Data Product Output Port 스펙은 불변

### 4.5 RAG 소스 / Vector Store 방식

**적합 케이스**: LLM 기반 AI 에이전트, 기업 내부 Q&A 챗봇, 지식 검색

**아키텍처**:
1. Data Product에서 비정형 문서(기술 매뉴얼·SOP·품질 보고서) 또는 구조화 데이터를 제공
2. 임베딩 모델(embedding model)이 청크(chunk)를 벡터로 변환
3. 벡터 DB(Weaviate·Pinecone·pgvector·Chroma)에 저장
4. AI 에이전트가 유사도 검색(Cosine Similarity) 후 LLM 컨텍스트로 주입

**Data Product로 관리하면 좋은 이유**:
- 임베딩 재생성 주기·버전을 SLA로 관리 가능
- 접근 제어(어떤 AI 에이전트가 어떤 벡터 컬렉션에 접근 가능한지) 거버넌스 적용
- 원천 문서 변경 시 벡터 갱신 파이프라인 Data Product 내부에서 관리

**설계 포인트**:
- Hybrid Search(키워드 + 벡터) 패턴 적용 권장
- 메타데이터 필터(공장·제품·날짜)와 벡터 검색 결합
- 벡터 Output Port = 내부 RAG 시스템의 전용 컬렉션(Collection) URL

### 4.6 분석 패키지(Analytics Package) 방식

**적합 케이스**: 도메인 전문 분석·ML 모델을 패키지화하여 다른 팀이 재사용

**구현**:
- Python 패키지(PyPI·내부 패키지 레지스트리)
- dbt 패키지(dbt Hub)
- MLflow 모델 레지스트리를 통한 Feature Serving

**설계 포인트**:
- 패키지 버전과 Data Product Spec 버전 동기화
- 사용 예시 노트북(Example Notebook) 함께 제공
- 의존성 명세(requirements.txt)를 Data Product Spec에 포함

---

## 5. Pain Point & AI 기반 해결

### 5.1 수작업 Pain Point 목록

| Pain Point | 현황 | 규모 |
|---|---|---|
| Product 후보 수동 식별 | 수백 개 데이터셋 중 반복 요청·다팀 사용 데이터를 사람이 직접 파악 | 수주~수개월 소요 |
| 메타데이터·명세 수기 작성 | Data Product Spec YAML 수동 작성, 누락·오류 발생 | 제품당 수일 |
| Data Contract 수동 정의 | 스키마, SLA, 품질 기준을 팀 간 회의로 수동 협의 | 협의 지연 |
| SLA 모니터링 수동 확인 | 대시보드 수동 점검, 이상 탐지 지연 | 지연 인지 |
| 소비자 온보딩 수동 설명 | 사용법·스키마를 Slack·위키로 개별 안내 | 반복 비효율 |
| 이질적 원천 스키마 수동 매핑 | ERP·MES·QMS 스키마를 사람이 직접 비교·매핑 | 오류 다발 |

### 5.2 AI 기반 해결 기능 (구체적)

#### A. LLM 기반 Data Product 후보 추천

- **방식**: 데이터 카탈로그의 데이터셋 메타데이터(이름, 설명, 태그, 접근 로그, 쿼리 이력)를 LLM에 입력 → 후보 Data Product 스코어링 및 순위 추천
- **근거**: USPTO 등록 특허 "LLM-based recommender system for data catalog"(특허번호 12443607)에서 하이브리드 LLM+메타데이터 추천 방법론 공개 (출처: USPTO)
- **구현**: 후보 에셋 집합 → 메타데이터 기반 정제 → LLM 프롬프트 생성 → 구조화된 추천 목록 반환

#### B. 자동 명세·문서 생성 (LLM 기반 메타데이터 자동화)

- **방식**: 기존 스키마, DDL, 쿼리 이력, 데이터 샘플을 LLM에 입력하여 Data Product Spec YAML 초안 자동 생성
- **근거**: arXiv "Metadata Extraction Leveraging Large Language Models"(2025): Zero-shot·Few-shot 프롬프팅으로 LLM이 인간이 작성한 메타데이터에 준하는 품질 생성, 대형 모델이 소형 모델보다 우수, Fine-tuning이 정확도를 크게 향상
- **근거**: arXiv "Exploring LLM Capabilities in Extracting DCAT-Compatible Metadata for Data Cataloging"(2025): DCAT 호환 메타데이터 추출에서 LLM 자동화 가능성 확인
- **Atlan 사례**: AI 에이전트가 Enterprise Data Graph를 읽어 자산 설명, 비즈니스 용어 연결, 상위 비즈니스 질문 자동 생성 — 인간 검토 전 컨텍스트 레이어의 80% 자동 완성

#### C. NL→Data Contract 자동 생성

- **방식**: 자연어 요구사항("매일 갱신, 결측 허용 0%, 지연 30분 이내") → LLM이 ODCS YAML Data Contract 자동 생성
- **근거**: arXiv "AI-Driven Generation of Data Contracts in Modern Data Engineering Systems"(2025): LoRA·PEFT 기반 파인튜닝으로 도메인 특화 Data Contract 자동 생성. "AI-driven contract generation holds promise as a cutting-edge solution for agile, scalable data governance"
- **근거**: biorXiv "Multi-agent AI System for High Quality Metadata Curation at Scale"(2025): 다중 에이전트 AI 시스템으로 대규모 메타데이터 큐레이션 자동화 가능성 확인

#### D. 이상탐지 기반 SLA 자동 모니터링

- **방식**: ML 이상탐지(Anomaly Detection)로 Data Product SLA 자동 위반 감지 → 알림·인시던트 생성
- **사례**: SYNQ의 Scout — 자기학습(self-learning) 이상 모니터(freshness·volume·bite size·schema·growth 5종)로 모든 이슈를 자동 탐지, MCP(Model Context Protocol) 지원으로 AI 워크플로우 통합(2025년 8월 업데이트)
- **구현**: Prometheus as-code 모니터링 스펙을 ODPS SLA 객체에 내장하여 배포 시 자동 적용

#### E. Semantic 자동 매핑 (이질적 원천 간)

- **방식**: 여러 원천 시스템(ERP·MES·QMS)의 스키마를 LLM이 자동 비교·매핑하여 공통 Semantic Layer 생성
- **구현**: 두 스키마의 컬럼명·설명·샘플 데이터를 LLM에 입력 → 매핑 관계(동의어·의미적 동치) 자동 탐지 → Data Product Spec의 `semanticLinks` 필드 자동 생성
- **Atlan 사례**: Vector Embedding 기반 시맨틱 검색 — 팀 간 용어가 달라도 의미적으로 유사한 에셋 탐지

---

## 6. Tech Spec — 시장 솔루션/플레이어 맵

| 솔루션명 | 제공사 | 분류 | 핵심 기능 | AI 기능 | 비고 |
|---|---|---|---|---|---|
| **Atlan** | Atlan | 상용 (SaaS) | Active Metadata 기반 데이터 카탈로그, Data Product 발행·소비, 200+ 커넥터 자동 연동, 컬럼 레벨 리니지 | AI 자동 문서화, NL 검색(Vector Embedding), AI 에이전트 기반 메타데이터 80% 자동 완성, 자동 민감정보 분류 | 2025 Gartner Critical Capabilities Top 3. Data Mesh 기능 강화. [atlan.com](https://atlan.com) |
| **Collibra** | Collibra | 상용 (SaaS) | 카탈로그·거버넌스·프라이버시·품질 통합, 데이터 마켓플레이스, 사전 구축 통합 템플릿 | AI 기반 분류·태깅, 자동 리니지 감지 | 엔터프라이즈 거버넌스 선도. [collibra.com](https://www.collibra.com) |
| **Informatica IDMC** | Informatica | 상용 (클라우드) | 카탈로그·거버넌스·품질·MDM·개인정보 통합 플랫폼, Cloud Data Marketplace(큐레이션 Data Product 발행·소비), REST API 기반 주문·배달 워크플로우 | CLAIRE(AI 엔진) 기반 자동 분류·매핑·품질 추천, NLP 기반 검색 | Microsoft Fabric 디자인 파트너. [informatica.com](https://www.informatica.com/products/data-governance/cloud-data-marketplace.html) |
| **Microsoft Fabric + Purview** | Microsoft | 상용 (클라우드) | OneLake 기반 통합 분석 플랫폼, Purview Unified Catalog(Databricks·Snowflake·AWS 등 외부 소스 포함), Data Product 발행 워크플로우, GraphQL API | Copilot 기반 자연어 데이터 탐색, AI 자동 분류, DLP 정책 AI 추천 | Microsoft 생태계 내 강력한 통합. Fabric Sep 2025 업데이트 OneLake Catalog 보안 탭 추가. [microsoft.com](https://learn.microsoft.com/fabric) |
| **Databricks Unity Catalog + Marketplace** | Databricks | 상용 (클라우드/온프레미스) | Delta Lake 기반 Unity Catalog(RBAC 거버넌스), Delta Sharing(오픈 프로토콜로 비Databricks 소비자 지원), Iceberg REST Catalog API, AI/ML 피처·모델 버전 관리 | AI/BI 대시보드, Genie(NL→SQL), 자동 PII 탐지, 자동 리니지 | ML·AI 워크로드 강점. [databricks.com](https://www.databricks.com/product/unity-catalog) |
| **Snowflake Data Marketplace + Horizon** | Snowflake | 상용 (클라우드) | Data Marketplace(외부 데이터 상용 공유·구독), Horizon(거버넌스·보안·컴플라이언스), Data Clean Room(조직 간 데이터 결합) | Cortex AI(ML 내장 함수·LLM), Document AI, Cortex Search(벡터 검색) | 조직 간 라이브 데이터 공유 성숙. [snowflake.com](https://www.snowflake.com/en/data-cloud/marketplace/) |
| **Denodo** | Denodo | 상용 (온프레미스·클라우드) | Data Virtualization 플랫폼(150+ 커넥터, 25년 이상 경력), Semantic Layer, 연합 쿼리(ERP·MES·클라우드 통합), 데이터 마켓플레이스 | AI 기반 추천 뷰, NL 쿼리, 자동 데이터 분류 | 이질적 원천 추상화 계층 구현에 최적. [denodo.com](https://www.denodo.com) |
| **Starburst (Trino 기반)** | Starburst | 상용 + 오픈소스 | Trino 기반 Federated Query(오픈소스), 다중 이질적 원천 SQL 연합 쿼리, Starburst Galaxy(관리형 클라우드), Gravity(데이터 카탈로그) | AI 쿼리 최적화, NL→SQL | 고도 이질적 환경·SQL 분석 강점. [starburst.io](https://www.starburst.io) |
| **dbt (Semantic Layer + Mesh)** | dbt Labs | 오픈소스 + 상용 | dbt Mesh(다중 프로젝트 도메인 분리), dbt Semantic Layer(메트릭 정의 중앙화, GA), Python SDK, Tableau·Sheets 연동 GA | AI 기반 문서 자동 생성, SYNQ와 연동으로 Data Product 신뢰성 모니터링 | BI·분석 파이프라인 중심. [getdbt.com](https://www.getdbt.com) |
| **Nextdata OS** | Nextdata (창업자: Zhamak Dehghani) | 상용 (SaaS) | Data Mesh 창시자 개발 플랫폼. 자율(Autonomous) Data Product 컨테이너화, AI 에이전트 기반 기존 에셋에서 Data Product 자동 부트스트랩(수개월→수시간), 분산 도메인 팀 Self-Serve | AI 에이전트 기반 Data Product 자동 생성, 정책 자동 집행, 자기조직(Self-orchestrating) 파이프라인 | 2025년 4월 Nextdata OS 정식 출시. Greycroft·Acrew Capital 투자. [nextdata.com](https://www.nextdata.com) |
| **SYNQ** | SYNQ | 상용 (SaaS) | Data Product 신뢰성 플랫폼, SLA 추적·인시던트 관리, dbt/SQLMesh 기반 Data Product 모니터링, 소유권 운영화 | Scout(AI 품질 에이전트, 자기학습 이상탐지, 자동 해결), MCP(Model Context Protocol) 지원 | 2024년 Data Product 관찰가능성 핵심 기능 출시. [synq.io](https://www.synq.io) |
| **data.world** | data.world (ServiceNow 인수 추진, 2025.05) | 상용 (SaaS) | Knowledge Graph 기반 데이터 카탈로그, AI 보조 데이터 탐색(Archie Bots), LLM 기반 자동 큐레이션, 비SQL 사용자 지원 | GPT 기반 자동 큐레이션·검색, NL 기반 데이터 탐색, AI 분류 | ServiceNow Workflow Data Fabric과 통합 예정. [data.world](https://data.world) |
| **datacontract.com 툴링** | 오픈소스 커뮤니티 | 오픈소스 | Data Contract Spec(YAML) 정의·관리, Data Contract CLI(lint·test·import·export), ODCS v3 지원, Bitol/Linux Foundation 표준화 진행 중 | LLM 기반 Data Contract 자동 생성(Data Contract GPT) | 두 표준(ODCS·datacontract.com spec) 수렴 중. [datacontract.com](https://datacontract.com) |

---

## 7. 구축 시 고려사항 및 함정(Anti-Patterns)

### 7.1 Over-Engineering 함정

**증상**: 완벽한 아키텍처를 설계하느라 실제 Data Product 출시가 지연. 모든 플랫폼 기능을 선구축한 뒤 실제 소비자를 찾음.

**해결**: "Product Thinking First" — 소비자 유스케이스부터 역방향 설계. 파일럿 Data Product 2~3개를 먼저 출시하고 플랫폼은 수요에 맞춰 진화시킨다. Gartner(2025): "define phased implementation plans starting with core capabilities and expanding based on learning."

### 7.2 Governance 부재 함정

**증상**: 각 도메인이 자체 방식으로 Data Product를 만들어 마켓플레이스가 파편화. 5가지 다른 KPI 정의가 공존. 리니지 불명확.

**해결**: Federated Computational Governance — 정책을 코드로 정의(OPA, Policy-as-Code), 플랫폼이 자동 검증. Data Contract CLI로 스키마·품질 기준 자동 검사를 CI/CD에 통합.

### 7.3 Ownership 공백 함정

**증상**: Data Product Spec에 오너 이름은 있지만 실제 책임자가 없음("Ownership without accountability is noise"). 장애 시 담당자 불명확.

**해결**: Data Product Owner 역할을 구체적 책임(SLA 준수·품질 모니터링·소비자 지원)과 함께 공식화. RACI 매트릭스로 Owner(Accountable)·Steward(Responsible) 구분. 온콜(On-call) 로테이션 포함.

**Data Product Owner 핵심 책임** (Actian·Zeenea 참조):
- Data Product 비전·로드맵 정의
- 소비자 기대 수집 및 유스케이스 설명
- Data Product Spec YAML 최신 유지(메타데이터 정확성 보증)
- 거버넌스 정책 준수 확인 및 피드백 제공
- SLA·품질 KPI 추적 및 이상 시 대응
- 버전 업그레이드·Deprecation 공지

### 7.4 소비자 미고려 함정

**증상**: 도메인 팀이 자신의 관점에서 Data Product를 설계. 소비자가 실제로 필요한 스키마·갱신 주기·소비 방식과 불일치.

**해결**: Data Product Canvas의 "Consumer & Use Case" 블록을 가장 먼저 작성. 실제 소비자(분석가·AI 엔지니어·BI 개발자)가 설계 워크숍에 참여. 소비자 피드백 루프를 제품 백로그로 운영.

### 7.5 Decentralized Governance의 "Data Hydration Gap"

**증상**: 분산 거버넌스 하에서 도메인 팀이 즉각적인 분석 니즈에 최적화된 Data Product만 만들고, 범용 재사용 가능한 Data Product 투자를 기피. 그 비용·편익이 타 도메인으로 귀속되기 때문.

**해결**: 크로스 도메인 사용 Data Product에 대한 인센티브 구조(내부 가격 체계·기여 인정·중앙 플랫폼 팀 지원). 플랫폼 팀이 공통 기반 Data Product(마스터 데이터·공통 참조 데이터)를 직접 소유·운영.

### 7.6 정적 플랫폼 함정

**증상**: 데이터 플랫폼을 1회성 변환 프로젝트로 접근. 3~4년마다 대규모 재구축.

**해결**: Data Product 플랫폼 자체를 진화하는 제품(Evolving Product)으로 운영. 애자일 방식(Scrum·Kanban)으로 기능을 점진적으로 추가. 소비자 피드백을 플랫폼 백로그에 반영.

---

## 출처

1. datamesh-architecture.com — Data Product Canvas: https://www.datamesh-architecture.com/data-product-canvas
2. Open Data Product Specification (ODPS) 3.0 — Linux Foundation: https://opendataproducts.org/v3.0/
3. ODPS 최신버전 홈: https://opendataproducts.org/
4. ODPS 4.0 (개발버전): https://opendataproducts.org/dev/
5. Agile Lab Engineering — Data Product Specification: https://medium.com/agile-lab-engineering/data-product-specification-50610de8c152
6. GitHub Agile Lab Data Product Specification: https://github.com/agile-lab-dev/Data-Product-Specification
7. datacontract.com — Data Contract 완전 가이드: https://datacontract.com/
8. Data Contract Specification (datacontract.com): https://datacontract-specification.com/
9. GitHub datacontract-specification: https://github.com/datacontract/datacontract-specification
10. Bitol Open Data Contract Standard (ODCS v3): https://bitol-io.github.io/open-data-product-standard/v1.0.0/
11. NIIS — Unveiling ODPS: https://www.niis.org/blog/2024/1/21/unveiling-the-open-data-product-specification
12. Andrea Gioia — Data Contract vs Data Product Spec (Medium): https://medium.com/@andrea_gioia/data-contract-vs-data-product-specifications-8ffa3cc16725
13. Nextdata OS 출시 발표 (GlobeNewswire, 2025.04.08): https://www.globenewswire.com/news-release/2025/04/08/3057901/0/en/Founded-by-Zhamak-Dehghani-Nextdata-Launches-Nextdata-OS-to-Help-Enterprises-Automate-Data-Management-for-Agents-Analytics-and-Apps.html
14. Nextdata 공식 사이트: https://www.nextdata.com/
15. Nextdata OS — 자율 Data Product 소개: https://www.nextdata.com/our-pov/introducing-nextdata-os-autonomous-data-products-for-the-autonomous-era
16. SYNQ — 2024 Highlights: https://www.synq.io/blog/2024-wrap
17. SYNQ + dbt 신뢰할 수 있는 Data Product: https://www.getdbt.com/blog/building-reliable-data-products-with-dbt-cloud-and-synq
18. SYNQ Data Observability Guide: https://www.synq.io/blog/data-observability-guide
19. dbt Mesh GA 및 Semantic Layer — TechTarget: https://www.techtarget.com/searchbusinessanalytics/news/366556394/DBT-Labs-updates-Semantic-Layer-adds-data-mesh-enablement
20. dbt Data Products and Data Mesh: https://www.getdbt.com/blog/data-products-data-mesh
21. dbt Semantic Layer 공식 문서: https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl
22. Atlan Data Mesh Setup Guide 2025: https://atlan.com/data-mesh-set-up/
23. Atlan Data Mesh Principles 2025: https://atlan.com/data-mesh-principles/
24. Atlan Data Product Lifecycle: https://atlan.com/know/data-product-lifecycle/
25. Atlan Collibra 대안 비교: https://atlan.com/collibra-alternatives-enterprise-data-governance/
26. Informatica Cloud Data Marketplace 공식: https://www.informatica.com/products/data-governance/cloud-data-marketplace.html
27. Informatica Fall 2025 Release: https://www.informatica.com/about-us/news/news-releases/2025/10/20251029-informatica-announces-fall-2025-release-with-latest-innovations-to-intelligent-data-management-cloud.html
28. Microsoft Purview Innovations for Fabric (2025.09): https://www.microsoft.com/en-us/security/blog/2025/09/16/microsoft-purview-innovations-for-your-fabric-data-unify-data-security-and-governance-for-the-ai-era/
29. Microsoft Fabric OneLake Catalog vs Purview (Medium): https://medium.com/@marcoOesterlin/understanding-microsoft-fabric-onelake-catalog-vs-microsoft-purview-unified-catalog-e3db5674a10a
30. Databricks Unity Catalog vs Snowflake Horizon: https://www.celestinfo.com/unity-catalog-vs-snowflake-governance.html
31. Starburst — Data Catalog vs Databricks vs Snowflake: https://www.starburst.io/blog/data-catalog-databricks-snowflake/
32. Starburst Data Mesh Roles and Responsibilities: https://www.starburst.io/blog/data-mesh-roles-and-responsibilities/
33. Starburst Data Product Lifecycle: https://www.starburst.io/blog/data-product-lifecycle/
34. Denodo Data Virtualization Guide (K2view): https://www.k2view.com/what-is-data-virtualization/
35. Data Virtualization and Semantic Layer — DEV Community: https://dev.to/alexmercedcoder/data-virtualization-and-the-semantic-layer-query-without-copying-2jom
36. Data Virtualization Tools 비교 (Promethium): https://promethium.ai/guides/data-virtualization-tools-vendors/
37. data.world Data Catalog: https://data.world/product/
38. ServiceNow — data.world 인수 발표 (2025.05): https://newsroom.servicenow.com/press-releases/details/2025/ServiceNow-Enhances-Its-Workflow-Data-Fabric-With-New-Ecosystem-to-Power-AI-agents-and-Workflows-With-Real-Time-Intelligence/default.aspx
39. Nexocode Data Mesh Implementation Roadmap: https://nexocode.com/blog/posts/data-mesh-implementation-roadmap/
40. Promethium Data Mesh Implementation Guide: https://promethium.ai/guides/data-mesh-principles-implementation-guide/
41. AWS Prescriptive Guidance — Data Mesh Strategy: https://docs.aws.amazon.com/prescriptive-guidance/latest/strategy-data-mesh/data-mesh-strategy-framework.html
42. Google Cloud Data Mesh Architecture: https://docs.cloud.google.com/architecture/data-mesh
43. Data Product Owner 역할 (Actian): https://www.actian.com/blog/data-intelligence/what-is-a-data-product-owner-role-skills-responsibilities/
44. Data Product Owner 역할 (Zeenea): https://zeenea.com/what-is-a-data-product-owner-role-skills-responsibilities/
45. Data Mesh 역할·조직 (Starburst): https://www.starburst.io/blog/data-mesh-roles-and-responsibilities/
46. Data Mesh 역할·조직 (InTechHouse): https://intechhouse.com/blog/how-to-build-a-data-mesh-team-roles-and-responsibilities/
47. arXiv — AI-Driven Generation of Data Contracts (2025): https://arxiv.org/pdf/2507.21056
48. arXiv — Metadata Extraction Leveraging LLMs (2025): https://arxiv.org/pdf/2510.19334
49. arXiv — Exploring LLM for DCAT Metadata (2025): https://arxiv.org/pdf/2507.05282
50. USPTO — LLM-based recommender for data catalog (특허): https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12443607
51. biorXiv — Multi-agent AI for Metadata Curation (2025): https://www.biorxiv.org/content/10.1101/2025.06.10.658658.full.pdf
52. arXiv — Data Hydration Gap (2026): https://arxiv.org/pdf/2604.00218
53. Medium — Data Governance Pitfalls in Data Product Development: https://arushi19.medium.com/steering-clear-of-trouble-3-common-data-governance-pitfalls-in-data-product-development-to-avoid-31eca23ceff3
54. Medium — Most Common Anti-Patterns in Enterprise Data Platforms (2026): https://medium.com/@sportsworldalltypes/the-most-common-anti-patterns-in-enterprise-data-platforms-37d39a70fdf8
55. Acceldata — Data Governance at Scale: https://www.acceldata.io/blog/the-hidden-reason-enterprise-data-governance-breaks-under-ai
56. Alation — Data Product Lifecycle Stages: https://www.alation.com/blog/data-product-lifecycle/
57. OvalEdge — Data Product Strategy Guide: https://www.ovaledge.com/blog/data-product-strategy-guide
58. Modern Data 101 — Versioning, Cataloging, Decommissioning Data Products: https://www.moderndata101.com/blogs/versioning-cataloging-and-decommissioning-data-products
59. Hasura — GraphQL for Data Products: https://hasura.io/blog/graphql-for-data-products
60. Sanjeev Mohan — GraphQL for Data Products (Medium): https://medium.com/data-mesh-learning/graphql-for-data-products-d6d92407a885
61. Beefed.ai — How to Design Data Products with SLAs: https://beefed.ai/en/design-data-products-with-slas
62. Siemens Opcenter — MES and ERP Systems: https://blogs.sw.siemens.com/opcenter/manufacturing-execution-systems-mes-and-enterprise-resource-planning-erp-systems-how-they-relate/
63. Promethium — Data Mesh Tools & Platforms 2025: https://promethium.ai/guides/data-mesh-tools-vendors-platform-guide/
