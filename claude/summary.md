# B-2 데이터 해설/주석 (Data Annotation & Labeling) — 스토리라인 요약 (콘텐츠 순서표)

> storyline.md를 펼치기 전 "무엇을, 어떤 순서로" 담는지 한눈에 보는 목차.
> 슬라이드 제목·핵심 메시지는 storyline.md와 일치한다(새 내용 창작 없음).

---

## 한 줄 정의

데이터 인스턴스의 일부(micro, Annotation)나 전체(macro, Label)에 인간 판단을 명시적 기호로 부착하여 AI 모델이 학습·평가할 Ground Truth를 생성하는 체계.

---

## 전체 흐름

Annotation(micro)이 없으면 "어디를", Label(macro)이 없으면 "무엇을" 학습해야 하는지 모델이 알 수 없다는 구조적 인과로 필요성을 설명한다(Why). 비정형 데이터·지도학습이 있는 모든 시점이 적용 조건이며 5가지 도입 트리거를 제시한다(When). Annotation/Label/해설 세 층위의 정의·유형(CV/NLP/시계열)·Label Taxonomy·Guideline·IAA·포맷 표준을 체계적으로 전개한다(What). 8단계 라이프사이클과 Phase별 구축 프레임워크, Workstream, AI 기반 해결책(Pre-labeling·HITL·Active Learning·Weak Supervision·Cleanlab), 솔루션 맵, 함정 8가지를 실행 가능한 수준으로 다룬다(How). IAA·LER·처리량·재작업률·AI 채택률 5개 KPI로 관리 체계를 정의하고(KPI), 단기·중기·장기 Data Flywheel 로드맵으로 마무리한다(Roadmap).

---

## 슬라이드 순서표

