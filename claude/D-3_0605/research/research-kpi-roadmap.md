# D-3 Prompt/Harness 자산화 — KPI & Roadmap 리서치

**주제코드:** D-3  
**주제명:** Prompt/Harness 자산화 (Prompt/Harness Assetization)  
**정의:** 사람의 업무 절차와 판단 기준을 AI가 실행 가능한 Prompt와 Harness 형태로 구조화·관리하는 체계.  
**담당 클러스터:** KPI / Roadmap  
**작성일:** 2026-06-05  

---

## 1. 관점 설정 — "AI 구축"이 아니라 "지시·판단 로직의 데이터 자산화"

Prompt/Harness 자산화 KPI는 모델 성능(accuracy, BLEU 등)을 측정하는 것이 아니라,
**"업무 절차와 판단 기준이 얼마나 잘 데이터 자산으로 전환·관리되고 있는가"** 를 측정한다.

핵심 관점 전환:
- 모델 품질 → **자산 커버리지·완전성·재사용성**
- 개발 속도 → **자산 거버넌스 준수율·변경 리드타임**
- 운영 안정성 → **품질 회귀 감지·드리프트 조기 탐지**

이 관점은 Langfuse, LangSmith, PromptLayer, Humanloop 등 LLMOps 플랫폼이 실제로 추적하는 메트릭 체계와도 일치한다.
이들 플랫폼은 공통적으로 Volume / Cost / Latency / Quality / Lifecycle을 Prompt 버전 단위로 슬라이싱하여 추적한다.

---

## 2. KPI 지표 설계 (5개)

### KPI-1. Prompt 자산화율 (Prompt Catalog Coverage)

| 항목 | 내용 |
|------|------|
| **지표명** | Prompt 자산화율 (Prompt Inventory Coverage Rate) |
| **쉬운 뜻** | "실제 운영 중인 Prompt 중 Registry에 등록·관리되는 비율" |
| **측정 목적** | 아직 코드나 개인 파일에 묻혀 있는 Prompt를 발굴해 조직 자산으로 편입하는 진척 관리 |
| **목표 방향** | ↑ (높을수록 좋음) |
| **산식** | (Registry 등록 Prompt 수 / 전체 운영 Prompt 추정 수) × 100 |
| **목표값** | 단기 50% → 중기 80% → 장기 95% 이상 |
| **측정 도구** | LangSmith Prompt Hub, Langfuse Prompt Management, PromptLayer Prompt Registry |

**기대효과 (두산 제조 맥락)**  
설비 점검 절차 판단 로직, 품질 이상 분류 기준, 설계 검토 체크리스트 등 현장 경험치가 개인 PC나 팀 채팅방에 산재하는 경우 다수. 자산화율 KPI 도입 시 이를 체계적으로 끌어올려 조직 지식의 휘발 방지 가능.

> **근거:** Digital Applied (2026). "Catalog coverage is the cheapest, highest-signal indicator of whether the library is being maintained or just accumulated. Target: >95%."  
> URL: https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026

---

### KPI-2. 메타데이터 완성도 (Prompt Metadata Completeness)

| 항목 | 내용 |
|------|------|
| **지표명** | Prompt 메타데이터 완성도 (Metadata Completeness Rate) |
| **쉬운 뜻** | "등록된 Prompt가 소유자·버전·용도·연계 모델·업무 맥락 등 필수 필드를 모두 채운 비율" |
| **측정 목적** | 메타데이터 미비로 인한 자산 재사용 불가·거버넌스 공백·감사 추적 실패 방지 |
| **목표 방향** | ↑ |
| **산식** | (6개 필수 필드 전부 입력된 Prompt 수 / 전체 Registry 등록 Prompt 수) × 100 |
| **필수 필드** | 소유자(Owner), 버전(Version), 모델(Model), 최종 수정일(Last-edited), 라이프사이클 단계(Lifecycle Stage), 연계 기능·업무(Feature/Use-case) |
| **목표값** | 80% 이상 유지 (미충족 시 배포 블록) |
| **측정 도구** | Langfuse Prompt Management, PromptLayer, 내부 Registry CI 규칙 |

