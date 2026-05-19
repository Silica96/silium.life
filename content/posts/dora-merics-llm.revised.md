+++
title = 'LLM을 도입했더니 개발이 빨라졌다고요? 근거는요?'
date = 2026-05-19T12:26:17+09:00
draft = true
+++

## 빨라진 것 같다는 느낌

"LLM을 도입하고 개발 생산성이 좋아졌습니다."

요즘 개발 조직에서 자주 들리는 말입니다. GitHub Copilot, Cursor, Claude Code, ChatGPT 같은 도구가 일상에 들어오면서 편해진 작업도 많습니다. 코드 작성, 테스트 초안, 문서화, 리팩터링, 오류 분석 같은 것들이죠.

그 말 자체가 틀렸다는 건 아닙니다.

다만 여기서 바로 헷갈리기 시작합니다. 개발자가 "빨라진 것 같다"고 느끼는 것과 조직의 변경 사항이 더 빨리 고객에게 도달하는 것은 다른 문제거든요.

코드를 빨리 썼다고 배포가 빨라지는 건 아닙니다. PR이 늘었다고 제품 품질이 좋아지는 것도 아니고요. AI 도구 사용량이 늘었다는 사실만으로는 더더욱 부족합니다.

그래서 질문은 하나입니다.

LLM 도입 이후, 변경 사항이 더 빠르고 안정적으로 프로덕션에 나가고 있는가?

이 질문에 답하지 못하면 "생산성이 좋아졌다"기보다 "좋아진 것처럼 느낀다"에 가깝습니다.

이런 체감이 완전히 허상이라는 뜻은 아닙니다. 실제로 AI 코딩 도구가 특정 작업에서는 큰 속도 개선을 만든다는 연구가 있습니다. GitHub Copilot 실험에서는 JavaScript HTTP server 구현 과제를 줬을 때 Copilot 사용 그룹이 비사용 그룹보다 55.8% 빠르게 과제를 끝냈습니다.

반대 방향의 결과도 있습니다. METR이 2025년에 발표한 RCT에서는 숙련된 오픈소스 개발자들이 자신이 잘 아는 저장소의 작업을 수행했을 때, AI 사용 시 작업 시간이 오히려 19% 더 길어졌습니다. 더 흥미로운 건 실험 후에도 개발자들 스스로는 20% 빨라졌다고 느꼈다는 점입니다.

물론 이 결과 하나로 "AI는 개발자를 느리게 만든다"고 말하긴 어렵습니다. 표본은 숙련된 OSS 개발자 16명이었고, 대상도 자신이 잘 아는 대형 저장소였습니다. 특정 시점의 AI 도구라는 조건도 붙어 있죠. METR도 2026년 업데이트에서 최신 도구에서는 speedup 가능성이 더 커졌다고 설명했습니다.

여기서 가져갈 건 "AI가 빠르다"나 "AI가 느리다"가 아닙니다.

체감과 실제가 어긋날 수 있다는 것. 이 지점에서 측정 이야기가 시작됩니다.

---

## 코드 양 ≠ 전달 속도

소프트웨어 개발은 코드 작성으로 끝나지 않습니다.

하나의 변경 사항이 고객에게 도달하려면 요구사항을 이해하고, 설계하고, 구현합니다. 그 다음에는 코드 리뷰, 테스트, 빌드, 배포 승인, 릴리스, 모니터링, 장애 대응이 이어집니다. 코드는 그 흐름의 한 구간일 뿐입니다.

LLM이 구현 단계를 빠르게 만들어도 다음 단계가 준비되어 있지 않으면 병목은 옮겨갑니다. 코드가 빨리 나오면 리뷰 대기열이 길어집니다. PR이 쌓이면 테스트와 배포 파이프라인이 밀립니다. 생성된 코드가 많아지면 보안 검토와 운영 리스크의 면적도 넓어지고요.

Faros AI의 분석에서도 비슷한 현상이 보고되었습니다. AI 사용 팀에서 task completion과 PR merge는 증가했지만, PR review time도 크게 증가했고 조직 수준의 DORA 지표 개선은 뚜렷하지 않았습니다. 개인의 산출량은 늘었지만 병목이 코드 생성에서 리뷰와 검증으로 이동한 것입니다.

DORA 2025 보고서도 비슷한 쪽에 서 있습니다. AI를 "자동으로 생산성을 높여주는 도구"라기보다 조직의 강점과 약점을 함께 증폭하는 도구로 봅니다. 좋은 개발 시스템에서는 효과가 더 잘 드러나고, 리뷰·테스트·배포·운영 체계가 약한 조직에서는 기존 병목이 더 커지는 식입니다.

