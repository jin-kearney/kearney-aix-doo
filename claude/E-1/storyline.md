# E-1 데이터 Product화 (Data as a Product) — AI-Ready Data 가이드 스토리라인

> 슬라이드 작성용 스토리라인. 한 슬라이드 = 한 메시지. [슬라이드 N] 단위로 구분.
> 주제 정의: "데이터를 반복 재사용 가능한 상품 단위로 정의하고 책임자·품질·제공 방식을 갖춰 운영하는 체계."
> MECE 경계: 재사용 가능한 Data Product의 정의·운영·소비자 경험에 집중. 데이터 자산 검색 등록 자체는 데이터 카탈로그(A-1)에서 다룬다.

---

## 1. Executive Summary

### [슬라이드 1] 한 장 요약 — 데이터를 '추출물'이 아니라 '제품'으로 운영한다
**헤드라인:** 반복 요청·다팀 사용·AI 활용이 잦은 데이터를 오너·품질·인터페이스를 갖춘 '제품'으로 만들어, 한 번 만들면 여러 팀이 신뢰하고 재사용한다.

- **정의:** Data Product = 코드·데이터/메타데이터·인프라가 하나로 캡슐화된 독립 단위로, 명세된 Output Port를 통해 소비자에게 신뢰·재사용 가능한 형태로 데이터를 제공하는 것.
- **왜 지금:** 일회성 추출물은 책임자·인터페이스·품질 약속이 없어 매번 재작업되고, AI/RAG가 신뢰할 수 없다. '제품화'는 이 구조적 공백을 메운다.
- **핵심 구축 접근(키워드 4):** ① Product Owner(오너십) ② Output Port + Data Contract(명세된 인터페이스) ③ SLA/SLO(품질 약속) ④ 카탈로그 등록(검색가능성).
- **대표 기대효과/KPI(3):** 재사용률 ↑(중복 제거), SLA 준수율 ↑(신뢰 확보), Time-to-Data ↓(접근 리드타임 단축).
- **로드맵 한 줄:** 파일럿 2~4개 제품·명세 표준화(단기) → LLM 자동 명세·마켓플레이스·Data Contract 연계(중기) → AI Agent 자율 소비(MCP)·Composable Data Product(장기).

[도식 설명: 4분면 — (좌상) 정의/구성, (우상) 왜 지금, (좌하) 4대 구축 접근, (우하) 대표 KPI 3개 + 로드맵 3단계 타임라인]

---

## 2. Why? — 왜 필요한가 (개념·구조 중심)

> 외부 통계·시장자료에 기대지 않고, 데이터를 '제품'으로 다루는 사고 전환과 그 구조가 없을 때 무엇이 원리적으로 불가능해지는지로 필요성을 논증한다. 정량 현황은 발표 본편의 회사 as-is에서 보강.

### [슬라이드 2] 사고 전환 — '자산을 쌓는 것'에서 '제품을 공유하는 것'으로
**헤드라인:** 자산(asset)은 모아 쌓는 것이고, 제품(product)은 공유하고 소비자 경험을 좋게 만드는 것이다 — 데이터를 제품으로 보는 순간 책임·인터페이스·품질이 따라온다.

- 전통적 데이터 관리: 요청이 올 때마다 SQL/ETL로 뽑는 **일회성 추출물(ad-hoc extract)**. 프로젝트가 끝나면 방치·폐기 → 데이터 부채(data debt) 누적.
- Product thinking: 소비자가 실제로 풀려는 문제에서 **역산(work backwards)**하여 미리 설계하고 반복 제공. "Build it and they will come"이 아니다.
- [도식 설명: 좌우 대비 표 — 기존(추출물) vs 제품(Product)]

| 기존 접근 (데이터 = 추출물) | 제품 접근 (데이터 = Product) |
|---|---|
| 요청 시마다 생산 | 미리 설계, 반복 제공 |
| 생산 후 책임자 없음 | Product Owner가 생애주기 전체 책임 |
| 인터페이스 없음(직접 DB 접근) | 명세된 Output Port 제공 |
| 품질은 소비자가 감수 | SLA/SLO로 품질을 명시적으로 약속 |
| 소비자가 데이터를 찾아 헤맴 | 카탈로그에 등록(Discoverable) |

### [슬라이드 3] 구조로 보는 필요성 — 5가지 구조가 없으면 원리적으로 무엇이 막히는가
**헤드라인:** Data Product의 5대 구조(오너십·Output Port·Data Contract·SLA·검색가능성)는 각각 특정 실패를 막기 위해 존재한다 — 구조를 설명하는 것 자체가 필요성의 논증이다.

