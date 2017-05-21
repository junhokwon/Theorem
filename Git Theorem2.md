7.3/7.5/7.6/7.7

#git 도구


: 어떤 프로젝트에서 한 부분을 담당하고 있을때, 다른 요청이 들어와서 잠시 브랜치를 변경할 일이 생겼을때, 
이런상황에서 완료하지 않은 파일을 커밋하는것이 껄끄럽기 때문에, 커밋하지 않고 다시 돌아와서 작업을 하고 싶을경우

* git stash명령으로 해결!

: stach 명령을 사용하면 워킹 디렉토리에 수정한 파일만을 저장.
**stach는 modified면서 tracked 상태인 파일과 staging area에 있는 파일을 보관해 두는 장소이다.**

```

$ git status
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   index.html

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   lib/simplegit.rb
    
 ```
 
 : git stash명령을 실행하면, 아래와 같은 결과
 
 ```
 
 $ git stash
Saved working directory and index state \
  "WIP on master: 049d078 added the index file"
HEAD is now at 049d078 added the index file
(To restore them type "git stash apply")

```

: 여기 작업중인 파일은 커밋할 게 아니라서 모두 stash한다.
여기서 git stash나 git stash save를 실행하면 스택에 새로운 stash가 만들어진다.

```
$ git status
# On branch master
nothing to commit, working directory clean

```
: 워킹 디렉토리가 깨끗해졌다. 이제 아무 브랜치나 쉽게 골라서 바꿀수 있다. 수정한것을 스택에 저장했고 아래와 같이 'git slash list'를 사용하여 저장한 stash를 확인한다.

```
$ git stash list
stash@{0}: WIP on master: 049d078 added the index file
stash@{1}: WIP on master: c264051 Revert "added file_size"
stash@{2}: WIP on master: 21d80a5 added number to log

```

: stash 두개는 원래 있었다. **그래서 현재 총 세개의 stash를 사용할 수 있다.** **'git stash apply'를 사용하여 stash를 다시 적용할 수 있다.**

```

$ git stash apply
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

 modified:   index.html
 modified:   lib/simplegit.rb

no changes added to commit (use "git add" and/or "git commit -a")

```

: `git stash apply`을 사용하여, git은 `stash`에 저장할 때 수정했던 파일들을 복원해준다. **하지만 , 꼭 깨끗한 워킹 디렉토리나 Stash 할 때와 같은 브랜치에 적용해야 하는 것은 아니다. 어떤 브랜치에서 Stash 하고 다른 브랜치로 옮기고서 거기에 Stash를 복원할 수 있다. 그리고 꼭 워킹 디렉토리가 깨끗한 상태일 필요도 없다. 워킹 디렉토리에 수정하고 커밋하지 않은 파일들이 있을 때도 Stash를 적용할 수 있다. 만약 충돌이 있으면 알려준다.**

: `git은 stash`를 적용할 때 staged 상태였던 파일을 자동으로 다시 staged 상태로 만들어주지 않는다.  `git stash apply` 명령을 실행할때 `--index` 옵션을 주어 staged상태까지 적용한다. 그래야 원래 작업하던 상태로 돌아갈 수 있다.

: `apply옵션`은 단순히 stash를 적용하는 것 뿐이고, stash는 여전히 스택에 남아있기에 `git stash drop`명령을 사용하여 해당 stash를 제거한다.



```
$ git stash drop stash@{0}
Dropped stash@{0} (364e91f3f268f0900bc3ee613f9f733e82aaed43)
```

* `git stash pop`명령은 stash를 적용하고 나서 바로 스택에서 제거해준다.


#stash를 만드는 새로운 방법


: stash를 만드는 방법은  stash save 명령과 같이 쓰는 `--keep-index` 이다. 이 옵션을 이용하면 이미 staging area에 들어 있는 파일을 stash 하지 않는다.

: **기본적으로 `git stash`는 추적 중인 파일만 저장한다.**
: 추적 중이지 않은 파일을 같이 저장하려면 stash명령을 사용할 때 `--include-untracked`나 `-u` 옵션을 붙여준다.

