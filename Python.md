#pyenv

: pyenv는 프로젝트 별로 파이썬 버전을 따로 관리할 수 있도록 도와주는 라이브러리, 각각에 설치된 라이브러리간 충돌이 일어날 수도 있다.

* virtualenv : 파이썬 개발환경을 프로젝트별로 분리해서 관리할 수 있게 해주는 라이브러리이다. pyenv와 다른점은, pyenv는 파이썬의 버전을 관리해주는 것이며, virtualenv는 파이썬 패키지 설치 환경을 따로 관리해 주는것이다. 

* pyenv global 3.5.3 : 로컬 global을 3.5.3버전으로 만듬

 : system 파이썬을 건드리지 말자!

* 가상환경 생성
: pyenv virtualenv 버전명 폴더이름

* 가상환경을 해당 폴더에서 사용하도록 지정

`pyenv local 가상환경폴더이름`

-

#ipython

: 기본 파이썬 셀보다 다양한 기능을 사용할 수 있도록 해주는 셸을 제공

`pip install ipython`

* vi 단축키

```
shift + g : 가장 아래로
shift + a : 현재줄에서 가장 마지막으로
```

-

#pip

: 파이썬 패키지 관리자, pip는 맥의 brew와 같은 파이썬 패킷관리자이다.

* pip 리스트 정리

`pip list --format=columns`

* 관련명령어

```
pip list
pip uninstall
pip install
```

* requirements가 있을 경우 설치


`pip install -r requirements.txt`

* 에러해결

: ImportError: No module named 'packaging'

```
pip install --upgrade pip
```

: 실행후 다시 pip install

: Could not import setuptools which is required to install from a source distribution.

```
pip install -U setuptools
```

: 실행 후 다시 pip install


-

#연습문제/Q&A


lol딕셔너리의 champions키에 셋(Set)을 이용해 lux, ahri, ezreal을 할당하고,

lol['champions'] = {'lux','art','ezreal'}

lol딕셔너리의 items키에 아래의 각 항목들을 딕셔너리를 이용해 리스트로 할당한다.

lol['items'].append{'doranting' : 400'}

lol = { 'champions' : {'lux','ezreal','lux'},'items' : [{   },{  }, ] }

* vacation에 1에서 10중 아무 값이나 할당 후, if, elif, else문과 중첩 조건표현식을 사용해서 각각 vacation이 7이상이면 'Good', 5이상이면 'Normal', 그 이하면 'Bad'를 출력하는 코드를 짜본다.

: print('good') if vacation >= 7 else print('normal') if vacati
     ...: on >=5 else print('bad')
     
     
* 딕셔너리 키,값 순회할때에는 dict.itmes() 사용

: for key,value in champion_dict.items():
     print('champions : {}, description : {} '.format(key,value))


* 여러 시퀀스 동시순회 : zip

```

for index in range(len(fruits)):
     ...:     print(fruits[index])
     ...:     print(fruits[index])
     

> for fruit, color in zip(fruits, colors):
...   print('fruit:', fruit, ' color:', color)
이렇게 변환! 
     

```
* 만약 1~5중 짝수만 해당하는 리스트를 만들고 싶다면? 
     
[item for item in range(10) if item % 2 ==0]
     
     
(1) for문을 2개 중첩하여 (0,0), (0,1), (0,2), (0,3), (1,0), (1,1)..... (6,3)까지 출력되는 반복문을 구현한다.

```
for x in range(6):
	for y in range(3):
	print((x,y))
	
```

(2) 리스트 컴프리헨션을 중첩하여 위 결과를 똑같이 출력한다.

[(x,y) for x in range(6) for y i range(3)]


* 1, 2번의 반복문에서 튜플의 첫 번째 항목이 짝수일때만 출력하도록 조건을 변경한다.

[(x,y) for x in range(6) for y in srange(3) if x % 2 ==0]

* 1000에서 2000까지의 숫자 중, 홀수의 합을 구해본다.

(1)
sum = 0
i = 1000
while i <= 2000:
	if i % 2 ==0:
		sum += i
	i+=1
	
	
(2)sum([x for x in range(1000,2000) if % 2 ==1])
	
	
* 리스트 컴프리헨션을 사용하여 구구단 결과를 갖는 리스트를 만들고, 해당 리스트를 for문을 사용해 구구단 형태로 나오도록 출력해본다.각 단마다 한 번 더 줄바꿈을 넣어준다.

(1) 

l = ['{}+{} = {}'.format(x,y,x*y if y !=9 else '{}\n'.format(x*y)) for x in range(2,10) for y in range(2,10)]

(2)

l = ['{]+{} = {}'.format(x,y,x*y if y !=9 else str(x*y) + '\n') for x in range(2,10) for y in range(2,10)]


* 1에서 99까지의 정수 중, 7의 배수이거나 9의 배수인 정수인 리스트를 생성한다. 단, 7의 배수이며 9의 배수인 수는 한 번만 추가되어야 한다.

l2 : [x for x in range(100) if not x % 7 or not x % 9)]


```

a = 63
b = 81 
     
if not a % 7 or not b % 9:

: a를 나눈 나머지  0이되니까 컴퓨터는 1을 true로 받기때문에 false다

if not false 
	true
	
:b를 나눈 나머지  0이 되니까 컴퓨터트 1을 true로 받기때문에 not false = true

```
* 위치인자묶음, 키워드인자 묶음 동시사용

![](/Users/mac/projects/images/스크린샷 2017-05-18 오후 2.43.36.png)

* 'call func'를 출력하는 함수를 정의하고, 함수를 인자로 받아 실행하는 함수를 정의하여 첫 번째에 정의한 함수를 인자로 전달해 실행해보자.

: 함수로 인자를 전달

![](/Users/mac/projects/images/스크린샷 2017-05-18 오후 3.13.13.png)

* 함수자체를 return하는법

![](/Users/mac/projects/images/스크린샷 2017-05-18 오후 3.22.09.png)




