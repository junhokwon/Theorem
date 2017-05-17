#반응형 웹

-

* Responsive Web

하나의 HTML파일에 적용되는 CSS속성만 바꾸어, 
여러 너비의 디바이스에서 적절히 레이아웃의 모양을 변경하는 기술

- 뷰포트(viewport)

스마트폰에서 내용이 표시되는 영역 
모바일 브라우저의 기본 viewport width는 980px로, 
가로크기가 제각각인 모바일 기기의 특성상 뷰포트 값을 지정해야 함

- 뷰포트 태그

> meta태그를 사용하며, head와 /head사이에 기록

	<meta name='viewport' content='width=device-width, initial-scale=1'>
	일반적으로 모바일 기기의 가로너비를 전체 표시되는 영역으로 설정하며, 그 경우 content에
	width=device-width를 지정

- 뷰포트 태그의 content에 설정 가능한 속성/값

속성	|설명	|값	| 기본값
----|------|----|----
width	|뷰포트 너비|	device-width 또는 크기값|브라우저 기본 값(대부분 980px)
height	|뷰포트 높이	|device-height 또는 크기	|브라우저 기본 값
user-scalable|	확대/축소 가능여부|	yes/no	|yes
initial-scale|	초기 확대/축소 값	|1~10	|1
minimum-scale|	최소 확대/축소 값|	0~10	|0.25
maximum-scale|	최대 확대/축소 값|	0~10	|1.6

- 미디어쿼리 (media queries)

접속한 장치에 따라 특정 CSS스타일을 사용할 수 있도록 해주는 CSS모듈

- 미디어쿼리 구문

	기본문법
	@media [only | not] 미디어유형 [and 	조건(복수)]

- 일반적으로 많이 사용하는 구문

- @media screen (min-width: 1024px) : 최소크기가 1024px 이상일 때 (PC)
- @media screen (max-width: 1023px) : 너비가 1023px 이하일 때 (태블릿)
- @media screen (max-width: 768px) : 너비가 768px 이하일 때 (모바일)
- @media screen (max-width: 320px) : 너비가 320px 이하일 때 (모바일, 아이폰5이하)


-
#emmet 정리

-

* child : >(하위요소설정) : nav>ul>li
* sibling : + (연속해서 만들기) : div+p+bq
		
		<div></div>
		<p></p>
		<bq></bq>
		
* climb-up: ^ : (한단계 올리기) : div+div>p>span+em^bq

		<div></div>
		<div>
    	<p><span></span><em></em></p>
    	<blockquote></blockquote>
		</div>
		
* multiplication : *(복사) : 	ul>li*5
* item numbering : $(숫자대체) : ul>li*item$*5
* ID선택자/class 선택자 : #header/.title
* custom 속성 : p[title="hello world"]

		td[rowspan=2 colspan=3 title]
	
: [a='value1' b='value2']


		<div a="value1 b="value2></div>


* text: a{click me} 
: a요소에 내용 click me

* p>{Click }+a{here}+{ to continue}

		<p>Click <a href="">here</a> to continue</p>
		

* form#search.wide


		<form id="search" class="wide"></form>


-

#vim


: unix환경에서 사용하는 텍스트 편집기(마우스가 없었을때 발달)(모든것을 키보드로 활용)

* 조작법

>insert모드 : 내용삽입

>esc버튼 : insert모드를 나온 후 여라가지 단축키를 사용하여 편집을 한다.

* 삽입

키|        기능       
----|----
i | 커서 위치에 insert
I | 줄맨 앞에서 insert
a | 커서 다음에 insert
A | 줄 맨뒤에서 insert
o | 커서 아래로 한 줄 띄우고 insert
O | 커서 위로 한줄 띄우고 insert

* 이동

키 | 기능
----|----
w| 단어 첫글자 기준으로 다음으로 이동
W | 공백 기준으로 다음(단어의시작)으로 이동
b|단어 첫 글자 기준으로 이전으로 이동
B|공백 기준으로 이전으로 이동
e | 단어 마지막 글자 기준으로 다음으로 이동
E| 공백 기준으로 다음(단어의 끝)으로 이동
gg| 문서 맨앞으로 이동
G|문서 맨 아래로 이동
^ | 문장 맨 앞으로 이동
$ | 문장 맨 뒤로 이동



* 검색


키 | 기능
----|----
/|해당 word를 검색, n/N으로 다음/이전 찾기

* 편집

|키 | 기능|
|:---:|:----:|
|dd|현재 줄 잘라내기|
yy|현재 줄 복사하기
p|붙여넣기
u|실행취소(undo)
ctrl+r|재실행(redo)
v | visual모드
y | 복사
c | 잘라내기

* 저장 : shift + 따음표를 사용한후 사용

키 | 기능
----|----
:w | 저장
:q | 닫기
:q! | 저장하지 않고 닫기
:wq | 저장하고 닫기
:숫자 | 지정한 줄 번호로 이동


* vim 쉘 설정들어가는법 

: git config --global core.editor vim(vim편집기로 설정을 바꾼다)

* vim설정 다보기 : vim ~/.zshrc

* ls - al : 전체목록
* l - : 전체목록

* touch README.md : macdown파일을 만든다.

* vim/vi 파일명 : vim파일 만들기

* echo 'show me the money' > abc4.txt : abc4.txt파일의 내용을 쇼미더머니라고 적어서 파일을 만들기

* git : init - git하고 연결하기

* git상태보기 : git status

* git add 파일명 : untracked영역에서
staged영역으로 옮김

* git add --all : 모든파일을 staged영역으로 옮긴다.

* git log : commit된 파일목록보기


* git commit : staged에서 unmodified영역으로 옮김

* git commit -m "README 파일 수정"

: git commit -m "내용" : commit된 파일에 내용을 넣는다.

* git reset HEAD abc2.txt : staged영역에서 untracked영역으로 돌린다.

* git diff --staged : staged영역에 있는 내용보기

* git diff : unmodified영역에 있는 내용보기