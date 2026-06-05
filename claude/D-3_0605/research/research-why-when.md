# D-3 Prompt/Harness 자산화 — Why & When 리서치
> 주제코드: D-3 | 주제명: Prompt/Harness 자산화 (Prompt/Harness Assetization)
> 클러스터: Why / When | 작성일: 2026-06-05

---

## 1. 개념 정의: Prompt와 Harness는 무엇인가

### 1-1. Prompt (프롬프트)
Prompt란 사람이 AI 모델에게 내리는 **지시·판단 기준을 담은 자연어 명세(specification)**다.
코드가 "어떻게 처리하라"를 명시한다면, Prompt는 "어떤 관점·기준으로 판단하라"를 명시한다.

- 예시 ①: `"이 코드를 검토해 줘"` — 모호, 표면적 오류만 찾음
- 예시 ②: `"성능·신뢰성·스타일 관점에서 이 코드를 검토해 줘"` — 구체적, 심층 검토

단 한 단어의 차이가 AI 출력 품질과 범위를 결정한다.
[출처: Tessl, "Prompts are the new source code"](https://tessl.io/blog/prompts-are-the-new-source-code/)

### 1-2. Harness (하네스)
Harness란 LLM(대형 언어 모델)을 **감싸는 실행 제어 레이어(operating layer)**다.
모델이 "토큰을 생성하는 두뇌"라면, Harness는 "두뇌가 무엇을 보고, 어떤 도구를 쓰고, 언제 멈추는지를 결정하는 신체"다.

Harness가 관장하는 6가지 도메인:
1. **Context assembly** — 모델에게 무엇을 보여줄지 선택·압축
2. **Tool connectors** — 파일시스템·API·검색·코드 실행 환경 연결
3. **Memory & state** — 컨텍스트 창을 넘는 지식 유지
4. **Control loop** — 목표 달성까지 반복 실행·재시도 제어
5. **Guardrails & policy** — 입출력 필터, 권한 경계, 안전 제약
6. **Telemetry & evaluation** — 추적·지연·품질 측정, 회귀 감지

> "모델은 점점 서로 비슷해지고 있다. 신뢰할 수 있는 프로덕션 에이전트와 예측 불가능한 데모를 가르는 것은 거의 항상 Harness 설계다."
[출처: Pinggy, "AI Harness Engineering"](https://pinggy.io/blog/best_ai_harnesses_to_supercharge_llm_models/)

---

## 2. WHY — 왜 Prompt/Harness를 "자산"으로 정비해야 하는가

### 2-1. 지금 조직에서 판단 로직이 어디에 있는가

현재 대부분 조직에서 Prompt는 다음 세 곳에 **흩어진 채로 존재**한다:

| 위치 | 문제 |
|---|---|
| 개인 PC / 메모장 | 퇴직·이동 시 소멸. 재현 불가 |
| 채팅창(ChatGPT, Claude 등) 대화 이력 | 버전 없음. 누가 무엇을 썼는지 추적 불가 |
| 소스코드 인라인(하드코딩) | 배포 주기에 묶여 수정 불가. 모델 교체 시 전면 파손 |

이 상태가 "Prompt 기술 부채(Prompt Technical Debt)"다.
Prompt 부채는 **조용히 부패한다**: 1월에 잘 동작하던 Prompt가 2월에 모델이 업데이트되면 성능이 저하되는데, 이 사실을 사용자 불만이 터지기 전까지 아무도 모른다.
[출처: seangoedecke.com, "Prompts are technical debt too"](https://www.seangoedecke.com/prompts-are-technical-debt-too/)

### 2-2. 자산화가 없으면 원리적으로 무엇이 불가능한가

#### (a) 재현 불가 (Irreproducibility)
Prompt가 버전 관리 없이 존재하면, 어제의 판단 결과를 오늘 재현할 수 없다.
어떤 Prompt 버전이 프로덕션에서 돌고 있는지, 누가 마지막에 바꿨는지, 변경 이유가 무엇인지 알 수 없다.

> "Prompt 버전 관리는 어떤 버전이 프로덕션에서 실행되는지 알게 하고, 무언가 망가졌을 때 롤백을 가능하게 한다."
[출처: Agenta, "Prompt Versioning: The Complete Guide"](https://agenta.ai/blog/prompt-versioning-guide)

#### (b) 속인화 (Knowledge Personalization / Single-Point-of-Failure)
특정 담당자만 아는 "좋은 Prompt"는 그 사람이 없으면 사라진다.
이는 제조 현장의 **베테랑 정비원 문제**와 동일한 구조다.

> [두산 제조 현장 유추] 20년 경력 정비원은 설비 이상음만 듣고도 "3번 베어링이 2주 내로 교체 시점"임을 안다. 그러나 이 판단 기준은 그의 머릿속에만 있고, 작업표준서 어디에도 없다. 정비원이 은퇴하면 이 지식은 사라진다. Prompt가 개인 채팅창에 있는 것은 이 판단 기준이 정비원 개인 노트북에만 있는 것과 같다.

MIT 연구에 따르면 조직 지식의 80%는 문서화되지 않은 채 개인의 머릿속에 존재한다.
제조업 숙련 인력의 은퇴 가속으로 이 암묵지(tacit knowledge)의 소멸 문제는 이미 임계점에 도달했다.
[출처: Augmentir, "Tacit Knowledge in Manufacturing"](https://www.augmentir.com/glossary/tacit-knowledge)

#### (c) 감사 불가 (Non-Auditability)
Prompt가 관리되지 않으면, 규제 기관이나 내부 감사팀에 "이 AI가 왜 이 결론을 냈는가"를 설명할 수 없다.

> "거버넌스 없이 관리되지 않는 Prompt는 ISO 27001, SOC 2, 그리고 확장되는 AI 특화 규제 프레임워크의 범위 안에서 즉각적인 컴플라이언스 노출을 만든다."
[출처: CertPro, "Prompt Security Risks: The Hidden Compliance Gap"](https://certpro.com/prompt-security-risks-enterprise-ai/)

#### (d) 모델 교체 시 전면 파손 (Model-Swap Catastrophe)
Prompt가 소스코드 내에 하드코딩되어 있으면, 모델을 교체할 때(GPT-4 → Claude, 또는 사내 LLM으로 전환) 모든 Prompt를 일일이 찾아 수동으로 수정해야 한다.
코드 변경 없이 Prompt만 갱신하는 경로가 없기 때문에, AI 업그레이드 비용이 기하급수적으로 커진다.

> "Prompt가 코드 안에 직접 삽입되어 있으면 마찰이 생기고, 개선을 억제하며, 운영 비용을 높인다."
[출처: Wipro Tech Blogs, "Managing Prompt Technical Debt in Enterprise AI"](https://wiprotechblogs.medium.com/managing-prompt-technical-debt-in-enterprise-ai-7985f4bc7ff7)

#### (e) 품질 회귀 추적 불가 (Quality Regression Blindness)
"AI 답변이 요즘 예전보다 이상해진 것 같다"는 느낌은 있지만, 언제 어떤 Prompt 변경이 원인인지 추적할 수 없다.
Prompt가 버전 관리·평가셋과 연결되지 않으면, 품질 회귀가 사용자 불만으로 터지기 전에 감지할 방법이 없다.

> "전통적 버전관리는 언제 무엇이 바뀌었는지 추적하지만, Prompt는 추가 복잡성을 갖는다: 변경이 미묘한 표현 조정인가, 아니면 중요한 행동 변경인가?"
[출처: Tessl, "Prompts are the new source code"](https://tessl.io/blog/prompts-are-the-new-source-code/)

### 2-3. Prompt/Harness는 어떤 종류의 "데이터 자산"인가

Prompt/Harness는 **"지시·판단 로직(instruction & judgment logic)"을 담은 데이터 자산**이다.
다른 데이터 자산(카탈로그, Glossary, 리니지, 거버넌스)이 "무엇이 있고 어디서 왔는가"를 기술한다면,
Prompt/Harness는 "그 데이터를 가지고 AI가 무엇을 어떻게 판단해야 하는가"를 정의한다.

이것이 Prompt/Harness를 **오케스트레이션(orchestration) 상위 레이어**로 보는 이유다.

> "프롬프트는 단순한 개발자 실험이 아니다. 챗봇·자동화·고객 서비스 에이전트·내부 생산성 도구에 내장된 운영 자산이다. 결과·위험·컴플라이언스에 영향을 미치는 자산이므로 구조가 필요하다."
[출처: Salesboom Blog, "Turning Prompt Engineering into an Enterprise Asset"](https://blog.salesboom.com/2025/08/enterprise-prompt-management.html)

---

## 3. WHY — Prompt/Harness가 다른 데이터 자산 "위에" 올라타는 방식

AI 시스템의 레이어 구조를 아래에서 위로 보면:

```
[ 데이터 카탈로그 / 메타데이터 / 온톨로지 / 리니지 ]  ← 기반 데이터 자산
             ↓ 무엇이 있는지, 어디서 왔는지 알려준다
[ 모델 (LLM) ]  ← 토큰을 생성하는 두뇌
             ↓ 무엇을 보고 어떻게 판단할지 결정한다
[ Prompt / Harness ]  ← 지시·판단 로직 레이어 (오케스트레이션)
             ↓ 전체 AI 행동을 정의하는 최상위 제어층
[ Tool / 평가셋(Eval Set) / 사용자 인터페이스 ]
```

구체적으로:
- **Prompt**는 데이터 카탈로그에서 어떤 데이터를 참조해야 하는지, 온톨로지의 어떤 용어 기준으로 판단해야 하는지를 명시한다.
- **Harness**는 어떤 Tool을 어떤 순서로 호출하고, 어떤 평가 기준(eval)을 통과해야 다음 단계로 진행하는지를 제어한다.
- 따라서 하위 데이터 자산이 아무리 잘 정비되어 있어도, Prompt/Harness가 없거나 제멋대로이면 AI 행동은 통제되지 않는다.

> "하네스는 제어(work를 어떻게 분해·스케줄링할지), 계약(어떤 산출물을 만들고, 어떤 게이트를 통과해야 하는지), 상태(단계 간에 무엇이 유지되어야 하는지)를 명세한다."
[출처: arxiv.org, "Natural-Language Agent Harnesses"](https://arxiv.org/html/2603.25723v1)

---

## 4. WHY — Key Objectives: 달성해야 할 핵심 목표 (동사형)

Prompt/Harness 자산화를 통해 달성해야 할 핵심 목표 5가지:

| # | 목표 (동사형) | 설명 |
|---|---|---|
| 1 | **인벤토리화하라** (Inventory) | 조직 내 Prompt·Harness를 개인 PC·채팅창·코드 인라인에서 끌어내어 중앙 저장소에 목록화한다 |
| 2 | **구조화·재사용가능하게 만들어라** (Structurize & Reuse) | 한 번 검증된 판단 로직이 다른 업무·팀에도 적용될 수 있도록 템플릿·모듈로 분리한다 |
| 3 | **버전 관리·변경 추적하라** (Version & Track Changes) | 어떤 Prompt 버전이 언제, 누가, 왜 바꿨는지 히스토리를 유지하고 롤백 경로를 확보한다 |
| 4 | **품질 평가 가능하게 연결하라** (Link to Evaluation) | Prompt 버전과 평가셋(eval set)을 연결하여 변경이 품질에 미치는 영향을 자동으로 측정한다 |
| 5 | **거버넌스·감사 가능하게 관리하라** (Govern & Audit) | 역할 기반 접근 제어(RBAC)·승인 워크플로·감사 로그를 통해 Prompt 변경을 책임 추적 가능하게 만든다 |

[출처 종합: Fulcrum Digital, "Prompt Governance"; AWS Prescriptive Guidance; AISquare Medium](https://medium.com/@aisquarecommunity/prompt-governance-scale-versioning-access-and-audits-c76b58a166f1)

---

## 5. WHEN — 언제 Prompt/Harness 자산화가 필요해지는가

다음 조건 중 하나라도 해당되면 Prompt/Harness 자산화 시작이 필요한 시점이다.

### 5-1. 반복 사용되는 Prompt가 3개 이상 존재할 때
한 번 쓰고 버리는 실험 Prompt와 달리, 매주·매일 반복 사용되는 Prompt는 더 이상 개인 자원이 아니다.
이 시점부터 버전이 없으면 "어떤 것이 현재 버전인지" 혼란이 시작된다.

> [두산 제조 현장 유추] 현장 반장이 매일 아침 설비 점검 기준을 AI에게 물을 때 사용하는 Prompt가 5개 있다면, 이 Prompt들은 이미 사실상의 "작업표준서"다. 작업표준서는 개인 수첩이 아니라 공식 문서 시스템에 있어야 한다.

### 5-2. 2명 이상이 동일 Prompt를 사용하거나 수정할 때
여러 사람이 같은 Prompt를 각자 복사해서 사용하기 시작하면, 조직 내에 서로 다른 버전이 병존하게 된다.
"어떤 버전이 옳은가"를 결정할 수 없는 상태가 된다.

> [두산 제조 현장 유추] A 팀장은 불량 판정 기준을 "엄격하게" 적용하는 Prompt를 쓰고, B 팀장은 6개월 전 수정 전 버전을 쓴다. 동일 공정에서 서로 다른 판단 기준이 적용되는 것이다.

### 5-3. 업무 판단에 실질적 영향을 줄 때
AI 출력이 단순 참고가 아니라 실제 의사결정(발주·품질 판정·고객 응대)에 사용되기 시작하면,
그 판단을 만들어낸 Prompt는 계약서·작업표준서와 동일한 수준의 관리가 필요하다.

### 5-4. 규제·감사 대상 업무에 AI가 투입될 때
금융·의료·제조 안전·환경 규제 등 외부 감사가 존재하는 업무에서 AI를 사용한다면,
"AI가 왜 이 판단을 내렸는가"에 대한 설명 가능성(explainability)이 필수다.
Prompt 버전 관리 없이는 이 설명이 불가능하다.

> "GDPR, HIPAA, SOC 2와 같은 규제 컴플라이언스 기준을 충족하려면 Prompt 거버넌스가 필요하다."
[출처: SecurityScientist.net, "Prompt Governance for Compliance Teams"](https://www.securityscientist.net/blog/12-questions-and-answers-about-prompt-governance-for-compliance-teams-complete-guide-for-2026/)

### 5-5. 여러 AI Agent를 동시에 운영하기 시작할 때
단일 AI 어시스턴트에서 복수의 Agent(검색 Agent, 요약 Agent, 판단 Agent 등)가 연동되는 구조로 전환되면,
각 Agent의 Harness가 정의·버전 관리되지 않으면 전체 시스템의 신뢰성을 보장할 수 없다.

> "멀티 에이전트 시스템에서 Prompt 인젝션은 에이전트 체인을 따라 전파될 수 있고, 공유 컨텍스트는 규제 데이터를 도메인 경계를 넘어 유출시킬 수 있다."
[출처: Augment Code, "Multi-Agent AI Security"](https://www.augmentcode.com/guides/multi-agent-ai-security-risks-compliance-fixes)

### 5-6. 모델 교체 또는 사내 LLM 전환을 계획할 때
클라우드 AI에서 사내 LLM(보안·비용 이유)으로 전환하거나, 모델 버전을 업그레이드할 때,
Prompt가 코드에 하드코딩되어 있으면 전환 비용이 기하급수적으로 증가한다.
Prompt가 독립 자산으로 관리될 때만 "모델은 바꾸고 Prompt는 재사용"이 가능하다.

---

## 6. 제조 현장 인과 스토리: 왜 두산 계열사에 즉각 적용되는가

```
현재 상태 (AS-IS)
  베테랑 정비원의 20년 판단 기준
  → 개인 메모 + 채팅창 프롬프트로 존재
  → 은퇴 시 소멸 / 다른 공장에 전파 불가

  품질 검사 기준
  → 팀마다 다르게 AI에게 요청
  → 일관성 없는 판정 → 감사 불합격

문제의 원리
  지시·판단 로직이 "사람 속인화" 상태
  = 소프트웨어 코드가 버전 관리 없이
    개발자 개인 PC에만 있는 것과 동일

Prompt/Harness 자산화 후 (TO-BE)
  판단 기준 → 중앙 Prompt 저장소에 버전 관리
  → 누가 써도 동일 기준 적용
  → 모델 교체 시에도 Prompt 재사용
  → 변경 이력으로 감사 대응 가능
  → 베테랑 은퇴 후에도 지식 유지
```

[개념 근거: Augmentir; Dovient; MIT 암묵지 연구]
[출처: Dovient, "Tacit Knowledge in Manufacturing: The Hidden Asset You Can't Afford to Lose"](https://dovient.com/resources/blog/tacit-knowledge-manufacturing-hidden-asset)

---

## 7. Shadow AI: 자산화 없는 프롬프트 확산의 위험

조직이 Prompt를 자산으로 관리하지 않으면, 직원들은 각자 승인 없이 AI 도구를 사용하기 시작한다.
이것이 **Shadow AI** 현상이다.

Shadow AI 상태의 특징:
- 공유 Prompt 엔지니어링 기준 없음
- 버전 없음, 피드백 루프 없음
- 민감 데이터가 외부 AI 서비스로 무단 전송
- 어떤 AI 도구가 조직 내에서 사용되는지 IT가 파악 불가

> "Shadow AI는 공인된 승인 없이, 보안 팀의 모니터링 없이 조직 내에서 AI를 사용하는 것이다."
[출처: Portkey.ai, "What is Shadow AI"](https://portkey.ai/blog/what-is-shadow-ai/)

---

## 8. 핵심 용어 병기 및 쉬운 풀이

| 한국어 | 영어 | 쉬운 풀이 |
|---|---|---|
| 프롬프트 자산화 | Prompt Assetization | 채팅창에 흩어진 AI 지시문을 공식 자산으로 등록·관리 |
| 하네스 | Harness | AI 모델을 감싸는 실행 제어 구조. "AI에게 무엇을 보여주고 어떻게 행동하게 할지" 정의 |
| 버전 관리 | Versioning | 언제 누가 무엇을 바꿨는지 추적하는 이력 관리. Git과 같은 개념 |
| 프롬프트 기술 부채 | Prompt Technical Debt | 관리되지 않는 Prompt가 쌓여 나중에 수정·이해 비용이 기하급수적으로 증가하는 현상 |
| 속인화 | Knowledge Personalization | 지식이 특정 개인에게만 귀속되어 그 사람이 없으면 사용 불가한 상태 |
| 오케스트레이션 레이어 | Orchestration Layer | 여러 AI 에이전트·도구·데이터를 조율하는 상위 제어 계층 |
| 암묵지 | Tacit Knowledge | 문서화되지 않은 채 숙련자 개인에게만 존재하는 경험적 지식 |
| 평가셋 | Eval Set | Prompt 품질을 자동으로 측정하기 위한 기준 문제-정답 쌍 |
| PromptOps | PromptOps | Prompt의 버전 관리·테스트·피드백 루프·거버넌스를 결합한 운영 체계 |
| Shadow AI | Shadow AI | IT 또는 보안팀의 승인·모니터링 없이 사용되는 AI 도구 |

---

## 출처

- [Tessl — Prompts are the new source code: Why we need version control](https://tessl.io/blog/prompts-are-the-new-source-code/)
- [Agenta — Prompt Versioning: The Complete Guide](https://agenta.ai/blog/prompt-versioning-guide)
- [Pinggy — AI Harness Engineering: The Layer That Makes Your LLM Applications Actually Work](https://pinggy.io/blog/best_ai_harnesses_to_supercharge_llm_models/)
- [Requesty — Agent Harness: Why Your LLM Gateway Is the Backbone of Production Agents](https://www.requesty.ai/blog/agent-harness-why-your-llm-gateway-is-the-backbone-of-production-agents)
- [arxiv.org — Natural-Language Agent Harnesses](https://arxiv.org/html/2603.25723v1)
- [Fulcrum Digital — Prompt Governance: The Emerging Enterprise Control Layer](https://fulcrumdigital.com/blogs/prompt-governance-the-emerging-enterprise-control-layer/)
- [Salesboom Blog — Turning Prompt Engineering into an Enterprise Asset](https://blog.salesboom.com/2025/08/enterprise-prompt-management.html)
- [AISquare / Medium — Prompt Governance Scale: Versioning, Access, and Audits](https://medium.com/@aisquarecommunity/prompt-governance-scale-versioning-access-and-audits-c76b58a166f1)
- [AWS Prescriptive Guidance — Prompt, agent, and model lifecycle management](https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html)
- [PromptLayer — Platform: Prompt Management](https://www.promptlayer.com/prompt-management/)
- [CertPro — Prompt Security Risks: The Hidden Compliance Gap in Enterprise AI Usage](https://certpro.com/prompt-security-risks-enterprise-ai/)
- [Wipro Tech Blogs / Medium — Managing Prompt Technical Debt in Enterprise AI](https://wiprotechblogs.medium.com/managing-prompt-technical-debt-in-enterprise-ai-7985f4bc7ff7)
- [seangoedecke.com — Prompts are technical debt too](https://www.seangoedecke.com/prompts-are-technical-debt-too/)
- [arxiv.org — PromptDebt: A Comprehensive Study of Technical Debt Across LLM Projects](https://arxiv.org/html/2509.20497v1)
- [SecurityScientist.net — Prompt Governance for Compliance Teams: Complete Guide for 2026](https://www.securityscientist.net/blog/12-questions-and-answers-about-prompt-governance-for-compliance-teams-complete-guide-for-2026/)
- [Portkey.ai — What is Shadow AI, and why is it a real risk for LLM apps](https://portkey.ai/blog/what-is-shadow-ai/)
- [Augment Code — Multi-Agent AI Security: Enterprise Risks, Compliance, and Mitigation](https://www.augmentcode.com/guides/multi-agent-ai-security-risks-compliance-fixes)
- [Augmentir — Tacit Knowledge in Manufacturing: Unlocking Hidden Expertise with AI](https://www.augmentir.com/glossary/tacit-knowledge)
- [Dovient — Tacit Knowledge in Manufacturing: The Hidden Asset You Can't Afford to Lose](https://dovient.com/resources/blog/tacit-knowledge-manufacturing-hidden-asset)
- [Squirro — Corporate Amnesia: Capturing Tacit Knowledge with Enterprise AI](https://squirro.com/squirro-blog/ai-tacit-knowledge-capture)
- [ItSoli — Why Prompt Engineering Needs Its Own Governance Framework](https://itsoli.ai/why-prompt-engineering-needs-its-own-governance-framework/)
- [LaunchDarkly — Prompt Versioning & Management Guide for Building AI Features](https://launchdarkly.com/blog/prompt-versioning-and-management/)
- [Veso AI — The Agentic Harness: Why the Orchestration Layer Is the Product](https://veso.ai/blog/the-agentic-harness-architecture/)
- [Decodingai.com — Agentic Harness Engineering: LLMs as the New OS](https://www.decodingai.com/p/agentic-harness-engineering)
