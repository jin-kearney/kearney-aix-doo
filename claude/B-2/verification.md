# B-2 데이터 해설/주석 — 출처 검증 리포트

> 검증일: 2026-06-02 | 검증자: Claude Sonnet 4.6 (자동 검증)
> 검증 범위: storyline.md 말미 "## 참고 출처" 35개 URL + 본문 핵심 정량 주장

---

## 요약

| 항목 | 수치 |
|------|------|
| 검증 시도 URL 수 | 35 |
| OK (내용 일치 또는 접근 가능·주장 지지) | 25 |
| MISMATCH (URL은 살아 있으나 귀속 사실과 불일치) | 3 |
| BROKEN (404·빈 응답·소멸) | 1 |
| UNVERIFIABLE (2차 출처로 1차 근거 미확인) | 6 |
| 자동 보정 건수 | 3 |
| 사람 확인 필요(⚠️) | 3 |

---

## 출처별 검증 표

| # | URL | 귀속 주장(요약) | 판정 | 조치 | 대체 출처 |
|---|-----|----------------|------|------|-----------|
| 1 | https://arxiv.org/abs/2409.09412 | LVIS 주석 오류 누적 시 mAP 상한 62.63~67.52로 수렴 | **OK** | 유지 | — |
| 2 | https://arxiv.org/html/2408.00714v1 (SAM 2) | "기존 대비 6배 정확도 향상" | **MISMATCH** | 수정 필요: 논문 원문은 "6× faster than SAM (이미지 분할 속도)", 정확도가 아닌 속도 향상 | 동일 URL — 내용 정정 |
| 3 | https://arxiv.org/pdf/1911.00068 (Confident Learning) | "MentorNet 대비 30% 이상 성능 우위" | **MISMATCH** | 논문에서 CL이 MentorNet보다 성능 우수하다는 내용은 있으나 "30% 이상"이라는 단순 수치 미확인. 표(Table) 결과는 노이즈 수준·희소성 조건별로 다름 | 수치 삭제, 일반 표현으로 완화 |
| 4 | https://labelbox.com/blog/using-labelboxs-model-assisted-labeling-to-speed-up-annotation-efficiency-advancing-ai-in-cutting-edge-research/ | "pre-labeling 적용 시 라벨링 시간·비용 최대 50% 절감" | **MISMATCH** | 해당 URL은 도쿄대 연구팀의 Labelbox 활용 사례(Mask-RCNN 2D 소재 탐지)를 소개하는 짧은 블로그로 "50% 절감" 수치를 명시하지 않음 | 삭제하고 일반 표현으로 완화, 또는 SourceBae 출처로 대체 (30~60% 범위 언급) |
| 5 | https://cleverx.com/blog/data-labeling-cost-optimization-playbook-strategic-automation-for-ml-operations/ | KPI 섹션 처리량·비용 출처 | **BROKEN** | URL 접근 시 빈 응답. 페이지 소멸 추정 | 참고 출처 목록에서 제거 |
| 6 | https://github.com/cleanlab/cleanlab | Cleanlab 오픈소스 라이브러리 | **OK** | 유지 | — |
| 7 | https://cacm.acm.org/research/datasheets-for-datasets/ | Datasheets for Datasets (Gebru et al., 2021, CACM) | **OK** | 유지 | — |
| 8 | https://snorkel.ai/blog/alfred-data-labeling-with-foundation-models-and-weak-supervision/ | Snorkel Alfred(2024) — 자연어로 LF 정의, Llama 3.1·GPT-4 통합 | **OK (내용 부분 보완 필요)** | 유지. 단, Alfred는 Snorkel AI 공식 제품이 아니라 Brown University PhD(Peilin Yu)가 개발한 오픈소스 연구 프로젝트임. ⚠️ 표현 주의 필요 | — |
| 9 | https://imerit.ai/resources/blog/pre-labeling-automation-accelerating-ai-annotation-with-smarter-first-drafts/ | Pre-labeling 시간 절감 효과 | **UNVERIFIABLE** | URL 접근 가능. 해당 iMerit 페이지는 임상 NLP 사례에서 "최대 21.5% 절감" 언급. "최대 50%" 주장을 직접 지지하지 않음 | — |
| 10 | https://sourcebae.com/blog/data-labeling-annotation-guide/ | "데이터 라벨링 비용이 88배 증가(2023→2024)" | **UNVERIFIABLE** | SourceBae 블로그 URL 접근 가능하고 해당 수치 언급 확인. 그러나 SourceBae는 2차 블로그로 1차 데이터 출처(원보고서)를 명시하지 않음. 해당 수치의 원저자 불명 | — |
| 11 | https://arxiv.org/abs/2406.17633 | "Knowledge Distillation in Automated Annotation" | **OK (제목 불일치 주의)** | URL 접근 가능. 실제 논문은 CSS 텍스트 분류에서 GPT-4 생성 라벨로 fine-tuning한 소형 모델이 사람 라벨 수준 성능 달성을 다룸. 논문 제목 원문: "Can Large Language Models be Used to Provide Feedback in Programming Courses? ..." — 아님. 실제 제목은 LLM 라벨 비교 CSS 논문. "Knowledge Distillation"은 storyline의 별도 설명 내용이며 논문과 직결되지는 않음 ⚠️ | — |
| 12 | https://arxiv.org/html/2602.14102v1 | DALL: Data Labeling via Data Programming and Active Learning Enhanced by LLMs | **OK** | 유지. 2026년 CHI 논문으로 확인됨 | — |
| 13 | https://www.truppglobal.com/blog/quality-control-in-data-annotation/ | KPI·품질 관리 모범 사례 | **OK** | 유지 | — |
| 14 | https://kili-technology.com/data-labeling/top-data-quality-metrics-for-assessing-your-data-labeling-quality | 라벨 품질 지표 | **OK** | 유지 | — |
| 15 | https://encord.com/blog/achieving-annotation-consensus-strategies-for-high-agreement-datasets/ | IAA·합의 전략 | **OK** | 유지 | — |
| 16 | https://www.cvat.ai/resources/blog/annotation-quality-assurance | 다층 QC 방법 | **OK** | 유지 | — |
| 17 | https://www.numberanalytics.com/blog/mastering-inter-annotator-agreement | IAA 지표 설명 | **OK** | 유지 | — |
| 18 | https://datasaur.ai/blog-posts/inter-annotator-agreement-krippendorff-cohen | Krippendorff's Alpha IAA | **OK** | 유지 | — |
| 19 | https://mbrenndoerfer.com/writing/inter-annotator-agreement-kappa-alpha-reliability | Cohen·Fleiss·Krippendorff IAA 지표 | **OK** | 유지 | — |
| 20 | https://roboflow.com/formats/coco-json | COCO JSON 포맷 | **OK** | 유지 | — |
| 21 | https://roboflow.com/formats/pascal-voc-xml | Pascal VOC XML 포맷 | **OK** | 유지 | — |
| 22 | https://roboflow.com/formats/yolo | YOLO 포맷 | **OK** | 유지 | — |
| 23 | https://en.wikipedia.org/wiki/Inside%E2%80%93outside%E2%80%93beginning_(tagging) | BIO/BIOES 태깅 스킴 | **OK** | 유지 | — |
| 24 | https://snorkel.ai/data-centric-ai/weak-supervision/ | Weak Supervision 개요 | **OK** | 유지 | — |
| 25 | https://www.ibm.com/think/topics/ground-truth | Ground Truth 정의 | **OK** | 유지 | — |
| 26 | https://encord.com/blog/best-data-labeling-platform-2026/ | 2026 라벨링 플랫폼 가이드 | **OK** | 유지 | — |
| 27 | https://aws.amazon.com/sagemaker/data-labeling/ | AWS SageMaker Ground Truth | **OK** | 유지 | — |
| 28 | https://docs.dataloop.ai/docs/active-learning-pipeline | Dataloop Active Learning | **OK** | 유지 | — |
| 29 | https://doc.dvc.org/use-cases/versioning-data-and-models | DVC 데이터 버전 관리 | **OK** | 유지 | — |
| 30 | https://imerit.ai/resources/blog/data-annotation-vs-data-labeling-whats-the-difference/ | Annotation vs Labeling 정의 | **OK** | 유지 | — |
| 31 | https://toloka.ai/blog/annotation-vs-labeling/ | Annotation vs Labeling | **OK** | 유지 | — |
| 32 | https://www.labelvisor.com/data-annotation-vs-data-labeling-explained/ | Annotation vs Labeling | **OK** | 유지 | — |
| 33 | https://www.sama.com/blog/image-annotation-guide | 이미지 Annotation 가이드 | **UNVERIFIABLE** | 직접 접근 미확인. 일반적으로 알려진 출처 | — |
| 34 | https://labelyourdata.com/articles/data-annotation/polygon-annotation | Polygon Annotation 2026 | **UNVERIFIABLE** | 직접 접근 미확인 | — |
| 35 | https://gigabpo.com/data-annotation-best-practices/ | Annotation 모범 사례 | **UNVERIFIABLE** | 직접 접근 미확인 | — |

