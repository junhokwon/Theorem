
# Making instagram

![](/Users/mac/projects/images/스크린샷 2017-06-08 오후 4.37.01.png)

: `startproject 모듈명`에서 `모듈명`을 `config`로 바꾼다.

: 인스타그램에는 뉴스피드,팔로우기능,로그인기능,회원가입기능,사진업로드기능,댓글달기기능,좋아요누르기기능,친구찾기기능,태그달기,친구추천기능,알림기능,마이페이지(내가올린글,내 정보관리)

`회원가입모듈` : 로그인,회원가입,팔로우,친구찾기,친구추천,개인페이지(내가올린글,내 정보관리)

`글 관련모듈` : 뉴스피드,사진업로드,댓글달기,좋아요누르기,태그달기

`알림 관련모듈 ` : 팔로워의 글 등록 알림, 댓글알림


## User authentication

: 장고는 사용자 인증 시스템과 함께 제공
사용자계정,그룹,권한,쿠키기반 사용자세션(서버쪽에서 쿠키(해당세션값을 브라우저에 저장)를 남겨두는것), 사용자에게 특정 키값을 주고, 서버쪽에 어떤 세션을 생성하고 저장해두어 비교한다.

: 인증시스템은 사용자인증(유효한사용자인지)과 권한부여(유효한 사용자인경우 권한부여)를 모두 제공

`pluggable backend system` : 유저 id와 패스워드 제공, 

`sessionmiddlewarare` : 단순히 서버의 어떤 키값을 저장해놓는것

`authentificationdmiddleware` 응답마다 세션을 거쳐서, 로그인 인증을 유지시키는것


## using django objects

: 인증과 권한부여, 일반적으로 사이트와 상호작용하는 사람들을 대표,5가지 기본요소가 들어간다. 

` from django.contrib.auth.models import User`에서 가져다써라



## Customizing and Extending existing User model

`from django.contrib.auth.models import User`

: 장고가 기본적으로 제공하는 `User`와 연결

* 커스터마이징한 User을 쓰고 싶을땐,

 `config/settings.py`
 
 ```
 AUTH_USER_MODEL = 'member.User`
 ```
 
 : `member` `appname`에 있는 `User`을 쓰고 싶다고 가정,
 
 `member/models`
 
```
from django.contrib.auth.models import AbstractUser
 
class User(AbstractUser):
	pass	
```

`from django.contrib.auth.models import User`

`u = User.objects.create_user('lhy')`

: 장고모델안의 `User`를 만들기

`u.set_password('0904ss')`

: 장고모델안의`User`의 패스워드 변경


# model 세팅


`post/models.py`


```
from django.conf import settings
from django.db import models

class Post(models.Model):
	title = models.CharField(max_length=50)
	created_date = models.DateTimeField(auto_now_add=True)
	modified_date = models.DateTimeField(auto_now=True)
	author = models.ForeignKey(settings.AUTH_USER_MODEL)
	photo = models.ImageField(upload_to='post',blank=True)
	like_users = models.ManyToManyField(
	setting.AUTH_USER_MODEL,
	related_name = 'like_users',
	through = 'PostLike'
	)
	tags = models.ManyToManyField('Tag',blank=True)
	
	def add_comment(self,user,content)
	return self.comment_set.create(author=user,content=content)
	
	def add_tag(self,tag_name):
		tag,tag_created = Tag.objects.get_or_create(name=tag_name)
		if not self.tags.filter(id=tag.id).exists():
		self.tags.add(tag)
		
	@property
	def like_count(self):
		return self.like_users.count()
	


class PostLike(models.Model):
	post = models.ForeignKey(Post)
	user = models.ForeignKey(settings.AUTH_USER_MODEL)
	created_date = models.DateTimeField(auto_now_add=True)
	
class Comment(models.Model):
	Post = models.ForeignKey(Post)
	author = models.ForeignKey(settings.AUTH_USER_MODEL)
	content = models.TextField()
	created_date = models.DateTimeField(auto_now_add=True)
	modified_date = models.DateTimeField(auto_now=True)
	like_users = models.ManyToManyField(
	settings.AUTH_USER_MODEL
	related_name = 'like_comment',
	through = 'CommentLike',
	)
	
class CommentLike(models.Model):
	comment = models.ForeignKey(Comment)
	author = models.ForeignKey(settings.AUTH_USER_MODEL)
	created_date = models.DateTimeField(auto_now_add=True)
	