- **오너십(Ownership):** 없으면 → 품질 저하에 책임지는 주체가 없어 방치되고 신뢰가 소멸.
- **Output Port(명세된 인터페이스):** 없으면 → 소비자가 생산자 내부 DB에 직접 의존, **결합도 폭증**. 스키마 변경 1건이 수십 개 파이프라인을 동시에 깨뜨린다.
- **Data Contract:** 없으면 → 묵시적 의존, 통지 없는 변경 → AI/파이프라인이 '조용한 깨짐(silent breaking change)'에 노출.
- **SLA/SLO:** 없으면 → 품질 목표가 비공식적이라 측정 불가, 소비자가 매번 스스로 검증(반복적 신뢰 비용).
- **검색가능성(Discoverability):** 없으면 → 구전 지식 의존, 동일 데이터를 여러 팀이 중복 재작업.
- [도식 설명: 5개 구조 박스 → 각 박스 아래 '없을 때 발생하는 문제' 매핑]

### [슬라이드 4] Key Objectives — Data Product화로 달성하는 5가지 목표
**헤드라인:** 각 목표는 위 구조·원리와 직접 연결된다.

- **① 반복 재사용으로 중복 작업 제거** — Product = 재사용 building block. 동일 데이터를 여러 팀이 독자 구축할 필요가 사라진다.
- **② 오너십 명확화로 품질 책임 확보** — 품질 책임이 데이터 소스에 가장 가까운 곳으로 이동(shift-left). 중앙팀 사후 정제 대신 도메인이 출처 단계에서 보장.
- **③ 명세된 인터페이스로 소비자 자율 소비** — Output Port + Data Contract로 결합도가 낮아져 생산자·소비자가 독립적으로 진화.
- **④ AI/RAG가 신뢰할 수 있는 데이터 공급** — lineage·신선도·접근제어·계약이라는 '신뢰 신호'가 갖춰져야 AI가 환각·편향 없이 데이터를 쓴다.
- **⑤ 검색가능성 확보로 발견·접근 비용 절감** — 오너·설명·스키마·SLO·사용 패턴이 한 곳에 노출되어 탐색·검증 비용 급감.
- (정량 현황·중복 비용·요청 대기시간 등은 [현황에서 보강])

---

## 3. When? — 언제 필요한가

### [슬라이드 5] 적용 시점과 조건 — 모든 데이터가 아니라 '선택'한다
**헤드라인:** Data Product화는 전수 적용이 아니라 우선순위 기반 투자다 — 트리거가 보일 때 검토하고, 선행 기반 위에서 효과가 극대화된다.

- **항상 vs 조건부:** 조건부. 모든 데이터를 Product화하면 'Data Lake Monster'(무차별 확장)가 재현된다. 의도적·가치 지향적 선택이 원칙.
- **도입 트리거(하나 이상 충족 시 검토):**
  - 반복 요청 다발(여러 팀이 같은 데이터를 반복 요청) → 요청 자체를 제거
  - 다수 팀 공유 필요(2개 이상 도메인 소비) → 중복 파이프라인 제거
  - AI/ML 과제 활용 빈번 → 신뢰 신호·계약 없이는 모델 신뢰성 보장 불가
  - 품질·책임 명확화 필요(오류 반복, 책임자 불명확)
  - SLA 요구 발생(소비자가 특정 신선도·가용성 요구)
- **선행 조건(이 기반 위에서 효과 극대화):** 데이터 카탈로그 기초(A-1), 메타데이터 관리 관행, 도메인 경계 정의, Self-serve 플랫폼 최소 수준.
- **우선순위 원리:** 비즈니스 목표 연결 → 실제 소비자/소비 방식 특정 → 재사용 범위 큰 것부터 → 한 팀이 감당할 범위(bounded context)로 규모 제한.

---

## 4. What? — 무엇인가 (핵심)

### [슬라이드 6] 정의와 범위 — Data Product가 무엇이고, 무엇이 아닌가
**헤드라인:** Data Product는 '소비자를 위해 패키지화·품질보증·재사용을 보장하는 데이터 단위'이며, 단순 데이터셋·자산·카탈로그 등록과는 다르다.

- **정의(종합):** 도메인의 분석 데이터에 대한 접근을 제공하기 위해 **코드·데이터/메타데이터·인프라**가 캡슐화된, 독립 배포 가능하고 읽기 최적화된 표준 단위. 하나 이상의 Output Port로 소비자에게 제공된다. (Dehghani, martinfowler.com; datamesh-architecture.com)
- **인접 개념과의 경계(MECE):**

| 개념 | Data Product와의 차이 |
|---|---|
| 데이터셋(Dataset) | 오너·SLA·소비 인터페이스가 없음 — Product가 되려면 거버넌스 레이어 필요 |
| 데이터 자산(Data Asset) | 소비자용 패키지화·품질보증·재사용을 반드시 보장하지 않음 |
| 데이터 서비스 / DaaS | 전달 메커니즘(배관). DaaP는 설계·거버넌스 철학 — 상호보완적 |
| 데이터 카탈로그 | 카탈로그는 '발견(Discovery)' 지원, Data Product는 '소비자 경험·상품화' (E-1 경계) |

