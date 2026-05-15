---
tags: [vault, 01-ai-ready-data-module, framework]
---

### Pillar 1. Findable

#### 1-1. 데이터 카탈로그

**Sub-agent 전문가 구성**

1. **카탈로그 아키텍트** — 통합 카탈로그 구조 설계
2. **데이터 디스커버리 엔지니어** — 분산 시스템 자동 스캔/등록
3. **Semantic Search 전문가** — AI 친화적 검색·연결 체계
4. **카탈로그 운영 자동화 엔지니어** — Crawler·Sync 자동화
5. **사용자 경험(UX) 설계자** — 카탈로그 활용성·접근성 설계

**방법론·데이터 체계 목차**

- 1장. 분산 데이터 소스 인벤토리 수립 (DB, DW, Lake, SaaS, 파일 시스템)
- 2장. 통합 카탈로그 아키텍처 (Open Metadata / DataHub / Unity Catalog 비교)
- 3장. AI Agent용 카탈로그 API 및 Semantic Index 설계
- 4장. 자동 등록·갱신 파이프라인 (스키마 Drift 감지, 변경 이벤트 처리)
- 5장. 카탈로그 활용성 KPI 및 거버넌스 모델

---

#### 1-2. 메타데이터

**Sub-agent 전문가 구성**

1. **메타데이터 모델러** — Technical/Business/Operational 메타데이터 스키마 설계
2. **AI 컨텍스트 엔지니어** — LLM이 이해 가능한 메타데이터 표현 설계
3. **메타데이터 추출 자동화 전문가** — Profiling·NLP 기반 자동 태깅
4. **메타데이터 표준화 전문가** — DCAT, schema.org 등 표준 적용
5. **변경 관리(Change Management) 전문가** — Drift 탐지 및 자동 갱신

**방법론·데이터 체계 목차**

- 1장. 메타데이터 3계층 모델 정의 (Technical / Business / Operational)
- 2장. AI Readable Metadata Schema 설계 (Description, Examples, Constraints)
- 3장. 자동 메타데이터 추출 기법 (데이터 프로파일링, LLM 기반 설명 생성)
- 4장. 메타데이터 표준 및 상호운용성 (DCAT, OpenLineage 연계)
- 5장. 메타데이터 변경 감지·자동 갱신 워크플로우

---

#### 1-3. 비즈니스 Glossary

**Sub-agent 전문가 구성**

1. **용어 표준화 전문가** — 부서 간 용어 통일 프로세스
2. **도메인 전문가(SME) Coordinator** — 현업 지식 수집·정의
3. **Glossary-Ontology 연계 전문가** — 용어와 개념 모델 매핑
4. **AI Grounding 엔지니어** — LLM에 Glossary 주입 기법(RAG, Fine-tuning)
5. **거버넌스 위원회 운영자** — 변경 승인 프로세스 설계

**방법론·데이터 체계 목차**

- 1장. 용어 수집·중복·충돌 진단 방법론
- 2장. 표준 용어 정의 템플릿 (정의, 동의어, 약어, 사용 맥락, 책임자)
- 3장. Glossary-Data Asset 자동 연결 체계
- 4장. AI 모델에 Glossary 주입 패턴 (Prompt Context, Embedding 사전)
- 5장. 용어 라이프사이클 거버넌스 (제안→검토→승인→폐기)

---

### Pillar 2. Understandable

#### 2-1. 데이터 전처리

**Sub-agent 전문가 구성**

1. **비정형 데이터 파서** — PDF/이미지/표 구조화 전문가
2. **데이터 품질 엔지니어** — 결측·이상치·중복 처리
3. **Feature/Schema 표준화 전문가** — AI 입력 포맷 정의
4. **전처리 파이프라인 엔지니어** — ETL/ELT 자동화
5. **회귀 테스트 전문가** — 원천 변경에 따른 품질 모니터링

**방법론·데이터 체계 목차**

