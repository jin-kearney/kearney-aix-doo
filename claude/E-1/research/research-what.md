# E-1 Data Product화 (Data as a Product) — What 클러스터 리서치

> 담당 클러스터: **What(핵심)** — 정의·구성요소·표준/프레임워크·라이프사이클·유형/성숙도 모델·핵심 용어집
> 작성일: 2026-06-03

---

## 1. 정의 및 인접 개념과의 경계

### 1-1. Data Product / "Data as a Product" 정의

**Data as a Product(DaaP)** 는 철학(Mindset)이자 거버넌스 운영 모델이다. 데이터를 소비자 지향 소프트웨어 애플리케이션과 동일한 엄격함으로 설계·소유·유지관리하는 접근법이다. 이는 Data Mesh의 4대 원칙 중 하나로 Zhamak Dehghani가 2020년 martinfowler.com에 발표한 「Data Mesh Principles and Logical Architecture」에서 공식화했다.

**Data Product(데이터 프로덕트)** 는 DaaP 철학의 결과물로 탄생하는 실제 자산이다. 여러 정의를 종합하면:

- "도메인의 분석 데이터에 대한 접근을 제공하기 위해 세 가지 구조적 구성요소(코드·데이터 및 메타데이터·인프라)가 캡슐화된 노드" — Dehghani (martinfowler.com, 2020)
- "반복 재사용 가능하고, 능동적이며, 표준화된 데이터 자산으로서 내부·외부 사용자에게 측정 가능한 가치를 제공하는 것. 하나 이상의 데이터 아티팩트(데이터셋, 모델, 파이프라인)로 구성되며 메타데이터, 거버넌스 정책, 품질 규칙, 데이터 계약으로 보강된다." — entropy-data.com
- "도메인 데이터를 처리하고 분석 활용을 위한 데이터셋을 Output Port를 통해 제공하는 모든 구성요소를 포함하는 논리적 단위" — datamesh-architecture.com
- "자율적이고, 읽기에 최적화된, 표준화된 데이터 단위로서 적어도 하나의 데이터셋(Domain Dataset)을 포함하며 사용자 니즈를 충족시키기 위해 생성된다." — J. Majchrzak et al. (Data Mesh in Action, Manning)

### 1-2. 인접 개념과의 경계 (MECE 구분)

| 개념 | 핵심 특성 | Data Product와의 차이 |
|------|-----------|----------------------|
| **데이터셋(Dataset)** | 원시 또는 정제된 데이터 파일·테이블 | 소유자·SLA·소비 인터페이스가 없음. Product가 되려면 추가 거버넌스 레이어 필요 |
| **데이터 자산(Data Asset)** | 조직이 보유한 모든 데이터 | 반드시 소비자를 위해 패키지화·품질보증·재사용을 보장하지 않음 |
| **데이터 서비스(Data Service)** | 운영 데이터를 API로 노출하는 것 | 주로 최신 트랜잭션 데이터 제공 (operational plane). Data Product는 분석 데이터에 집중 |
| **Data as a Service (DaaS)** | 데이터 전달 모델 (API·구독 방식) | DaaS는 전달 메커니즘(배관)이고 DaaP는 설계·거버넌스 철학. 두 개념은 상호보완적 |
| **데이터 카탈로그(Data Catalog)** | 데이터 자산 검색·등록 | 카탈로그는 발견(Discovery)을 지원; Data Product는 소비자 경험·상품화에 집중 (E-1의 MECE 경계) |

**DaaS vs DaaP 핵심 구분**: DaaS는 스케일·접근성을 우선하고 데이터 흐름을 제공하며, DaaP는 신뢰·재사용·비즈니스 정렬을 우선한다. 많은 기업은 DaaS로 전달(Distribution)하고 DaaP로 품질·거버넌스·재사용을 보장하는 방식으로 두 가지를 병용한다. (출처: Alation, 2025)

**"Data as a Product" vs "Data Product" 구분**: "Data as a Product"는 상위 철학/접근법이고, "Data Product"는 그 결과물인 실제 자산이다. (출처: Alation, 2025)

---

## 2. Data Mesh에서의 Data Product 위치

### 2-1. Data Mesh 4대 원칙 (Dehghani, martinfowler.com 2020)

Data Mesh는 분석 데이터를 대규모로 관리하기 위한 패러다임으로, 4개 원칙이 집합적으로 필요충분하다:

| 원칙 | 목적 |
|------|------|
| **1. Domain-Oriented Decentralized Data Ownership (도메인 기반 분산 소유권)** | 데이터를 가장 잘 아는 도메인 팀이 분석 데이터를 소유·제공 |
| **2. Data as a Product (데이터의 Product화)** | 사용자가 분산된 여러 도메인의 고품질 데이터를 쉽게 발견·이해·사용 |
| **3. Self-Serve Data Infrastructure as a Platform (자가서비스 데이터 플랫폼)** | 도메인 팀이 플랫폼 추상화를 활용해 Data Product를 자율적으로 생성·소비 |
| **4. Federated Computational Governance (연합적 거버넌스)** | 독립 Data Product들이 글로벌 표준 준수를 통해 상호 운용 가능 |

