# D-3 Prompt/Harness 자산화 — AI-Ready Data 가이드 스토리라인

> 슬라이드 작성용 스토리라인.
> **메인 장표 = 이해에 필요한 내용으로 이어지는 맞는 스토리라인.** `[슬라이드 N]`
> **백업 장표 = 메인에서 필요하면 들어가는 디테일.** `[백업 N-x]` (해당 메인 바로 아래)
> 메인 슬라이드는 ① 한 문장 요약 ② 용어 즉시 풀이 ③ 제조 현업 예시를 반드시 포함한다.
> 어려운 표·스펙·스키마·툴 비교는 메인이 아니라 백업으로 내린다.

---

## 1. Executive Summary

### [슬라이드 1] 한 장 요약

**👉 한 문장 요약:** 현장 숙련자의 판단 기준과 업무 절차를 AI가 실행할 수 있는 Prompt·Harness 형태로 구조화하고, 인벤토리·메타데이터·버전관리·리니지·품질검증·거버넌스 체계로 정비해 조직이 재사용·재현·감사할 수 있는 데이터 자산으로 만드는 것이 Prompt/Harness 자산화다.

**헤드라인:** "흩어진 판단 로직을 데이터로 정비하면 AI를 조직 자산으로 쓸 수 있다"

- **정의:** 사람의 업무 절차와 판단 기준을 AI가 실행 가능한 Prompt·Harness 형태로 구조화하고, 인벤토리·버전관리·메타데이터·리니지·품질·거버넌스 체계로 관리하는 데이터 자산화 체계 (지시·판단 로직만을 대상으로 한다)
- **왜 지금:** 현장 AI 파일럿이 늘면서 판단 로직이 개인 채팅창·코드 내부에 흩어지기 시작함. 정비 없이 쌓이면 담당자 퇴사 시 소멸, 모델 교체 시 전면 파손, 감사 대응 불가로 이어짐
- **핵심 구축 접근(키워드):** ① 인벤토리화 → ② 표준 구조화·메타데이터 부착 → ③ 버전관리·의존성 연결 → ④ 품질 평가셋·거버넌스 연결
- **대표 기대효과/KPI:** Prompt 자산화율 95% 달성 / 회귀 테스트 통과율 90% 이상 / Harness 재사용률 60% (중복 개발 제거)
- **로드맵 한 줄:** 단기(0~6개월) 인벤토리·Registry 구축 → 중기(6~18개월) 의존성 맵·거버넌스·골든셋 연동 → 장기(18개월~) 자동 드리프트 탐지·전사 카탈로그 통합

---

## 2. Why? — 왜 필요한가

### [슬라이드 2] 지금 판단 로직은 어디에 있는가 — 속인화·재현 불가의 구조

**👉 한 문장 요약:** 현장 숙련자의 판단 기준이 개인 채팅창이나 코드 속에 묻혀 있으면, 담당자가 없을 때 사라지고 AI가 "언제 어떤 기준으로 판단했는지"를 추적할 수 없다.

**헤드라인:** "Prompt가 개인 채팅창에 있는 것은 작업표준서가 정비원 개인 수첩에만 있는 것과 같다"

지금 대부분의 조직에서 Prompt(프롬프트 = AI에게 내리는 지시·판단 기준을 담은 자연어 명세)는 세 곳에 흩어진 채로 존재한다.

- **개인 PC·메모장:** 퇴직·이동 시 소멸, 재현 불가
- **채팅창 대화 이력:** 버전 없음, 누가 무엇을 썼는지 추적 불가
- **소스코드 인라인 하드코딩:** 배포 주기에 묶여 수정 불가, 모델 교체 시 전면 파손

이 상태를 "Prompt 기술 부채(Prompt Technical Debt = 관리되지 않는 Prompt가 쌓여 나중에 수정·이해 비용이 기하급수적으로 증가하는 현상)"라 한다. 기술 부채는 조용히 부패한다 — 1월에 잘 동작하던 Prompt가 2월에 모델이 업데이트되면 성능이 저하되는데, 이 사실을 사용자 불만이 터지기 전까지 아무도 모른다.

자산화가 없으면 원리적으로 다섯 가지가 불가능해진다:

1. **재현 불가:** 어떤 버전의 Prompt가 어떤 판단을 만들었는지 알 수 없다
2. **속인화(知識 개인 귀속):** 설계 의도를 아는 사람이 떠나면 판단 기준이 사라진다
3. **감사 불가:** "이 AI가 왜 이 결론을 냈는가" 규제·감사 기관에 설명할 수 없다
4. **모델 교체 시 전면 파손:** 코드에 하드코딩된 Prompt는 모델 변경 시 일일이 수작업 수정이 필요하다
5. **품질 회귀 추적 불가:** "AI 답변이 요즘 이상하다"는 느낌은 있지만 언제 어떤 변경이 원인인지 찾을 수 없다

🏭 **제조 현업 예시:** 두산 계열사의 20년 경력 정비원은 설비 이상음만 듣고 "3번 베어링이 2주 내로 교체 시점"임을 안다. 이 판단 기준이 작업표준서가 아닌 개인 노트북에만 있다면, 정비원 은퇴 시 이 지식은 사라진다. Prompt가 개인 채팅창에 있는 것은 정확히 같은 구조다. 실제로 품질 검사 팀에서 불량 판정 기준을 만든 엔지니어가 퇴사하자 Prompt 소재 파악 불가, 신입이 다시 만들었지만 기준이 달라져 불량율이 갑자기 튀었고 품질 감사에서 "과거 판정 기준이 무엇이었냐"는 질문에 답하지 못하는 상황이 발생한다.

---

### [슬라이드 3] Prompt/Harness 자산화가 달성해야 할 목표

**👉 한 문장 요약:** 판단 로직을 데이터로 정비하면 인벤토리화·재사용·버전 추적·품질 평가·거버넌스 다섯 목표가 비로소 가능해진다.

**헤드라인:** "흩어진 지시·판단 로직을 재사용·재현·감사 가능한 AI-Ready 자산으로 만들자"

| 목표 | 설명 |
|---|---|
| ① 인벤토리화 | 개인 PC·채팅창·코드에 묻힌 Prompt·Harness를 발굴해 중앙 저장소에 목록화 |
| ② 구조화·재사용 | 한 번 검증된 판단 로직을 템플릿·모듈로 분리해 다른 업무·팀에도 적용 |
| ③ 버전 관리·변경 추적 | 언제, 누가, 왜 바꿨는지 이력 유지, 롤백(이전 버전으로 복원) 경로 확보 |
| ④ 품질 평가 연결 | Prompt 버전과 평가셋(eval set = 정답이 있는 테스트 케이스 묶음)을 연결해 변경이 품질에 미치는 영향 자동 측정 |
| ⑤ 거버넌스·감사 | 역할 기반 접근 제어·승인 워크플로·감사 로그로 Prompt 변경을 책임 추적 가능하게 관리 |

🏭 **제조 현업 예시:** 설비 예지보전 판정 기준 Prompt를 Registry(중앙 저장소)에 등록하면 — "이 기준은 생산기술팀 김과장이 2025-04-01 작성, v2.1.3, 분기별 검토"처럼 작업표준서와 동일한 관리가 된다. 팀 어디서든 같은 기준을 쓰고, 기준이 바뀌면 이력이 남는다.

---

## 3. When? — 언제 Prompt/Harness 자산화가 필요한가

### [슬라이드 4] 자산화를 시작해야 하는 6가지 신호

**👉 한 문장 요약:** 반복 사용 Prompt가 3개 이상 생기거나, 여러 사람이 같은 Prompt를 쓰거나, AI 판단이 업무에 실질적 영향을 미치기 시작한 순간이 자산화를 시작해야 할 시점이다.

**헤드라인:** "한 번 쓰고 버리는 Prompt는 없다 — AI가 업무에 들어오는 순간 자산화가 시작돼야 한다"

다음 중 하나라도 해당되면 지금이 시작 시점이다:

1. **반복 사용 Prompt가 3개 이상 존재할 때** — 매일·매주 쓰이는 Prompt는 사실상 작업표준서다. 공식 문서 시스템에서 관리해야 한다
2. **2명 이상이 동일 Prompt를 사용하거나 수정할 때** — 각자 복사해 쓰기 시작하면 조직 내에 서로 다른 버전이 병존하고, "어떤 버전이 옳은가"를 결정할 수 없다
3. **업무 판단에 실질적 영향을 줄 때** — AI 출력이 발주·품질 판정·고객 응대 등 실제 의사결정에 사용되면, 그 Prompt는 계약서·작업표준서와 동일한 수준의 관리가 필요하다
4. **규제·감사 대상 업무에 AI가 투입될 때** — "AI가 왜 이 판단을 내렸는가"의 설명 가능성(explainability)은 Prompt 버전 관리 없이는 불가능하다
5. **여러 AI Agent를 동시에 운영하기 시작할 때** — 각 에이전트의 Harness(실행 제어 구조)가 정의·버전 관리되지 않으면 전체 시스템의 신뢰성을 보장할 수 없다
6. **모델 교체 또는 사내 LLM 전환을 계획할 때** — Prompt가 독립 자산으로 관리될 때만 "모델은 바꾸고 Prompt는 재사용"이 가능하다

🏭 **제조 현업 예시:** 현장 반장이 매일 아침 설비 점검 기준을 AI에게 묻는 Prompt가 5개라면, 이 순간이 자산화 시작 시점이다. A 팀장은 엄격한 기준을 적용하는 Prompt를, B 팀장은 6개월 전 수정 전 버전을 쓴다면 동일 공정에서 서로 다른 판단 기준이 적용되고 있는 것이다.

---

## 4. What? — Prompt/Harness 자산화란 무엇인가