- **용어 구분:** "Data as a Product(DaaP)"는 상위 철학·운영 모델, "Data Product"는 그 결과물인 실제 자산. (Alation)

### [슬라이드 7] Data Mesh에서의 위치 — Data Product는 '아키텍처 퀀텀'이다
**헤드라인:** Data Product는 Data Mesh 4원칙 중 하나이자, 메시(그래프)의 노드 역할을 하는 '독립 배포 가능한 최소 아키텍처 단위(architectural quantum)'다.

- **Data Mesh 4원칙:** ① 도메인 기반 분산 소유권 ② **Data as a Product** ③ Self-serve 데이터 플랫폼 ④ 연합 거버넌스(Federated Computational Governance). 4원칙은 '집합적으로 필요충분'하다.
- **왜 Product 원칙이 핵심인가:** Product 원칙이 없으면 원칙①(도메인 분산)이 오히려 사일로·다크데이터를 늘린다. Product가 품질·사일로 문제를 구조적으로 해결.
- **아키텍처 퀀텀:** 과거에는 파이프라인(코드)·데이터·인프라가 따로 관리됐다. Data Product는 셋을 도메인의 Bounded Context 수준에서 하나로 묶는다.
- (출처: martinfowler.com, dbt Labs, datamesh-architecture.com)

### [슬라이드 8] 핵심 구성요소 — 3대 구조 + Input/Output Port
**헤드라인:** Data Product는 코드·데이터/메타데이터·인프라를 캡슐화하고, 표준 포트로 외부와 소통한다.

- **3대 구조적 구성요소(Dehghani):**
  - **Code** — 데이터 파이프라인 코드, API 코드, 접근제어·컴플라이언스 정책 코드
  - **Data & Metadata** — 분석·이력 데이터 + 품질 지표·시맨틱 정의·거버넌스 메타데이터
  - **Infrastructure** — 빌드·배포·실행·스토리지 인프라
- **Input Port / Output Port:** Input Port는 소스(운영 시스템 또는 다른 Data Product)에서 데이터 수신, Output Port는 소비자에게 데이터 게시(형식·프로토콜 명시).
- **5가지 포트 유형(DPSD, Agile Lab):** Input / Output / Discovery(정적 역할) / Observability(동적 행동) / Control(로컬 정책·거버넌스).
- [도식 설명: 가운데 Data Product 박스(Code+Data+Infra), 좌측 Input Port(소스 유입), 우측 Output Port(소비자 유출), 상/하단 Discovery·Observability·Control 포트]

### [슬라이드 9] 동작 원리 — Data Contract와 SLA/SLO/SLI로 신뢰를 보장한다
**헤드라인:** Output Port에는 생산자-소비자 합의(Data Contract)가 붙고, 품질은 SLI→SLO→SLA 3단계로 약속·측정된다.

- **Data Contract:** 스키마·시맨틱·SLA·이용약관·데이터 공유 조건을 명시한 공식 합의. 업계 표준은 **ODCS(Open Data Contract Standard)** — PayPal에서 기원, 현재 Linux Foundation Bitol 프로젝트(v3.1.0), YAML. (datacontract.com은 Data Contract CLI 등 ODCS 호환 툴링을 제공하는 별도 독립 사이트로 운영 중)
- **SLI / SLO / SLA 구분:**

| 개념 | 정의 | 예시 |
|---|---|---|
| SLI (지표) | 측정 지표 자체 | 신선도 지연(초) |
| SLO (목표) | 내부 목표치 | 신선도 95%ile < 1시간 |
| SLA (약속) | 소비자와의 명시적 약속(위반 시 결과) | "24시간 내 갱신 보장" |

- **핵심 SLA 차원(ODPS 4.1, 11개):** Freshness, Completeness, Accuracy, Availability(UpTime), IntervalOfChange, Timeliness 등.

### [슬라이드 10] 라이프사이클 — 아이디에이션부터 폐기까지 '살아있는 제품'으로 운영
**헤드라인:** Data Product는 한 번 만들고 끝이 아니라 설계→개발→배포→운영→버전관리→진화→폐기의 생애주기로 관리된다.

- 통합 단계: ① 아이디에이션(가치·소비자·후보) → ② 설계(Data Contract·명세·Output Port·거버넌스 내재) → ③ 개발(파이프라인·품질검사·접근제어) → ④ 배포(Output Port 게시·카탈로그 등록) → ⑤ 운영·거버넌스(SLA 모니터링·피드백) → ⑥ 버전관리(SemVer·하위 호환) → ⑦ 진화 → ⑧ 폐기(소비 분석 후 단계적 종료).
- 프레임워크별 단계 수는 다르나(Databricks 7단계, Snowflake 4단계, Alation 4단계) 골격은 동일.
- **운영 원칙:** 거버넌스 체크포인트 내재화, DataOps·CI/CD, 품질 Shift-Left, AI 지원 유지보수.
- [도식 설명: 순환형 라이프사이클 8단계 화살표]