```

$ git status -s
M  index.html
 M lib/simplegit.rb
?? new-file.txt

$ git stash -u
Saved working directory and index state WIP on master: 1b65b17 added the index file
HEAD is now at 1b65b17 added the index file

```

: 끝으로 --patch 옵션을 붙이면 git은 모두 수정된 모든 사항을 저장하지 않는다. 

* stash를 적용한 브랜치 만들기


: 보통 stash를 저장하면 , 그대로 유지한 채로 그 브랜치에서 계속 새로운 일을 한다. stash한 것을 쉽게 다시 테스트하는 방법은 
**'git stash branch'명령을 실행하면 stash할 당시의 커밋을 checkout한 후 새로운 브랜치를 만들고 여기에 적용한다.**

: **이명령은 브랜치를 새로 만들고 stash를 복원해주는 매우 편리한도구**

* 워킹 디렉토리 청소하기

: 작업하고 있던 파일을 모두 stash하지 않고, 단순히 그 파일들을 치워버리고 싶을 때가 있을경우 `git clean` 명령이 그 일을 한다. 보통은 merge나 외부도구가 만들어낸 파일을 지울경우 각종 파일을 지우는데 필요하다. 보통은 merge나 외부 도구가 만들어낸 파일을 지우거나 이전 빌드 작업으로 생성된 각종 파일을 지우는 데 필요하다. **주의 할 점은 추적 중이지 않은 모든 정보를 워킹 디렉토리에서 지우고 싶으면 `git clean -f -d` 명령을 사용하자. `-f`의 뜻은 강제의 의미 "진짜로 그냥 해라"**

* 실행결과를 미리 보고 싶을 경우 : `git clean -d -n`

```

$ git clean -d -n
Would remove test.o
Would remove tmp/

```

: **`git clean` 명령은 추적중이지 않은 파일만 지우는게 기본 동작이다** .gitignore에 명시했거나 해서 무시되는 파일은 지우지 않는다. 
무시된 파일까지 함께 지우려면 `-x`옵션이 필요하다.

```

$ git clean -n -d -x
Would remove build.TMP
Would remove test.o(빌드 파일도 지워진다)
Would remove tmp/

```
#git 도구 - 검색

: 프로젝트가 크든 작던 함수의 정의나 함수가 호출되는 곳을 검색해야 하는 경우가 많다. 

* `git grep`

: grep명령을 이용하면 커밋 트리의 내용이나 워킹 디렉토리의 내용을 문자열이나 정규표현식을 이용해 쉽게 찾을 수가 있다.

```
$ git grep -n gmtime_r
compat/gmtime.c:3:#undef gmtime_r

```

: `-n`명령을 추가하면, 찾을 문자열이 위치한 라인 번호도 같이 출력된다.

```
$ git grep --count gmtime_r
compat/gmtime.c:4(4개)
compat/mingw.c:1(1개)
compat/mingw.h:1
date.c:2
git-compat-util.h:2
```

: `--count`옵션을 이용하여 어떤파일에서 몇개나 찾았는지 나타낸다.

```
$ git grep -p gmtime_r *.c
date.c=static int match_multi_number(unsigned long num, char c, const char *date, char *end, struct tm *tm)
date.c:         if (gmtime_r(&now, &now_tm))

```

: 매칭되는 라인이 있는 함수나 메서드를 찾고 싶다면 `-p`옵션을 준다.
: gmtime_r 함수를 date.c파일에서 match_multi_number, match_digit 함수에서 호출된다는 것을 알수 있다.

* `break`와 `--heading` 옵션을 붙여 더 읽기 쉬운 형태로 출력할수 있다.

```
$ git grep --break --heading \
    -n -e '#define' --and \( -e LINK -e BUF_MAX \) v1.8.0
v1.8.0:builtin/index-pack.c
62:#define FLAG_LINK (1u<<20)

v1.8.0:cache.h
73:#define S_IFGITLINK  0160000
74:#define S_ISGITLINK(m)       (((m) & S_IFMT) == S_IFGITLINK)

v1.8.0:environment.c
54:#define OBJECT_CREATION_MODE OBJECT_CREATION_USES_HARDLINKS

```