**기대효과 (두산 제조 맥락)**  
밥캣 굴착기 이상진단 Prompt의 설계 근거 문서화, 테스나 웨이퍼 불량 판정 기준 버전 추적 등. 감사·인증 시 "이 판단 로직은 언제 누가 왜 만들었나"를 즉각 답변 가능.

> **근거:** Digital Applied (2026). "A prompt missing any of those fields [owner, model, version, edit date, lifecycle stage, feature] is a partial entry. Target above 95%."  
> URL: https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026  
> Nexla (2025). AI Metadata best practices. https://nexla.com/ai-readiness/ai-metadata/

---

### KPI-3. 버전관리·승인 준수율 (Governance Compliance Rate)

| 항목 | 내용 |
|------|------|
| **지표명** | 버전관리·승인 준수율 (Version Control & Approval Compliance Rate) |
| **쉬운 뜻** | "Prompt 변경 시 Git-based 버전관리 + 리뷰어 승인 절차를 거친 비율" |
| **측정 목적** | 무단 변경으로 인한 판단 로직 훼손·사고 방지, 감사 추적(Audit Trail) 확보 |
| **목표 방향** | ↑ (100%가 이상) |
| **산식** | (버전 태깅 + 리뷰어 승인 기록이 있는 변경 건수 / 전체 Prompt 변경 건수) × 100 |
| **목표값** | 단기 70% → 장기 100% |
| **측정 도구** | LangSmith (버전 diff 추적), Humanloop (승인 워크플로우), PromptLayer (audit log) |

**기대효과 (두산 제조 맥락)**  
품질검사 판정 기준 Prompt를 현장 담당자가 임의 수정 → 불량 유출 사고 발생 리스크 차단. 변경 이력이 있어 사고 발생 시 root cause 분석 가능.

> **근거:** Medium - AISquare (2026). "Every change to a prompt must be logged, and this audit trail is essential for compliance, especially in regulated industries."  
> URL: https://medium.com/@aisquarecommunity/prompt-governance-scale-versioning-access-and-audits-c76b58a166f1  
> Humanloop Docs. Best practices for production deployments. https://www.zenml.io/llmops-database/best-practices-for-llm-production-deployments-evaluation-prompt-management-and-fine-tuning

---

### KPI-4. Prompt 품질 평가 통과율 — 회귀 테스트 기반 (Eval Pass / Regression Rate)

| 항목 | 내용 |
|------|------|
| **지표명** | Prompt 회귀 테스트 통과율 / 일일 회귀 발생률 (Regression Pass Rate / Daily Regression Rate) |
| **쉬운 뜻** | "변경된 Prompt가 기존 골든셋(Gold Set) 테스트를 얼마나 통과하는가; 전날 대비 품질 하락이 발생한 Prompt 비율" |
| **측정 목적** | 모델 업데이트·Prompt 수정 시 판단 로직 품질 저하를 고객·현장 영향 전에 사전 탐지 |
| **목표 방향** | 통과율 ↑ / 일일 회귀 발생률 ↓ |
| **산식** | - 통과율: (Gold Set 통과 케이스 / 전체 테스트 케이스) × 100  - 일일 회귀 발생률: (전일 대비 품질 하락 Prompt 수 / 평가 대상 Prompt 수) × 100 |
| **목표값** | 통과율 90% 이상; 일일 회귀 발생률 2% 이하 (5% 초과 시 즉각 조사 트리거) |
| **측정 도구** | Langfuse Eval, LangSmith Evaluations, Humanloop Evaluations, Promptfoo, DeepEval |

**기대효과 (두산 제조 맥락)**  
LLM API 공급사가 모델을 업데이트했을 때 설비 이상 탐지 Prompt의 판정 기준이 미묘하게 변하는 "조용한 드리프트(Silent Drift)"를 현장 사고 발생 전에 자동 탐지.

