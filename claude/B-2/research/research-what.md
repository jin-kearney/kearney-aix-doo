# B-2 데이터 해설/주석 (Data Annotation & Labeling) — What 클러스터 리서치

> 담당 클러스터: What(핵심) — 정의/구성요소/표준/동작원리/유형/용어
> 작성일: 2026-06-02

---

## 1. Annotation vs Label vs Description — 정확한 정의와 경계

### 1.1 개념의 세 층위 (Micro / Macro / Free-form)

| 층위 | 용어 | 단위 | 설명 |
|------|------|------|------|
| Micro | Annotation (주석) | 데이터 인스턴스의 **일부** (영역·픽셀·토큰·구간) | 이미지 내 바운딩박스, 텍스트 내 엔티티 구간, 시계열의 이상 구간 등 하위 요소에 의미를 표시 |
| Macro | Label (라벨) | 데이터 인스턴스 **전체** (이미지 한 장, 문장/문서 하나, 레코드 하나) | 해당 샘플의 최종 분류값·정답값. 예: "불량(defect)" vs "정상(normal)", 감성 "긍정/부정" |
| Free-form | Description (해설) | 데이터셋/컬럼/필드 | 데이터가 무엇이고 어떻게 수집·생성되었는지 자유 서술 문서. Datasheets for Datasets 등의 문서화 표준이 해당 |

### 1.2 학술·업계 정의의 공통점과 차이

**업계(실무) 관점:**
- iMerit, Toloka, LabelVisor 등 주요 annotation 서비스 기업들은 다음과 같이 구분한다:
  - **Labeling**: 단순 분류·태깅. 미리 정해진 카테고리를 데이터 전체에 부여하는 빠르고 비용 효율적인 작업
  - **Annotation**: 더 광범위한 개념. 데이터의 특정 부분(영역, 구간, 요소)에 의미 표기. 복잡한 ML 과제에 필요한 풍부한 맥락 제공
  - "All labeling is annotation, but not all annotation is labeling." (모든 라벨링은 annotation이지만, 모든 annotation이 라벨링은 아니다)

**학술(연구) 관점:**
- NLP·CV 학술 문헌에서는 두 용어를 종종 혼용하지만, **annotation이 상위 개념**으로 사용되는 경향
  - Stanford NLP, ACL(Association for Computational Linguistics) 전통에서 annotation = 언어·시각 데이터에 구조·의미 정보를 덧붙이는 모든 행위
  - Label은 annotation의 한 형태 (instance-level classification result)

**실질적 경계:**
- Annotation은 "데이터 안의 특정 부분"에 의미를 부여 → bounding box 좌표, NER span 범위, 시계열 이상 구간의 시작/끝
- Label은 "데이터 전체 인스턴스"에 카테고리를 부여 → 이미지 분류 태그, 문서 주제 코드, 레코드의 결함 유형 코드
- 실제 AI 과제에서는 둘이 **함께 쓰임**: 이미지에 bounding box(annotation)를 그린 뒤 그 안의 객체 클래스(label)를 부여

### 1.3 무엇이 아닌지 (경계 명확화)

- **평가용 정답(Evaluation Ground Truth)**: 모델 성능 측정에 쓰는 test set 라벨 → 본 주제(B-2) 범위 외 (E-3 담당)
- **메타데이터(Metadata)**: 데이터의 속성·맥락 정보(파일 크기, 촬영 날짜, 센서 종류 등) → 별도 주제
- **피처 엔지니어링(Feature Engineering)**: 모델 입력을 위한 변환·파생 변수 생성 → annotation과 다름
- **데이터 정제(Data Cleaning/Preprocessing)**: 노이즈 제거, 결측 처리 → annotation 선행 작업이나 별도 범주

---

## 2. Annotation 유형 분류

### 2.1 이미지 Annotation (Computer Vision)

#### 2.1.1 Bounding Box (경계 상자)
- **정의**: 이미지 내 객체를 둘러싸는 직사각형 영역. 네 꼭짓점 또는 (x, y, width, height) 형태로 표현
- **특징**: 가장 기본적이고 빠르며 비용 효율적인 annotation 방식
- **ML 과제**: Object Detection (YOLO, Faster R-CNN, SSD 등)
- **적용 사례**: 결함 이미지에서 불량 부위 위치 특정, 차량/보행자 탐지, 품질 검사 카메라
- **한계**: 객체의 정확한 형태를 표현하지 못함 (직사각형이 실제 객체보다 큰 여백 포함)

