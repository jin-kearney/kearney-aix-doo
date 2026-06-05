# E-1 데이터 Product화 (Data Product / Data as a Product)
## Why / When 리서치 노트 — 개념·구조 중심

---

## 1. "프로젝트 추출물"에서 "제품(Product)"으로의 사고 전환

### 1-1. 패러다임 전환의 의미

전통적인 데이터 관리에서 데이터는 특정 프로젝트나 보고서를 위한 **일회성 추출물(ad-hoc extract)**로 취급된다. 팀이 필요할 때마다 SQL을 짜거나 ETL을 돌려 데이터를 뽑아내며, 그 결과물은 해당 프로젝트가 끝나면 사실상 폐기되거나 방치된다.

Zhamak Dehghani는 이를 "데이터를 자산(asset)처럼 모으고 쌓는 행위"로 규정하면서, "제품(product)은 그 반대 — 공유하고, 소비자가 기쁘게 사용할 수 있도록 만드는 것"이라고 대비한다.

> "Data as a product is very different from data as an asset. What do you do with an asset? You collect and hoard it. With a product it's the other way around. You share it and make the experience of that data more delightful."
> — Zhamak Dehghani (Thoughtworks)

**Product thinking을 데이터에 적용한다는 것**은 아래를 의미한다:

| 기존 접근 (데이터 = 추출물) | Product 접근 (데이터 = 제품) |
|---|---|
| 요청이 올 때마다 생산 | 미리 설계하고 반복 제공 |
| 생산 후 책임자 없음 | 오너(Product Owner)가 생애주기 전체 책임 |
| 인터페이스 없음(직접 DB 접근) | 명세된 Output Port / 인터페이스 제공 |
| 품질은 소비자가 감수 | SLA/SLO로 품질 약속을 명시 |
| 소비자가 데이터를 찾아 헤맴 | Discoverable — 카탈로그에 등록 |

**Thoughtworks**는 이를 "Build it and they will come" 방식에서 "소비자의 문제에서 역산(work backwards)하여 설계"하는 방식으로의 전환이라고 설명한다.

### 1-2. Data Product의 정의 (아키텍처 관점)

datamesh-architecture.com의 Jochen Christ 정의:

> "A data product is a logical unit that contains all components to process and store domain data for analytical or data-intensive use cases and makes them available to other teams via output ports."

Dehghani의 Data Mesh 원칙(martinfowler.com)에서는 Data Product를 **"아키텍처 퀀텀(architectural quantum)"** — 독립 배포 가능하며 고도의 기능 응집도를 갖춘 최소 아키텍처 단위 — 으로 규정한다. 이 퀀텀은 세 가지 구조 요소를 내포한다:

1. **Code**: 데이터 파이프라인 코드, API 코드, 접근 제어·컴플라이언스 정책 코드
2. **Data and Metadata**: 분석·이력 데이터 본체 + 품질 지표, 의미론적 정의, 거버넌스 메타데이터
3. **Infrastructure**: 데이터 제품 빌드·배포·실행·스토리지를 위한 인프라

---

## 2. Data Product의 구조적 특성 5가지와 "없을 때" 발생하는 원리적 문제

### 2-1. 명확한 오너십 (Ownership)

**구조**: 도메인 팀의 **Data Product Owner**가 품질·진화·소비자 만족에 대한 책임을 전담한다. 오너는 데이터 사용자가 누구인지, 어떻게 사용하는지, 어떤 형식으로 소비하기를 원하는지에 대한 깊은 이해를 보유해야 한다(Dehghani, martinfowler.com).

**없을 때 무엇이 불가능해지는가**: 품질 저하가 발생해도 책임지는 주체가 없다. Thoughtworks Roche 구현 사례에서 확인된 것처럼, 소유자가 없으면 SLO 위반 시 복구까지 복잡한 우선순위 경쟁이 벌어지고, 소비자(하위 비즈니스 프로세스)에 심각한 영향을 준다. 결국 품질은 방치되고 신뢰는 소멸된다.

### 2-2. 명세된 인터페이스 / Output Port

**구조**: Data Product는 하나 이상의 **Output Port**를 통해 읽기 전용 데이터를 노출한다. Output Port는 SQL 뷰, 파일, 스트림 토픽 등 다양한 형태로 구현되며, 각 Port는 **Data Contract**와 연결된다. 출력 포트는 데이터 모델(스키마)을 명시적으로 정의하며 추상화 레이어 역할을 한다 — 내부 구현이 바뀌어도 소비자는 영향을 받지 않는다(datamesh-architecture.com).

