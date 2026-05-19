+++
title = 'LLM을 도입했더니 개발이 빨라졌다고요? 근거는요?'
date = 2026-05-19T12:26:17+09:00
draft = false
+++

## 체감 생산성과 실제 전달 속도 사이의 간극을 DORA 메트릭으로 확인하기

“LLM을 도입하고 개발 생산성이 좋아졌습니다.”

요즘 개발 조직에서 자주 들리는 말입니다. GitHub Copilot, Cursor, Claude Code, ChatGPT 같은 도구가 개발자의 일상에 들어오면서 코드 작성, 테스트 초안 작성, 문서화, 리팩터링, 오류 분석 같은 작업이 훨씬 쉬워졌다는 이야기도 많습니다.

그런데 이 말을 들으면 꼭 하나의 질문을 해야 합니다.

**근거는요?**

개발자가 “빨라진 것 같다”고 느끼는 것과 실제로 조직의 개발 생산성이 좋아지는 것은 같은 말이 아닙니다. 코드를 더 빨리 작성했다고 해서 변경 사항이 고객에게 더 빨리 전달되는 것은 아닙니다. PR이 늘었다고 해서 배포가 빨라진 것도 아닙니다. AI 도구를 많이 썼다고 해서 제품 품질이 좋아진 것도 아닙니다.

진짜 질문은 이것입니다.

> **LLM 도입 이후, 우리 조직은 변경 사항을 더 빠르고 안정적으로 프로덕션에 전달하고 있나요?**

이 질문에 답하지 못한다면, 우리는 생산성이 좋아졌다고 말하는 것이 아니라 생산성이 좋아진 것처럼 느끼고 있을 뿐일 수 있습니다.

---

## 체감 생산성은 중요하지만, 충분하지 않습니다

LLM의 장점은 분명합니다.

반복적인 코드 작성이 빨라집니다. 낯선 API나 라이브러리를 파악하는 시간이 줄어듭니다. 테스트나 문서의 초안을 더 쉽게 만들 수 있습니다. 오류 메시지나 로그를 해석할 때 빠르게 힌트를 얻을 수도 있습니다.

실제로 AI 코딩 도구가 특정 작업에서는 큰 속도 개선을 만든다는 연구도 있습니다. GitHub Copilot 실험에서는 JavaScript HTTP server 구현 과제에서 Copilot 사용 그룹이 비사용 그룹보다 **55.8% 빠르게** 과제를 완료했습니다.

하지만 모든 연구가 같은 결론을 내리지는 않습니다.

METR이 2025년에 발표한 RCT에서는 숙련된 오픈소스 개발자들이 자신이 잘 아는 실제 저장소의 작업을 수행했을 때, AI 사용 시 작업 시간이 오히려 **19% 더 길어졌습니다**. 흥미로운 점은 개발자들이 실험 후에도 스스로는 20% 빨라졌다고 느꼈다는 점입니다.

물론 이 결과를 “AI는 개발자를 느리게 만든다”는 결론으로 일반화하면 안 됩니다. 이 연구는 숙련된 OSS 개발자 16명이라는 작은 표본, 자신이 잘 아는 대형 저장소, 특정 시점의 AI 도구라는 조건에서 수행되었습니다. METR도 2026년 업데이트에서 최신 도구에서는 speedup 가능성이 더 커졌다고 설명했습니다.

이 연구에서 우리가 가져가야 할 핵심은 단순합니다.

> **체감 생산성은 실제 생산성과 다를 수 있습니다.**

그래서 LLM 도입 효과는 느낌이나 사용량이 아니라 측정으로 확인해야 합니다.

---

## “많이 만들었다”와 “빨리 전달했다”는 다릅니다

소프트웨어 개발은 코드 작성으로 끝나지 않습니다.

하나의 변경 사항이 고객에게 도달하기까지는 요구사항 이해, 설계, 구현, 코드 리뷰, 테스트, 빌드, 배포 승인, 릴리스, 모니터링, 장애 대응까지 이어지는 흐름이 있습니다.