#### 2.1.2 Polygon / Instance Segmentation (다각형·인스턴스 분할)
- **정의**: 객체의 외곽선을 꼭짓점(vertex)들로 추적하는 다각형 annotation
- **특징**: Bounding Box 대비 15~30% 높은 IoU(Intersection over Union) 달성, 생성 시간 3~10배 소요
- **ML 과제**: Instance Segmentation (Mask R-CNN), 정밀 객체 탐지
- **적용 사례**: 복잡한 형태의 결함 부위, 의료 영상 병변 경계, 위성 이미지 건물 윤곽
- **Polygon vs Segmentation Mask 차이**: Polygon은 꼭짓점 기반(벡터), Mask는 픽셀 단위(래스터)

#### 2.1.3 Semantic Segmentation (의미론적 분할)
- **정의**: 이미지의 모든 픽셀을 클래스에 할당하는 pixel-wise annotation
- **특징**: 가장 세밀하나 가장 시간·비용 소요
- **ML 과제**: Semantic Segmentation (FCN, DeepLab, U-Net)
- **적용 사례**: 자율주행(도로/보도/차량/보행자 픽셀 구분), 의료 영상 장기 분할

#### 2.1.4 Keypoint / Skeleton (핵심점·골격)
- **정의**: 객체의 특정 관절, 랜드마크 포인트를 점(x, y, visibility)으로 표기
- **ML 과제**: Pose Estimation, Facial Landmark Detection
- **적용 사례**: 인체 자세 분석, 얼굴 인식, 산업 현장 작업자 안전 모니터링

#### 2.1.5 Image Classification Tag (이미지 분류 태그)
- **정의**: 이미지 전체에 하나 또는 여러 개의 카테고리 태그를 부여 (Macro 단위 → Label)
- **ML 과제**: Image Classification (ResNet, VGG, EfficientNet)
- **적용 사례**: 불량/정상 이진 분류, 제품 카테고리 분류

#### 2.1.6 Polyline (폴리라인)
- **정의**: 닫히지 않은 선분 annotation
- **ML 과제**: 차선 감지, 전력선·강 경로 탐지
- **적용 사례**: 자율주행 차선 인식, 지도 도로 추출

### 2.2 텍스트 Annotation (NLP)

#### 2.2.1 NER / Named Entity Recognition (개체명 인식)
- **정의**: 텍스트 내에서 인명, 기관명, 지명, 날짜 등 특정 엔티티 구간을 식별하고 유형 태그를 부여
- **표기 방식**: BIO 또는 BIOES 태깅 스킴 (아래 2.2.7 참조)
- **ML 과제**: Sequence Labeling, Information Extraction
- **적용 사례**: C/S Report에서 부품명·불량 유형·발생 일자 추출, 고객 클레임에서 제품명·증상 구간 태깅

#### 2.2.2 Span Annotation (구간 주석)
- **정의**: 텍스트 내 특정 구간(토큰 범위)에 의미·역할 태그를 부여
- **확장 사례**: 중첩 엔티티(Nested Entities), 공지시(Coreference Resolution), 논거 구간(Argument Span)
- **적용 사례**: 클레임 문서에서 원인 분석 구간, 조치 이력 구간 식별

#### 2.2.3 Relation Extraction (관계 추출)
- **정의**: 식별된 두 엔티티 사이의 관계 유형을 태깅
- **예시**: Person–works_at–Company, Defect–caused_by–Process
- **ML 과제**: Relation Classification, Knowledge Graph Construction

#### 2.2.4 Sentiment Annotation (감성 주석)
- **정의**: 텍스트 전체 또는 특정 측면(aspect)에 감성 극성(긍정/부정/중립) 부여
- **유형**: 문서 수준(Document-level), 문장 수준(Sentence-level), 측면 수준(Aspect-level Sentiment Analysis, ABSA)
- **적용 사례**: 고객 클레임 감성 분석, VOC 자동 분류

#### 2.2.5 Document/Text Classification (문서 분류)
- **정의**: 문서 전체에 카테고리 태그 부여 (Macro 단위 → Label)
- **ML 과제**: Text Classification (BERT, RoBERTa Fine-tuning)
- **적용 사례**: C/S Report 원인 분류 코드 부여, 실험 결과 문서 유형 분류

#### 2.2.6 Token Classification (토큰 분류)
- **정의**: 텍스트를 토큰 단위로 분해하여 각 토큰에 클래스 부여
- **ML 과제**: POS Tagging, Chunking, NER (BIO 스킴 포함)

#### 2.2.7 BIO / BIOES 태깅 스킴 (CoNLL 표준)
- **BIO**: B(Beginning), I(Inside), O(Outside) — 세 가지 태그로 임의 flat entity 표현 가능
  - 예: "Apple Inc. released iPhone" → B-ORG I-ORG O B-PROD (BIO 기준)