* 결과값

![](/Users/mac/projects/images/스크린샷 2017-05-18 오후 3.21.46.png)

: print(f1) : call func
: print(type(f1)) : call func

: print(f2) : none : f2는  함수를 직접 실행했을경우, print_call_func에   return값을 주지 않았기 때문에 none이 뜬다.


* 딕셔너리에서 키를 반환하는 방법

 : locals().get('champion')
 
 * list는  id값이 바뀌지 않는다. 그내용물만 바꾸는것이다. 리스트 안의 각각의 내용에 메모리가 할당되어있기에 내용만 바뀌지 list id위치에 내용만 집어넣은것이다.

 * 람다함수는 : 한줄짜리로 만들어진 함수
 : 표현식이 return된다.
 
 : lamdba 매개변수 : 표현식 (인자)


* 클로저 

: 같이 개발할 경우, 함수명도 같고 매개변수도 같을때 구별하는 방법

파이썬 파일 하나는 하나의 모듈

* 데코레이터 : 함수에서 가장 가까운 것부터 실행한다.

* 제너레이터 : 파이썬의 시퀀스 데이터를 생성 : 


#파이썬 본문

##각종 용어

* 리터럴 : 변하지 않는 고정된 데이터 자체의 표현

* 5(정수형데이터)
* "fastcampus" (문자열데이터)
* 1.4937(부동소수점 데이터)

* 표현식 : 값을 의미하는 표현 또는 값을 반환하는 표현

```
sec = 60
365*24*sec #표현식
525600	# 정수 525600의 리터럴값
```
* 구문 (제어문) : 값의 의미를 지니지 않으며, 어떠한 목적을 수행하는 코드

```
for char in '안녕하세요': 
	- #구문(제어문)
	print(char)
	
```

* 변수

: 파이썬은 모든것(정수,문자열,함수)등이 객체(Object)로 이루어져 있다.

: **객체는 데이터의 형태로 결정해주는 타입**으로, 파이썬에서는 객체의 타입을 바꿀 수 없다.

: 프로그래머는 변수를 선언하는 형태로 컴퓨터의 메모리에 값을 할당하고 , 참조할수 있다.

> 프로그래밍언어에서는 같다의 의미는 '=='이다.

```
val = 100 # val라는 변수에 100이라는 정수를 할당(**100은 정수 type의 데이터형태인 객체를 의미**)
	print(val1)
100

```

: 변수는 단지 이름뿐이며, 그자체가 어떤 값을 가지고 있는것이 아니다. 100이라는 정수형 객체가 있는것 뿐이다.

* **어떠한 변수가 참조하고 있는 객체가 메모리상에서 가지고 잇는 고유의주소(id)를 출력하는 id내장함수를 사용할 경우 **

```
id(var1)
4520513888

```

: 변수는 언제던지 다른 객체를 가르킬 수 있다.

* 변수의 타입 확인

: 내장함수 type사용

```
>>> type(var1)
<class 'int'>

```
: 정수형 변수임을 할수 있다.

```
>>> type('안녕하세요')
<class 'str'>
```
: 문자열은  str(문자열)형임을 나타낸다.

: 클래스는 객체의 타입(정의)를 나타낸다.
`class`와 `type`은 거의 같은 의미로 사용

* 변수의 이름 제한

(1) 사용가능한 문자
: 소문자,대문자,숫자,언더스코어(_)

: 이름은 숫자로 시작못하고, 언더스코어로 시작하는 변수명은 일반적으로 사용안함

(2) 사용못하는 예약어

```
False, class, finally, is, return,
None, continue, for, lambda, try,
True, def, from, nonlocal, while,
and, del, global, not, with,
as, elif, if, or, yield,
assert, else, import, pass,
break, except, in, raise
```

* 변수의 입력과 출력

: 입력은 내장함수 `input` 사용

```
var = input('숫자를 입력해주세요 : ')
```

* 내장데이터 타입(숫자)
-
수학연산자
-
연산자|설명|예|결과
---|---|---|---
/|나누기|7/2|3.5
//|정수나누기|7//2|3
%|나머지|7%3|1
**|지수(승)|2**10|1024

* 진수(base)

: 기본적으로 숫자형 데이터는 10진수로 간주하나, 2진수,8진수,16진수를 표현할 수 있다.

(1) 2진수 : 0b 또는 0B로 시작
(2) 8진수 : 0o 또는 0O로 시작
(3) 16진수 : 0x 또는 0X로 시작

```
>>> 10
10
>>> 0b10
2
>>> 0o10
8
>>> 0x10
16
```

* 형변환

: 내장함수 int(정수),float(부동 소수점)을 사용

```
>>> int("3.14")
3
```

* 문자열

: 파이썬3에서는 문자열에서 기본적으로 유니코드를 사용하며 불변한다.

* 문자열 표현

: 작은 따음표 또는 큰 따음표

```
>>> '패스트캠퍼스 "웹 프로그래밍 스쿨"'
'패스트캠퍼스 "웹 프로그래밍 스쿨"'
```

: 세 개의 작은 따음표 또는 큰 따음표 (여러줄 표현)

```
>>> '''소환사 여러분.
... 
... 7.1 패치를 소개합니다.
... 
... 앞으로 있을 여러 번의 패치에 대해서는 차차...
... 하지만 그렇다고 이번 패치가 하향....
... 정의의 전장에서 승리를 기원합니다.'''
```

* 문자열 더하기

```
>>> notice = ''
>>> notice += '공지사항'
>>> notice += '(패치노트)'
>>> notice += ': 7.1 패치노트'
```
![](/Users/mac/projects/images/스크린샷 2017-05-18 오후 6.23.39.png)

* 형변환

: 내장함수 `str`을 이용

```
>>> str(147)
'147'
```
: 문자열을 제외한 객체를 print함수로 호출하면, 내부적으로 str함수를 사용한 결과를 나타내준다.

* 이스케이프 문자