class Tag(models.Model):
	name = models.CharField(max_length=10)
	
	def __str__(self):
		return "tag ({})".format(self.name)
		

```
(1) `Post모델`

title,created_date,modified_date,author,photo,like_users(종아요기능),tags을 넣어야한다.

`photo = models.ImageField(upload_to='post',blank=True)`

: 이미지처리 프레임워크 (`pillow`)를 깔아야한다.

`brew install libtiff libjpeg webp little-cms2`

`pip install pillow`

: `upload_to`에는 `파일이름을 포함한 경로`나 `MEDIA_ROOT`다음에 올 경로의 이름을 만들어줘야한다. imagefield는 filefield을 상속받기에, file 업로드하는것과 비슷하다고 생각하면 된다. file업로드를 하기위해서는 `MEDIA_ROOT`를 설정해줘야 한다.


`config/settings.py`

```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR,'media')
```

: `MEDIA_URL` : url패턴  `/media/`로 오는 모든 패턴

`MEDIA_ROOT` : 파일을 저장할 경로 `django_app/media`까지이며, 그이후는  `upload_to = ?`에 따라 달라진다. 

`config/urls.py`

```
urlpatterns += static(
	prefix = settings.MEDIA_URL,
	document_root = settings.MEDIA_ROOT,)
```
: `urls.py`에서는 `url`을 요청받아서 `view.py`에 돌려주기 때문에, media파일을 제공하는 url패턴등록부분을 고쳐줘야 한다. static함수에 `첫번째 인자`로 `media file url('/media/') : url패턴` 두번째 인자로 `document_root`로 media file이 위치한 경로를 의미한다.

![](/Users/mac/projects/images/스크린샷 2017-06-13 오후 8.01.07.png)



* post 좋아요 기능


`like_users = models.ManyToManyField(
	User,
	related_name = 'like_posts',
	through = 'PostLike',
	)`
	
	
: `User`와 `Post`를 다대다관계로 맺어주고, `author = models.ForeignKey(User)`에서 `User`와 연결해준 관계가 하나 더 있으므로, `related_name`을 사용하여 역참조이름을 하나더 만든다. `through=중간자모델명`을 통해 중간자 모델을 선언한다.

`tags = models.ManyToManyField('Tag')` : `Post`와 `Tag`모델은 다대다 관계

* Post와 관련된 댓글 쓰기 

: `Post`와 `Comment`는 1 대 다 관계를 맺었다.

```

def add_comment(self,user,content):
	self.comment_set.create(author=user,content=content)
```

: `Post`클래스에서 참조하는것이기 때문에 역참조이름 `comment_set`과 `Comment`안에 있는 속성들을 사용하여 comment를 만들어준다.

	
* Post와 관련된 태그 쓰기

: `Tag`와 `Post`는 다 대 다 관계를 맺었다.


```
def add_tag(self,tag_name):
	tag,tag_created = Tag.objects.get_or_create(name=tag_name)
	if not self.tags.filter(id=tag.id).exists():
	self.tags.add(tag)
