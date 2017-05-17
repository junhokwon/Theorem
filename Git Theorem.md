#zsh shell

: zsh쉘은 현재도 활발하게 개발되는 강력한 쉘

!!!

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

* 터미널 파일 복사하기

![](/Users/mac/projects/images/스크린샷 2017-05-15 오후 8.47.06.png)

현재 디렉토리 : 
cp 복사하고자하는 파일의 주소 현재위치(.)

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

: 기존 VCS는 각 파일에 대한 변화를 저장한다. 각 단계 verion마다 변경사항만 지정하고
마지막 version일때 원본파일을 불러내어 version과 같이 저장한다.

(2) git

![](https://git-scm.com/book/en/v2/images/snapshots.png)

: 시간순으로 프로젝트의 스냅샷을 저장.
: 다른 VCS와 달리 파일이 달라지지 않았으면, git는 성능을 위해서 파일을 새로 저장하지 않는다. 스냅샷은 각 version에 변경사항과 파일을 다 복사한다.git

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

: **마지막 커밋이후 아무것도 수정하지 않은 상태에서 어떤 파일을 수정하면 그파일은 modified 상태로 인식한다.** - staged area에서 commit하면 unmodified상태로 넘어간다.

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

````
$ echo 'My Project' > README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    README

nothing added to commit but untracked files present (use "git add" to track)

````

: README파일은 untracked 부분에 속해 있고(관리대상이아님)을 의미, **tracked 상태가 되기 전까지는 git은 절대 그 파일을 커밋하지 않는다.

* 파일을 새로 추적하기(git add 파일명,tracked로 바꾸기)
: git add명령으로 파일을 새로 추적할 수 있다.

````
$ git add README
````
: git status명령을 다시 실행하면 tracked상태이면서 커밋에 추가될 staged상태라는것을 의미 , 파일은 staging area에 저장되어 있다. changes to be committed는 즉 staged 상태라는것을 의미 한다.
커밋하면 git add를 실행한 시점의 파일이 커밋되어
저장소 히스토리에 남는다. git add 파일명을 통해 디렉토리에 있는 파일을 추적하고 관리하도록 한다.

#modified 상태의 파일을 stage하기
: 이미 tracked 상태인 파일을 수정하는 법은 파일을 수정한후 git status 명령을 실행하면 changes not staged for commit라는 상태가 되는데, 이것은 수정한 파일이 tracked상태이지만 아직 staged상태가 아니라는것이다.
다시 git add명령을 통해 staged상태로 만들어주어야 한다.

* 파일 상태를 확인하기

````
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
````

(1) 아직 추적하지 않은 새파일(untracked): ??'

(2) staged상태로 추가한 파일 : 'A

(3) 수정한파일 : M

# 파일 무시하기

: 어떤 파일은 git이 관리할 필요가 없다. 그럴경우
.gitignore 파일을 만들고 그안에 무시할 파일 패턴을 적는다.

````
$ cat .gitignore //명령어
''''
''''
*.[oa]
*~ //내용
````

: 내용에서 첫번째 라인은 확장자가 ".o"나 ".a"인 파일을 git가 무시하라는 의미

* .gitignore 파일의 예

````
# 확장자가 .a인 파일 무시
*.a

# 윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
!lib.a

# 현재 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않음
/TODO

# build/ 디렉토리에 있는 모든 파일은 무시
build/

# doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
doc/*.txt

# doc 디렉토리 아래의 모든 .pdf 파일을 무시
doc/**/*.pdf
```

#staged와 unstaged 상태의 변경내용을 보기

: 단순히 파일이 변경된 사실이 아닌 어떤 내용이 변경되었는지 살펴보려면 git diff 명령을 사용해야한다.

(1) git diff명령 : 워킹 디렉토리와 staging area에 있는것을 비교한다. 그래서 수정하고 아직 stage하지 않은것을 보여준다.(unstaged인 상태인 것들만 보여준다)

(2) git diff --staged : staging area에 넣은 파일의 변경 부분을 보고 싶을경우 

#변경사항 커밋하기

````
$ git commit
````

* 메세지를 인라인으로 첨부하는 법

````
$ git commit -m "Story 182: Fix benchmarks for speed"
````

: git commit -m"컨텐츠 내용"

````
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
````

(1) **master브랜치에 커밋했다.**
(2) 체크섬은 463dc4f이다.


#staging area 생략하기

: staging area는 커밋할 파일을 정리한다는 점에서
매우 유용하지만 필요하지 않을 때도 있다. 

* git commit -a : git은 tracked 상태의 파일을 자동으로 staging area에 넣는다.



#파일 삭제하기


(1) git에서 파일을 제거하려면 git rm명령으로 tracked상태의 파일을 삭제한후에(staging area)에서 삭제커밋해야한다.이 명령은 워킹 디렉토리에 있는 파일도 삭제하기때문에 실제로 파일도 지워진다.

(2) staging area에서만 제거하고 워킹 디렉토리에 있는 파일은 지우지 않고 남겨둘 수 있다.(하드디스크에 있는 파일은 그대로 두고, git만 추적하지 않게 한다)

```
$ git rm --cached README
$ git rm log/\*.log

```
: 이 명령은 log안에 있는 모든 .log파일을 삭제한다.
'\' 역슬래쉬는 파일명 확장기능이다.


```
$ git rm \*~
```


: 이 명령은 ~로 끝나는 파일을 모두 삭제한다.

# 파일 이름 변경하기

:git은 파일이름이 변경되었다는 별도의 정보를 저장하지 않는다.

* git 이름변경하기전파일 이름변경한후파일

```
$ git mv file_from file_to


$ mv README.md README
$ git rm README.md
$ git add README

```

: 위내용을 축약한것이 mv명령어이다. 파일이름명을 바꾸고 기존의 파일명의 파일을 지우고, 다시 staged상태로 파일을 만든다.

#커밋 히스토리 조회하기

: 새로 저장소를 만들어서 몇번 커밋을 했을수도 있고, 커밋히스토리가 있는 저장소를
clone했을수도 있다. 저장소의 히스토리를 보고싶을때 

# git log 명령어

```

$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
    
```

: git log 명령을 실행하면 저장소의 커밋 히스토리를 시간순으로 보여준다.
가장 최근의 커밋이 먼저나오고 , 각 커밋의 sha-1체크섬,저자이름,저자이메일,커밋한날짜, 커밋메세지가 나온다.

* 동료가 무엇을 커밋했는지 알려면  git log -p 명령을 할 경우 최근의 커밋한 두개의 파일만 보여준다.

* git --stat 

: --stat 옵션은 어떤 파일이 수정됐는지, 얼마나 많은 파일이 변경되었는지, 또 얼마나 많은 라인을 삭제했는지 요약정보를 다 보여준다.

```
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
```

: --pretty옵션을 뒤에 oneline을 쓰면 한줄로 커밋된 결과물만 보여준다.









#리모트 저장소

![](https://git-scm.com/book/en/v2/images/areas.png)

(1).git 디렉토리 : github홈페이지에서 repository를 만든다. 저장소의 링크를 가져온다.

(2) git remote add origin(로컬저장소이름) url을 사용하여 git저장소 링크를 가진 origin이라는 이름의 working 디렉토리를 만든다.

```
git remote add origin https://github.com/junhokwon/remote-project.git

```

(3) git remote -v 를 사용하여 워킹디렉토리에 잘 만들어졌는지 확인한다.

(4) git add와 commit를 한후 git.디렉토리에 파일들을 올린다.

(5) 커밋한 내용을 다시 새로운 로컬에서 클론을 통해 복사한다.

```
git clone url 새로운폴더명

git clone https://github.com/junhokwon/remote-project.git remote-project-clone

```


* git tag는 특정시점을 저장할때 사용
* git show 버전 : git 태그내용볼때
* git push origin 버전 : 태그를 커밋할경우
```
github release를 보면 있다
![](/Users/mac/projects/images/스크린샷 2017-05-16 오후 12.22.32.png)



* 브랜치 : 현재상태에서 사본을 뜨는 형식, 사본을 떠서 사용하다가, 갈래를 나눠서 나중에 합치거나 갈래끼리 합치는경우 각 커밋을 가르키는 포인터

* origin master : origin이라는 로컬에 master이라는 브랜치를 옮긴다.

* git은 데이터를 각 커밋 상황상황(스냅샷)으로 기록한다.

* 객체 : 데이터를 가진다
* 포인터 객체 : 데이터를 가르키는 것(내용없음)

* 메타데이터 : 프로젝트 설명
* 커밋 : 하나의 스냅샷
* 트리개체 : 디렉토리 개체 > 블록개체


* git 경로 확인

```
git log --decorate

```
* git브랜치 만들기

: git branch 브랜치이름

* git브랜치 바꾸기

```
git checkout testing

```
![](/Users/mac/projects/images/스크린샷 2017-05-16 오후 12.36.03.png)

* git브랜치 이름 바꾸기

: git branch -m master produciton

: git branch -m 기존브랜치이름 새로운브랜치이름

* git 브랜치 삭제하기

```
git branch -d 브랜치명

```

* 커밋한 로컬저장소에 있는 내용 되돌리기

```
git checkout -- <파일 이름>
```

: 변경전상태로 되돌려준다.

* tracked 된 파일이면 스냅샷(커밋)에 계속 남아있다.

* release: 버전
* develop : 커밋,수정,이슈
: 문제가생길경우 브랜치를 바꿔서 만들기 merge로 원래 브랜치에 합병




