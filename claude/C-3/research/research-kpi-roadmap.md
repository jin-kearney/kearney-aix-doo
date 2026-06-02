# C-3 데이터 계통(Data Lineage) — KPI / Roadmap 리서치 원자료

> 작성일: 2026-06-02
> 담당 클러스터: KPI / 확산 로드맵
> 청중: 제조업 현업 실무 공유용

---

## 1. 데이터 리니지 성과 KPI — 5개 핵심 지표

### 1.1 KPI 요약 표

| # | KPI 명칭 | 정의 | 측정 방법 | 측정 목적 | 기대효과 / 벤치마크 |
|---|---|---|---|---|---|
| K-1 | **리니지 커버리지(Lineage Coverage)** | 전체 프로덕션 데이터 자산 중 리니지(출처~활용 경로)가 문서화된 비율 | (리니지 등록 자산 수 ÷ 전체 프로덕션 자산 수) × 100 | 리니지 체계의 전반적 완성도·공백 파악 | 성숙도 Stage 1 <20%, Stage 2: 20~50%, Stage 4(목표): 90%+ [source: https://atlan.com/know/data-lineage-best-practices/] |
| K-2 | **컬럼 단위 리니지 비율(Column-Level Lineage Ratio)** | 비즈니스 크리티컬 자산(CDE) 중 컬럼 단위 리니지가 확보된 비율 | (컬럼 단위 리니지 등록 CDE 수 ÷ 전체 CDE 수) × 100 | 정밀 영향 분석·AI 학습 데이터 추적 가능 여부 확인 | GDPR/SOX/AI Act 등 컴플라이언스 요건 충족에 필수. 컬럼 단위가 없으면 테이블 단위 리니지로는 감사 대응 불가 [source: https://atlan.com/know/data-lineage-best-practices/] |
| K-3 | **자동 캡처 비율(Automated Capture Rate)** | 전체 리니지 이력 중 코드 파싱·런타임·API 등 자동화 수단으로 기록된 비율(수동 입력 제외) | (자동 캡처 리니지 이벤트 수 ÷ 전체 리니지 이벤트 수) × 100 | 리니지 지도의 최신성(Freshness) 보장 여부 점검. 수동 기록은 변경 시 즉시 낡음 | 업데이트 지연 목표: 24시간 이내(프로덕션 자산). 수동 방식은 수주 내 스탈(stale)화 [source: https://atlan.com/know/data-lineage-best-practices/] |
| K-4 | **데이터 인시던트 MTTR(Mean Time to Resolution)** | 데이터 품질·파이프라인 문제 발생 시점부터 근본원인 파악 및 해결 완료까지 평균 소요 시간 | 인시던트 티켓 기록에서 오픈~클로즈 시간 계산 후 월/분기 평균. 리니지 활성화 전후 비교 | 리니지가 근본원인 추적 속도에 미치는 실질적 업무 단축 효과 측정 | Takealot: 자동화 리니지 도입 후 근본원인 분석 시간 **50% 단축** [source: https://atlan.com/data-lineage-explained/] / Aliaxis: 파이프라인 문제 탐지 **95% 빠르게**, 해결 시간 1일 → 1시간 [source: https://humansofdata.atlan.com/2023/12/aliaxis-worldwide-glossary/] |
| K-5 | **AI 답변 Citation 부착률(AI Answer Citation Rate)** | AI(RAG/Agent) 시스템이 생성한 답변 중 근거 출처(Citation)가 구조적으로 기록·제공된 비율 | (Citation 로그 포함 AI 답변 수 ÷ 전체 AI 답변 수) × 100 | AI 결과 신뢰성·감사 추적 가능성 확보. AI 답변의 리니지 구간(5~7단계) 커버 여부 측정 | 규제 대응(EU AI Act Article 11) 및 내부 신뢰성 보증에 필수. 목표: 핵심 AI 과제 100% Citation 로그 보유 [source: https://air-governance-framework.finos.org/mitigations/mi-13_providing-citations-and-source-traceability-for-ai-generated-information.html] |

---

### 1.2 KPI별 상세 해설

#### K-1. 리니지 커버리지 (Lineage Coverage)

**정의**: 조직이 운영하는 전체 프로덕션 데이터 자산(테이블·대시보드·AI 모델 등) 중 출처에서 활용까지 경로가 문서화된 비율.

**측정 방법**: 데이터 카탈로그 또는 리니지 도구에서 "리니지 그래프가 연결된 자산 수 ÷ 전체 자산 수 × 100"으로 산출. Monte Carlo 등 데이터 옵저버빌리티 플랫폼은 "Coverage Overview" 지표로 이를 자동 제공. [source: https://montecarlo.ai/blog-what-is-data-observability/]

**측정 목적**: 리니지 공백 구간 식별 및 우선순위화.

**기대효과**: Atlan의 4단계 성숙도 모델 기준, Stage 2(수동 문서화, 20~50%) → Stage 4(거버넌스 완성, 90%+ ) 진화가 목표. 커버리지 90% 이상일 때 규제 감사 대응 가능. [source: https://atlan.com/know/data-lineage-best-practices/]

---

#### K-2. 컬럼 단위 리니지 비율 (Column-Level Lineage Ratio)

**정의**: 핵심 데이터 요소(CDE, Critical Data Element) 중 개별 컬럼의 출처·파생 로직까지 추적 가능한 비율.

**측정 방법**: 데이터 카탈로그에서 컬럼 단위 리니지 등록 현황 집계. 제조업 맥락에서는 AI 예지보전 피처(feat_vib_rms_x 등), 품질 핵심 지표(불량률 계산 컬럼 등)를 CDE로 지정 후 우선 측정.

**측정 목적**: 테이블 단위 리니지는 "어떤 테이블이 연결됐는가"만 답하지만, 컬럼 단위는 "이 수치가 어떤 연산으로 만들어졌는가"까지 답한다. 정밀 영향 분석과 AI 학습 데이터 추적에 필수. [source: https://atlan.com/know/data-lineage-best-practices/]

**기대효과**: GDPR Article 30(개인정보 필드 추적), SOX 재무 계산 감사, EU AI Act Article 11(AI 훈련 데이터 출처 문서화)에서 컬럼 단위 리니지를 명시 또는 실질 요구. [source: https://atlan.com/know/data-lineage-best-practices/]

---

#### K-3. 자동 캡처 비율 (Automated Capture Rate)

**정의**: 전체 리니지 이력 기록 중 사람이 수동 입력하지 않고 코드 파싱·런타임 로그·플랫폼 API 등 자동화 수단으로 기록된 비율.

**측정 방법**: 리니지 이벤트 로그에서 캡처 출처(source) 필드 기준 분류. 예: dbt 파싱·Airflow OpenLineage 이벤트·Snowflake Access History → "자동"; 스튜어드 수동 입력 → "수동".

**측정 목적**: 수동 관리 리니지는 파이프라인 변경 시 수주 내 낡는다(stale). 자동 캡처율이 높을수록 리니지 지도의 신뢰도(Freshness)가 유지된다. [source: https://atlan.com/know/data-lineage-best-practices/]

**기대효과**: 자동화 리니지 업데이트 목표: 프로덕션 자산 24시간 이내, 고우선 자산 실시간~준실시간. 수동 의존도가 낮아질수록 운영 부담 감소. [source: https://atlan.com/know/data-lineage-best-practices/]

> 제조업 참고: OT(OPC-UA/MQTT 기반 historian) → IT 구간은 표준 SQL 파서로 자동 캡처가 어렵다. 이 구간의 "historian 태그 ID ↔ 웨어하우스 컬럼 매핑"은 별도 수동 관리 프로세스를 두되, 관리 현황 자체를 KPI로 추적한다.

---

#### K-4. 데이터 인시던트 MTTR (Mean Time to Resolution)

**정의**: 데이터 품질 이상·파이프라인 오류 등 데이터 인시던트 발생부터 근본원인 파악 및 수정 완료까지 평균 소요 시간.

**측정 방법**: 인시던트 트래킹 시스템(JIRA, ServiceNow 등)에서 "데이터 관련" 티켓의 오픈~클로즈 시간 집계. 리니지 도입 전(Baseline) 대비 도입 후 변화를 월별·분기별 추적.

**측정 목적**: 리니지가 실제 업무 시간을 얼마나 단축하는지 정량적으로 보여주는 대표 ROI 지표. 경영진 설득용으로 가장 효과적.

**정량 벤치마크**:
- Takealot(이커머스): 자동화 리니지 도입 → 근본원인 분석 시간 **50% 단축** [source: https://atlan.com/data-lineage-explained/]
- Aliaxis(제조업 배관 부품 제조): 파이프라인 문제 탐지 **95% 빠르게**, 해결 소요 **1일 → 1시간** 단축 [source: https://humansofdata.atlan.com/2023/12/aliaxis-worldwide-glossary/]
- Xometry(제조 플랫폼): 컬럼 단위 리니지 도입 → 근본원인 분석 시간 **"며칠" → "몇 시간"** 단축, 반복적 데이터 중단 제거 [source: https://www.selectstar.com/resources/data-governance-metrics-and-kpis]

---

#### K-5. AI 답변 Citation 부착률 (AI Answer Citation Rate)

**정의**: RAG/Agent 기반 AI가 생성한 답변 중 근거 출처(참조 문서·청크·테이블·Tool 호출 결과)가 구조적으로 기록·표시된 답변의 비율.

**측정 방법**: AI 답변 로그에서 "retrieved_chunks", "tables_used", "tool_calls" 등 근거 필드가 null이 아닌 답변 비율로 집계. 핵심 AI 과제(PdM, 품질 RAG 등)를 우선 측정.

**측정 목적**: 리니지의 AI 구간(⑤RAG 인덱스~⑦AI 답변)이 실제로 작동하는지 확인. Citation이 없는 AI 답변은 근거를 역추적할 수 없어 신뢰성·감사 대응 불가.

**기대효과**: EU AI Act Article 11은 고위험 AI 시스템의 훈련 데이터 출처 문서화를 명시 요구. FINOS AI Governance Framework는 "모든 AI 생성 정보에 출처 추적 가능성(Source Traceability)"을 미티게이션 요건으로 제시. 내부 목표: 핵심 AI 과제 Citation Rate **100%** 유지. [source: https://air-governance-framework.finos.org/mitigations/mi-13_providing-citations-and-source-traceability-for-ai-generated-information.html]

---

### 1.3 KPI 측정 기반 조건 (전제)

KPI를 측정하려면 다음이 선행돼야 한다:
1. **기준선(Baseline) 확보**: 도입 전 현황을 먼저 측정해야 개선 효과 증명 가능. [source: https://www.selectstar.com/resources/data-governance-metrics-and-kpis]
2. **자동 수집 인프라**: 수동 집계는 운영 부담이 크고 오류가 많다. 카탈로그·옵저버빌리티 툴에서 자동화.
3. **KPI와 비즈니스 목표 연결**: 단순 커버리지 수치가 아니라 "MTTR 30% 단축 → 설비 가동률 X% 향상" 같은 비즈니스 언어로 재표현해야 경영진 지지를 유지한다. [source: https://www.selectstar.com/resources/data-governance-metrics-and-kpis]

---

## 2. 확산 로드맵 — 파일럿에서 전사 자동화까지

### 2.1 단계 개요

핸드북 7.2의 6단계 로드맵(0. 준비 → 5. 활용 자동화)을 거버넌스 성숙도 모델과 연결하면 아래와 같다.

Atlan의 4단계 성숙도 모델 [source: https://atlan.com/know/data-lineage-best-practices/]과 Data-Pilot의 거버넌스 로드맵 [source: https://data-pilot.com/blog/data-governance-roadmap/]을 제조업 맥락에 적용:

| 단계 | 명칭 | 성숙도 수준 | 핵심 활동 | 주요 산출물 | 기간 감각 |
|---|---|---|---|---|---|
| **Phase 0** | 준비(Foundation) | Ad-hoc → Reactive (Stage 1) | 스튜어드 지정, CDE 선정, 현황 기준선(Baseline) 측정, 도구 선택 | CDE 목록, 스튜어드 배정표, KPI Baseline | 1~2개월 |
| **Phase 1** | 파일럿(Pilot) | Reactive (Stage 1) | AI 과제 1개의 원천→AI 답변 전 사슬 리니지 종단 적용. OT→IT 구간 수동 매핑 포함 | 단일 과제 종단 리니지 지도, 파일럿 MTTR 측정 | 2~3개월 |
| **Phase 2** | 핵심 흐름 확장(Core Expansion) | Passive (Stage 2) | 시스템 경계 우선 연결. 원천→전처리→분석셋→보고서 테이블 단위 리니지 확대. 커버리지 50% 목표 | 핵심 흐름 테이블 단위 리니지. Lineage Coverage 50%+ | 3~6개월 |
| **Phase 3** | 컬럼 단위 심화(Column-Level Deepening) | Active (Stage 3) | CDE·컴플라이언스 대상 자산에 컬럼 단위 리니지 적용. 영향 분석(Impact Analysis) 워크플로 활성화 | 컬럼 단위 리니지. Impact Analysis 알림 자동화. Column-Level Coverage 60%+ | 3~6개월 |
| **Phase 4** | AI 구간 적용(AI Lineage) | Active → Governed (Stage 3→4) | AI 답변 근거 로그(answer_id~retrieved_chunks) 스키마 설계·배포. Citation UI 구현. AI 과제 리니지 사슬 완성 | AI 답변 근거 로그, Citation 화면, AI Answer Citation Rate 100%(핵심 과제) | 2~4개월 |
| **Phase 5** | 전사 자동화(Enterprise Automation) | Governed (Stage 4) | 영향도 조회·Lineage Report 버튼화. 전체 리니지 자동 캡처 체계 완성. 커버리지 90%+ 목표 | 영향 분석·감사 리포트 자동화. Lineage Coverage 90%+. 자동 캡처율 80%+ | 6~12개월 |

---

### 2.2 단계별 핵심 활동 상세

#### Phase 0. 준비 (Foundation)

**목표**: 시작 전 최소 인프라와 데이터 확보.

주요 활동:
- **스튜어드 지정**: 각 데이터 도메인(생산·품질·설비 등)의 기술+비즈니스 담당자 지정.
- **CDE 선정**: AI 과제에 직결되거나 틀렸을 때 타격이 큰 데이터 요소 15~30개 선정.
- **현황 기준선 측정**: 현재 Lineage Coverage, MTTR, Citation Rate 등 KPI 초기값 기록.
- **도구 선택**: OpenLineage 호환 플랫폼(Atlan, Collibra, DataHub 등) 또는 사내 카탈로그 선택. [source: https://atlan.com/know/data-lineage-best-practices/]

산출물: CDE 목록(오너·갱신 주기 포함), 스튜어드 배정표, KPI Baseline 시트.

---

#### Phase 1. 파일럿 (Pilot)

**목표**: AI 과제 1개의 전 사슬(원천①~AI 답변⑦)을 끝-끝(End-to-End)으로 리니지 적용. "작게 시작해서 빠르게 가치 증명"이 핵심.

주요 활동:
- 파일럿 대상 선정: 예지보전(PdM) 또는 품질 RAG 등 영향이 크고 가시적인 AI 과제 1개.
- OT→IT 매핑 수동 작성: historian 태그 ID ↔ 웨어하우스 컬럼 매핑표 생성. 제조업 특유 난관 구간 [source: /Users/user/Documents/Claude/Projects/AI-ready Data 가이드 작성/C-3_데이터_리니지_가이드.md 7.3절].
- OpenLineage 기반 자동 캡처 시험: Airflow/dbt 파이프라인에 OpenLineage 방출(emit) 설정.
- 파일럿 MTTR 측정: 인시던트 발생 시 리니지 활용 전·후 소요 시간 비교.

산출물: 단일 과제 종단 리니지 지도, 파일럿 MTTR 측정 결과, OT→IT 매핑표 v1.

---

#### Phase 2. 핵심 흐름 확장 (Core Expansion)

**목표**: 시스템 경계를 먼저 잇는다. "시스템 내부 깊이"보다 "시스템 간 연결"이 우선. 사고는 경계에서 난다.

주요 활동:
- 핵심 데이터 흐름(생산실적→품질지표, 센서→PdM 피처 등)의 테이블 단위 리니지 연결.
- BI 도구(Power BI, Tableau 등) 리니지 연동 — 보고서 계층까지 연결.
- Lineage Coverage 모니터링 대시보드 구축.

성숙도 연결: Atlan Stage 2 "Passive" — 리니지가 문서화되지만 일상 워크플로에는 아직 미통합. 이 단계 완료 후 50%+ 커버리지 달성이 Stage 3 진입 전제. [source: https://atlan.com/know/data-lineage-best-practices/]

산출물: 핵심 흐름 테이블 단위 리니지(카탈로그에 노출), Lineage Coverage 대시보드.

---

#### Phase 3. 컬럼 단위 심화 (Column-Level Deepening)

**목표**: CDE와 컴플라이언스 대상 자산에 컬럼 단위 리니지 적용. 영향 분석 자동화 시작.

주요 활동:
- dbt·SQL 파싱으로 컬럼 단위 리니지 자동 추출.
- 변경 전 **영향도 조회 의무화**: 원천 스키마·단위·코드값 변경 시 하류 영향 자동 알림.
- 컬럼 단위 리니지 커버리지 측정 시작(Column-Level Ratio KPI 가동).

벤치마크 참고: Aliaxis는 이 단계에서 컬럼 단위 리니지 자동화 후 문제 탐지 **95% 빠르게**, 해결 시간 **1일 → 1시간**으로 단축. [source: https://humansofdata.atlan.com/2023/12/aliaxis-worldwide-glossary/]

성숙도 연결: Stage 3 "Active" — 리니지가 일상 워크플로(변경 전 영향 조회, 품질 알림)에 자동 노출. [source: https://atlan.com/know/data-lineage-best-practices/]

산출물: 컬럼 단위 리니지(CDE 대상), 영향 분석 자동 알림, Column-Level Coverage KPI.

---

#### Phase 4. AI 구간 적용 (AI Lineage)

**목표**: RAG/Agent AI 답변의 근거 역추적 체계 완성. 리니지 사슬 ⑤~⑦ 구간 커버.

주요 활동:
- AI 답변 근거 로그 스키마 설계: `answer_id`, `retrieved_chunks`, `tables_used`, `tool_calls`, `prompt_version`, `model_version` 등.
- Citation UI 구현: 답변 화면에 근거 출처 표시.
- AI Answer Citation Rate KPI 가동(핵심 AI 과제 100% 목표).

설계 원칙: "나중에 붙이려면 거의 불가능" — AI 과제 설계 초기부터 요구사항에 포함. [source: /Users/user/Documents/Claude/Projects/AI-ready Data 가이드 작성/C-3_데이터_리니지_가이드.md 4.3절]

규제 연결: EU AI Act Article 11 고위험 AI 시스템 기술 문서화 요건. FINOS AI Governance Framework 출처 추적성 요건. [source: https://air-governance-framework.finos.org/mitigations/mi-13_providing-citations-and-source-traceability-for-ai-generated-information.html]

산출물: AI 답변 근거 로그 스키마, Citation 화면, AI Answer Citation Rate 대시보드.

---

#### Phase 5. 전사 자동화 (Enterprise Automation)

**목표**: 커버리지 90%+, 자동 캡처율 80%+, Lineage Report 버튼 한 번으로 출력.

주요 활동:
- Lineage Report 자동 출력 기능(감사·증빙용): 핵심 자산·AI 과제별 원클릭 리포트.
- 전사 프로덕션 자산으로 리니지 확대.
- 리니지 "제품처럼 운영": 오너·갱신 주기·점검 주기를 정기 거버넌스 회의에 포함.
- 리니지 지도를 카탈로그 검색·자산 화면에 상시 노출(평소에도 보이게).

성숙도 연결: Atlan Stage 4 "Governed" — 규제 감사 대응, 정책 집행, 의사결정 추적 가능. [source: https://atlan.com/know/data-lineage-best-practices/]

Gartner 추정: 데이터 품질 문제로 인한 연간 손실 평균 **$12.9M**. 리니지 기반 거버넌스 강화로 이 비용의 일부를 직접 절감 가능. [source: https://www.selectstar.com/resources/data-governance-metrics-and-kpis]

산출물: 영향 분석·감사 리포트 자동화, Lineage Coverage 90%+, 자동 캡처율 80%+ 달성.

---

### 2.3 거버넌스 성숙도 모델 연결 요약

```
Phase 0+1  →  Stage 1 (Reactive):   리니지 있지만 문제 날 때만 조회. 커버리지 <20%
Phase 2    →  Stage 2 (Passive):    문서화됐지만 일상 워크플로 미통합. 커버리지 20~50%
Phase 3    →  Stage 3 (Active):     일상 변경·알림에 자동 노출. 커버리지 50~85%
Phase 4+5  →  Stage 4 (Governed):   규제 대응·AI 추적·의사결정 추적 가능. 커버리지 90%+
```
[source: https://atlan.com/know/data-lineage-best-practices/]

---

### 2.4 로드맵 리스크와 제조업 특이사항

1. **OT→IT 경계**: 산업 프로토콜(OPC-UA, MQTT, historian) 구간은 표준 SQL 파서로 자동 캡처 불가. Phase 1에서 수동 매핑으로 시작해 Phase 3~5에서 전용 OT 커넥터 또는 historian API 연동으로 자동화 추진.

2. **"시스템 경계 먼저" 원칙**: 한 시스템 내부만 깊게 파지 말고, 시스템 간 연결을 먼저 확보. 최악의 데이터 사고는 경계에서 발생. [source: /Users/user/Documents/Claude/Projects/AI-ready Data 가이드 작성/C-3_데이터_리니지_가이드.md 7.2절]

3. **낡은 리니지는 "없느니만 못함"**: 수동 관리 의존도가 높으면 파이프라인 변경 직후 지도가 거짓말이 된다. 자동 캡처율 KPI(K-3)를 주기적으로 점검.

4. **ROI 조기 가시화**: Phase 1 파일럿에서 MTTR 단축 수치를 즉시 측정·공유해 경영진 지지 확보. 69%의 데이터·AI 리더가 ROI 증명에 어려움을 겪는다. [source: https://atlan.com/data-lineage-explained/]

---

## 참고 출처

| # | 출처 | URL |
|---|---|---|
| 1 | Atlan — Data Lineage Best Practices: A Maturity Framework | https://atlan.com/know/data-lineage-best-practices/ |
| 2 | Atlan — What Is Data Lineage? How It Works, Benefits & Tools 2026 | https://atlan.com/data-lineage-explained/ |
| 3 | Atlan — How Data Lineage & Impact Analysis Work | https://atlan.com/know/data-lineage-impact-analysis/ |
| 4 | Atlan — Automated Data Lineage | https://atlan.com/automated-data-lineage/ |
| 5 | Humans of Data (Atlan) — Aliaxis Case Study | https://humansofdata.atlan.com/2023/12/aliaxis-worldwide-glossary/ |
| 6 | Select Star — Defining Data Governance Metrics and KPIs | https://www.selectstar.com/resources/data-governance-metrics-and-kpis |
| 7 | Select Star — Xometry Case Study (Column-Level Lineage) | https://www.selectstar.com/case-studies/how-xometry-uses-select-stars-column-level-lineage-to-eliminate-data-outages |
| 8 | KPI Depot — Data Lineage Accuracy KPI | https://kpidepot.com/kpi/data-lineage-accuracy |
| 9 | Monte Carlo — What Is Data Observability? | https://montecarlo.ai/blog-what-is-data-observability/ |
| 10 | Monte Carlo — The Ultimate Guide To Data Lineage | https://montecarlo.ai/blog-data-lineage/ |
| 11 | FINOS AI Governance Framework — Citation & Source Traceability | https://air-governance-framework.finos.org/mitigations/mi-13_providing-citations-and-source-traceability-for-ai-generated-information.html |
| 12 | Alation — 7 Benefits of Data Lineage | https://www.alation.com/blog/7-benefits-of-data-lineage/ |
| 13 | Alation — 4 Pillars of Data Lineage | https://www.alation.com/blog/data-lineage-pillars-end-to-end-journey/ |
| 14 | Atlan — Data Governance Maturity Model | https://atlan.com/data-governance-maturity-model/ |
| 15 | Data-Pilot — Data Governance Roadmap | https://data-pilot.com/blog/data-governance-roadmap/ |
| 16 | OpenLineage 공식 사이트 | https://openlineage.io/ |
| 17 | OWOX — Data Lineage, Profiles & Quality Metrics Guide 2025 | https://www.owox.com/blog/articles/data-lineage |
| 18 | Buzzclan — Data Lineage Tracking: Why It's Essential in 2026 | https://buzzclan.com/data-engineering/data-lineage/ |

---

*리서치 완료: 2026-06-02. 다음 단계: 이 원자료를 바탕으로 가이드 KPI/Roadmap 섹션 작성.*