4개 원칙의 부분 구현(예: 도메인 소유권 도입 후 자가서비스 플랫폼 없이)은 일반적으로 실패한다. (출처: datamesh-architecture.com, dbt Labs)

### 2-2. Data Product as Architectural Quantum

Dehghani는 Data Product를 **"아키텍처 퀀텀(Architectural Quantum)"** 으로 정의했다. 이는 Thoughtworks의 『Building Evolutionary Architectures』에서 차용한 개념으로, **"독립적으로 배포 가능한 가장 작은 아키텍처 단위이며, 높은 기능적 응집도를 가지고 자신의 기능에 필요한 모든 구조적 요소를 포함한다"** 는 의미다.

Data Product는 Data Mesh에서 그래프(메시)의 노드 역할을 하며, 각 노드는 Bounded Context 내에 존재하고 하나의 Bounded Context에 여러 Data Product가 있을 수 있다.

기존 패러다임과의 차이: 과거에는 파이프라인(코드)이 생산하는 데이터와 독립적으로 관리되고, 인프라(웨어하우스·레이크)는 많은 데이터셋이 공유했다. Data Product는 코드·데이터·인프라 모두를 도메인의 Bounded Context 수준에서 하나로 구성한다. (출처: martinfowler.com)

---

## 3. Data Product의 핵심 구성요소

### 3-1. Dehghani의 3대 구조적 구성요소 (martinfowler.com, 2020)

**① Code (코드)**
- 업스트림 데이터를 소비·변환·제공하는 데이터 파이프라인 코드
- 데이터·시맨틱·구문 스키마·관찰성 지표 및 기타 메타데이터에 대한 접근을 제공하는 API 코드
- 접근 제어 정책·컴플라이언스·출처 등의 특성을 강제하는 코드

**② Data and Metadata (데이터 및 메타데이터)**
- 도메인의 분석·이력 데이터 (다중 언어 형식: 이벤트, 배치 파일, 관계형 테이블, 그래프 등)
- 데이터 계산 문서화, 시맨틱·구문 선언, 품질 지표 등 내재적 메타데이터
- 연합적 거버넌스가 요구하는 접근 제어 정책 등 행동 메타데이터

**③ Infrastructure (인프라)**
- Data Product 코드의 빌드·배포·실행을 지원
- 빅데이터 및 메타데이터의 스토리지와 접근

### 3-2. Input Port와 Output Port

Data Product는 외부와 표준화된 포트를 통해 소통한다:

- **Input Port**: 내부 변환을 위한 소스 데이터를 수집. 운영 소스 시스템 또는 다른 Data Product로부터 데이터를 수신하는 메커니즘. 데이터를 읽는 형식과 프로토콜 정의
- **Output Port**: 생성된 데이터를 신뢰성 있게 공유. 외부 소비를 위해 데이터를 명시적으로 게시. 형식 및 소비 프로토콜 정의

Output Port 유형 (예시): 데이터베이스 테이블, 파일(CSV, Parquet 등), REST API, 시각화/대시보드, Kafka 토픽(스트리밍), GraphQL, Feature Store

**Data Product Descriptor Specification(DPSD, Agile Lab 오픈소스)의 5가지 포트 유형:**
1. Input Ports — 데이터 수신
2. Output Ports — 데이터 노출
3. Discovery Ports — 아키텍처에서의 정적 역할 정보 제공
4. Observability Ports — 동적 행동에 대한 통찰 제공
5. Control Ports — 로컬 정책/거버넌스 운영 관리

(출처: dpds.opendatamesh.org, datamesh-architecture.com)

### 3-3. Data Contract (데이터 계약)

데이터 계약은 데이터 생산자(Producer)와 소비자(Consumer) 간의 합의를 정의한다. 포함 내용:

- **스키마(Schema)**: 데이터의 구조, 유형, 필드, 기본값, 제약조건, 규칙의 공식적 선언
- **시맨틱**: 데이터의 의미(Logical Model) 설명
- **SLA/SLO**: 품질, 가용성, 신선도 등에 대한 서비스 수준 약속
- **이용 약관(Terms & Conditions)**: 데이터 사용 방법, 제한사항, 이용 허가 조건
- **데이터 공유 계약(Data Sharing Agreement)**: 이용 목적, 프라이버시, 제한 사항

표준화: 업계 표준은 ODCS(Open Data Contract Standard)로, PayPal의 Data Contract Template에서 출발해 현재 Linux Foundation의 Bitol 프로젝트가 관리한다. 현재 버전은 v3.1.0(2024 이후). datacontract.com은 2024년 ODCS v3.1에 맞춰 자체 Data Contract Specification을 폐기하고 ODCS로 통합했다. (출처: datacontract.com, bitol-io/open-data-contract-standard GitHub)

### 3-4. SLA / SLO / SLI 개념 구분