: 특수문자 또는 특별한 역할을 하는 의미를 나타낸다

이스케이프문자|설명
---|---|
\a	|비프음 발생
\t	|탭(tab)
\n	|줄바꿈
\	|\(역슬래시) 입력
\'	|작은따옴표(') 입력
\"	|큰따옴표(") 입력

* 인덱스 연산

: 문자열에서 문자를 추출하기 위해 대괄호와 오프셋을 지정 할 수 있다.
**가장 왼쪽은 0이며, 가장 오른쪽은 -1로 시작한다.**

* 슬라이드 연산

* [:] : 처음부터 끝까지
* [start:] : start오프셋으로부터 마지막까지
* [:end] :  처음부터 end오프셋까지
* [start:end] 
* [::-1] : 역순으로
* [1:3] : 1이상 3미만


```
* 오프셋이란 : 동일 객체안에서 객체 처음부터 주어진요소나 지점까지의 변위차이를 나타내는 정수형이다

문자 A의 배열이 abcdef를 포함한다면 'c' 문자는 A 시작점에서 2의 오프셋을 지닌다고 할 수 있다.

```

* 길이 

: 내장함수 len을 사용

* 문자열 나누기(spilt)

: 문자열의 내장함수 spilt을 사용,spilt함수에 인자로 주어진 구분자를 기준으로 하나의 문자열을 리스트 형태로 변환해준다.

```
>>> girlsday = "민아,유라,소진,혜리"
>>> girlsday.split(',')
['민아', '유라', '소진', '혜리']

```

: split함수에 인자를 주지 않을 경우, 공백문자로 구분자를 사용한다.


* 문자열 결합(join)

:split함수와 반대의 역할을 한다.
:문자열 리스트를 하나의 문자열로 결합해준다.


```
girlsday_list = girlsday.split(',')
girlsday_str =','.join(girlsday_list)
print(girlsday_str)
민아, 유라, 소진, 혜리
```



 `','.join(해당리스트(문자열))`

* 대소문자 다루기

```

lux = 'lux, the Lady of Luminosity'

lux.capitalize()
'Lux, the lady of luminosity'

lux.title()
'Lux, The Lady Of Luminosity'

lux.upper() #대문자
'LUX, THE LADY OF LUMINOSITY'

lux.lower() #소문자
'lux, the lady of luminosity'

lux.swapcase()
'LUX, THE lADY OF lUMINOSITY'
```


* 문자열 포맷

변환타입	|설명
---|---
%s	|문자열
%d	|10진수
%x	|16진수
%o	|8진수
%f	|10진 부동소수점수
%e	|지수로 나타낸 부동소수점수
%g	|10진 부동소수점수 혹은 지수로 나타낸 부동소수점수
%%	|리터럴 %

`string % data`

```
'%d x %d : %d' % (3, 4, 12)
'3 x 4 : 12'
```

```
>>> d = 37
>>> f = 3.14
>>> s = 'Fastcampus'
>>> '%d %f %s' % (d, f, s)
'37 3.140000 Fastcampus'
>>> '%10d %10f %10s' % (d, f, s)
'        37   3.140000 Fastcampus'
```

* `{}.format(변수)`

```
dict = {'d': d, 'f': f, 's': s}
'{0[d]} {0[f]} {0[s]} {1}'.format(dict, 'WPS')

```

: fotmat에서 dict은 0번째 , wps는 1번째

* 타입 지정자 지정

```
>>> '{:d} {:f} {:s}'.format(d, f, s)
'37 3.140000 Fastcampus'
```
: d는 정수 , f는 부동소수점 , s는 문자열


: 필드길이 10, 우측정렬
```
'{:10d}'.format(d)
		37'
```
: 필드길이 10, 좌측정렬

```
'{:<10d}'.format(d)
```

: 필드길이 10 , 가운데 정렬

```
 '{:^10d}'.format(d)
```
: 필드길이 10, 가운데정렬 , 빈공간은 ~로 채움
```
'{:~^10d}'.format(d)
'~~~~37~~~~'
```

-

#시퀀스 타입

: 파이썬에 내장된 시퀀스 타입에는 문자열,리스트,튜플이 있다. 문자열은 인용부호(작은따음표,큰따음표)를 사용하며, 리스트는 대괄호([]), 튜플은 괄호()를 사용하여 나타낸다.

* 리스트

: 리스트는 순차적인 데이터를 나타나는데 유용하며, 문자열과는 달리 내부 항목을 변경할 수 있다.


* 리스트 **항목** 추가(append)

```
>>> sample_list.append('e')
>>> sample_list
['a', 'b', 'c', 'd', 'e']
```

* 리스트 병합(expend, +=)
: 리스트끼리 병합

```
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.extend(colors)
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']

```

* expend대신 append를 사용하면??
```
: ['apple', 'banana', 'melon', ['red', 'green', 'blue']]

: 리스트안에 리스트가 들어가는경우


* del 리스트[오픈셋(순서)]
`del fruits[0]`

* 값으로 리스트 항목 삭제(remove)
`fruits.remove('값')`

* 리스트 항목 추출 후 삭제(pop)

```
>>> fruits.pop()
>>> fruits.pop(-3)
```

* 값으로 리스트 항목 오프셋 찾기(index)

```
>>> fruits.index('red')
```

* fruits리스트의 1번째 위치에 'mango'를 추가해보자
```
: fruits.insert(0,'mango')
```

* 존재 여부 확인 (in)
```
>>> 'red' in fruits
True
```

* 값세기(count)

```
fruits.count('red')
```

* 정렬하기(sort,sorted)

: sort는 리스트 자체를 정렬
: sorted는 리스트 정렬 복사본을 반환

* 리스트 복사

: copy함수,list 함수, 슬라이스연산[:]

-

#튜플

: 리스트와 비슷하나 **내부항목의 삭제나 수정이 불가**

```
>>> colors = 'red', 
>>> fruits = 'apple', 'banana'
```
: 튜플의 요소가 1개일때는 요소의 뒤에 ,(쉼표)를 붙여야한다.

* 튜플 언패킹
```
>>> f1,f2 = fruits
```
* 튜플을 사용하는 이유는 : 리스트보다 적은 메모리를 사용하고, 정의후에는 변하지 않는 내부값이 있기 때문이다.

-

#딕셔너리

: key,value형태로 항목을 가지는 자료구조

* 항목 찾기/추가/변경[key]

```
>>> champion_dict['Lux']
'the Lady of Luminosity'
>>> champion_dict['Sona'] = 'Maven of the Strings'
>>> champion_dict['Lux'] = 'Demacia'
```

* 딕셔너리끼리의 결합 (update)

```
com_dict.update(item_dict)