```
	
`get_or_create()`

: 가져올 값이 있으면 가져오고, 없으면 만들어서 가져오기, 가져올 값이 없을경우, 새로만들고, 튜플안의 두번째 요소가 `created =True`로 변환된다. 이미 존재할경우 `created=False` 두개의 인자 `tag`와 `tag_created`이며 `tag`안에 내용이 들어간다.

* `if not self.tags.filter(id=tag.id).exists()`

: `Post(self).tags(Post의 Tag인스턴스명).filter(id=tag.id).exist()`에서 `exists() 메서드`는, 최소한 하나의 레코드가 존재하는지 여부를 확인하여 알려준다. 현재 추가한 `(self,tag_name)`에서  분류조건 : `id=tag.id`라면 추가하지 말고, 없으면 `self.tags.add(tag)` =`Post.tags.add(tag)`



(2) `PostLike`모델

: 중간자 모델로써, `Post`와 `User`을 1대 다 관계로 맺어야 한다. `created_date` 넣어야한다. 중간자 모델이므로, `Post`와 `User`을 `PostLike`와 `ForeignKey`로 연결해줘야 한다.


(3) `Comment` 모델

:post,author,content,created_date,modified_date가 필요하다.

`post = models.ForeignKey(Post)`

: `Post`와 `Comment`는 1대 다 관계

`author = models.ForeignKey(User)`

: `Comment`와 `User`는 다대 1관계

* 댓글 좋아요 기능 추가

```
class Comment(models.Model):
like=users = models.ManyToManyField(
	settings.AUTH_USER_MODEL,
	related_name = 'like_comments',
	through = 'CommentLike`,
	
```

: `author`인스턴스에 외래키로 1대 다관계를 만들었으므로, 역참조할 이름이 하나더 필요하다 . `related_name=like_comments`
`through = 'CommentLike'`중간자 모델설정.

(4) `CommentLike`모델

중간자 모델 `CommentLike`을 설정

`Comment`와 `CommentLike`의 1 대 다 관계 설정

`User`와 `CommonLike`의 1 대 다 관계 설정


(5) `Tag`모델

: name이 필요하다.


* 중간자 모델 설정하기전 기존 데이터베이스의 값을 유지하고 싶다면,

`like_users = models.ManyToManyField(
        User,
        related_name ='like_posts',
        through = 'PostLike',
    )`
    
 : `through='PostLike`로 중간자 모델을 일단 넣어두고,
 
 ```
 class PostLike(models.Model):
    post = models.ForeignKey(Post)
    user = models.ForeignKey(User)
   
   class Meta:
   		db_table = 'post_post_like_users'
 ```
 
 : 중간자 모델에 `Meta`클래스를 이용하여, `db_table = post_post_like_users`로 테이블이름을 정해준다.
 
 `./manage.py makemigrations`
 
 `./manage.py migrate --fake`
 
 `--fake`를 통해 `PostLike`클래스의 테이블을 `post_post_like_users`테이블로 바꿔치기 한후,
 
 
 
```
 class PostLike(models.Model):
    post = models.ForeignKey(Post)
    user = models.ForeignKey(User)
    created_date = models.DateTimeField(auto_now_add=True)
    
```


: `created_date`를 넣어주고, `./ manage.py makemigrations`를 할 경우 `(1)`을 선택한후, 현재시간 `timezone.now()`을 넣어줘야한다.

![](/Users/mac/projects/images/스크린샷 2017-06-12 오후 12.45.45.png)




# admin 설정

![](/Users/mac/projects/images/스크린샷 2017-06-12 오후 3.42.41.png)

```
from django.contrib import admin

from .models import Post

class PostAdmin(admin.ModelAdmin):
	pass
admin.site.register(Post,PostAdmin)

```

: `ModelAdmin`안에는 많은 메서드가 저장되어 있다. 그곳에서는 `media`메서드가 저장되어 있기에, `Post`메서드에서 정의한 속성들 `title`,`Author`,`Photo`,`tags`들을 `admin`페이지에서 쓸 수 있다.

![](/Users/mac/projects/images/스크린샷 2017-06-13 오후 8.32.45.png)

# urls 설정

`config/urls.py`

```
urlpatterns = [
	url(r'^admin/', admin.site.urls,),
	url(r'^post/',include('post.urls'))
```

: 항상포함되는 url패턴 `post/`을 넣어주고, `include`함수를  사용하여 `(post.urls)` : `post`앱명의 `urls`을 포함하여 url패턴을 검사한다.

`post/urls.py`

```
from django.conf.urls import url

from . import views

app_name = 'post'
urlpattern = [
	ex) /post/
	url(r'^%', views.post_list, name='post_list'),
	ex /post/1
	url(r'^(?P<post_pk>\d+)/$', views.post_detail, name='post_detail'),
	ex /post/create
	url(r'^create/$', views.post_detail, name='post_detail'),
	ex /post/123124124~
	url(r'^.*/$', views.post_anyway, name='post_anyway'),
	ex) /post/1/modify
	url(r'^(?P<post_pk>\d+)/modify/$', views.post_modify, name='post_modify'),
	url(r'^(?P<post_pk>\d+)/delete/$',views.post_delete,name='post_delete'),





```

: `url(url패턴, views.클래스명, name = url네임스페이스)`


`django.conf.urls.url`안에 `def url(regex, view, kwargs=None, name=None):`

: 키워드인자로 줘야한다.`(?P<post_pk>\d+)` : 정규표현식을 그룹화하면 키워드인자가 된다.

# views 설정

```

User = get_user_model()

def post_list(request):
	posts = Post.objects.all()
	context = {
		'posts' : posts,
	return render(request, 'post/post_list.html',context)
	

def post_detail(request,post_pk):
	try:
		post = Post.objects.get(pk=post_pk)
	except Post.DoesNotExist as e:
	return HttpResponse('post not found,detail : {}'.format(e))
	return redirect('post:post_list')
	context = {
		'post' : post,
	return render(request,'post/post_detail.html',context)
	
def post_create(request):
	if request.method == 'POST:
		user = User.objects.first()
		post = Post.objects.create(
		author=user,
		photo=request.FILES['file'],
		)
		comment_string = request.POST.get('comment','')
		if comment_string:
			post.comment_set..create(
			author=user,
			content=comment_string,
			)
		return redirect('post:post_detail',post_pk = post.pk)
	else:
		return render(request,'post/post_create.html')
		
		
		
		
		
		
		
		
```

`User = get_user_model()` : 자동으로 django에서 인증에 사용되는 User모델클래스를 리턴

* `get_user_model()`

: User을 직접 참조하는 대신에, `django.contrib.auth.get_user_model()`을 사용한다. 이 메서드는 현재의 활동중인 `user model`이나 `custom user`을 리턴한다. `models.py`에 `User`대신에`settings.AUTH_USER_MODEL`로 바꿔준다.

## post_detail만들기

`post = Post.objects.get(pk=post_pk)`

: Model(DB)에서 post_pk에 해당하는 Post객체를 가져와 변수(인스턴스)에(post)할당, 모델매니저(objects)의 get메서드를 사용해서, 단 한개의 객체만 가져온다. get(키워드인자)메서드는 주어진인자에 맞는 객체를 찾지 못한다면 `DoesNotExist`오류를 발생한다.
예외처리를 해주기 위해서

```
try:
	post = Post.objects.get(pk=post_pk)
except Post.DoesNotExist as e:
```

: 이경우는  `/post/124214124`와 같이 아무숫자나 올수 있기때문에 404 에러가 발생한다. 

(1) 404 Notfound를 띄어준다.

`return HttpResponseNotFound('post not found , detail : {}'.format(e))`

(2) post_list view로 돌아간다.

(2)-1 `redirect`를 사용

`return redirect('post:post_list')`

* redirect()

`redirect(to,위치인자,키워드인자)`

: `redirect()`함수를 쓰는 경우는 몇가지가 있다.

(1) `Model.get_absolute_url()`에서 만들어진 url경로를 되돌려주고 싶을때,

`get_absolute_url`은 함수를 사용하여 , url 경로를 문자열로 나타낼 수 있다.

```
def get_absolute_url(self):
	return "/people/%i/" % self.id
```

(2) url에서 설정한 view 네임스페이스를 되돌려 주고 싶을때, 위치인자나 키워드 인자가 같이 들어갈 수 있다. `url(name=?)`

`return redirect('post:post_detail',post_pk=post.pk)`

(3) 하드코딩된 url경로일경우
`return redirect('/some/url/')`
`return redirect('https://~절대경로)`


(2)-2 `HttpResponseRedirect`를 사용

```
url = reverse('post:post_list')
return HttpResponseRedirect(url)
```
== `redirect('post:post_list')`


* render 대신 전체과정을 서술

`template = loader.get_template('post/post_detail.html')`

: 인자로 주어진 문자열값 ('post/post_detail.html')과 일치하는 템플릿이있는지 확인후 결과를 리턴 `django.template.backends.django.Template`의 결과를 template 인스턴스에 할당

`post = Post.objects.get(pk=post_pk)`의 `post`인스턴스는 context의 딕셔너리에 값으로 들어간다.

`context = { 'post' : post, }`


: context로 전달될 dict의 `키`는 템플릿에서 사용가능한 변수명이다.

`rendered_string = template.render(context=context,request=request)`

: template에 인자로 주어진 context,request를 render함수를 사용해서 해당 **template를 string으로 변환**

`return HttpResponse(rendered_string)`

: `request`에 대해 `response`를 돌려줄때는 `HttpResponse`나 `render`를 사용가능하다.

## post_create 만들기

`if request.method == 'POST'`
: `request.method` : 받아오는 요청 방식(데이터베이스에 어느정도 영향을 준다면 `POST`요청을 받아 처리한다)

`user = User.objects.first()`

: `get_user_model`을 이용해서 얻은 `User클래스(장고에서 인증에 사용하는 유저모델)에서 임의의 유저 한명을 가져온다. (처음 관리자 설정할때 만든 id명)

`post = Post.objects.create(
	author=user,`
	
: 새 Post객체를 생성하고 DB에 저장(create함수는 저장이 자동으로 된다)

	`photo=request.FILES['file']`
	
: request.FILES(딕셔너리이며,각각 `FileField또는 ImageField의 "키"를 포함하고 있다) =  **파일 업로드 방식**(파일을 가져와야한다)

`post/post_create.html`

```
<div>
	<form action="" method="POST" enctype="multipart/form-data">
		{% csrf_token %}
		<input type="file" name="file">
		<input type="text" name="comment" placeholder="댓글">
		<button type="submit">등록</button>
	</form>
</div>
```

: 파일선택창의 <input type=file, name="file>이라고 설정되어있다. 딕셔너리의 "키"명으로 `{file : ~~~~}로 저장되어 있다.

: `request.FILES`는 요청파일업로드방식이고, request.method가 POST, form속성중 파일전송을 하기 위해서는`enctype="multipart/form_data"`를 가져야 한다.

`photo = request.FILES['file']` : [키] ==값을 반환

`comment_string = request.POST.get('comment','')`

: `request.POST`는 딕셔너리이며, POST요청시 name이 `comment`인 input에서 전달한 값을 가져옴. `<input type="text" name="comment" placeholder="댓글">` 즉 `request.POST`자체가 dict(딕셔너리)이고 `dict.get(key, (default=None or 키가 존재하지 않을때의 value))` 즉 `request.POST.get('comment','')`

:  빈 문자열('')이나 `default = None` 모두 False으로 평가되므로, `False`인경우 `if`문이 실행되지 않고 리턴된다. `default_value`는 선택적이고, 빈 문자열을 넣었기에, `comment`키에 해당하는 값이 오지 않았을경우, 오류가 발생되지 않는다. 만약  `('comment',)`일경우, 댓글로 쓸내용이 전달되지 않았기에 예외처리를 `if not`을 써서 해줘야 한다.

* hasattr(object,name)

: 인자는 하나의 객체나 문자열이며, 문자열이 객체의 속성중 하나의 이름이라면, 결과는 `True`이고, 이름과 맞는 문자열이나 객체가 없다면, 결과는 `False`이다.

`if comment_string:`

: 댓글로 사용할 문자열이 전달된 경우, 위에서 생성한 post객체에 연결

```
post.comment_set.create(
	author=user,
	content=comment_string,
		)
```
or
```
Comment.objects.create(
	post=post,
	author=user,
	content=comment_string,
	)
```

`return redirect('post:post_detail',post_pk = post.pk)`

`else:(request.method=="GET")`일경우 

`return render(request,'post_post_create.html')`

		



	
# template 세팅


![](/Users/mac/projects/images/스크린샷 2017-06-14 오후 7.46.29.png)

: `common`폴더안에는 `base.html`이 있고, `include`폴더안에는 `post.html`이 있다.

: `include`폴더안에 `post.html`은 중복되는 부분에 모두 사용될 수 있는 `post.html`이다.

* `if`

```
{% if athlete_list %}
	number of athletes : {{ athlete_list|length }}

{% elif athlete_in_locker_room_list %}
	Athletes should be out of the locker room soon!
	
{% else %}
	no athletes.
{% endif %}
```

: `athlete_list`가 텅빈값이 아니라면,  운동선수들의 숫자는 `{{ athlete_list|length }}` 운동선수목록의 길이일것이다. 

: `if tags`는 `and` `or` `not`,`==`,`!=`,`<=`,`>=`,`in`,`not`,`is not`

: `if tag`에서 `filter`을 사용할 수 있다.

```
{% if messages|length >=100 %}
	you have lots of messages today!
{% endif %}

```

: `|(filter 조건)`



`post/post_list.html`

: 앞부분은 `base.html`에서 상속 

`{% extends 'common/base.html' %}`

: 중복되는 부분은 `{% block content %}`와 `{% endblock %}`안에 감싸준다.

`{% for post in posts %}`
`{% endfor %}`

: `posts`는 인스턴스로 `views.py`에서 만든 `post_list` 매서드에서 모든 Post목록을  posts라는 인스턴스에 담고`{'posts' : posts }`에 딕셔너리의 값으로 주고, 딕셔너리의 키값인 `posts`을  이용해 `html`파일에서 사용할 수 있다. `for`문을 돌리는 것은 반복적인 `post`를 리턴하겠다는 의미 

* `for`

```
{% for athlete in athlete_list %}
	<li>{{ athlete.name }}</li>
{% endfor %}
```

`{% for obj in list reversed %}` 

```
{% for x,y in points %}
	there is a point at {{ x }},{{ y }}
{% endfor %}
```

: 리스트들의 리스트를 순회할 필요가 있을경우, `unpacking`을 사용하면 된다.

```
{% for key,value in date.items %}
		{{ key }} : {{ value }}
{% endfor %}
```

: 딕셔너리의 아이템으로 접근할 필요가 있을경우, 



`{% include 'include/post.html' %}`

* `include`

: 템플릿을 읽고, 현재의 context에 렌더링해준다. 템플릿 안에 다른 템플릿을 `including` 하는 방식이다. 

ex) `Hello, Jonh!"`이라는 결과를 보여주고 싶다면, `person`객체는 `Jonh`이라고 셋팅하고 `greeting`객체는 `Hello`라고 셋팅한다. 