| # | 섹션 | 슬라이드 제목 | 한 줄 핵심 메시지 |
|---|------|--------------|-----------------|
| 1 | Executive Summary | Annotation & Labeling: AI 모델의 성능 상한을 결정하는 Ground Truth 체계 | Annotation(micro)과 Label(macro)이 없으면 지도학습 자체가 불가능하며, 그 품질이 곧 모델 성능의 천장이다 |
| 2 | Why | Ground Truth = 모델 성능의 상한 — annotation/label 품질이 병목이다 | 지도학습 모델은 Ground Truth의 품질을 초과해 학습할 수 없다 |
| 3 | Why | Annotation 없으면 "어디를", Label 없으면 "무엇을" — 두 층위가 각각 다른 학습 신호를 제공한다 | micro annotation이 없으면 localization 신호가 없고, macro label이 없으면 classification 목표가 정의되지 않는다 |
| 4 | Why | 불일관 라벨은 단순 노이즈가 아니라 학습을 구조적으로 오염시킨다 | 모순 신호를 받은 모델은 수렴하지 못하거나 오류 패턴을 정답으로 암기한다. IAA로 일관성을 정량화하고 선제 관리해야 한다 |
| 5 | Why | Key Objectives — Annotation·Labeling 체계가 달성하는 5가지 목표 | Ground Truth 확보·일관성·도메인 지식 데이터화·성능 상한 제고·HITL 효율화 5가지를 동시에 달성한다 |
| 6 | When | 비정형 데이터와 지도학습이 있는 곳이라면 — Annotation·Labeling은 선택이 아닌 필수다 | 이미지·텍스트·센서 데이터를 지도학습으로 다루는 모든 시점에 도입이 요구되며, 5가지 트리거가 착수 신호를 제공한다 |
| 7 | What | 세 층위 구분: Annotation(micro) / Label(macro) / 해설(Description) — 하나의 개념이 아니다 | Annotation은 "어디에 무엇이 있나(localization)", Label은 "전체가 무엇인가(classification)", 해설은 보조 자유 서술이다 |
| 8 | What | Annotation 유형 (이미지 CV): Bounding Box → Polygon → Semantic Mask → Keypoint | 정밀도와 비용이 비례한다. 과제(Detection/Segmentation/Pose)에 맞는 유형 선택이 핵심이다 |
| 9 | What | Annotation 유형 (텍스트 NLP): NER/Span·BIO·BIOES 태깅 스킴 | 텍스트 annotation의 핵심은 BIO/BIOES 태깅 스킴으로 토큰 구간 경계를 명확히 표기하는 것이다 |
| 10 | What | Label 체계 설계: Taxonomy — Flat vs. Hierarchical, 상호배타성, 다중라벨 | Taxonomy 3원칙(상호배타성·포괄성·일관성)과 과제 특성에 맞는 Flat/Hierarchical/Multi-label 구조를 선택한다 |
| 11 | What | Annotation Guideline & Gold Set: 일관성을 제도화하는 두 기둥 | Guideline(판단 기준 문서화)과 Gold Set(실물 기준점 제공)이 함께 있어야 IAA를 높이고 annotator drift를 방지한다 |
| 12 | What | IAA (Inter-Annotator Agreement): 라벨 일관성을 정량화하는 핵심 지표 | Cohen's κ·Fleiss' κ·Krippendorff's α 중 annotator 수와 데이터 유형에 맞는 지표를 선택하고, κ ≥ 0.8을 목표로 한다 |
| 13 | What | 표준 포맷: COCO / Pascal VOC / YOLO (이미지) vs. CoNLL BIO (텍스트) | Annotation 결과물은 표준 포맷으로 저장해야 ML 프레임워크와 호환된다 |
| 14 | What | 핵심 용어집: Annotation·Labeling 체계에서 반드시 알아야 할 12개 용어 | Ground Truth·Gold Set·IAA·Taxonomy 등 핵심 용어의 정확한 정의와 상호 관계가 체계 구축의 출발점이다 |
| 15 | How | 라벨링 프로젝트 라이프사이클 8단계: 정의 → 라벨링 → 검수 → 반복 | 대상 데이터 선정부터 데이터셋 버전 관리까지 8단계 각각의 산출물과 선행 조건을 명확히 해야 진입 여부를 판단할 수 있다 |
| 16 | How | 규모별 4-Phase 구축 프레임워크: Pilot → PoC → Growth → Scale | 라벨링 규모(500건 → 10만 건 → 100만 건+)에 따라 Phase별로 다른 접근이 필요하며, Phase가 올라갈수록 AI 자동화 비중이 높아진다 |
| 17 | How | 해야 할 작업 (Workstream): 데이터·프로세스·조직·기술 4개 축 | 라벨링 체계 구축은 기술 도구 선택만이 아니라 4개 축을 동시에 정비해야 한다 |
| 18 | How | Pain Point & AI 기반 해결 (1/2): Pre-labeling · HITL · Active Learning | 대량·고비용·일관성 부족·희소 클래스 문제는 Pre-labeling + HITL + Active Learning 3가지 조합으로 구조적으로 해소할 수 있다 |
| 19 | How | Pain Point & AI 기반 해결 (2/2): Weak Supervision · LLM Auto-labeling · Cleanlab | Snorkel은 수작업 라벨 총량을 줄이고, LLM은 텍스트 대규모 처리를 가능하게 하며, Cleanlab은 기존 라벨의 오류를 자동 탐지한다 |
| 20 | How | Tech Spec — 시장 솔루션/플레이어 맵 (12개) | 과제 유형·규모·온프레미스 여부·AI 자동화 수준에 따라 오픈소스와 상용 SaaS를 조합하는 것이 실용적이다 |
| 21 | How | 구축 시 고려사항·함정(Pitfall) 8가지 | 라벨링 프로젝트 실패의 대부분은 기술이 아닌 가이드라인 모호성·기준 편향·자동 라벨 과신·Gold Set 오염 같은 운영·설계 문제에서 온다 |
| 22 | KPI | Annotation·Labeling 관리 KPI 5가지 | IAA·LER·처리량/CPL·재작업률·AI Pre-label 채택률 5가지로 품질과 효율을 동시에 측정한다. 처리량 단독 추적의 허영 지표 위험을 경계해야 한다 |
| 23 | Roadmap | Annotation·Labeling 확산 로드맵: 단기 → 중기 → 장기 | 단기(Guideline·Gold Set 정립) → 중기(AI 보조·HITL 파이프라인) → 장기(Weak Supervision·Data Flywheel 자동화)로 단계적으로 확산한다 |

---

## 이 발표의 핵심 메시지 3가지

1. **Annotation(micro)과 Label(macro)은 구분된 학습 신호다** — micro가 없으면 "어디를", macro가 없으면 "무엇을" 학습해야 하는지 모델이 알 수 없다. 두 층위의 차이를 명확히 이해하고 과제에 맞게 설계해야 한다.
2. **Ground Truth 품질이 모델 성능의 상한이다** — 아키텍처 개선보다 Guideline + IAA + Gold Set 기반의 라벨 품질 관리가 먼저다. 라벨 오류는 숨어있고 누적되며, Cleanlab으로 자동 탐지할 수 있다.
3. **AI 보조(Pre-labeling·HITL·Active Learning·Weak Supervision)로 사람의 역할이 "창작자"에서 "검수자·규칙 설계자"로 전환된다** — 자동화율 극대화는 장기 Data Flywheel 구조로 완성된다.