```

* 삭제(del)

```
>>> del com_dict['Doran\'s Blade']
>>> del com_dict['Doran\'s Ring']
>>> del com_dict['Doran\'s Shield']
```

* 전체삭제(clear)
* in으로 키 검색 : true/false를 반환한다.
* keys() : 모든키 얻기
* values() : 모든 값 얻기
* **items() : 모든 키-값 얻기(튜플로변환)**

* 복사(copy())

-

#셋(set)

: 셋은 **키만 있는 딕셔너리와 같으며, 중복된 값이 존재 할 수 없다.**

```
>>> empty_set = set()
>>> champions = {'lux', 'ahri', 'ezreal'}
```

* 형변환

: 문자열,리스트,튜플,딕셔너리를 셋으로 변환할 수 있으며, 중복된 값이 사라진다.

* 집합연산

연산자 |	설명
---|---
|	|합집합(Union)
&	|교집합(Intersection)
-	|차집합(Difference)
^	|대칭차집합(Exclusive)
<=	|부분집합(Subset)
<	|진부분집합(Proper subset)
>=	|상위집합(Superset)
>	|진상위집합(Proper superset)


* 오름차순으로 정렬한 list반환하는법

* 딕셔너리의 키에 셋을 이용해 키를 할당하는법

* 딕셔너리의 items키에 각 항목들을 딕셔너리를 이용해 리스트를 할당한다.

-

#if,elif,else(조건문)

```
if 조건:
	조건이 참일 경우
else:
	조건이 거짓일 경우
	
```

```
if 조건1:
	조건1이 참일 경우
else:
	조건1이 거짓일 경우
	
	if 조건2:
		조건1은 거짓이나, 조건2는 참일 경우
	else:
		조건1,2가 모두 거짓일 경우
		
```
: elif로 줄여 쓸 수 있다.

```
if 조건1:
	조건1이 참일 경우
elif 조건2:
	조건1은 거짓이나, 조건2가 참일 경우
else:
	조건1,2가 모두 거짓일 경우
```

* 조건표현식

`참일경우 if 조건식 else 거짓일 경우`

* is_holiday에 True또는 False값을 할당한 후, if문과 조건표현식을 사용해서 각각 'Good'과 'Bad'를 출력하는 코드를 짜본다.


* 중첩 조건 표현식

```
# 조건이 2개일 경우

조건1이 참일경우 if 조건1 else 조건1은 거짓이나 조건2가 참일경우 if 조건2 else 조건1,2가 모두 거짓일 경우

```
: print('good') if vacation >= 7 else print('normal) if vacation >= 5 else print('bad')

#for문(조건에 따른 순회)

```
for 항목 in 순회가능(iterable)객체:
   <항목을 사용한 코드>
```
: 순회가능 객체에는 문자열,튜플,딕셔너리,셋이 있다.

* 키순회 : dict.keys(), 값순회 : dict.values()를 사용하고, 키,값을 모두 순회할때에는 dict.items()를 사용한다.

* 중첩

```
for 항목1 in 객체1:
	객체 1을 순회하며 실행할 코드
		for 항목2 in 객체2
		객체1 내부에서 객체2를 순회하며 실행할 코드
		
```

* 중단하기 : break

: 데이터를 순회하던 중, 특정 조건에서 순회를 멈추고 반복문을 빠져나갈때 사용한다.

```
for 항목 in 객체:
	(반복문을 중단하고 싶을때) break
```

* 건너뛰기(continue)

: 데이터를 순회하던중, 반복문을 중단하지 않고, 다음반복으로 건너뛸때 사용한다.

```
for 항목  in 객체;
	(현재의 반복을 그만두고 다음 반복으로 건너뛰고 싶을때) continue
	
* break확인(else)
: for문으로 데이터를 순회하던중, break문이 호출되지 않고, 반복문이 종료되면 else문이 실행된다.

```
for 항목 in 객체:
	pass
else:
	break가 한번도 호출되지 않았을 경우의 코드
	
```

* 여러 시퀀스 동시순회(zip) : zip으로 묶은 시퀀스들중, 가장 짧은 시쿼스가 완료되면 순회가 종료된다.

* zip을 사용하면 여러 시퀀스로 튜플을 만들수 있다.

list(zip(fruits,colors))

: zip으로 반환되는 것은 리스트가 아닌 zip클래스 형태의 객체이기에, 리스트 형태로 사용하려면 list()함수 사용 , dict()함수를 사용할 경우 딕셔너리 객체가 만들어진다.

* 숫자 시퀀스 생성(range)

:range()함수는 특정 범위의 숫자 스트림 데이터를 반환한다.

`range(start, stop, step)`

* while 반복문

: for문과 유사하나, while뒤의 조건이 참일 경우에 계속해서 반복한다.

```
while 조건:
	조건이 참일경우 실행
	조건이 거짓이 될경우까지 계속해서 반복
	
-
	
#컴프리헨션(comprehension)

: 함축 또는 내포(객체로부터 파이썬의 자료구조만드는 방법, 가독성과 사용성에서 이득을 얻음)

(1) 리스트 컴프리헨션

`[표현식 for 항목 in 객체]`
` [item for item in range(1:6)]'