at `template`

`{% include "name_snippet.html" %}`

at `name_snippet.html` template

`{{ greeting }}, {{ person|default:"friend" }}!`

`{% include "name_snippet.html" with person='Jane' greeting='Hello' %}`

: 이렇게 인자를 전달하거나, 템플릿 자체를 삽입시킬 수도 있다.


# html 파일 만들기

* `static/css`

: static파일안에 css파일들을 넣어줘야 한다. static파일은 css,scss,images모음디렉토리들이 들어가야 한다. 또한 `settings.py`에서 경로를 지정해줘야 한다.

```
STATIC_DIR = os.path.join(BASE_DIR,'static')

STATIC_URL = '/static/'
STATICFILES_DIRS = [
	STATIC_DIR,
]
```

: `/static`으로 오는 url패턴의 모든 파일들을 `STATIC_DIR`의 경로 `django_app/static`으로 들어오게 설정

`include/base.html`

: base.html에서는 모든 템플릿에 중복되는 html의 요소를 다 넣는다.

`nav 상단바`

![](/Users/mac/projects/images/스크린샷 2017-06-14 오후 8.48.38.png)

```
{% load static %}

<link rel="stylesheet" href="{% static 'css/normalize.css' %}">
<link rel="stylesheet" href="{% static 'css/base.css' %}">
	<link rel="stylesheet" href="{% static 'css/layout.css' %}">
	<link rel="stylesheet" href="{% static 'css/post.css' %}">
```

