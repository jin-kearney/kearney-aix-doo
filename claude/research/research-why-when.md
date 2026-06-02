# B-2 데이터 해설/주석 (Data Annotation & Labeling) — Why / When 리서치

> 담당 클러스터: 개념·구조 중심의 필요성 논증 및 적용 시점·조건

---

## 1. 개념 정의 및 프레이밍

본 주제에서 세 용어는 다음과 같이 층위를 구분한다.

| 용어 | 범위 | 역할 |
|------|------|------|
| **Annotation** (주석, micro) | 인스턴스 *내부* 특정 구간·위치·속성 | "어디에 무엇이 있는가" — localization, 구간 지정, 관계 표기 |
| **Label** (라벨, macro) | 인스턴스 *전체* | "이것이 무엇인가" — classification, 정답 범주 부여 |
| **해설 (Description/Commentary)** | 자유양식 설명 | 위 둘을 보조하는 맥락·근거 서술 |

Annotation은 텍스트의 개체명 구간(Named Entity span), 이미지의 바운딩 박스(bounding box)·세그멘테이션 마스크(segmentation mask), 영상의 특정 프레임 구간 등 데이터 *안에서* 의미 있는 부분을 특정한다. Label은 해당 이미지가 "고양이"인지, 해당 리뷰가 "긍정"인지처럼 인스턴스 전체에 하나의 범주를 부여한다. 두 작업은 상호 보완적이며, 많은 실전 파이프라인에서 동시에 수행된다(예: 객체 탐지 — 바운딩 박스 annotation + 클래스 label).

---

## 2. Why — 개념·구조·원리로 보는 필요성

### 2-1. 지도학습(Supervised Learning)에서 라벨/주석 = Ground Truth = 모델 성능의 상한

지도학습은 입력(X) → 정답(Y) 매핑을 데이터로부터 귀납하는 패러다임이다. 이때 Y가 바로 label(macro)과 annotation(micro)으로 구성된 **Ground Truth**다. Ground Truth란 모델 예측과 비교할 "검증된 정답 집합"을 의미하며, 모델 성능은 이 기준으로만 측정된다.

핵심 원리: **모델은 Ground Truth의 품질을 초과해 학습할 수 없다.** 즉, annotation/label의 정확성·완전성·일관성이 곧 모델이 도달할 수 있는 최고 성능의 천장(ceiling)을 결정한다. 이를 흔히 "Garbage In, Garbage Out(GIGO)"로 표현하며, 최신 연구(Label Convergence, arXiv 2409.09412)는 실제 데이터셋에서 주석 오류가 누적될 경우 이론적 성능 상한(label convergence)이 명시적으로 낮아짐을 실증했다. LVIS 데이터셋 분석에서 mAP 상한이 62.63~67.52 수준으로 수렴하는 현상이 관찰되었으며, 이는 모델 아키텍처가 아닌 **주석 품질이 병목**임을 의미한다.

> "The quality, coverage, and consistency of labeled data directly determines the upper bound of model performance." — SourceBae, Data Labeling & Annotation: The Complete Expert Guide (2026)

### 2-2. Annotation(micro) 부재 시 모델이 원리적으로 할 수 없는 것

Annotation은 "데이터 내부에서 어디를, 무엇을" 학습해야 하는지를 모델에 알려주는 신호다. 이것이 없으면:

- **Object Detection / Instance Segmentation**: 이미지 전체에서 "어느 픽셀 집합이 특정 객체인지" 모델이 알 수 없다. 바운딩 박스나 세그멘테이션 마스크 없이는 위치 예측 자체가 학습 불가능하다.
- **Named Entity Recognition (NER)**: 문장 내 어느 토큰 구간이 인명(PER)·기관명(ORG)인지 경계 표기가 없으면, 모델은 올바른 span을 추출하는 기준을 배울 수 없다.
- **시계열 이상탐지(Anomaly Detection, 지도 방식)**: 어느 구간이 이상인지 구간 annotation 없이는 이상 패턴을 학습할 수 없다.

요약: **micro annotation이 없으면 "localization(어디에)"이라는 학습 신호 자체가 존재하지 않는다.**

### 2-3. Label(macro) 부재 시 모델이 원리적으로 할 수 없는 것

Label은 인스턴스 전체의 범주·정답을 부여한다. 이것이 없으면:

- **분류(Classification)**: "이 이미지가 무엇인가"를 지도학습으로 배울 수 없다. 모델은 어떤 특징이 어떤 범주에 해당하는지 매핑 자체를 학습할 수 없다.
- **감성 분석(Sentiment Analysis)**: 텍스트가 "긍정/부정/중립" 중 어디에 속하는지 정답이 없으면, 해당 신호로 파라미터를 갱신할 방법이 없다.
- **품질 판정**: 제조 공정 데이터에서 "불량/양품" 라벨 없이는 불량 패턴을 지도학습으로 포착 불가하다.