### [슬라이드 11] 유형과 성숙도 — 3대 아키타입과 성숙도 차원
**헤드라인:** Data Product는 Source-aligned / Aggregate / Consumer-aligned로 분류되고, 성숙도는 여러 차원의 '곱셈'으로 평가된다.

- **3대 유형(archetype 필드로 명세):**
  - **Source-aligned** — 운영 시스템 원시 데이터를 최소 변환으로 캡슐화. 목적: 소스를 신뢰 가능하게. (예: ERP 생산 실적)
  - **Aggregate** — 여러 제품을 통합. 설계 초기에 계획하지 않고 중복 로직이 반복될 때 '드러난다'. (예: 고객 360, 공통 KPI 집계)
  - **Consumer-aligned** — 특정 소비자·의사결정에 최적화. 다른 Data Product를 소비. (예: 이탈 예측 피처, RAG 소스 임베딩)
- **성숙도(Shri Salem / ZS Associates 10차원):** Awareness·Usability·Interoperability·Actionability·Speed·Innovation·Trust·Adoption·Business Impact·Product Orientation. 핵심 통찰 — 차원은 **곱셈 관계**라 하나만 낮아도 전체 효과가 억제된다.
- **DATSIS / FAIR 특성:** Discoverable·Addressable·Trustworthy·Self-describing·Inter-operable·Secure / Findable·Accessible·Interoperable·Reusable.

### [슬라이드 12] 표준·프레임워크 — ODPS·ODCS·Data Product Canvas
**헤드라인:** Data Product 메타데이터(ODPS)와 데이터 계약(ODCS)이 오픈 표준으로 수렴 중이며, 설계는 Data Product Canvas로 시작한다.

- **ODPS(Open Data Product Specification):** Linux Foundation 주관, 벤더 중립 YAML 메타데이터 모델, v4.1(2025.10). 구성: Details / **Product Strategy(KPI↔비즈니스 목표 연결, v4.1 신규)** / Data Contract / SLA(11차원) / Quality(8차원) / Pricing / Licensing / Access / Holder / Catalog. 도입: NATO·BASF·Migros 등.
- **ODCS(Open Data Contract Standard):** Bitol(Linux Foundation), v3.x, YAML. fundamentals·schemas·quality·SLA·stakeholders·access 포함.
- **Data Product Canvas(datamesh-architecture.com, CC BY 4.0):** 8개 블록 — Domain / Name / Consumer & Use Case / Data Contract / Sources / Architecture / Ubiquitous Language / Classification.

### [슬라이드 13] 핵심 용어집
**헤드라인:** Data Product를 이해하기 위한 필수 용어.

| 용어 | 영문 | 정의 |
|---|---|---|
| 데이터 프로덕트 | Data Product | 코드·데이터·인프라가 캡슐화된 독립 배포 단위, 소비자에게 신뢰·재사용 형태로 제공 |
| 프로덕트 오너 | Data Product Owner | 가치 극대화에 최종 책임 — 비전·로드맵·품질·소비자 만족 관장 |
| 아웃풋 포트 | Output Port | 외부 소비자에게 데이터를 제공하는 표준화된 인터페이스(테이블·파일·API·스트림 등) |
| 데이터 계약 | Data Contract | 생산자-소비자 간 공식 합의(스키마·SLO·이용조건). ODCS로 표준화 |
| SLA / SLO | Service Level Agreement / Objective | SLO=내부 목표, SLA=소비자와의 명시적 약속(위반 시 결과) |
| 데이터 메시 | Data Mesh | 분석 데이터의 분산 소유·제공 아키텍처. 4원칙으로 구성 |
| 자가서비스 플랫폼 | Self-Serve Data Platform | 도메인 팀이 Data Product를 독립 구축·운영하게 하는 인프라 추상화 |
| 아키텍처 퀀텀 | Architectural Quantum | 독립 배포 가능한 최소 아키텍처 단위 — Data Mesh에서 Data Product가 이 역할 |
| 페더레이티드 거버넌스 | Federated Computational Governance | 도메인 자율성 + 글로벌 표준 자동 적용을 결합한 거버넌스 |

---

## 5. How? — 어떻게 구축하는가 (핵심)

### [슬라이드 14] 구축 단계 — 0→1, 5단계 Phase 로드맵
**헤드라인:** 한꺼번에 전부가 아니라, 파일럿에서 시작해 표준화·확장·상시 운영으로 단계적으로 키운다(통상 12~24개월).