| 개념 | 정의 | 예시 |
|------|------|------|
| **SLI (Service Level Indicator)** | 측정 지표 자체 | 데이터 신선도 지연 시간(초) |
| **SLO (Service Level Objective)** | 내부 목표치 | 신선도 95%ile < 1시간 |
| **SLA (Service Level Agreement)** | 소비자와의 명시적 약속 (위반 시 결과 수반) | "24시간 이내 데이터 갱신 보장" |

핵심 SLA 차원 (ODPS 4.1 기준, 11개 표준 차원):
- **Freshness**: 비즈니스 사실 발생~데이터 가시화까지의 지연 시간
- **Completeness**: 데이터 존재율
- **Accuracy**: 허용 오차율
- **Availability (UpTime)**: 포트 가용성 비율
- **IntervalOfChange**: 변경이 반영되는 빈도
- **Timeliness**: 소스 업데이트~포트 반영까지의 지연

(출처: opendataproducts.org/v4.1, getdbt.com, bigeye.com)

### 3-5. 정책 및 보안

Data Product는 다음을 내재화한다:
- 접근 제어 정책 (Access Control)
- 컴플라이언스 규칙 (Compliance)
- 데이터 출처(Provenance) 추적
- 암호화 요구사항
- 개인정보 및 데이터 분류(Classification)

---

## 4. 표준 및 프레임워크

### 4-1. Open Data Product Specification (ODPS)

**주관**: Linux Foundation (2024년 5월 14일 이관)
**공식 사이트**: https://opendataproducts.org/
**최신 버전**: v4.1 (2025년 10월 24일)

ODPS는 벤더 중립적, 오픈소스, 기계 가독(YAML) 데이터 프로덕트 메타데이터 모델이다. 데이터 프로덕트를 일관되고 상호 운용 가능한 방식으로 설명하는 것이 목적이다.

**ODPS 4.1 주요 구성 객체:**
- **Data Product Details**: 기본 정보 (id, 이름, 도메인, 상태, 소유자, 버전 등)
- **Data Product Strategy** (신규, v4.1): Data Product를 비즈니스 목표·KPI와 직접 연결하는 객체. ODPS 최초로 Data Product와 비즈니스 성과 간 연결을 공식화
- **Data Contract**: 스키마, 이용 조건, 가격 등 (ODCS와 연동 가능)
- **Data SLA**: 11개 표준 차원으로 SLA 정의
- **Data Quality**: 8개 표준 옵션으로 품질 정의·측정
- **Data Pricing Plans**: 12개 표준 가격 모델 지원
- **Data Licensing**: 라이선스 정보
- **Data Access**: 접근 방법
- **Data Holder**: 소유권·책임 정보
- **Data Product Catalog**: 카탈로그 수준의 자동화 지원

**도입 사례**: NATO, BASF, Migros, Alation, FIWARE 등

(출처: opendataproducts.org/v4.1, niis.org, codecentric.de)

### 4-2. Open Data Contract Standard (ODCS)

- **주관**: Bitol (Linux Foundation AI & Data 프로젝트)
- **GitHub**: bitol-io/open-data-contract-standard
- **현재 버전**: v3.1.0
- **형식**: YAML
- **기원**: PayPal의 Data Contract Template → Bitol로 이관

포함 항목: fundamentals, datasets, schemas, data quality, pricing, stakeholders, roles, SLAs, other properties.

2024년 ODCS v3.0.0이 데이터 품질 지원 강화와 함께 출시. 이후 datacontract.com이 자체 스펙을 폐기하고 ODCS 단일 표준으로 통합. ODPS 4.1은 ODCS를 참조 확장으로 지원.

(출처: datacontract.com, bitol-io.github.io)

### 4-3. Data Product Specification (datamesh-architecture.com)

INNOQ의 오픈소스 사양(버전 0.0.1). YAML 기반. Data Mesh Manager와 호환.

핵심 필드: `dataProductSpecification`, `id`, `info` (title, description, status, archetype, owner, domain), `inputPorts`, `outputPorts` (각 포트에 id, name, type, status, location, dataContractId, containsPii, links 등 포함).

`archetype` 값: `source-aligned`, `aggregate`, `consumer-aligned` (Data Product 유형 분류와 직접 연결)

(출처: dataproduct-specification.com)

### 4-4. Data Mesh 4원칙과 자가서비스 플랫폼(Self-Serve Platform) 추상화

계열사별 이질적 솔루션 환경(예: 일부는 BigQuery, 일부는 Databricks, 일부는 SAP)에서 표준 Data Product를 제공하려면 **Self-Serve Data Platform**이 핵심 역할을 한다. Dehghani는 3개 플레인(Plane)을 제시했다:

1. **Data Infrastructure Provisioning Plane**: 분산 파일 스토리지, 접근 제어, 오케스트레이션 등 하위 인프라 프로비저닝
2. **Data Product Developer Experience Plane**: 도메인 팀이 일반 개발자 수준에서 Data Product 생성·유지·실행 가능하도록 복잡성 추상화
3. **Data Mesh Supervision Plane**: 메시 수준에서 Data Product 발견, 상관 분석, 시맨틱 쿼리 실행 등 제공

