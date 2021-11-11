# Jira

### 서울 1반 이진호

------

## Index

- Jira1
- Jira2Ticket DescriptionWork FlowDashboardBurndown Chart

------

## Jira1

#### (1) JQL

- 지라에서 사용하는 쿼리

#### (2) BULK

- 티켓 한 번에 업데이트

#### (3) Components

- 티켓의 해시태그
- 인덱스가 걸려서 해당 컬럼으로 통계 차트
- 라벨과의 차이점

#### (4) Watchers and mention

- 티켓 모니터링

#### (5) Releases

- 빌드의 버전 컨트롤
- 가장 중요

## Jira 2

#### (1) Ticket Description

#### (2) Work Flow

#### (3) Dashboard

#### (4) Burndown Chart

## Ticket Description

#### (1) Ticket Description

- 정해진 것 없음
- 최대한 상세히 쓰자
- 문서 Link 환영

#### (2) Description Template

- QA의 경우 템플릿이 있는 경우 존재

```
(QA 환경) 환경 설명 (상세 설명) 상세 설명 (참고) 참고 설명 (영상) 영상 링크 (기대 결과) 기대 결과 설명
```

## Work Flow

#### (1) Work Flow Relation

- Jira Configs
- Ticket이 생성되고 완료될 때까지의 상태 변화
- 프로젝트 진행 상황을 한눈에 파악 가능
- 불필요한 커뮤니케이션 감소

#### (2) Status

>  이슈의 ‘상태(Status)’를 통해 진행상황을 전체 구성원에게 공유

- **열기(Open)**: 스프린트 내 이슈를 인지한 단계를 의미합니다. 해당 상태에서 SM과 PO는 협의를 거쳐 진행할 필요가 없는 이슈는 취소(Cancel) 상태로 변경합니다.
- 할 일(To-do): 실제로 수행해야할 이슈로 인지한 단계를 의미합니다. 이슈(보통은 스토리 형태이나 작업일수도 있음)를 진행하기 위한 실질적인 준비상태입니다.
- **진행중(In Progress):** 수행해야할 이슈들이 실제로 개발 단계에 들어섰음을 의미합니다. 개발과 단위테스트를 진행하게되며 물리적으로 가장 오랜 시간을 차지합니다. **담당자가 인지**한 것을 의미
- **해결됨(Resolved):** 진행 중 상태였던 이슈를 **개발자(디자이너)가 1차 완료**했음을 의미합니다. 해당 단계에서 SM이 2차 단위테스트(확인)을 진행하고 수행이 마무리됐다고 판단하면 PO, SM, 개발자가 함께 참여해 이슈의 완료 상태를 점검하는 ‘리뷰’를 진행합니다. ‘리뷰’는 PO, SM, 개발자가 일정을 협의해 일정 장소에 모여 이슈 상태를 확인합니다.
- 완료됨(Done): 해결된 이슈를 ‘리뷰’를 통해 완벽하게 마무리됐음을 확인했됐음을 의미합니다. 개발팀 전체의 합의에 의해 부여되는 상태이므로 ‘리뷰’ 세션에서 PO, SM, 개발자가 함께하는 가운데 이슈 상태를 완료로 넘기는 것을 추천합니다.
- **닫힘(Closed):** 완료된 이슈나 취소된 이슈를 완전히 **종결된** 상태로 전환했음을 의미합니다. 불필요하다고 생각될 경우 프로젝트 구성원들의 협의에 의해 프로젝트에서 사용하는 상태 값에서 제외할 수 있습니다. **2차 종료로 Done**에 해당한다.
- **다시열기(Reopened):** 취소했던 이슈가 **재검토**를 통해 필요하다고 판단될 경우 다시 할 일 단계로 되돌리는 것을 의미합니다. 협의는 보통 SM과 PO가 하게됩니다.
- 빌드(Build): 완료된 이슈를 통해 개발된 물량(화면, 소스 등)을 배포 완료했음을 의미합니다. 마찬가지로 협의에 의해 제외하고 별도의 시스템(Jenkins 등)을 통해 관리하는 것도 가능합니다.
- 빌드 실패(Build Failure): 말그대로 빌드가 실패했을 의미합니다.
- 취소(Cancel): 특정 상태(열기 등)에서 이슈가 수행할 필요가 없다고 판단된 경우를 의미합니다. 보통 SM과 PO가 협의를 거쳐 취소 여부를 결정합니다.

#### (3) mention

- Resolved를 한 경우 모든 팀원들에게 반드시 mention
- Reopened

### 3. Dashboard

#### (1) Dashboard

- 본인의 업무를 편하게 핸들링하기 위해 사용
- Dashboard > Manage Dashboard > Create New Dashboard
- Filter JQL

#### (2) Dashboard

- Add gadget도표를 선택Saved Filter
- Edit Layout

#### (3) Filter (JQL)

- Issues > Search for Issues > Saved as
- 에픽을 제외한 해당 프로젝트들 내용을 Filter로 설정 후 Saved as

```
project = S05P31A501 and issuetype != epic
```

- assignee 현재 유저로 추가하고 Status가 done이 아닌 것들 Filter로 설정 후 Saved as

```
project = S05P31A501 and issuetype != epic and assignee = currentUser()and status != done
```

- assignee 현재 유저로 추가하고 status 7일간 업데이트 있는 경우Saved as

```
project = S05P31A501 and issuetype != epic and assignee = currentUser() and status changed DURING(startOfDay(-7), endOfDay(0))
```

### 4. Burndown Chart

- Reports > Burndown Chart