### [슬라이드 5] What 세부목차

**👉 한 문장 요약:** 이 섹션은 Prompt와 Harness가 각각 무엇인지, 어떤 구조로 이루어지는지, 자산으로서 어떻게 분류·관리되는지를 순서대로 설명한다.

이 섹션이 답할 질문:
- ① Prompt와 Harness는 각각 무엇이고, 어떤 관계인가?
- ② 자산으로서 Prompt는 어떤 구성요소를 가지는가?
- ③ Prompt 자산은 어떤 라이프사이클(생애주기)을 거치는가?
- ④ Prompt 드리프트(Drift)란 무엇이고 왜 중요한가?
- ⑤ 자산화 성숙도는 어떻게 단계적으로 올라가는가?

---

### [슬라이드 6] Prompt와 Harness — 두 종류의 지시·판단 로직 자산

**👉 한 문장 요약:** Prompt는 AI에게 "무엇을 어떻게 판단하라"는 지시문이고, Harness는 AI 모델을 감싸 "무엇을 보고, 어떤 도구를 쓰고, 언제 멈추는지"를 제어하는 실행 구조다.

**헤드라인:** "Prompt = 판단 지시서, Harness = 실행 제어 구조 — 둘 다 정비해야 할 데이터 자산이다"

**Prompt (프롬프트)란:**
- AI 모델에게 내리는 지시·판단 기준을 담은 자연어 명세(specification)
- 코드가 "어떻게 처리하라"를 명시한다면, Prompt는 "어떤 관점·기준으로 판단하라"를 명시한다
- 단 한 단어의 차이가 AI 출력 품질과 범위를 결정한다

**Harness (하네스)란:**
- LLM(대형 언어 모델, Large Language Model)을 제외한 에이전트 실행 인프라 전체
- 모델이 "토큰을 생성하는 두뇌"라면, Harness는 "두뇌가 무엇을 보고, 어떤 도구를 쓰고, 언제 멈추는지를 결정하는 신체"
- 모델은 점점 서로 비슷해지고 있다. 신뢰할 수 있는 프로덕션 에이전트와 예측 불가능한 데모를 가르는 것은 거의 항상 Harness 설계다

**두 개념의 관계:**
```
[Agent = Model + Harness]
  Model  → 토큰을 생성하는 두뇌
  Harness → Prompt 집합 + 입력 데이터 정의 + Tool 호출 순서 + 평가 기준 + 출력 포맷
```

쉬운 비유: 사람이 공장 현장에서 작업할 때 필요한 것 = 작업복 + 공구함 + 절차서 + 체크리스트 + 통신장비. 이 모든 것이 "Harness"다. LLM은 그 사람 자신의 두뇌.

🏭 **제조 현업 예시:** 두산 설비 예지보전 AI를 예로 들면 — Prompt는 "진동 RMS값이 기준치 120%를 초과하면 즉시 교체 판정"이라는 판단 지시문이고, Harness는 MES 센서 데이터를 가져오고(Tool 호출), 정비 이력을 조회하고, 판정 결과를 JSON으로 출력하는 전체 실행 구조다.

> ▸ **백업 장표:** [백업 6-1] Scaffold와 Harness의 공식 정의 비교

#### [백업 6-1] Scaffold와 Harness — 공식 용어 구분 (디테일)

HuggingFace 공식 Agent 용어집(2026)의 정의:

| 개념 | 정의 | 비유 |
|---|---|---|
| **Scaffold (스캐폴드)** | 모델의 행동을 결정하는 레이어: 시스템 프롬프트, 도구 설명, 응답 파싱 방식, 컨텍스트 관리. 모델이 세상을 어떻게 보는지를 결정. | 작업 지시서, 절차서 |
| **Harness (하네스)** | 실행 레이어: 모델을 호출하고, 도구 호출을 처리하고, 언제 멈출지 결정. 에이전트를 실제로 동작시키는 것. | 작업 도구함, 통신 시스템 |
| **Agent = Model + Harness** | 모델 자체가 아닌 모든 것이 Harness | 사람 + 작업 환경 전체 |

실무에서는 혼용된다. 용어집은 Claude Code·Codex 같은 코딩 에이전트를 전체를 "harness"로 통칭하는 대표 예시로 든다. 둘을 구분할 필요가 있을 때만 scaffold/harness를 분리해서 사용한다.

Harness가 관장하는 6개 도메인: ① Context assembly(무엇을 모델에게 보여줄지 선택·압축) ② Tool connectors(파일·API·검색·코드 실행 환경 연결) ③ Memory & state(컨텍스트 창을 넘는 지식 유지) ④ Control loop(목표 달성까지 반복 실행·재시도 제어) ⑤ Guardrails & policy(입출력 필터, 권한 경계, 안전 제약) ⑥ Telemetry & evaluation(추적·지연·품질 측정, 회귀 감지)

---

### [슬라이드 7] Prompt 자산의 구성요소 — 무엇이 들어가야 "자산"인가

**👉 한 문장 요약:** Prompt를 자산으로 만들려면 지시문 텍스트만으로는 부족하고, 메타데이터(소유자·버전·용도·연계 모델)·의존 데이터·평가 기준·변경 이력·거버넌스 정보가 함께 묶여야 한다.

**헤드라인:** "자산 = 지시문 + 메타데이터 + 평가 기준 + 버전 이력 — 이 패키지가 Registry에 등록돼야 관리된다"

**Prompt 자산 패키지의 핵심 구성 요소:**

- **지시문 (Template):** 역할·판단 기준·출력 형식 등을 담은 실제 지시 텍스트. `{{변수}}` 형태로 런타임에 실제 값이 채워진다
- **메타데이터 (Metadata):** 소유자, 버전, 작성일, 담당 부서, 대상 모델, 연계 업무 — "이 Prompt는 누가, 언제, 왜 만들었는가"
- **의존 데이터 정의:** 이 Prompt가 참조하는 설비 Glossary·데이터 소스·Tool 목록 — "이 Prompt는 어떤 데이터와 연결돼 있는가"
- **평가 기준 및 평가셋:** 정답이 있는 테스트 케이스 묶음 — "이 Prompt가 얼마나 잘 작동하는가"
- **변경 이력:** 버전별 변경 이유와 승인자 — "이 Prompt가 언제 어떻게 바뀌었는가"

**완성된 Prompt 자산 예시 (두산 설비 예지보전 판정):**

| 필드 | 값 |
|---|---|
| 이름 | `bearing_failure_detection` |
| 버전 | `v2.1.3` |
| 설명 | 베어링 진동 데이터로 교체 시점 3단계 판정 |
| 소유자 | 설비기술팀장 |
| 작성자 | 김설비_정비팀 (kim.jiho@doosan.com) |
| 작성일 | 2025-04-01 |
| 최종수정일 | 2026-03-15 |
| 대상 모델 | claude-sonnet-4 |
| 연계 업무 | 설비 정기점검 → 이상 판정 |
| 의존 데이터 | 설비 기술 Glossary v3, 정비 이력 DB, 센서 API v2 |
| 평가셋 | defect-eval-set-v2 (500건) |
| 평가 기준 | 정확도 ≥ 90%, 허용 false_positive ≤ 5% |
| 환경 | production |
| 마지막 변경 이유 | 2026년 설비 기준치 개정 반영 |

🏭 **제조 현업 예시:** 위 표가 채워진 Prompt는 "작성자가 누군지 모르는 최종_최최종.txt"와 달리, 새 엔지니어도 즉시 활용하고, 감사 시에도 "이 판정 기준은 언제 누가 왜 만들었는지" 답변할 수 있다.

> ▸ **백업 장표:** [백업 7-1] Prompt 자산 전체 메타데이터 필드 스펙표 / [백업 7-2] Harness 자산 패키지 구조 상세

#### [백업 7-1] Prompt 자산 전체 메타데이터 필드 스펙표 (디테일)

| 필드 카테고리 | 필드명 | 설명 | 예시 |
|---|---|---|---|
| 식별 | `name` | 프롬프트 고유 이름 | `bearing_failure_detection_v2` |
| 식별 | `version` | 시맨틱 버전 (MAJOR.MINOR.PATCH) | `2.1.3` |
| 식별 | `id` / `asset_id` | 시스템 발급 고유 ID | `pmt_0xA3B2...` |
| 설명 | `description` | 이 Prompt가 하는 일의 설명 | "베어링 진동 데이터로 교체 시점 판정" |
| 분류 | `tags` | 조직, 도메인, 용도 태그 | `["설비관리", "예지보전", "production"]` |
| 입력 | `input_variables` | 런타임에 채워질 변수 목록 | `[{name: "vibration_data", required: true}]` |
| 실행 | `execution_settings` | 모델 ID, temperature, max_tokens | `{model_id: "claude-sonnet-4", temperature: 0}` |
| 출력 | `output_variable` | 예상 출력 형식·스키마 | `{description: "판정결과 JSON"}` |
| 메타데이터 | `author` | 작성자 | `"김설비_정비팀"` |
| 메타데이터 | `department` | 담당 부서 | `"생산기술팀"` |
| 메타데이터 | `created_at` | 생성일 | `"2025-04-01"` |
| 메타데이터 | `last_updated_at` | 최종 수정일 | `"2025-11-20"` |
| 거버넌스 | `business_owner` | 업무 책임자 | `"설비기술팀장"` |
| 거버넌스 | `review_cycle` | 검토 주기 | `"분기별"` |
| 거버넌스 | `approver` | 프로덕션 배포 승인한 사람 | `"kim.tech.lead@doosan.com"` |
| 거버넌스 | `label` / `alias` | 어느 환경에서 사용 중 | `production`, `staging` |
| 품질 | `test_cases` | 테스트 케이스 목록 | `[{input: {...}, expected_output: {...}}]` |
| 품질 | `evaluation_criteria` | 평가 기준 | `["정확도 ≥ 90%", "허용 오류 유형 목록"]` |
| 품질 | `eval_score` | 최근 평가 점수 | `{"accuracy": 0.94, "user_edit_rate": 0.03}` |
| 의존성 | `data_dependencies` | 참조하는 데이터 자산 | `["설비 용어 Glossary v3", "정비 이력 DB"]` |
| 의존성 | `eval_dataset` | 성능 테스트에 사용하는 데이터 | `maintenance-eval-set-v2` |
| 변경이력 | `change_log` | 버전별 변경 내역 | `[{version: "2.0.0", reason: "출력 형식 변경"}]` |