- **BIOES/BILOU**: B, I, O에 E(End), S(Single) 추가 → 경계 명확화, NER F1 1~2% 향상
  - 예: "Apple Inc. released iPhone" → B-ORG E-ORG O S-PROD (BIOES 기준)
- **CoNLL 2003**: NER 공유 과제 기준 데이터셋, BIO-2 스킴 채택. 영어/독어 PER/ORG/LOC/MISC 4개 유형

### 2.3 오디오 / 시계열 Annotation

#### 2.3.1 오디오 Annotation
- **유형**:
  - 전사(Transcription): 음성→텍스트 변환의 정답 텍스트 작성
  - 화자 분리(Speaker Diarization): 구간별 화자 태깅
  - 소리 이벤트 탐지(Sound Event Detection, SED): 특정 소리 이벤트의 시작/끝 타임스탬프 + 유형 태깅
  - 감성/의도 분류: 오디오 전체 or 구간에 감성·의도 라벨
- **ML 과제**: ASR(자동 음성 인식), Speaker Identification, Sound Classification

#### 2.3.2 시계열 Annotation
- **유형**:
  - 이상 구간 태깅(Anomaly Segment Labeling): 정상/이상 구간 시작·끝 타임스탬프 + 이상 유형
  - 이벤트 태깅(Event Labeling): 특정 공정 이벤트 발생 시점 마킹
  - 전체 시퀀스 분류(Sequence Classification): 시계열 레코드 전체에 클래스 부여
- **ML 과제**: Anomaly Detection, Time Series Classification, Predictive Maintenance
- **적용 사례 (제조)**: 설비 센서 데이터의 불량 발생 구간 태깅, 진동 데이터의 이상 패턴 표시

### 2.4 문서·비정형 텍스트 Annotation (산업 특화)

- **원인 분석 문서 주석 (Root Cause Annotation)**: C/S Report, 8D Report에서 원인(Root Cause), 대책(Counter Measure), 영향 범위 구간을 각각 다른 태그로 annotation
- **클레임 문장 주석 (Claim Sentence Annotation)**: 고객 클레임 텍스트에서 불량 유형, 발생 조건, 심각도, 요구 사항 등을 span annotation으로 표기
- **실험 결과 주석 (Experimental Result Annotation)**: 실험 보고서의 수치·단위·측정 조건·결론 구간에 구조화 태그 부여
- **조치 이력 주석 (Action History Annotation)**: 조치 유형, 담당 부서, 조치 날짜, 효과 판정 구간을 태깅

---

## 3. Label 체계 설계

### 3.1 Taxonomy (분류 체계)

**정의**: 라벨들의 집합을 계층적·논리적으로 조직한 구조. 라벨 간 관계(상위/하위, 포함/배제)를 명시.

**설계 원칙**:
1. **상호배타성(Mutually Exclusive)**: 하나의 인스턴스에 동일 수준의 라벨 중 하나만 해당
2. **포괄성(Exhaustive)**: 가능한 모든 경우를 커버하는 카테고리 존재 (필요 시 "기타(Other/Unknown)" 카테고리 포함)
3. **일관성(Consistent)**: 같은 개념은 하나의 라벨로만 표현

**Flat vs Hierarchical 비교**:
| 구분 | Flat Taxonomy | Hierarchical Taxonomy |
|------|--------------|----------------------|
| 구조 | 단일 수준 카테고리 목록 | 트리 구조, 레벨별 상위/하위 카테고리 |
| 장점 | 단순, 구현 쉬움 | 세분화 조절 가능, 상위 수준 집계 용이 |
| 단점 | 세분화 어려움 | 설계 복잡, 라벨 간 경계 모호해질 수 있음 |
| 예시 | ["불량", "정상"] | 불량 > [외관불량 > [스크래치, 찍힘], 치수불량] |

**Multi-label (다중라벨)**:
- 하나의 인스턴스에 여러 라벨이 동시에 적용될 수 있는 설계
- 예: 이미지에 "스크래치"이면서 동시에 "오염" 라벨 부여 가능
- 모델 아키텍처에서 Sigmoid 활성화함수 + Binary Cross-Entropy Loss 사용
- 계층적 multi-label: 부모 라벨이 참(True)이면 자식 라벨도 고려 (Hierarchy-preserving learning)

### 3.2 라벨 정의서 (Label Definition Document)