- 1장. 비정형 데이터 유형별 변환 전략 (PDF→Markdown, Table→JSON, Image→OCR/VLM)
- 2장. 데이터 클렌징 규칙 라이브러리 (결측·이상치·중복·정규화)
- 3장. 표준 입력 스키마 및 Feature Store 설계
- 4장. 멱등성(Idempotency) 기반 전처리 파이프라인 아키텍처
- 5장. 전처리 품질 회귀 테스트 및 SLA 관리

---

#### 2-2. 데이터 해설/주석 (Annotation)

**Sub-agent 전문가 구성**

1. **Annotation 가이드라인 설계자** — 라벨링 기준·예시 정의
2. **LLM 기반 자동 주석 엔지니어** — Pre-labeling 자동화
3. **Human-in-the-Loop(HITL) 운영 전문가** — 검토 워크플로우
4. **Inter-Annotator Agreement 분석가** — 일관성·신뢰도 측정
5. **Active Learning 전문가** — 효율적 주석 우선순위 선정

**방법론·데이터 체계 목차**

- 1장. 주석 스키마 및 라벨링 가이드라인 작성 원칙
- 2장. LLM Pre-labeling + Human Review 하이브리드 파이프라인
- 3장. IAA(Cohen's Kappa, Krippendorff's α) 기반 품질 측정 체계
- 4장. Active Learning 기반 샘플링 전략
- 5장. 주석 버전 관리 및 Golden Dataset 구축법

---

#### 2-3. 온톨로지

**Sub-agent 전문가 구성**

1. **온톨로지 아키텍트** — 도메인 개념 모델링
2. **Knowledge Graph 엔지니어** — RDF/Property Graph 구현
3. **SME Interview 전문가** — 암묵지 추출·구조화
4. **온톨로지-LLM 연계 전문가** — GraphRAG, KG-augmented LLM
5. **온톨로지 진화 관리자** — 버전·확장성 관리

**방법론·데이터 체계 목차**

- 1장. 도메인 온톨로지 구축 방법론 (Top-down vs Bottom-up)
- 2장. 핵심 개체(Entity)·관계(Relation)·속성(Property) 정의
- 3장. Knowledge Graph 구축 및 저장 기술 선택 (Neo4j, GraphDB, TigerGraph)
- 4장. LLM과 KG 결합 패턴 (GraphRAG, Cypher 생성)
- 5장. 온톨로지 버전·확장·폐기 거버넌스

---

### Pillar 3. Trustworthy

#### 3-1. Observability

**Sub-agent 전문가 구성**

1. **데이터 파이프라인 모니터링 엔지니어** — 단계별 가시성 확보
2. **Anomaly Detection 전문가** — 통계·ML 기반 이상 탐지
3. **AI 추론 모니터링 전문가** — Drift, Hallucination 감지
4. **Root Cause Analysis(RCA) 전문가** — 데이터 vs 모델 원인 분리
5. **알림·대응 자동화 엔지니어** — Incident Response 체계

**방법론·데이터 체계 목차**

- 1장. 데이터 Observability 5대 지표 (Freshness, Volume, Schema, Distribution, Lineage)
- 2장. 자동 이상 탐지 알고리즘 (Statistical, ML-based, LLM-judge)
- 3장. Data Drift vs Model Drift 분리 진단 프레임워크
- 4장. RCA 자동화 (Lineage + Metric Correlation)
- 5장. SLO/SLI 기반 Incident Response Playbook

---

#### 3-2. 데이터 품질/권한 관리

**Sub-agent 전문가 구성**

1. **데이터 품질 룰 엔지니어** — DQ 규칙 정의·측정
2. **접근 권한(Access Control) 설계자** — RBAC/ABAC/PBAC
3. **AI 학습 데이터 적격성 심사관** — 학습 가능 여부 판정
4. **컴플라이언스 전문가** — GDPR·개보법·산업 규제
5. **권한 감사(Audit) 전문가** — 접근 로그 분석

**방법론·데이터 체계 목차**