LLM이 구현 단계를 빠르게 만들어도 다음 단계가 준비되어 있지 않으면 병목은 다른 곳으로 이동합니다.

- 코드가 빨리 만들어질수록 리뷰 대기열이 길어질 수 있습니다.
- PR이 많아질수록 테스트와 배포 파이프라인이 밀릴 수 있습니다.
- 생성된 코드가 많아질수록 보안 검토와 운영 리스크도 커질 수 있습니다.

Faros AI의 분석에서도 비슷한 현상이 보고되었습니다. AI 사용 팀에서 task completion과 PR merge는 증가했지만, PR review time도 크게 증가했고 조직 수준의 DORA 지표 개선은 뚜렷하지 않았습니다. 즉, 개인의 산출량은 늘었지만 병목이 코드 생성에서 리뷰와 검증으로 이동한 것입니다.

DORA 2025 역시 AI를 “자동으로 생산성을 높여주는 도구”라기보다 조직의 강점과 약점을 함께 증폭하는 도구로 설명합니다. 좋은 개발 시스템에서는 AI 효과가 더 잘 나타날 수 있지만, 리뷰·테스트·배포·운영 체계가 약한 조직에서는 기존 병목이 더 커질 수 있습니다.

따라서 LLM 도입 효과를 보려면 이렇게 물어야 합니다.

> **코드를 더 많이 만들었나요?**  
> 가 아니라  
> **변경 사항을 더 빠르고 안정적으로 고객에게 전달했나요?**

이 질문에 답하기 위한 출발점이 DORA 메트릭입니다.

---

## DORA 메트릭은 전달 성과를 보는 중심 지표입니다

DORA 메트릭은 오랫동안 4대 지표로 알려져 있었습니다.

| 전통적 DORA 4대 메트릭 | 질문 |
|---|---|
| **Deployment Frequency** | 얼마나 자주 프로덕션에 배포하나요? |
| **Lead Time for Changes** | 변경 사항이 프로덕션에 반영되기까지 얼마나 걸리나요? |
| **Change Failure Rate** | 배포 후 장애, 롤백, 핫픽스가 얼마나 발생하나요? |
| **MTTR / Time to Restore Service** | 문제가 생겼을 때 얼마나 빨리 복구하나요? |

최근 DORA 공식 가이드는 이를 조금 더 구체화해 5개 delivery metrics로 설명합니다.

| 구분 | 지표 | 의미 |
|---|---|---|
| **Throughput** | Change Lead Time | commit부터 production 배포까지 걸리는 시간 |
| **Throughput** | Deployment Frequency | 일정 기간 동안의 배포 빈도 |
| **Throughput** | Failed Deployment Recovery Time | 배포 실패 후 복구까지 걸리는 시간 |
| **Instability** | Change Fail Rate | 배포 후 즉각적인 개입, rollback, hotfix가 필요한 비율 |
| **Instability** | Deployment Rework Rate | 운영 이슈나 사용자 영향 버그 때문에 발생한 계획되지 않은 배포 비율 |

LLM 도입 효과를 볼 때 이 지표들이 중요한 이유는 명확합니다.

LLM이 정말 생산성을 높였다면 변경 사항이 더 빨리 프로덕션에 도달해야 합니다. 배포 빈도도 개선될 수 있어야 합니다. 동시에 장애, 롤백, 핫픽스, 재작업이 늘지 않아야 합니다.

즉, LLM 생산성은 단순히 “AI를 많이 썼다”가 아니라 다음 질문에 답할 수 있어야 합니다.

- 더 빠르게 전달했나요?
- 더 자주 배포할 수 있게 되었나요?
- 실패율은 유지되거나 낮아졌나요?
- 문제가 생겼을 때 더 빨리 복구하나요?
- 재작업이 늘지는 않았나요?

이 질문에 답하는 데 DORA 메트릭이 유용합니다.

---

## 하지만 DORA만으로는 충분하지 않습니다