: gir grep 명령은 매우 빠르고, 워킹디렉토리 뿐만 아니라 git 히스토리 내의 어떤 정보라도 찾아낼 수 있다.

#git 로그 검색


: 히스토리에서 언제 추가되었는지 변경되었는지 찾아 볼 수도 있다. '-S' 옵션을 이용해 해당 문자열이 추가된 커밋과 없어진 커밋만 검색가능

```

git log -SZLIB_BUF_MAX --oneline

e01503b zlib: allow feeding more than 4GB in one go
ef49a7a zlib: zlib can only process 4GB at a time

```

: 두 커밋의 변경 사항을 살펴보면, ef49a7a에서 ZLIB_BUF_MAX 상수가 처음 나오고 e01503b 에서는 변경된 것을 알 수 있다.

* **`git log -L`** 


: 어떤 함수나 한 라인의 히스토리를 볼 수 있다.

: 예를 들어 zlib.c파일에 있는 git_deflate_bound 함수의 모든 변경 사항을 보기원할경우

```
$ git log -L :git_deflate_bound:zlib.c
```



#git 도구 - 히스토리 단장하기

* git의 장점

(1) staging area로 커밋할 파일을 고르는 일을 순간으로 미루기
(2) stash명령으로 하던일 미루기
(3) 이미 커밋에서 결정한 내용 수정
(4) 커밋들의 순서 바꾸기
(5) 커밋을 합치거나 커밋을 분리
(6) 커밋 전체 삭제

: 이 모든 행위는 다른 사람과 코드를 공유하기 전에 해야 한다.

* 마지막 커밋을 수정하기(커밋메세지수정)

`$ git commit --amend`


: 이 명령은 자동으로 텍스트 편집기를 실행시켜서 마지막 커밋 메세지를 열어준다.

* 수정한파일이나 새로운 파일을 최근 커밋에 집어넣는 방법

: 파일수정한후 `git add`명령으로 staging area에 넣거나 git rm명령으로 추적하는 파일 삭제 , `git commit --amend명령`으로 커밋하면 된다.

: 이때 SHA-1값이 바뀌기 때문에 과거의 커밋을 변경할 때 주의해야 한다. **Rebase와 같이 이미 push한 커밋은 수정하면 안된다**

* 커밋 메세지를 여러 개 수정하기

: 어느 시점부터 HEAD까지의 커밋을 한번에 Rebase 한다. 대화형 Rebase 도구를 사용하면 커밋을 처리할 때마다 잠시 멈춘다.

: 마지막 커밋 메세지 세개를 수정할경우


`$ git rebase -i HEAD~3`


: 이 명령은 Rebase하는 것이기 때문에 메세지의 수정여부에 관계없이 HEAD~3..HEAD범위에 있는 모든 커밋을 수정한다.(이미 중앙서버에 push한 커밋은 절대 고치지 말아야 한다.)

* **git log명령과는 정반대의 순서로 나열된다**

: 대화형 Rebase는 스크립트에 적혀 있는 순서대로 'HEAD~3'부터 적용하기 시작하고 위에서 아래로 각각의 커밋을 순서대로 수정한다. 제일 위에있는것이 가장오래된것

: 특정 커밋에서 실행을 멈추게 하려면 스크립트를 수정해야한다. 'pick'이라는 단어를 'edit'로 수정하면 그 커밋에서 멈춘다. 가장 오래된 커밋 메세지를 수정할경우

```
edit f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

: 저장하고 편집기를 종료하면 git은 목록에 있는 커밋중에서 가장 오래된 커밋으로 이동

```
$ git rebase -i HEAD~3
Stopped at f7f3f6d... changed my name a bit
You can amend the commit now, with

       git commit --amend

Once you’re satisfied with your changes, run

       git rebase --continue
       
```

: 명령 프롬프트가 나타날 때 git은 Rebase과정에서 현재 정확히 뭘해야하는지 알려준다.

```
$ git commit --amend

$ git rebase --continue

