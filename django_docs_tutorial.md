# `Django_docs_tutorial`


#django basic settings

* 장고에서 함수(view)는 컨트롤러이다.

* 컨트롤러는 특정 정보를 데이터베이스에서 찾을 수 있는곳

* pyenv 가상환경을 새로 설정해줘야한다.

(1) 가상환경생성 

`pyenv virtualenv '버전명' '폴더이름'`

(2) 가상환경을 해당 폴더에서 사용하도록 지정

`pyenv local '가상환경폴더이름`

`pip install --upgrade pip` : `pip`가 최신 버전인지 확인


`pip install django~=1.10.0` : `장고` 설치

`pip install django` : `장고` 설치


(3) pycharm에서 가상환경을 적용해줘야 한다.



(4) 새 디렉토리를 만들고 git 주소를 ssh로 선택

(5) git설정

`git remote add origin git주소`

`git push -u origin master`

(6) `pip freeze` : 현재 깔려있는 버전들을 다 보여준다. 프로젝트를 같이 할때, 지금 깔려있는 여러가지 버전을 보여줘야기 때문에 requirements.txt를 만든다.

`pip freeze`

`echo 'abc' > abc.txt`

`cat abc.txt`

`pip freeze > 
requirements.txt`

: 내가 프로젝트를 할때 사용한 버전을  `requirements.txt`에 추가한다.

`rm abc.txt`


(7) 이미 커밋한 내용을 수정

`git add requirements.txt`

`git commit --amend`

* git tag 달기 

`git tag -a part1 -m 'django_docs part1' `

`git push origin master`


(8) `django-admin startproject mysite` 명령어 실행

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
        
```

* `outer mysite` : 단순한 박스로, 자신이 원하는 이름으로 바꾸면 된다.

* `manage.py` : 장고프로젝트와 연결해주는 커맨드라인

* `inner mysite` : 프로젝트를 위한 파이썬 패키지모음

* `mysite/__init__.py` : mysite가 파이썬패키지로 간주하게 해주는 파일

* `mysite/settings.py` : 장고프로젝트의 기본세팅

* `mysite/urls.py` : 장고프로젝트의 URL선언

* `mysite/wsgi.py` : 프로젝트를 지원하는 WSGI 호환 웹서버 진입점

# creating the polls app

: `python manage.py startapp polls`

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

* `polls/views.py` : 들어온 요청(url)이  views(컨트롤러)에 들어오면, 컨트롤러에 만든 함수들을 이용하여 처리한후, 다시 response(응답)을 되돌려준다.


```
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

* `polls/urls.py`

```
from django.conf.urls import url

from . import views
#from polls import urls와 같은의미, `.`은 현재위치를 의미

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
```


* `mysite/urls.py`

```
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```
: `include` : 패키지명.파일명(문자열형태), /polls/로 시작하는 중복된 url패턴을 줄여주기 위해서 상단처럼 써준다.

```
from polls import urls as polls_urls

url(r'^polls/', include(polls_urls)),

```

: `polls`라는 패키지명이 바뀔수 있기에 별칭을 사용하여 polls_url로 할당


# creating models

: 두가지 모델 `Question` 과 `choice`모델을 만든다. 질문클래스안에 선택클래스를 포함해야한다.

`polls/models.py`

```

from django.db import models


class Question(models.Model):
    question_text(인스턴스) = models.CharField(컬럼)(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    # ForeginKey는 각각의 choice가 각각의 질문과 연계되게 만드는 함수
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

```


: `ForeignKey` : 각 선택에 대한 질문 클래스를 연결, `ForeignKey.on_delete` : `ForeignKey`가 참조한 객체가 삭제될때, null값으로 설정되기를 원할때

```
class Choice(models.Model):
    question = models.ForeignKey(Question, verbose_name ='해당질문', on_delete=models.CASCADE)
    
    choice_text = models.CharField('선택내용', max_length=200)
    votes = models.IntegerField('총 투표수', default=0)
```

: 각 인스턴스의 이름을 한글로 적어줌

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 12.48.00.png)

: `ForeignKey`때문에 `question_id`가 생긴것을 볼 수 있다. 질문에 해당하는 선택지를 보기위해서, 연결해준다. **관계설정**

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 12.51.07.png)


# database setup