그러면 질문이 바뀝니다.

"얼마나 많이 만들었나?"보다 "얼마나 잘 전달됐나?"가 먼저입니다.

---

## DORA 다시 보기

DORA 메트릭은 오랫동안 4대 지표로 알려져 있었습니다.

| 전통적 DORA 4대 메트릭 | 질문 |
|---|---|
| Deployment Frequency | 얼마나 자주 프로덕션에 배포하나요? |
| Lead Time for Changes | 변경 사항이 프로덕션에 반영되기까지 얼마나 걸리나요? |
| Change Failure Rate | 배포 후 장애, 롤백, 핫픽스가 얼마나 발생하나요? |
| MTTR / Time to Restore Service | 문제가 생겼을 때 얼마나 빨리 복구하나요? |

최근 DORA 공식 가이드는 이를 5개 delivery metrics로 조금 더 쪼개서 봅니다.

Throughput 쪽에는 Change Lead Time, Deployment Frequency, Failed Deployment Recovery Time이 있습니다. Instability 쪽에는 Change Fail Rate와 Deployment Rework Rate가 있고요. 앞쪽은 얼마나 빨리, 자주 전달하고 복구하는지를 봅니다. 뒤쪽은 전달 과정에서 얼마나 깨지고 다시 손대야 했는지를 봅니다.

LLM이 정말 생산성을 높였다면 변경 사항은 더 빨리 프로덕션에 도달해야죠. 배포 빈도도 좋아질 겁니다. 동시에 장애, 롤백, 핫픽스, 재작업이 같이 튀면 곤란합니다.

"AI를 많이 썼다"가 근거가 되려면 이 숫자들이 같이 움직여야 합니다.

---

## DORA 밖에 있는 것들

DORA는 중요한 지표지만, 개발 생산성을 전부 설명하진 못합니다.

DORA가 답하는 질문은 주로 "변경 사항이 고객에게 얼마나 빠르고 안정적으로 전달되는가?"입니다. LLM 도입 효과에는 이 외에도 중요한 질문들이 있습니다.

예를 들면 이런 것들입니다.

- 반복 업무가 실제로 줄었나
- 신규 입사자의 온보딩이 빨라졌나
- 문서화 품질이 좋아졌나
- 번아웃이 줄었나
- 배포된 기능이 사용자 가치로 이어졌나
- 서비스 신뢰성이 유지되고 있나

이런 질문은 DORA 숫자에 잘 잡히지 않습니다.

그래서 SPACE나 DevEx 같은 프레임워크를 같이 보게 됩니다. SPACE는 개발자 생산성을 Satisfaction, Performance, Activity, Communication, Efficiency 다섯 관점에서 봅니다. DevEx는 개발자가 일을 더 잘할 수 있는 환경과 경험에 초점을 둡니다.

서비스 안정성은 SLO, latency, error rate, incident 쪽을 봅니다. 제품 가치는 activation, retention, feature adoption 쪽이 더 가깝습니다. AI 비용 효과라면 token cost per successful change 같은 지표를 따로 둘 수 있고요.

DORA는 중심에 둘 만합니다. 다만 전체 그림은 아닙니다.

---

## 토큰은 비용이지 성과가 아니다

LLM 도입 효과를 측정할 때 가장 자주 빠지는 함정은 AI 사용량을 성과로 착각하는 것입니다.

AI 도구 활성 사용자 수, AI-assisted PR 비율, 토큰 사용량, 토큰 비용, AI 호출 수, 제안 수락률. 이런 데이터는 모으기 쉽습니다. 그래서 더 위험합니다.

운영상 필요하긴 합니다. 하지만 성과 지표는 아닙니다.

토큰을 많이 썼다는 것이 곧 생산성이 좋아졌다는 뜻은 아닙니다. AI-assisted PR이 많다고 조직이 빨라진 것도 아니고, 제안 수락률이 높다고 그게 고객 가치로 이어졌다고 보기도 어렵습니다.

토큰 사용량은 기본적으로 비용입니다. AI 사용량은 설명 변수에 가깝고요.

토큰이 늘었다면 해석은 여러 갈래입니다. 정말 활용이 늘었을 수도 있습니다. 요구사항이 불명확해서 같은 질문을 반복 중일 수도 있습니다. 코드베이스 맥락 정리가 안 돼 있어서 매번 다시 설명하고 있을 수도 있고, 생성된 코드를 고치느라 비용이 새고 있을 수도 있습니다.

그래서 토큰은 결과 지표 옆에 놓아야 의미가 생깁니다.