```

: 이렇게 두개의 커밋에 적용하면 끝이다. 다른것도 pick을 edit로 수정해서 이 작업을 반복할 수 있다.

* 커밋 순서 바꾸기

: 대화형 Rebase 도구로 커밋 전체를 삭제하거나 순서를 조정할 수 있다.  “added cat-file” 커밋을 삭제하고 다른 두 커밋의 순서를 변경하려면 아래와 같은 Rebase 스크립트를

```
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file

```
이것을 아래와 같이 수정한다.

```
pick 310154e updated README formatting and added blame
pick f7f3f6d changed my name a bit
```

: 수정할 내용을 저장하고, 편집기를 종료하면 git은 브랜치를 이 커밋의 부모로 이동시키고 나서 `310154e`와 `f7f3f6d`를 순서대로 적용한다.

* 커밋 합치기

: `squash`를 입력하면 Git은 해당 커밋과 바로 이전 커밋을 합칠 것이고 커밋 메시지도 Merge 한다. 그래서 3개의 커밋을 모두 합치려면 스크립트를 아래와 같이 수정한다.

```
pick f7f3f6d changed my name a bit
squash 310154e updated README formatting and added blame
squash a5f4a0d added cat-file

```

: f7f3f6d에 나머지 커밋을 다 합친다는 뜻 (pick은 합칠려고 하는 커밋)

```
# This is a combination of 3 commits.
# The first commit's message is:
changed my name a bit

# This is the 2nd commit message:

updated README formatting and added blame

# This is the 3rd commit message:

added cat-file

```

: pick한 해당 커밋과 바로 이전 커밋을 합칠 것이고, 커밋 메세지도 merge한다. 이 메세지를 저장하면 3개의 커밋을 모두 합쳐진 커밋 한개만 남는다.

* 커밋 분리하기

: 커밋을 분리한다는 것은 기존의 커밋을 해체하고(되돌려놓고) stage를 여러개로 분리하고 나서 그것을 원하는 횟수만큼 다시 커밋한다는 의미

: 커밋 세개중에서 가운데것을 분리한다고 하면 이 커밋의 updated README formatting and added blame’을 ‘`updated README formatting'과 ``added blame’'으로 분리 해보자

: 'git rebase -i ' 스크립트에서 해당 커밋을 'edit'로 변경한다

```
pick f7f3f6d changed my name a bit
edit 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

: 저장하고 나서 명령 프롬프트로 넘어간 다음에 그커밋을 해체하고 그내용을 다시 두개로 나눠서 커밋하면 된다. **커밋을 해체하는 명령 : 'git reset HEAD^'라는 명령으로 커밋을 해체한다. 그러면 수정했던 파일은 unstaged 상태가 된다. stage한후 커밋하고 'git rebase --contiune'라는 명령을 실행하면 남은 rebase 작업이 끝난다.

```

$ git reset HEAD^
$ git add README
$ git commit -m 'updated README formatting'
$ git add lib/simplegit.rb
$ git commit -m 'added blame'
$ git rebase --continue


```

: **rebase 하면 목록에 있는 모든 커밋의 SHA-1 값은 변경된다. 절대로 이미 서버에 push 한 커밋을 수정하면 안된다.**

* filter-branch는 포크레인

: 수정할 커밋이 많아서 rebase 스크립트로 수정하기 어렵다면, 다른방법이 있다. 커밋의 이메일 주소를 변경하거나 어떤 파일을 삭제하는 경우는 **filter-branch'라는 명령으로 수정할수 있는데  rebase가 삽이라면 이 명령은 포크레인이다.

: 모든 커밋에서 파일을 제거하기

: filter-branch는 히스토리 전체에서 필요한 것만 골라내는데 사용하는 도구다. 'filter-branch'의 --tree-filter'라는 옵션을 사용하면 히스토리에서 파일을 아예 제거할 수 있다.

```
$ git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
Rewrite 6b9b3cf04e7c5686a9cb838c3f36a8cb6a0fc2bd (21/21)
Ref 'refs/heads/master' was rewritten

