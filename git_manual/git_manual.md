### Git

- 필요한 이유

  - 싱글 플레이: 코드 관리/백업 용으로 나만 알아볼 수 있는 커밋 메세지

  - 멀티 플레이
    - 협업 workflow
    - 서비스/제품 이력관리
    - 커밋 규칙과 정확한 메시지 (convention, atomic한 단위로 구성 -> cherypick 가능)
    - 코드 리뷰

- Git 구조
  - Local - working directory, staging area, local repositiory
  - Remote - remote repository
- Git Command: 구글에 cheat sheet로 검색하면 출력 가능한 것 존재. 시트 없이도 사용할 수 있도록 익히기.

### Git Branch

- GIt Branch
  - 분기되는 흐름을 만드는 용으로 사용. 특정 커밋을 포인팅.
  - 병합하고자 하는 branch를 작업 branch에 병합, 본류에 합류시킴.
- GIt Source Tree
  - 문제 발생 시 빠르게 해결 가능
  - 트리 상 커밋 중 과거 시점의 상태로 전환 또는 메세지 변경 가능
    - push된 커밋일 경우 팀원과 충돌 주의

### Git Flow

- git 협업의 모범사례, branch 전략
  - 기본 브랜치 이외에는 필요에 따라 생성 및 병합.
  - jenkins에서 브랜치별 자동 배포 가능.
  - 보통 팀 내에서 다같이 정해서 사용, 조금씩 수정.
- branch
  - feature
  - **develop**: 개발에 사용. 기본 브랜치.
    - release에 bug fix된 내용, master에 hot fix된 내용도 가져와서 반영 필요.
  - bugfix: release에서 따서 수정
  - release: QA, 기획자 등 test, 품질 보증 활동, 배포 전 테스트 등.
  - hotfix
  - **master**: 출시에 사용. 기본 브랜치.

- command
  - `git flow init` -> 프로덕션 릴리즈용, 다음 출시용 branch, prefix, version 등 설정 가능
  - develop에서 `git flow feature start sign_up` -> develop을 기준으로 파생, 생성, 스위칭
    - 기능 개발 종료 후 `finish` 사용
      - 2학기에는 사용 X. 자동으로 develop에 merge하므로, gitlab에서 Merge Request 진행 필요. 코드 리뷰 후 merge, 배포
      - git flow 명령보다 직접 branch 생성, 작업하고, 코드 리뷰와 merge는 gitlab에서

- Trunk based: git branch 전략의 다른 모델
  - master에서 동시 개발. 짧은 주기로 feature 브랜치를 생성해서 master 브랜치를 fresh하게 유지.
  - release: 배포 버전 관리.
  - 코드 리뷰 없이 시니어로 구성된 팀에서 시도해볼만함. 빠른 제품 출시 가능.
- 권장 Git Flow
  - Feature
  - Develop: QA 활동까지 여기서 진행. jenkins 붙여서 자동화하기.
  - Hotfix
  - Master: Sprint 끝날 때마다 버전 태깅, 출시

### Git commit 사전 작업

- Merge 시 gitlab의 Main settings 정보를 사용. 확인하기.
  - 로컬 설정과 맞출 필요: 동일인 작업 내용이 나눠지지 않도록.
    - 개인별 기여 통계, 커밋 로그 확인 시에도 편리.
    - 원격 저장소 인증 정보와 커밋용 사용자 정보는 별개.
  - 전역 설정 변경: `git config --global user.name "김싸피"`와 같이, 모든 프로젝트에 적용
  - 프로젝트별 설정: `git config user.name "김싸피"` 를 develop 브랜치에서 하는 식
- Commit Message
  - 일자 표시: 불필요.
  - 불친절한 메세지는 내용 유추 불가; Convention 맞춰서 commit하기

- `.gitignore` 관리
  - 형상 관리 시 올라가면 안 될 파일들
    - .class, node_modules... 무거운 라이브러리. 코드가 어지럽혀짐.
    - 로그, 백업 파일 등

### 연습 및 추가 학습

- Git 명령어 연습: https://learngitbranching.js.org/
  - 미션 해결해보기. 브라우저에 진행 기록 남음. commit graph 함께 확인 가능.
- 추가 학습 자료
  - git with d3: 명령어별 graph 확인 가능. https://onlywei.github.io/explain-git-with-d3/
  - pro git: 레퍼런스 문서 풀버전. https://git-scm.com/book/ko/v2
- 정리
  - 개발에 방해되지 않을 정도까지는 git 기본 개념/명령어 숙지
  - git branch를 사용한 코드 관리의 흐름 이해
  - 회사/팀별 상황에 맞는 git branch 전략 선택
  - 팀별 합의된 정책에 맞춰 일관된 git 사용 노력 필요
  - 궁금하거나 자주 마주하는 케이스에 대한 git 사용법 꼭 연습하기