**필수 구성 요소**:
1. **라벨 코드(Label Code)**: 고유 식별자 (예: DEF_SCR_001)
2. **라벨명(Label Name)**: 인간이 읽을 수 있는 명칭
3. **정의(Definition)**: 해당 라벨이 무엇인지 1~3문장 명확한 서술
4. **포함 기준(Inclusion Criteria)**: 이 라벨을 부여해야 하는 조건
5. **제외 기준(Exclusion Criteria)**: 이 라벨을 부여하지 않아야 하는 조건 (혼동 라벨과의 경계)
6. **긍정 예시(Positive Examples)**: 실제 데이터 예시 2~5개 (이 라벨이 맞는 케이스)
7. **부정 예시(Negative Examples)**: 혼동하기 쉬운 라벨의 예시 (이 라벨이 틀린 케이스)
8. **경계 케이스(Edge Cases)**: 판단이 어려운 사례와 처리 기준

### 3.3 계층적 라벨 (Hierarchical Labels)

- **트리 구조**: 각 레벨의 라벨들은 같은 레벨 내에서 상호배타적이며, 각 자식 라벨은 하나의 부모 라벨을 가짐
- **라벨 전파(Label Propagation)**: 자식 라벨이 참이면 상위 부모 라벨도 참으로 추론 가능
- **불완전 라벨 처리**: 예를 들어 "결함"은 알지만 세부 유형을 모를 때 상위 레벨에서만 라벨 부여 허용
- **모델 학습 활용**: 계층 정보를 Loss function에 반영하면 라벨 품질과 모델 성능 향상

---

## 4. Annotation Guideline (주석 가이드라인)

### 4.1 가이드라인 구성 요소

효과적인 Annotation Guideline은 다음 섹션으로 구성된다:

1. **과제 목적(Task Purpose)**: 이 annotation이 어떤 ML 모델 학습에 쓰이는지, 모델이 달성해야 할 목표
2. **annotation 대상 정의(Scope)**: 무엇을 annotation하는지 명확히 (대상 데이터 유형, 범위)
3. **라벨/태그 목록 및 정의(Label Definitions)**: 각 라벨/태그의 상세 정의 (3.2 참조)
4. **annotation 절차(Procedure)**: 작업 순서, 도구 사용법, 결과물 형식
5. **긍정/부정 예시(Examples)**: 올바른 annotation과 잘못된 annotation의 실제 예
6. **경계 케이스 처리 규칙(Edge Case Rules)**: 애매한 상황별 판단 기준
7. **품질 기준(Quality Criteria)**: 허용 오류율, IAA 기준값
8. **에스컬레이션 프로세스(Escalation)**: 판단 불가 시 상위 리뷰어에게 전달 방법

### 4.2 가이드라인 반복 개선 프로세스

1. 초안 작성 → Pilot annotation (100~200개 샘플) 수행
2. IAA(Inter-Annotator Agreement) 측정
3. 불일치 케이스 분석 → 가이드라인 수정
4. 3~5회 반복 후 가이드라인 확정(Lock)
5. Gold Set 구축 후 신규 annotator 교육에 활용

### 4.3 Gold Standard (골드 표준)

**정의**: 전문가 검토를 거쳐 확정된 신뢰도 높은 annotation 기준 샘플 집합.

**Gold Set 구성 권장 비율**:
| 유형 | 비율 | 설명 |
|------|------|------|
| 표준 사례(Standard Cases) | 40% | 가이드라인에 명확히 해당하는 전형적 예시 |
| 경계 사례(Boundary Cases) | 30% | 결정 경계 근처의 애매한 케이스 |
| 엣지 케이스(Edge Cases) | 20% | 드물지만 유효한 특수 케이스 |
| 혼동 쌍(Confusion Pairs) | 10% | 서로 헷갈리는 라벨 쌍의 사례 |

**활용**:
- Annotator 채용 평가: Gold Set 기준 90% 이상 일치 필요
- 온보딩 교육 및 주기적 캘리브레이션 테스트
- 모델 성능 측정의 기준 데이터셋

### 4.4 애매 케이스 (Ambiguous Cases) 처리

**처리 플로우**:
1. Annotator가 애매 케이스 플래그 지정
2. 시니어 리뷰어(또는 도메인 전문가)가 검토
3. 판단 근거 문서화 → 가이드라인 업데이트
4. 전체 Annotator 재교육(Recalibration)
5. 필요 시 Taxonomy 확장(새 라벨 추가 또는 분할)

---

## 5. 라벨/주석 품질·일관성 — IAA (Inter-Annotator Agreement)

### 5.1 IAA의 개념

**Inter-Annotator Agreement (IAA, 주석자 간 일치도)**: 동일 데이터에 대해 여러 annotator가 부여한 annotation의 일치 수준을 정량화하는 지표. 높은 IAA = annotation의 일관성·신뢰도 높음.

**IAA가 낮을 때 의미하는 것**:
- 가이드라인이 불명확함
- 라벨 정의가 모호함
- Annotator 간 도메인 지식 차이
- 과제 자체의 주관성이 높음

### 5.2 Cohen's Kappa (코헨의 카파)