**없을 때 무엇이 불가능해지는가**: 소비자가 생산자의 내부 DB나 테이블에 직접 의존하게 되어 **결합도(coupling)가 폭증**한다. 생산자의 스키마 변경 한 건이 수십 개 파이프라인을 동시에 깨뜨린다. 소비자 자율 소비가 불가능해지고, 모든 변경이 상호 협의를 요구하는 병목이 된다.

### 2-3. Data Contract

**구조**: Output Port에 대해 생산자와 소비자 간 공식 합의를 문서화한 것. 스키마, 의미론(semantics), 신선도(freshness) 기대값, 접근 규칙, 컴플라이언스 제약 등을 정의한다. Open Data Contract Standard (ODCS), Open Data Product Standard (ODPS) 같은 산업 표준이 존재한다(datacontract.com, datamesh-manager.com).

**없을 때 무엇이 불가능해지는가**: 생산자와 소비자 사이의 묵시적 의존이 형성된다. 변경이 공식 통지 없이 이루어지며, AI 모델·파이프라인이 "조용한 깨짐(silent breaking change)"에 노출된다. AI/ML 시스템은 계약 없는 데이터를 신뢰할 수 없어 지속적 재검증이 필요해진다.

### 2-4. SLA (Service Level Agreement) / SLO (Service Level Objective)

**구조**: "전일 데이터가 매일 오전 4시 이전에 적재됨", "99.5% 가용성" 등 측정 가능한 품질 목표를 선언한다. SLI(Service Level Indicator)로 지속 측정하고, Error Budget을 정의해 위반 시 개선 작업을 강제한다(Thoughtworks, Roche 구현 사례).

**없을 때 무엇이 불가능해지는가**: 품질 목표가 비공식적이어서 측정 자체가 불가능하다. 소비자는 데이터의 신선도나 완전성을 판단할 기준이 없어 스스로 검증해야 하며, 반복적 신뢰 비용이 발생한다. 리테일 예시(Thoughtworks)처럼 수요 계획 지표가 지연되면 재고 과잉·부족으로 직결된다.

### 2-5. 검색가능성 (Discoverability)

**구조**: Data Product는 데이터 카탈로그에 등록되어 오너, 소스, 리니지(lineage), 샘플 데이터 등 메타데이터를 노출한다. 고유 주소(Addressability)도 보장되어 소비자가 일관되게 찾을 수 있다(Dehghani 원문; Thoughtworks 가이드).

**없을 때 무엇이 불가능해지는가**: 소비자는 데이터를 찾기 위해 구전 지식(tribal knowledge)에 의존하거나 데이터 엔지니어에게 물어봐야 한다. 동일한 데이터를 여러 팀이 독자적으로 재작업하는 중복이 발생한다. Adevinta 사례에서 확인된 것처럼, 검색 가능성이 없으면 데이터 엔지니어는 "데이터 어디 있어요?" 문의에 시달리며 생산적 업무를 방해받는다.

---

## 3. 핵심 방향성 개념 쌍

### 3-1. Producer(생산자/도메인 오너) ↔ Consumer(소비자) 관점

Data Product 모델은 **도메인 팀이 생산자**가 되어 자신의 분석 데이터를 제품으로 게시하고, **다른 팀이 소비자**로서 자율적으로 발견·접근·소비하는 구조를 만든다.

이 모델의 핵심 효과는 **중앙 계획 프로세스의 단축**이다. 과거에는 데이터 팀이 모든 요청을 중앙에서 처리했으나, Product 모델에서는 생산자-소비자 간 직접 소통(data contract 기반)이 이루어지며 대기열이 제거된다(Thoughtworks, datamindedbe).

Product thinking은 "소비자를 고객으로 대우"함을 핵심으로 한다. Dehghani는 데이터 제품 오너가 "사용자가 누구인지, 어떻게 데이터를 사용하는지, 어떤 방법으로 소비하기를 원하는지를 친밀하게 이해해야 한다"고 강조한다.

### 3-2. 데이터 부채(Data Debt)와 재사용성(Reusability)

**데이터 부채**는 Product 없이 일회성 추출을 반복할 때 누적된다. Netflix의 Data Health 이니셔티브는 이를 "복잡성 감소, 데이터 부채 해소, 데이터 처리 방식에 대한 의도적 트레이드오프 확보"를 위한 전사적 노력으로 규정한다(Netflix Technology Blog).