---

## 자동 보정 내역 (3건)

### 보정 1: SAM 2 "6배 정확도 향상" → "6배 속도 향상" (슬라이드 18, 연구 파일 how.md)
- **원문**: "SAM 2(2024.7, 기존 대비 6배 정확도 향상)"
- **수정**: "SAM 2(2024.7, 기존 SAM 대비 이미지 분할 속도 6배 향상)"
- **근거**: arXiv 2408.00714v1 원문 "more accurate and 6× faster than the Segment Anything Model (SAM)" — 정확도는 소폭 개선, 속도가 6배. "6배 정확도 향상"은 사실과 반대.

### 보정 2: "MentorNet 대비 30% 이상 성능 우위" 구문 삭제
- **원문(research-how.md)**: "균일하지 않은 노이즈(non-uniform noise)에서 MentorNet 대비 30% 이상 성능 우위 (원논문)"
- **수정**: 수치 삭제, "복수 비교 방법론 대비 성능 우위"로 완화
- **근거**: 논문 표에서 노이즈 조건·희소성 수준에 따라 결과가 다양하며 일괄 "30% 이상"으로 요약 불가. 정확한 단일 수치 미확인.

### 보정 3: "최대 50% 절감 (Labelbox 공식 문서)" 표현 완화
- **원문**: "pre-labeling 적용 시 라벨링 시간·비용 최대 50% 절감 (Labelbox 공식 문서)"
- **수정**: "pre-labeling 적용 시 라벨링 시간·비용 절감 가능 (작업 유형·정확도에 따라 다양)"
- **근거**: 해당 Labelbox URL은 "50%"를 명시하지 않는 짧은 연구 소개 글임. 50% 수치의 1차 출처 미확인.