#### [백업 7-2] Harness 자산 패키지 구조 상세 (디테일)

Harness 자산은 5개 요소를 묶은 재사용 가능한 업무 실행 패키지다:

```
Harness 자산 패키지: predictive_maintenance_agent / v2
├── [1] Prompt 집합
│       시스템 프롬프트: "두산의 설비 예지보전 전문 진단 AI. 센서 데이터를 분석해 교체 시점을 판정한다."
│       입력 템플릿: {{sensor_id}}, {{timestamp}}, {{vibration_rms}}, {{temperature}}
├── [2] 입력 데이터 정의
│       {sensor_id, timestamp, vibration_rms, temperature}
├── [3] Tool 호출 순서
│       1.get_sensor_data → 2.check_threshold → 3.query_history → 4.generate_report
├── [4] 평가 기준
│       {정확도 ≥ 90%, false_positive_rate ≤ 5%, 응답시간 ≤ 3s}
└── [5] 출력 포맷
        JSON {status: "교체필요|주의|정상", confidence: float, reason: string}
```

---

### [슬라이드 8] Prompt 자산의 내부 구조 — 업무 절차를 어떻게 담는가

**👉 한 문장 요약:** Prompt 지시문 안에는 역할·판단 조건·참조 기준·출력 형식·금지 행동 다섯 요소가 있어야 하며, 이 구조를 표준화해야 재사용 가능한 자산이 된다.

**헤드라인:** "업무 절차를 AI 지시문으로 바꿀 때 빠뜨리면 안 되는 5요소가 있다"

현업 전문가의 암묵지(tacit knowledge = 문서화되지 않은 채 숙련자 개인에게만 존재하는 경험적 지식)를 체계 없이 Prompt로 옮기면 재현이 불가하고 품질이 불안정하다. 표준 5요소를 빠짐없이 채워야 자산으로 관리될 수 있다.

**Prompt 구조화 5요소:**

| 요소 | 역할 | 왜 필요한가 |
|---|---|---|
| ① 역할 및 맥락 | AI에게 전문가 관점 부여 | 도메인 용어와 전문 지식 수준 조정 |
| ② 입력 조건 및 판단 단계 | 업무 절차의 순서와 분기 조건 명시 | 복잡한 판단 절차를 단계별로 실행 |
| ③ 참조 기준 | AI가 참조해야 할 데이터·문서·Glossary 지정 | 표준 없이 판단하는 것 방지 |
| ④ 출력 형식 | 응답의 구조·형식 강제 | 후속 시스템 연동 가능하게 함 |
| ⑤ 금지 행동 | AI가 하면 안 되는 것 명시 | 환각(Hallucination)·정보 누출 방지 |

🏭 **제조 현업 예시 — 정비 절차서 → Prompt 자산 변환:**

| AS-IS (사람 절차서) | TO-BE (Prompt 구조화) |
|---|---|
| "이상 소음 발생 시 담당자 판단" | 역할: "30년 경력 설비 진단 전문가" |
| "진동 수치 확인 후 조치" | 입력 조건: `{{vibration_data}}`, `{{temperature_data}}` |
| "정비 매뉴얼 3장 참조" | 참조 기준: `{{maintenance_manual_v3}}` |
| "수리 또는 교체 결정" | 출력 형식: `{"action": "repair"\|"replace"\|"monitor"}` |
| (암묵적 금지사항 없음) | 금지 행동: "매뉴얼에 없는 조치 권고 금지" |

핵심 설계 원칙: 기준값(`{{pressure_spec}}` 등)을 하드코딩하지 않고 변수 참조로 처리하면, 기준 변경 시 Prompt 재작성 없이 데이터만 업데이트하면 된다.

> ▸ **백업 장표:** [백업 8-1] Prompt 5요소 표준 템플릿 (YAML 형식) / [백업 8-2] Microsoft Semantic Kernel YAML 스키마 공식 예시

#### [백업 8-1] Prompt 구조화 표준 템플릿 — YAML 형식 (디테일)

```yaml
prompt_template:
  # ① 역할 및 맥락 (Role & Context)
  role: |
    당신은 [두산테스나]의 [반도체 테스트 공정] 전문가입니다.
    [웨이퍼 전기적 특성 검사] 업무를 담당합니다.

  # ② 입력 조건 및 판단 단계 (Input & Decision Steps)
  instruction: |
    다음 [검사 데이터]를 분석하여 아래 순서로 판단하세요:
    1단계: [불량 여부] 판정 (기준: {{defect_threshold}} 초과 시 불량)
    2단계: [불량 유형] 분류 (종류: {{defect_types}})
    3단계: [조치 권고] 제시 (기준: {{action_policy}})

  # ③ 참조 기준 (Reference Standards)
  context: |
    - 판정 기준서: {{spec_document}} (버전: {{spec_version}})
    - 용어 정의: {{glossary_terms}}
    - 과거 유사 사례: {{similar_cases}}

  # ④ 출력 형식 (Output Format)
  output_format: |
    반드시 다음 JSON 형식으로만 응답하세요:
    {
      "defect_judgment": "pass" | "fail",
      "defect_type": "...",
      "confidence": 0.0-1.0,
      "recommended_action": "...",
      "reason": "..."
    }

  # ⑤ 금지 행동 (Prohibited Actions)
  constraints: |
    - 판정 기준서에 없는 유형으로 분류하지 마세요
    - 확신 없을 때 추측하지 말고 confidence < 0.6으로 표시하세요
    - 고객사 정보를 출력에 포함하지 마세요
```

#### [백업 8-2] Microsoft Semantic Kernel YAML 스키마 공식 예시 (디테일)

Microsoft Semantic Kernel은 프롬프트를 YAML 파일로 정의하는 공식 스키마를 제공한다:

```yaml
name: GenerateMaintenanceReport
description: 설비 점검 결과를 바탕으로 정비 보고서를 생성
template_format: handlebars
template: |
  당신은 두산의 설비 정비 전문가입니다.
  다음 점검 데이터를 기반으로 정비 보고서를 작성하세요.
  설비명: {{equipment_name}}
  점검일: {{inspection_date}}
  이상 항목: {{anomaly_list}}
input_variables:
  - name: equipment_name
    description: 점검 대상 설비명
    is_required: true
  - name: inspection_date
    description: 점검 수행 날짜
    is_required: true
  - name: anomaly_list
    description: 발견된 이상 항목 목록
    is_required: false
    default: "이상 없음"
output_variable:
  description: 구조화된 정비 보고서 텍스트
  json_schema:
    type: object
    properties:
      summary: {type: string}
      recommendations: {type: array}
execution_settings:
  default:
    model_id: claude-sonnet-4
    temperature: 0.2
    max_tokens: 1000
```

---

### [슬라이드 9] Prompt 자산의 라이프사이클 — 정비에서 폐기까지

**👉 한 문장 요약:** Prompt 자산은 발굴→구조화→등록→테스트→배포→모니터링→개선→폐기 8단계 생애주기를 거치며, 이 흐름 전체가 "Prompt를 데이터로 정비"하는 과정이다.

**헤드라인:** "Prompt 자산은 소프트웨어처럼 배포되고 작업표준서처럼 관리된다"

| 단계 | 이름 | 핵심 활동 |
|---|---|---|
| 1 | **발굴 (Discovery)** | 현장 업무 인터뷰·관찰로 AI 적용 가능 업무 절차·판단 기준 식별 |
| 2 | **변환 (Conversion)** | 업무 절차 → 5요소 템플릿으로 Prompt 구조화 |
| 3 | **등록 (Registration)** | Registry에 v1.0.0으로 등록, 메타데이터 태깅, 소유자 지정 |
| 4 | **테스트 (Testing)** | 다양한 입력값으로 검증, 경계 조건 테스트, 성능 기준선 수립 |
| 5 | **배포 (Deployment)** | staging → production 환경으로 승격. 코드 변경 없이 레이블만 이동 |
| 6 | **모니터링 (Monitoring)** | Prompt Drift(아래 참조) 감지, 지연시간·정확도·비용 추적 |
| 7 | **개선 (Iteration)** | 피드백 루프 → 새 버전 작성 → 회귀 테스트 통과 → 배포 |
| 8 | **폐기/아카이브 (Deprecation)** | 하위 호환 버전 유지 + 더 이상 사용되지 않는 버전 아카이브 |

**Prompt Drift (프롬프트 드리프트)란:**
Prompt 자체를 수정하지 않아도, 기반 LLM이 업데이트되거나 입력 데이터 분포가 변하면 출력 품질이 서서히 저하되는 현상. 버전 관리 없이는 드리프트 발생 시점 추적 불가, 기준선(baseline) 없이는 "언제부터 나빠졌는지" 알 수 없다.

실증 사례: GPT-4를 동일 조건으로 테스트했을 때 소수 판별(prime number identification) 정확도가 3개월 사이 97.6% → 2.4%로 급락한 사례가 보고됐다. 모델 명칭은 동일한 "GPT-4"였으나 내부 변경으로 인한 조용한 드리프트였다. (Chen, Zaharia, Zou, Stanford/UC Berkeley, 2023)