> **근거:** Langfuse (2024). "Quality is measured through user feedback, model-based scoring, and assessed over time as well as across prompt versions."  
> URL: https://langfuse.com/faq/all/llm-analytics-101  
> Digital Applied (2026). "Regression rate — failing prompts ÷ evaluated prompts — is the leading indicator of customer-visible quality drops. Target: ≤2% daily."  
> URL: https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026  
> Statsig (2025). Prompt regression testing. https://www.statsig.com/perspectives/slug-prompt-regression-testing

---

### KPI-5. Harness 재사용률 (Harness/Prompt Reuse Rate)

| 항목 | 내용 |
|------|------|
| **지표명** | Harness·Prompt 재사용률 (Reuse Rate across Projects) |
| **쉬운 뜻** | "등록된 Harness/Prompt 자산이 2개 이상의 서비스·프로젝트에서 재활용되는 비율" |
| **측정 목적** | 자산화의 실질 가치 검증 — 재사용 없으면 Registry는 파일함에 불과 |
| **목표 방향** | ↑ |
| **산식** | (2개 이상 프로젝트에서 호출된 Prompt/Harness 수 / 전체 Registry 자산 수) × 100 |
| **목표값** | 단기 20% → 중기 40% → 장기 60% |
| **측정 도구** | LangSmith (Prompt Hub 활용 현황), Langfuse (prompt 호출 추적), PromptLayer (usage analytics) |

**기대효과 (두산 제조 맥락)**  
DX 부문에서 개발한 "설비 매뉴얼 요약 Harness"를 밥캣 서비스팀·두산에너빌리티 O&M팀이 공동 활용. 중복 개발 비용 절감, 판단 기준 일관성 확보.

> **근거:** PromptLayer Docs. "Track cost, latency, usage and feedback for each prompt version."  
> URL: https://docs.promptlayer.com/why-promptlayer/analytics  
> Harness Engineering (Medium, 2026). "Prompt caching is the most effective harness-level cost lever: caching repeated system prompts, tool definitions across requests for maximum reuse."  
> URL: https://medium.com/@cenrunzhe/from-prompt-engineering-to-harness-engineering-the-layer-that-makes-ai-agents-actually-work-466fe0489fbe

---

## 3. KPI 요약표

| # | 지표명 | 방향 | 단기 목표 | 장기 목표 | 주요 측정 도구 |
|---|--------|------|-----------|-----------|---------------|
| KPI-1 | Prompt 자산화율 | ↑ | 50% | 95% | LangSmith, Langfuse, PromptLayer |
| KPI-2 | 메타데이터 완성도 | ↑ | 70% | 95% | Langfuse, PromptLayer |
| KPI-3 | 버전관리·승인 준수율 | ↑ | 70% | 100% | Humanloop, LangSmith |
| KPI-4 | 회귀 테스트 통과율 / 일일 회귀 발생률 | 통과율 ↑ / 발생률 ↓ | 통과율 80% / 발생률 <5% | 통과율 90% / 발생률 <2% | Langfuse Eval, DeepEval |
| KPI-5 | Harness·Prompt 재사용률 | ↑ | 20% | 60% | LangSmith, PromptLayer |

---

## 4. Roadmap — 단기 / 중기 / 장기

### 관점 고정
로드맵은 "AI 기능 구축"이 아니라 **"지시·판단 로직을 데이터 자산으로 정비하는 성숙도 향상"** 관점으로 설계.  
Microsoft Azure GenAIOps 성숙도 모델(Level 1~4)과 Digital Applied 10-KPI 프레임워크를 참조.

---

### 단기 (0~6개월) — 인벤토리·Registry·메타데이터 구축

**목표:** "우리 조직에 어떤 Prompt/Harness가 있는가"를 처음으로 가시화

| 과제 | 내용 |
|------|------|
| Prompt 인벤토리 수행 | 코드베이스, 공유 드라이브, 개인 문서 전수 탐색 → 운영 Prompt 목록화 |
| Prompt Registry 도입 | LangSmith Prompt Hub 또는 Langfuse Prompt Management 도입. 중앙 저장소 1개로 단일화 |
| 메타데이터 스키마 정의 | 필수 6개 필드(소유자, 버전, 모델, 수정일, 라이프사이클 단계, 연계 업무) 표준화 |
| 버전관리 기반 배포 | Git 연동 또는 플랫폼 내 버전 태깅 → "코드처럼 관리" 출발점 |
| KPI 베이스라인 측정 | KPI-1~2 초기값 측정; 대시보드 최초 구축 |
| 소유권(Ownership) 할당 | 모든 Prompt에 단일 소유자 지정 (공동 소유 금지 원칙) |

