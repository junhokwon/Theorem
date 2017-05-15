#zsh shell

: zsh쉘은 현재도 활발하게 개발되는 강력한 쉘



* 설치방법

```
brew install zsh zsh-completions
curl -L http://install.ohmyz.sh | sh
```

* 확인법 : echo $SHELL
* 필수적으로 추가해야 할 부분

```
export PYENV_ROOT=/usr/local/var/pyenv
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

* vim설정 다보기 : vim ~/.zshrc
* 자주 사용하는 alias(명령어별칭) 지정하기
* 터미널 윈도우 추가 : cmd + n

```
alias <사용할 명령어>="<명령어 내용>"

# Pycharm 실행
alias py="open -a /Applications/PyCharm\ CE.app/Contents/MacOS/pycharm"
```
- 명령어를 지정하고자 하는 프로그램의 경로를 볼려면
- 터미널창에 프로그램을 끌면 주소가 나온다.



#github 정리


**단축키 종류**

* git 사용자 정보

````
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
````

* git 편집기 설정

````
$ git config --global core.editor zsh
````

* 설정확인

````
git config --list명령 실행
````
* 도움말

````
$ git help <verb>
$ git <verb> --help
````

* ls - al : 전체목록
* l - : 전체목록

* touch 파일명 : 파일만들기

* vim/vi 파일명 : vim파일내용 들어가기

* echo 'show me the money' > abc4.txt : abc4.txt파일의 내용을 쇼미더머니라고 적어서 파일을 만들기

* git : init: git하고 연결하기

* git상태보기 : git status

* git add 파일명 : untracked영역에서
staged영역으로 옮김

* git add --all : 모든파일을 staged영역으로 옮긴다.

* git commit : staged에서 unmodified영역으로 옮김

* git commit -m "README 파일 수정"

* git commit -m "내용" : commit된 파일에 내용을 넣는다.

* git reset HEAD abc2.txt : staged영역에서 untracked영역으로 돌린다.

* git diff --staged : staged영역에 있는 내용보기

* git diff : unmodified영역에 있는 내용보기

* git log : commit된 파일목록보기

-

* 버전관리란 ?

버전관리시스템(VCS)는 파일 변화를 시간에 따라 기록했다가 나중에 특별한 시점의 버전을 다시 꺼내올 수 있는 시스템이다. 소프트웨어 소스코드 뿐만 아니라 거의 모든 컴퓨터 파일의 버전을 관리할 수 있다.
VCS를 이용하면, 각 파일을 이전 상태로 되돌릴 수 있고, 누가 언제 만들어낸 이슈인지도 알수 있다. 또한 파일을 잃어버리거나 잘못고쳤을 경우 쉽게 복구 할 수 있다.

(1) 로컬버전관리

![로컬버전관리](https://git-scm.com/book/en/v2/images/local.png)

(2) 중앙집중식 버전관리(CVCS)
: 프로젝트를 진행하면 다른개발자와 함께 작업하는 경우가 있다. 파일을 관리하는 서버가 별도로 있고
클라이언트가 중앙서버에서 파일을 받아서 사용한다.

* 치명적인 결점
(1) 중앙서버에 문제가 생긴다면 협업,백업불가
(2) 중앙데이터 베이스가 있는 하드디스크에 문제가 생길경우 모든 히스토리가 사라진다.

![중앙집중식 버전관리](https://git-scm.com/book/en/v2/images/centralized.png)

(3)분산 버전관리 시스템(DVCS)
DVCS에서의 클라이언트는 단순히 마지막 스냅샷을 사용하지 않고 **저장소를 전부 복제**한다. 서버에 문제가 생기면 이 복제물로 다시 작업을 시작 할 수 있다.

![분산관리시스템](https://git-scm.com/book/en/v2/images/distributed.png)


-
#git의 기초

: git는 다른 VCS과는 달리 데이터를 저장,취급 하지도 않고, 대신 데이터를 파일시스템 **스냅샷**으로 취급하기에 크기가 매우 적다.


* 스냅샷 : 데이터의 전체가 아니라 데이터에 대한 이미지를 복사해서 스냅샷 생성 시점의 데이터를 유지하는 방법

(1) 기존 VCS

![](https://git-scm.com/book/en/v2/images/deltas.png)

: 기존 VCS는 각 파일에 대한 변화를 저장한다.

(2) git

![](https://git-scm.com/book/en/v2/images/snapshots.png)

: 시간순으로 프로젝트의 스냅샷을 저장.
: 다른 VCS와 달리 파일이 달라지지 않았으면, git는 성능을 위해서 파일을 새로 저장하지 않는다.

(3) git의 장점

* 모든명령을 로컬에서 실행
: 거의 모든 명령이 로컬파일과 데이터만 사용하기 때문에
네트워크에 있는 다른 컴퓨터가 필요없다.
네트워크속도에 영향을 받는 CVCS(중앙버전관리시스템)보다 획기적이다. 

* 오프라인상태, vpn에 연결하지 못해도 커밋가능!

모든 히스토리가 로컬 디스크에 있기 때문에 모든 명령이 순식간에 실행된다.
로컬데이터에 서버가 복사해있기 때문에 서버없이 로컬데이터베이스에서 히스토리를 읽어서 보여준다. 


* git의 무결성

: git은 데이터를 저장하기 위해 항상 체크섬을 구하고
그 체크섬으로 데이터를 관리한다. 체크섬을 이해하는 git없이 어떠한 파일이나 디렉토리도 변경할수 있다.

* 체크섬 : 중복검사의 한 형태로 , 오류 정정을 통해, 공간이나 시간 속에서 송신된 자료의 무결성을 보호하는 단순한 방법이다. git에서 사용하는 가장 기본적인 데이터 단위이자 철학이다.

: git은 SHA-1 해시를 사용하여 체크섬을 만든다. 만든 체크섬은 40자 길이의 16진수 문자열이다.

````
24b9da6552252987aa493b52f8696cd6d3b00373
````

: git는 모든 것을 해시(암호화함수)로 식별한다. git는 파일을 이름으로 저장하지 않고, 해당파일의 해시로 저장한다.

* git은 데이터를 추가할뿐

: git로 무얼하든 git 데이터베이스에 추가되기 때문에 되돌리거나 데이터를 삭제할 방법이 없다. 일단 스냅샷을 커밋하고 나면 데이터를 잃어버리기 어렵다. 

* 세 가지 형태
: git은 파일을 세가지 형태로 나누는데
	* committed : 데이터가 로컬데이터베이스에 안전하게 저장되었다는 것을 의미한다.
	* modified : 수정한 파일을 아직 로컬데이터베이스에 커밋하지 않은것을 말한다.
	* staged란 현재 수정할 파일을 곧 커밋할것이라고 표시한 상태를 의미한다.

: 이 세가지 형태는 git 프로젝트의 세 가지 단계와 연결되어 있다. git디렉토리, 워킹디렉토리, staging area이렇게 세가지이다.

![](https://git-scm.com/book/en/v2/images/areas.png)

(1) git디렉토리 : git 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳을 의미,지금 작업하는 디스크

(2) 워킹디렉토리 : 프로젝트의 특정버전을 checkout한 것이다. git디렉토리에 압축된 데이터베이스에서 파일을 가져와서 워킹디렉토리를 만든다.

(3) staging Area : git 디렉토리에 있고 단순한 파일이고, 곧 커밋할 파일에 대한 정보를 저장한다.

![](https://git-scm.com/book/en/v2/images/areas.png)

* 순서

(1) 워킹 디렉토리에서 파일을 수정한다.

(2) git add 파일명을 통해 파일을 수정하고 staging area에 속하게 만든다. 이때 파일의 형태는  staged가 된다. 아직 git add 파일명을 통해 staging area로 넘어가지 않은 것은 working 디렉터리에 존재하며, 이때 파일의 상태는 modified다.


-

#git 저장소 만들기

(1)기존 디렉토리를 git저장소로 만들기
: 프로젝트의 디렉토리로 이동해서 아래와 같은 명령을 실행
````
$ git init
````

: 이 명령은 .git라는 하위 디렉토리를 만든다. .git 디렉토리에는 저장소에 필요한 뼈대 파일이 들어있다.

````
$ git add *(전체).(py,txt,md등등)
$ git add LICENSE
$ git commit -m 'initial project version'
````

: git이 파일을 관리하게 하려면 저장소에 파일을 추가하고 커밋해야한다. git add명령으로 파일을 추가하고 git commit 명령으로 커밋한다.

(2) 기존 저장소를 clone 하기

: 다른 프로젝트에 참여하려거나 git 저장소를 복사하고 싶을 때 git.clone명령을 사용한다. git clone을 실행하면 프로젝트 히스토리를 전부 받아온다. 

````
git clone url명령으로 저장소를 clone한다.
$ git clone https://github.com/libgit2/libgit2
````
: 이 명령은 libgit2이라는 디렉토리를 만들고 그안에 .git디렉토리를 만든다. 그리고 저장소의 데이터를 모두 가져와 자동으로 가장 최신버전을 checkout을 해놓는다. 

-

#수정하고 저장소에 저장하기
: 이제는 파일을 수정하고 파일의 스냅샷을 커밋하면 된다.**파일을 수정하다가 저장하고 싶으면 스냅샷을 커밋한다**.

: **워킹디렉토리의 모든파일은 크게 tracked(관리대상임)과 untracked(관리대상이 아님)으로 나눈다.** 
tracked파일은 이미 스냅샷에 포함돼 있던 파일이다.
즉(git add 파일명)을 사용하여 스냅샷에 파일을 포함시킨다. **tracked(관리대상임) 파일은 또 unmodified(수정하지않음)과 modified(수정함) 그리고 staged(커밋으로 저장소에 기록할)상태중 하나이다.** 그리고 나머지 파일은 untracked파일이다. untracked파일은 워킹 디렉토리에 있는 파일 중 스냅샷에도 staging area(커밋되기전 파일 저장소)에도 포함되지 않는 파일이다.

: 즉 처음 저장소를 clone하게 되면, 모든 파일은 tracked(관리대상임)이면서 unmodified(수정하지 않는 형태이다) - 파일을 checkout하고 나서 아무것도 수정하지 않았기 때문이다.

: **마지막 커밋이후 아무것도 수정하지 않은 상태에서 어떤 파일을 수정하면 그파일은 modified 상태로 인식한다.** - 마지막 커밋을 한경우는 각 단계로 리셋했기에 git디렉토리에서는 수정하지않음으로 의미

: 실제로 커밋을 하기 위해서는 이 수정한 파일을 staged로 만들고, staged 상태의 파일을 커밋해야한다.

* 파일 상태 사이클

![](https://git-scm.com/book/en/v2/images/lifecycle.png)

* 파일의 상태 : git status

````
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
````

: 위의 내용은 파일을 하나도 수정하지 않았다는 의미
즉 tracked이나 modified인 상태인 파일이 없다는 의미이다.