이 추상화 계층이 존재해야 도메인 팀이 특정 플랫폼(BigQuery인지 Snowflake인지)에 의존하지 않고 표준 Data Product를 게시할 수 있다. (출처: martinfowler.com)

---

## 5. Data Product 라이프사이클

### 5-1. 주요 단계 (복수 프레임워크 통합)

여러 기관이 제시한 단계를 통합하면 다음과 같다:

| 단계 | 주요 활동 | 비고 |
|------|-----------|------|
| **① 아이디에이션(Ideate)** | 비즈니스 가치·유즈케이스 정의, 소비자 니즈 파악, Data Product 후보 선정 | "비즈니스 가치에서 출발" 원칙 |
| **② 설계(Design)** | Data Contract 작성, 명세서 작성, 거버넌스 체크포인트 내재화, Output Port 유형 결정, 마켓플레이스 등록 준비 | Data Product Canvas 활용 가능 |
| **③ 개발(Build)** | 데이터 파이프라인·변환 로직 구현, 품질 검사 자동화, 접근 제어 구현 | DataOps·CI/CD 적용 |
| **④ 배포(Deploy/Publish)** | Output Port 게시, Data Catalog 등록, 소비자 접근 활성화 | |
| **⑤ 운영 및 관리(Operate & Govern)** | SLA 모니터링, 품질 검사, 소비자 피드백 수집, 이슈 대응 | 관찰성(Observability) 포트 활용 |
| **⑥ 버전관리(Version)** | 스키마 변경 관리, 하위 호환성 유지, 소비자 이행 지원 | Semantic Versioning 적용 |
| **⑦ 진화(Evolve/Iterate)** | 소비자 피드백 기반 개선, 신규 Output Port 추가, 유즈케이스 확장 | |
| **⑧ 폐기(Retire/Deprecate)** | 소비 분석 후 더 이상 가치 없는 Product 단계적 폐기, 소비자 마이그레이션 지원 | |

- Databricks: Inception → Design → Creation → Publish → Operate & Govern → Consume → Retire (7단계)
- Snowflake: Discovery → Design → Development → Deployment (4단계)
- Alation: Ideate → Design → Operationalize → Evolve (4단계)
- KPMG도 Data Product Lifecycle을 별도 프레임워크로 발표 (2025)

(출처: alation.com/blog/data-product-lifecycle, starburst.io/blog/data-product-lifecycle, snowflake.com)

### 5-2. 라이프사이클 운영 핵심 원칙

- **거버넌스 체크포인트 내재화**: 각 단계마다 거버넌스 검사 포인트 삽입 (사후가 아닌 내재)
- **DataOps·CI/CD**: 신뢰성·효율성·확장성 보장을 위한 자동화
- **품질의 Shift-Left**: 품질 검사를 데이터 소스 가까이에서 수행하여 책임을 개발팀에 귀속
- **AI 지원 유지보수**: 메타데이터 업데이트, 품질 이상 탐지, 폐기 후보 자동 식별
- Gartner 조사: 2025년 기준 조직의 50%가 이미 Data Product를 배포했고 29%가 검토 중

---

## 6. Data Product 유형 및 분류

### 6-1. 3대 유형 (Data Mesh Architecture 표준 분류)

**① Source-Aligned Data Product (소스 정렬형)**
- 운영 시스템에서 원시 데이터를 최소한의 변환으로 캡슐화
- 한 비즈니스 도메인 내에서 생성·소유
- 목적: 소스를 신뢰 가능하게 만들기 (Make sources reliable)
- 예: 주문 시스템의 원시 주문 이벤트, ERP의 생산 실적 데이터

**② Aggregate Data Product (집계형)**
- 여러 Source-Aligned 또는 Consumer-Aligned 제품에서 데이터를 소비·통합
- 설계 초기에 계획하는 것이 아니라, 여러 Consumer 제품에서 중복 로직이 반복될 때 자연스럽게 등장 ("Aggregates reveal themselves")
- 목적: 중복 제거, 안정적·거버닝된 중간 레이어 제공
- 예: 고객 360 뷰(Customer 360), 공통 KPI 집계 테이블

**③ Consumer-Aligned Data Product (소비자 정렬형)**
- 특정 소비자·의사결정·프로세스를 위해 최적화
- 하나의 의사결정/프로세스를 위해 특정 대상에 맞게 구성
- 직접 소스 시스템에서 소싱하지 않고 다른 Data Product를 소비
- 목적: 비즈니스 결과 실현 (Deliver business outcomes)
- 예: 이탈 예측 데이터셋(ML용), 영업 대시보드, RAG 소스용 제품 문서 임베딩

분류는 Data Product Specification의 `archetype` 필드에서 `source-aligned`, `aggregate`, `consumer-aligned` 값으로 명세화한다.

(출처: datamesh-architecture.com, themoderndatacompany.com, dataproduct-specification.com)

### 6-2. Output Port 소비 방식 분류