- **적용 조건**: 두 명의 annotator, 범주형(명목/이진) 데이터
- **수식**: k = (P_o - P_e) / (1 - P_e)
  - P_o: 관측된 일치율(Observed Agreement)
  - P_e: 우연에 의한 기대 일치율(Expected Agreement by Chance)
- **해석 기준** (Landis & Koch, 1977):
  | k 값 | 일치 수준 |
  |------|----------|
  | < 0.20 | 미약(Slight) |
  | 0.21-0.40 | 보통(Fair) |
  | 0.41-0.60 | 중간(Moderate) |
  | 0.61-0.80 | 상당(Substantial) |
  | 0.81-1.00 | 거의 완전(Almost Perfect) |
- **한계**: 두 명 annotator만 지원, 순서형 데이터에 부적합

### 5.3 Fleiss' Kappa (플라이스 카파)

- **적용 조건**: 세 명 이상의 annotator, 범주형 데이터
- Cohen's Kappa의 다인(多人) 확장 버전
- 각 항목을 독립적으로 여러 rater가 평가하는 설계에 적합

### 5.4 Krippendorff's Alpha (크리펜도르프의 알파)

- **적용 조건**: 다수의 annotator, 다양한 데이터 유형(이진/명목/순서/구간/비율), 결측값 허용
- 수백~수천 명의 annotator가 서로 다른 부분집합에 annotation하는 crowdsourcing 파이프라인에 가장 적합
- 표본 크기, 카테고리 다양성, 우연 일치 가능성을 동시에 고려
- **권장 임계값**: alpha >= 0.80 (신뢰할 수 있는 분류 과제), alpha >= 0.67 (잠정적 결론 가능)

### 5.5 Consensus & Adjudication (합의·중재)

**Consensus(합의) 방법**:
- **다수결(Majority Voting)**: 가장 많은 annotator가 선택한 라벨을 정답으로 채택. 단순 분류 과제에 효과적
- **가중 다수결(Weighted Voting)**: 전문성·과거 정확도에 따라 annotator별 가중치 부여
- **순차 검토(Sequential Review)**: 1차 annotation → 시니어 검토 순서. 전문성이 필요한 복잡 과제에 적합

**Adjudication(중재) 프로세스**:
- annotation 버전 간 불일치를 골드 표준으로 승격하기 전 해소하는 절차
- 초기 단계: 전체 팀이 함께 수동 중재(가이드라인 내재화에 효과적)
- 성숙 단계: 반자동/자동 중재 도구 활용
- 전문가 불일치 시: 구조적 토론 + 최종 결정 근거 문서화 (기관 지식 축적)

**다층 QC(Multi-layer Quality Control)**:
1. Peer Review: 동료 annotator가 오류 1차 검수
2. Centralized Audit: 시니어 리뷰어가 체계적 오류 2차 검수
3. Reconciliation Pass: 고위험 데이터에 대한 불일치 최종 조정

---

## 6. 표준 및 포맷

### 6.1 이미지 Annotation 포맷

#### 6.1.1 COCO JSON Format (Common Objects in Context)
- **개발**: Microsoft Research
- **구조**: 단일 JSON 파일에 전체 데이터셋 annotation 통합
  ```json
  {
    "info": {},
    "licenses": [],
    "images": [{"id": 1, "file_name": "img.jpg", "width": 640, "height": 480}],
    "annotations": [{"id": 1, "image_id": 1, "category_id": 1,
                      "segmentation": [[x1,y1,...]], "bbox": [x,y,w,h],
                      "area": 5000, "iscrowd": 0}],
    "categories": [{"id": 1, "name": "defect", "supercategory": "quality"}]
  }
  ```
- **지원 과제**: Object Detection, Keypoint Detection, Stuff Segmentation, Panoptic Segmentation, Image Captioning
- **Bbox 형식**: [x_min, y_min, width, height] (절대 픽셀값)
- **Keypoint 형식**: [x, y, v]의 반복 (v=0: 미표기, v=1: 표기되었지만 비가시, v=2: 표기·가시)
- **장점**: 다양한 annotation 유형 통합 지원, 대부분의 CV 프레임워크에서 지원
- **활용**: COCO benchmark를 사용하는 거의 모든 Object Detection/Segmentation 연구

#### 6.1.2 Pascal VOC XML Format
- **개발**: Visual Object Classes Challenge (옥스퍼드 대학)
- **구조**: 이미지별 독립 XML 파일
  ```xml
  <annotation>
    <folder>images</folder>
    <filename>image.jpg</filename>
    <size><width>640</width><height>480</height><depth>3</depth></size>
    <object>
      <name>defect</name>
      <truncated>0</truncated>
      <occluded>0</occluded>
      <difficult>0</difficult>
      <bndbox><xmin>100</xmin><ymin>150</ymin><xmax>200</xmax><ymax>300</ymax></bndbox>
    </object>
  </annotation>
  ```