- 1장. 데이터 품질 차원 정의 (정확성·완전성·일관성·적시성·유효성·유일성)
- 2장. AI 학습 데이터 적격성 체크리스트 (저작권, 동의, 민감도)
- 3장. RBAC/ABAC 기반 다층 권한 모델 설계
- 4장. Policy-as-Code 기반 접근 통제 자동화 (OPA, Ranger)
- 5장. 권한 사용 감사 및 이상 접근 탐지

---

#### 3-3. 데이터 계통(Lineage)

**Sub-agent 전문가 구성**

1. **Lineage 수집 엔지니어** — Column-level Lineage 자동 추출
2. **End-to-End Lineage 통합 전문가** — 시스템 간 연결
3. **AI 모델 Lineage 전문가** — Feature→Model→Output 추적
4. **영향도 분석(Impact Analysis) 전문가** — 변경 파급효과 분석
5. **규제 대응 Lineage 전문가** — 감사·증빙 자동화

**방법론·데이터 체계 목차**

- 1장. Lineage 수준 정의 (Table, Column, Cell, Model Feature)
- 2장. OpenLineage 표준 기반 자동 수집 아키텍처
- 3장. End-to-End Lineage (Source → ETL → ML → AI Output)
- 4장. 영향도 분석 및 Backward/Forward Tracing
- 5장. 규제 대응 Lineage 리포팅 자동화

---

### Pillar 4. Governed

#### 4-1. 데이터 운영관리

**Sub-agent 전문가 구성**

1. **DataOps 아키텍트** — End-to-End 파이프라인 설계
2. **오케스트레이션 엔지니어** — Airflow/Dagster/Prefect 운영
3. **장애 복구(Resilience) 전문가** — Failure Handling
4. **협업 거버넌스 전문가** — Multi-team 충돌 관리
5. **CI/CD for Data 전문가** — 데이터 자산 배포 자동화

**방법론·데이터 체계 목차**

- 1장. DataOps 성숙도 모델 및 진단
- 2장. 파이프라인 오케스트레이션 아키텍처 패턴
- 3장. Failure-aware 파이프라인 설계 (재시도, 백필, 격리)
- 4장. Data Contract 기반 팀 간 협업 모델
- 5장. CI/CD for Data (테스트, 배포, 롤백)

---

#### 4-2. 데이터 생명주기 관리

**Sub-agent 전문가 구성**

1. **데이터 분류 전문가** — Retention Class 설계
2. **법적 보존(Legal Hold) 전문가** — 규제 보존 의무 매핑
3. **Tiered Storage 엔지니어** — Hot/Warm/Cold/Archive
4. **자동 폐기·이관 엔지니어** — Lifecycle Automation
5. **비용 최적화(FinOps) 전문가** — 저장 비용 관리

**방법론·데이터 체계 목차**

- 1장. 데이터 분류 체계 및 Retention Policy 매트릭스
- 2장. 법적 보존·삭제 의무 매핑 (개보법·GDPR·산업법)
- 3장. Tiered Storage 자동 이관 아키텍처
- 4장. 폐기 자동화 및 증빙 (Right to be Forgotten)
- 5장. 데이터 저장 비용 모니터링·최적화

---

#### 4-3. 데이터 디지털화

**Sub-agent 전문가 구성**

1. **OCR/Document AI 전문가** — 문서·수기 인식
2. **Vision/Multimodal 전문가** — 사진·이미지 정보 추출
3. **현장 프로세스 재설계자** — Digital-Native 업무 흐름 설계
4. **품질 검증(QA) 전문가** — 디지털화 정확도 검증
5. **레거시 마이그레이션 전문가** — 종이→디지털 일괄 전환

**방법론·데이터 체계 목차**

- 1장. 디지털화 대상 자산 인벤토리 및 우선순위 (ROI 기반)
- 2장. OCR/VLM 기반 자동 추출 파이프라인 설계
- 3장. 디지털화 품질 검증 체계 (Sample QA, Double-entry)
- 4장. Digital-First 현장 업무 프로세스 재설계
- 5장. 디지털화 자산 카탈로그 등록 및 활용 체계