🏭 **제조 현업 예시:** LLM API 공급사가 모델을 업데이트했을 때 설비 이상 탐지 Prompt의 판정 기준이 미묘하게 변한다. 평가셋이 연결돼 있지 않으면 현장 사고가 발생한 뒤에야 원인을 파악하게 된다.

---

### [슬라이드 10] Prompt/Harness 자산화 성숙도 — 지금 어디에 있는가

**👉 한 문장 요약:** 자산화 수준은 "흩어진 상태(Level 1)"에서 "전사 자동 최적화(Level 5)"까지 단계적으로 올라가며, 현재 Level을 파악하는 것이 시작점이다.

**헤드라인:** "우리 조직의 현재 수준을 먼저 진단하고, 한 단계씩 올라가는 전략을 세운다"

| 단계 | 명칭 | 특징 | 두산 현업 적용 모습 |
|---|---|---|---|
| **Level 1** | 임시방편 (Ad-hoc) | Prompt가 개인 노트·슬랙에 산재, 관리 없음 | AI 파일럿 시도는 있으나 표준 없음 |
| **Level 2** | 관리됨 (Managed) | 공유 폴더나 Git에 저장, 기초 버전 관리 시작 | 팀 내 Google Drive에 prompt.txt 저장 |
| **Level 3** | 표준화됨 (Standardized) | 중앙 Registry, 메타데이터 스키마, 소유권 정의 | LangSmith/MLflow에 등록, 담당자·태그 관리 |
| **Level 4** | 자동화됨 (Automated) | CI/CD 통합, 자동 회귀 테스트, Drift 모니터링 | Prompt 변경 시 자동 테스트 → 스테이징 배포 |
| **Level 5** | 최적화됨 (Optimizing) | A/B 테스트 상시화, 자동 최적화, 전사 재사용 | 계열사 공통 Prompt 라이브러리, 자동 최적화 |

현재 대부분의 기업은 Level 1~2에 위치한다. Level 3(표준화)가 조직적 관리의 출발점이며, 이 가이드의 How 섹션이 Level 1→3 전환에 집중한다.

🏭 **제조 현업 예시:** Level 1은 "각자 채팅창에서 AI에게 묻는 단계", Level 3은 "설비 판정 기준이 Registry에 등록되고 담당자가 있는 단계", Level 5는 "밥캣·테스나·두산에너빌리티가 공통 Prompt 라이브러리를 쓰고 자동으로 품질이 관리되는 단계"다.

> ▸ **백업 장표:** [백업 10-1] 핵심 용어집

#### [백업 10-1] 핵심 용어집 (디테일)

| 용어 | 쉬운 한 줄 풀이 | 현업 맥락 |
|---|---|---|
| **Prompt** | AI에게 보내는 지시·판단 기준 텍스트 | 설비 이상 여부를 판정하는 기준서 |
| **System Prompt** | AI의 역할·규칙·출력 형식을 정하는 초기 설정 지시문 | "당신은 두산밥캣 유압계통 진단 전문가입니다" |
| **Prompt Template** | 런타임에 변수로 채워지는 재사용 가능한 Prompt 구조 | "{{설비명}}의 {{측정값}}으로 교체 시점을 판정하라" |
| **Harness** | LLM 제외 에이전트 실행 인프라 전체 | 센서 조회·이력 검색·판정·보고서 생성을 묶은 실행 패키지 |
| **PromptOps** | DevOps를 모델로 한 Prompt 생애주기 관리 방법론 | 설계→테스트→배포→모니터링→개선 반복 체계 |
| **Prompt Registry** | 조직의 Prompt를 중앙에서 등록·검색·버전관리하는 저장소 | 회사 공식 Prompt 보관함 |
| **Prompt Drift** | Prompt 미수정 상태에서도 LLM 업데이트 등으로 품질이 서서히 저하되는 현상 | 작년엔 맞았던 판정이 올해는 틀리는 현상 |
| **Semantic Versioning** | MAJOR.MINOR.PATCH 형식으로 변경의 심각도를 표시하는 버전 체계 | v2.1.3 = 두 번째 대형 개편·첫 번째 기능 추가·세 번째 오타 수정 |
| **Eval Set (평가셋)** | Prompt 품질을 자동으로 측정하기 위한 기준 입력-정답 쌍 묶음 | 설비 판정 500건의 정답 포함 테스트 케이스 |
| **Prompt Alias** | production, staging 같이 특정 버전을 가리키는 가변 레이블 | 코드 변경 없이 "현재 프로덕션 버전"을 교체하는 방법 |

---

## 5. How? — Prompt/Harness 자산을 어떻게 정비하는가

### [슬라이드 11] How 세부목차

**👉 한 문장 요약:** 이 섹션은 흩어진 Prompt를 발굴하는 인벤토리부터 버전관리·의존성 연결·품질 검증·거버넌스 운영까지 데이터 정비의 전 단계를 다룬다.

이 섹션이 답할 질문:
- ① Prompt 자산을 어떤 순서로 정비하는가? (7단계 정비 방법론)
- ② 버전관리를 어떻게 하는가? (Registry와 Semantic Versioning)
- ③ Prompt 의존성은 어떻게 추적하는가? (Dependency Map)
- ④ 품질 저하를 어떻게 감지하고 개선하는가? (평가셋·드리프트 탐지)
- ⑤ 어떤 도구를 쓰는가? (솔루션 선택 길잡이)

---

### [슬라이드 12] 7단계 정비 방법론 — 인벤토리에서 거버넌스까지

**👉 한 문장 요약:** Prompt 자산 정비는 "있는지 찾기(인벤토리)→구조화→메타데이터 부착→버전관리→의존성 연결→품질 검증→거버넌스" 7단계로, 각 단계가 다음 단계의 토대가 된다.

**헤드라인:** "작업표준서를 정비하듯 Prompt를 정비한다 — 발굴부터 감사 대응까지 7단계"

```
[1단계] 인벤토리 (Inventory)    ← 개인 PC·코드·채팅창에서 Prompt 발굴
        ↓
[2단계] 구조화 (Structuring)    ← 5요소 표준 템플릿으로 변환
        ↓
[3단계] 메타데이터 부착 (Metadata) ← 소유자·버전·용도·연계모델 등록
        ↓
[4단계] 버전관리 (Versioning)   ← Registry 등록, 변경 이력 관리
        ↓
[5단계] 리니지·의존성 연결 (Lineage) ← 참조 데이터·Tool·평가셋 연결
        ↓
[6단계] 품질 검증 (Quality)     ← 평가셋으로 성능 측정, 회귀 테스트
        ↓
[7단계] 거버넌스 (Governance)   ← 승인 워크플로·접근권한·감사 로그
```

각 단계는 앞 단계가 있어야 다음 단계가 가능하다:
- 인벤토리 없이 구조화 불가, 메타데이터 없이 버전관리 불가, 의존성 연결 없이 영향도 분석 불가, 평가셋 없이 품질 회귀 감지 불가

🏭 **제조 현업 예시:** 설비 점검 절차서를 정비하는 방식과 동일하다. "어디에 어떤 절차서가 있는지 파악(인벤토리)" → "표준 양식으로 재작성(구조화)" → "담당자·개정일·적용 설비 기입(메타데이터)" → "개정 이력 관리(버전관리)" → "어떤 설비에 어떤 절차서가 연결되는지 연계(리니지)" → "실제 작업에 적용해 검증(품질)" → "변경 시 팀장 승인(거버넌스)" 순서다.

> ▸ **백업 장표:** [백업 12-1] 단계별 활동·산출물·선행조건 상세표

#### [백업 12-1] 단계별 활동·산출물·선행조건 (디테일)

| 단계 | 핵심 활동 | 산출물 | 선행조건 |
|---|---|---|---|
| 1. 인벤토리 | 코드 내 인라인 Prompt, 설정 파일(YAML/JSON), Slack, 개인 PC, 노션 전수 탐색. 유형 분류: System Prompt / User Prompt / Few-shot 예시 / Tool 정의 / 판단 기준 | Prompt 목록표 (ID·위치·담당자·용도·수정일) / 미발견 Prompt 위험 목록 | 없음 (최초 단계) |
| 2. 구조화 | 5요소 표준 템플릿으로 변환. Text Prompt vs Chat Prompt 유형 명시. 변수(`{{variable}}`) 추출 | 표준화된 Prompt 템플릿 파일(YAML/JSON) / 변수 목록 | 인벤토리 완료 |
| 3. 메타데이터 | 소유자·버전·설명·태그·모델·수정일·라이프사이클 단계·연계 업무 입력. LLM 자동 생성으로 초안 작성 후 담당자 확정 | 메타데이터 완성된 Prompt 자산 파일 | 구조화 완료 |
| 4. 버전관리 | Registry 등록(v1.0.0 시작). 변경 시 시맨틱 버전(MAJOR.MINOR.PATCH) 규칙 준수. 불변 버전 원칙(한 번 등록된 버전은 수정 불가, 변경은 새 버전으로) | Registry 등록 이력 / 버전 Diff 뷰 | 메타데이터 부착 완료 |
| 5. 리니지·의존성 | 참조 데이터 자산·Glossary·Tool·평가셋·업스트림/다운스트림 Prompt 연결. 의존성 맵 작성 | Prompt Dependency Map / 영향 범위 분석 | 버전관리 시작 |
| 6. 품질 검증 | 평가셋(Golden Set) 구축 (실제 사례 20~500건). 자동 회귀 테스트 CI/CD 연동 | 평가 리포트(정확도·형식 준수율·실패 사례) / 성능 기준선 | 의존성 연결 완료 |
| 7. 거버넌스 | RBAC(역할 기반 접근 제어) 설정. 승인 워크플로 (고영향 Prompt는 변경 전 승인자 검토). 감사 로그 구성 | 역할 정의서 / 감사 로그(Audit Trail) / 접근 정책 문서 | 버전관리 기반 완성 |