토큰 사용량이 늘었다면 Change Lead Time은 줄었는가. AI 호출이 많은 PR의 review time과 rework는 어떤가. AI-assisted PR의 Change Fail Rate는 일반 PR과 다른가. 수락된 코드는 배포 후에도 문제 없이 유지되는가.

이런 식으로 붙이면 의미가 생깁니다. 토큰만 보면 그냥 비용 그래프입니다.

---

## verification tax

LLM은 코드 생성 속도를 높입니다. 하지만 속도에는 검증 비용이 붙습니다.

DORA는 이를 verification tax라는 용어로 설명합니다.

AI가 코드나 아이디어를 빠르게 만들어 줘도 개발자는 그 output을 검토합니다. 맞는지 확인하고, 맥락에 맞는지 보고, 환각이 끼어 있지는 않은지 확인합니다. 틀렸다면 다시 프롬프트를 쓰거나 직접 고칩니다.

특히 코드 리뷰에서 잘 드러납니다.

AI를 사용한 작성자는 큰 PR을 빠르게 만들 수 있습니다. 하지만 리뷰어는 그 PR을 여전히 사람이 읽습니다. 의도를 이해하고, 시스템 맥락에 맞는지 확인하고, 놓친 테스트가 없는지도 봅니다.

작성자의 속도 향상이 리뷰어의 인지 부하로 넘어가는 셈입니다.

여기서 핵심은 단순히 "코드 작성 시간이 줄었나?"가 아닙니다. 절약된 시간이 리뷰, 검증, 재작업, 장애 대응으로 다시 빠져나가고 있지는 않은가. 이쪽이 더 중요합니다.

---

## PR 지표는 보조 신호

LLM 도입 효과를 측정할 때 PR 수, PR cycle time, review time, test failure rate 같은 지표도 중요합니다.

하지만 이 지표들은 DORA와 경쟁하는 별도의 최종 성과 지표가 아닙니다. 대부분 DORA 메트릭을 계산하거나, 왜 좋아졌는지 또는 나빠졌는지를 설명하는 진단 지표입니다.

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

LLM 도입 후 전체 Lead Time이 줄었다면 좋은 신호입니다. 다만 어느 구간이 줄었는지가 중요합니다.

`commit → PR open` 구간이 줄었다면 코드 작성과 PR 준비가 빨라진 겁니다. 반대로 `PR open → first review`가 늘었다면 리뷰 대기열이 새로운 병목입니다.

`first review → approval`이 길어졌다면 AI가 만든 코드의 검증 비용이 커졌을 가능성이 있습니다. `merge → deploy`가 그대로라면 개발자 속도의 문제가 아닐지도 모릅니다. CI/CD 파이프라인, 릴리스 정책, 보안·컴플라이언스 승인, 배포 window 같은 운영 쪽 병목일 수 있죠.

그러니까 PR 수나 리뷰 시간은 최종 점수가 아닙니다. DORA 숫자를 설명하기 위한 보조 신호에 가깝습니다.

---

## 어디서부터 볼까

처음부터 완벽한 대시보드를 만들 필요는 없습니다. 오히려 처음부터 크게 만들면 정의 싸움만 하다가 끝나기 쉽습니다.

서비스 하나를 고르는 편이 낫습니다.

조직 전체 평균을 바로 내면 서비스별 맥락이 섞입니다. 어떤 서비스는 하루에도 여러 번 배포할 수 있고, 어떤 서비스는 보안이나 규제 때문에 배포 주기가 느릴 수밖에 없습니다. 한 숫자로 뭉개면 둘 다 안 보입니다.

먼저 현재 흐름을 그립니다.

commit이 어디서 시작되는지, PR은 언제 열리는지, 리뷰는 언제 시작되는지, merge 후 배포까지 얼마나 걸리는지. 배포 후 검증은 어디서 끝나는지도 같이 정리합니다.

그다음 baseline입니다. LLM 도구를 전사 도입하기 전이라면 몇 주에서 몇 달 정도 데이터를 확보해 둡니다. 이미 도입했다면 도입 이전 데이터를 가능한 범위에서 복원하거나, 팀별 AI 사용률 차이를 비교 군으로 활용합니다.

처음엔 DORA 5종을 먼저 봅니다. Change Lead Time, Deployment Frequency, Change Fail Rate, Failed Deployment Recovery Time, Deployment Rework Rate입니다.

그 숫자가 왜 그렇게 나왔는지는 진단 지표로 파고듭니다. PR cycle time, Time to first review, Review time, Test failure rate, Merge-to-deploy time, Rollback/hotfix 비율 같은 것들입니다.