- **Phase 1 — 전략·진단(1~2개월):** 플랫폼 성숙도 평가, 도메인 경계(Bounded Context) 식별, Product Owner 후보 지정. 산출물: 현황 진단·도메인 맵·파일럿 후보 목록·로드맵.
- **Phase 2 — 파일럿 MVP(2~4개월):** 1개 도메인·2~3개 제품을 실제 구현. 후보 도출 → Canvas 작성 → ODPS 명세 → Data Contract 초안 → Output Port 1종 구현 → 카탈로그 등록 → 소비자 피드백. 산출물: Canvas·Spec YAML·Contract YAML·Output Port 코드·회고.
- **Phase 3 — 표준화·플랫폼 강화(3~6개월):** Spec 템플릿 표준화, Contract 자동 검증 CI/CD, 거버넌스 정책 코드화(Policy-as-Code), 마켓플레이스 v1.0.
- **Phase 4 — 확장(6~12개월):** 도메인 순차 온보딩, AI/ML 연동(Feature Store·RAG), 마켓플레이스 고도화, KPI 대시보드.
- **Phase 5 — 지속 운영(상시):** SemVer 버전관리, Breaking Change 자동 탐지, Deprecation→Sunset 프로세스, Retirement 판정.
- [도식 설명: 5단계 수평 타임라인 + 각 단계 산출물]

### [슬라이드 15] 해야 할 작업 ① — 후보 선정 스코어링
**헤드라인:** 수백 개 데이터셋을 사람이 훑는 대신, 가중 스코어링으로 상위 5~10개를 파일럿 후보로 뽑는다.

| 평가 기준 | 설명 | 가중치(예) |
|---|---|---|
| 반복 요청 빈도 | 최근 6개월 동일 데이터 요청 티켓 수 | 25% |
| 소비 팀 수 | 이미 사용 중인 팀·개인 수 | 20% |
| AI/분석 과제 연동 | 진행·계획 과제의 필수 데이터 여부 | 20% |
| 데이터 품질 가능성 | 원천 정합성·최신성 | 15% |
| 구현 복잡도(역산) | 낮을수록 우선 | 10% |
| 비즈니스 임팩트 | 연결 지표·의사결정 중요도 | 10% |

- 스코어 = Σ(기준별 점수 × 가중치). 분류 병행: Source-aligned(원천 1:1, 초기 용이) / Aggregate(다팀 소비) / Consumer-aligned(특정 유스케이스).

### [슬라이드 16] 해야 할 작업 ② — 명세서(Spec)와 Data Contract 작성
**헤드라인:** Data Product 명세서는 '소비자가 알아야 할 모든 것'을 기계가독 YAML(ODPS)로, 인터페이스 합의는 Data Contract(ODCS)로 명시한다.

- **명세서 필수 항목(최종 주제.md 요구 + ODPS):** 대상 사용자, 제공 데이터, 사용 목적(valueProposition), 품질 기준, 갱신 주기(updateFrequency), SLA, 오너, 사용 조건, 예시 쿼리(useCase), 제공 방식(Output Port).
- **ODPS 필수 필드:** name·productID·visibility·status·type. + 선택: valueProposition·useCases·outputFileFormats·recommendedDataProducts·SLA(11차원)·dataQuality(8차원).
- **Data Contract(ODCS/datacontract.com):** info(title·owner)·servers·models(필드·타입·required)·quality(SodaCL 등)·servicelevels(freshness·availability). Data Contract CLI로 lint·test 자동화.
- [도식 설명: ODPS YAML 스니펫 + Data Contract YAML 스니펫 나란히]

### [슬라이드 17] 해야 할 작업 ③ — 계열사 이질적 솔루션 환경 추상화
**헤드라인:** ERP·MES·QMS·LIMS가 제각각인 그룹사에서 표준 Data Product를 제공하려면, 원천을 직접 노출하지 않고 추상화 계층으로 감싼다.

- **A. Data Virtualization** — 물리 이동 없이 분산 원천에 통합·실시간 접근. 원천 변경(ERP 업그레이드)이 하위 분석에 영향 안 줌. (Denodo·Starburst·Dremio)
- **B. Semantic Layer** — 가상화 위에 메트릭·문서·조인·정책 추가, 소비자는 비즈니스 개념으로 접근. (dbt Semantic Layer·AtScale·Cube)
- **C. Federated Query** — 단일 쿼리를 여러 이질적 원천에 분산 실행 후 통합. (Trino/Starburst·Athena Federation)
- **D. Unified Namespace(UNS)** — 제조 현장 전용. PLC·SCADA·MES가 단일 브로커(MQTT/Kafka)에 발행, 필요 시스템이 구독. OT/IT 통합.
- **Self-Serve 플랫폼 최소 구성:** 스토리지 추상화·쿼리 엔진·오케스트레이션·CI/CD·카탈로그·접근제어 자동화·SLA 모니터링.