- **Bbox 형식**: [xmin, ymin, xmax, ymax] (절대 픽셀값, XYXY 포맷)
- **특이 필드**: truncated(잘림), occluded(가림), difficult(탐지 어려움) — 데이터 품질 메타정보 포함
- **지원**: Bounding Boxes, Segmentation Masks, Keypoints, Classification Tags
- **장점**: XML 구조로 인간 가독성 높음, 오래된 프레임워크와의 호환성 우수

#### 6.1.3 YOLO TXT Format
- **개발**: YOLO(You Only Look Once) 논문 시리즈
- **구조**: 이미지별 .txt 파일, 각 줄 = 하나의 객체
  ```
  <class_id> <x_center> <y_center> <width> <height>
  0 0.273 0.500 0.078 0.167
  ```
- **좌표 방식**: 이미지 크기 대비 정규화(Normalized) 좌표 (0.0~1.0), 중심점 + 너비·높이
- **장점**: 파일 크기 최소, 파싱 단순, 엣지 디바이스 배포에 유리
- **단점**: segmentation, keypoint 등 복잡한 annotation 유형 지원 제한 (YOLOv8 이후 일부 확장)
- **현황**: YOLOv5/v8/v11/Ultralytics 계열이 산업 배포에서 지배적

**포맷 비교 요약**:
| 특성 | COCO JSON | Pascal VOC XML | YOLO TXT |
|------|-----------|---------------|---------|
| 파일 구조 | 데이터셋 단일 JSON | 이미지별 XML | 이미지별 TXT |
| 좌표계 | 절대값 [x,y,w,h] | 절대값 [xmin,ymin,xmax,ymax] | 정규화 중심+크기 |
| 지원 annotation | Detection+Segmentation+Keypoint+Caption | Detection+Segmentation | 주로 Detection |
| 복잡도 | 높음 | 중간 | 낮음 |
| 프레임워크 지원 | 광범위(연구) | 중간(레거시) | YOLO 생태계 |

### 6.2 텍스트 Annotation 포맷

#### 6.2.1 CoNLL Format (NER 표준)
- **기원**: CoNLL(Conference on Computational Natural Language Learning) 2003 공유 과제
- **구조**: 탭/공백으로 구분된 열(column), 문장 사이 빈 줄로 구분
  ```
  Apple  NNP  B-NP  B-ORG
  Inc.   NNP  I-NP  I-ORG
  is     VBZ  B-VP  O
  ```
- **태깅 스킴**: BIO-2 (B-TYPE: 엔티티 시작, I-TYPE: 엔티티 내부, O: 비엔티티)
- **CoNLL-2003**: 영어/독어 4개 유형(PER/ORG/LOC/MISC) — NER 벤치마크의 표준
- **확장 스킴 BIOES**: B(Beginning), I(Inside), O(Outside), E(End), S(Single) — 경계 정확도 향상, NER F1 1~2% 향상

### 6.3 데이터셋 문서화 표준

#### 6.3.1 Datasheets for Datasets (Gebru et al., 2021)
- **출처**: Timnit Gebru 외, Communications of the ACM 2021
- **목적**: 전자부품 데이터시트를 모델로, 모든 AI 데이터셋에 표준 문서 첨부 제안
- **섹션**: 동기(Motivation), 구성(Composition), 수집 과정(Collection Process), 권장 사용(Recommended Uses), 배포(Distribution), 유지보수(Maintenance)
- **annotation 관련**: 수집·annotation 과정, 라벨 유형, annotator 정보, 동의(Consent) 포함 여부

#### 6.3.2 Data Statements (Bender & Friedman, 2018)
- **출처**: Emily Bender & Batya Friedman, ACL 2018
- **대상**: NLP 데이터셋 특화
- **포함 항목**: 언어 다양성, 화자 인구통계, 주석 과정, 큐레이션 합리성

#### 6.3.3 Data Cards (Pushkarna et al., 2022)
- **출처**: Google Research
- **초점**: 생산 환경의 ML 데이터셋 투명성. 데이터 출처, annotation 방법, 알려진 편향, 적합/부적합 사용 사례 기술

#### 6.3.4 Data Provenance Standard (Data & Trust Alliance)
- IBM, Nike, Mastercard, Walmart, Pfizer, UPS 등 19개 기업 공동 수립
- 데이터 출처·annotation 이력의 추적성(Traceability) 표준화

---

## 7. 핵심 용어집 (Glossary)