DORA는 중요한 지표입니다. 하지만 DORA만으로 개발 생산성을 모두 설명할 수는 없습니다.

DORA는 주로 “변경 사항이 고객에게 얼마나 빠르고 안정적으로 전달되는가?”에 답합니다. 하지만 LLM 도입 효과에는 그 외에도 중요한 질문들이 있습니다.

- 개발자가 지속 가능하게 일하고 있나요?
- 반복 업무가 줄었나요?
- 신규 입사자의 온보딩이 빨라졌나요?
- 문서화 품질이 좋아졌나요?
- 개발자의 번아웃이 줄었나요?
- 배포된 기능이 실제 사용자 가치로 이어졌나요?
- 서비스 신뢰성은 유지되고 있나요?

이 질문들은 DORA만으로는 충분히 답하기 어렵습니다.

그래서 SPACE나 DevEx 같은 프레임워크를 함께 봐야 합니다. SPACE는 개발자 생산성을 Satisfaction, Performance, Activity, Communication, Efficiency 관점에서 봅니다. DevEx는 개발자가 일을 더 잘할 수 있는 환경과 경험을 봅니다.

정리하면 이렇게 볼 수 있습니다.

| 질문 | 봐야 할 지표 |
|---|---|
| 변경이 더 빠르고 안정적으로 전달되나요? | DORA |
| 개발자가 지속 가능하게 일하고 있나요? | SPACE / DevEx |
| 서비스가 안정적인가요? | SLO, latency, error rate, incident |
| 제품 가치가 개선되었나요? | activation, retention, feature adoption |
| AI 비용 대비 효과가 있나요? | token cost per successful change |

DORA는 전달 성과를 보는 중심 지표입니다.  
하지만 거기서 멈추면 안 됩니다.

---

## 토큰을 많이 썼다고 생산성이 좋아진 것은 아닙니다

LLM 도입 효과를 측정할 때 자주 빠지는 함정이 있습니다.

바로 **AI 사용량을 성과로 착각하는 것**입니다.

예를 들어 이런 지표들이 있습니다.

- AI 도구 활성 사용자 수
- AI-assisted PR 비율
- 토큰 사용량
- 토큰 비용
- AI 호출 수
- 제안 수락률

이 데이터들은 필요합니다. 하지만 성과 지표는 아닙니다.

토큰을 많이 썼다는 것은 생산성이 좋아졌다는 뜻이 아닙니다.  
AI-assisted PR이 많다는 것도 개발 조직이 빨라졌다는 뜻이 아닙니다.  
제안 수락률이 높다는 것도 고객 가치가 더 빨리 전달되었다는 뜻은 아닙니다.

토큰 사용량은 기본적으로 **비용**입니다.  
AI 사용량은 기본적으로 **설명 변수**입니다.

토큰 사용량이 늘었다는 것은 AI를 적극적으로 활용하고 있다는 뜻일 수 있습니다. 하지만 동시에 요구사항이 불명확해서 AI에게 계속 다시 설명하고 있거나, 코드베이스 맥락을 제대로 정리하지 못했거나, AI가 낸 답을 고치느라 여러 번 재질문하고 있거나, 생성된 코드의 품질이 낮아 재작업이 늘고 있다는 신호일 수도 있습니다.

따라서 토큰 사용량은 단독으로 보면 안 됩니다. 반드시 전달 성과와 연결해서 봐야 합니다.

| 잘못된 해석 | 더 나은 해석 |
|---|---|
| 토큰 사용량이 늘었다 → AI 활용이 잘 되고 있다 | 토큰 사용량 대비 Change Lead Time이 줄었나요? |
| AI 호출 수가 많다 → 생산성이 높다 | AI 호출 후 review time과 rework가 줄었나요? |
| AI-assisted PR이 많다 → 개발 효율이 높다 | AI-assisted PR의 Change Fail Rate는 어떤가요? |
| 제안 수락률이 높다 → AI 품질이 좋다 | 수락된 코드가 배포 후 문제 없이 유지되나요? |
| 토큰 비용이 증가했다 → AI 투자가 늘었다 | token cost per successful change가 개선되었나요? |