---

### [슬라이드 13] Prompt Registry와 버전관리 — 코드처럼 관리하되 현업도 쓸 수 있게

**👉 한 문장 요약:** Prompt Registry(중앙 저장소)에 등록된 Prompt는 코드 재배포 없이 핫픽스하고, 레이블(label = 특정 버전을 가리키는 가변 포인터)만 바꿔 즉시 롤백할 수 있다.

**헤드라인:** "Registry = 조직의 모든 Prompt에 대한 단일 진실 원천 — 버전을 바꿔도 코드는 그대로"

**Prompt Registry의 핵심 원칙:**
- **단일 진실 원천(Single Source of Truth):** Prompt를 코드에서 분리해 중앙에서 관리
- **버전 불변성(Immutable Versions):** 한 번 등록된 버전은 절대 수정 불가. 변경은 반드시 새 버전으로 — 이 불변성이 분산 추적의 신뢰성을 보장한다
- **레이블(Label) = 배포 포인터:** 코드는 "production 레이블"을 참조한다. production 레이블이 가리키는 버전을 바꾸면 코드 변경 없이 즉시 배포 전환·롤백이 가능하다

**시맨틱 버저닝(Semantic Versioning = MAJOR.MINOR.PATCH 형식으로 변경의 심각도를 표시하는 체계):**

| 버전 구분 | 변경 의미 | 두산 현업 예시 |
|---|---|---|
| MAJOR (X.0.0) | 출력 형식 변경, 역할 변경 등 하위 호환이 깨지는 근본 변경 | "판정 결과를 3단계에서 5단계로 변경" |
| MINOR (x.Y.0) | 새 판단 조건 추가, 기존 호환 유지 | "계절적 온도 보정 로직 추가" |
| PATCH (x.y.Z) | 표현 명확화, 오타 수정 (동작 변화 없음) | "단위 표기 mm/s → mm/s(RMS)로 수정" |

**배포 워크플로 (7단계):**
새 버전 생성(latest 레이블) → 개발 환경 검증 → staging 레이블 이동 → 자동 회귀 테스트 → [고영향 Prompt] 승인자 검토 → production 레이블 이동 → 모니터링 / 문제 시 즉시 롤백

🏭 **제조 현업 예시:** 설비 판정 기준이 개정됐을 때, 기존 방식이라면 개발팀에 의뢰해 코드를 수정·배포해야 했다. Registry를 쓰면 도메인 담당자가 Registry UI에서 새 버전을 등록하고, 승인자 확인 후 production 레이블을 이동하면 코드 변경 없이 새 기준이 즉시 적용된다.

> ▸ **백업 장표:** [백업 13-1] MLflow Prompt Registry 등록 코드 예시 / [백업 13-2] Langfuse 버전-레이블 분리 모델 상세

#### [백업 13-1] MLflow Prompt Registry 등록 예시 (디테일)

```python
import mlflow

mlflow.genai.register_prompt(
    name="maintenance-defect-judge",           # 고유 이름
    template=prompt_template_text,             # 5요소 포함 템플릿
    commit_message="2026년 기준 개정 반영",     # 변경 이유 (Git 커밋 메시지 역할)
    tags={
        "author": "hong.gildong@doosan.com",   # 작성자
        "task": "defect_classification",        # 업무 유형
        "language": "ko",
        "domain": "maintenance",
        "eval_dataset": "defect-eval-v2",       # 연결된 평가 데이터셋
    },
    model_config={
        "model_name": "claude-sonnet-4-6",
        "temperature": 0.1,
        "max_tokens": 500,
    }
)
```

한 번 등록된 버전은 수정 불가(Immutable). 변경이 필요하면 위 코드를 다시 실행해 새 버전을 생성한다.

#### [백업 13-2] Langfuse 버전-레이블 분리 모델 (디테일)

```
Version 1  ──────────────────────────────
Version 2  ──── [staging label] ─────────
Version 3  ──── [production label] ───── ← 현재 프로덕션
Version 4  ──── [latest label] ──────── ← 최신 개발 버전
```

- **Version(버전):** 변경 불가(Immutable)한 이력. 자동 증가
- **Label(레이블):** 특정 버전을 가리키는 포인터. 코드는 레이블을 참조하므로, 레이블만 변경하면 코드 수정 없이 배포 버전 전환 가능
- **보호 레이블:** `production` 레이블은 Project Admin/Owner만 변경 가능. 일반 Member는 수정 불가해 무단 프로덕션 변경 방지

---

### [슬라이드 14] Prompt 의존성 맵 — "이 Prompt를 바꾸면 무엇이 영향받는가"

**👉 한 문장 요약:** Prompt 의존성 맵(Dependency Map)은 특정 Prompt가 의존하는 데이터·Glossary·Tool·평가셋·연결된 다른 Prompt를 한눈에 보여줘, 변경 전에 영향 범위를 파악하게 한다.

**헤드라인:** "Prompt 리니지 = 데이터 리니지의 Prompt 버전 — 무엇에 의존하는지 알아야 안전하게 바꿀 수 있다"

Prompt Dependency Map(프롬프트 의존성 맵)은 데이터 리니지(Data Lineage = 데이터가 어디서 왔고 어디로 흘러가는지를 추적하는 계보)의 Prompt 버전이다.

**의존성 맵 예시 (설비 이상 판정 Prompt v3.1):**
```
[설비 이상 판정 Prompt v3.1]
    ├── 데이터 의존: 설비 센서 API v2.3 / 정비 이력 DB / 부품 수명 기준 테이블
    ├── Glossary 의존: 설비 기술 용어집 v3 (용어 247개)
    ├── Tool 의존: 진동 분석 API / 정비 이력 조회 함수
    ├── 평가셋 의존: defect-eval-set-v2 (500건)
    └── 다운스트림 Prompt: 작업 지시 생성 Prompt v1
```

**의존성 맵의 3가지 핵심 활용:**

- **모델 변경 시:** GPT-4o → Claude Sonnet으로 교체 시 이 모델에 의존하는 모든 Prompt를 추출해 회귀 테스트 자동 실행
- **데이터 자산 변경 시:** 설비 Glossary v3 → v4 업데이트 시 이 Glossary에 의존하는 모든 Prompt 담당자에게 자동 검토 알림 발송
- **Prompt 체인 변경 시:** 업스트림 Prompt 변경이 다운스트림 Prompt의 출력을 바꾸는 연쇄 영향 사전 파악 (의존성 맵 없이는 운영 중 발견하게 된다)

🏭 **제조 현업 예시:** 설비 기술 용어집이 개정될 때, 용어집에 의존하는 판정 기준 Prompt·보고서 생성 Prompt·작업 지시 Prompt에 자동으로 "검토 필요" 알림이 간다. 수동으로 연관 자산을 찾는 수고 없이 영향 범위가 즉시 가시화된다.

> ▸ **백업 장표:** [백업 14-1] 의존성 맵 표준 필드 및 자동화 방법

#### [백업 14-1] 의존성 맵 표준 필드 및 자동화 (디테일)

| 의존 유형 | 필드명 | 설명 | 예시 |
|---|---|---|---|
| 데이터 자산 | `data_dependencies` | 참조하는 DB·API·파일 | `["sensor_api_v2", "maintenance_log_db"]` |
| 용어·Glossary | `glossary_dependencies` | 참조하는 도메인 용어집 | `["equipment_glossary_v3"]` |
| Tool·기능 | `tool_dependencies` | 호출하는 함수·API·에이전트 | `["vibration_analyzer", "history_lookup"]` |
| 평가 데이터 | `eval_dependencies` | 성능 테스트 기반 데이터셋 | `["defect-eval-set-v2"]` |
| 업스트림 Prompt | `prompt_dependencies` | 이 Prompt에 입력을 주는 다른 Prompt | `["retrieval-prompt-v2"]` |
| 다운스트림 Prompt | `downstream_prompts` | 이 Prompt의 출력을 사용하는 Prompt | `["action-recommendation-v1"]` |

**자동화 방법:**
- MLflow Prompt Registry: Tracing·Evaluation·Monitoring 통합으로 어떤 Prompt 버전이 어떤 추적(Trace)을 생성했는지 자동 연결
- LangSmith: 각 실행 Trace를 특정 Prompt 커밋 해시에 자동 연결
- Azure AI Prompt Flow: DAG(방향성 비순환 그래프)로 노드 간 연결을 시각화해 의존성 명시적 관리

---

### [슬라이드 15] 품질 검증과 드리프트 탐지 — Prompt 자산을 건강하게 유지하는 방법

**👉 한 문장 요약:** 평가셋(기준 테스트 케이스)을 Prompt에 연결해두면, 모델이 바뀌거나 데이터 분포가 달라져도 품질 저하를 현장 영향 전에 자동으로 탐지할 수 있다.

**헤드라인:** "골든셋(Golden Set) = Prompt의 건강검진표 — 변경할 때마다 반드시 통과해야 하는 자동 관문"

**Prompt 품질 저하의 3가지 원인:**

| 원인 | 설명 | 탐지 방법 |
|---|---|---|
| 모델 조용한 업데이트 | 제공사가 모델을 변경하면서 공지하지 않음 | 모델 버전 고정(Pin) + 정기 회귀 테스트 |
| 입력 분포 변화 | 사용자 입력 패턴이 시간에 따라 달라짐 | 프로덕션 입력 분포 모니터링 |
| 체인 Prompt 연쇄 변경 | 업스트림 Prompt 변경이 다운스트림에 영향 | 의존성 맵 + 전체 체인 테스트 |