**재사용성**은 Data Product의 본질적 설계 목표다. Data Product는 특정 Use Case를 위한 단발 파이프라인이 아니라, **여러 Use Case에 걸쳐 반복 재사용 가능한 구성 요소(building block)**로 설계된다. 조직이 첫 번째 제품을 완성하면, 이후 유사한 Use Case는 기존 제품 위에 빠르게 구축되어 time-to-market이 단축된다(Dataminded, Snowflake).

### 3-3. Product Thinking — 사용자 중심·생애주기·책임

**Product thinking**의 세 기둥:
- **사용자 중심(User-centric)**: 데이터를 공급자 관점이 아닌 소비자가 실제로 해결하려는 문제에서 역산하여 설계한다.
- **생애주기(Lifecycle)**: 프로젝트는 납품으로 끝나지만 Product는 지속적으로 진화한다. 오너는 소비자 요구 변화에 맞게 제품을 개선하고 새 기능을 릴리즈한다.
- **책임(Accountability)**: Thoughtworks Roche 사례에서 Data Product MVP 체크리스트가 명시하듯, "Owner/Steward"는 필수 요소이며 단순 custodian이 아닌 SLO와 비즈니스 가치에 책임지는 역할이다.

### 3-4. Data as a Product — Data Mesh 4원칙 중 하나로서의 의미

Dehghani의 Data Mesh는 4개 원칙으로 구성된다:

1. **도메인 지향 분산 데이터 오너십 및 아키텍처**
2. **Data as a Product** ← 현재 주제
3. **Self-serve 데이터 인프라 플랫폼**
4. **연합 계산 거버넌스 (Federated Computational Governance)**

이 4원칙은 "집합적으로 필요충분(collectively necessary and sufficient)"하다. **Data as a Product 원칙이 없으면 원칙 1(도메인 분산)은 오히려 역효과**를 낳는다. 도메인마다 데이터를 생산하지만 사일로가 증가하고, 다크 데이터(Gartner 용어: 조직이 수집·처리하지만 다른 목적에는 활용하지 못하는 정보 자산)가 늘어날 뿐이다. Product 원칙은 이 품질·사일로 문제를 구조적으로 해결하는 역할을 담당한다.

Dehghani의 원문 요약:

> "Data as a product — so that data users can easily discover, understand and securely use high quality data with a delightful experience; data that is distributed across many domains."

---

## 4. Data Product화로 달성하는 Key Objectives (개념·구조와 연결)

### Objective 1: 반복 재사용으로 중복 작업 제거
- **구조 연결**: Product = 재사용 가능한 building block. 카탈로그에 공개되어 있어 동일 데이터를 여러 팀이 독자 구축할 필요가 없다.
- **원리**: 재사용이 일어날수록 초기 투자 비용이 분산되고 subsequent use case의 time-to-value가 단축된다.

### Objective 2: 오너십 명확화로 품질 책임 확보
- **구조 연결**: Product Owner + SLO/SLI 체계. 품질 저하 감지 시 책임자가 즉시 대응하는 구조가 내장된다.
- **원리**: "품질 책임이 데이터 소스에 가장 가까운 곳으로 이동(shift left)"한다. 중앙 거버넌스 팀이 사후 정제하는 모델 대신, 도메인이 출처 단계에서 품질을 보장한다.

### Objective 3: 명세된 인터페이스로 소비자 자율 소비
- **구조 연결**: Output Port + Data Contract. 소비자는 계약을 읽고 독립적으로 접근·사용할 수 있다.
- **원리**: 결합도(coupling)가 낮아지므로 생산자와 소비자가 독립적으로 진화할 수 있다. 소비자가 데이터 엔지니어에게 매번 요청하지 않아도 된다.

### Objective 4: AI/RAG가 신뢰할 수 있는 데이터 공급
- **구조 연결**: Discoverability + Lineage + Quality 신호 + Data Contract의 조합이 AI 소비 기반을 형성한다.
- **원리**: AI 시스템은 데이터의 출처(lineage), 신선도, 접근 제어, 컴플라이언스 제약을 "신뢰 신호"로 요구한다. 이 신호가 없으면 AI 모델은 고립된 사실을 추론할 뿐이며 환각(hallucination)이나 편향 출력이 증가한다. Alation의 분석에 따르면 Generative AI와 RAG는 "강력한 출처(provenance)와 신선도 보장을 갖춘 대규모 비정형 콘텐츠에 대한 governed 접근"을 필수로 요구한다.