* 만약 1~5중 짝수만 해당하는 리스트를 만들고 싶다면? 
     
 `[item for item in range(10) if item % 2 ==0]`
 
* 리스트 컴프리헨션의 중첩

``` 
for color in colors:
	for fruit in fruits:
	
[(color,fruit) for color in colors for fruit in fruits)]

```

* 셋 컴프리헨션

` {표현식 for 표현식 in 객체}`

* 제너레이터 컴프리헨션

`(표현식 for 표현식 in 객체)`

: 괄호로 되어있지만 튜플을 생성하지 않는다
(** 튜플은 컴프리헨션이 없다 **)

**: 제너레이터 객체는 순회가능하며, 리스트형태로 만들 수 있다.**

* 1000에서 2000까지의 숫자 중, 홀수의 합을 구해본다.

* 리스트 컴프리헨션을 사용하여 구구단 결과를 갖는 리스트를 만들고, 해당 리스트를 for문을 사용해 구구단 형태로 나오도록 출력해본다.
각 단마다 한 번 더 줄바꿈을 넣어줘라

: l = ['{}*{]={}'.format(x,y,x*y) if x in range(1,10) for y in range(1,10)]

: for문을 활용하여 구구단 형태로 줄바꿈을 넣어준다.?

* 1에서 99까지의 정수 중, 7의 배수이거나 9의 배수인 정수인 리스트를 생성한다. 단, 7의 배수이며 9의 배수인 수는 한 번만 추가되어야 한다.

l2 : [x for x in range(100) if not x % 7 or not x % 9)]


```

a = 63
b = 81 
     
if not a % 7 or not b % 9:

: a를 7로 나눈 나머지 0이되니까 컴퓨터는 1을 true로 받기때문에 false다.

if not false 
	true
	
:b를 나눈 나머지  0이 되니까 컴퓨터는  1을 true로 받기때문에 not false = true

```

-
#함수

: 반복적인 작업을 하는 코드를 재사용이 가능하게 정의해 놓은것.

```
def 함수명(매개면수):
	동작
	
```

```
def func():
	print('call func')
	
#실행시 'call func'를 print하는 함수 정의

>>> func
>>> <function func at 0x10dabf378>

# 함수 자체는 function객체를 참조하는 변수

>>> func() #함수를 실행시키기 위해 ()사용

```

* 리턴값이 있는 함수의 정의

```
>>> def return_true():
...   return True
... 
>>> return_true()
True
```

: 함수의 결과로 Bool값을 갖는 데이터를 리턴하여 if문이나 while문의 조건으로 사용 가능하다.

* 매개변수를 사용하는 함수 정의

```
>>> def print_arguments(something):
...   print(something)
... 
>>> print_arguments('ABC')
ABC
```

: 매개변수 : something, 실행인자 : ABC

* 매개변수와 인자의 차이

: 함수 내부에서 전달되어 온 변수는 매개변수라 불며,
함수를 호출할 떄 전달하는 변수를 인자로 부른다.

* **함수에서 리턴해주는 값이 없을 경우, 아무것도 없다는 뜻을 가진 `None` 객체를 얻는다.

* 위치인자 : 매개변수의 순서대로 인자를 전달하여 사용

```

>>> def student(name, age, gender):
...   return {'name': name, 'age': age, 'gender': gender}
... 
>>> student('hanyeong.lee', 30, 'male')
{'name': 'hanyeong.lee', 'age': 30, 'gender': 'male'}

```

* 키워드 인자 : 매개변수의 이름을 지정하여 인자로 전달하여 사용하는 경우

```
>>> student(age=30, name='hanyeong.lee', gender='male')
{'name': 'hanyeong.lee', 'age': 30, 'gender': 'male'}

```

: **위치인자와 키워드 인자를 동시에 쓴다면, 위치인자가 먼저 와야한다.**

* 기본 매개변수값 지정

: 인자가 제공되지 않을경우, 기본 매개변수로 사용할 값을 지정할 수 있다.

```
>>> def student(name, age, gender, cls='WPS'):
...   return {'name': name, 'age': age, 'gender': gender, 'class': cls}

```
: 매개변수로 cls (키워드인자) : 'wps')를 추가했다.

* **기본 매개변수값의 정의 시점 **

: 기본 매개변수의 값은 실행될 때마다 계산되지 않고,
**함수가 정의되는 시점에 계산되어 사용된다**

```
>>> def return_list(value, result=[]):
...   result.append(value)
...   return result
... 
>>> return_list('apple')
['apple']
>>> return_list('banana')
['apple', 'banana']


```

: 이것을 보면, result라는 매개변수를 추가했고
result라는 리스트에다 value리스트를 추가하고 result를 반환했을 경우

: 정의도니 시점에 계산되어 사용되기에 apple이라는 실행인자를 넣고, 그다음 바나나라는 실행인자를 넣었지만 누적되어 사과가 포함되어 있다.

* 함수가 실행되는 시점에 기본 매개변수값을 계산하기 이위해서는

```

>>> def return_list(value, result=None):
...   if result is None:
...     result = []
...   result.append(value)
...   return result
... 
>>> return_list('apple') #result실행인자가 없기에 none 
['apple']
>>> return_list('banana')
['banana']

```
: **result 키워드인자에 None이라는 값을 주고, result값을 None으로 지정하여 함수가  실행될때마다 리스트를 비운다. 매개변수 result**

* 실습

```
 1 def return_list(value, result=None):
  2     if result == None:
  3         result = []
  4     print('input value : {},result id : {}'.format(value, id(result)))
  5     result.append(value)
  6     return result
  7 
  
  
  # 기존에 result에 전달할 리스트 변수가 있던 경우
  
  
  9 l =['apple']
 10 new_l = return_list('banana',l)
 11 print(new_l)
 12 
 13 new_l2 = return_list('melon',new_l)
 14 print(new_l2)
 
 
 # return_list함수에 result매개변수로 사용할 list변수를 전달하지 않은 경우
 
 16 l2 = return_list('민아')
 17 print(l2)
 18 
 19 l3 = return_list('혜리')
 20 print(l3)
 21 


```


