---
tags: [ai-ready-data, B-2, annotation, labeling, KPI, roadmap, 장표]
created: 2026-06-10
related: "[[storyline]]"
---

# B-2 데이터 해설/주석 — KPI · Roadmap 장표안 (각 1장)

---

## [장표 1] KPI: 데이터 해설·주석 관리 지표

**거버닝 메시지:** Annotation·Labeling 체계의 건강성은 품질(IAA·오류율)과 효율(처리량·자동화율)을 함께 측정해야 하며, 처리량 단독 추적은 허영 지표(Vanity Metric)가 되므로 품질 지표와 반드시 병행 관리함

### KPI 5가지

| # | KPI | 정의 (산식) | 목표 | 측정 주기 | 연계 프로세스 단계 |
|---|-----|------------|------|----------|------------------|
| 1 | **IAA (라벨 일관성)** | Cohen's κ = (Po−Pe)/(1−Pe), 3인 이상은 Fleiss' κ / Krippendorff's α | **κ ≥ 0.80** (κ < 0.6 시 가이드라인 재작성 트리거) | Pilot 시 + 월 1회 Calibration | Pilot 라벨링, IAA 측정·보정 |
| 2 | **라벨 오류율 (LER)** | 오류 라벨 수 ÷ 검수 라벨 수 × 100% (Gold Set 대비) | **≤ 5%** (초기 ~8% → 성숙기 3~4%) | QA 검수 시 상시 | QA 및 검수 |
| 3 | **처리량 / 건당 비용** | Throughput = 완료 건수/시간, CPL = 총비용 ÷ 완료 건수 | 수작업 대비 **2~4배** (AI 보조 도입 후) | 주간 | 본 라벨링 |
| 4 | **QA 통과율 / 재작업률** | 통과 건수 ÷ 검수 건수 / 재작업 건수 ÷ 완료 건수 | Pass **≥ 95%** / Rework **≤ 4%** | 배치 단위 | QA 및 검수 |
| 5 | **AI Pre-label 채택률 (ALAR)** | 수정 없이 승인된 AI 라벨 ÷ AI 생성 라벨 × 100% | **≥ 75%** (성숙기 90%+) | 월간 | 자동 라벨링·HITL 검수 |

### 운영 원칙 (하단 박스)

- **품질 우선**: IAA·LER이 기준 미달이면 처리량 확대 중단 → 가이드라인 보정 선행
- **선행 지표 활용**: QA 통과율 하락은 가이드라인 모호·Taxonomy 결함의 조기 신호
- **자동화 피드백**: ALAR 저조 클래스는 Active Learning 우선 샘플링 대상으로 연동

[도식 설명: 5개 KPI를 카드형으로 배치하고, 각 카드에 목표 수치를 대형 숫자로 강조. 하단에 "품질 지표(1·2·4) ↔ 효율 지표(3·5)" 균형을 저울 아이콘으로 표시]

---

## [장표 2] Roadmap: 데이터 해설·주석 체계 확산 로드맵

**거버닝 메시지:** 단기에 가이드라인·Gold Set으로 신뢰 기준선을 확립하고, 중기에 AI 보조·HITL 파이프라인으로 처리량을 확장하며, 장기에 Weak Supervision·Data Flywheel로 자동화율을 극대화함

| 구분 | **단기 (0~6개월): 기반 정립** | **중기 (6개월~2년): AI 보조 효율화** | **장기 (2년~): 자동화 고도화** |
|------|------|------|------|
| 핵심 목표 | 라벨 체계 신뢰 기준선(Baseline) 확립 | AI Pre-labeling·HITL로 처리량 확대 | 자동화율 극대화 + 자기강화 루프 |
| 주요 활동 | • Annotation Guideline v1 + Taxonomy 확정<br>• Gold Set 구축 (전문가 검증 200~500건)<br>• Pilot 라벨링 + IAA 기준선 측정<br>• Annotator 교육·샘플링 검수(5~10%) | • AI Pre-labeling + Confidence 기반 라우팅<br>• Active Learning (불확실성 샘플 우선)<br>• 라벨 오류 자동 탐지 (Cleanlab)<br>• 데이터셋 버전 관리 (DVC·lakeFS) | • Weak Supervision (Snorkel) 대량 생성<br>• Foundation Model 라벨링 (SAM 2, GPT-4o)<br>• 합성 데이터로 희소 클래스 보강<br>• Data Flywheel: 운영 데이터 → 재학습 루프 |
| 도입 기술 | Label Studio / CVAT / Labelbox + 기본 대시보드 | Active Learning 프레임워크, Cleanlab, DVC/lakeFS | Snorkel Flow, SAM 2, 멀티모달 LLM, MLOps 파이프라인 |
| 목표 KPI | κ ≥ 0.7 / LER ≤ 8% | κ ≥ 0.8 / LER ≤ 5% / ALAR ≥ 75% / 처리량 2~4배 | ALAR ≥ 90% (고신뢰 자동 승인) |
| 연계 주제 | [[데이터 카탈로그]] (주석 대상 선정) | [[데이터 품질]] (오류 탐지 통합) | [[E-3 평가셋]] · [[데이터 거버넌스]] |

[도식 설명: 3단계 수평 타임라인 위에 단계별 박스 배치. 우측 또는 하단에 "수작업 비율 ↓ / AI 자동화 비율 ↑" 스택 막대 차트를 병치하여 단계별 자동화 전환을 시각화]
