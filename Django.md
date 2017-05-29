# Django

* 장고에서 함수(view)는 컨트롤러이다.

* 컨트롤러는 특정정보를 데이터베이스에서 찾을 수 있는 곳

* pyenv 가상환경을 새로 설정해줘야한다.

(1) 가상환경생성 : `pyenv virtualenv '버전명' '폴더이름'`

(2) 가상환경을 해당 폴더에서 사용하도록 지정

`pyenv local '가상환경폴더이름`

`pip install --upgrade pip`

: `pip`가 최신 버전인지 확인


`pip install django~=1.10.0`

`pip install django`

: `장고` 설치


* pycharm에서 가상환경을 적용해줘야 한다.

![](/Users/mac/projects/images/스크린샷 2017-05-25 오후 6.19.15.png)

![](/Users/mac/projects/images/스크린샷 2017-05-25 오후 6.19.25.png)

* 루트 스스로 설정
![](/Users/mac/projects/images/스크린샷 2017-05-25 오후 6.19.57.png)

-


* SSH KEYS 설정하는 법

(1) cd ~ : 상위 디렉토리로 가기

(2) mkdir .ssh : 디렉토리만들기

(3) cd .ssh : 디렉토리 들어가기

(4) ssh키 생성 명령어 : `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

(5) 계속 엔터

![](/Users/mac/projects/images/스크린샷 2017-05-26 오후 3.57.48.png)

(6) ssh폴더안의 `l` 명령어 실행 : `id_rsa`, `id_rsa.pub`생성되어있는지 확인

(7) `cat id_rsa.pub`를 실행하여, 내용을 복사

![](/Users/mac/projects/images/스크린샷 2017-05-26 오후 4.00.37.png)

: key값에 붙여넣기

-

![](/Users/mac/projects/images/스크린샷 2017-05-26 오후 4.01.33.png) 

: 새 디렉토리를 만들고 git 주소를 ssh로 선택

(9) git설정

`git remote add origin git주소`

`git push -u origin master`


(10) 

`django-admin startproject mysite` 명령어 실행

![](/Users/mac/projects/images/스크린샷 2017-05-26 오후 4.19.21.png)

: tree 생성

: mysite가 2개니까, 상단 폴더이름 `django_app`로 변경

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 6.37.07.png)


(11) `gitigmore.io` 들어가서 gitignore 파일을 만들기


![](/Users/mac/projects/images/스크린샷 2017-05-26 오후 4.26.00.png)

: 이 파일들의 내용을 장고걸스 안에 .gitingore파일을 열어서  붙여넣기 해주고,

![](/Users/mac/projects/images/스크린샷 2017-05-26 오후 4.29.57.png)

: 2개 더 추가한다.

(12) ./manage.py runserver실행

![](/Users/mac/projects/images/스크린샷 2017-05-26 오후 4.35.52.png)

(13) 127.0.0.1:8000을 웹 주소창에다 검색

![](/Users/mac/projects/images/스크린샷 2017-05-26 오후 4.38.10.png)

: 컨트롤 c : 끄기


(14) 어플리케이션 만들기

: 프로젝트 내부에 별도의 애플리케이션을 만들기,

`./manage.py startapp blog`

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 1.04.56.png)

: `blog`디렉토리가 생성되고, 여러파일도 같이 들어있다.

```
djangogirls
    ├── mysite
    |       __init__.py
    |       settings.py
    |       urls.py
    |       wsgi.py
    ├── manage.py
    └── blog
        ├── migrations
        |       __init__.py
        ├── __init__.py
        ├── admin.py
        ├── models.py
        ├── tests.py
        └── views.py
        
```

(15) 애플리케이션을 생성한 후, 장고에 사용해야 한다고 알려줘야 한다.

:`mysite/settings.py`에 들어가기

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 1.07.43.png)

: `INSTALLED_APPS`를 열어, 바로 위에 `blog`를 추가한다.



(16) 모든 `model`객체는 `blog/models.py`파일에 선언하여 모델을 만든다. `blog/models.py` 파일을 열어서 안의 모든 내용을 삭제한후 아래 코드를 추가


![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 12.14.35.png)

`django_app :source loot설정` : 헷갈리지않게 동작하도록!

`class post(models.Model)`은 모델을 정의하는 코드, `post`는 모델의 이름,`models`은 post가 장고모델임을 의미, 이 코드 때문에, 장고는 `post`가 데이터베이스에 저장되어야 한다고 알게 된다.

: `models.CharField` : 글자수가 제한된 텍스트를 정의할 때 사용한다. 글 제목같이 짧은 문자열 정보를 저장할 때 사용

: `models.TextField` : 글자수에 제한이 없는 긴 텍스트를 위한 속성(블로그 컨텐츠)

: `models.DateTimeField` : 날짜와 시간을 의미

: `models.ForeignKey` : 다른 모델에 대한 링크를 의미

`def publist(self):`는 `publish`라는 메서드다. `__str__`메서드를 호출하면, `return self.title`는 post 모델의 제목 텍스트(string)을 얻게된다.

(17) 데이터베이스에 모델을 위한 테이블 만들기

: 데이터베이스에 우리의 새모델 `post` 모델을 추가한다.

`./manage.py makemigrations`

: 장고는 데이터베이스에 지금 반영 할 수 있도록, 마이그레이션 파일을 준비해두었다.

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 1.17.41.png)

`./manage.py migrate`
: 명령실행으로, 실제 데이터베이스에 모델 추가를 반영

(18) 장고관리자

: 방금 막 모델링한 글들을 장고관리자에서 추가하거나 수정,삭제 할 수 있다. `blog/admin.py`를 실행하여 내용을 바꾼다.

```
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