* 위치인자 묶음

:함수에 위치인자로 주어진 변수의 묶음은 `*매개변수명` 으로 사용할 수 있다.

```
def print_args(*args):
  print(args)
```

* 키워드인자 묶음

: 함수에 키워드인자로 주어진 변수의 묶음은 `**매개변수명**으로 사용할 수 있다. `**kwargs**
```
def print_kwargs(**kwargs):
  print(kwargs)
  ```
  
* 실습

```


  1 def print_args(*args, **kwargs):
  2     print(args)
  3     print(kwargs)
  4     
  5     
  6 def print_kwargs(**kwargs):
  7     print(kwargs)
  8     
  9 print_args('python', 'ruby','java', language='python', ide ='pycharm')
 10 print('')
 11 

* 실행결과

('python', 'ruby', 'java')
{'ide': 'pycharm', 'language': 'python'}


```
  
* **함수로 인자를 전달**

파이썬에서는 함수 역시 다른 객체와 동등하게 취급하므로, 함수에서 인자로 함수를 전달,실행,리턴하는 형태로 사용할 수 있다.

* 'call func'를 출력하는 함수를 정희하고, 함수를 인자로 받아 실행하는 함수를 정의하여 첫 번째 정의한 함수를 인자로 전달해 실행해보자 

```
1 def call_func():
  2         print('call func')
  3         
  4         
  5 f1 = call_func
  6 
  7 print(f1)
~               
```

: call_func라른 함수를 f1변수에 할당했다.
함수를 인자로 사용하여 print함수를 통하여 출력

* 내부 함수 

: 함수안에서 또다른 함수를 정의해 사용

* 실습



1. msg라는 매개변수를 갖는 함수를 정의, 해당 함수는 print(msg)를 실행하는 또 다른 함수를 생성해서 리턴해준다. 1의 함수명은return_print_function으로 작성

2. execute_another_function에서 위의 함수를 호출해서 만든 함수를 변수 f1에 할당 후, f1변수를 인자로 전달하여 실행

```
def print_call_func():
	print('call func')
	
def execute_another_function(another_function):
	another_function()
	
	
	
def return_print_function(msg):
    # 이 내부에서 새로운 함수를 생성 (def 어떤함수)하고
    # 아래에서 리턴해주셔야합니다
    def return_function():
        print(msg)

    return return_function
    # 내부함수자체를 리턴


print(return_print_function('test'))


f1 = return_print_function('이걸출력해주세요')

2번의 execte_another_function(f1)으로 실행

execute_another_function(f1)
	
```

#스코프(영역)

: 파이썬에서는 코드 작성시, 각 함수마다 독립적인(스코프(영역))을 가진다.

```
champion = 'Lux' # 전역영역(global scope)

def show_global_champion():
	# global_champion함수의 내부영역 local 	scope라고 한다.
	
    print('show_global_champion : {}'.format(champion))

show_global_champion()
print('print champion : {}'.format(champion))

```
```
champion = 'Lux'

def show_global_champion():
    print('show_global_champion : {}'.format(champion))

def change_global_champion():
    print('before change_global_champion : {}'.format(champion))
    champion = 'Ahri'
    print('after change_global_champion : {}'.format(champion))

show_global_champion()
change_global_champion()
```

: 결과를 실행해보면 change_global_champion 함수에서 오류가 발생하는것을 알 수 있다.

첫번째 코드에서는 champion의 변수가 함수의 로컬스코프에 존재하지 않기 때문에 글로벌 영역의 'lux'와 같은 해당변수를 찾아 출력된 반면에 change_global_champion은 로컬스코프에 또다른 변수가 존재하기 때문에, 할당하기 전인 변수를 사용한것을 판단하여 프로그램에서 오류를 발생시킨다.

: 각 영역에 해당하는 데이터는 `locals()` 함수를 사용해 확인 할 수 있으며, 전역 영역의 데이터들은 globals()함수를 사용해 확인 할 수 있다.

* 스코핑 룰

: 스코프는 지역, 전역외에도 내장(bulit-in)영역이 존재하며, 내장영역이 가장바깥, 그 내부에 전역, 그내부에 지역영역으로 분리도니다.

: 분리된 영역에서 외부영역에서는 내부영역의 데이터를 사용할 수 없지만, 내부 영역에서는 자신의 외부 영역에 있는 데이터를 참조할 수 있다.

* 내장함수와 내장영역

: `print`와 `dict`등 지정하지 않고, 사용했던 내장함수들은 위 스코핑 룰의 내장 스코프에 존재하는 함수들이다. 전역스코프의 `__builtin__` 변수에 할당되어 있으며, 전역 스코프에서는 해당 변수의 내부를 참조 할 수 있도록  파이썬이 시작될때 처리된다.

: 확인시  `dir`함수를 사용하며, `dir`함수는 해당 객체가 사용가능한 속성 및 함수들을 리스트 형태로 나타내준다.

* 로컬스코프에서 글로벌 스코프의 변수를 사용

```
champion = 'Lux'

def change_global_champion():

    global champion
    # global영역의 것을 쓰겠다고 선언!!!
    
    champion = 'Ahri'
    print('after change_global_champion : {}'.format(champion))

change_global_champion()
print('print global champion : {}'.format(champion))
```

: 이 경우, `change_global_champion` 함수는  champion 변수에 새로운 값을 대입한다. 만약 로컬 스코프에서 글로벌 스코프의 변수를 변경해야 한다면, 해당 변수가 로컬스코프에 생성되는 것이 아닌 **글로벌 영역에 이미 존재하는 값을 사용함을 명시해주어야 한다.**


* 내부함수의 로컬스코프(nonlocal)