* `TIEM_ZONE`설정

`mysite/settings`에 들어가서 
`TIME_ZONE = 'asia/seoul`추가

`mysite/settings`

```
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    #위 문장 추가
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

`./manage.py makemigrations`

: 장고는 데이터베이스에 지금 반영 할 수 있도록, 마이그레이션 파일을 준비해두었다. 마이그레이션은 변경사항을 저장해두는 방법이다.

`./manage.py migrate` : 애플리케이션을 생성한 후, 장고에 사용해야 한다고 알려줘야 한다.


```
 polls/migrations/0001_initial.py
    - Create model Choice
    - Create model Question
    - Add field question to choice
```


`python manage.py sqlmigrate polls 0001`

: squlmigrate는 django_migration(내가 만든 데이터베이스)들의 기능들을 가져와서 SQL에 되돌려주는 기능

`./manage.py migrate`

: 필요한 데이터베이스 테이블을 만들고 장고에다 **적용**한다.

# Playing with the API

`python manage.py shell`

: 파워쉘 실행

`from polls.models import Question, Choice`
: 우리가썼던 모델(클래스)들을 불러온다.

`Question.objects.all()` : `Question`이라는 이름의 클래스에 적은 모든 내용을 가져온다.

`from django.utils import timezone`

* 현재시각설정 : `mysite/settings`에 `TIME_ZONE = 'Asia/Seoul'`을 추가

`q = Question(question_text="What's new?", pub_date=timezone.now())`

: q라는 인스턴스에다 `Question`클래스에 담은 내용을 할당. 

: `pub_data` 에 `timezone.now()(현재시간)`을 할당

: 새 질문 만들기 

`q.save()` : 데이터베이스에 저장

`q.id` : 데이터베이스에서의 id값

`q.question_text` : `Question`클래스에서 만든 속성 `question_text`를 파이썬 문법을 통하여 `q(인스턴스).속성`으로 실행

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 2.22.09.png)

`q.pub_date`

`q(인스턴스).속성`으로 실행

`q.question_text = "What's up?"`

: 값을 바꾸기

`q.save()`

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 2.29.27.png)

`Question.objects.all()`
: `Question`클래스에서 만든 모든 객체를 가져온다. `<QuerySet [<Question: Question object>]>`으로 표시한 이유는 `str(문자열)메서드를 만들지 않아서 이다.`

`polls/models.py`


```
class Question(models.Model):
    def __str__(self):
        return self.question_text


class Choice(models.Model):

  def __str__(self):
        return self.choice_text
```
: 문자열메서드 추가

```
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
    # ...
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
        
```

: 발행한지 하루 이내의 게시시간의 질문들



* 장고 쉘 새로 시작

`Question.objects.filter(id=1)` : id(pk)값이 1인객체를 필터링해서 보여주기

`Question.objects.get(pk=1) 
` : pk값이 1인 Question클래스와 관련된 모든 객체를 가져오기

`Question.objects.filter(question_text__startswith='What')` : what으로 시작하는 질문 텍스트를 필터링해서 보여주기


```
from django.utils import timezone
current_year = timezone.now().year
Question.objects.get(pub_date__year=current_year) >> True
```
: 이번년도에 게시된 질문들을 얻기 `polls/models` 안의 `Question`클래스에서 만든 메서드 `was_published_recently`를 호출하여 하루이내에 게시된 글인지 확인함

```
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
>>> true
```
: pk=1인 질문클래스를 q라는 인스턴스에 할당하여, `인스턴스.메서드`실행

`q = Question.objects.get(pk=1)`
`q.choice_set.all()`

: 질문클래스를 q라는 인스턴스에 할당한후, q에 관련된 모든 선택을 보여라`choice_set.all`명령어(역참조)


```
# Create three choices.
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)
```

: 새로운 선택만들기(`choice_set.create`명렁어), 
`Choice`클래스에서 만든 속성 `choice_text`,`votes`에 값을 할당한다. `Choice.objects.create(question = q)`와 `choice_set.create`는 같은 의미이다.

`c.question`
: 아까 만든 Choice클래스의 모든속성을 c라는 인스턴스에 담은다음 `Choice`클래스속성중 하나인 `question`속성을 실행시킨다.`question`속성은 ForeginKey함수로 `Question`클래스와 연결되어 있다.

`q.choice_set.all()` : 아까 만든 q인스턴스에 들어있는 모든 쿼리셋(값)들을 보여주는 명령어

`q.choice_set.count()` : 만든 선택지(쿼리셋)의 갯수

`Choice.objects.filter(question__pub_date__year=current_year)`

: 게시일이 이번년도인 모든 선택지를 가져와라.

```
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
```

: 선택을 삭제,  q인스턴스(모든선택지를담은)에서 just hacking이라는 이름의 선택지를 필터링하여 실행하고, c라는 인스턴스에 담고, delete함수를 통하여 삭제한다.

# creating an admin user

`./manage.py createsuperuser`

# make the poll app

`polls/admin.py`


```
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```

`polls/views.py`

```
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