LLM을 잘 쓰는 조직은 토큰을 많이 쓰는 조직이 아닙니다.

> **적은 마찰로 더 나은 변경을 더 빠르고 안정적으로 고객에게 전달하는 조직입니다.**

---

## LLM이 만든 속도에는 검증 비용이 붙습니다

LLM은 코드 생성 속도를 높일 수 있습니다. 하지만 코드 생성이 빨라질수록 검증 부담도 커질 수 있습니다.

DORA는 이를 **verification tax**로 설명합니다. AI가 코드나 아이디어 생성을 빠르게 해주더라도, 개발자는 AI output의 정확성을 검토하고, 환각 가능성을 확인하고, 추가 프롬프트로 수정하는 데 시간을 다시 쓸 수 있습니다.

이 검증 비용은 특히 코드 리뷰에서 나타납니다.

AI를 사용한 작성자는 빠르게 큰 PR을 만들 수 있습니다. 하지만 리뷰어는 그 PR을 여전히 사람이 읽고, 의도를 이해하고, 시스템 맥락에 맞는지 확인해야 합니다. 작성자의 속도 향상이 리뷰어의 인지 부하로 전가될 수 있는 것입니다.

따라서 LLM 도입 후 봐야 할 질문은 단순히 “코드 작성 시간이 줄었나요?”가 아닙니다.

> **절약된 시간이 리뷰, 검증, 재작업, 장애 대응으로 다시 지출되고 있지는 않나요?**

이 질문에 답하려면 DORA 메트릭과 함께 PR cycle time, review time, test failure rate 같은 진단 지표를 함께 봐야 합니다.

---

## PR 수와 리뷰 시간은 DORA와 경쟁하는 지표가 아닙니다

LLM 도입 효과를 측정할 때 PR 수, PR cycle time, review time, test failure rate 같은 지표도 중요합니다.

하지만 이 지표들은 DORA와 경쟁하는 별도의 최종 성과 지표가 아닙니다. 대부분은 DORA 메트릭을 계산하거나, DORA 메트릭이 왜 좋아졌는지 혹은 왜 나빠졌는지 설명하기 위한 진단 지표입니다.

예를 들어 Change Lead Time은 하나의 숫자로 보이지만 실제로는 여러 구간의 합입니다.

```text
commit
  → PR open
  → first review
  → approval
  → merge
  → deploy
  → production verification
```

LLM 도입 후 전체 Lead Time이 줄었다면 좋은 신호입니다. 하지만 어느 구간이 줄었는지를 봐야 합니다.

- `commit → PR open`이 줄었다면 코드 작성과 PR 준비가 빨라진 것입니다.
- `PR open → first review`가 늘었다면 리뷰 대기열이 병목이 된 것입니다.
- `first review → approval`이 길어졌다면 AI가 만든 코드의 검증 비용이 커졌을 수 있습니다.
- `merge → deploy`가 그대로라면 병목은 개발자가 아니라 CI/CD 파이프라인, 릴리스 정책, 보안·컴플라이언스 승인, 배포 window 같은 운영 거버넌스에 있을 수 있습니다.

즉, PR 수나 리뷰 시간은 최종 목표가 아닙니다.

> **DORA 메트릭을 해석하기 위한 하위 신호입니다.**

---

## 측정은 이렇게 시작하면 됩니다

처음부터 완벽한 대시보드를 만들 필요는 없습니다. 중요한 것은 같은 정의로 꾸준히 보는 것입니다.

먼저 서비스 단위로 시작하는 것이 좋습니다. 조직 전체 평균을 바로 내면 서비스별 맥락이 섞입니다. 어떤 서비스는 하루에도 여러 번 배포할 수 있지만, 어떤 서비스는 보안이나 규제 때문에 배포 주기가 느릴 수 있습니다.

다음 순서로 시작하면 됩니다.

### 1. 현재 흐름을 그립니다