`{% load static %}`

: static파일을 load할것이다.

`{% static 'css/base.css' %}`

: 연결할 static 파일의 링크를 선언해준다.

: `nav바`는 크게 `wrap`이라는 `id요소`로 만든 박스로 묶고, 그안에 `top-header`라는 `div박스`안에 `left div`,`center div`, `right div`요소를 묶은후 각 3개의 div폴더중 center만 제외하고 왼쪽과 오른쪽은 a태그안에 img태그를 넣고  `<img src="{% static 'images/파일명' %}"`을 넣어줘야 한다. center div요소 안에는 `<input type="text" placeholder="검색">`을 넣어준다.

`div.container` : 본문 전체 `div`박스 만들기

```
<body>
< div id="wrap">
<header class="top-header">
<nav>
	<div class ="nav-left nav-item">
	<a href="#">
	<img src="{% static 'images/파일명' %}>
	</a>
	</div>
	
	<div class ="nav-center nav-item">
	<input ="text" placeholer="검색">
	</div>
	<div class="nav-right nav-item">
	<a href="#">
	<img src="{% static 'image/파일명' %}
	</a>
	<a href="#">
	<img src="{% static 'image/파일명' %}
	</a>
	<a href="#">
	<img src="{% static 'image/파일명' %}
	</a>
</div>
</nav>
</header>

<div class="container">
{% block content %}
{% endblock %}
</div>
</div>
</body>
</html>
```