### Objective 5: 검색가능성 확보로 데이터 발견·접근 비용 절감
- **구조 연결**: Discovery Port + 카탈로그 등록. 오너, 설명, 스키마, SLO, 사용 패턴이 한 곳에 노출된다.
- **원리**: 소비자가 "이 데이터가 있는지, 믿을 수 있는지, 내 Use Case에 맞는지"를 사전에 판단할 수 있어 탐색·검증 비용이 급감한다.

---

## 5. When — 데이터 Product화의 적용 시점·조건

### 5-1. Product화가 항상 필요한가? vs 조건부인가?

**결론: 조건부다. 모든 데이터를 Product화할 필요는 없다.**

Data Mesh 원칙 연구(arXiv 2604.00218)는 각 도메인이 독립적으로 데이터 제품을 구조화할 때, 즉각적인 분석 Use Case에 최적화하는 경향이 있어 조직 전체 재사용을 위한 범용적(general-purpose) 제품에는 투자를 적게 한다고 지적한다. 즉, Product화는 판단이 필요한 투자 행위이며 "전수(全數) 적용"보다 **우선순위 기반 선택**이 원칙이다.

### 5-2. 도입 트리거 — 이 신호가 보이면 Product화를 검토한다

다음 조건 중 하나 이상이 충족될 때 특정 데이터 영역의 Product화를 적극 검토한다:

| 트리거 | 설명 |
|---|---|
| **반복 요청 다발** | 동일·유사 데이터 추출 요청이 여러 팀에서 반복된다면, 재사용 가능 Product 설계로 요청 자체를 제거할 수 있다 |
| **다수 팀 공유 필요** | 두 개 이상의 독립적인 도메인/팀이 동일 데이터를 소비해야 할 때 — 중복 파이프라인 없이 단일 Product로 제공 |
| **AI/ML 과제 활용 빈번** | 특정 데이터가 AI 모델 학습·추론·RAG에 반복 사용된다면, 신뢰 신호와 계약이 없는 상태로는 모델 신뢰성을 보장할 수 없다 |
| **품질·책임 명확화 필요** | 데이터 오류가 반복되지만 책임자가 불명확한 경우, Product 오너십 도입이 근본 해결책이다 |
| **SLA 요구 발생** | 소비자(비즈니스 프로세스, AI 시스템)가 특정 신선도·가용성을 요구할 때 — SLO 없이는 약속 이행 불가 |

Thoughtworks Roche 사례의 접근 방식: "조직 목표 → 고가치 분석 Use Case → 필요한 Data Product"로 역산하며, Use Case 가설 템플릿으로 각 제품화의 비즈니스 가치를 사전에 명시적으로 검증한다.

### 5-3. 적용 조건 — 선행 기반이 어느 정도 갖춰진 뒤 효과 극대화

Data Product화는 고립적으로 시작하기보다, 다음 기반 위에서 효과가 극대화된다:

- **데이터 카탈로그(Data Catalog)의 기초 존재**: Product의 검색가능성(Discoverability)을 지원하는 최소 수준의 카탈로그가 필요하다. Adevinta 사례에서 "인트라넷의 데이터셋 목록"처럼 간단한 형태도 출발점이 된다.
- **메타데이터 관리 관행**: 오너, 설명, 스키마, 리니지 등 핵심 메타데이터를 지속 등록·유지하는 문화와 프로세스가 병행되어야 한다.
- **도메인 경계 정의**: Data Mesh 원칙 1(도메인 오너십)과 연동되어, 어떤 도메인이 어떤 데이터를 "생산 책임"지는지 합의가 선행되어야 한다.
- **Self-serve 플랫폼 최소 수준**: Product 팀이 파이프라인·스토리지·배포를 독립 운영하려면 셀프서비스 가능한 데이터 플랫폼 기반이 필요하다.

### 5-4. 우선순위를 두는 원리 — 모든 데이터를 Product화하지 않는다

Thoughtworks는 "Data Lake Monster"(모든 것을 다 집어넣는 무차별 확장)의 재현을 피하기 위해, **의도적·가치 지향적 결정**으로 어떤 데이터 제품을 만들지 선택하도록 권고한다.

우선순위 판단 기준:

1. **비즈니스 목표 연결**: Lean Value Tree(LVT)처럼 조직 목표와 직접 연결되는 데이터인지 확인한다.
2. **소비자 존재 확인**: 실제 소비자와 소비 방식이 특정된 경우에만 Product화한다 — "누군가 유용하게 쓰겠지"는 Product 설계의 출발점이 될 수 없다.
3. **재사용 범위**: 한 팀만 쓰는 데이터는 내부 asset으로 유지하고, 여러 팀이 반복 소비하는 데이터를 Product화 1순위로 삼는다.
4. **최대 범위 제한**: Data Product의 규모는 "한 팀이 감당할 수 있는 범위(bounded context)"로 제한한다 — 무한 확장은 Product의 원칙에 어긋난다(datamesh-architecture.com).