```

: --tree-filter 옵션은 **프로젝트를 checkout한 후에 각 커밋에 명시한 명령을 실행시키고 그결과를 다시 커밋한다.** 

: 이런작업은 테스팅 브랜치에서 실험하고나서 master 브랜치에 적용하는게 좋다.

* 하위 디렉토리를 루트 디렉토리로 만들기

: 모든 커밋에 대해 trunk 디렉토리를 프로젝트 루트 디렉토리로 만들때도 filter-branch 명령이 유용하다.


```
$ git filter-branch --subdirectory-filter trunk HEAD
Rewrite 856f0bf61e41a27326cdae8f09fe708d679f596f (12/12)
Ref 'refs/heads/master' was rewritten

```

: trunk 디렉토리를 루트 디렉토리로 만들었다.

* 모든 커밋의 이메일 주소를 수정하기

: 'filter-branch 명령의 --commit-filter 옵션을 사용하여 해당 커밋만 골라서 이메일 주소를 수정할 수 있다.


```
$ git filter-branch --commit-filter '
if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
 then
                GIT_AUTHOR_NAME="Scott Chacon";
                GIT_AUTHOR_EMAIL="schacon@example.com";
                git commit-tree "$@";
        else
                git commit-tree "$@";
        fi' HEAD
        
```
 
 
: 이메일 주소를 새 주소로 변경했다.


#Reset 명확히 알고가기

: git을 서로 다른 세 트리를 관리하는 컨텐츠관리자로 생각한다면,

* 트리 : 파일의 묶음

트리 | 역할
---|---
HEAD | 마지막 커밋 스냅샷, 다음 커밋의 부모커밋
index | 다음에 커밋할 스냅샷
워킹 디렉토리 | 샌드박스

: 실제로는 index는 트리가 아니다.

* HEAD

: HEAD는 현재 브랜치를 가르키는 포인터, **브랜치는 브랜치에 담긴 커밋중 가장 마지막 커밋을 가르킨다.**

: 단순하게 생각하면 **HEAD는 마지막 커밋의 스냅샷이다 **

* Index

: index는 바로 다음에 커밋할것들이다. staging area라는 개념이며 사용자가 git commit 명령을 실행하기전 보관소이다.

: index는 워킹 디렉토리에서 마지막 checkout 한 브랜치의 파일 목록과 파일 내용으로 채워진다.

* 워크플로우

![](https://git-scm.com/book/en/v2/images/reset-workflow.png)

![](https://git-scm.com/book/en/v2/images/reset-ex1.png)

: 파일이 하나 있는 디렉토리로 이동할경우, git init명령을 실행하면 git 저장소가 생기고 HEAD는 아직 없는 브랜치를 가르킨다

![](https://git-scm.com/book/en/v2/images/reset-ex2.png)

: git add명령으로 워킹 디렉토리의 내용을 index로 복사한다.

![](https://git-scm.com/book/en/v2/images/reset-ex3.png)

: git commit 명령을 실행하면, index의 내용을 스냅샷으로 영구히 저장하고, 그 스냅샷을 가르키는 커밋객체를 만든다. 그리고는 **'master'가 그 커밋 객체를 가르키도록 한다.

: git status명령을 실행하면, 아무런 변경사항이 없다고 한다. 세 트리가 모두가 같기 때문이다.

: 파일내용을 바꾸고 커밋할경우, 워킹디렉토리의 파일을 고친다. 이파일명을 v2라고 할경우

![](https://git-scm.com/book/en/v2/images/reset-ex4.png)

: git add명령

![](https://git-scm.com/book/en/v2/images/reset-ex5.png)

: git commit명령

![](https://git-scm.com/book/en/v2/images/reset-ex6.png)

: git status명령을 실행하면, 아무것도 출력하자ㅣ 않는다. 세트리의 내용이 다시 같아졌기 때문이다.

: 브랜치를 바꾸거나 clone명령도 내부에서는 비슷한 절차를 밟는다. 브랜치를 checkout(바꾸기)한다면,HEAD가 새로운 브랜치를 가르키도록 바뀌고, 새로운 커밋의 스냅샷을 index에 놓는다. 그리고 index의 내용을 워킹 디렉토리에 복사한다.


* Reset의 역할

: file.txt 파일 하나를 수정하고 커밋하고 세번 반복하면

![](https://git-scm.com/book/en/v2/images/reset-start.png)


(1) HEAD 이동
: reset 명령이 하는 첫번째 일은 HEAD 브랜치를 이동시킨다. checkout 명령처럼 HEAD가 가르키는 브랜치를 바꾸지는 않는다. **현재 브랜치를 가르키는 커밋을 바꾼다** **즉 브랜치는 똑같은 상황에서, 커밋(스냅샷)을 바꾸는것이다.** 

: git reset 9e5e6a4 명령은 master 브랜치가 `9e5e6a4`를 가리키게 한다.

![](https://git-scm.com/book/en/v2/images/reset-soft.png)

: reset --sort 옵션을 사용하면 딱 여기까지 진행하고 동작을 멈춘다.

: reset 명령은 가장 최근의 git commit명령을 되돌린다. 즉 새로운 커밋을 생성하고 HEAD가 가르키는 브랜치가 새로운 커밋을 가르키도록 업데이트한다. 

: reset HEAD~를 주면 index나 워킹디렉토리는 그대로 놔두고 브랜치가 가르키는 커밋만 이전으로 되돌린다. 

(2) Index 업데이트(--mixed)

: reset명령은 index를 현재 HEAD가 가르키는 스냅샷으로 업데이트 할수 있다.

![](https://git-scm.com/book/en/v2/images/reset-mixed.png)

: **가르키는 대상을 가장 최근의 커밋으로 되돌리는 것은 같으며,staging area를 비우고, git commit 명령도 되돌리고 git add명령도 되돌리는것이다.**

(3) 워킹 디렉토리 업데이트(--hard)

: reset명령은 세번째로 워킹 디렉토리까지 업데이트 한다. --hard옵션을 사용하면 reset 명령은 이 단계까지 수행한다.

![](https://git-scm.com/book/en/v2/images/reset-hard.png)

: reset명령을 통해 git add와 git commit명령으로 생성한 마지막 커밋으로 되돌린다. 그리고 워킹디렉토리의 내용까지도 되돌린다.

: **git에는 데이터를 실제로 삭제하는 방법이 별로 없기에, 이 삭제하는 방법은 그중 하나이다.** **--hard옵션은 되돌리는 것이 불가능하다** 이 옵션을 사용할 경우, 워킹 디렉토리의 파일까지 강제로 덮어쓴다.

```
* 복습
reset 명령은 정해진 순서대로 세 개의 트리를 덮어써 나가다가 옵션에 따라 지정한 곳에서 멈춘다.