| 소비 방식 | 형태 | 적합한 소비자 |
|-----------|------|---------------|
| **SQL/테이블** | DB 테이블, View, Glue Catalog, BigQuery 테이블 | 데이터 분석가, BI 도구 |
| **파일(File)** | CSV, Parquet, ORC (S3, ADLS, GCS) | 데이터 엔지니어, 배치 처리 |
| **API** | REST API, GraphQL | 애플리케이션, 마이크로서비스 |
| **스트리밍(Streaming)** | Kafka 토픽, Pub/Sub | 실시간 처리, 이벤트 기반 시스템 |
| **대시보드(Dashboard)** | BI 보고서, 시각화 | 비즈니스 사용자 |
| **RAG 소스** | 벡터 임베딩, 문서 청크 | LLM 기반 AI 애플리케이션 |
| **Feature Store** | ML 피처(Feature) | ML 모델 학습·서빙 |
| **분석 패키지** | 사전 집계 데이터, OLAP Cube | 고급 분석, 자가서비스 분석 |

(출처: datamesh-architecture.com, confluent.io/blog/implementing-streaming-data-products)

### 6-3. Data Product 성숙도 모델

**ZS Associates의 10개 성숙도 차원 프레임워크** (Shri Salem, Medium 2023):

| 차원 | 설명 |
|------|------|
| **Awareness (인지)** | 잠재 사용자가 Data Product의 존재와 용도를 아는 정도 |
| **Usability (사용성)** | 발견·접근·사용이 쉬운 정도; 자가서비스 즉시 접근 |
| **Interoperability (상호운용성)** | 다른 데이터, AI/분석/시각화 역량과 결합 가능한 정도 |
| **Actionability (실행가능성)** | 명확히 정의된 유즈케이스와 직접 연결된 정도 |
| **Speed (속도)** | 소비자가 빠르게 발견·접근·이해·사용할 수 있는 정도 |
| **Innovation (혁신성)** | 소비자가 실험하고 데이터 사이언스를 적용할 수 있는 정도 |
| **Trust (신뢰)** | 신뢰성·보안·품질 통제 수준 |
| **Adoption (채택)** | 주요 도메인·프로세스에서의 실제 사용 및 성공 사례 |
| **Business Impact (비즈니스 영향)** | 명시적이고 정량화된 비즈니스 영향 |
| **Product Orientation (제품 지향성)** | 고객 중심, 반복적 라이프사이클 접근 방식으로 관리 여부 |

핵심 통찰: 이 차원들은 "곱셈" 관계다. 하나의 차원만 낮아도 전체 영향이 억제된다.

**단계별 성숙도 평가**: 조직은 Data Product 포트폴리오를 "잠재 영향 vs 성숙도 격차" 2x2 매트릭스에 매핑하여 투자 우선순위를 결정한다.

(출처: medium.com/@salemshriram)

---

## 7. Data Product의 특성: DATSIS와 FAIR 원칙

### 7-1. Dehghani의 DATSIS (Data Mesh 원칙)

Dehghani는 Data Product가 갖춰야 할 특성을 열거했다:
- **D**iscoverable (발견 가능): 소비자가 쉽게 찾을 수 있어야 함
- **A**ddressable (주소 지정 가능): 고유하고 영속적인 식별자를 가져야 함
- **T**rustworthy (신뢰 가능): 품질 보장과 SLO를 통해 신뢰할 수 있어야 함
- **S**elf-describing (자기 설명적): 메타데이터와 문서로 스스로를 설명해야 함
- **I**nter-operable (상호 운용 가능): 글로벌 표준을 준수하여 조합 가능해야 함
- **S**ecure (보안): 적절한 접근 통제와 보안 정책 내재화

*Note: 원문에서는 위 특성들이 열거되나 "DATSIS"라는 약어는 일부 해석에서 파생됨. Dehghani 원문(martinfowler.com)에서는 "discoverability, security, explorability, understandability, trustworthiness"로 표현됨.*

### 7-2. FAIR 데이터 원칙 적용

FAIR 원칙(Wilkinson et al., 2016, Scientific Data)은 과학 데이터 관리를 위해 고안되었으나 Data Product 설계에 광범위하게 적용된다:

| 원칙 | 의미 | Data Product 적용 |
|------|------|------------------|
| **F**indable (발견 가능) | 전역적으로 고유하고 영속적인 식별자, 풍부한 메타데이터 | Output Port URL, 데이터 카탈로그 등록 |
| **A**ccessible (접근 가능) | 표준 통신 프로토콜로 접근 | REST API, SQL 엔드포인트 등 표준 Output Port |
| **I**nteroperable (상호 운용 가능) | 공식적·공유된 지식 표현 언어 사용 | 글로벌 식별자, 표준 시맨틱 레이어 |
| **R**eusable (재사용 가능) | 명확한 이용 허가 및 출처 정보 | Data Contract의 Terms & Conditions, 데이터 리니지 |

(출처: go-fair.org/fair-principles, nature.com/articles/sdata201618)

---

## 8. Data Product Owner 역할

### 8-1. 정의 및 책임