요약: **macro label이 없으면 "classification(무엇인가)"이라는 학습 목표 자체가 정의되지 않는다.**

### 2-4. 일관성(Consistency) 없는 라벨이 학습을 직접 망가뜨리는 구조

라벨의 불일관성은 단순한 노이즈가 아니라 학습 과정의 구조적 오염이다.

1. **모순 신호 주입**: 같은 입력 패턴에 서로 다른 라벨이 붙으면, 경사하강법(gradient descent)이 모순된 방향으로 파라미터를 동시에 당겨 학습이 수렴하지 못하거나 잘못된 지점으로 수렴한다.
2. **노이즈 패턴 암기**: 모델이 실제 신호가 아닌 라벨 오류의 패턴을 "정답"으로 암기(overfitting)하는 현상이 발생한다.
3. **평가 지표 왜곡**: 테스트 셋에도 불일관 라벨이 포함되면, 모델 성능 평가 자체가 무의미해진다 — 좋은 모델이 나쁜 점수를, 나쁜 모델이 좋은 점수를 받을 수 있다.
4. **실증 규모**: Google Emotions 데이터셋의 30%, MS COCO 데이터셋 주석의 최대 37%가 오류 또는 불일관으로 분석된 바 있다.

불일관성의 원인: 주석자 간 해석 기준 상이, 가이드라인 모호성, 주석자 피로(annotator fatigue), 라벨 기준의 시간에 따른 변화(label drift) 등. 이를 측정하는 지표가 **Inter-Annotator Agreement (IAA)** — Cohen's κ, Fleiss' κ 등 — 이며, IAA가 낮을수록 라벨 일관성 위험이 높다.

### 2-5. 도메인 지식의 데이터화: 사람의 판단이 곧 학습 신호

AI 모델은 인간이 명시화한 판단 기준을 데이터 형태로 흡수해 일반화한다. 특히 의료(영상 판독), 법률(계약서 조항 분류), 산업(설비 이상 판단) 등 전문 도메인에서는 일반인이 아닌 **도메인 전문가(domain expert)**의 주석만이 유효한 Ground Truth가 된다. 전문가 주석(Expert Annotation)이 없는 경우, 모델은 해당 도메인에서 의미 있는 패턴을 학습할 기반 자체를 갖지 못한다.

---

## 3. Key Objectives — 왜 체계적 Annotation & Labeling 체계가 필요한가

아래 5개 목표는 위 원리에서 직접 도출된다.

| # | 목표 (동사형) | 연결 원리 |
|---|--------------|-----------|
| 1 | **학습 가능한 Ground Truth 확보** | 라벨/주석이 없으면 지도학습 자체 불가(§2-2, §2-3) |
| 2 | **사람 해석 기준의 일관화·재현성(Consistency & Reproducibility) 확보** | 불일관 라벨은 학습을 구조적으로 오염시킴(§2-4) |
| 3 | **도메인 지식의 데이터화(Domain Knowledge Encoding)** | 전문 판단을 학습 신호로 변환하는 유일한 경로(§2-5) |
| 4 | **모델 성능 상한(Performance Upper Bound) 제고** | Ground Truth 품질 = 성능 천장(§2-1) |
| 5 | **HITL(Human-In-The-Loop) 기반 검수 효율화** | IAA 측정 및 품질 게이트로 라벨 오류를 조기 차단(§2-4) |

---

## 4. When — 적용 시점·조건

### 4-1. Annotation / Label이 반드시 필요한 경우 (필수 조건)

| 조건 | 이유 |
|------|------|
| **비정형 데이터(Unstructured Data)** — 이미지, 영상, 음성, 자유형 텍스트, 센서 원시 신호 | 기계가 자동으로 의미를 해석할 수 있는 구조가 없어 사람의 의미 부여가 필수 |
| **지도학습(Supervised Learning) 및 파인튜닝(Fine-tuning)** | 입력→정답 매핑을 배우려면 반드시 정답(Y)이 존재해야 함 |
| **모델 평가(Evaluation) 및 벤치마킹** | 테스트 셋의 Ground Truth가 없으면 성능 수치 자체를 산출 불가 |
| **사람의 해석·판단이 개입하는 데이터** — 감성, 의도, 적합성, 이상 여부 | 기계적 규칙만으로는 정의 불가능한 범주 → 사람 라벨러 필수 |
| **도메인 전문 지식이 요구되는 데이터** — 의료 영상, 법률 문서, 산업 공정 신호 | 일반 규칙으로 자동화 불가, 전문가 판단만이 유효한 Ground Truth 생성 가능 |
| **Fine-grained Localization이 필요한 과제** — 객체 탐지, NER, 이상 구간 탐지 | micro annotation 없이는 위치 학습 신호 자체가 없음(§2-2) |

### 4-2. Annotation / Label이 조건부로 필요하거나 완화될 수 있는 경우

