---
title: "레슨 8 - 기능 PR 만들기 및 병합"
description: "필터링, 지침, 스킬 업데이트, QA 프로필, 테스트를 함께 검토하고 PR을 만든 다음 Agent Merge를 사용합니다."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

필터링 구현, 지침 업데이트, 스킬 업데이트, 품질 보증(QA) 프로필, 테스트를 하나의 브랜치에 저장했습니다. 이제 모두 함께 검토하고 풀 리퀘스트를 엽니다. 별점 PR은 직접 병합했지만, 이번에는 **Agent Merge**가 병합 과정을 관리하도록 합니다.

> [!NOTE]
> 일반적으로 기능, 지침 업데이트, 스킬 업데이트, QA 에이전트는 여러 PR로 나눕니다. 워크숍을 간소화하기 위해 필터링과 품질 작업 전체를 하나의 세션과 브랜치에서 진행하고 이 PR에 포함했습니다.

이 레슨에서는 다음을 수행합니다.

- Agent Merge가 무엇이며 병합 수명 주기를 어떻게 자동화하는지 알아봅니다.
- 전체 기능 PR과 검증 근거를 살펴봅니다.
- 검토를 마친 뒤에만 Agent Merge에 권한을 부여하고 PR이 병합되었는지 확인합니다.

## 시나리오

필터링 작업 전반에서 Copilot을 사용해 기능을 계획하고 구현하고 검증했습니다. 이제 Tailspin Toys는 병합 권한을 개발자가 계속 제어하면서 남은 PR 작업을 자동화하려고 합니다.

## Agent Merge 소개

**Agent Merge**는 GitHub Copilot app에서 풀 리퀘스트를 병합하는 데 필요한 나머지 작업을 자동화합니다. 이 기능을 사용하도록 설정하면 앱의 세션이 PR을 확인하고, 실패한 지속적 통합(CI) 검사 수정, 검토 의견 대응, 필요한 경우 리베이스 수행 등 병합을 막는 문제를 해결합니다. 그런 다음 GitHub에서 허용하는 즉시 병합합니다. Agent Merge는 백그라운드에서 실행되고 앱을 다시 시작해도 계속 작동하며 PR이 병합되면 자동으로 꺼집니다.

지금까지는 직접 **Merge pull request**를 선택했습니다. Agent Merge가 이 작업을 맡을 수 있지만, 코드를 수정하고 병합하려면 여전히 명시적인 권한 부여가 필요합니다. 병합 권한을 주기 전에 허용되는 작업과 변경 사항을 검토합니다.

## Agent Merge로 PR 관리

코드를 모두 작성하고 검토했으므로 Agent Merge가 PR 절차를 관리하도록 합니다.

1. 에이전트 선택기에서 **Default agent**를 선택합니다.
2. **Create PR** 옆의 드롭다운을 선택합니다.
3. **Agent Merge**를 선택합니다. 버튼 레이블이 **Agent Merge**로 바뀝니다.
4. **Agent Merge** 버튼을 선택해 절차를 시작합니다. 드롭다운에서 옵션을 선택하는 것만으로는 시작되지 않습니다.

Agent Merge가 다음 작업을 수행합니다.

- 제목과 설명을 포함해 풀 리퀘스트를 만듭니다.
- 이슈에서 세션을 시작했다면 설명 본문에 관련 이슈를 연결합니다.
- 대상 브랜치에 리베이스하거나 잠재적 병합 충돌을 처리합니다.
- 모든 검사가 통과하는지 CI 프로세스를 모니터링합니다.
- 다른 개발자나 Copilot 코드 검토에서 피드백이 있는지 PR을 모니터링하고, 의견을 해결하도록 변경합니다.
- 모든 조건이 충족되면 PR을 자동으로 병합하도록 설정할 수 있습니다.

모든 조건이 충족된 뒤 Agent Merge가 PR도 병합하도록 설정합니다.

5. 병합 권한을 주기 전에 PR diff와 검증 근거를 검토합니다.
6. **Agent Merge** 옆의 드롭다운을 선택합니다.
7. **Merge pull request** 옆에 체크 표시가 있는지 확인합니다.
8. 다음과 같이 요청해 에이전트에 명시적으로 병합을 지시합니다.

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 진행 상황을 확인하고 보고된 차단 요인을 해결합니다. 다음 단계로 넘어가기 전에 PR 상태가 **Merged**인지 확인합니다. 병합 권한이 활성화되었거나 에이전트가 실행 중이라는 사실만으로는 병합이 완료된 것이 아닙니다.

> [!IMPORTANT]
> Agent Merge는 리포지토리 보호 규칙이나 권한 부족을 우회하지 않습니다. 계속하기 전에 이러한 차단 요인을 해결합니다.

## 요약 및 다음 단계

코드 생성, 테스트와 검증, 풀 리퀘스트 처리 등 개발 과정의 여러 단계를 자동화했습니다. 이 레슨에서는 다음을 수행했습니다.

- Agent Merge와 병합 수명 주기 자동화 방식을 알아봤습니다.
- 전체 기능 PR과 검증 근거를 살펴봤습니다.
- 검토를 마친 뒤에만 Agent Merge를 승인하고 PR 병합을 확인했습니다.

다음으로 에이전트와 작업을 살펴보고 계획하고 시각화하는 더 다양한 방법을 알아보기 위해 [기존 캔버스를 사용하고 이슈 분류 캔버스를 만듭니다][next-lesson].

## 리소스

- [GitHub Copilot app으로 이슈 및 풀 리퀘스트 관리][managing-issues-prs]
- [GitHub Copilot app 정보][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
