add html/css theorem

-

#Html 기본
-


* 마크업 언어 : 태그 등을 이용하여 데이터의 구조를 명기하는 언어

* 하이퍼텍스트 : 링크를 이용해 서로 연결하는 것을 의미

* 웹표준 : [W3C](https://ko.wikipedia.org/wiki/W3C)

* 웹표준지원정도 : [html5test](http://html5test.com)

* 주석 : cmd + ? <!---->

* 태그의 요소와 속성(html 기본구조) : <요소 속성="값">또는 <요소 속성="값>내용</요소>

* 절대경로와 상대경로 : (1) 절대경로는 http://로 시작하는 전체주소를 의미 (2) 상대경로는 해당 html파일을 기준으로 한 경로

* head 태그 : 

(1) 문서의 메타데이터 집합,html:5 + tag하면 웹페이지 인코딩방식(meta charset="UTF-8")


(2) IE에서의 렌더링 방식 최신(meta http-equiv="X-UA-Compatible" content="ie=edge">

(3) css파일연결,javascript연결,문서의제목(title)등이 있다.

* body 태그 : 브라우저에 표시할 내용 : h1,p,blockquote등

-

#블록과 인라인

(1) 블록요소 : 줄바꿈이 일어나는 형태,기본적으로 width가 전체너비의 값을 지닌다.
	

	<h1>블록요소</h1>
	<p>p요소는 블록형태입니다</p>
	<div>div요소도 블록형태입니다</div>




(2) 인라인요소 : 줄바꿈이 일어나지 않는형태, 기본적으로 자신의 내용만큼의 너비(width)를 가진다. 블록요소안에 인라인요소를 넣을수 있다.

	<strong>strong 요소</strong> 
	<a href=“”>a요소</a>  
	<span>span요소</span>
	

(3)레이아웃 요소 : div와 span은 블록과 인라인 방식의 레이아웃을 구현하는데 사용


			<div>  			<p>블럭요소 내부에 <span>인라인 요소를 사용합니다</span></p>  			</div>

-

#HTML 태그
: 텍스트와 관련된 태그




(1) 헤드라인 : h1~h6,중요도 순으로 개요를 나타낼때 사용,학술문서나 검색엔진검색시,단계별로 구분할 제목이 있다면 hr태그를 사용하는것이 적합

>
<h1>HTML</h1>   
<h3>개발</h3>   
<h3>최초규격</h3>
<h4>역사</h4>


(2) 줄바꾸기 태그: p태그,br태그


> *p태그   <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Repudiandae sequi ratione tenetur reiciendis cum in totam atque ipsum similique quae.</p>  <p>Lorem ipsum dolor sit amet.</p>
각 p태그 아래에 공백이 생긴다

> *br태그
> 
> 
 Lorem ipsum dolor sit amet, consectetur adipisicing elit. Tenetur <br>Lorem ipsum dolor sit amet.</br>
 줄바꿈간의 여백이 없다. 


(3) 그외 태그

* hr 태그 : 수평선
* blackquote태그 : 인용문

		<blockquote>인용문 내용</blockquote>

* pre태그 : 이미 형식화된 텍스트

 			<pre> 			def pretag_test():    			 val = ‘pretag’ 			</pre>
 
 
 (4) 줄바꾸지 않는 태그
 
* strong,b태그(강조,굵게표시)
 
 >
	<strong>강조할 텍스트</strong>   
	<b>굵게 표시할 텍스트</b>
	
* em,i태그(문맥상특정부분강조,이탤릭표시)
>
	<em>강조할 텍스트</em>   
	<i>기울여 표시할 텍스트</i>
	
	
* mark 태그(형광펜효과)
> <mark>형광펜으로 그은 효과의 텍스트</mark>


(5) 링크태그

	<a href="http://www.naver.com" target="_blank" title=“네이버 열기”>sdf</a>
	
* href : 이동할 페이지주소
* target_self : 기본창의 주소 바꾸기
* target_blank : 새창 열어서 바꾸기
* title : 마우스를 올렸을 때 보여줄 제목

(6) 이미지태그

	<img src=“이미지 경로" width="100" height=“200" alt=“이미지 설명">
	
* src: 이미지의 경로
* width,height :이미지의 가로/세로 크기(px단위)
* alt : 대체 텍스트

(7) 데이터태그

1.목록

* ol/ul : ol은 순서  ul은 순서없는 항목

>	<ol>   
><li>항목</li><li>항목</li> 
<li>항목</li> 
<li>항목</li>
<li>항목</li></ol>         >	<ul>   
><li>항목</li> <li>항목</li>
 <li>항목</li> 
 <li>항목</li> 
 <li>항목</li></ul>

2.목록속성

	<ol type="A" start="3" reversed>   	<li>First</li>  	<li>Second</li>  	<li>Third</li>  	</ol>
	
* type : 1은 숫자(기본값),a는 영문소문자,A는 영문대문자 ,i는 로마숫자 소문자, I는 로마숫자 대문자
* start : 시작할 숫자 지정
* reversed : 역순으로 표시

3.정의목록

	<dl>  	<dt>HTML</dt>  	<dd>Hyper Text Markup Language</dd>   	<dd>웹 페이지를 구현하는 마크업 언어이다</dd>
	</d1>

* dl: 정의목록태그
* dt : 목록중 개념을 나타냄
* dd: 해당개념의 정의를 나타냄
* 목록과 정의목록은 서로 중첩에서 사용이 가능

-

#테이블 요소

(1) 테이블의 기본구조

<table>   
<tr>  <th>이름</th> 
<th>나이</th> 
<th>점수</th></tr>   
<tr>  <td>철수</td> 
<td>23세</td>   
<td>70점</td>  </tr>  
<tr>  <td>영희</td>  
<td>21세</td>   
<td>89점</td>  </tr>   
</table>

* emmet을 사용할 경우  table>(tr>th*3)*3

(2)가로셀병합 : colspan

<table>   
<tr>  <td>a</td>  <td>b</td>   
</tr>  <tr>  <td colspan="2">c</td>  </tr>   
</table>

* emmet : table>(tr>td)+(tr>td[colspan=2]

(3)세로셀병합 : rowspan

<table>  
<tr>  <td rowspan="3">a</td>  <td>b</td>   
</tr>  <tr>   
<td>c</td>  </tr>  
<tr>  <td>d</td>   
</tr>  </table>

* emmet : table>(tr>td[rowspan=3])+((tr>td)*2))


(4) 행의 구조화 : 행을그룹화 하는것으로

제목 : thead

본문 : tbody  

도표하단 : tfoot(전체의합계나 결과를 표시)

-


* 실습


<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Table</title>
  <style>
    html, body {
      font-size: 12px;
      color: #333;
    }
    table {
      border-collapse: collapse;
      text-align: center;
    }
    th, td {
      border: 1px solid #999;
      padding: 4px 8px;
    }
  </style>
</head>
<body>
  <h1>Table</h1>
  <h3>Table basic, colspan</h3>
  <!-- table>(tr>th*3)+(tr>td*3)*2 -->
  <table>
    <tr>
      <th>이름</th>
      <th>나이</th>
      <th>점수</th>
    </tr>
    <tr>
      <td>철수</td>
      <td>23세</td>
      <td>70점</td>
    </tr>
    <tr>
      <td>영희</td>
      <td>21세</td>
      <td>89점</td>
    </tr>
    <tr>
      <td colspan="2">평균</td>
      <td>79.5점</td>
    </tr>
  </table>

  <h3>rowspan</h3>
  <!-- 오른쪽 구조를 emmet으로 한 번에 나올 수 있도록 작성 -->
  <!-- table>(tr>td[rowspan=3]+td)+(tr>td)*2 -->
  <table>
    <tr>
      <td rowspan="3">a</td>
      <td>b</td>
    </tr>
    <tr>
      <td>c</td>
    </tr>
    <tr>
      <td>d</td>
    </tr>
  </table>

  <h3>thead, tbody, tfoot</h3>
  <!-- table>thead>tr>th*5 -->
  <table>
    <thead>
      <tr>
        <td colspan="5">주인공 스탯 정보</td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>이름</th>
        <th>나이</th>
        <th>성별</th>
        <th>성적</th>
        <th>메모</th>
      </tr>
      <tr>
        <td>홍길동</td>
        <td>22세</td>
        <td>남자</td>
        <td>98점</td>
        <td>잘생김</td>
      </tr>
      <tr>
        <td>김춘향</td>
        <td>19세</td>
        <td>여자</td>
        <td>99점</td>
        <td>예쁨</td>
      </tr>
      <tr>
        <td colspan="3">평균</td>
        <td>87</td>
        <td></td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td colspan="5">Copyright @ ...</td>
      </tr>
    </tfoot>
  </table>

  <h3>Example</h3>
  <table>
    <thead>
      <tr>
        <td colspan="9">포켓몬스터의 전설의 포켓몬</td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>지방</th>
        <th colspan="2">메인 전설의 포켓몬</th>
        <th colspan="6">그 외 전설의 포켓몬</th> <!--총 9칸 가정-->
      </tr>
      <tr>
        <td>관동지방</td>
        <td colspan="2">뮤츠</td>
        <td colspan="2">프리져</td>
        <td colspan="2">썬더</td>
        <td colspan="2">파이어</td>
      </tr>
      <!-- tr>(td*3)+(td[colspan=2])*3 -->
      <tr>
        <td>성도지방</td>
        <td>칠색조</td>
        <td>루기아</td>
        <td colspan="2">라이코</td>
        <td colspan="2">엔테이</td>
        <td colspan="2">스이쿤</td>
      </tr>
      <tr>
        <td rowspan="2">호연지방</td>
        <td>그란돈</td>
        <td>가이오가</td>
        <td colspan="2">레지락</td>
        <td colspan="2">레지아이스</td>
        <td colspan="2">레지스틸</td>
      </tr>
      <tr>
        <td colspan="2">레쿠쟈</td>
        <td colspan="2">라티아스</td>
        <td colspan="2">라티오스</td>
        <td colspan="2">테오키스</td>
      </tr>
      <tr>
        <td rowspan="2">칼로스지방</td>
        <td>제르네아스</td>
        <td>이벨타르</td>
        <td rowspan="2" colspan="6"></td>
      </tr>
      <tr>
        <td colspan="2">지가르데</td>
      </tr>
      <tr>
        <td rowspan="3">알로라지방</td>
        <td>코르모그</td>
        <td>코스모움</td>
        <td colspan="3">카푸꼬꼬꼭</td>
        <td colspan="3">카푸나비나</td>
      </tr>
      <tr>
        <td>솔가레오</td>
        <td>루나아라</td>
        <td colspan="3">카푸브루루</td>
        <td colspan="3">카푸느지느</td>
      </tr>
      <tr>
        <td colspan="2">네크로즈마</td>
        <td colspan="3"></td>
        <td colspan="3"></td>
      </tr>
    </tbody>
  </table>
</body>
</html>

# Form 요소 
: 데이터를 입력하거나 전송할때 사용하는 HTML요소


(1) form 태그 양식 : 브라우저(클라이언트)에서 서버로 데이터를 전송하기 위해 사용하는 태그

	<form action="" method="get">  	<label for="username">ID</label>   		<input type="text" name="username">  	</form>

* action : form에서 데이터를 전송할 URL
* method : get(url을 데이터에 담아 전달),post(url과는 별도로 데이터를 전달)


(2) input태그 : input요소는 값을 가지거나 form에 영향을 줍니다.

<input type="text" id="username">  <input type="password" id="password">  <input type="radio" id="radio">  <!--동그란 버튼--><input type="checkbox" id="checkbox">  <input type="button">  <input type="file" id="file">  <input type="submit">  <input type="reset">  <input type="hidden" id="hidden" 		value="hiddenValue">

<input type="text" value="disabled" 		disabled>   
<input type="text" value="readonly" 		readonly> 
<input type="text" required><input type=“text" placeholder=“공백은 	안됩니다">  
<input type="text" size="3">  <input type="text" maxlength="10">  <input type="checkbox" checked="checked">  <input id="radio1" type="radio" name="agree" checked="checked">   
<input id="radio2" type="radio" name="agree"> 
<!--- 같은 이름을 가진 radio요소는 동시에 체크되지 않는다-->

(3) label 태그 : 폼 요소에 레이블을 붙임

- label 내부에 표현 


			<label>ID<input type="text" value:아이디></label>
	
- label와 별도로 표현



				<label for="username">username</label>
				<input type="text" id="username">
	
	- for속성과 form요소의 id를 연결시킬경우
	- label이 정확히 해당 form요소를 가르킴

(4) select 태그(multiple,size속성)

	<select name='number id="select-id">
	<option value="1">FIRST</option>
	<option value="2">Second</option>   		<option value="3">Third</option>   		<option value="4">Fourth</option>  	</select>
	
	- select태그는 여러개의 주어진 값중
	- 일부를 선택하는 역할을 한다.

	
	- multiple 속성 : 여러개의 값을 가질수있다.
	<select multiple="multiple">  	<option value="apple">Apple</option>   	<option value="banana">Banana</option>  
	 <option value="orange">Orange</option>  	</select>
	
	
	- size 속성 : 한번에 option을 몇개 보여줄지 정합니다.
	<select size="2">  	<option value="apple">Apple</option>   	<option value="banana">Banana</option>   
	<option value="orange">Orange</option>  	</select>
	
(5) outgroup 태그 : select요소의 카테고리를 구분지음
	
	<select>  	<optgroup label="Fruits">  	<option value="apple">Apple</option>   	<option value="banana">Banana</option>   
	<option value="orange">Orange</option>  	</optgroup>  	<optgroup label="Colors">  	<option value="red">Red</option>   		<option value="blue">Blue</option>   	<option value="green">Green</option>  	</optgroup>   </select>
	
(6) button 태그 


	<button type="submit">submit type button</button>   
	<button type="reset">reset type button</button>   
	<button type="button">button type button</button>
	
	
(7) fieldset,legend 태그

: form요소를 담는 카테고리 
: fieldset의 제목을 나타냅니다.
: legend요소는 fieldset의 첫번째 자식으로 사용해야함

	<fieldset>  	<legend>Login</legend>  	<label>username : <input type="text"></label>   
	<label>password : <input type="password"></label>  	</fieldset>
	
	
-

#크래스와 아이디 속성

: 아이디는 페이지에서 딱 한번만 설정가능,요소의 유니크한 특성을 나타냄
: class는 여러번 사용가능, 범용적인 부분을 나타냄
: 이름을 의미에 적합하게 짓는것이 중요!

	<div class="chapter" id="chapter1">   	<h2>HTML</h2>  	<p>HTML강의를 시작해봅시다</p>  	</div>  
	
	- 챕터라는 클래스이름을 가지고 
	- 챕터1이라는 id를 가졌다		
	
	
* 색상 : [색상표](https://www.w3schools.com/colors/colors_names.asp)

-

* 실습 		

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <h3>Input tags</h3>
  <form action="">
    <!-- input#username[type=text] -->
    <input type="text" name="username"><br>
    <input type="password" name="password"><br>
    <input type="radio" name="radio"><br>
    <input type="button" value="Button"><br>
    <input type="file" name="file"><br>
    <input type="submit"><br>
    <input type="reset"><br>
    <input type="hidden" name="hidden" value="hiddenValue"><br>
  </form>
  <h3>Radio</h3>
  <form action="">
    <input type="radio" name="radio-name" value="a">
    <input type="radio" name="radio-name" value="b">
    <button type="submit">전송</button>
  </form>

  <h3>Label</h3>
  <!-- form>(div>label>input[type=text])+(div>label+input[type=text]) -->
  <form action="">
    <div>
      <label>ID <input type="text"></label>
    </div>
    <div>
      <label for="username">Username</label>
      <input type="text" id="username">
    </div>
  </form>

  <h3>select</h3>
  <select name="number" id="" size="2" multiple>
    <!-- option[value=$]*4 -->
    <option value="1">First</option>
    <option value="2">Second</option>
    <option value="3">Third</option>
    <option value="4">Fourth</option>
  </select>

  <h3>select - optgroup</h3>
  <select name="optgroup-select" id="">
    <!-- (optgroup[label=]>(option[value=]*3))*2 -->
    <optgroup label="Fruits">
      <option value="apple">Apple</option>
      <option value="banana">Banana</option>
      <option value="orange">Orange</option>
    </optgroup>
    <optgroup label="Colors">
      <option value="red">Red</option>
      <option value="blue">Blue</option>
      <option value="green">Green</option>
    </optgroup>
  </select>

  <h3>fieldset</h3>
  <form action="">
    <fieldset>
      <legend>Login</legend>
      <label>username : <input type="text"></label>
      <label>password : <input type="password"></label>
    </fieldset>
  </form>
</body>
</html>

-

#CSS

: 마크업언어(html)가 실제 표시되는 방법을 기술하는 언어
레이아웃과 스타일을 정의할때 사용

(1)css 문법


			selector {
			property(속성): value;(값)
			}
 
 
 
(2) css 사용법

* 내부스타일 시트(권장)


		<style type="text/css">   
		#body-title {  		font-size: 14px;   
		font-weight: bold;
		color: DarkSlateGrey;  		} 
		
* 인라인 스타일 시트

		<p id=“body-title" 
		style=
		font-size:14px; 
		font-weight: bold; 
		color: DarkSlateGrey”>
		Inline Style Sheet
		</p>   
		
* 외부 스타일 시트

		<link rel="stylesheet" href=“style.css">
		
		
- link태그를 사용하여 외부 css파일을 html문서에 연결

-

#개발자 도구

* cmd + alt + i/cmd + shift + c : 단축키(요소선택)

-

#css 선택자


			* { 				padding: 0;  
				margin: 0;  				}


- html페이지 내부의 모든 요소에 같은 css속성을 적용합니다.

(1) 태그선택자


			h1 {  			color: red;  			}  

			p{ 			color: gray;  			margin-bottom: 10px;  
 			}
 

 
(2) 클래스 선택자(마침표기호)



		.section {  		color: #333;   
		margin-bottom: 40px;  		}  



(3) ID선택자(#기호)


		#index-title {   
		font-size: 18px; 
		}

- html에서 id값은 하나만 존재해야 합니다.

(4) 체인선택자(스타일내에 다른 아이디,클래스적용)

	#index-title.header-title 

- 한 요소에 아이디와 클래스, 또는 여러 클래스가 적용되어 있을경우에 사용합니다.

(5) 그룹선택자(둘 이상요소에 같은 스타일 적용)

		#index-title, #index-description
 
- 여러 선택자에 같은 스타일을 적용하는 경우
- 쉼표로 구분해 선택자를 나열해 사용합니다.

	
(6) *** 복합선택자<!!!> ***

	section ul {  	border: 1px solid black;  	} 
	
	- 하위선택자 : 부모요소에 포함된 모든 하위요소(ul)을 지정
		section > ul {  	border: 1px solid black;   
	}
	
	- 자식선택자 : 부모요소의 바로아래 자식만을 지정(ul)

	h1 + ul {   
	background: Azure;   color: DarkBlue;  	}
	
	- 인접형제 선택자 : 같은 부모요소를 지닌 형제관계에서
	- 먼저나오는 요소를 형요소, 나중에 나오는 요소를 동생요소	라 할때 , 형요소의 ul이 아닌 형요소 바로 다음에 오는
	- 동생요소 ul을 지정

	h1 ~ u1 {
		background : Azure;
		color : red;
		}
		
	- 일반형제 선택자 : 모든 동생요소를 지정





(7) 속성선택자
패턴|의미
----|----
E[attr]|attr속성이 포함된 요소 E
E[attr="val"]| attr속성의 값이 val인 요소
E[attr~="val"]| attr속성의 값에 val이 포함되는 요소 E(공백으로 분리된 값이 일치해야함/띄어쓰기 주의)
E[attr|="val"]|attr속성의 값에 val이 포함되거나 val-로 시작되는 요소 E
E[attr^="val"]|attr속성의 값이 val로 시작되는 요소
E[attr$="val"]|attr속성의 값이 val로 끝나는 요소
E[attr*="val"]|attr속성의 값에 val이 포함되는 요소(공백이나 dash(-)에 영향받지 	않음
 
(8)가상 클래스 선택자
 
 패턴|의미
 ----|----
 E:link| 방문하지않은 링크 E
 E:visited| 방문한 링크 E
 E:active| E요소에 마우스 클릭 또는 키보드가 눌린 동안
 E:hover|  E요소에 마우스가 올라가 있는동안
 E:focus| E요소에 포커스가 머물러 있는 동안
 E::first-line| E요소에 첫번째 라인
 E::first-letter| E요소에 첫번째 문자
 E::before| E요소의 시작 지점에 생성된 요소
 E::after| E요소의 끝 지점에 생성된 요소
 
 
 -
 
#css 우선순위

* 특정도 값이 높은 순서대로 적용됩니다.
 
 스타일|특정도
 ----|----
 인라인스타일|A
 ID선택자 | B
 class 선택자 | C
 Tag 선택자 | D
 
 
 * 인라인 스타일과 important
 
 : important값은 해당하는 요소에 지정된 어떤
 특정도도 무시하고 가장 우선순위로 적용됩니다.
 
 
 
 			p { 			font-size: 30px !important;   
			color: green !important;  			}

- important 쓴것을 잊어버리면 나중에 왜 안되는지 헤매게됨   -

#css 서체

* color : 글자색 으로 hex코드,rgb(), rgba(), 색상명이 올수 있다.

* font-family : 서체로 돋움,굴림,dotum,arial,helvetica등이 있다.

* font-size : emmet으로 fz + tag버튼 이며 글자크기를 지정한다.

> body {  font-size: 14px;  } h1 {  font-size: 28px(2em);   
}

- 부모의 글자크기가 14px이면 h1의 글자크기가
- 14px * 2.0 = 28px다.

* font-style : 글자스타일이며 lighter,normal,bold,bolder,inherit등이 있다.
* font-weight: 글자 굵기


* **line-height** : 텍스트 상하높이를 의미하며, height와 line-height를 맞춰주면 한줄상의 모든 요소들이 수직정렬이 된다.

* text-decoration : 문자꾸미기며, none,underline(밑줄),overline(윗줄),line-through(취소선)이 있다.
* text-align : 문자정렬이며 left,right,center,justify(양쪽정렬)이 있다.
- justify는 우측 끝 부분을 깔끔하게 양쪽 정렬해주지만
- 줄의 내부간격이 뒤틀리므로 잘 사용하지 않는다.

* text-indent :문자 들여쓰기
* text-transform : 
> 대소문자변환으로 capitalize(대문자변환),uppercase(모두 대문자로),lowercase(모두 소문자로)
* letter-spacing : 자간으로 각 글자간의 간격을 의미
* word-spacing : 각 단어간의 간격
* vertical-align :박스내의 수직정렬이 아닌, 나란히 오는 인라인 요소간의 정렬(중요)
* text-align :줄바꿈 설정


* **white-space** :
> preline/nowrap/pre/pre-wrap;(띄어쓰기,줄바꿈 자동)

> p.item1 {  	white-space: nowrap; }
	
- 줄바꿈이 없는 형태 nowrap;


> p.item2 {  white-space: pre; }

- 줄바꿈,띄어쓰기,공백등 모든것이 보여지는 형태 pre
- 박스를 벗어나도 줄 바꿈이 일어나지 않음


> p.item3 {  white-space: pre-line; }


- 줄바꿈만 보여주며, 띄어쓰기는 무시하는 형태 pre-line
- 자동으로 줄바꿈이 된다.


> p.item4 {  white-space: pre-wrap;   }

- 줄바꿈과 띄어쓰기를 모두 보여주고 자동으로 줄바꿈이 되는 형태 pre-wrap


-

#css 배경스타일

* background-color : 배경색


> div {  background-color: #eee;  background-color: #efefef;   
background-color: rgb(230, 222, 120);   background-color: rgba(230, 22, 120, 0.4);  } <!--- 0.4는 투명도를 조절 --->  
  
 * background-image : 배경이미지

> div {  background-image: 
url('../images/icon.png');  }
- 이미지의 주소는 상대경로,절대경로 모두 사용이 가능함

* background-repeat : 배경이미지 반복

> div {  background-repeat: no-repeat;(반복하지않음)  
background-repeat: repeat;(가로세로바둑판형식)
background-repeat: repeat-x; (가로으로만 반복)  
background-repeat: repeat-y;  (세로으로만 반복)}
 

* background-position : 배경이미지 위치

> div {  background-position: 50% 16px;   
background-position: center(x축,y축모두사용) bottom;  }

- 삽입된 이미지의 좌표를 정해줌
- 두개의값을 받으며 각각 x축 y축 값을 가집니다.
- 각각은 양의값을 가질경우 x축은 우측 y축은 하단으로 이동
- %를 사용할 경우 이미지 사이지를 기준으로 움직임

* background-attachment : 배경이미지 고정

div {  background-attachment: local;(스크롤할때 이미지가 같이 움직임)
background-attachment: scroll; (요소에고정(스크룰에 무관)  
background-attachment: fixed;  }(background-position좌표를 웹페이지 화면 전체 기준으로 합니다)
 
-
#css 테두리 스타일
: 요소의 상하좌우 속성을 정의 할 떄, 순서는 시계방향으로 진행됩니다. 상단으로 시계방향으로 값을 정한다고 생각(시계방향 아주중요!!)

* border-width : 선굵기

> div {  border-width: 3px;   
border-top-width: 4px;   
border-width: 3px 4px 5px 6px;  
border-width: 5px 10px;  }

* border-style : 테두리를 구성하는 요소

> div {  border-style: solid;(실선)border-top-style: double;(이중선)border-style: solid double dashed dotted;  }(실선,이중선,바느질선,점선)

* border-color: 선색상

>div {  border-color: #aaa;  border-color: red blue green yellow;  } - 상하좌우(빨,파,초록,노랑)

* border 속성 속기법 

> div {  border: 1px(선굵기) solid(실선) red(빨강);  }

-

#css 박스 모델

![css 박스 모델](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQpDJOW2FeGe1aRYlIpv87r6UHs00STI7Fnmg_lhwx6cc4DQYFH)

- css요소는 박스형태를 이루며
- 박스는 콘텐츠,패딩(내부여백),보더(선),마진(외부여백) 4가지로 이루어진다

* 인라인 요소는 가로마진만 가질수 있다
* 인라인의 요소 길이/높이 지정은 불가능하다(중요!!)
* 블록 요소는 가로/세로마진을 모두 가진다.

* margin : 외부여백

> div {  margin-top: 10px;   
margin-bottom: 30px;  margin: 10px 0;  margin: 40px 20px 30px 50px;  }

- 마진이 겹칠경우 한쪽의 값에만 몰아주거나,padding을 활용하는 방안으로 맞추어야 한다.

> div {  width: 200px;  height: 50px;  padding: 10px;  border: 1px solid black;   
margin: 15px;box-sizing: border-box; - 요소의 width값에 맞추어 padding,border값을 제외한 새로운 width값이 새로적용}

- 콘텐츠의 길이는 200px-(10px+1px)x2 = 178px

-

#css 리스트 스타일
-

: css 리스트에 스타일을 지정한다.

* list-style-type 
ul {   list-style-type: disc;  
 list-style-type: circle;   
 list-style-type: square;  list-style-type: decimal;(숫자 1부터)(기억)
 list-style-type: lower-alpha;   
 list-style-type: upper-alpha;   
 list-style-type: lower-roman;   
 list-style-type: upper-roman;  }

* list-style-image : 리스트블릿에 이미지 지정

> ul {  list-style-image: url('images/mic.png');  }

* list-style-position : 리스트 블릿 위치 지정

> ul {  list-style-position: inside; (본문 내에 위치)
list-style-position: outside;(기본적인 리스트형태)}


* 리스트 스타일 속기법 

> ul {  list-style: square(블릿타입)
url('images/mic.png')(이미지사용할 경우)
inside; (블릿 위치내부)}

-


#css의 테이블 스타일

* border-collapse : 테두리 합치기

<pre>
<style>
table {  border-collapse: collapse; (테두리합치기)}  
tr, td {  border: 1px solid black;   
width: 30px;  text-align: center;  

</style>
<table>   
<tr>      <td>A</td>    <td>B</td>    <td>C</td></tr>   <tr>      <td>1</td>    <td>2</td>    <td>3</td></tr>   <tr>      <td>4</td>    <td>5</td>    <td>6</td></tr>   
</table>

</pre>



- border-spacing : 테두리간 띄우기


- empty-cells: hide;(셀내용 사라지기)


- table-layout : auto;(내용에따라 테이블 길이가 자동으로 조절된다)


- table-layout : fixed(내용이 길어도 셀길이가 일정하게 유지)



-

#실습 




<style>
html {
  font-size: 16px;
}

body {
  font-size: 0.9rem;
  margin :0;
  padding: 0.7rem;
  line-height: 1rem;
}

 a {
   color: #0275d8;
   text-decoration: none;
 }

ul, ol {
  -moz-padding-start: 20px;
  -webkit-padding-start: 20px;
  -khtml-padding-start: 20px;
  -o-padding-start: 20px;
  padding-start: 20px;
}

.modified-date {
  text-align: right;
}
.link-container {
  margin-bottom: 20px;
}

.link-container * {
  vertical-align: middle;
}

.link-container img.arrow {
  height:22px;
}

table.wiki-table {
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid black;
  border-collapse: collapse;
  margin-bottom: 20px;
}

table.wiki-table tr td {
  border: 1px solid #ccc;
  padding: 0.3125rem;
  text-align: center;
}

table.wiki-table tr td:nth-child(1) {
  background-color: #b38b6e;
  color: white;
}

table.wiki-table tr td:nth-child(2) {
    white-space: pre-line;
    color:black;

}

table.wiki-table tr:nth-child(1) td {
  background-color: white;
}
table.wiki-table tr:nth-child(2) td {
  background-color: red;
}
table.wiki-table tr:nth-child(3) td {
  background-color: blue;
}



table.wiki-table tr td img {
  max-width: 100%;
}

.table-of-contents {
  border: 1px solid #ccc;
  padding: 12px 20px 18px 20px;
  margin-left: 5px;
  font-size: 0.9rem;
  line-height: 1.2rem;

}

.blockquote {
  white-space: pre-line;
  line-height: 1.3rem;
  border: 2px dashed #ccc;
  border-left: 5px solid #71BC6D;
  padding: 10px;
}

p.blockquote {
  margin : 0;
}

</style>



<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>박보영-나무위키따라하기</title>
  <link rel="stylesheet" href="../css/wiki.css">
</head>
<body>
  <h1>박보영</h1>
  <p class="modified-date">최근 수정 시각 : 2017-05-09</p>
  <ul>
    <li>상위문서 : <a href="#">배우/한국</a></li>
  </ul>

  <div class="link-container">
    <img src="../image/arrow.png" alt="" class="arrow">
    <span>대법관 박보영에 대해서는</span>
    <a href="">박보영(대법관)</a>
    <span>문서를 참고하십시오.</span>
  </div>

  <table class="wiki-table">
    <tr>
      <td colspan="2">
        <img src="https://cdn.namuwikiusercontent.com/6b/6bb56f670ab9984cdc5af1d9cf98ed54ea8d940f2c3e1bf287ecb382c9615a69.jpg?e=1495520738&k=QEVr1boDI8qT5fRrV3Zb7A" alt="">
      </td>
      </tr>
      <tr>
        <td>이름</td>
        <td>박보영</td>
      </tr>
      <tr>
        <td>다른이름</td>
        <td>중국어</td>
      </tr>
  </table>

  <div class="table-of-contents">
    <h3>목차</h3>
    <ol>
      <li>소개</li>
      <li>활동</li>
      <li>생애
        <ol>
        <li>데뷔</li>
        <li>아역시절</li>
        <li>과속스캔들이라는 홈런</li>
      </ol>
    </li>
    <li>개인방송</li>
    </ol>
  </div>

  <h2>
    <a href="#">1.</a>
    <span>소개</span>
  </h2>

  <iframe width="560" height:"315" src=""https://www.youtube.com/embed/ot3wAWjCVsM" frameborder="0" allowfullscreen" frameborder="0"></iframe>

  <div class="blockquote">
    <p>
      2006년 데뷔부터 올해까지 총 일곱 편에서 교복을 입었다. 대부분 소녀 역할. 그러니까 <오 나의 귀신님>은 그녀가 데뷔한 지 10년 만에 맡은 첫 번째 (성인) 멜로드라마다. 처음 만난 ‘어른’ 박보영은 너무나 사랑스러운 여자였다. 남자의 시선이 아니라 엄마도, 여동생도, 누나도 그녀를 어여쁘게 여긴다. 뭐든 자연스러워서 그런 건 아닐지. 웃어도, 울어도, 유혹해도, 애교를 부려도 누군가를 흉내 내지 않는다. 온전히 자신으로 연기하는 배우.

누구보다 어른 같은 그녀에게 향수를 선물하고 싶다. (드라마에서 모기약을 뿌릴 때 어찌나 안타깝던지.) 프레드릭 말의 오 드 매그놀리아는 그녀에게 꼭 어울리는 향수다. 목련은 지구에서 최초로 핀, 피기 전이 오히려 예쁜, 봄의 시작을 알리는 꽃. 꽃말은 고귀함. 늘 첫 번째로 꼽는 동명의 영화 DVD도 함께 선물할 수 있으면 더욱 좋겠다. 예전부터 어른의 ‘아이템’을 시계라고 생각했다. 그녀가 시계를 찬 모습을 본 적이 거의 없다. 만약 그 이유가 지나치게 얇은 손목 때문이라면 빈티지 까르띠에 ‘미니어처’ 탱크를 권하고 싶다.

한편 학생 역할은 많이 했지만, 지금 대학 생활은 전혀 못하는 건 아닌지 걱정이다.
    </p>
  </div>
  

</body>
</html>


-

#css 화면표시 속성
* **display :block**

> span {  display: block;  } 
- span은 원래 인라인 속성이지만,
- display속성에 block값을 받으면
- 블럭속성을 지니게 된다.

* display : inline;

> div {
> display : inline;
> }
- 반대로 div블록속성을
- 인라인속성으로 바꾼다.

* display:inline-block

> span {
> display : inline-block;
> }
> 
> - 기본적으로 inline요소로 취급하지만
> - block요소와 같이 높이 상/하값을 가질수 있다
> - 인라인요소에 높이와 상/하 패딩,마진값을 줄때 사용한다.
> 
* display:none

>  div {
>	display:none;
>	}
> - 화면이 보이지 않는다(공간도 차지하지 않는다)
>
* visibility:hidden;(공간은 차지하나 투명해짐)
* visibility:visible;(공간도 차지하고 보임)

* 화면넘침 표시방법 : overflow

> div {  overflow: hidden; (넘친콘텐츠 숨기기)
overflow: visible;(영역밖으로 콘텐츠가 넘침)
overflow: auto;  (스크롤바가 자동생성) 
overflow: scroll;  (콘텐츠가 넘치지 않아도 스크롤바가 있음)}


* 실습

<style>

.block, .inline, .inline-block {
  /*그룹선택자 ,(쉼표)로 구분*/
  border-width: 3px;
  border-style:solid;
}

.block {
  border-color: red;
}

.inline {
  display: inline;
  border-color: blue;
}

.inline-block {
  display: inline-block;
  border-color : green;
  height: 150px;
}

</style>


<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>박보영-나무위키따라하기</title>
  <link rel="stylesheet" href="../css/wiki.css">
</head>
<body>
  

  <div style="border : 1px solid black;">
    <div class="block">Block</div>
    <div class="inline">
      Inline
    </div>
    <div class="inline-block">
      Inline block
      <div class="block">inner-block</div>
      <div class="block">inner-block</div>
      <div class="block">inner-block</div>
      </div>
      <div class="inline-block">
        inline-block
        <div class="block">inner-block</div>
        <div class="block">inner-block</div>
        <div class="block">inner-block</div>
      </div>
  </div>


</body>
</html>


----

#css float 속성
: float 띄우기 속성

* float 레이아웃


: css로 레이아웃을 구현할때는
float속성을 자주 사용합니다.

* 실습

		<style>
		.float-frame {  		width: 300px;   
		background-color: #eee;   
		border: 1px solid #ddd;   
		padding: 10px;  		} 

		.float-unit {
		width: 50px;
		background :#333;
		color :#fff;
		font-size:18px;
		font-weight:bold;
		text-align:center;
		padding : 15px 0;
		margin-right : 5px;
		float:left;

		</style>

		<div class="float-frame">  		<div class="float-unit">A</div>  
		<div class="float-unit">B</div>   
		<div class="float-unit">C</div>   
		<div class="float-unit">D</div>  		</div>




-



* float-unit각각의 항목들은 float-left;를 따라 잘 배열되었지만, 
* 부모요소의 height가 인식되지 않는다.

- 4가지 방법이 있다.
(1) 임의의 요소를 삽입하여 html파일에 br style="clear:both;" 삽입

(2) overflow를 이용하여 부모클래스에  overflow:auto:/overflow:hidden;을 삽입

(3) 부모에다 float:left;삽입

(4) 추천방법 : 가상선택자를 이용 

		.float-frame::after {		content: “ “;		display: block;		height: 0; clear: both;} 삽입
		- 부모요소에 .클래스명 :: after { 클래스를 준다.
		- html파일에 쓰고자하는 부모클래스 옆에 클래스명을 뒤에다 써준다.
	
-

#css 포지션
: css의 요소의 위치를 설정합니다.

* static : 기본값

* relative : static과 같은 순서로 배치되지만 top,right,bottom,left속성을 이용해 위치를 지정 가능

* fixed : 브라우저 창 기준으로 위치

* absolute : 자신과 가장 가까운 부모요소를 따라간다(없을경우 본문(body)기준

* 사용방법 : 부모요소에 position:relative를 설정한후 자식요소에 position:absolute를 사용하여 위치를 조정

-



#css 가운데 정렬

(1) width를 설정한후  
margin:0 auto; 설정

(2) 자주쓰는 방법 : 요소를 absolute 포지션으로 설정한뒤, 상단과 왼쪽에서 부모의 각각 50%만큼 이동후 자신의 높이와 가로의 -50%만큼을 다시 위와왼쪽으로 이동합니다.

	position:absolute;
	width:200px;
	height:100px;
	top:50%;
	left:50%;
	transform translate(-50%,-50%);
	
	
-

#시맨틱 태그

: 웹사이트에서 주로 사용하는 문서구조를 태그로 만든것 , 검색엔진, 시각장애인용 스크린리더,개발자에게 문서의 각 부분이 어떤역할을 하는지 쉽게 알려줌

* 종류
	* header : 머릿말
	* nav : 내비게이션링크,가로/세로형태의 **메뉴**에사용
	* section: 콘텐츠영역
	* article: 콘텐츠내용
	* aisde: 본문이외의내용
	* footer: 꼬릿말,제작자 및 저작권 정보 표시


-

#Sass

: css확장언어의 파일을 웹 브라우저에서 해석
할수 있는 css파일로 만들어주는 처리기

* sass의 기본구조

<pre>
div.container {   
padding: 	15px;   
margin: 0;   
>p#main-title {   
font-size: 16px;   
font-weight: bold;  
 }   

- css파일에서 : div.container p#main-title - sass에서 >(꺽쇠)는 자식선택자를 의미


>.fixed {   
position: fixed;   
bottom: 10px;   
right: 10px;  }  
(css의 경우 : div.container .fixed {


</pre>

* sass의 기본문법

(1) 주석은 // 혹은 */주석내용*/을 사용
(2) 중첩을 사용할 경우 > (자식선택자)

<pre>

.container {  
width: 1100px;  
margin: 0 auto;    > #page { width: 800px; border: 1px solid #eee; 
padding: 10px;  - css에서 .container #page를 의미
 - 자식선택자는 부모요소 상속받는 하나의 요소만 지정하기위한것 
 </pre>
 
(2) 부모 참조 선택자(&)

: 부모의 클래스나 아이디를 상속한다
 
<pre>
#page {  width: 800px;  border: 1px solid #eee;   
padding: 10px;  }   a {
	color : red;
	
	&:hover {
	color :blue;
	
	(&:hover는 css에서 #page a:hover를 의미)
	
	.link-container & {
	
	(link-container #page a를 의미)
		font-size :30px;
		}
	}
}

 
</pre>
 
(3) 선택자 상속

<pre>

.btn {  background-color: #cdcdcd;   
font-weight: bold;  color: white;  padding: 5px 20px;  } .btn-ok {  @extend .btn;  background-color: #d9edf7;   
} .btn-cancel {  @extend .btn;   
background-color: #bbb;  } 

@extend 상속을 사용하면 같은 속성에서
일부만 바뀐 요소를 쉽게 컨트롤 할수 있다.

css문서에서

.btn, .btn-ok, .btn-cancel {

(그룹선택자 쉼표(,)로 같은 요소를 공유)   
background-color: #cdcdcd;   
font-weight: bold;  color: white;  padding: 5px 20px;   } 

</pre>

(4) 대체 선택자(%)

: 대체선택자는 css구문에서 해석되지 않는다.

<pre>

%btn으로 대체선택자에 btn클래스일경우

css문서에서

.btn-ok, .btn-cancel {   

(.btn이 없어진것을 볼수있다.
즉 css파일에서는 해석되지 않는다.)

background-color: #cdcdcd;   
font-weight: bold;  color: white;  padding: 5px 20px;  

</pre>



(5) 변수는 '$'를 사용하여 지정한다


	$padding: 10px;   
	$bg-color: #ececec;  
	$title-font-weight: bold;
	

(6) sass파일 호출 : @import 'css파일';

<pre>
@import 'variables';  
  %btn {  background-color: 
$brand-primary;   
font-weight: bold;  color: white;  padding: 5px 20px;  }

- variables를 sass파일로 만들경우
- 앞에 dash를 붙여야한다
- -variables.scss로 저장해야 한다.
</pre>


-