---

## "## 참고 출처" 목록 갱신 내역
- **제거**: CleverX URL (BROKEN) → storyline 말미 참고 출처 목록에서 삭제
- **유지**: 나머지 34개 URL 유지 (BROKEN 1개 제거)

---

## 사람 확인 필요(⚠️)

| 항목 | 내용 | 권고 조치 |
|------|------|-----------|
| ⚠️ 88배 비용 증가 (슬라이드 18, Pain Point 1) | "라벨링 비용이 88배 증가" 주장은 SourceBae 블로그에서 확인되나 해당 블로그가 원본 데이터 출처(보고서 명칭·발행기관)를 명시하지 않음. 2차 블로그 인용 상태. | 담당자가 원출처 보고서를 확인하거나, "사례에 따라 대폭 상승" 등 일반 표현으로 완화 권고 |
| ⚠️ arXiv 2406.17633 귀속 설명 ("Knowledge Distillation in Automated Annotation") | 해당 논문은 CSS 텍스트 분류에서 LLM 생성 라벨의 효율성을 다루는 논문으로, storyline에서 설명하는 "Knowledge Distillation" 개념과 직결되지 않을 수 있음. | 담당자가 논문 내용 직접 확인하여 인용 문맥이 적절한지 검토 권고 |
| ⚠️ Alfred의 출처 설명 (슬라이드 19, Snorkel Alfred) | Alfred는 Snorkel AI 직원이 아닌 Brown University PhD 연구자(Peilin Yu)의 오픈소스 연구 프로젝트임. "Snorkel Alfred"라는 표현이 Snorkel AI 공식 제품으로 오인될 수 있음. | "Alfred (Brown University·Snorkel AI 협력 연구, 2024)" 또는 "Alfred 오픈소스 라이브러리 (Snorkel AI 블로그 소개)"로 표현 명확화 권고 |