**성능 저하 감지와 개선 사이클:**
```
[평시] 프로덕션 출력 → 자동 샘플링 → LLM-as-Judge 채점 → 대시보드 트렌드
        ↓ 임계값 위반 시 경보
[이상 탐지] Prompt 변경 없음 + 점수 하락 → 모델/입력 드리프트 의심 → 회귀 테스트로 원인 확인
            최근 Prompt 변경 있음 + 점수 하락 → 이전 버전으로 즉시 롤백
[개선] 프로덕션 실패 사례 → 평가셋에 추가 → 새 버전 테스트 → A/B 테스트 후 전환
```

🏭 **제조 현업 예시:** 설비 판정 Prompt에 과거 판정 사례 500건을 평가셋으로 연결한다. LLM API가 업데이트될 때마다 자동으로 이 500건을 테스트하고, 정확도가 90% 아래로 떨어지면 자동 경보가 울린다. 현장 사고 전에 문제를 잡는다.

> ▸ **백업 장표:** [백업 15-1] 품질 감지 지표(KPI) 상세 / [백업 15-2] A/B 테스트와 골든셋 구축 방법

#### [백업 15-1] Prompt 품질 감지 지표 (디테일)

| 지표 | 설명 | 기준값 예시 |
|---|---|---|
| 정확도(Accuracy) | Golden Set 대비 정답률 | 95% 이상 유지 |
| 사용자 수정률(User Edit Rate) | AI 출력을 사용자가 수정한 비율 | 5% 이하 |
| 실패율(Failure Rate) | 형식 오류·빈 응답·예외 응답 비율 | 1% 이하 |
| 출력 길이 변화 | 평균 응답 길이 트렌드 | ±20% 이내 |
| 지연 시간(Latency) | 응답 속도 변화 | 기준 대비 +30% 이내 |
| LLM-as-Judge 점수 | 별도 Judge 모델이 품질 자동 평가 | 4.0/5.0 이상 |

LLM-as-Judge(LLM을 판사로 사용 = 고신뢰 모델이 프로덕션 출력 샘플을 자동 채점하는 방법): 점수 하락 패턴이 감지되면 드리프트 경보. 사람이 레이블링 없이 자동화 가능.

#### [백업 15-2] A/B 테스트와 골든셋 구축 (디테일)

**골든셋(Golden Dataset) 구축 방법:**
- 실제 프로덕션 로그에서 추출한 20~500개의 대표 입력·기대 출력 쌍
- 합성 예시보다 실제 사용 패턴을 반영하므로 신뢰도 높음
- 새 Prompt 버전 배포 전 필수 통과 게이트(Gate)

**A/B 테스트 절차:**
1. 신규 Prompt 버전을 전체 트래픽의 10%에만 적용
2. 동일 기간 동안 기존 버전(90%)과 성능 지표 비교
3. 통계적 유의성(p < 0.05) 확인 후 전면 전환

**CI/CD 통합 예시:**
```yaml
on: [prompt_registry_update]
jobs:
  regression_test:
    - run: agenta eval --prompt maintenance-defect-judge --dataset defect-eval-v2
    - check: score >= 0.90  # 90% 미만이면 배포 차단
```

---

### [슬라이드 16] 솔루션 선택 길잡이 — 우리 스택에 맞는 도구를 고르는 법

**👉 한 문장 요약:** Prompt/Harness 자산화를 지원하는 도구는 전용 Prompt 생명주기 관리 도구, 통합 LLMOps 플랫폼, 클라우드 관리형 서비스 세 범주로 나뉘며, 기존 스택과의 연계성으로 선택한다.

**헤드라인:** "기존 인프라가 무엇이냐에 따라 시작 도구가 달라진다"

도구 선택의 첫 번째 기준은 "기존 개발·데이터 인프라와 연계":

| 상황 | 권장 도구 방향 |
|---|---|
| 기존 MLflow 인프라 활용 | MLflow Prompt Registry (오픈소스, 기존 파이프라인 통합) |
| AWS 환경 | Amazon Bedrock Prompt Management (관리형 서비스) |
| Azure DevOps + CI/CD | Azure AI Foundry Prompt Flow |
| GCP + Gemini 중심 | Google Vertex AI Prompt Management |
| 현업(비기술 담당자) 협업 중시 | PromptLayer, Agenta, Vellum (UI 친화적) |
| 오픈소스 + 자체 호스팅 | Agenta, Langfuse, MLflow |
| 소규모 팀 + 빠른 시작 | Langfuse Cloud, LangSmith (무료 티어 있음) |

어느 도구를 선택하든 핵심 기능은 동일해야 한다: **버전관리 + 메타데이터 + 배포 레이블 + 회귀 테스트 연동**. 이 네 기능이 없으면 자산화 도구가 아니라 단순 파일 저장소에 불과하다.

🏭 **제조 현업 예시:** 두산 계열사가 이미 AWS를 쓰고 있다면 Bedrock Prompt Management로 시작하는 것이 연동 비용이 가장 낮다. 현업 품질 담당자가 직접 Prompt를 검토해야 한다면 UI가 직관적인 PromptLayer나 Agenta를 추가로 고려한다.

> ▸ **백업 장표:** [백업 16-1] Prompt/Harness 자산화 지원 도구 전체 비교표

#### [백업 16-1] Prompt/Harness 자산화 지원 도구 전체 비교표 (디테일)

| 솔루션 | 제공사 | 분류 | 오픈소스 | 핵심 기능 | AI/자동화 기능 | 비고 |
|---|---|---|---|---|---|---|
| LangSmith / LangChain Hub | LangChain | 통합 LLMOps | 부분 | 커밋 해시 버전관리, Diff 비교, Webhook, RBAC, 평가셋 관리 | 자동 Trace-Prompt 연결, A/B 테스트 | 광범위한 생태계 |
| MLflow Prompt Registry | Linux Foundation/Databricks | 통합 MLOps | O | 버전관리, Alias, Tags, model_config, Jinja2 템플릿 | 자동 캐싱, Prompt-Trace 리니지 통합 | 기존 MLflow 팀 적합 |
| Langfuse | Langfuse GmbH | 통합 LLMOps | O | 버전(Immutable)+레이블 분리, 보호 레이블, config 버전관리 | 자동 성능 모니터링 | EU 데이터 호스팅, 자체 호스팅 지원 |
| PromptLayer | PromptLayer | 전용 Prompt | X(SaaS) | Prompt Registry(CMS), A/B 테스트, Release 레이블 | 자동 회귀 테스트, 비용·지연·품질 추적 | 비기술 담당자 친화적 UI |
| Agenta | Agentatech | 전용 Prompt | O | 버전관리, 평가, Human Annotation, 배포, 관찰성 통합 | 자동 드리프트 탐지, 온라인 평가 | 오픈소스 중 완성도 높음 |
| Vellum | Vellum AI | 전용 Prompt | X(SaaS) | 시각적 워크플로 빌더, 멀티 모델 비교, RBAC | 시각적 DAG 의존성 관리 | 엔터프라이즈 VPC 지원 |
| Braintrust | Braintrustdata | 전용 Prompt | X(SaaS) | 버전관리, A/B 테스트, Golden Set 평가 | LLM-as-Judge 자동 평가 | 평가 기능 강점 |
| Azure AI Foundry (Prompt Flow) | Microsoft | 클라우드 관리형 | 부분 | DAG 기반 워크플로, YAML 정의, Git/DevOps 통합 | 자동 평가 통합, 배포 자동화 | Azure 생태계 통합 ※Prompt Flow는 기능개발 종료·퇴역 예정(후속 Azure AI Foundry로 이전 권고) |
| Amazon Bedrock Prompt Mgmt | AWS | 클라우드 관리형 | X | ARN 기반 식별, 버전 스냅샷, Prompt 최적화 | 자동 Prompt 최적화 제안 | AWS 생태계 통합 |
| Google Vertex AI Prompt Mgmt | Google Cloud | 클라우드 관리형 | X | SDK 기반 버전 생성, CMEK/VPC 지원 | 자동 버전 이력 | GA 출시(2025), 엔터프라이즈 보안 |

---

### [슬라이드 17] 자산화 시 흔한 함정과 회피책

**👉 한 문장 요약:** Prompt 자산화에서 가장 자주 실패하는 지점은 인벤토리를 건너뛰고 도구부터 도입하거나, 메타데이터 없이 파일만 저장하거나, 평가셋 없이 배포하는 것이다.

**헤드라인:** "도구 도입이 자산화가 아니다 — 인벤토리·메타데이터·평가셋이 없으면 Registry는 파일함에 불과하다"

| 함정 | 증상 | 회피책 |
|---|---|---|
| **인벤토리 없이 시작** | 새 Registry는 비어 있고, 실제로 쓰이는 Prompt는 코드 안에 여전히 하드코딩 | 코드 스캔으로 인라인 Prompt 자동 탐지 후 Registry로 마이그레이션 |
| **메타데이터 없는 등록** | Registry에 Prompt는 있지만 "누가 왜 만들었는지" 아무도 모름 | LLM 자동 생성으로 초안 작성 후 담당자 확정 의무화 |
| **평가셋 없는 배포** | 새 버전이 기존보다 나은지 나쁜지 배포 후에야 현장에서 발견 | 배포 전 최소 20건 Golden Set 통과를 CI 게이트로 강제 |
| **소유권 없는 공유 자산** | "공동 소유"인 Prompt는 실질적으로 아무도 관리 안 함 | 모든 Prompt에 단일 소유자 지정, 공동 소유 금지 원칙 |
| **Shadow AI 방치** | 승인 없이 각자 채팅창 Prompt를 업무에 쓰는 상태가 지속 | Registry 없는 AI 업무 사용을 정책으로 금지, 쉬운 등록 경로 제공 |

