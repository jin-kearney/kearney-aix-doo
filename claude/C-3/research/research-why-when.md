# C-3 데이터 계통(Data Lineage) — Why / When 리서치

> 담당 클러스터: Why(필요성) / Key Objectives(핵심 목표) / When(적용 시점)
> 작성일: 2026-06-02 | 청중: 제조업 현업 실무자

---

## 1. Why — 리니지가 없으면 생기는 문제

### 1.1 데이터 신뢰 붕괴와 비용

조직의 67%는 2024년 말 기준으로 의사결정에 사용하는 데이터를 완전히 신뢰하지 못한다고 밝혔으며, 이는 전년도 55%에서 증가한 수치다. 불정확하거나 추적 불가능한 데이터는 기업에 연평균 **1,290만 달러**의 비용을 발생시킨다(낭비된 자원·기회 손실·평판 손상 포함). [source: https://www.decube.io/post/data-lineage-concepts]

리니지가 없는 조직이 겪는 핵심 문제는 다음과 같다:
- **데이터 손실·중복·불일치**: 출처를 거슬러 올라갈 수 없어 오류 복구가 어렵고, 중복 저장으로 분석 오류가 발생한다. [source: https://www.secoda.co/blog/risks-of-neglecting-data-lineage]
- **조용한 데이터 부패(Silent Corruption)**: 변환 이력이 없으면 데이터가 언제부터 오염되었는지 파악하지 못한 채 AI 모델이 계속 오염된 값을 학습한다. [source: https://www.secoda.co/blog/risks-of-neglecting-data-lineage]
- **영향 분석 불가**: 원천 스키마·단위·코드값을 변경할 때, 하류에 어떤 보고서·모델·프롬프트가 영향을 받는지 파악하지 못해 연쇄 장애가 발생한다. [source: https://montecarlo.ai/blog-data-lineage/]
- **디버깅·인시던트 해소 지연**: 리니지가 있는 경우 근본원인 분석 시간이 약 95% 단축된다는 사례가 보고되었다(글로벌 파이프 제조사 Aliaxis: "컬럼 문제 추적이 1시간 → 수 분으로 단축"). [source: https://atlan.com/gartner-data-lineage/]
- **데이터 부채(Data Debt) 누적**: ML 파이프라인에서 가장 큰 비용은 모델 성능 저하가 아니라 **업스트림 데이터 문제**에서 발생한다. 리니지가 없으면 어디서 부채가 발생했는지 모르는 채로 계속 쌓인다. [source: https://datahub.com/blog/data-lineage-for-ml/]

### 1.2 AI·LLM 시대에 리니지 중요성이 커진 이유

#### RAG 환각(Hallucination) 추적 불가

RAG(Retrieval-Augmented Generation) 기반 AI는 검색된 문서 청크를 근거로 답변을 생성한다. 그러나 리니지가 없으면 AI가 **오래된 문서, 오염된 청크, 잘못된 인덱스**를 근거로 답했을 때 그 원인을 역추적할 수 없다. 모델이 정확한 소스를 잘못 해석하거나, 상충하는 증거를 적절한 귀속 없이 합치거나, 검색된 문서에 답이 없는데도 인정하지 못하는 경우가 발생한다. [source: https://www.k2view.com/blog/rag-hallucination/]

**답변 리니지**(어떤 청크·테이블·Tool 호출이 이 답변의 근거가 되었는가)는 AI 시대에 새롭게 요구되는 리니지 구간으로, 전통적 데이터 리니지(원천→보고서)와 구별된다.

#### AI 거버넌스와 설명가능성(Explainability)

Gartner의 2025년 연구는 다음을 명시한다: "AI 및 생성형 AI는 메타데이터 요구사항을 주도하는 가장 중요한 힘이다. AI 알고리즘은 신뢰할 수 있는 결과를 생성하기 위해 명확한 데이터 의미(Semantics)와 **리니지**를 필요로 한다." AI 시스템이 통찰이나 결정을 생성할 때, 리니지는 어떤 데이터가 산출물에 기여했으며 어떻게 변환되었는지를 보여주는 **신뢰 레이어(Trust Layer)**다. [source: https://atlan.com/gartner-data-lineage/]

메타데이터 분석을 활용하는 조직은 Gartner 2025년 전망 기준 신규 데이터 자산을 최대 **70% 빠르게** 제공할 수 있다. [source: https://atlan.com/gartner-data-lineage/]

#### EU AI Act — 리니지가 법적 의무가 되는 맥락

EU AI Act (Regulation (EU) 2024/1689) Article 10은 고위험 AI 시스템의 훈련 데이터셋에 대해 다음 다섯 가지 구체적 요건을 부과한다:
1. 대상 모집단 대비 **대표성 있고 관련성 있는** 데이터
2. 데이터 품질 문제를 탐지·측정·해소하는 **운영 중인 프로세스**
3. 적절한 통계 특성(완전성·분포) 증명
4. **편향(Bias) 검사** 수행 및 문서화
5. 훈련 데이터의 **출처, 변환, 각 파이프라인 단계의 품질 상태**를 담은 감사 기록

특히 의료, 신용평가, 제조 안전 등 고위험 영역의 AI 시스템에 대해 **2026년 8월 2일부터** 전면 시행된다. 정책 문서가 아니라 **파이프라인 레벨의 타임스탬프된 증거**가 없으면 감사를 통과할 수 없다. [source: https://www.ataccama.com/blog/real-time-data-observability-is-the-missing-layer-in-eu-ai-act-compliance]

Medium의 데이터 거버넌스 전문가 분석: "GDPR이 데이터 거버넌스를 가르쳤다면, AI Act는 **데이터 리니지를 요구**한다. 이 둘의 차이가 바로 의도(Intention)와 증거(Evidence)의 차이다." [source: https://medium.com/@pulsr-io-enrico/gdpr-taught-us-data-governance-the-ai-act-demands-data-lineage-heres-the-difference-eb3c3466f324]

#### 규제 리스크 현황

- Gartner: 2027년까지 데이터 거버넌스 이니셔티브의 **80%가 실패**할 것이며, 이는 추적성·투명성·규정 준수 준비 같은 비즈니스 성과와 연결되지 못한 경우 해당한다. [source: https://www.secoda.co/blog/risks-of-neglecting-data-lineage]
- By 2026년, 대형 기업의 **60%**가 규제·운영 리스크 대응을 위해 데이터 리니지 도구를 도입할 전망(2023년 20%에서 급증). [source: https://atlan.com/gartner-data-lineage/]

---

## 2. Key Objectives — 리니지 구축의 핵심 목표

리니지를 구축하면 아래 다섯 가지 핵심 목표를 달성한다:

| # | 목표 | 설명 |
|---|---|---|
| 1 | **추적성(Traceability)** | 데이터의 출처부터 최종 사용처(보고서·AI 답변)까지 완전한 이력 추적. "이 숫자 어디서 왔나?" 즉답 가능 |
| 2 | **영향 분석(Impact Analysis)** | 원천 데이터·변환 로직 변경 전, 영향받는 하류 보고서·AI 모델·프롬프트 목록 사전 파악. 변경 리스크 통제 |
| 3 | **AI 설명가능성(Explainability)** | AI 답변마다 근거 출처(문서·청크·테이블·Tool)를 역추적해 "왜 이렇게 답했는가"를 설명. 환각 발생 시 원인 색출 |
| 4 | **감사 대응(Audit Readiness)** | 데이터 출처·변환 이력·접근 이력·사용 목적을 즉시 출력. EU AI Act, 내부 감사, 규제기관 점검에 "의도"가 아닌 "증거"로 대응 |
| 5 | **데이터 신뢰(Data Trust)** | 모든 데이터 소비자(현업·분석가·AI)가 "이 데이터를 믿을 수 있는가"에 즉시 답할 수 있는 신뢰 기반 제공 |

> **제조 맥락 예시**: 진동 센서 교체 시(단위 변경) — 리니지가 없으면 예지보전 모델이 조용히 틀어진다. 리니지가 있으면 센서 ID 하나에서 하류 모델·대시보드·프롬프트 오너 전체에게 사전 공지가 즉시 가능하다.

---

## 3. When — 언제 리니지를 도입·확대해야 하는가

### 3.1 도입 트리거 (언제 시작할 것인가)

다음 상황 중 **하나라도 해당되면** 리니지 구축을 즉시 시작해야 한다:

| 트리거 | 구체적 신호 | 이유 |
|---|---|---|
| **AI 과제 착수** | 예지보전(PdM), 품질 RAG, 공정 최적화 AI 등 첫 번째 AI 프로젝트 시작 | AI 답변 근거 로그는 **처음부터** 설계해야 함. 나중에 붙이기 불가 |
| **데이터 스택 복잡화** | 5개 이상 소스·테이블이 연결되거나, 도메인팀 간 데이터 재사용 시작 | 5개 테이블 수작업 추적과 100개 테이블 추적은 차원이 다름 [source: https://www.ovaledge.com/blog/data-lineage-best-practices] |
| **반복 인시던트 발생** | "이 숫자 왜 이렇게 나왔지?" 질문에 며칠씩 소요 | 데이터 인시던트 해소 지연은 리니지 부재의 가장 명확한 징후 |
| **규제·감사 대응** | ISO, 품질 인증, EU AI Act, 고객사 감사 등 | Article 10 전면 시행(2026.8) 이전 파이프라인 레벨 증거 필수 [source: https://www.ataccama.com/blog/real-time-data-observability-is-the-missing-layer-in-eu-ai-act-compliance] |
| **원천 변경 빈발** | 센서 교체·단위 변경·MES 업그레이드 등이 잦을 때 | 변경 전 영향도 파악 없이 진행 시 연쇄 장애 리스크 |

### 3.2 성숙도 단계별 적용 시점

Gartner의 메타데이터 관리 성숙도 모델에 따르면, 리니지는 **Level 2(카탈로그 수준)**에서 체계적으로 시작되며 Level 3~4로 갈수록 운영 자동화 수준이 높아진다. [source: https://atlan.com/gartner-data-lineage/]

| 성숙도 단계 | 조직 특성 | 리니지 적용 수준 | 권장 시작점 |
|---|---|---|---|
| **Level 1 — 반응적(Reactive)** | 인시던트 발생 후 수작업 대응. 데이터 경로 기억에 의존 | 없거나 산발적 문서 | 핵심 AI 과제 1개의 ①~⑦ 사슬을 화이트보드로 그리기 |
| **Level 2 — 카탈로그(Catalog)** | 데이터 카탈로그 도입, 데이터 흐름 체계적 추적 시작 | 테이블 단위 자동 캡처, 핵심 흐름 우선 | OpenLineage 기반 파이프라인 런타임 캡처 도입 |
| **Level 3 — 관리(Managed)** | 거버넌스 정책 기업 전체 조율, 컬럼 단위 리니지 | 컬럼 단위, AI 답변 근거 로그, 자동 영향 분석 | AI 서비스 전체에 Citation 구조 적용 |
| **Level 4 — 최적화(Optimized)** | 리니지가 일상 워크플로우에 내재화, 자동 알림·정책 시행 | 실시간 갱신, 자동 알림, 감사 리포트 버튼 한 번 | 리니지 리포트 즉시 출력, 변경 전 영향도 조회 의무화 |

### 3.3 도입 우선순위 원칙 (한정된 자원 배분)

모든 데이터에 동시에 리니지를 걸 필요는 없다. **핵심 데이터 요소(CDE, Critical Data Element)** 기준으로 우선 적용한다:

1. AI 과제(PdM, 품질 RAG 등)에 **실제로 쓰이는 흐름**
2. 틀리면 안전·품질·비용에 **타격이 큰 데이터**
3. **여러 부서가 반복 재사용**하는 데이터
4. **규제 대상**이거나 감사 빈도가 높은 데이터

> "While some data teams with relatively small environments can survive by manually parsing data when anomalies are flagged, manually tracing the lineage of 5 tables with 7 data sources isn't the same as manually tracing 100 tables across 50 data sources consumed by multiple domain teams." [source: https://www.montecarlodata.com/blog-data-quality-maturity-curve/]

### 3.4 제조 현장 특화 — OT→IT 경계에서의 도입 시점

제조업의 리니지 공백은 특히 **센서(PLC/SCADA) → MES → 데이터 웨어하우스** 경계에 집중된다. 이 구간은 일반 SQL 파서로 자동 추적이 어렵고, 기존 도구 대부분이 IT/클라우드 스택을 전제로 설계되어 있다.

**OT→IT 리니지를 즉시 시작해야 하는 신호**:
- 센서 태그(historian tag) ID와 웨어하우스 컬럼 간 매핑이 누군가의 기억에만 있을 때
- 센서 교정·단위 변경이 AI 모델 성능 저하로 이어졌는데 원인을 사후에야 파악했을 때
- MES 업그레이드 후 분석 데이터셋이 조용히 틀어진 경험이 있을 때

초기에는 수동으로라도 **"historian 태그 ID ↔ 웨어하우스 컬럼 매핑표 + 센서 교정·단위 변경 이력"**을 오너·갱신 주기가 있는 메타데이터 산출물로 관리하는 것이 현실적인 시작점이다.

---

## 참고 출처

1. Atlan — Gartner Data Lineage: Research, Trends & Tools for 2026: https://atlan.com/gartner-data-lineage/
2. Secoda — What are the risks of neglecting data lineage in data management?: https://www.secoda.co/blog/risks-of-neglecting-data-lineage
3. Ataccama — EU AI Act Article 10 Explained: Data Quality, Lineage, and Pipeline Evidence: https://www.ataccama.com/blog/real-time-data-observability-is-the-missing-layer-in-eu-ai-act-compliance
4. Monte Carlo — The Ultimate Guide To Data Lineage: https://montecarlo.ai/blog-data-lineage/
5. Monte Carlo — Data Quality Maturity Curve: https://www.montecarlodata.com/blog-data-quality-maturity-curve/
6. Atlan — EU AI Act Compliance: Everything You Need Before August 2026: https://atlan.com/know/eu-ai-act-compliance/
7. Medium / Pulsr.io — GDPR taught us data governance. The AI Act demands data lineage: https://medium.com/@pulsr-io-enrico/gdpr-taught-us-data-governance-the-ai-act-demands-data-lineage-heres-the-difference-eb3c3466f324
8. Ovaledge — Data Lineage Best Practices for 2026: https://www.ovaledge.com/blog/data-lineage-best-practices
9. Decube — Data Lineage: Concepts, Types, and Why It's the Foundation of AI-Ready Data: https://www.decube.io/post/data-lineage-concepts
10. DataHub — Why Data Lineage is Non-Negotiable for Reliable ML: https://datahub.com/blog/data-lineage-for-ml/
11. k2view — RAG hallucination: What is it and how to avoid it: https://www.k2view.com/blog/rag-hallucination/
12. Atlan — LLM Training Data Lineage: Provenance, Tracking & Compliance: https://atlan.com/know/training-data-lineage-for-llms/
13. Quinnox — Data Governance for AI in 2025: Challenges, Best Practices and Solutions: https://www.quinnox.com/blogs/data-governance-for-ai/
14. Foundational.io — 2025 Data Governance Recap and 2026 AI Governance Outlook: https://www.foundational.io/blog/2025-data-governance-recap-2026-ai-governance-outlook
