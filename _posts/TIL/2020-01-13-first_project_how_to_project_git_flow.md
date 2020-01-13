# Git Branch

* 분리된 작업 영역이며 사본이라고 생각하면 좋음(원본에 영향이 없음)
* 각각의 브랜치는 독립된 작업 영역
* 왜 필요할까?
  * 여러명이 하나의 프로젝트를 작업할 때, 하나의 코드로 작업할 수 없기 때문이다.
  * 각자 자신의 브랜치에서 서로에게 영향을 주지 않고 기능을 개발할 수 있다.
  * 새로운 것을 시도해 보고 싶을 때
* 브랜치는 항상 현재 작업 공간을 베이스로 만들어 진다.
* 작업 공간(브랜치)을 옮기는 방법
  * git checkout <브랜치 이름>
* 브랜치 생성 방법
  * git checkout -b <기능1> : 해당 브랜치를 생성 + 해당 브랜치로 이동이 합쳐진 명령어
* 원본을 베이스로 하는 다른 브랜치 생성 방법
  1. 베이스로 하고 싶은 브랜치로 작업 공간을 옮긴다.
     * git checkout <이동할 브랜치>
  2. 브랜치를 생성한다.
     * git checkout -b <기능2>

![2020-01-13-first_project_how_to_project_git_flow_1](/Users/joseongcheol/Desktop/josworks27.github.io/images/TIL/2020-01-13-first_project_how_to_project_git_flow_1.png)

# Project git workflow

![2020-01-13-first_project_how_to_project_git_flow_2](/Users/joseongcheol/Desktop/josworks27.github.io/images/TIL/2020-01-13-first_project_how_to_project_git_flow_2.png)

* fork 후 마스터 레포를 갖고 온다.
* git clone로 로컬로 가져온다.
* 마스터 레포와 로컬을 연결한다.
  1. pull 명령어로 dev 브랜치를 최신화한다. (항상)
  2. 최신화한 코들 새로운 브랜치 만든다.
  3. 코드를 작성한다.
  4. 코드를 다 작성하면 자신의 레포에 Push
     * 여기서 중요한 점은 자신의 레포의 마스터 브랜치에 push X!!!!!!
     * 자신의 레포에 새로운 브랜치를 만들어서 push
  5. 마스터 레포에 pull request 한다.
  6. 마스터 레포 관리자는 PR을 확인하여 merge해 주면 하나의 기능 개발이 완료!
  7. 이후 새로운 기능은 1~5를 반복 진행