```

champion = 'Lux'

def local1():
    champion = 'Ahri'
    print('local1 locals() : {}'.format(locals()))

    def local2():
        nonlocal champion
        champion = 'Ezreal'
        print('local2 locals() : {}'.format(locals()))
    local2()
    print('local1 locals() : {}'.format(locals()))

print('global locals() : {}'.format(locals()))
local1()

```
: 로컬스코프 내부에는 또 다른 로컬 스코프가 존재할 수 있다. 전역 스코프가 아닌, **자신의 바로 바깥 영역의 로컬스코프(자신보다 한단계 위의 로컬스코프)의 데이터를 참조한다.**

* global키워드와 인자전달의 차이

(1) 인자로 전달한 경우

```
global_level = 100
def level_add(value):
    value += 30
    print(value)

level_add(global_level)
print(global_level)

>>>130
>>>100

```

: 인자로 전달할 경우, 같은 객체를 가르키는 글로벌 변수 global_level과 매개변수 value가 존재한다. 이때 매개변수인 value의 값을 변경하는 것은 global_level에 영향을 주지 않는다.

(2) global키워드를 사용한 경우

```
global_level = 100
def level_add():
    global global_level
    global_level += 30
    print(global_level)

level_add()
print(global_level)

>>> 130
>>> 130
```
* 실습(리스트 변수가 할당될 경우)


```
def return_list(value, result=None):
    if result == None:
        result = []
    print('input value: {}, result id: {}'.format(value, id(result)))
    result.append(value)
    return result


# 기존에 result에 전달할 리스트 변수가 있던 경우
l = ['apple']
new_l = return_list('banana', l)
print(new_l)

new_l2 = return_list('melon', new_l)
print(new_l2)

# return_list함수에 result매개변수로 사용할 list변수를 전달하지 않은 경우
l2 = return_list('민아')
print(l2)

l3 = return_list('혜리')
print(l3)

```
![](/Users/mac/projects/images/스크린샷 2017-05-21 오후 12.53.02.png)

: result매개변수로 사용할 list변수를 주지 않았을경우 result ==None이면 result = []으로 처리했기에 리셋될때마다 id값도 달라지고 내용도 reset된다.

* 람다함수

: 한 줄 짜리 표현식으로 이루어지며, 반복문이나 조건문등이 제어문등은 포함될 수 없다. 또한, 함수이지만 정의/호출이 나누어져 있지 않으며, 표현식 자체가 바로 호출된다.

`lambda <매개변수> : <표현식>`

```
import string
>>> for char in string.ascii_lowercase:
...   if char > 'i':
...     print(char.upper())
...   else:
...     print(char)
```


```
for char i string.ascii_lowercase:
	print((lambda x : x.upper() if x > 'i' else x)(char))

```
* 클로져(Closure)

: 함수가 정의된 환경을 말하며, 파이썬 파일이 여러개인 경우 각 파일은 하나의 `모듈` 역할을 하고 각 모듈은 독립적인 환경을 가진다. 각 독립된 환경은 각자의 영영을 전역영역으로 사용한다.

closure/module_a.py
```
level = 100
def print_level():
    print(level)
    
```
closure/module_p.py
```
import module_a
level = 50
def print_level():
    print(level)
    
module_a.print_level()
print_level()
```
: python module_b.py로 module_b를 실행한다. 함수의 전역 영역은 해당 함수가 정의된 모듈 전역영역으로, 전역변수는 모듈의 영역에 영향을 받는다.

* 내부함수의 클로져

```
>>> level = 0
>>> def outter():
...   level = 50
...   def inner():
...     nonlocal level
...     level += 3
...     print(level)
...   return inner
...
>>> func = outter()
```

: 위의 경우, outter 함수는  inner함수를 반환하여 결과를 func 전역변수에 할당한다. inner함수의 호출 결과가 아닌 함수 자체를 반환하기 때문에 func변수는 실행할 수 있는 함수 객체이다. 이 때 inner함수가 사용하는 level변수는 nonlocal키워드를 사용했기 때문에 outter에 새로 정의된 지역변수 level을 사용한다. 반복적으로 func함수를 실행하면, inner의 외부(outter)에 존재하는 level변수의 값을 증가시키고 print 시키기때문에 값은 계속해서 증가한다.

* func2를 만든다면??

```
level = 0

def outer():
    level = 50
    
    def inner():
        nonlocal level
        level += 3
        print(level)
    return inner

f = outer()
f()
f()
f()

f2 = outer()
f2()
f2()

print(level)

>>>53
>>>56
>>>59
>>>53
>>>56
>>>0

```

: 이경우는 func2라는 변수에 outer()함수를 할당했더니 처음부터 시작해서 누적된것을 볼 수 있다. nonlocal level을 썼기에 outer()함수의 내부 로컬영역에 있는 level = 50이 값부터 바뀌기에 global 영역에 있는 level은 0이 된다.


* 데코레이터(decorator)

함수를 받아 다른 함수를 반환하는 함수, 예를 들면, 기존에 존재하던 함수를 바꾸지 않고, 전달된 인자를 보기위한 디버깅 `print` 함수를 추가한다던가 하는 기능을 할 수 있다.

(1) 기능을 추가할 함수를 인자로 받음
(2) 데코레이터 자체에 추가할 기능을 함수로 정의
(3) 인자로 받은 함수를 데코레이터 내부에서 적절히 호출
(4) 위 2가지를 행하는 내부함수를 변환

* 실습

```
def double(f):
    def inner_f(*args, **kwargs):
        result = f(*args, **kwargs)
        return result * 2
    return inner_f


def plus(f):
    def inner_f(*args, **kwargs):
        result = f(*args, **kwargs)
        return result + 10
    return inner_f


@double
@plus
def multi(n):
    '제곱을 반환'
    return n * n

result = multi(3)
print(result)

>>> 38
```

: multi(n) 함수에 가까운 plus함수 내부를 돌고 그후에 double함수를 돈다. **또한 데코레이터는 여러개를 가질 수 있으며, 함수에서 가장 가까운 것 부터 실행한다.**

* 제너레이터(generator)