---

#### 4-4. 데이터 보안 (민감정보 마스킹)

**Sub-agent 전문가 구성**

1. **PII/PHI 탐지 엔지니어** — NLP/Pattern 기반 자동 탐지
2. **마스킹 기법 설계자** — Tokenization, Pseudonymization, DP
3. **AI 성능-프라이버시 균형 전문가** — Utility-Privacy Tradeoff
4. **컨텍스트 기반 동적 마스킹 전문가** — 목적별 차등 적용
5. **재식별 위험 평가(Re-identification) 전문가** — k-anonymity 평가

**방법론·데이터 체계 목차**

- 1장. 비정형 데이터 민감정보 자동 탐지 (NER, Regex, LLM)
- 2장. 마스킹 기법 카탈로그 (Redaction, Hashing, Tokenization, DP)
- 3장. AI Utility 보존 마스킹 전략 (Format-preserving Encryption 등)
- 4장. 사용 목적·역할 기반 동적 마스킹 정책 (ABAC + Masking)
- 5장. 재식별 위험 평가 및 차등 프라이버시 적용

---

### Pillar 5. Evolvable

#### 5-1. 데이터 Product화

**Sub-agent 전문가 구성**

1. **Data Product 매니저(DPM)** — Product Thinking 적용
2. **Data Mesh 아키텍트** — Domain-oriented Ownership
3. **셀프서비스 플랫폼 엔지니어** — Discovery·Provisioning UX
4. **Data Contract 전문가** — Producer-Consumer 계약
5. **Data Product 측정 전문가** — 사용량·만족도 KPI

**방법론·데이터 체계 목차**

- 1장. Data Product 정의 및 분류 (Source-aligned, Aggregate, Consumer-aligned)
- 2장. Data Product 캔버스 (Owner, SLA, Schema, Quality, Cost)
- 3장. 셀프서비스 Discovery·Access 플랫폼 설계
- 4장. Data Contract 표준화 및 자동 검증
- 5장. Data Product KPI (Adoption, Reuse, NPS)

---

#### 5-2. 합성데이터(Synthetic)

**Sub-agent 전문가 구성**

1. **생성 모델 엔지니어** — GAN, VAE, Diffusion, LLM 기반 생성
2. **통계적 충실도(Fidelity) 평가 전문가** — 분포·상관 보존 검증
3. **프라이버시 평가 전문가** — Membership Inference Risk
4. **편향(Bias) 분석가** — 합성 데이터 편향 진단
5. **활용 시나리오 설계자** — Use case별 생성 전략

**방법론·데이터 체계 목차**

- 1장. 합성데이터 활용 시나리오 (희소 데이터 보강, 프라이버시, 시뮬레이션)
- 2장. 생성 기법 선택 가이드 (Tabular, Text, Image, Time-series)
- 3장. Fidelity 평가 지표 (Statistical Similarity, ML Efficacy)
- 4장. Privacy 평가 (Membership Inference, Attribute Disclosure)
- 5장. 편향·과적합 방지 및 거버넌스

---

#### 5-3. AI 평가 데이터

**Sub-agent 전문가 구성**

1. **Eval Set 설계자** — Golden Dataset 구축
2. **도메인 SME Coordinator** — 전문가 판단 수집
3. **LLM-as-a-Judge 엔지니어** — 자동 평가 체계
4. **Benchmark 운영 전문가** — 지속적 평가 인프라
5. **Eval 데이터 거버넌스 전문가** — 버전·라이선스 관리

**방법론·데이터 체계 목차**

- 1장. 평가 차원 정의 (Accuracy, Faithfulness, Safety, Tone, Cost)
- 2장. Golden Dataset 구축 방법론 (Stratified Sampling, Edge Cases)
- 3장. SME 판단 수집 워크플로우 (Pairwise, Likert, Rubric)
- 4장. LLM-as-a-Judge + Human Calibration 체계
- 5장. 평가 데이터 버전 관리 및 Continuous Evaluation

