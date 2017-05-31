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


# 어플리케이션 만들기

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

# 데이터베이스에 모델을 위한 테이블 만들기

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

# 정규표현식

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

# 장고 ORM과 쿼리셋

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

작성자로서 `User`사용자 모델의 인스턴스를 가져와 전달해줘야 한다.

`user = User.objects.get(id=1)`

![](/Users/mac/projects/images/스크린샷 2017-05-29 오후 3.42.44.png)

# 데이터베이스에 새로운 포스트 생성하기

`Post.objects.create(title = 'Text Title', text = 'Test text', author=user)`

# 필터링하기

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


# 데이터 전달

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

`Post의 published_date가 timezone.now()보다 작은 값을 가질때만 해당하도록 필터를 사용한다.`

`context의 딕셔너리에 대한 값에 객체인(posts)를 할당해서 HTML파일에서 중괄호 
{{ title }}{{ post.title }}을 넣을수 있다.`


# 부트스트랩 적용하기


: 부트스트랩을 설치하고 압축을 푼 폴더를 `bootstrap`으로 이름명을 바꾸고, `django_app폴더` 안에 `static폴더`를 만들고, `bootstrap`으로 이름을 바꾼 파일을 `static폴더`안에 넣는다.


![](/Users/mac/projects/images/스크린샷 2017-05-30 오전 10.54.22.png)

:`{% load staticfiles %}`
: href ="`{% static 'bootstrap/css/bootstrap.css %}`"

: `{%`은 붙어 있어야한다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 8.51.19.png)

`STATIC_DIR = os.path.join(BASE_DIR,'static')` : STATIC_DIR이라는 변수에 django_app/static폴더의 경로를 할당


![](/Users/mac/projects/images/스크린샷 2017-05-30 오전 11.06.55.png)


`STATIC_URL = /'static'/` : 이 경로로 시작하는 url은 정적파일들의 위치에서 파일을 찾아 리턴


`STATICFILES_DIR = ( STATIC_DIR,)` : django_app/static경로를 `STATIC_DIR`이라는 객체에 할당했고, `STATICFILES_DIR`폴더는 `static_url`로 요청된 파일을 찾는데 사용 : `STATICLFIELS_DIR`는 스태틱 경로(STATIC_URL)를 한 곳에모아주는 역할을 한다.





: body문안에 

`<div class="container">내용</div>`으로 감싸준다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오전 11.15.10.png)



![](/Users/mac/projects/images/스크린샷 2017-05-30 오전 11.26.02.png)
![](/Users/mac/projects/images/스크린샷 2017-05-30 오전 11.18.52.png)
![](/Users/mac/projects/images/스크린샷 2017-05-30 오전 11.52.10.png)



# TEMPLATE 확장(post_detail 만들기)


![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.00.52.png)

`url(r'^post/\d+/$', views.post_detail),` : post_detail이라는 이름의 url의 정규표현식이다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.04.47.png)

: `post_detail_html`에서 for문 삭제



![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.21.00.png)


* 겹치는 부분 base.html만들기

(1) base.html

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.24.35.png)

```
{% block content %}
	- 내용 - 
{% endblock %}

```

(2) post_list.html

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.27.24.png)

```
{% extend blog/base.html %}

{% block content %}
	추가할 내용
{% enddblock %}
```

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.33.23.png)

```
{% extend blog/base.html %}

{% block content %}
	추가할 내용
{% enddblock %}
```



![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.38.48.png)

:`/post/{{ post.pk }}/` : post에 있는 title을 누르면 `post_detail.html파일`을 불러온다. post_detail.html은 `urls.py`에  `url(r'^post/(?P<pk>\d+)/$', views.post_detail, name='post_detail')`은 url이름을 `post_detail`로 주었다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.41.31.png)

`url(r'^$', views.post_list, name='post_list'),`

`url(r'^post/(?P<pk>\d+)/$', views.post_detail, name='post_detail')`

: **장고 탬플릿에서  url을 쓰기 위해서는, name명을 주어야 한다.**

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.44.33.png)

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 12.45.34.png)

`{% url 'post_detail' pk=post.pk %}`

: name에 대한 인자를 줘야 하기에 `pk=post.pk`를 넣어야 한다.
`def post_detail(request,pk):`에 매개변수 pk가 있어서 실행인자로 
`pk = post.pk`를 주어야한다.

# 장고폼(글 쓰기 화면 만들기)


![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.21.21.png)

: `urls.py` post_create라는 이름의 url에 정규표현식을 써준다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.24.55.png)

: `views.py`에 `post_create(request)`함수를 만들어준다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.36.31.png)

: `post_create.html`파일내의 내용

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.37.13.png)

`크로스 사이트간 요청위조 (CSRF)`: 로그인한상태에서 url과 똑같은 사이트를 만들어서 실제서버에 요청하는것, 사용자들에게 html파일을 조작해서 보여줌,사이트를 해킹해서 url요청을 바꿈 