🏭 **제조 현업 예시:** "Prompt Registry 도구를 도입했지만 등록한 Prompt가 실제 운영에 쓰이는 것과 달라서 Registry가 무용지물"인 경우가 많다. 인벤토리 → 메타데이터 → 평가셋 연결 순서를 지키는 것이 핵심이다.

---

## 6. KPI? — Prompt/Harness 자산화 관리 지표

### [슬라이드 18] 관리 KPI — 5개 지표로 자산화 진척을 측정한다

**👉 한 문장 요약:** 자산화 KPI는 모델 성능이 아니라 "지시·판단 로직이 얼마나 잘 데이터 자산으로 전환·관리되고 있는가"를 5개 지표로 측정한다.

**헤드라인:** "자산화가 실질적으로 일어나고 있는지 5개 숫자로 확인한다"

| KPI | 쉬운 뜻 | 측정 목적 | 방향 | 단기 목표 | 장기 목표 |
|---|---|---|---|---|---|
| **① Prompt 자산화율** | 운영 중인 Prompt 중 Registry에 등록·관리되는 비율 | 코드·개인 파일에 묻힌 Prompt를 발굴해 조직 자산으로 편입하는 진척 관리 | ↑ | 50% | 95% |
| **② 메타데이터 완성도** | 등록된 Prompt가 소유자·버전·용도·연계모델 등 필수 6개 필드를 모두 채운 비율 | 메타데이터 미비로 인한 재사용 불가·감사 실패 방지 | ↑ | 70% | 95% |
| **③ 버전관리·승인 준수율** | Prompt 변경 시 버전 태깅 + 리뷰어 승인 절차를 거친 비율 | 무단 변경으로 인한 판단 로직 훼손·사고 방지 | ↑ | 70% | 100% |
| **④ 회귀 테스트 통과율** | 변경된 Prompt가 골든셋 테스트를 통과하는 비율 (일일 회귀 발생률: 전날 대비 품질 하락 Prompt 비율) | 모델 업데이트·Prompt 수정 시 품질 저하를 현장 영향 전에 사전 탐지 | 통과율 ↑ / 발생률 ↓ | 통과율 80% / 발생률 <5% | 통과율 90% / 발생률 <2% |
| **⑤ Harness·Prompt 재사용률** | 등록된 자산이 2개 이상 서비스·프로젝트에서 재활용되는 비율 | 자산화의 실질 가치 검증 — 재사용 없으면 Registry는 파일함에 불과 | ↑ | 20% | 60% |

🏭 **제조 현업 기대효과:** ① 설비 점검 절차 판단 로직이 누구인지 즉시 확인 가능 ② 품질 불량 판정 기준 버전 추적으로 감사 대응 ③ 현장 담당자 임의 수정 → 불량 유출 사고 리스크 차단 ④ LLM API 업그레이드 시 설비 이상 탐지 Prompt 품질 하락 자동 감지 ⑤ 밥캣·테스나·두산에너빌리티 공통 Prompt 중복 개발 제거

> ▸ **백업 장표:** [백업 18-1] KPI 정식 산식 및 측정 방법 상세

#### [백업 18-1] KPI 정식 산식 및 측정 방법 (디테일)

**KPI-1. Prompt 자산화율**
- 산식: (Registry 등록 Prompt 수 / 전체 운영 Prompt 추정 수) × 100
- 측정 도구: LangSmith Prompt Hub, Langfuse Prompt Management, PromptLayer Prompt Registry
- 근거: Digital Applied (2026) — "Catalog coverage is the cheapest, highest-signal indicator of whether the library is being maintained or just accumulated. Target: >95%."

**KPI-2. 메타데이터 완성도**
- 산식: (6개 필수 필드 전부 입력된 Prompt 수 / 전체 Registry 등록 Prompt 수) × 100
- 필수 6개 필드: 소유자(Owner), 버전(Version), 모델(Model), 최종 수정일(Last-edited), 라이프사이클 단계(Lifecycle Stage), 연계 기능·업무(Feature/Use-case)
- 목표값 미충족 시 배포 블록 적용 권장

**KPI-3. 버전관리·승인 준수율**
- 산식: (버전 태깅 + 리뷰어 승인 기록이 있는 변경 건수 / 전체 Prompt 변경 건수) × 100
- 측정 도구: LangSmith(버전 diff 추적), Humanloop(승인 워크플로), PromptLayer(audit log)

**KPI-4. 회귀 테스트 통과율 / 일일 회귀 발생률**
- 통과율 산식: (Golden Set 통과 케이스 / 전체 테스트 케이스) × 100
- 일일 회귀 발생률 산식: (전일 대비 품질 하락 Prompt 수 / 평가 대상 Prompt 수) × 100
- 5% 초과 시 즉각 조사 트리거
- 측정 도구: Langfuse Eval, LangSmith Evaluations, DeepEval, Promptfoo

**KPI-5. Harness·Prompt 재사용률**
- 산식: (2개 이상 프로젝트에서 호출된 Prompt/Harness 수 / 전체 Registry 자산 수) × 100
- 측정 도구: LangSmith(Prompt Hub 활용 현황), Langfuse(prompt 호출 추적), PromptLayer(usage analytics)

---

## 7. Roadmap — Prompt/Harness 자산화 확산 로드맵

### [슬라이드 19] 단기 / 중기 / 장기 로드맵

**👉 한 문장 요약:** 단기에는 "무엇이 있는지 보이게" 인벤토리·Registry를 구축하고, 중기에는 "잘 작동하는지 측정 가능하게" 의존성·평가·거버넌스를 연결하고, 장기에는 "스스로 건강을 모니터링하는" 자동화와 전사 통합을 완성한다.

**헤드라인:** "Level 1(흩어짐) → Level 3(표준화) → Level 5(최적화), 18개월 안에 두 단계 도약"

**단기 (0~6개월) — 인벤토리·Registry·메타데이터 구축**
> 목표: "우리 조직에 어떤 Prompt/Harness가 있는가"를 처음으로 가시화

- Prompt 인벤토리 수행: 코드베이스·공유 드라이브·개인 문서 전수 탐색 → 운영 Prompt 목록화
- Prompt Registry 도입: LangSmith Prompt Hub 또는 Langfuse Prompt Management → 중앙 저장소 단일화
- 메타데이터 스키마 정의: 필수 6개 필드 표준화 + 소유권(단일 소유자) 할당
- 버전관리 기반 배포: Git 연동 또는 플랫폼 내 버전 태깅 → "코드처럼 관리" 출발점
- KPI-1, KPI-2 베이스라인 측정 및 대시보드 구축

**중기 (6~18개월) — 리니지·의존성·거버넌스·평가셋 연계**
> 목표: "이 Prompt가 어디서 왔고 어디에 쓰이며 얼마나 잘 작동하는가" 측정 가능하게

- Prompt 리니지·의존성 맵: Prompt → 모델 → 데이터소스 → 다운스트림 서비스 간 의존성 시각화
- 거버넌스 승인 워크플로: Prompt 변경 시 도메인 전문가 + AI 엔지니어 승인 의무화
- 골든셋(Golden Set) 구축: Prompt별 10~50개 대표 테스트 케이스 + 자동 회귀 테스트 CI/CD 연동
- 라이프사이클 정책 시행: Draft → Production → Deprecated → Retired 4단계 표준화
- 재사용 메커니즘: Prompt Library UI를 통한 검색·브라우징, Harness 재사용 가이드 발행
- KPI-3, KPI-4, KPI-5 전체 측정 체계 완성

**장기 (18개월~) — 자동 드리프트 탐지·전사 카탈로그 통합**
> 목표: "Prompt/Harness 자산이 자기 건강을 스스로 모니터링하고 전사 데이터 체계와 통합"

- 자동 드리프트 탐지: 모델 업데이트·데이터 분포 변화를 야간 자동 평가로 탐지. 품질 하락 2% 초과 시 소유자 자동 알림
- LLM-as-Judge 평가 자동화: 사람 레이블링 없이 LLM이 출력 품질 자동 채점. 평가 커버리지 80% 이상
- 데이터 카탈로그 통합: Prompt/Harness 자산을 기업 데이터 카탈로그(Apache Atlas, Collibra 등)에 1급 항목으로 등록
- Glossary·온톨로지 연계: 업무 용어 정의와 Prompt 내 용어 정합성 자동 검증
- Feedback Loop 선순환: 현장 피드백 → 골든셋 자동 확장 → Prompt 품질 지속 개선
- 전사 거버넌스 위원회: Prompt/Harness 포트폴리오 운영위원회 (IT + 비즈니스 도메인 전문가)

🏭 **제조 현업 기대효과 종합:**

| 단계 | 시나리오 | 기대효과 |
|---|---|---|
| 단기 | 설비 점검 판단 로직 Prompt 50개 Registry 등록 | "누가 만든 기준인지" 즉시 확인, 신규 엔지니어 온보딩 시간 단축 |
| 중기 | 품질 불량 판정 Harness에 골든셋 100건 연결 | LLM API 업그레이드 시 품질 하락 자동 감지 → 설비 라인 영향 사전 차단 |
| 중기 | Harness 재사용률 40% 달성 | 밥캣·테스나·두산에너빌리티 공통 Prompt 중복 개발 제거 → 연 개발비 ~30% 절감 추정 |
| 장기 | 전사 데이터 카탈로그 연동 | "이 판단 Prompt는 어떤 학습 데이터·설비 데이터를 쓰는가" 리니지 조회 → ISO/IEC AI 감사 대응 |
| 장기 | Feedback Loop 구축 | 현장 오판정 피드백 → 골든셋 자동 확장 → Prompt 품질 지속 개선 선순환 |