---

#### 5-4. 데이터 파이프라인 Feedback Loop

**Sub-agent 전문가 구성**

1. **Agent Telemetry 엔지니어** — 실행 로그·결과 수집
2. **피드백 라벨링 자동화 전문가** — 사용자/시스템 피드백 구조화
3. **Continuous Learning 엔지니어** — 재학습 트리거 설계
4. **Prompt 개선 자동화 전문가** — Auto-prompt Optimization
5. **A/B Testing 전문가** — 개선 효과 검증

**방법론·데이터 체계 목차**

- 1장. Agent 실행 Telemetry 스키마 설계 (Trace, Tool calls, Outcomes)
- 2장. 피드백 수집 채널 (Implicit, Explicit, System-detected)
- 3장. Feedback → Training Data 변환 파이프라인
- 4장. Continuous Improvement 주기 설계 (Daily, Weekly, On-demand)
- 5장. A/B 실험 및 점진적 롤아웃 체계

---

### Pillar 6. Actionable

#### 6-1. Physical 데이터

**Sub-agent 전문가 구성**

1. **IoT/센서 통합 엔지니어** — 프로토콜 통합 (MQTT, OPC-UA)
2. **Edge Computing 전문가** — Edge 전처리·필터링
3. **Time-series 데이터 엔지니어** — TSDB 설계·운영
4. **Stream Processing 전문가** — Kafka, Flink 실시간 처리
5. **Digital Twin 아키텍트** — 물리-가상 연계

**방법론·데이터 체계 목차**

- 1장. 현장 데이터 소스 인벤토리 (설비, 센서, PLC, SCADA)
- 2장. 통합 수집 아키텍처 (OPC-UA, MQTT, Kafka)
- 3장. Edge 전처리 및 노이즈/이상값 필터링
- 4장. Stream vs Batch 분기 설계 기준
- 5장. Digital Twin 데이터 모델 및 실시간 동기화

---

#### 6-2. API/Tool 연계 데이터

**Sub-agent 전문가 구성**

1. **Tool 표준화 아키텍트** — Tool Schema 정의 (OpenAPI, JSON Schema)
2. **MCP/Function Calling 전문가** — 공통 프로토콜 구현
3. **Tool Registry 운영자** — 등록·버전·Deprecation
4. **Tool 실행 로그 엔지니어** — Call History 자산화
5. **Tool 보안·인증 전문가** — Auth, Rate Limit, Sandbox

**방법론·데이터 체계 목차**

- 1장. Tool 인벤토리 및 분류 체계 (Read, Write, Compute, External)
- 2장. Tool Schema 표준화 (이름, 입력, 출력, 부작용, 비용)
- 3장. MCP 기반 Tool Registry 아키텍처
- 4장. Tool Call Telemetry 및 재사용 자산화
- 5장. Tool 보안·인증·Sandboxing 모델

---

#### 6-3. Prompt/Harness 자산화

**Sub-agent 전문가 구성**

1. **Prompt 엔지니어링 표준화 전문가** — 작성 패턴·템플릿
2. **업무 분석(Task Decomposition) 전문가** — 사람의 판단 구조화
3. **Prompt 버전·CMS 운영자** — Git for Prompts
4. **Prompt 평가·회귀 테스트 전문가** — Prompt Regression
5. **Harness/Agent Framework 아키텍트** — 실행 환경 표준화

**방법론·데이터 체계 목차**

- 1장. 업무 → Prompt 구조화 방법론 (SOP → Prompt 패턴 매핑)
- 2장. Prompt 작성 표준 및 템플릿 라이브러리
- 3장. Prompt 버전 관리 시스템 (Git, PromptLayer, LangSmith)
- 4장. Prompt 성능 회귀 테스트 및 모니터링
- 5장. Harness 표준화 (Agent Framework, Tool 연결, 메모리 관리)

## Backlinks
- [[주제별 백서 구성 및 Key Question 답변 가이드]]