Data Product Owner (또는 Data Product Manager/DPM)는 조직 내 Data Product의 가치 극대화에 궁극적으로 책임지는 역할이다. 도메인 팀 내에서 Data Product의 비전·로드맵·개발을 이끈다.

**주요 책임:**

| 책임 영역 | 상세 내용 |
|-----------|-----------|
| **비전 정의** | Data Product의 목적, 사용자, 기대치를 제품 사고(Product Thinking) 관점에서 정의 |
| **전략적 로드맵** | 개발 로드맵 작성 및 KPI 정의 |
| **품질 보장** | 메타데이터 설명 제공, 표준·거버넌스 규칙 준수 확인 |
| **소비자 만족** | 데이터 사용자가 누구인지, 어떻게 사용하는지 이해하고 인터페이스 설계 |
| **백로그 관리** | Data Product 백로그 관리 및 우선순위 결정 |
| **스키마 변경 의사결정** | 스키마 변경, 품질 이슈 대응, SLA 위반 시 소비자 소통 |

**책임 구조의 역전**: 기존 패러다임에서는 중앙 데이터 팀이 품질 책임을 가졌다. Data Mesh에서는 데이터 소스에 가장 가까운 도메인 Data Product Owner가 품질 책임을 진다.

(출처: equalexperts.com, zeenea.com, actian.com)

### 8-2. Data Product Developer 역할

Dehghani는 도메인 팀 내에 **Data Product Developer** 역할을 별도로 정의했다. Data Product Developer는 Domain Data Product Owner와 함께 도메인 Data Product를 빌드·유지·제공한다.

---

## 9. Data Product KPI 및 측정

### 9-1. 기술적 KPI (SLA 관련 지표)

- **데이터 신선도(Freshness)**: 이벤트 발생~데이터 가시화까지의 지연 시간 (dataset_freshness_seconds)
- **가용성(Availability)**: 포트 업타임 비율 (dataset_availability_ratio)
- **완전성(Completeness)**: pct_null_customer_id 등 필수 필드 결측률
- **정확성(Accuracy)**: 허용 오차 내 데이터 비율
- **처리 성능(Performance)**: 쿼리 응답 시간, 파이프라인 처리 시간
- **오류율(Error Rate)**: 파이프라인 실패율, 데이터 검증 실패율

SLA 체크 주기는 SLA 요건에 맞춰야 함: 예) 1시간 SLA → 30분 간격 모니터링.

### 9-2. 비즈니스 KPI (Product 관련 지표)

- **채택률(Adoption Rate)**: Data Product 구독·활성 사용자 수
- **Net Promoter Score (NPS)**: 데이터 사용자 만족도 (Dehghani가 직접 언급)
- **데이터 소비 리드 타임**: 소비자가 Data Product를 발견~사용까지 걸리는 시간
- **재사용률(Reuse Rate)**: 동일 Data Product를 여러 유즈케이스에 활용하는 비율
- **비즈니스 영향**: 해당 Data Product가 기여한 매출 개선, 비용 절감, 의사결정 속도 향상
- **중복 감소**: Data Product 도입 후 중복 데이터셋·파이프라인 수 감소

ODPS 4.1의 `productStrategy` 객체는 Data Product를 비즈니스 목표·KPI와 직접 연결하는 최초의 오픈 표준 구조다.

(출처: martinfowler.com, getdbt.com, bigeye.com, opendataproducts.org/v4.1)

---

## 10. Data Product 후보 선정 기준

비즈니스 도메인이 생성·소유하는 데이터로서 다음 조건을 충족할 때 Data Product화 우선 후보가 된다:

| 선정 기준 | 설명 |
|-----------|------|
| **복수 소비자 존재** | 여러 팀·도메인이 동일 데이터를 필요로 하는 경우 |
| **반복 재사용** | 일회성이 아니라 지속적으로 사용되는 데이터 |
| **크로스 도메인 활용** | 도메인 경계를 넘어 다른 도메인에서도 필요한 데이터 |
| **AI/ML 입력 데이터** | ML 모델 학습이나 AI 애플리케이션의 신뢰 가능한 입력으로 필요한 데이터 |
| **규제·컴플라이언스 데이터** | 정확성과 출처 추적이 필수인 데이터 (재무, 법무, 품질 데이터 등) |
| **현재 중복이 많은 데이터** | 여러 팀이 각자 파이프라인을 구축하여 중복하는 데이터 영역 |
| **비즈니스 임계 데이터 요소(CDE)** | 조직의 핵심 의사결정에 직접 영향을 미치는 데이터 |

가치·노력 2x2 매트릭스를 활용해 우선순위를 결정하며, "고잠재 영향 + 낮은 현재 성숙도" Data Product를 먼저 투자한다.

(출처: datamesh-architecture.com, alation.com, snowflake.com/blog/building-data-products-data-economy)

---

## 11. Data Product Canvas (설계 도구)

datamesh-architecture.com(INNOQ)이 개발한 오픈소스 시각적 설계 도구. CC BY 4.0 라이선스. 8개 빌딩 블록으로 구성:

1. **Domain**: 책임 도메인 (Who owns it? Who fixes it when it breaks?)
2. **Data Product Name**: 고유 식별 이름
3. **Consumer and Use Case(s)**: 존재 이유 - 소비자와 분석 유즈케이스
4. **Data Contract**: Output Port 형식·소비 프로토콜 정의, Data Contract 명세
5. **Sources**: Input Port - 운영 소스 시스템 또는 다른 Data Product
6. **Data Product Architecture**: Input~Output Port 사이의 내부 설계 (수집·저장·변환·보강·분석 등)
7. **Ubiquitous Language**: 공통 도메인 용어집 (DDD에서 차용)
8. **Classification**: source-aligned / aggregate / consumer-aligned 분류

Miro 템플릿 또는 PDF(DIN A0)로 다운로드 가능.

(출처: datamesh-architecture.com/data-product-canvas)

---

## 12. 핵심 용어집

| 용어 (한국어) | 영문 | 정의 |
|--------------|------|------|
| **데이터 프로덕트** | Data Product | 도메인의 분석 데이터에 접근을 제공하는 코드·데이터·인프라가 캡슐화된 독립 배포 가능 단위. 소비자에게 신뢰·재사용 가능한 형태로 제공 |
| **데이터 프로덕트 오너** | Data Product Owner / Data Product Manager | Data Product의 가치 극대화에 최종 책임을 지는 역할. 비전·로드맵·품질·소비자 만족을 관장 |
| **아웃풋 포트** | Output Port | Data Product가 외부 소비자에게 데이터를 제공하는 표준화된 인터페이스. DB 테이블, 파일, API, 스트림 등 다양한 형태 |
| **데이터 계약** | Data Contract | 데이터 생산자와 소비자 간의 공식 합의. 스키마, SLO, 이용 조건, 품질 기준을 포함. ODCS(YAML)로 표준화 |
| **SLA / SLO** | Service Level Agreement / Service Level Objective | SLO는 내부 목표치(예: 신선도 95%ile < 1시간), SLA는 소비자와의 명시적 약속(위반 시 결과 수반) |
| **데이터 메시** | Data Mesh | 분석 데이터의 분산 소유·제공을 위한 아키텍처·운영 모델. 4대 원칙(도메인 소유권, Data as a Product, Self-Serve Platform, 연합 거버넌스)으로 구성 |
| **자가서비스 플랫폼** | Self-Serve Data Platform | 도메인 팀이 Data Product를 독립적으로 빌드·배포·운영할 수 있게 하는 인프라 추상화 계층. 3개 플레인(프로비저닝·개발자 경험·메시 감독)으로 구성 |
| **데이터 마켓플레이스** | Data Marketplace | 조직 내부 소비자가 Data Product를 발견·구독·피드백하는 자가서비스 포털. 95%의 조직이 운영 중이거나 계획 중 (erwin 2024 조사) |
| **시맨틱 레이어** | Semantic Layer | Data Product 메시 위에 통합 메트릭·비즈니스 로직 레이어를 제공. 비즈니스 사용자에게 데이터 위치를 추상화하여 일관된 뷰 제공 |
| **아키텍처 퀀텀** | Architectural Quantum | 독립적으로 배포 가능한 가장 작은 아키텍처 단위. Data Mesh에서 Data Product가 이 역할. 코드·데이터·인프라를 모두 포함 |
| **ODPS** | Open Data Product Specification | Linux Foundation이 주관하는 벤더 중립 데이터 프로덕트 메타데이터 YAML 표준. 현재 v4.1. SLA·품질·가격·전략 등 포괄 |
| **ODCS** | Open Data Contract Standard | Linux Foundation Bitol 프로젝트의 데이터 계약 YAML 표준. PayPal에서 기원, 현재 v3.1.0. 업계 사실상 표준 |
| **페더레이티드 거버넌스** | Federated Computational Governance | 도메인 자율성을 유지하면서 글로벌 표준을 플랫폼이 자동 적용하는 거버넌스 모델. 중앙팀 대신 도메인 대표자 연합이 정책 결정 |

---

## 출처