`static/scss/layout.scss`

```
$screen-tablet : 700px;
# 반응형웹 최대 크기  700px
$header-height : 83px;
# header높이 지정

header.top-header {
	height : $header-height;
	padding : 0 10px;
	border-bottom : 1px solid black;
}

header.top-header > nav {
	max-width : 1010px;
	margin: 0 auto;
	# 가운데정렬
	# nav도 시맨틱태그로 크기가 있음
	
	height : $header-height;
	.nav-item {
		height: $header-height;
		line-height: $header-height;
		
	.nav-item > a {
		height: $header-height;
		line-height: $header-height;
		display: inline-block
		#인라인요소를 유지하면서 블록처럼 어느정도 높이를 주는방식
		
		> img {
			verticle-align:middle;
			#수직중앙정렬
			
	.nav-left {
		float:left;
		margin-right:20px;
		width:182px;
	}
	.nav-center {
		width:147px;
		float:left;
	}
	@media screen and (max-width: $screen-tablet {
	# 반응형웹을 사용할때
	# @mediea screen을 사용하고
	#  $screen-tablet :반응형웹적용될 크기
	.nav-center {
		display:none;
		#검색창은 사라지도록
		}
	}
	# 반응형웹은 .nav-center만 포함
	.nav-right {
		width:150px;
		float:right;
		a {
			padding : 0 10px;
			}
		}
	}
	
	.container {
		max-width:1010px;
		margin : 0 auto
		padding-top : 40px;
		@media screen and (max-width: $screen-tablet) {
		padding-top: 0;
		}
	}
```