### [슬라이드 18] 해야 할 작업 ④ — 소비 방식별 Output Port 설계
**헤드라인:** 소비자 유형에 맞춰 Output Port를 설계하되, 물리 테이블 직접 노출 대신 추상화(뷰·계약)를 둔다.

| 소비 방식 | 형태 | 적합 소비자 | 설계 포인트 |
|---|---|---|---|
| 테이블/SQL | DB 뷰·공유 스키마, Delta Sharing, Iceberg | 분석가·BI | 물리 테이블 직접 접근 지양, Row/Column 보안 |
| 파일 | Parquet·CSV·Delta/Iceberg (S3·ADLS·GCS) | 데이터 엔지니어·배치·ML | 파티셔닝·네이밍 규칙 표준화 |
| API | REST(OpenAPI)·GraphQL | 앱·마이크로서비스 | 버저닝·Rate Limit·Gateway |
| 대시보드/BI | Power BI·Tableau·Looker | 경영진·현업 | 대시보드는 최종 소비층이지 Product 자체가 아님 |
| RAG 소스/Vector | 임베딩·청크(Weaviate·pgvector 등) | LLM 에이전트·Q&A | 임베딩 갱신 주기·접근제어를 SLA로 관리 |
| 분석 패키지 | PyPI·dbt 패키지·MLflow | 분석·ML팀 | 패키지 버전 = Spec 버전 동기화 |

### [슬라이드 19] Pain Point & AI 기반 해결
**헤드라인:** 수작업으로 막히는 6개 지점을 LLM·이상탐지·시맨틱 매핑이 자동화한다.

| Pain Point (수작업) | AI 기반 해결 |
|---|---|
| 수백 개 중 Product 후보 수동 식별 | **LLM 후보 추천** — 카탈로그 메타데이터·접근로그·쿼리이력 기반 스코어링·추천 |
| Spec YAML 수기 작성(누락·오류) | **자동 명세·문서 생성** — 스키마·DDL·샘플로 Spec 초안 자동 생성(Atlan 등 AI 지원 카탈로그 사례) |
| Data Contract 수동 협의 | **NL→Data Contract** — 자연어 요구를 ODCS YAML로 자동 생성(LoRA/PEFT 파인튜닝) |
| SLA 수동 점검(지연 인지) | **이상탐지 SLA 모니터링** — freshness·volume·schema 자기학습 감지(예: SYNQ Scout, MCP 지원) |
| 이질적 원천 스키마 수동 매핑 | **Semantic 자동 매핑** — 컬럼명·설명·샘플을 LLM이 비교해 semanticLinks 자동 생성(Vector Embedding 검색) |
| 소비자 온보딩 개별 안내 | 카탈로그·예시 쿼리·NL 검색으로 셀프 온보딩 |

### [슬라이드 20] Tech Spec — 시장 솔루션/플레이어 맵
**헤드라인:** 카탈로그·거버넌스형, 레이크하우스형, 가상화/연합형, Data Mesh 전용/계약 툴링으로 구분해 선택한다.

| 솔루션 | 제공사 | 분류 | 핵심 기능 | AI 기능 |
|---|---|---|---|---|
| Atlan | Atlan | 상용(SaaS) | Active Metadata 카탈로그, Data Product 발행·소비, 200+ 커넥터, 컬럼 리니지 | AI 자동 문서화, NL 검색, 메타데이터 80% 자동 |
| Collibra | Collibra | 상용(SaaS) | 카탈로그·거버넌스·품질·마켓플레이스 통합 | AI 분류·태깅, 자동 리니지 |
| Informatica IDMC | Informatica | 클라우드 | 카탈로그·거버넌스·품질·MDM, Cloud Data Marketplace | CLAIRE AI 분류·매핑·품질 추천 |
| MS Fabric + Purview | Microsoft | 클라우드 | OneLake, Unified Catalog, Data Product 발행, GraphQL | Copilot NL 탐색, AI 분류 |
| Databricks Unity Catalog + Marketplace | Databricks | 클라우드/온프레 | Delta Lake·RBAC, Delta Sharing, Iceberg REST | Genie(NL→SQL), 자동 PII·리니지 |
| Snowflake Marketplace + Horizon | Snowflake | 클라우드 | Data Marketplace, Horizon 거버넌스, Clean Room | Cortex AI·Search(벡터) |
| Denodo | Denodo | 온프레/클라우드 | Data Virtualization(150+ 커넥터), Semantic Layer, 연합 쿼리 | AI 추천 뷰, NL 쿼리 |
| Starburst (Trino) | Starburst | 상용+오픈소스 | Federated Query, Galaxy, Gravity 카탈로그 | AI 쿼리 최적화, NL→SQL |
| dbt (Mesh + Semantic Layer) | dbt Labs | 오픈소스+상용 | dbt Mesh, Semantic Layer(GA), 메트릭 중앙화 | 문서 자동 생성, SYNQ 연동 |
| Nextdata OS | Nextdata (Z. Dehghani) | 상용(SaaS) | 자율 Data Product 컨테이너화, 기존 에셋 자동 부트스트랩 | AI 에이전트 기반 자동 생성·정책 집행 |
| SYNQ | SYNQ | 상용(SaaS) | Data Product 신뢰성, SLA·인시던트 관리 | Scout(자기학습 이상탐지), MCP 지원 |
| datacontract.com 툴링 | 오픈소스/Bitol | 오픈소스 | Data Contract Spec·CLI(lint·test), ODCS 지원 | LLM 기반 Contract 자동 생성 |