: `question %s = question question_id` = `detail(request=<HttpRequest object>,question_id ='34')`같은 의미이다.

`urls.py`

```
from django.conf.urls import url

from . import views

urlpatterns = [
    # ex: /polls/
    url(r'^$', views.index, name='index'),
    # ex: /polls/5/
    url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
    # ex: /polls/5/results/
    url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
    # ex: /polls/5/vote/
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```

`polls.views.py`

```
from django.http import HttpResponse

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([q.question_text for q in latest_question_list])
    return HttpResponse(output)
   
```
: 게시된 순서 5개의 질문을

: `', '.join([q.question_text for q in latest_question_list])` : 쉼표로 구분하여 리스트화한다.

# making templates

`polls/templates/polls/index.html`

```

{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}

```

`polls/views.py`



```
from django.http import HttpResponse
from django.template import loader

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))

```

* `render`함수를 사용하면 `loader`와 `httpResponse`를 사용할 필요가 없다.

```
from django.shortcuts import render

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```

# raising a 404 error

`polls/views.py`

```
from django.http import Http404
from django.shortcuts import render

from .models import Question
# ...
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```

: 404에러 수정

```
from django.shortcuts import get_object_or_404, render

from .models import Question
# ...
def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
```

: try-except구문과 raise 404error를 사용하지 않고, 상단의 구문 `get_object_or_404()`로 해결할 수 있다.

# making detail template

`polls/detail.html`

```
<h1>{{ question.question_text }}</h1>
<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}
</ul>

```

: 다양한속성에 접근할수 있도록 `.`을 사용한다. `views.py`에서 만든 `question`이라는 인스턴스.속성(`question_text`)을 실행, `question.choice_set.all`은 `Choice`클래스를 담은 모든 객체를 반환하여 파이썬 코드로 인터프리트하게 하는 명령어이다.

`polls/index.html`에서

`<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>`을

`<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>` 으로 바꾼다.
: urls.py에서 url정규표현식을 만들때 url의 name을 사용하여 편하게 쓸수 있다.

# namespacing URL names

: 수많은 앱을 만들수 있으므로, 해당 앱의 url함수들을 모아놔야 한다.

`polls/urls.py`

```
from django.conf.urls import url

from . import views

app_name = 'polls'
urlpatterns = [
    url(r'^$', views.index, name='index'),
    url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
    url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```
: `app_name = 'polls`을 추가

`polls/detail.html`

```
<h1>{{ question.question_text }}</h1>

{% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
{% csrf_token %}
{% for choice in question.choice_set.all %}
    <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}" />
    <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br />
{% endfor %}
<input type="submit" value="Vote" />
</form>
```

: 

(1) 폼(Form)양식

`<FORM ACTION="URL" METHOD=POST NAME="FORM_NAME">`

:폼태그의 가장 중요한 속성은 action과 method이다. `action`은 폼이 전송되었을때, 넘어가는 페이지의 url을 나타내는것이며, `method`는 폼의 데이터를 전송하는 방법을 정의한 곳이다.

:`{% url 'polls:vote' question.id %}` : 넘어가는 페이지 url(vote.html로 넘어감),`method="post"`은 이폼을 제출할때, 데이터서버쪽 데이터로 변경될때 **무조건 `post`을 써야한다. 서버쪽의 데이터가 변경될경우 다쓴다.**in

![](/Users/mac/projects/images/스크린샷 2017-05-31 오후 5.30.08.png)

(2) 라디오 버튼