`static/scss/base.scss`

```
html,body {
	font-size : 14px;
	line-height : 18px;
}
input[type="text"] {
	border : 1px solid black;
	border-radius : 3px
	color :
	font-size : 12px;
	outline : none;
	padding : 3px 10px 3px 10px;
	background-color :
}
```

: `line-height`은 텍스트의 상하높이를 의미하며, `height`와 높이를 맞추면, 텍스트간의 수직정렬이 된다.

: `border-radius`은 텍스트의 외곽선을 둥글게 하는것이다.




`include/post.html`

![](/Users/mac/projects/images/스크린샷 2017-06-14 오후 10.26.02.png)

: 웹사이트에서 주로 사용하는 문서구조를 태그로 약속한것이다. `header : 머릿말`,`nav : 내비게이션 링크`,`section:콘텐츠영역`,`article:콘텐츠내용`,`aside:본문이외의내용`,`footer:꼬릿말,제작자 및 저작권 정보 표시`


: `article.post`로 전체로 묶고, `header부분`은 `div요소`로 `좌측과 우측`을 만든다. 그다음에 올 `photo container`안에 이미지 링크`{{ post.photo.url }}`를 걸어둔다. 그다음에는 `button클래스`를 만들고, 그안에 `a태그 안에 이미지태그`를 넣어둔다. 그다음에 `p태그로 좋아요``{{ post.like_count }}`를 만든다. `comment container div`안에 `comment div요소 `안에` a링크`로 `{{ comment.author }}`, `span요소`로  `{{ comment.content }}`, `a링크`로 `comment-tag`를 만든다. 그리고 마지막에  `p요소`로 `{{ post.created_date }}`를 만들고 `form요소`로 안에 `input type ="text"`로 댓글달기를 만들어준다.

```
<article class="post">
<header>
<div class="post-header-left">
	<img src="" alt="">
	<span>{{ post.author }}</span>
	<div class="post-header-right">
	??
	</div>
</header>
<div class="post-photo-container">
	<img src ="{{ post.photo.url }}" alt="">
</div>
<div class="post-btn-container">
	<a href="#">
	<img src="" alt ="">
	</a>
	<a href ="#">
	<img src="" alt="">
	</a>
	</div>
<p>좋아요 {{ post.like_count }}개 </p>
<div class="post-comment-container">
{% for comment in post.comment_set.all|slice:":4" %}

<div class="post-comment">
	<a href="" class="comment-author">{{ comment.author }}</a>
	<span class="comment-content">{{ comment.content }}</span>
	<a href=""  class="comment-tag"></a>
	</div>
{% endfor %}
</div>
<p class="created">{{ post.created_date }}</p>
<form action="">
	<input type="text" placeholder="댓글달기">
	</form>
</article>
```

`static/scss/post.scss`

```
article.post {
	post-photo-container {
		img { width : 100%; }
		# 반응형웹에서 사진크기 자동조절
		# width:100%;
		}
	}
```