**추가 기술:** Git/GitHub(버전관리), 스프레드시트 기반 카탈로그, LangSmith/Langfuse (무료 티어)  
**연계 주제:** 데이터 카탈로그(D-1), 메타데이터 관리(D-2)  

> **근거:** Microsoft Azure (2025). GenAIOps Level 1 - "Start experimenting with structured prompt design and basic prompt engineering."  
> URL: https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/concept-llmops-maturity  
> Digital Applied (2026). "Catalog coverage moves from 40-60% to above 90% within two weeks of committing to the metric."  
> URL: https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026

---

### 중기 (6~18개월) — 리니지·의존성맵·거버넌스·평가셋 연계

**목표:** "이 Prompt가 어디서 왔고 어디에 쓰이며 얼마나 잘 작동하는가"를 측정 가능하게

| 과제 | 내용 |
|------|------|
| Prompt 리니지·의존성 맵 | Prompt → 모델 → 데이터소스 → 다운스트림 서비스 간 의존성 시각화. 변경 영향 범위 사전 파악 |
| 거버넌스 승인 워크플로우 | Prompt 변경 시 리뷰어(도메인 전문가 + AI 엔지니어) 승인 의무화. Humanloop/LangSmith 워크플로우 활용 |
| 골든셋(Gold Set) 구축 | 각 Prompt별 10~50개 대표 테스트 케이스 작성. 정답 포함. 회귀 테스트 기반 마련 |
| 자동 회귀 테스트 CI/CD 연동 | Prompt 변경 → CI 파이프라인에서 자동 골든셋 평가 → 품질 하락 시 배포 블록 |
| 라이프사이클 정책 시행 | Draft → Production → Deprecated → Retired 4단계 표준화. Deprecated 전환 후 60일 내 Retired 강제 |
| 재사용 메커니즘 | Prompt Library UI를 통한 검색·브라우징 제공. Harness 재사용 사례 가이드 발행 |
| KPI 전체 측정 체계 완성 | KPI-3·4·5 측정 개시. 주간 스탠드업에서 대시보드 리뷰 루틴화 |

**추가 기술:** Langfuse/LangSmith 고급 평가(LLM-as-a-Judge), DeepEval/Promptfoo, CI/CD(GitHub Actions), 의존성 그래프 도구  
**연계 주제:** 데이터 리니지(D-4), 데이터 거버넌스(D-6), 평가 데이터(D-8)  

> **근거:** Langfuse Docs. "Quality is assessed over time as well as across prompt versions — side-by-side analytics that highlight performance differences."  
> URL: https://langfuse.com/docs/prompt-management/overview  
> Traceloop Blog (2025). "CI/CD Integration automates evaluation, failing a build if a prompt change causes a quality regression."  
> URL: https://www.traceloop.com/blog/automated-prompt-regression-testing-with-llm-as-a-judge-and-ci-cd  
> Microsoft Azure (2025). GenAIOps Level 2 - "Implement CI/CD integration, employ advanced evaluation metrics like groundedness, relevance, and similarity."  
> URL: https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/concept-llmops-maturity

---

### 장기 (18개월~) — 품질 자동화·드리프트 탐지·타 데이터 주제 통합

**목표:** "Prompt/Harness 자산이 자기 건강을 스스로 모니터링하고, 전사 데이터 체계와 통합된다"