HEAD가 가리키는 브랜치를 옮긴다. (--soft 옵션이 붙으면 여기까지)

Index를 HEAD가 가리키는 상태로 만든다. (--hard 옵션이 붙지 않았으면 여기까지)

워킹 디렉토리를 Index의 상태로 만든다.

```

* 경로를 주고 Reset 하기

: reset명령을 실행할 때, 경로를 지정하면 1단계를 건너뛰고 정해진 경로의 파일에만 reset   단계를 적용한다.

: git reset file.txt명령을 실행한다고 가정할때,
`git reset --mixed HEAD file.txt`를 짧게 쓴 것이다.


```
HEAD의 브랜치를 옮긴다. (건너뜀)

Index를 HEAD가 가리키는 상태로 만든다. (여기서 멈춤)
```

![](https://git-scm.com/book/en/v2/images/reset-path1.png)

: 이 명령은 해당파일을 unstaged상태로 만든다. git add명령과 정확히 반대 


![](https://git-scm.com/book/en/v2/images/reset-path3.png)

**특정 커밋을 명시하면 Git은 “`HEAD에서 파일을 가져오는” 것이 아니라 그 커밋에서 파일을 가져온다. git reset eb43bf file.txt 명령과 같이 실행한다.**

**이 명령을 실행한 것과 같은 결과를 만들려면 워킹 디렉토리의 파일을 v1으로 되돌리고 git add 명령으로 Index를 v1으로 만들고 나서 다시 워킹 디렉토리를 v3로 되돌려야 한다(결과만 같다는 얘기다.**

: git add 명령처럼 reset 명령도 Hunk 단위로 사용할 수 있다. `--patch` 옵션을 사용해서 Staging Area에서 Hunk 단위로 Unstaged 상태로 만들 수 있다. 이렇게 선택적으로 Unstaged 상태로 만들거나 내리거나 이전 버전으로 복원시킬 수 있다.



#합치기(Squash)

: 여러 커밋을 하나로 합치는 도구

![](https://git-scm.com/book/en/v2/images/reset-squash-r1.png)

: 첫번째 파일은 커밋하나를 추가했고, 두번째 커밋은 기존파일을 수정하고 새로운 파일도 추가했고, 세번째 커밋은 첫번째 파일을 다시 수정했을경우 , 두번째 커밋은 아직 작업중인 커밋으로 이 커밋을 세번째 커밋과 합치고 싶은 상황


`git reset --soft HEAD~2  : HEAD포인터를 이전 커밋으로 되돌릴 수 있다.`


![](https://git-scm.com/book/en/v2/images/reset-squash-r2.png)

: git commit명령 실행

![](https://git-scm.com/book/en/v2/images/reset-squash-r3.png)

: file-a.txt 파일이 있는 v1 커밋이 하나 그대로 있고, 두 번째 커밋에는 v3버전의 file-a.txt 파일과 새로 추가된 file-b.txt 파일이 있다. v2 버전은 더는 히스토리에 없다.

#checkout

: git checkout [branch] 명령은 git reset --hard [branch] 명령과 비슷하게 [branch] 스냅샷을 기준으로 세 트리를 조작한다. 하지만, 두 가지 사항이 다르다.

(1) reset --hard 명령과는 달리 checkout 명령은 워킹 디렉토리를 안전하게 다룬다.

: 저장하지 않은 것이 있는지 확인해서 날려버리지 않는다는 것을 보장한다. 사실 보기보다 좀 더 똑똑하게 동작한다. 워킹 디렉토리에서 Merge 작업을 한번 시도해보고 변경하지 않은 파일만 업데이트한다. 반면 reset --hard 명령은 확인하지 않고 단순히 모든 것을 바꿔버린다.

(2) HEAD를 업데이트 하는가 안하는가?

: reset 명령은 HEAD가 가리키는 브랜치를 움직이지만(브랜치 Refs를 업데이트하지만), checkout 명령은 HEAD 자체를 다른 브랜치로 옮긴다.

: reset 명령은 HEAD가 가리키는 브랜치를 움직이지만(브랜치 Refs를 업데이트하지만), checkout 명령은 HEAD 자체를 다른 브랜치로 옮긴다.

![](https://git-scm.com/book/en/v2/images/reset-checkout.png)

![](https://git-scm.com/book/en/v2/images/reset-checkout.png)


* 경로있음
: checkout 명령을 실행할 때 파일 경로를 줄 수도 있다. reset 명령과 비슷하게 HEAD는 움직이지 않는다. 동작은 git reset [branch] file 명령과 비슷하다. Index의 내용이 해당 커밋 버전으로 변경될 뿐만 아니라 워킹 디렉토리의 파일도 해당 커밋 버전으로 변경된다. 완전히 git reset --hard [branch] file 명령의 동작이랑 같다. 워킹 디렉토리가 안전하지도 않고 HEAD도 움직이지 않는다.

git reset이나 git add 명령처럼 checkout 명령도 --patch 옵션을 사용해서 Hunk 단위로 되돌릴 수 있다.

* 요약

 dsaf| HEAD | INDEX | WORKDIR | WD SAFE?
 ---|---|---|---|---|
 Commit Level |
 reset --soft [commit]|REF|NO|NO|YES
 reset [commit]|REF|YES|NO|YES
 reset --hard [commit]|REF|YES|YES|NO
 checkout [commit]|HEAD|YES|YES|YES
 FILE LEVEL|
 reset (commit) [file]|NO|YES|NO|YES
 checkout (commit) [file]|NO|YES|YES|NO