: 제너레이터는 함수의 파이썬 시퀀스데이터를 사용하는데 생성, 시퀀스전체 데이터를 가지고 있는것이 아닌, 어떠한 루틴만을 가지고 있는것이다. 메모리를 적게 쓸 수 있다. 제너레이터는 마지막에 호출한 위치(항목)을 기억하고 있으며, 한 번 순회할 때 마다 그 다음값을 반환한다. 제너레이터는 함수를 통해 만들어지며, 함수 내부의 반복문에서  `yield`키워드를 사용하면 제너레이터가 된다.

```
def range_gen(num):
    i = 0
    while i < num:
        yield i
        i += 1

        
gen = range_gen(10)
print(gen)
print(type(gen))

# gen.__next__()
# gen.__next__()
# gen.__next__()
# gen.__next__()
# gen.__next__()

# for item in gen:
#     print(item)
    
i = 0
while i < 8:
    print(gen.__next__())
    i += 1
    
```
: 함수 내부에 `yield`키워드가 사용되어, 제너레이터 함수가 되었으며, 함수를 실행하면 제너레이터 객체를 반환한다. `yield`부분에서 멈춘 제너레이터 객체를 순회하기 위해서는  `__next__()`함수를 실행해준다.


* 실습

(1) 한개 또는 두개의 숫자 인자를 전달받아, 하나가 오면 제곱, 두개의 숫자를 받으면 두수의 곱을 반환해주는 함수를 정의하고 사용

```
(1)

def number(args1,args2 =None):
	if args2:
		return args1 * args2
		
	if args1:
		return args1 * args1
		
result = number(1)
print(result)
result1 = number(1,3)
print(result1)

>>> 1
>>> 3
(2)

def number(*args):
	if len(args) > 1:
		return args[0] * args[1]
		
	elif len(args) == 1:
		return args[0] * args[0]
	else:
		raise Exception('raise arguments')
		
result = number(2)	
print(result)
result1 = number(3,4)
print(result1)

>>> 4
>>> 12
```

(2) 위치인자 묶음을 매개변수로 가지며, 위치인자가 몇개 전달되었는지를 print하고 개수를 리턴해주는 함수를 정의하고 사용

```
 def number(*args):
     count = len(args)ㅑ
     print('위치인자가 {}개 전달되었습니다.'.format(len(args))
     return count
     
```

(3) 람다함수와 리스트 컴프리헨션을 사용해 한 줄로 구구단의 결과를 갖는 리스트를 생성해본다.

```
(1)

list = [(lambda a,b : '{}*{}={}'.format(a,b,a*b)(x,y) for x in range(2,10) for y in range(1,10)


(2) 
list = []
for x in range(2,10);
	for y in range(1,10):
		list2.append((lambda a,b : '{}*{} ={}'.format(a,b,a*b))(x,y))
		
```

#알고리즘 실습

* 순차검색

(1) 문자열과 키 문자 1개를 받는 함수 구현
(2) while문을 이용하여, 문자열에서 키문자가 존재하는 index위치를 검사 후 해당 index를 리턴
(3) 찾지 못했을 경우 0을 리턴

```
(1) 

def search(source, key):
	index = 0
	while index < len(source)
		if key == sourse[index]
			return index
		index += 1
	return -1

: while문을 사용하는 경우, index 순서를 0으로 할당한후 한개씩 증가시키며, source리스트의 길이가 초과되지 않는다면, 우리가 찾고자 하는 key와 source의 리스트[순서]의 값과 같으면 index 순서를 반환하고 아니면 -1로 리턴해라. while문은 가정이 참일경우 실행된다.

(2) 

def search1(source,key):
	index = 0
	for index in range(len(source):
		if key == source[index]:
			return index
		index += 1
	return -1
	
: for 문을 사용할 경우, 리스트의 길이 범위에서 index가 있고, key가 리스트의 어떤순서의 값과 같다면 index(순서) 반환
	
(3)

def search2(source,key):
	for index,char in enumerate(source)
	if char == key:
		return index
	return -1
	
list = 'lux, the lady of luminosity'
r1 = search(source,'x')
r2 = search1(source,'x')
r3 = search2(source,'x')

: 3번의 경우, 순서와 문자 2가지 변수를 enumerate함수를 사용하였다(순서와 문자가 같이 for문을 돌기에, index(순서)에 해당하는 문자가 같이 묶여있다 그러므로  char가 찾고자하는 key와 같다면 index(순서)를 반환하게 만들수 있다.
```

* 선택정렬(selection sort)

(1) [9, 1, 6, 8, 4, 3, 2, 0, 5, 7] 를 정렬한다.

(2) 정렬과정
 a. 리스트중 최소값을 검색
 b. 그 값을 맨 앞의 값과 교체(패스)
 c. 나머지 리스트에 위의 과정을 반복
 
 (3) 해결방법
 a. 알고리즘 진행과정 그려보기
 b. 의사코드 작성
 c. 실제코드 작성
 
 패스| 	테이블|	최솟값
 ---|---|---
0	|[9,1,6,8,4,3,2,0]	|0
1	|[0,1,6,8,4,3,2,9]	|1
2	|[0,1,6,8,4,3,2,9]	|2
3	|[0,1,2,8,4,3,6,9]	|3
4	|[0,1,2,3,4,8,6,9]	|4
5	|[0,1,2,3,4,8,6,9]	|6
6	|[0,1,2,3,4,6,8,9]	|8


* 의사코드

for i = o to n:
a[i]부터 a[n-1]까지 차례로 비교하여 가장작은 값이 a[j]에 있다고 할때,a[i]와 a[j]의 값을 서로 맞바꾼다.

```
def selectionsort(list):
	length = len(list)
	for i in range(length-1):
		indexmin = i
		for j in range(i+1, length):
			if list[indexmin] > list[j]:
				indexmin = j
			list[i], list[indexmin] = list[indexmin],list[j]
	return list
		
```
		
	
  
	