| 과제 | 내용 |
|------|------|
| 자동 드리프트 탐지 | 모델 업데이트·데이터 분포 변화를 야간 자동 평가로 탐지. 품질 하락 2% 초과 시 소유자 자동 알림 |
| LLM-as-a-Judge 평가 자동화 | 사람이 레이블링 없이 LLM이 출력 품질을 자동 채점. 평가 커버리지 80% 이상 유지 |
| 모델 적합 다이버전스(Model-Fit Divergence) 추적 | 동일 Prompt를 저비용 모델로 전환 가능 여부 자동 평가 → 비용 최적화 |
| 데이터 카탈로그 통합 | Prompt/Harness 자산을 기업 데이터 카탈로그(예: Apache Atlas, Collibra)에 First-class 항목으로 등록 |
| Glossary·온톨로지 연계 | 업무 용어 정의(Glossary)와 Prompt 내 용어 정합성 자동 검증. 용어 변경 시 영향받는 Prompt 자동 식별 |
| Feedback Loop 구조화 | 현장 사용자 피드백(좋음/나쁨, 수정 요청) → Prompt 개선 → 골든셋 자동 확장 선순환 구축 |
| 전사 거버넌스 위원회 | Prompt/Harness 포트폴리오 운영 위원회 설치 (IT + 비즈니스 도메인 전문가 협력) |
| KPI 스테이크홀더 보고 | 분기별 경영진 보고: 자산화율 추이, 비용 절감액(재사용), 사고 예방 건수(회귀 탐지) |

**추가 기술:** 고급 LLMOps 플랫폼 (Arize AI, Datadog LLM Observability), 엔터프라이즈 데이터 카탈로그 연동 API, Feature Store 개념 확장  
**연계 주제:** 데이터 카탈로그(D-1), Glossary(D-5), 평가 데이터 자산(D-8), Feedback Loop(D-9), 데이터 거버넌스(D-6)  

> **근거:** Microsoft Azure (2025). GenAIOps Level 4 - "Sophisticated approach to LLM application development, deployment, and monitoring. Continuously evaluate alignment with evolving business objectives."  
> URL: https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/concept-llmops-maturity  
> Agenta Blog (2025). "Prompt Drift: What It Is and How to Detect It — model updates, changing user patterns, and evolving data distributions cause output drift."  
> URL: https://agenta.ai/blog/prompt-drift  
> Digital Applied (2026). "Model-fit divergence surfaces the third of the library that can move down a price tier without quality loss."  
> URL: https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026

---

### 로드맵 한눈에 보기

```
단기 (0~6개월)              중기 (6~18개월)            장기 (18개월~)
─────────────────────       ─────────────────────       ─────────────────────
인벤토리 수행               리니지·의존성 맵            자동 드리프트 탐지
Registry 도입               거버넌스 승인 워크플로우     LLM-as-a-Judge 자동화
메타데이터 스키마            골든셋 구축·CI 연동          모델 적합 다이버전스 추적
버전관리 기반 배포           라이프사이클 정책 시행       데이터 카탈로그 통합
소유권 할당                 재사용 메커니즘              Glossary·온톨로지 연계
KPI-1,2 베이스라인           KPI-3,4,5 전체 측정          Feedback Loop 선순환
                                                       전사 거버넌스 위원회
```

---

## 5. 두산 제조 현업 적용 시 기대효과 종합

| 단계 | 현업 시나리오 | 기대효과 |
|------|--------------|----------|
| 단기 | 설비 점검 체크리스트 Prompt 50개 Registry 등록 | "누가 만든 판단 로직인지" 즉시 확인. 신규 엔지니어 온보딩 시간 단축 |
| 중기 | 품질 불량 판정 Harness에 골든셋 100케이스 연결 | LLM API 업그레이드 시 품질 하락 자동 감지 → 설비 라인 영향 사전 차단 |
| 중기 | Harness 재사용률 40% 달성 | 밥캣·테스나·두산에너빌리티 공통 Prompt 중복 개발 제거 → 연 개발비 ~30% 절감 추정 |
| 장기 | 전사 데이터 카탈로그 연동 | "이 판단 Prompt는 어떤 학습 데이터·설비 데이터를 쓰는가" 리니지 조회 가능 → ISO/IEC AI 감사 대응 |
| 장기 | Feedback Loop 구축 | 현장 오판정 피드백 → 골든셋 자동 확장 → Prompt 품질 지속 개선 선순환 |