| # | 출처 | URL |
|---|------|-----|
| 1 | Zhamak Dehghani - Data Mesh Principles and Logical Architecture (martinfowler.com, 2020) | https://martinfowler.com/articles/data-mesh-principles.html |
| 2 | datamesh-architecture.com - Designing Data Products / Data Product Canvas | https://www.datamesh-architecture.com/data-product-canvas |
| 3 | datamesh-architecture.com - Data Mesh Architecture Overview | https://www.datamesh-architecture.com/ |
| 4 | Open Data Product Specification v4.1 (Linux Foundation) | https://opendataproducts.org/v4.1/ |
| 5 | Open Data Product Specification - Home | https://opendataproducts.org/ |
| 6 | NIIS - Unveiling the Open Data Product Specification (ODPS) | https://www.niis.org/blog/2024/1/21/unveiling-the-open-data-product-specification |
| 7 | codecentric - ODPS: The Standard for Data Products | https://www.codecentric.de/en/knowledge-hub/blog/odps-the-standard-for-data-products |
| 8 | Open Data Contract Standard (ODCS) v3.1.0 (Bitol/Linux Foundation) | https://bitol-io.github.io/open-data-contract-standard/v3.1.0/ |
| 9 | bitol-io/open-data-contract-standard (GitHub) | https://github.com/bitol-io/open-data-contract-standard |
| 10 | datacontract.com - Complete Guide to Data Contracts | https://datacontract.com/ |
| 11 | Data Product Specification (datamesh-architecture.com / INNOQ) | https://dataproduct-specification.com/ |
| 12 | Alation - Data as a Product vs Data as a Service (2025) | https://www.alation.com/blog/data-as-a-product-vs-data-as-a-service/ |
| 13 | Alation - Stages of the Data Product Lifecycle (2025) | https://www.alation.com/blog/data-product-lifecycle/ |
| 14 | Alation - What Are the 5 Types of Data Products? | https://www.alation.com/blog/data-product-types/ |
| 15 | Data Product Descriptor Specification (DPSD) - Open Data Mesh (Agile Lab) | https://dpds.opendatamesh.org/concepts/data-product-descriptor/ |
| 16 | Entropy Data - What is a Data Product? | https://www.entropy-data.com/learn/what-is-a-data-product |
| 17 | Data Mesh Manager - What is a Data Product? | https://www.datamesh-manager.com/learn/what-is-a-data-product |
| 18 | Shri Salem (ZS Associates) - A framework to assess and enhance the maturity of data products (Medium, 2023) | https://medium.com/@salemshriram/a-framework-to-assess-and-enhance-the-maturity-of-data-products-64bc412a20fc |
| 19 | The Modern Data Company - How to Organize Your Data Product Ecosystem | https://www.themoderndatacompany.com/blog/how-to-organize-your-data-product-ecosystem |
| 20 | FAIR Principles - GO FAIR | https://www.go-fair.org/fair-principles/ |
| 21 | FAIR Data Principles original paper (Wilkinson et al., Scientific Data, 2016) | https://www.nature.com/articles/sdata201618 |
| 22 | dbt Labs - How to ensure data product SLAs and SLOs | https://www.getdbt.com/blog/data-product-slas-and-slos |
| 23 | Bigeye - The complete guide to understanding data SLAs | https://www.bigeye.com/blog/the-complete-guide-to-understanding-data-slas |
| 24 | Confluent - Implementing Streaming Data Products | https://www.confluent.io/blog/implementing-streaming-data-products/ |
| 25 | Equal Experts - What is a Data Product Owner? | https://www.equalexperts.com/blog/data/what-is-a-data-product-owner-responsibilities-challenges-and-best-practices/ |
| 26 | Starburst - Data as a Product / Data Mesh | https://www.starburst.io/blog/data-as-a-product/ |
| 27 | Thoughtworks - Core Principles of Data Mesh / Data as a Product | https://www.thoughtworks.com/about-us/events/webinars/core-principles-of-data-mesh/data-as-a-product |
| 28 | dbt Labs - The Four Principles of Data Mesh | https://www.getdbt.com/blog/the-four-principles-of-data-mesh |
| 29 | Dawiso - What Is Data Mesh? The Four Principles Explained | https://www.dawiso.com/glossary/data-mesh |
| 30 | Atlan - Data Product Lifecycle | https://atlan.com/know/data-product-lifecycle/ |
| 31 | Starburst - Data Product Lifecycle (8 stages) | https://www.starburst.io/blog/data-product-lifecycle/ |
| 32 | Snowflake - Understand the 4 Stages of the Data Product Lifecycle | https://www.snowflake.com/en/blog/understand-data-product-lifestyle-stages/ |
| 33 | KPMG - The Data Product Lifecycle (2025) | https://kpmg.com/us/en/articles/2025/data-product-lifecycle.html |
| 34 | When Standards Collide: Clarifying ODPS and ODCS (Dr. Jarkko Moilanen, 2025) | https://medium.com/open-data-product-specification/when-standards-collide-clarifying-odps-and-odcs-in-the-data-product-landscape-c2978f9c13d9 |
| 35 | Open Data Mesh Wikipedia | https://en.wikipedia.org/wiki/Data_mesh |
| 36 | Agile Lab - Data Product Specification GitHub | https://github.com/agile-lab-dev/Data-Product-Specification |
| 37 | Zeenea - Everything you need to know about Data Products | https://zeenea.com/everything-you-need-to-know-about-data-products/ |
| 38 | Snowflake - Building Data Products in the Data Economy | https://www.snowflake.com/en/blog/building-data-products-data-economy/ |
| 39 | Gartner (인용) - Dark Data 정의, Data Product 도입률 50% 조사 | https://www.gartner.com/en/information-technology/glossary/dark-data |
| 40 | KPMG - Data Product Strategy Report | https://kpmg.com/kpmg-us/content/dam/kpmg/corporate-communications/pdf/Data%20Product%20Strategy.pdf |