| 조건 | 설명 |
|------|------|
| **자기지도학습(Self-supervised Learning)** — BERT, GPT 류 사전학습 | 데이터 자체에서 pseudo-label을 생성해 표현(representation)을 학습. 단, 최종 태스크 적용 시 일부 fine-tuning용 라벨은 여전히 필요 |
| **반지도학습(Semi-supervised Learning)** | 소량의 labeled 데이터 + 대량의 unlabeled 데이터를 함께 활용. labeled 부분의 품질이 핵심 |
| **비지도학습(Unsupervised Learning)** — 군집화(clustering), 차원 축소 | 라벨 없이 패턴 탐색 가능. 단, 군집 결과의 *의미 해석*은 사람이 수행해야 함 |
| **규칙 기반 자동화 가능 데이터** | 명확한 규칙(예: 수치 임계값 초과 = 이상)이 존재하면 자동 라벨링 가능. 단 규칙 자체가 도메인 전문가 설계 필요 |

### 4-3. 도입 트리거 (Annotation 체계 구축 시작 신호)

1. **AI 모델 학습 프로젝트 착수 시점** — 지도학습 파이프라인 설계와 동시에 annotation 계획 수립
2. **기존 라벨 재사용 불가 판정 시점** — 도메인 전환, 클래스 재정의, 기준 변경으로 기존 데이터가 무효화될 때
3. **모델 성능 정체(plateau) 발생 시점** — 아키텍처 개선에도 성능이 오르지 않을 때 → 데이터 품질 병목 의심, annotation 감사(audit) 필요
4. **HITL 검수 결과 IAA 임계값 이하 탐지 시점** — 라벨 일관성 붕괴 감지, 즉각 가이드라인 재정비 필요
5. **새로운 데이터 유형·도메인 편입 시점** — 기존 annotation 스키마(schema)가 적용되지 않는 새 데이터 등장

---

## 5. 개념 구조 요약 (텍스트 다이어그램)

```
[원시 데이터 (Raw Data)]
        |
        | 사람의 판단·해석 개입
        v
[Annotation (micro)] ───> 위치·구간·속성 특정 (localization)
[Label (macro)]      ───> 전체 범주·정답 부여 (classification)
[해설 (Commentary)]  ───> 보조 맥락·근거
        |
        v
[Ground Truth Dataset]
        |
        | 지도학습 / 파인튜닝 / 평가
        v
[AI 모델]
        |
        | 성능 상한 = Ground Truth 품질
        v
[예측 결과 (Prediction)]
```

핵심 인과: Ground Truth 품질(정확성 × 일관성 × 완전성) → 학습 신호 품질 → 모델 성능 상한

---

## 출처

- [What Is Ground Truth in Machine Learning? | IBM](https://www.ibm.com/think/topics/ground-truth)
- [Do I Need to Build a Ground Truth Dataset? | Label Studio](https://labelstud.io/blog/do-i-need-to-build-a-ground-truth-dataset/)
- [Data Labeling & Annotation: The Complete Expert Guide (2026) — SourceBae](https://sourcebae.com/blog/data-labeling-annotation-guide/)
- [Label Convergence: Defining an Upper Performance Bound in Object Recognition through Contradictory Annotations — arXiv 2409.09412](https://arxiv.org/abs/2409.09412)
- [Data Annotation vs. Data Labeling: Explained — LabelVisor](https://www.labelvisor.com/data-annotation-vs-data-labeling-explained/)
- [Data Annotation vs Data Labeling — Toloka AI](https://toloka.ai/blog/annotation-vs-labeling/)
- [Data Labeling vs. Data Annotation — GigaBPO](https://gigabpo.com/data-labeling-vs-data-annotation/)
- [Label Noise Definition | OpenTrain AI Glossary](https://www.opentrain.ai/glossary/label-noise/)
- [Labeling Inconsistencies Impact on Model Evaluation — FutureBeeAI](https://www.futurebeeai.com/knowledge-hub/labeling-inconsistencies-model-evaluation)
- [The impact of inconsistent human annotations on AI driven clinical decision making — PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC9944930/)
- [Active label cleaning for improved dataset quality under resource constraints — PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC8897392/)
- [Human-in-the-Loop Machine Learning (HITL) Explained — Encord](https://encord.com/blog/human-in-the-loop-ai/)
- [Human-in-the-loop ML (HITL): When, where, how much, and how — Toloka AI](https://toloka.ai/blog/hitl-machine-learning/)
- [Expert Annotation Definition | OpenTrain AI Glossary](https://www.opentrain.ai/glossary/expert-annotation)
- [What Is Data Labeling? | IBM](https://www.ibm.com/think/topics/data-labeling)
- [Data Annotation: Types, tools, techniques, and use cases — LeewayHertz](https://www.leewayhertz.com/data-annotation/)
- [Semi Supervised Learning: 2026 Best Practices — Label Your Data](https://labelyourdata.com/articles/semi-supervised-learning)
- [Shifting to machine supervision: annotation-efficient semi and self-supervised learning — Nature Scientific Reports](https://www.nature.com/articles/s41598-024-61822-9)