---

## 출처

| # | 출처명 | URL |
|---|--------|-----|
| 1 | Digital Applied. "Prompt Library Metrics: Coverage, Regression Framework 2026" (2026-05-15) | https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026 |
| 2 | Langfuse. "LLM Analytics 101 — How to Measure and Improve Your LLM Application" | https://langfuse.com/faq/all/llm-analytics-101 |
| 3 | Langfuse. "Open Source Prompt Management - Overview" | https://langfuse.com/docs/prompt-management/overview |
| 4 | LangChain. "LangSmith: AI Agent & LLM Observability Platform" | https://www.langchain.com/langsmith/observability |
| 5 | LangChain. "LangSmith - LLM & AI Agent Evals Platform" | https://www.langchain.com/langsmith/evaluation |
| 6 | Humanloop. "What is Prompt Management?" | https://humanloop.com/blog/prompt-management |
| 7 | Humanloop. "LLM Evaluation for AI Apps" | https://humanloop.com/platform/evaluations |
| 8 | PromptLayer. "Analytics" | https://docs.promptlayer.com/why-promptlayer/analytics |
| 9 | Medium - AISquare. "Prompt Governance Scale: Versioning, Access, and Audits" (2026-03) | https://medium.com/@aisquarecommunity/prompt-governance-scale-versioning-access-and-audits-c76b58a166f1 |
| 10 | Medium - Segev Shmueli. "Managing Prompts as Enterprise Assets: A Portfolio Approach" | https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327 |
| 11 | Microsoft Azure. "Advance your maturity level for GenAIOps" (2025-11-18) | https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/concept-llmops-maturity |
| 12 | Microsoft Azure Blog. "Achieve generative AI operational excellence with the LLMOps maturity model" | https://azure.microsoft.com/en-us/blog/achieve-generative-ai-operational-excellence-with-the-llmops-maturity-model/ |
| 13 | Traceloop Blog. "Automated Prompt Regression Testing with LLM-as-a-Judge and CI/CD" | https://www.traceloop.com/blog/automated-prompt-regression-testing-with-llm-as-a-judge-and-ci-cd |
| 14 | Agenta Blog. "Prompt Drift: What It Is and How to Detect It" | https://agenta.ai/blog/prompt-drift |
| 15 | Statsig. "Prompt regression testing: Preventing quality decay" | https://www.statsig.com/perspectives/slug-prompt-regression-testing |
| 16 | Medium - Steven Cen. "From Prompt Engineering to Harness Engineering" (2026-03) | https://medium.com/@cenrunzhe/from-prompt-engineering-to-harness-engineering-the-layer-that-makes-ai-agents-actually-work-466fe0489fbe |
| 17 | Atlan. "Prompt vs Context vs Harness Engineering: Key Differences" | https://atlan.com/know/harness-engineering-vs-prompt-engineering/ |
| 18 | AWS Prescriptive Guidance. "Prompt, agent, and model lifecycle management" | https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html |
| 19 | ZenML Blog. "HumanLoop: Best Practices for LLM Production Deployments" | https://www.zenml.io/llmops-database/best-practices-for-llm-production-deployments-evaluation-prompt-management-and-fine-tuning |
| 20 | Nexla. "AI Metadata: Key Concepts & Best Practices" | https://nexla.com/ai-readiness/ai-metadata/ |
| 21 | Braintrust. "What is prompt versioning? Best practices for iteration" | https://www.braintrust.dev/articles/what-is-prompt-versioning |
| 22 | C# Corner. "How Enterprise Teams Are Managing Thousands of AI Prompts in Production" | https://www.c-sharpcorner.com/article/how-enterprise-teams-are-managing-thousands-of-ai-prompts-in-production/ |
| 23 | Datadog Blog. "Track, compare, and optimize your LLM prompts with Datadog LLM Observability" | https://www.datadoghq.com/blog/llm-prompt-tracking/ |
| 24 | NexaStack. "Assessing Your Enterprise's LLMOps Maturity: A Strategic Self-Audit" | https://www.nexastack.ai/blog/enterprise-llmops-maturity |
