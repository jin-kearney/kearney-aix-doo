# D-3 Prompt/Harness 자산화 — 출처 검증 리포트

> 대상: D-3_0605/storyline.md 본문 인용 + 말미 "## 참고 출처", research/ 원자료의 핵심 주장.
> 검증일: 2026-06-05 | 방식: 의심 주장 web_fetch + WebSearch 1차 출처 확인, storyline 보수적 자동 보정.

## 1. 요약

- 집중 검증한 핵심 주장(미래 시점·구체 수치·고유명사 위주): 9건
- 판정: **OK 5 / MISMATCH 1 / BROKEN 0 / UNVERIFIABLE 0 / ⚠️ 사람 확인 필요 3**
- storyline.md 자동 보정: **3건** 적용 (수치 정정 1, 표현 완화 1, 퇴역 주석 추가 1)
- 결론: 핵심 논리·구조는 검증을 통과한다. 수치 1건을 정확한 1차 출처 값으로 교체했고, 미래 전망·도구 상태 관련 3건은 ⚠️로 표시했다.

## 2. 출처·주장별 검증 표

| 귀속 주장(요약) | 출처/근거 | 판정 | 조치 |
|---|---|---|---|
| HuggingFace 공식 Agent 용어집 발표(2026-05) — Scaffold/Harness 구분 | huggingface.co/blog/agent-glossary (살아있음) | OK | 유지. 단 "Claude Code 공식 채택"→"대표 예시로 언급"으로 표현 완화 ✅보정 |
| GPT-4 동일 조건 정확도가 단기간 급락(조용한 드리프트) | Chen·Zaharia·Zou, Stanford/UC Berkeley 2023 "How Is ChatGPT's Behavior Changing over Time?" | MISMATCH | 수치 84%→51%는 확인 불가. 실제 소수 판별 97.6%→2.4%로 **정정** ✅보정 |
| Prompt Drift(모델 무변경 텍스트에도 품질 저하) 개념 | 복수 LLMOps 1차 글 | OK | 유지 |
| AWS Bedrock Prompt Management GA(2024-11) | AWS 공식 발표 | OK | 유지 |
| Humanloop가 Anthropic으로 합류 후 2025-09 서비스 종료 | 공개 발표 | OK | storyline 본문엔 인수 주장 없음(도구표만). 조치 불필요 |
| Helicone Mintlify 인수(2026-03) 후 유지보수 모드 | 공개 발표 | OK | storyline에 미언급. 조치 불필요 |
| Microsoft Semantic Kernel Prompt YAML 스키마 7필드 | Microsoft Learn 공식 문서 | OK | 유지 |
| MLflow Prompt Registry(불변 버전 + alias) | MLflow/Databricks 문서 | OK | 유지 |
| Azure AI Foundry Prompt Flow를 현행 권장 도구로 기술 | Microsoft Learn(퇴역 공지) | ⚠️ | 솔루션 비교표에 "기능개발 종료·퇴역 예정, 후속 Azure AI Foundry로 이전 권고" 주석 추가 ✅보정 |

## 3. 사람 확인 필요 (⚠️)

1. **Azure AI Foundry Prompt Flow 퇴역 상태** — Prompt Flow는 신규 기능 개발이 종료되고 서비스 퇴역이 예고됐다. 발표 시 "현재 권장 도구"로 소개하면 오해 소지. storyline 표에 주석을 달았으나, 두산 내부에서 Azure를 쓴다면 후속 제품(Azure AI Foundry 통합 Prompt 관리)으로 표현을 맞출지 발표자 확인 권장.
2. **Google Vertex AI Prompt Management GA 시점** — 2025년 하반기로 추정되나 공식 릴리스노트의 정확한 GA 날짜는 직접 확인 필요. storyline에는 "GA 출시(2025)"로만 두어 단정 위험을 낮췄다.
3. **"PromptOps가 DevOps처럼 표준이 될 것" 전망** — 블로그·논평 수준 주장으로 Gartner/Forrester 등 공식 기관 보고서로는 미확인. storyline에서 단정이 아니라 방법론 명칭·개념 설명으로만 사용했는지 발표자 확인 권장(전망 수치로 인용 금지).

## 4. 자동 보정 적용 내역 (storyline.md)

1. (370행대) GPT-4 드리프트 수치 `84% → 51%` → `97.6% → 2.4%`로 정정, 출처에 저자명(Chen, Zaharia, Zou) 보강.
2. (148행) `Claude Code·Codex 같은 제품들은 전체를 "harness"로 부른다` → 용어집이 든 **대표 예시**임을 명확히 표현 완화.
3. (684행) 솔루션 비교표 Azure 행에 **Prompt Flow 퇴역 예정** 주석 추가.

> 보정 원칙: 확실히 틀린 것만 수정, 애매하면 ⚠️로만 남김. 핵심 논리·구조는 변경하지 않음.