### [슬라이드 21] 구축 시 고려사항 / 함정(Anti-Patterns)
**헤드라인:** 실패는 대부분 기술이 아니라 조직·프로세스에서 온다 — 6개 함정을 사전에 차단한다.

- **Over-Engineering:** 완벽 아키텍처 선구축 → 출시 지연. ▶ 소비자 유스케이스부터 역산, 파일럿 먼저, 플랫폼은 수요에 맞춰 진화.
- **Governance 부재:** 도메인마다 제각각, KPI 정의 5종 난립. ▶ Policy-as-Code + CI/CD 자동 검증(Data Contract CLI).
- **Ownership 공백:** "이름만 있고 책임자 없음". ▶ Owner 책임(SLA·품질·소비자 지원) 공식화, RACI, On-call.
- **소비자 미고려:** 생산자 관점 설계. ▶ Canvas의 'Consumer & Use Case'를 가장 먼저, 소비자가 워크숍 참여.
- **Data Hydration Gap:** 분산 거버넌스에서 범용 재사용 제품 투자 기피(편익이 타 도메인 귀속). ▶ 크로스 도메인 인센티브, 플랫폼 팀이 공통 기반 제품 직접 소유.
- **정적 플랫폼:** 3~4년마다 대규모 재구축. ▶ 플랫폼을 진화하는 제품으로 운영.

---

## 6. KPI? — 관리 지표 (5개)

### [슬라이드 22] 관리 지표 5개 — 운영 KPI와 채택 KPI의 균형
**헤드라인:** Data Product가 신뢰성 있게 운영되는지(운영)와 실제 가치를 창출하는지(채택)를 5개 지표로 균형 있게 관리한다.

**KPI 관리 목적:** Data Product가 ① 신뢰성 있게 운영되는지(운영 KPI)와 ② 실제 가치를 창출하는지(채택 KPI)를 균형 있게 측정한다. 기술 성능이 좋아도 채택이 없으면 낭비이고, 채택이 많아도 SLA를 못 지키면 신뢰가 무너진다.
**기대효과:** 중복 제거·소비자 신뢰·접근 속도·품질 보증을 정량 관리해, 투자 우선순위와 폐기 판단의 근거를 확보한다.

| KPI | 정의(산식) | 측정 목적 | 방향 | 기대효과 |
|---|---|---|---|---|
| 재사용률 (Reuse Rate) | 2회 이상 소비 팀 수 ÷ 전체 소비자 수 × 100 | 반복 재사용 검증 | ↑ | 중복 데이터셋·개발비용 감소, 일관성 |
| 활성 소비자 수 (Active Consumers) | 월/분기 내 실제 쿼리·API 호출 사용자 수 | 수요·관련성 측정 | ↑ | 채택 확대, 데이터 민주화 |
| SLA/SLO 준수율 | SLA 충족 기간 ÷ 전체 측정 기간 × 100 | 신뢰성·가용성 이행 | ↑ (목표 95%+) | 소비자 신뢰, 섀도 데이터 억제 |
| Time-to-Data | 요청 시점 ~ 사용 가능 시점 경과 시간 | 접근성·셀프서비스 효율 | ↓ | 의사결정 속도, 병목 해소 |
| 데이터 품질 점수 | 완전성·정확성·최신성·일관성 가중 평균(0~100/티어) | 피트니스(fitness for use) | ↑ | 신뢰도, AI 학습 데이터 품질 보증 |

> 비즈니스 영향 증거가 추가로 필요하면 **AI 과제 활용 건수**(Data Product를 입력/피처로 쓴 AI·ML 과제 수)를 보강 지표로 포함.
> 주의: KPI 과부하 금지(핵심 5개 압축), 활동 지표(쿼리 수)만으로 불충분 — 비즈니스 아웃컴과 연결, 도메인 이해관계자와 공동 설계.

---

## 7. Roadmap — 확산 로드맵

### [슬라이드 23] 단기 / 중기 / 장기 확산 로드맵
**헤드라인:** 파일럿·표준화(단기) → 자동화·마켓플레이스·거버넌스 연계(중기) → AI Agent 자율 소비·Composable(장기)로 성숙도를 끌어올린다.