| 용어 | 영문 | 정의 |
|------|------|------|
| **지상 진실** | Ground Truth | ML 모델이 학습·평가할 때 참고하는 인간이 검증한 정답 annotation. 모델 성능의 상한선을 결정 |
| **골드 셋** | Gold Set / Gold Standard | 전문가가 검토하여 확정한 고신뢰도 annotation 샘플 집합. Annotator 평가 및 모델 벤치마크 기준으로 활용 |
| **주석 가이드라인** | Annotation Guideline | Annotator가 일관된 annotation을 수행하도록 정의, 예시, 경계 케이스 처리 규칙을 담은 공식 문서 |
| **분류 체계** | Taxonomy | 라벨들을 계층적·논리적으로 조직한 구조. 상위/하위 관계, 상호배타성, 포괄성을 정의 |
| **주석자 간 일치도** | IAA (Inter-Annotator Agreement) | 동일 데이터를 여러 annotator가 annotation했을 때 일치 수준을 정량화한 지표. Cohen's Kappa, Fleiss' Kappa, Krippendorff's Alpha 등으로 측정 |
| **인간 참여 루프** | HITL (Human-In-The-Loop) | AI 학습·예측 과정에 인간 전문가가 주기적으로 개입하여 annotation 수정, 오류 교정, 불확실 샘플 라벨링을 수행하는 방식 |
| **약한 지도학습** | Weak Supervision | 라벨링 함수(규칙, 휴리스틱, 기존 모델 출력)를 다수 적용하고 노이즈를 모델링하여 대규모 학습 데이터를 프로그래매틱하게 생성하는 접근. Snorkel이 대표 프레임워크 |
| **능동 학습** | Active Learning | 모델이 불확실성이 높은 샘플을 선택적으로 골라 인간에게 annotation을 요청하여 최소한의 라벨링으로 최대 성능 향상을 추구하는 반복 학습 프레임워크 |
| **중재** | Adjudication | 여러 annotator의 불일치 annotation을 전문가 검토를 통해 단일 정답으로 확정하는 프로세스. Gold Set 생성의 핵심 단계 |
| **라벨 정의서** | Label Definition Document | 각 라벨의 코드, 정의, 포함/제외 기준, 긍부정 예시, 경계 케이스를 공식화한 문서. Annotation Guideline의 핵심 부속 자료 |
| **프로그래매틱 라벨링** | Programmatic Labeling | 규칙, 정규식, 사전(Dictionary), 기존 모델 출력 등 여러 라벨링 함수(Labeling Functions)를 코드로 작성하여 대규모 데이터에 자동 적용하는 방식. Weak Supervision의 구현 방법 |
| **데이터시트** | Datasheet for Dataset | 데이터셋의 동기, 구성, 수집 과정, annotation 방법, 권장 사용, 알려진 편향을 기술한 표준 문서. Gebru et al.(2021)이 제안한 데이터 투명성 표준 |

---

## 8. 참고: Annotation 유형 x ML 과제 매핑표

| Annotation 유형 | ML 과제 | 대표 모델/알고리즘 |
|----------------|---------|-----------------|
| Bounding Box | Object Detection | YOLO, Faster R-CNN, SSD |
| Polygon/Instance Mask | Instance Segmentation | Mask R-CNN, SAM (Segment Anything) |
| Semantic Mask (Pixel-wise) | Semantic Segmentation | FCN, DeepLab, U-Net |
| Keypoint | Pose Estimation, Facial Landmark | OpenPose, MediaPipe, DEKR |
| Image Classification Tag | Image Classification | ResNet, EfficientNet, ViT |
| NER/Span | Named Entity Recognition, Information Extraction | BERT-NER, spaCy, Flair |
| Relation | Relation Extraction, KG Construction | RE-BERT, REBEL |
| Document Classification Label | Text Classification | BERT, RoBERTa Fine-tuning |
| Sentiment Label | Sentiment Analysis, ABSA | BERT-SA, LLM fine-tuning |
| Audio Segment Label | SED, Speaker Diarization | CNN-RNN, wav2vec 2.0 |
| Time Series Anomaly Label | Anomaly Detection | LSTM-AE, Isolation Forest + HITL |

---

## 출처