**연계 AI-Ready Data 주제:**
- 단기: 데이터 카탈로그(D-1), 메타데이터 관리(D-2)
- 중기: 데이터 리니지(D-4), 데이터 거버넌스(D-6), 평가 데이터(D-8)
- 장기: Glossary(D-5), Feedback Loop(D-9), 온톨로지(D-7)

> ▸ **백업 장표:** [백업 19-1] 로드맵 단계별 산출물·추가 기술 상세

#### [백업 19-1] 로드맵 단계별 산출물·추가 기술 (디테일)

| 단계 | 과제 | 핵심 산출물 | 추가 기술 |
|---|---|---|---|
| 단기 | Prompt 인벤토리 수행 | Prompt 목록표 (ID·위치·담당자·수정일) | 코드 스캔 도구 (grep, ast 파싱) |
| 단기 | Prompt Registry 도입 | 중앙 저장소 1개 | LangSmith(무료 티어), Langfuse Cloud |
| 단기 | 메타데이터 스키마 정의 | 6개 필수 필드 표준 문서 | 스프레드시트 기반 카탈로그 |
| 단기 | KPI 베이스라인 측정 | 초기 KPI 대시보드 | LangSmith/Langfuse 분석 기능 |
| 중기 | 리니지·의존성 맵 | Prompt Dependency Graph | MLflow Lineage, Azure Prompt Flow DAG |
| 중기 | 거버넌스 승인 워크플로 | 역할 정의서 + 감사 로그 | Humanloop, LangSmith 워크플로 |
| 중기 | 골든셋 구축·CI 연동 | 프로덕션 사례 20~500건 평가셋 | DeepEval, Promptfoo, GitHub Actions |
| 중기 | 라이프사이클 정책 | Draft/Production/Deprecated/Retired 정책 문서 | Langfuse 레이블 보호 기능 |
| 장기 | 자동 드리프트 탐지 | 야간 자동 평가 파이프라인 | Arize AI, Datadog LLM Observability |
| 장기 | 데이터 카탈로그 통합 | Prompt 자산 카탈로그 등록 항목 | Apache Atlas, Collibra, Alation |
| 장기 | Glossary 연계 | 용어 정합성 자동 검증 규칙 | 자체 스크립트 또는 카탈로그 API |
| 장기 | 전사 거버넌스 위원회 | 분기별 경영진 보고서 | 포트폴리오 리뷰 프레임워크 |

---

## 별첨

### [별첨 A] Prompt 자산 등록 요청 필수항목 체크리스트

새 Prompt를 Registry에 등록할 때 다음 항목이 모두 채워져야 배포 가능하다:

**필수 항목 (미입력 시 배포 차단):**
- [ ] `name` — 고유 이름 (URI 형태, 예: `doosan-maintenance-defect-judge`)
- [ ] `version` — 시맨틱 버전 (예: `v1.0.0`)
- [ ] `description` — 이 Prompt가 하는 일을 한 문장으로
- [ ] `author` — 최초 작성 담당자 (이메일 포함)
- [ ] `business_owner` — 업무 책임자 (팀장급 이상)
- [ ] `model_name` — 최적화 대상 모델 (예: `claude-sonnet-4`)
- [ ] `use_case` — 연결된 업무 프로세스 (예: `설비 정기점검 → 이상 판정`)
- [ ] `lifecycle_stage` — 현재 상태 (`draft` / `staging` / `production` / `deprecated`)
- [ ] `commit_message` — 이번 버전 변경 이유 (예: `"2026년 설비 기준치 개정 반영"`)

**권장 항목 (가능하면 채울 것):**
- [ ] `tags` — 도메인·업무 분류 키워드 (예: `["maintenance", "quality", "inspection"]`)
- [ ] `data_dependencies` — 참조하는 데이터 자산 목록
- [ ] `eval_dataset` — 성능 테스트에 사용하는 평가셋 이름
- [ ] `review_cycle` — 검토 주기 (예: `분기별`)
- [ ] `model_config` — temperature, max_tokens 등

**배포 전 필수 검증:**
- [ ] 골든셋 회귀 테스트 통과 (통과율 ≥ 90%)
- [ ] 승인자 리뷰 완료 (MAJOR 버전 변경 시 필수, MINOR 이하는 선택)

---

### [별첨 B] Prompt 자산화 성숙도 자가 진단표

현재 조직의 수준을 파악하기 위한 자가 진단 체크리스트:

| 질문 | 예 | 아니오 |
|---|---|---|
| 운영 중인 Prompt 목록이 중앙에서 관리되고 있는가? | Level 3+ | Level 1-2 |
| 모든 운영 Prompt에 소유자가 지정되어 있는가? | Level 3+ | Level 1-2 |
| Prompt 변경 시 버전이 기록되고 롤백이 가능한가? | Level 3+ | Level 1-2 |
| 각 Prompt에 평가셋이 연결되어 있는가? | Level 4+ | Level 1-3 |
| Prompt 변경 시 자동 회귀 테스트가 실행되는가? | Level 4+ | Level 1-3 |
| 모델이 업데이트됐을 때 영향받는 Prompt를 즉시 파악할 수 있는가? | Level 4+ | Level 1-3 |
| 계열사 간 공통 Prompt 재사용이 이루어지고 있는가? | Level 5 | Level 1-4 |

---

## 참고 출처

1. [Shmueli, S. "Managing Prompts as Enterprise Assets: A Portfolio Approach." Medium.](https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327)
2. [Juršėnas, J. "Prompt Engineering Is Dead – Long Live PromptOps." Dataversity.](https://www.dataversity.net/articles/prompt-engineering-is-dead-long-live-promptops/)
3. [Paniego, S. & Gosthipaty, A. R. "Harness, Scaffold, and the AI Agent Terms Worth Getting Right." HuggingFace Blog.](https://huggingface.co/blog/agent-glossary)
4. [Parallel Web Systems. "What is an agent harness in the context of large-language models?"](https://parallel.ai/articles/what-is-an-agent-harness)
5. [Anthropic Engineering. "Effective context engineering for AI agents."](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
6. [Anthropic Engineering. "Effective harnesses for long-running agents."](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
7. [AWS Prescriptive Guidance. "Prompt, agent, and model lifecycle management."](https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html)
8. [Microsoft Learn. "YAML Schema Reference for Semantic Kernel Prompts."](https://learn.microsoft.com/en-us/semantic-kernel/concepts/prompts/yaml-schema)
9. [MLflow / Databricks. "Prompt Registry."](https://mlflow.org/docs/latest/genai/prompt-registry/)
10. [Amazon Web Services. "Amazon Bedrock Prompt Management."](https://aws.amazon.com/bedrock/prompt-management/)
11. [Rodek, M. "Why Large Enterprises Need a Prompt Registry for AI Governance." Medium.](https://medium.com/@martin_rodek/why-large-enterprises-need-a-prompt-registry-for-ai-governance-04d744039bb4)
12. [Tessl. "Prompts are the new source code: Why we need version control."](https://tessl.io/blog/prompts-are-the-new-source-code/)
13. [Agenta. "The Definitive Guide to Prompt Management Systems."](https://agenta.ai/blog/the-definitive-guide-to-prompt-management-systems)
14. [Agenta. "Prompt Drift: What It Is and How to Detect It."](https://agenta.ai/blog/prompt-drift)
15. [Agenta. "Prompt Versioning: The Complete Guide."](https://agenta.ai/blog/prompt-versioning-guide)
16. [Langfuse. "Version Control 공식 문서."](https://langfuse.com/docs/prompt-management/features/prompt-version-control)
17. [LangChain / LangSmith. "Manage Prompts 공식 문서."](https://docs.langchain.com/langsmith/manage-prompts)
18. [Digital Applied. "Prompt Library Metrics: Coverage, Regression Framework 2026."](https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026)
19. [Pinggy. "AI Harness Engineering: The Layer That Makes Your LLM Applications Actually Work."](https://pinggy.io/blog/best_ai_harnesses_to_supercharge_llm_models/)
20. [Fulcrum Digital. "Prompt Governance: The Emerging Enterprise Control Layer."](https://fulcrumdigital.com/blogs/prompt-governance-the-emerging-enterprise-control-layer/)
21. [AISquare / Medium. "Prompt Governance Scale: Versioning, Access, and Audits."](https://medium.com/@aisquarecommunity/prompt-governance-scale-versioning-access-and-audits-c76b58a166f1)
22. [Microsoft Azure. "Advance your maturity level for GenAIOps."](https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/concept-llmops-maturity)
23. [Augmentir. "Tacit Knowledge in Manufacturing: Unlocking Hidden Expertise with AI."](https://www.augmentir.com/glossary/tacit-knowledge)
24. [CertPro. "Prompt Security Risks: The Hidden Compliance Gap."](https://certpro.com/prompt-security-risks-enterprise-ai/)
25. [Wipro Tech Blogs. "Managing Prompt Technical Debt in Enterprise AI."](https://wiprotechblogs.medium.com/managing-prompt-technical-debt-in-enterprise-ai-7985f4bc7ff7)
26. [seangoedecke.com. "Prompts are technical debt too."](https://www.seangoedecke.com/prompts-are-technical-debt-too/)
27. [Traceloop Blog. "Automated Prompt Regression Testing with LLM-as-a-Judge and CI/CD."](https://www.traceloop.com/blog/automated-prompt-regression-testing-with-llm-as-a-judge-and-ci-cd)
28. [Chen, Zaharia, Zou (2023). "How Is ChatGPT's Behavior Changing over Time?" Stanford/UC Berkeley.](https://agenta.ai/blog/prompt-drift)
29. [Microsoft Azure AI Foundry — What is Prompt Flow 공식 문서.](https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/overview-what-is-prompt-flow)
30. [arxiv.org. "Natural-Language Agent Harnesses."](https://arxiv.org/html/2603.25723v1)