- **단기(0~6개월) — 기반 구축:** 파일럿 Data Product 2~4개, 명세 표준화(오너·Output Port·SLA), 카탈로그 연동(A-1), 기본 SLI 수집(freshness·availability·quality), Data Contract 초안. ▶ 성숙도 Lv.1→2.
- **중기(6~18개월) — 확산·고도화:**
  - 자동화: LLM 자동 명세·문서·추천(dbt Cloud AI·Snowflake Cortex·Bedrock 연동)
  - Data Marketplace/카탈로그 구축(셀프서비스 포털, NL 검색)
  - 타 주제 연계: 데이터 카탈로그(A-1) 자동 등록, Data Contract CI/CD, Semantic Layer/온톨로지, Lineage(OpenLineage)
  - Federated Computational Governance(Policy-as-Code). ▶ Lv.2→3→4.
- **장기(18개월+) — AI-Native 고도화:**
  - **Composable Data Products** — 제품 간 조합으로 더 복잡한 제품 생성
  - **AI Agent 자율 소비(MCP/Tool 연계)** — Data Product를 MCP 서버로 노출, 에이전트가 자율 접근. Data Product Agent Mesh
  - 실시간·예측·처방 분석 내재화, MLOps 기반 확립. ▶ Lv.4→5(Strategic Enterprise).
- [도식 설명: 3단계 타임라인 + 하단에 성숙도 Level 1→5 진행 바]

---

## 참고 출처

- [Zhamak Dehghani — Data Mesh Principles and Logical Architecture (martinfowler.com, 2020)](https://martinfowler.com/articles/data-mesh-principles.html)
- [Thoughtworks — Treat Data as a Product (Modern Data Engineering Playbook)](https://www.thoughtworks.com/insights/e-books/modern-data-engineering-playbook/data-as-a-product)
- [datamesh-architecture.com — Data Mesh Architecture / Data Product Canvas](https://www.datamesh-architecture.com/data-product-canvas)
- [Open Data Product Specification v4.1 (Linux Foundation)](https://opendataproducts.org/v4.1/)
- [Open Data Contract Standard (ODCS) — Bitol / Linux Foundation](https://bitol-io.github.io/open-data-contract-standard/)
- [datacontract.com — Data Contracts & CLI](https://datacontract.com/)
- [Data Product Specification (datamesh-architecture.com / INNOQ)](https://dataproduct-specification.com/)
- [Alation — Data as a Product vs Data as a Service](https://www.alation.com/blog/data-as-a-product-vs-data-as-a-service/)
- [Alation — Stages of the Data Product Lifecycle](https://www.alation.com/blog/data-product-lifecycle/)
- [Shri Salem (ZS Associates) — Data Product Maturity Framework (Medium, 2023)](https://medium.com/@salemshriram/a-framework-to-assess-and-enhance-the-maturity-of-data-products-64bc412a20fc)
- [Equal Experts — What is a Data Product Owner?](https://www.equalexperts.com/blog/data/what-is-a-data-product-owner-responsibilities-challenges-and-best-practices/)
- [dbt Labs — How to ensure Data Product SLAs and SLOs](https://www.getdbt.com/blog/data-product-slas-and-slos)
- [beefed.ai — How to Design Data Products with SLAs](https://beefed.ai/en/design-data-products-with-slas)
- [Modern Data 101 — The Role of Data Product KPIs](https://www.moderndata101.com/blogs/the-role-of-data-product-kpis-in-measuring-business-impact)
- [InTechHouse — Measuring the Success of Data Mesh](https://intechhouse.com/blog/measuring-the-success-of-data-mesh-in-your-organization/)
- [Everforth ECS — Data Mesh Maturity Model](https://everforthecs.com/ecs-insight/article/how-do-you-implement-an-effective-data-mesh-maturity-model/)
- [Databricks — Building High-Quality and Trusted Data Products](https://www.databricks.com/blog/building-high-quality-and-trusted-data-products-databricks)
- [Nextdata — Introducing Nextdata OS (Autonomous Data Products)](https://www.nextdata.com/our-pov/introducing-nextdata-os-autonomous-data-products-for-the-autonomous-era)
- [SYNQ — Building reliable Data Products with dbt Cloud and SYNQ](https://www.getdbt.com/blog/building-reliable-data-products-with-dbt-cloud-and-synq)
- [Thoughtworks — Model Context Protocol's impact on 2025](https://www.thoughtworks.com/en-us/insights/blog/generative-ai/model-context-protocol-mcp-impact-2025)
- [arXiv — AI-Driven Generation of Data Contracts (2025)](https://arxiv.org/abs/2507.21056)
- [arXiv — Metadata Extraction Leveraging LLMs from Legal Contracts (2025)](https://arxiv.org/abs/2510.19334)
