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




-

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