: 이렇게 내용을 추가

(19) 데이터베이스 실행
`./manage.py runserver`


: 주소창에 127.0.0.1:8000/admin을 입력

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 1.24.54.png)

: 로그인 하기 위해서는 모든 권한을 가지는 슈퍼사용자(superuser)를 생성해야 한다.

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 12.40.10.png)

: 데이터베이스 아이디와 비밀번호 생성

: 새로운 커맨드을 킨후, `./manage.py createsuperuser`를 실행

: 다시 주소창에 127.0.0.1:8000/admin을 입력하여, 로그인

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 1.27.55.png)

(20) SQLITE 설치후, 파일경로에 따라 데이터베이스 열기

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 1.29.03.png)


![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 12.30.26.png)

: blog_post가 생성된것을 볼 수 있다.

: 데이터보기

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 1.29.57.png)

: 블로그포스트에 데이버베이스가 생성된것을 볼 수 있다.

(21) 장고 URLS

: URL은 네트워크 상에서 자원이 어디 있는지 알려주는 규약 웹사이트 

: 메인요청을 다 받을 수 있는 url

`url(r'^$', view.post_list)`

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 2.30.21.png)

: 작성한 각 view(컨트롤러)는 HttpResponse의 인스턴스화, 채우기 및 리턴을 담당

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 2.46.22.png)

: view.py에 이렇게 내용을 수정한다. `render함수 사용`

* 정규표현식

`^post/(\d+)/$`

: ^post/ : url이(오른쪽부터) `post/`로 시작합니다.

: (\d+) : 숫자(한 개 이상)가 있습니다. 이 숫자로 조회하고 싶은게시글을 찾을 수 있다.

: `/$` : url 마지막이 `/`로 끝납니다.

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 2.42.38.png)

: django_app폴더안에 templates폴더안에 blog폴더안에`post_list.html`을 만든다.

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.00.04.png)

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.00.36.png)

: `settings.py`안에 BASE_DIR은  `/Users/mac/projects/django/djangogirls`의미

정정 : `TEMPLATE_DIR = os.path.join(BASE_DIR,'templates')`

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.20.17.png)

: `TEMPLATE_DIR,`추가

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.10.05.png)

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.21.27.png)

`os.path.join(BASE_DIR,templates)`

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.22.19.png)

* 장고 ORM과 쿼리셋

장고쉘
`./manage.py shell`

쿼리셋  `Post.objects.all()`

: 쿼리셋은 전달받은 모델의 **객체목록**으로, **쿼리셋은 데이터베이스로부터 데이터를 읽고,필터를 걸거나, 정렬을 할 수 있다.**

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.38.52.png)

`from blog.models import Post`


: `Post`모델을 `blog.models`에서 불러와서 모든글을 출력

`pl = Post.objects.all()`
`print(pl.query)`

: 작성자로서 `User`사용자 모델의 인스턴스를 가져와 전달해줘야 한다.

`from django.contrib.auth.models import User`


![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.39.51.png)

`user = User.objects.get(id=1)`

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.42.44.png)

* 데이터베이스에 새로운 포스트 생성하기


`Post.objects.create(title = 'Text Title', text = 'Test text', author=user)`

* 필터링하기

: 쿼리셋의 중요한 기능은 데이터를 필터링하는 것이다. 

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.46.39.png)

: 모든 글 중에서,제목(title)에  `title`이라는 글자가 들어간 글을 뽑아서 보고 싶으면??

`Post.objects.filter(title__contains='title')`



![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.48.05.png)

: 게시글 목록을 볼 수 있다. 게시일(published_date)로 과거에 작성한 글을 필터링하면 목록을 불러올 수 있다.


`from django.utils import timezone`

`Post.objects.filter(published_date__lte=timezone.now())`


: 파이썬 콘솔에서 추가한 게시물은 보이지 않기에, 게시하려는 게시물의 인스턴스를 얻어야 한다.

`post = Post.objects.get(title="Test title")`

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.49.14.png)

`post.publish()`

: **`publish`메서드를 사용해서 게시한다.**(`publish`메서드에는 `self.published_date = timenow()` 게시된 날짜는 현재시간과 같다는 속성이 정의되어있음.

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.50.32.png)

`Post.objects.filter(published_date__lte=timezone.now())
[<Post : Test title>]`


* 데이터 전달

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.15.18.png)

: {{ title }} : 템플릿 파일 안에 중괄호 2개는 변수로 할당

: context = context
(키워드인자)

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.15.46.png)


![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.20.43.png)

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.25.35.png)

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.26.34.png)

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.24.51.png)

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.28.59.png)

: `{% for post in posts %}`와 `{% endfor %}` 사이에 넣은 모든 것은 목록의 모든 객체를 반환

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.38.27.png)

: 완성본 : `{{ title }},{{ post.published_date }}, {{ post.title }}, {{  post.text  }}`

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.41.37.png)
: Post의 published_date가 timezone.now()보다 작은 값을 가질때만 해당하도록 필터를 사용한다.

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 4.44.05.png)