commit은 어디서 시작되는지, PR은 언제 열리는지, 리뷰는 언제 시작되는지, merge 후 배포까지 얼마나 걸리는지, 배포 후 검증은 어떻게 끝나는지 확인합니다.

### 2. baseline을 잡습니다

LLM 도구를 전사 도입하기 전에 몇 주에서 몇 달 정도 baseline을 잡는 것이 좋습니다. 이미 도입했다면 도입 이전 데이터를 가능한 범위에서 복원하거나, 팀별 AI 사용률 차이를 비교합니다.

### 3. DORA 메트릭을 봅니다

최소한 다음 지표를 봅니다.

- Change Lead Time
- Deployment Frequency
- Change Fail Rate
- Failed Deployment Recovery Time
- Deployment Rework Rate

### 4. 진단 지표를 함께 봅니다

DORA 결과를 설명하기 위해 다음 지표를 함께 봅니다.

- PR cycle time
- Time to first review
- Review time
- Test failure rate
- Merge-to-deploy time
- Rollback / hotfix 비율

### 5. AI 비용을 연결합니다

AI 사용량은 성과가 아니라 비용과 설명 변수로 봅니다.

- Token cost per successful change
- Token cost per successful deployment
- AI-assisted PR의 Change Fail Rate
- AI-assisted PR의 review time
- AI-assisted PR의 rework rate

이렇게 봐야 “AI를 많이 썼다”가 아니라 “AI 사용이 실제 전달 성과로 이어졌다”고 말할 수 있습니다.

---

## AI-assisted PR 비교는 조심해야 합니다

가능하다면 AI-assisted PR과 일반 PR을 비교하는 것이 좋습니다. 하지만 단순히 PR에 `ai-assisted: true`를 붙이는 방식은 한계가 있습니다.

- 강제 태깅은 회피 동기를 만들 수 있습니다.
- 자율 태깅은 누락이 많을 수 있습니다.
- 개발자마다 “AI 도움”의 기준이 다를 수 있습니다.
- AI 사용 여부가 개인 평가와 연결되면 데이터가 왜곡될 수 있습니다.

따라서 PR label, AI 도구 사용 로그, IDE 또는 CLI 사용 이벤트, 개발자 설문, 코드 리뷰 샘플링 같은 여러 신호를 함께 보는 편이 좋습니다.

또한 AI-assisted PR과 일반 PR을 비교할 때는 selection bias를 조심해야 합니다. 개발자가 쉬운 작업에 AI를 더 자주 쓴다면 AI-assisted PR이 더 빨라 보일 수 있습니다. 반대로 복잡한 작업에서만 AI를 호출한다면 AI-assisted PR이 더 느려 보일 수도 있습니다.

그래서 비교할 때는 작업 유형, PR 크기, 변경 파일 수, 담당 서비스, 개발자 숙련도, 릴리스 기간 같은 맥락을 함께 봐야 합니다.

---

## Goodhart’s Law: 지표가 목표가 되면 망가집니다

DORA 메트릭을 도입할 때 가장 조심해야 할 점이 있습니다.

**지표를 목표로 만들면 안 됩니다.**

- Deployment Frequency를 목표로 만들면 의미 없는 작은 배포를 늘릴 수 있습니다.
- Lead Time만 압박하면 리뷰와 테스트를 건너뛸 수 있습니다.
- Change Fail Rate만 낮추려 하면 위험하지만 필요한 변경을 회피할 수 있습니다.
- AI 사용량을 목표로 만들면 필요 없는 토큰 사용과 형식적인 AI 사용이 늘 수 있습니다.

그래서 DORA와 AI 사용량은 개인 평가에 쓰면 안 됩니다.

Lead Time이 긴 개발자를 찾기 위한 도구가 아닙니다. PR 수가 적은 개발자를 압박하기 위한 도구도 아닙니다. AI 사용량이 적은 사람을 줄 세우기 위한 도구도 아닙니다.

이 지표들은 시스템 개선을 위한 도구입니다.