: 많은 옵션들중 하나를 선택할때 사용, 작은 원모양의 버튼이 나타나 사용자가 선택할 수 있도록 제공

`<input type="radio" name="radio1" value="radio_value" checked>`
: 여러개의 라디오 버튼을 사용할 때에는 `name`값은 동일하게 사용, `value`값을 다르게 입력하여 구분,`id`값은 자바스크립트적용할때 필요

`id="choice{{ forloop.counter }}"` : id에 들어가는 값은 안써도 상관 없다. `label for`의 id값과 일치해주기 위한것이다. value값은 라디오 버튼을 구분하기 위해서 달아준다.

```
<input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}" />
    <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br />
```

: `{{ forloop.counter }}`은 카운트수를 누적하는 것이다. 버튼과 `choice_text`은 연결되어 있으므로, id값을 일치시켜준다.

`polls/views.py`

```
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect, HttpResponse
from django.urls import reverse

from .models import Choice, Question
# ...
def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice.",
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```

:`request.POST['choice']`는 고른 선택지의 ID값을 딕셔너리의 `키`이름으로 접근할 수 있도록 한다. 또한 `keyerror`가 선택지가 POST데이터로 제공되지 않으면 발생한다. 위의 코드에서 `keyerror`가 발생하게 하고, 
`return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice.",
        })`

: 다시 선택지가 주어지지 않았다면 에러메세지 질문폼을 다시 보여준다.

`return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))` : reverse함수는 `/polls/3/results/`와 같은 경로로 보내준다.

`polls/views.py`

```
from django.shortcuts import get_object_or_404, render


def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/results.html', {'question': question})
```

: `context = { 'question' : question' }`을  지우고,render함수에 `context`대신 `{'question' : question}`으로 바꾼것이다.

`templates/polls/result.html`

```
<h1>{{ question.question_text }}</h1>

<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }} -- {{ choice.votes }} vote{{ choice.votes|pluralize }}</li>
{% endfor %}
</ul>

<a href="{% url 'polls:detail' question.id %}">Vote again?</a>
```

: `{{ choice.choice_text }} - {{ choice.votes }}`는 `polls/models.py`에 만든 속성들이다.

# Use generic views : Less code is better

: 코드를 줄여쓰기 위해서 `generic views system`을 사용하기 위해 `poll app`을 바꾼다.

(1) URL설정을 바꾼다

(2) 필요없는 코드 삭제

(3) `django generic views`를 기초로한 새로운 `views`(컨트롤러)제시

* amend URLconf

`polls/views.py`

```
class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]


class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'


class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'


def vote(request, question_id):
    ... # same as above, no changes needed.
```

: 두가지 `generic views`를 사용한다 `ListView`와 `DetailView` 각각은 `model`속성이 필요하다. `question-id`를 `pk(primary key value)`로 바꾸어 사용한다. 

`DetailView`에서는 `detail`함수에서 만들어놓은 `question`이라는 인스턴스는 자동으로 생성되나,
`ListView`에서는  `index`함수에서 만들어 놓은 `latest_question_list`인스턴스는 자동으로 생성이 안되기에 `context_object_name`인스턴스에 할당한후 `get_queryset`메서드를 만들어 인스턴스에 넣을` 클래스.속성`을 되돌려줘야 한다.

# Customize your app's look and feel

`settings.py`에 있는 `INSTALLED_APPS`에 `django.contrib.staticfiles`추가(아니면 `STATIC_DIR`의 경로를 만들고, `STATICFILEDS_DIR`인스턴스를 만들어서 튜플로 안에 `STATIC_DIR`을 넣어 줄수도 있다.)

`polls/static/polls/style.css`

```
li a {
    color: green;
}
```

`polls/templates/polls/index.html`

```
{% load static %}

<link rel="stylesheet" type="text/css" href="{% static 'polls/style.css' %}" />
```
: `{% static %}은 템플렛 태그고, 템플릿 태그는 스태틱파일들의 절대경로를 가져온다.

# Adding a background-image

`polls/static/polls/style.css`

```
body {
    background: white url("images/background.gif") no-repeat right bottom;
}
```

# Customize the admin form

`polls/admin.py`

```
from django.contrib import admin

from .models import Question


class QuestionAdmin(admin.ModelAdmin):
    fields = ['pub_date', 'question_text']