`방안` : 특정`키에 대한 값`을 보내서 값이 돌아와야지만 반응함

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.44.09.png)

: `{% csrf_token %}`사용

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.40.58.png)

`get요청 : url에 조회(현재페이지 보여줌)`

`post요청 : 서버에 데이터를 변하게 하고 싶을때,
해당 request의 메서드에 따라 구분함`

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.45.21.png)

`input name='title' type='text'`로 변환

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.49.21.png)

`딕셔너리에서  name은 키로 변환됨 `

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.52.54.png)

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 2.58.05.png)

: 상단내용추가

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 3.00.38.png)

: `redirect` 작성이완료된 후 작성된 페이지로 되돌려준다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 3.04.59.png)

`posts = Post.objects.order_by('-created_date')`






# form요소 동적으로 만들기 

: 장고코드를 활용하기위해서 `blog안에 forms.py`를 생성

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 3.48.34.png)
`required`속성이 추가하면 내용이 써지기전까지는 넘어가지 않는다.
`class postCreateForm(forms.Form)`은 `forms(내장함수)`에서 `Form클래스`에 있는 내용을 호출하는것이다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 3.41.12.png)

: 모듈이나 함수의 경로를 호출하고자 하는 함수에 커서를 두고 `alt+enter`

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 6.36.43.png)






`def post_create(request):`
` if request.method == 'GET':` `get`요청을 받을떄
`form = PostCreateForm()` : `form`이라는 인스턴스안에 `forms.py`에서 만들어준 `PostCreateForm()`클래스를 할당

`context = { 'form' : form, } `:은 form이라는 인스턴스를 딕셔너리의 키에대한 값으로 할당함, 실행하는것은 `'form'`인 키값이다.

`return render(request, 'blog/post_create.html', context=context)`

: `render함수는`  `Returns a HttpResponse whose content is filled with the result of calling` : 즉 요청에 대한 결과값에 대한 내용에 대한 응답을 리턴하는 함수, 즉 post_create.html파일에 context에 속해있는 내용들을 가지고 요청에 응답하겠다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 8.26.52.png)

: `post_create.html`에서 `vims.py`에 있는 `context`에 할당해줬던 인스턴스(form = PostCreateForm())에 대한 키값은 `{{ field.label_tag }}` `{{ field }}`라고 표현할수 있다.

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 3.49.23.png)
: `required`속성이 추가하면 내용이 써지기전까지는 넘어가지 않는다.

# 폼 검증하기












![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 3.56.05.png)

: `post_create`의 함수의 뒷부분을 추가한다.
`elif request.method == 'POST'` : 요청의 방법이 POST라면,







: `form = PostCreateForm(request.POST)`는 request.POST를 매개변수로 하여 form이라는 인스턴스로 저장

`if form.is_valid():` : `#Form인스턴스`의 유효성을 검사하는 `is_valid()메서드`
            
`return redirect('post_detail', pk=post.pk)` : 유효성 검사를 통과하지 못했을경우 error가 담긴 form을 이용해 기존페이지를 보여줌
        
* 유효성검사(max_length = 10이라고 클래스안에 설정)


![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 3.56.50.png)




* 박스만들기

![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 4.29.38.png)

: `forms.py`에서 `PostCreateForm`클래스를 생성할시 title이라는 인스턴스, text라는 인스턴스안에 상단의 내용을 넣어준다.




# 폼 수정하기


![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 4.43.42.png)

: `url.py`에서 url안의 정규표현식 변경
`url(r'^post/(?P<pk>\d+)/modify/$', views.post_modify, name='post_modify')`

: 'post_modify'라는 이름, 상단의 정규표현식 url이 있다면, `views(장고의 컨트롤러)`에서 만든 `post_modify`라는 이름의 함수로 보내는것이다. 어떠한 url을 처리하는 곳은 `views.py`이다.


![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 4.55.51.png)


: `post`요청을 할 경우는, 어떠한 내용들을 바꾸기 위해 요청하는 것으로 `if request.method == POST:`문을 사용, 요청의 방식은 2가지로 `POST`와 `GET`요청이며, `request.method` 라는 형식을 사용한다.

: `views.py`에서 `post_modify`함수를 만들어준다. 매개변수는 `(request)`,`(pk)`를 선언한다. `pk`를 선언하는 이유는 수정할 내용의 페이지들에 할당한 pk값에 해당하기 때문이다.

`post(인스턴스) = Post.objects.get(pk=pk)`
: `models.py`에서 만든 `Post`라는 클래스에 있는 모든 값들중에 pk=pk에 해당하는 값을 가져와서 post(인스턴스)에 할당하겠다.

`data = request.POST` : post요청(request)(어떠한값을 바꾼다는 의미)가 올경우, 전달받은 데이터를 	`data`라는 인스턴스에 할당하겠다.

`title = data['title']`,`text = data['text']` : 전달받은 데이터의 인스턴스 `data`에서 `name`을 title,text라고 써준 값들을 사용한다.