Lead Time이 길다면 “누가 느린가?”가 아니라 “어느 구간에서 대기가 발생하는가?”를 물어야 합니다.

리뷰가 병목이면 리뷰어 풀을 늘리거나 PR 크기를 줄여야 합니다. 테스트가 병목이면 테스트 자동화와 병렬화를 개선해야 합니다. 배포가 병목이면 릴리스 정책, 승인 단계, feature flag, rollback 전략을 손봐야 합니다.

---

## 마무리: LLM의 ROI는 사용량이 아니라 전달 성과로 봐야 합니다

LLM은 개발자의 손을 빠르게 만들 수 있습니다. 하지만 고객은 개발자의 손이 얼마나 빨라졌는지가 아니라 제품이 얼마나 빠르고 안정적으로 개선되는지를 경험합니다.

그래서 LLM 도입 효과를 말하려면 다음 질문에 답해야 합니다.

- PR이 늘었나요? 좋습니다. 그 PR은 더 빨리 리뷰되고 있나요?
- 코드 작성 시간이 줄었나요? 좋습니다. 그 변경은 더 빨리 프로덕션에 도달하나요?
- AI 사용량이 늘었나요? 좋습니다. 그 사용량은 Lead Time 단축, 배포 빈도 증가, 실패율 감소로 이어졌나요?
- 토큰을 많이 썼나요? 좋을 수도 있습니다. 하지만 그 토큰은 실제 고객 가치로 전환되었나요?

LLM 도입을 반대하자는 이야기가 아닙니다. 오히려 제대로 쓰자는 이야기입니다.

LLM을 도입합시다.  
하지만 측정해야 합니다.

토큰 사용량이 아니라 전달 성과를 봐야 합니다.  
PR 수가 아니라 Change Lead Time을 봐야 합니다.  
코드 생성량이 아니라 Change Fail Rate와 Deployment Rework Rate를 봐야 합니다.  
개발자의 체감 속도와 함께 고객에게 도달하는 속도를 봐야 합니다.

**LLM의 ROI는 “많이 썼다”로 증명되지 않습니다.**

**더 빠르고, 더 안정적으로, 더 적은 재작업으로, 더 지속 가능한 방식으로 고객에게 변화를 전달했을 때 증명됩니다.**

그걸 확인하기 위한 출발점은 DORA 메트릭입니다.

하지만 거기서 멈추면 안 됩니다.

LLM 시대의 개발 생산성 측정은 **DORA를 중심에 두되, DORA만으로 환원하지 않는 균형 잡힌 측정 체계**가 되어야 합니다.

---

## 참고자료

- Microsoft Research, *The Impact of AI on Developer Productivity: Evidence from GitHub Copilot*  
  https://www.microsoft.com/en-us/research/publication/the-impact-of-ai-on-developer-productivity-evidence-from-github-copilot/
- METR, *Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity*  
  https://arxiv.org/abs/2507.09089
- METR, *We are Changing our Developer Productivity Experiment Design*  
  https://metr.org/blog/2026-02-24-uplift-update/
- DORA, *DORA’s software delivery performance metrics*  
  https://dora.dev/guides/dora-metrics/
- DORA, *A history of DORA’s software delivery metrics*  
  https://dora.dev/insights/dora-metrics-history/
- DORA, *Balancing AI tensions: Moving from AI adoption to effective SDLC use*  
  https://dora.dev/insights/balancing-ai-tensions/
- Google Blog, *How are developers using AI? Inside our 2025 DORA report*  
  https://blog.google/innovation-and-ai/technology/developers-tools/dora-report-2025/
- Faros AI, *The AI Productivity Paradox Report 2025*  
  https://www.faros.ai/blog/ai-software-engineering
- ACM Queue, *The SPACE of Developer Productivity*  
  https://queue.acm.org/detail.cfm?id=3454124
- ACM Queue, *DevEx: What Actually Drives Productivity*  
  https://queue.acm.org/detail.cfm?id=3595878