admin.site.register(Question, QuestionAdmin)

```

: `fields`말 그대로 데이터베이스의 필드(제목열)을 추가하는 것이다. `pub_date`는 `Date published`라는 필드명으로, `question_text`는 `Question text`으로 변경된다.

`polls/admin.py`

```
from django.contrib import admin

from .models import Question


class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        (None,               {'fields': ['question_text']}),
        ('Date information', {'fields': ['pub_date']}),
    ]

admin.site.register(Question, QuestionAdmin)
```

: `fieldsets`은 각각의 튜플(리스트와 비슷하나 순서를 바꿀수 없다.)(소괄호로 이루어져있음)들의 모음이다. ???

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 8.25.25.png)

# Adding related objects

: 한질문에 대한 선택지가 많기에 선택지관리자페이지를 바꾸자

`polls/admin.py`

```
from django.contrib import admin

from .models import Choice, Question
# ...
admin.site.register(Choice)
```

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 8.28.50.png)
      
: form에서 `Quesiton`필드는 각 질문을 포함한 `select box`로 구성되어 있다. `ForeignKey`를 설정하여, 각 질문에 해당하는 선택지를 고르게 해놨으므로 저절로 생성된다.

`polls/admin.py`

```
from django.contrib import admin

from .models import Choice, Question


class ChoiceInline(admin.StackedInline):
    model = Choice
    extra = 3


class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        (None,               {'fields': ['question_text']}),
        ('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
    ]
    inlines = [ChoiceInline]

admin.site.register(Question, QuestionAdmin)
```

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 8.33.55.png)

: 질문에 대한 선택지가 한페이지에 들어갔다. 즉 `Question admin page`에서 `Choice 객체`를 편집하는 것이다.

* 너무 페이지의 길이가 길어지기에, 인라인요소를 추가 `polls/admin.py`

`class ChoiceInline(admin.TabularInline):`

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 8.37.20.png)

# Customize the admin change list

: 리스트 관리자를 변경해보자, 기본설정은 질문마다 str메서드로 인해 각각의 객체가 한줄씩 표현되어 있기에, 각각의 필드로 표현해보자(`list_display`)

`polls/admin.py`

```
class QuestionAdmin(admin.ModelAdmin):
    # ...
    list_display = ('question_text', 'pub_date', 'was_published_recently')
```

: 오류발생 ??????

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 9.52.49.png)

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 8.43.08.png)

`polls/models.py`

```
class Question(models.Model):
    # ...
    def was_published_recently(self):
        now = timezone.now()
        return now - datetime.timedelta(days=1) <= self.pub_date <= now
    was_published_recently.admin_order_field = 'pub_date'
    was_published_recently.boolean = True
    was_published_recently.short_description = 'Published recently?'
```

`return now - datetime.timedelta(days=1) <= self.pub_date <= now` : 현재시각으로부터 24시간 이내의 게시된 날짜표현??

`was_published_recently.boolean = True` : false와  true 두가지 값으로 이루어진 데이터타입으로 등괄호가 참인것을 의미 ????

```
0 AND 0 = 0
1 AND 0 = 0
1 AND 1 = 1
0 OR 0 = 0
0 OR 1 = 1
1 OR 1 = 1
```

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 9.09.19.png)

# Customizing your project's templates

:`mysite/settings.py`

```
TEMPLATES = [
    {
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        }
```

: `templates`의 경로를 설정


* 경로보기 : `python -c "import django; print(django.__path__)"`를 찾아서 `py 경로`를 실행하여,`django/contrib/admin/templates`경로에 있는 `base_site.html`을 찾아서 복사한후

* templates폴더안에 admin디렉토리를 만들고, admin/base_site.html만들고 덮어쓴다.

`templates/admin/base_site.html`

```
{% extends "admin/base.html" %}

{% block title %}{{ title }} | {{ site_title|default:_('Django site admin') }}{% endblock %}

{% block branding %}
<h1 id="site-name"><a href="{% url 'admin:index' %}">{{ site_header|default:_('Django administration') }}</a></h1>
{% endblock %}

{% block nav-global %}{% endblock %}
```

![](/Users/mac/projects/images/스크린샷 2017-06-01 오후 9.49.33.png)















    
    