`post.title = title`,`post.text = text` : `post.title/text`는 post요청을 한 데이터들의 title,text라는 이름의 값에 덮어씨우겠다.

`post.save` : DB에 업데이트 하는 save()메서드
`return redirect('post_detail', pk=post.pk)` : 기존 post인스턴스를 업데이트 한 후, 다시 글 상세화면으로 이동



![](/Users/mac/projects/images/스크린샷 2017-05-30 오후 5.14.00.png)

`post_modify`의 html파일은 수정해서 덮어줄 내용을 쓰는것이다.
`<button type ='submit', class = 'btn btn-primary btn-block'>내용</button>` : 부트스트랩에서 버튼형식 만드는 방법


`<input_type = 'text', name = 'title', value = '{{ post.title }}'>`에서 value의 값에 `{{ post.title }}`넣어준다.

# HTML 폼양식

(1) 폼(Form)양식

`<FORM ACTION="URL" METHOD=POST NAME="FORM_NAME">`

:폼태그의 가장 중요한 속성은 action과 method이다. `action`은 폼이 전송되었을때, 넘어가는 페이지의 url을 나타내는것이며, `method`는 폼의 데이터를 전송하는 방법을 정의한 곳이다.

![](/Users/mac/projects/images/스크린샷 2017-05-31 오후 5.30.08.png)

(2) INPUT양식

: 사용자가 입력할 수 있는 입력 필드를 만들때 사용한다.

`<INPUT TYPE="INPUT_TYPE" NAME="INPUT_NAME" VALUE="INPUT_VALUE">`

: `type`은 입력필드의 `모양`을 결정하는 가장 중요한 키워드, `name`은 입력필드를 구분하여 주는 이름(데이터베이스에 저장될때 `input type = 'name'`에서 `name`이 딕셔너리의 키값이 된다.)

(1) 일반 텍스트 상자

`<input type="text" name=textbox1 size="50" maxlength="80" value="일반 텍스트 상자">`

: 일반 텍스트 상자에서는 `size`로 상자의 크기를 결정, `maxlength`로 최대입력길이 설정, `value` : 초기값의 입력이 필요할 경우,

(2) 패스워드 상자

: 사용자가 텍스트를 입력하면 자동으로 (*)로 변환되어 입력하게 된다.

`<input type="password" name="password1" size="10" maxlength="10">`

(3) 체크박스

: 사용자의 선택여부를 나타내기 위한 상자를 나타낸다. 보통 YES/NO의 질문을 위해 사용하며, 체크박스라고 한다.

`<input type="checkbox" name="checkbox1" value="checkbox_value" checked>`

: 체크박스는 	`type`에 `checkbox`의 입력으로 구현된다. `name`은 체크박스의 이름을 나타내며, `value`는 체크박스가 가지는값을 의미한다. `checked`속성은 체크박스가 처음 실행될 때 체크되어 있는 상태로 나타낼때 사용

(4) 라디오 버튼

: 많은 옵션들중 하나를 선택할때 사용, 작은 원모양의 버튼이 나타나 사용자가 선택할 수 있도록 제공

`<input type="radio" name="radio1" value="radio_value" checked>`

: 여러개의 라디오 버튼을 사용할 때에는 `name`값은 동일하게 사용, `value`값을 다르게 입력하여 구분

(5) 이미지

: 선택된 이미지의 좌표값을 반환하고자 할때 사용
`<input type="image" name="image1" src="이미지경로" >`

(6) HIDDEN

: 브라우저에 의해 출력되지 않는 입력폼.

`<input type="hidden" name="hidden1" value="hidden_value">`

: input타입으로 서버에서 Request값으로 저장된 값은 전송 받지만, 사용자게는 보이지 않는 입력 폼이다. 주로 **서버에서 처리할 페이지로 변수나 데이터값을 전송할때 사용**

(7) 버튼

: 사용자가 클릭할 수 있는 버튼을 나타내고자 할때 사용

`<input type="button" name="button1" value="버튼">`

: 회색 버튼이 표시, `value`에 있는 값이 버튼의 `caption`문자로 나타난다.

(8) 서브밋

: 폼을 전송하는데 사용하는 버튼이다.

`<input type="submit" name="submit1" value="전송하기">`

: 입력필드에 입력되어 있는 값들을 `<form>`태그에서 정의된 action파일로전송하는 버튼, `등록버튼으로 사용 `

(9) 리셋

: 폼에 입력되어 있는 값들을 초기화하는 버튼

`<input type="reset" name="reset1" value="취소">`

: 주로 취소버튼으로 사용

# 폼 삭제기능

(1) post_detail.html

![](/Users/mac/projects/images/스크린샷 2017-05-31 오후 8.42.08.png)

(2) views.py

![](/Users/mac/projects/images/스크린샷 2017-05-31 오후 8.45.37.png)

(3) urls.py

![](/Users/mac/projects/images/스크린샷 2017-05-31 오후 8.46.10.png)