---

## 6. "Data as a Product" vs "Data Product" — 개념 구분

혼용이 잦은 두 용어를 명확히 구분한다:

| 개념 | 설명 |
|---|---|
| **Data as a Product** (DaaP) | 철학·운영 원칙: 오너십, 사용성, 책임, 가치 측정을 강조하는 마인드셋. Data Mesh 4원칙 중 하나. |
| **Data Product** | DaaP 원칙을 적용해 만든 **결과물(output)**: 특정 소비자와 Use Case를 위해 설계된 패키지 자산. 기술적 구현체(Output Port, Code, Infra 포함). |

많은 조직이 데이터셋에 이름만 붙여 "Data Product를 출시"한다고 하지만, 진정한 Product화는 **설계·거버넌스·운영 방식의 전환**을 수반한다(Alation, 2026).

---

## 출처

1. Zhamak Dehghani, "Data Mesh Principles and Logical Architecture," martinfowler.com, Dec 2020.
   https://martinfowler.com/articles/data-mesh-principles.html

2. Zhamak Dehghani, "How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh," martinfowler.com, May 2019.
   https://martinfowler.com/articles/data-monolith-to-mesh.html

3. Keith Schulze & Kunal Tiwary, "Shifting mindsets: why you should treat data as a product," Thoughtworks Modern Data Engineering Playbook.
   https://www.thoughtworks.com/insights/e-books/modern-data-engineering-playbook/data-as-a-product

4. Ammara Gafoor, Ian Murdoch & Kiran Prakash, "Data Mesh in practice: Product thinking and development (Part III)," Thoughtworks, May 2022.
   https://www.thoughtworks.com/insights/articles/data-mesh-in-practice-product-thinking-and-development

5. Jochen Christ, "What is a Data Product?," datamesh-manager.com / entropy-data.com.
   https://www.datamesh-manager.com/learn/what-is-a-data-product

6. datamesh-architecture.com — Data Mesh Architecture overview.
   https://www.datamesh-architecture.com/

7. Alation, "Data Products: The Foundation of AI-Ready Organizations," Feb 2026.
   https://www.alation.com/blog/data-products-ai-ready-organizations/

8. Xavier Gumara Rigol, "Data as a product vs data products. What are the differences?," Towards Data Science, Jul 2021.
   https://towardsdatascience.com/data-as-a-product-vs-data-products-what-are-the-differences-b43ddbb0f123/

9. Jean-Georges Perrin, "Data Product vs. Data Contract: What's the Difference?," Data Mesh Learning / Medium.
   https://medium.com/data-mesh-learning/data-product-vs-data-contract-whats-the-difference-d39e82cf8ed3

10. Andrea Gioia, "Data Contract vs. Data Product Specifications," Medium.
    https://medium.com/@andrea_gioia/data-contract-vs-data-product-specifications-8ffa3cc16725

11. Open Data Contract Standard / datacontract.com.
    https://datacontract.com/

12. Miruna Suru, "What Is Data Product Thinking?," Dataminded / Medium.
    https://medium.com/datamindedbe/what-is-data-product-thinking-and-why-5b58195b003b

13. Netflix Technology Blog, "Data as a Product: Applying a Product Mindset to Data at Netflix."
    https://netflixtechblog.medium.com/data-as-a-product-applying-a-product-mindset-to-data-at-netflix-4a4d1287a31d

14. Snowflake, "Are You Data Economy Ready? Start with Data Product Thinking."
    https://www.snowflake.com/en/blog/data-economy-ready-data-products/

15. Nexla, "A Guide to Achieving AI-Ready Data with Data Products."
    https://nexla.com/blog/unlocking-ai-potential-with-ai-ready-data-products/

16. arXiv, "The Data Hydration Gap: A Formal Model of Underinvestment in General-Purpose Data Products Under Decentralized Governance," 2604.00218.
    https://arxiv.org/pdf/2604.00218

17. Thoughtworks Technology Radar, "Data product thinking."
    https://www.thoughtworks.com/en-us/radar/techniques/data-product-thinking

18. dbt Labs, "The 4 principles of data mesh."
    https://www.getdbt.com/blog/the-four-principles-of-data-mesh