마지막으로 AI 비용을 붙입니다. Token cost per successful change, Token cost per successful deployment, AI-assisted PR의 Change Fail Rate, review time, rework rate처럼 결과와 비용을 같이 놓습니다.

처음엔 이 정도면 충분합니다.

---

## AI-assisted PR 비교의 함정

가능하면 AI-assisted PR과 일반 PR을 비교하는 게 좋습니다. 다만 `ai-assisted: true` 라벨 하나로 해결되지는 않습니다.

강제 태깅은 회피 동기를 만듭니다. 자율 태깅은 누락이 생깁니다. 개발자마다 "AI 도움"의 기준도 다릅니다.

무엇보다 AI 사용 여부가 개인 평가와 연결되는 순간 데이터는 왜곡됩니다. 이건 거의 확실합니다.

그래서 한 가지 신호만 믿기 어렵습니다. PR label, 도구 사용 로그, IDE나 CLI 이벤트, 개발자 설문, 코드 리뷰 샘플링을 같이 놓고 봐야 그나마 덜 흔들립니다.

selection bias도 문제입니다.

쉬운 작업에 AI를 더 자주 쓰면 AI-assisted PR은 빨라 보입니다. 반대로 복잡한 작업에서만 AI를 호출하면 느려 보이겠죠. 작업 유형, PR 크기, 변경 파일 수, 담당 서비스, 개발자 숙련도, 릴리스 기간 같은 맥락을 빼면 비교가 금방 이상해집니다.

---

## 지표를 KPI로 만들면

DORA 메트릭을 도입할 때 가장 조심해야 할 점은 지표를 목표로 만드는 것입니다. Goodhart's Law 얘기입니다.

Deployment Frequency가 KPI가 되면 의미 없는 작은 배포가 늘어납니다. Lead Time만 압박하면 리뷰와 테스트를 건너뛰게 됩니다.

Change Fail Rate만 낮추려 들면 위험하지만 필요한 변경을 피하게 됩니다. AI 사용량을 목표로 만들면 형식적인 AI 호출과 불필요한 토큰 사용이 따라옵니다.

개인 평가로 가져가는 순간 더 위험해집니다.

Lead Time이 긴 개발자를 찾기 위한 도구가 아닙니다. PR 수가 적은 개발자를 압박하기 위한 도구도 아니고요. 이 지표들은 시스템을 개선하기 위한 도구입니다.

Lead Time이 길다면 "누가 느린가?"보다 "어느 구간에서 대기가 생기는가?"를 물어야죠.

리뷰가 병목이면 리뷰어 풀이나 PR 크기 정책을 손봅니다. 테스트가 병목이면 자동화와 병렬화를 봅니다. 배포가 병목이면 릴리스 정책, 승인 단계, feature flag, rollback 전략을 다시 봅니다.

사람을 줄 세우는 순간 지표는 망가집니다.

---

## 결국 근거

LLM은 개발자의 손을 빠르게 만들 수 있습니다. 하지만 고객이 경험하는 건 개발자의 손속도가 아닙니다. 제품이 얼마나 빠르고 안정적으로 개선되는지입니다.

그래서 LLM 도입 효과를 말하려면 "PR이 늘었다" 같은 숫자 하나로는 부족합니다. 그 PR이 더 빨리 리뷰됐는지, 변경이 더 빨리 프로덕션에 도달했는지까지 이어져야 합니다.

AI 사용량 증가가 Lead Time 단축과 실패율 감소로 이어졌는지도 확인 대상입니다. 토큰 비용이 실제 고객 가치로 전환됐는지도 봐야 하고요.

LLM 도입을 반대하자는 이야기가 아닙니다. 오히려 제대로 쓰자는 이야기입니다.

도입은 하되, 사용량이 아니라 전달 성과로 봅시다.

DORA는 그 출발점입니다. 거기서 멈추지 말고 SPACE/DevEx, 서비스 안정성, 제품 가치까지 같이 봅니다.

그래야 "빨라진 것 같다"를 넘어 "정말 나아졌다"고 말할 수 있습니다.

---

## 참고자료

- Microsoft Research, *The Impact of AI on Developer Productivity: Evidence from GitHub Copilot*  
  https://www.microsoft.com/en-us/research/publication/the-impact-of-ai-on-developer-productivity-evidence-from-github-copilot/
- METR, *Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity*  
  https://arxiv.org/abs/2507.09089
- METR, *We are Changing our Developer Productivity Experiment Design*  
  https://metr.org/blog/2026-02-24-uplift-update/
- DORA, *DORA's software delivery performance metrics*  
  https://dora.dev/guides/dora-metrics/
- DORA, *A history of DORA's software delivery metrics*  
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
