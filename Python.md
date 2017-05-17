#pyenv

: pyenv는 프로젝트 별로 파이썬 버전을 따로 관리할 수 있도록 도와주는 라이브러리, 각각에 설치된 라이브러리간 충돌이 일어날 수도 있다.

* virtualenv : 파이썬 개발환경을 프로젝트별로 분리해서 관리할 수 있게 해주는 라이브러리이다. pyenv와 다른점은, pyenv는 파이썬의 버전을 관리해주는 것이며, virtualenv는 파이썬 패키지 설치 환경을 따로 관리해 주는것이다. 

-

* pyenv global 3.5.3 : 로컬 global을 3.5.3버전으로 만듬
: system 파이썬을 건드리지 말자!

* pyenv local 버전명 가상환경이름


* pip는 맥의 brew와 같은 파이썬 패킷관리자이다.

* pip list --format=columns : pip list 정리

* 1~100은 객체에 메모리를 할당되있다. 주소가 고정으로 할당

* 참조는 가르킨다라는 뜻

* id(변수명) : 객체의 주소를 알수있다.

* 문자열은 대입을 지원하지 않는다.

* [1:3] : 1이상 3미만

* lux[::-1] : 역순으로

* sample_list2를 이용, 끝에서부터 처음까지(거꾸로) 2월씩 건너뛴 결과를 reverse_two_steps에 할당

: reverse_two_steps = sample_list2[::-2]

* extend대신 append를 사용하면?

: ['apple', 'banana', 'melon', ['red', 'green', 'blue']]

: 리스트안에 리스트가 들어가는경우

* fruits리스트의 1번째 위치에 'mango'를 추가해보자

: fruits.insert(0,'mango')

정렬하기 (sort, sorted)

sort는 리스트 자체를 정렬
sorted는 리스트의 정렬 복사본을 반환

: sorted(fruits)
: ['banana', 'melon', 'red']

리스트 복사 (copy)

copy함수
list함수
슬라이스 연산[:]

: fruits.copy()
: fruits_copy = fruits[:]

* 튜플,리스트 합치기

: ''.join

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
     
     
     
  