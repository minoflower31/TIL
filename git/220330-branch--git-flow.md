## Branch
*분기점을 생성하여 독립적으로 코드를 변경할 수 있도록 도와주는 모델*
<br>
### Branch 흐름
- 처음 파일 등록 시
    1. main branch에서 등록할 파일 add, commit
    2. 수정 작업할 branch로 이동
    3. 해당 branch에서 작업 후 add, commit
    4. main으로 돌아온 후, merge하기

<br>
<br>
<br>
<br>
## Git Flow

*(hotfix) - master - (release) - develop - feature*
<br>
참고 사이트: https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html
<br>
<br>
### Git flow 흐름

###  alone
1. `$ git flow init`
2. `$ git flow feature start {featurename}`
3. 작업 시행
4. 작업이 끝나면 **add, commit** 진행(각 작업마다)
5. `$ git flow feature finish {featurename}`
6. `$ git flow release start v0.0.1`
    - 많은 작업이 일어남(사내 알고리즘 감추기)
7. `$ git flow release finish v0.0.1`
    - release -> main 으로 merge. (tag가 달림 - 제목 및 내용 작성)
    - release -> develop 으로 merge.
8. remote repo 에서는 main밖에 존재하지 않음.
    - `$ git push -u origin develop` (develop branch)
    - `$ git push origin main` (main branch)
    - `$ git push --tag` //특정한 상황을 알리고 싶을 때
9. main과 develop에 feature에서 작업했던 파일들이 기재됨.

c.f. github 내 **network**에서 git flow 라이프 사이클 확인 가능
<br>
<br>
<br>

### together

**section1: Init**

- *팀장* 
    1. remote repo를 clone.
    2. git flow 시작 (**alone** 참조)
    3. 작업할 파일 생성 후, add commit (현재 branch: develop)
    4. remote repo에 develop branch가 없기에
         - `$ git push -u origin develop`

- *팀원*
    1. 팀장의 remote repo 사본을 뜨기 위해 **Fork** 클릭
    2. `$ git clone {주소}`
    3. 하지만 main branch만 복제돼있기 때문에 git flow를 생성해서 develop branch 가져오기
         - 놀라지 말라,,, 팀장의 develop branch == 팀원의 develop branch
         - 이제 feature start 후 작업하기!
    4. 작업이 끝나면 add, commit하기 -> feature finish까지
    5. `$ git push -u origin develop` 

<br>
<br>

**section2: Confirm**
1. **팀원**- github에서 **Compare & pull request** 클릭
    1. 팀장 repo로 이동
    2. [Issues 탭]
         - 무언가를 개발하겠다, 버그 오류가 있다, 제안하겠다 등등 기재
         - 체크박스로 todo 만들어서 작업 진행
    3. [Code 탭]
         - branch가 **develop**인지 꼭 확인!! (탕비실 주의)
         - 여기서 Issue 해결을 알림. 내용에 #을 작성하면 등록된 Issue 선택 가능
<br>

2. **팀장**
    1. [Issues 탭]
         - **Assignees**: 팀원 추가
         - **Labels**: 상황에 맞게
    2. [Pull reqeusts 탭]
         - **Reviewers**: 팀장 본인(확인하는 사람) 추가
         - **Assignees**: 팀장, 팀원 추가
         - **Labels**: 상황에 맞게
    3. 팀원이 올린 #태그 확인 후, **Files changed**에서 변경사항 확인
         - 마음에 안들면 **+ 버튼**으로 comment 남기기
         - 우측에 **Finish your review** 클릭 후 마무리 comment 남기기 
         - c.f. Comment: 확인용, Approve: 승인, Request changes: merge 하기 전 피드백(빠꾸먹음)
<br>
3. **팀원**
    1. [Pull requests 탭]
         - 팀장이 게시한 변경사항 확인(댓글로 커뮤니케이션할 것)
         - **!!!pull requests가 Open 상태면 팀원 branch에서 push할 경우 코드가 바로 반영되기 때문에 팀장은 이를 닫아줘야 한다.**
         - 팀원이 **git push** 진행 시, pull requestst 아래에 commit한 내용 바로 반영
    2. 문제 수마다 add commit 진행할 것
    3. `$ git push origin develop`
    4. 해결 완료했으므로 Resolve 처리 해주기
<br>
4. **팀장**
    1. [Pull requests 탭]
         - 변경사항 commit 확인
    2. [Files changed 탭]
         - 코드 확인 후, 우측에 **Viewed** 체크
         - **Review changes** 버튼 클릭해서 comment 작성 후 Approve.
    3. [Pull requests 탭] 이동
         - !!! **Merge pull request**는 반드시 사수, 팀장급만 클릭
         - **Confirm merge** 버튼 클릭
    4. 팀장의 develop branch에서 파일을 확인하면 변경이 완료됨을 알 수 있음.
    5. 받아오려면 `$ git pull origin develop`
<br>
5. **나머지 팀원**
    1. 코드가 변경됐으므로 `$git remote add upstream {팀장repo git 주소}
    2. `$ git fetch upstream develop`
         - **fetch** 명령어는 **FETCH_HEAD** 라는 branch에 담음
         - (본래 fetch의 목적은 필요한 부분만 가져오는 것)
    3. `$ git merge FETCH_HEAD`
    위와 같은 방식으로 최신화 작업 가능

<br><br>
**다음 작업 시에는 feature start-end** <br>
**모든 기능이 쌓이면 팀장은 release 작업 실시**