1. iMerit — "Data Annotation vs Data Labeling: What's the Difference?" https://imerit.ai/resources/blog/data-annotation-vs-data-labeling-whats-the-difference/
2. Toloka AI — "Data Annotation vs Data Labeling: what you need to know" https://toloka.ai/blog/annotation-vs-labeling/
3. LabelVisor — "Data Annotation vs. Data Labeling: Explained" https://www.labelvisor.com/data-annotation-vs-data-labeling-explained/
4. Sama — "Image Annotation Guide: Types, Techniques & Best Practices for Machine Learning" https://www.sama.com/blog/image-annotation-guide
5. Label Your Data — "Polygon Annotation: Tools, Techniques, and Best Practices in 2026" https://labelyourdata.com/articles/data-annotation/polygon-annotation
6. DataVLab — "Image Annotation Techniques Explained for AI Projects" https://datavlab.ai/post/types-of-image-annotation-bounding-boxes-polygons-keypoints-and-more
7. GigaBPO — "Data Annotation Best Practices: A Practical Guide for High-Quality Labeling" https://gigabpo.com/data-annotation-best-practices/
8. Encord — "Achieving Annotation Consensus: Strategies for High-Agreement Datasets" https://encord.com/blog/achieving-annotation-consensus-strategies-for-high-agreement-datasets/
9. CVAT — "Annotation Quality Assurance: A Multi-Layered Approach to Reliable Labeled Data" https://www.cvat.ai/resources/blog/annotation-quality-assurance
10. Medium / Jorge Campos — "The adjudication process in collaborative annotation" https://medium.com/@jorgecp/the-adjudication-process-in-collaborative-annotation-61623c46b700
11. NumberAnalytics — "Mastering Inter-Annotator Agreement" https://www.numberanalytics.com/blog/mastering-inter-annotator-agreement
12. Datasaur — "Introducing Krippendorff's Alpha IAA Calculation" https://datasaur.ai/blog-posts/inter-annotator-agreement-krippendorff-cohen
13. Michael Brenndoerfer — "Cohen, Fleiss & Krippendorff: IAA Metrics & Implementation" https://mbrenndoerfer.com/writing/inter-annotator-agreement-kappa-alpha-reliability
14. Roboflow — "What is the COCO JSON Annotation Format?" https://roboflow.com/formats/coco-json
15. Roboflow — "What is the Pascal VOC XML Annotation Format?" https://roboflow.com/formats/pascal-voc-xml
16. Roboflow — "What is the YOLO Annotation Format?" https://roboflow.com/formats/yolo
17. DataVLab — "Best Annotation Formats: COCO, PASCAL VOC, YOLO" https://datavlab.ai/post/how-to-choose-the-right-annotation-format-coco-yolo-pascal-voc-and-beyond
18. Wikipedia — "Inside-outside-beginning (tagging)" https://en.wikipedia.org/wiki/Inside%E2%80%93outside%E2%80%93beginning_(tagging)
19. Medium / Shannon Rong — "Named Entity Recognition - Annotation Schemes" https://medium.com/@rongqianhui/named-entity-recognition-annotation-schemes-e684f9cd5a56
20. Snorkel AI — "Essential Guide to Weak Supervision" https://snorkel.ai/data-centric-ai/weak-supervision/
21. Snorkel AI Docs — "Active learning and weak supervision" https://docs.snorkel.ai/docs/25.4/user-guide/intro/active-learning-weak-supervision/
22. Gebru et al. (2021) — "Datasheets for Datasets", Communications of the ACM https://cacm.acm.org/research/datasheets-for-datasets/
23. arXiv:1803.09010 — "Datasheets for Datasets" (원본 논문) https://arxiv.org/pdf/1803.09010
24. IBM — "What Is Ground Truth in Machine Learning?" https://www.ibm.com/think/topics/ground-truth
25. Encord — "Ground Truth Definition" https://encord.com/glossary/ground-truth-definition/
26. Label Studio — "Implementing Audio Classification for Machine Learning Projects" https://labelstud.io/blog/implementing-audio-classification-for-machine-learning-projects-using-label-studio/
27. Encord — "Audio Segmentation for AI: Techniques and Applications" https://encord.com/blog/audio-segmentation-for-ai/
28. arXiv:2210.04772 — "An Ontology for Defect Detection in Metal Additive Manufacturing" https://arxiv.org/abs/2210.04772
29. Labelbox — "How to build defect detection models to automate visual quality inspection" https://labelbox.com/guides/how-to-build-defect-detection-models-to-improve-visual-quality-inspection/
30. ResearchGate — "Snorkel: rapid training data creation with weak supervision" https://www.researchgate.net/publication/334476066_Snorkel_rapid_training_data_creation_with_weak_supervision
31. Data Provenance Initiative (2024) — "Bridging the Data Provenance Gap Across Text, Images, and Video" https://www.dataprovenance.org/Multimodal_Data_Provenance.pdf
32. Ultralytics Docs — "Object Detection Datasets Overview" https://docs.ultralytics.com/datasets/detect
33. AWS Rekognition — "The COCO dataset format" https://docs.aws.amazon.com/rekognition/latest/customlabels-dg/md-coco-overview.html
34. arXiv:2402.13446 — "Large Language Models for Data Annotation: A Survey" https://arxiv.org/html/2402.13446v2
