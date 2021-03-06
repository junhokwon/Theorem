
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

: User을 직접 참조하는 대신에, `django.contrib.auth.get_user_model()`을 사용한다. 이 메서드는 현재의 활동중인 **`user model`이나 `custom user`**을 리턴한다. `models.py`에 `User`대신에`settings.AUTH_USER_MODEL`로 바꿔준다.

## post_detail만들기

`post = Post.objects.get(pk=post_pk)`

: Model(DB)에서 post_pk에 해당하는 Post객체를 가져와 변수(인스턴스)에(post)할당, 모델매니저(objects)의 get메서드를 사용해서, 단 한개의 객체만 가져온다. get(키워드인자)메서드는 주어진인자에 맞는 객체를 찾지 못한다면 `DoesNotExist`오류를 발생한다.
예외처리를 해주기 위해서

```
try:
	post = Post.objects.get(pk=post_pk)
except Post.DoesNotExist as e:


(1) return HttpResponseNotFound('post not found, 'detail : {}'.format(e))

(2) return redirect('post:post_list')

(3) return HttpResponse(reverse('post:post_list)

(4) url = reverse('post:post_list')
return HttpResponseRedirect(url)

```
==

```
* get_object_or_404()

from django.shortcuts import get_object_or_404

def post_detail(request,post_pk):
	post = get_object_or_404(Post,pk=post_pk)


```
: 이경우는  `/post/124214124`와 같이 아무숫자나 올수 있기때문에 404 에러가 발생한다. 

(1) 404 Notfound를 띄어준다.

`return HttpResponseNotFound('post not found , 'detail : {}'.format(e))`

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

(3) **하드코딩된 url경로일경우**
`return redirect('/some/url/'(상대경로)`

`return redirect('https://~절대경로)`


(2)-2 `HttpResponseRedirect`를 사용

```
url = reverse('post:post_list')
return HttpResponseRedirect(url)
```



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

:  빈 문자열('')이나 `default = None` 모두 `False`으로 평가되므로, `False`인경우 `if`문이 실행되지 않고 리턴된다. `if not`으로 `댓글로 쓸 내용` 또는 `comment키가 전달되지 않았음`(만약에 html파일에서  <input name="comment">를 쓰지 않았을 경우)를 검사가능

```
if not comment_string:
# comment_string이 오지 않았을경우
	return redirect('post:post_create')
```

: 이문장을 `comment_string = request.POST.get('comment,'')`에서 두번째 인자로 빈문자열 넣어놨기에, if문에서 빈문자열이나 None은 False로 처리되며 if문이 실행되지 않고 그대로 리턴된다.

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

# User기능 만들기

## (1) 이론

## Autentification request

`User objects`

`superusers`또는 `staff`users는`user object`이다. 전형적으로 사이트와 연결되어 있는 사람들이 이용할 수 있는 기능을 담는다. `접근제한`,`프로필등록`등


`user의 기본값 속성` : `username`,`password`,`email`,`first_name`,`last_name`

## Creating users

: 직접적인 방법은 `create_user()`함수를 사용하는 것이다.

`create_user(username,email=None,password=None, extra_fields)`

: `username`과 `password`은 주어지고, 이메일주소는 소문자처리가 자동으로 된다.

: `extra_fields`는 키워드인자로 `custom_user_model`에서 추가한 기능등이 포함된다.

```
from django.contrib.auth.models import User
user = User.objects.create_user('john','email주소,'password')
# create함수는 저장기능이 내제되어있다.
```

* User(장고User)에서 새로 만들경우,

`User.objects.create_user('name','email address','password')`


* 관리자 생성방법

`./manage.py createsuperuser`

## changing passwords

```
from django.contrib.auth.models import User
u = User.objects.get(username='john')
u.set_password('new password')
u.save()
```

: `set_password('바꿀 비밀번호')`

## Authenficating users

`authentificate(request=None, **credentials)`

: `authenticate()`은 `authentification backend`에서 요청한 user가 있는지 없는지 `username`과 `password`를 기준으로 확인한 결과를 user인스턴스에 할당

: `none`을 비교할때, 한개의 객체를 여러번 쓸때

` is not`은 객체를 비교하는것이다.
` !=` 은 값을 비교하는 것이다.

![](/Users/mac/projects/images/스크린샷 2017-06-15 오전 11.06.30.png)

```
from django.contrib.auth import authenticate

user = authenticate(username="john",password="secret")

if user is not None:
	# user가 백엔드에서 체크된다면,
else:
	# user가 백엔드에서 체크되지 않는다면,
	
```

: user인스턴스에는 요청한 user가 있는지 없는지가 들어있다. 

`if user in not None:`
: `요청한 user`가 있다면,
`else:`
: `요청한 user`가 없다면,

## Authentication in Web requests

`세션 : 서버쪽에서 어떤 특정 키값을 저장해두는곳`

`미들웨어 : 장고안쪽에서 request와 response 중간에서 작업을 하는것`

: 장고는 세션을 사용하고, 미들웨어에서 요청한 객체에서 인증시스템을 가져온다.

`request.user`는 현재 user을 나타내는 모든 request의 속성을 제공한다.

: 현재 user가 로그인하지 않았을 경우, 이 속성은 `AnonymousUser`의 인스턴스로 세팅이 된다. 

* `AnonymousUser`

`django.contrib.auth.models.AnonymouseUser`

`id`값은 항상 `None`
`username`은 항상 빈 문자열
`get_username()`은 항상 빈 문자열 리턴
`is_anonymous : True`
`is_authenticated : False`
`is_staff and is_superuser : False`
`is_active : False`
`groups and user_permissions` : 텅빈값

`set_password(),check_password(),save(),delete()`는 `NotImplementedError`발생

```
if request.user.is_authenticated:
	# authenticated users에서 
else:
	# anonymous users에서
```

: 이미 로그인된 유저가 있는가 없는가.

## How to log a user in

`login(request,user,backend=None)`함수를 사용한다. user인자에 들어갈 변수는 `이미 인증된 아이디와 비밀번호`  : `user=authenticate(request,username=username,password=password)`를 의미한다.

: `login()`함수는 `HttpRequest`객체와 `User` 객체를 내제한다. `login()`함수는  장고를 이용하여 `세션에서 user's ID`를 저장한다. 미래에 올 request에서 user's detail을 가져올 수 있다. (login함수에 세번째 인자로 `backend`인자를 줄경우 + 2가지조건)

```
from django.contrib.auth import authenticate,login

def my_view(request):
	username= request.POST['username']
	password = request.POST['password']
	user = authenticate(request,username=username,password=password)
	#세션키값들과 비교하여 회원가입한 id와 비밀번호 인지 확인된 user을 user인스턴스에 할당
	
	if user is not None:
		# 세션값과비교한 요청한 user가 있다면,
		login(request,user)
		#인증된 아이디와 비밀번호로 로그인시도
		#보내고싶은 페이지로 Redirect
	else:
		return HttpResponse("invalid login error")

```


## How to log a user out

`logout(request)`

`logout_view(request)`

: 로그아웃할때 쓰는 함수

```
from django.contrib.auth import logout

def logout_view(request):
	logout(request)
	redirect (" ")
```

: `logout()`함수는 user가 로그인 하지않는다면, 어떠한 에러도 발생시키지 않는다.

: 현재 세션에 저장되어있는 현재의 request 데이터는 완벽하게 삭제된다. 모든 존재하는 데이터는 제거된다. 다른사람들이 로그인해서 이전의 user세션데이터 접근을 막기 위한 방법이다. 
		
		
## Limiting access to logged-in users

: 이미 로그인한 유저의 접근제한 코드

```
from django.conf import settings
from django.shortcuts import redirect

def my_view(request):
	if not request.user.is_authenticated:
	return render(request,'myapp/login.html')
```

: `인증이 완료된 요청한 user` = `request.user.is_authenticated`


-

## (2) 실습

`config/urls.py`

```
urlpatterns = [
	url(r'^member/', include('member.urls')),
```

`member/urls.py`

```
from django.conf.urls import url

from . import views

app_name = 'member'
urlpatterns = [
	url(r'^login/$, views.login, name='login'),
	url(r'^logout/$, views.logout, name='logout'),
	url(r'^signup/$', views.signup, name='signup'),
```

`member/views.py`

```
from django.contrib.auth import authenticate,login as django_login,
logout as django_logout,get_user_model

from django.http import HttpResponse
from django.shortcuts import render,redirect

from .forms import LoginForm

User = get_user_model
```

* 로그인기능

**Form클라스 미사용시 **

```
def login(request):
	if request.method == 'POST':
	username = request.POST['username']
	password = request.POST['password']
```


: 요청받은 `Post데이터`(로그인시도를 한 아이디와 비밀번호)에서 username,password키가 가진값들을 username,password인스턴스에 할당,
```
	<input type='text' name='username'>
	<input type='password' name='password'>
```
에서 name으로 지정되는 이름은 `딕셔너리의 "키"`가 된다.

```
user = authenticate(
	request,
	username=username,
	password=password,
)
```

: `authenticate()`함수는 기존에 존재하는 세션값에 저장되어있는 user.detail()의 키값과 요청한 키값을 비교해서 결과를 리턴

```
if user is Not None:
	django_login(request,user)
	return redirect('post:post_list')
```

: 정상적으로 인증되어 User객체를 얻은경우(user변수가 None이 아닌경우) `login()`함수를 사용해서 로그인을 시킨다. `django_login`인 이유는 `def login`과 겹치기 때문이다. `import`할때도, `import login as django_login`별칭을 사용한다. 로그인한 이후의  `request/response`에서는 사용자가 인증된 상태로 통신이 이루어진다. 로그인이 완료된후 `post_list`뷰로 리다이렉트 처리 

```
else:
	if request.user.is_authenticated:
		return redircet('post:post_list')
	else:
		return render(request,'member/login.html')
```

: 이미 로그인이 완료된 상태일 경우에는,

`request.user.is_authenticated` 이미 인증이완료된 `user`일경우, `post_list로 redirect`, 이미 인증된 `user`가 아닐경우, `다시 login창으로 render`

**Form 동적클라스 사용시**

https://docs.djangoproject.com/en/1.11/topics/forms/#the-form-class


: form 동적구성으로 `html파일`에 직접  `input요소`를 만들지 않고, `forms.py`에다 만들어서 적용함.

`member/forms.py`

```
from django import forms
from django.contrib.auth import authenticate
```

```
class LoginForm(forms.Form):
	def __init__(self,*args,**kwargs):
	kwargs.setdefault('label_suffix','')
	super().__init__(*args, **kwargs)
```

: `forms.py`에서 `LoginForm`을 생성한다.

* `setdefault()`

: `get()`과 유사하지만, 딕셔너리에 키값이 존재하지 않아도 쓸 수 있다. 키값이 있으면 리턴하고 없을경우, default value(기본값)을 리턴한다.

`dict.setdefault(key, default=None)`

* `label_suffix`

: form이 렌더링될때, 어떠한 label name이 추가된후의 바뀔수있는 문자열(기본값은 `colon(:)`)

: `초기화메서드 사용후` `kwargs(키워드인자 딕셔너리).setdefault('label_suffix','')`

: `label_suffix`는 기본값이 `콜론`, 즉 콜론이 키값으로 오는 값을 가져온다. 콜론으로된 키값이 없을경우, ('')빈문자열을 리턴

`super().__init__(*args,**kwargs)` : setdefault로 키워드인자를 가져오고 부모메서드를 이어서 실행

```
username = forms.CharField(
	max_length=30,
	widget=forms.TextInput(
		attrs={
			'placeholder':'사용자아이디 입력하시오',
			}
		)
	)
```

: username을 CharField를 사용하여 만든다. `username`인스턴스(필드명)은 `Html form`에서 `name`으로 들어간다. 최대길이는 30, **`widget=forms.TextInput`은 `HTML form`에서의 `<input type="text"`>**와 같은 의미이다. 

`attrs = { 'placeholder' }`는 빈값에 들어갈 문자열이다. 즉 `Html form`으로 `<input type="text", name="username" placeholder="사용자아이디입력하세요">`와 같다.

```
password = forms.CharField(
	widget=forms.PasswordInput(
		attrs={
			'placeholder' : '비밀번호를 입력하세요.',
			}
		)
	)

```

: `Html form`에서 `<input type ='password', name='password', placeholder='비밀번호를 입력하세요'>`와 같다.

* `Form 유효성검사`

: `is_valid`를 실행했을때, `Form내부`의 `모든 field에 대한 유효성 검증`을 실행하는 메서드, 지금 예에서는 `max_length=30` 아이디 최대길이 30자를 넘으면 안되는 조건을 검사할것이다. 특수문자열이나, 반복되는 문자열(2번반복안되는것같은 조건)들은 유효성검사에서 실행해서 맞지않는것은 다시 `login.html`로 리턴한다.

```
def clean(self):
	cleaned_data = super().clean()
```

: `clean()`메서드를 실행한 딕셔너리로 된 기본결과를 가져와서 `cleaned_date`에 할당 `clean()`메서드는 모든필드를 다 가져온다. 즉 검사할 모든 필드요소들을 다 가져와서 `cleaned_data`에 할당한다. `clean()`메서드는 `부모요소의 함수`이므로 `super()`을 써서 `부모메서드`를 사용하겠다고 지정해야 한다. `super().clean()`은 일단 바로위의 부모요소에서 유효성검사를 실행한다. 어떤값을 리턴안할수도 있다. 

```
username = cleaned_data.get('username')
password = cleaned_date.get('password')
```

: `get(key, default=None)`메서드를 사용하여 딕셔너리에서 username을 키로 가지는 값, password를 키로 가지는 값을 가져와서 `username`,`password`라는 인스턴스에 할당한다.

```
user = authenticate(
	username=username,
	password=password,
)
```

: 로그인 요청한 `username`,`password`가 세션 키값들과 같은지 비교하기 위해서 `authenticate()`함수를 사용한다.

```
if user is Not None:
	self.cleaned_data['user'] = user
```

: `인증된 user`일경우, `세션 키`에 인증된 `user인스턴스`를 `self.cleaned_data['user']`에 할당한다. 즉 ['user'], Form에서 딕셔너리에 `user라는 이름의 키`에 해당하는 `모든 필드요소값을 가져오는것`에 `세션키에 인증된 user`을 넣어, 모든 `user`를 키로 가지는 **form의 값들은 인증된 user라고 할 수 있다.** `self`인스턴스 `def clean(self):`에서의 self인스턴스를 가져와서 `self.cleaned_data['user']` 을 실행시켜라 `self`는 딕셔너리이다.

```
else:
	raise forms.ValidationError(
		'login credential not valid')
```

: 세션키와 비교하여 없는 user인 경우에는 `is_valid()`를 통과하지 못하도록, `ValidationError`을 발생시킨다.

`return self.cleaned_date`

: `clean()`함수를 실행했을경우, 최종적으로 `self.cleaned_data`을 리턴한다.
`self.cleanded_data`는 `인증된 User`가 포함된 모든 필드 요소의 딕셔너리이다.

: `clean_<fieldname>` : `clean()`함수를 거친(유효성검사를 끝낸) 필드을 또다시 검사한다.

: `Field_clean()` : 필드타입에 대해 검사

: `form.clean` : 폼 자체의 어떤에러를 검사

`member/views.py`

* Form클래스 사용시

: `forms.py`에서 만들어둔 `LoginForm()`을 활용한다.

```
form = LoginForm(data=request.Post)
```

: `Bound form(어떤값이들어간)`을 생성한다. `data=request.POST`: `POST`요청을 한 딕셔너리를 data에 할당한다.

```
if form.is_valid():
	user = form.cleaned_data['user']
	django_login(request,user)
	return redirect('post:post_list')
```

: `member/forms.py`에서 이미 세션키의 값과 일치하는 user들을  form.cleaned_data['user']에 넣었기에, `user`인스턴스는 인증절차가 끝난 `user`객체들의 모음이다.
그렇기 때문에 로그인을 시켜준다.
`django_login(request,user)`
로그인이 완료되면 post_list로 redirect 해준다.

```
if form.is_valid():
	user = form.cleaned_data['user']
	django_login(request,user)
	return redirect('post:post_list')

else:
	return HttpResponse('login credentials invalid')
```

: `username`또는 `password`가 틀린경우는 `user`변수가 `None`이다. 로그인에  실패했음을 알리기 위해서 `HttpResponse`를 사용한다.

```
else:
	if request.user.is_authenticated:
		return redirect('post:post_list')
```

: `else`는 `get`요청이 왔을경우는 이미 로그인된 상태일것이다. 이미 로그인했기에 post_list로 redirect

```
else:
	if request.user.is_authenticated:
		return redirect('post:post_list')
	else:
		form = LoginForm()
		context = {
			'form' : form,
		}
	return render(request, 'member/login.html', context)
```

: 이미 로그인된것이 아닌데 get요청으로 왔다면, login.html을 render해서 리턴해준다. `form=LoginForm()`은 not Bound값으로 빈값을 가진 `LoginForm()`의 인스턴스 form객체를 만들어 `context`에 넘긴다.

* 로그아웃기능

```
def logout(request)
	django_logout(request)
	return redirect('post:post_list')
```

`templates/common/base.html` 수정

: 로그인과 로그아웃 기능을 만들었으니 template에서 사용

![](/Users/mac/projects/images/스크린샷 2017-06-15 오후 8.37.46.png)

```
<nav>
	<div>
		{% if user.is_authenticated %}
		<span> {{ user }}로 로그인중 </span>
		<a href="{% url 'member:logout' %} class="btn">로그아웃</a>
		{% else %}
		<a href="{% url 'member:login %}" class="btn">로그인</a>
		{% endif %}
	</div>
</nav>
</header>

```

: 구현하고자 하는 html파일요소는 로그인 되어있으면 <유저명>으로 로그인중 표시, 안되어있으면  a태그로 로그인창으로 이동할 수 있는 링크를 추가,

```
{% if ~ %}
{% else %}
{% endif %}
```
을 적절하게 사용하여 표현함

`templates/include/post.html`

: 중복될 요소를 담고있는 `post.html`에서 수정할 부분은 사진을 누르면 디테일 페이지로 링크를 거는것이다.

```
<div class="post-photo-container">
	{% if type='list'%}
	<a href="{% url 'post:post_detail' post_pk = post.pk %}
	<img src="{{ post.photo.url }}" alt="">
	{% else %}
	<img src="{{ post.photo.url }}" alt="">
	{% endif %}

```

: `{% if type =='list %}`는 타입이 리스트로 올경우에 `detail페이지로 링크`를 걸어줘라, `{% else %}` 타입이 리스트가 아닐경우는 링크를걸지말고 사진만 보여줘라

`post/post_list.html`

`{% include 'include/post.html' with type='list' %}`

: 키워드인자 `(type='list')`를 변수로 넣어서, `include`함수로 `type='list'`라는 변수를 넣어서`render`하라는 조건.

`member/login.html`


```
{% extends 'common/base.html' %}

{% block content %}
<div>
	<form action='' method='post'>
	{% csrf_token %}
	{{ form }}
	<button type="submit">로그인!</button>
</div>
{% endblock %}
```

`member/views.py`

* 회원가입 기능 만들기

```
def singup(request):
	if request.method =='POST':
		username = request.POST['username']
		password1 = request.POST['password1']
		password2 = request.POST['password2']
```

: 일단 회원가입은 서버에 어떤 데이터를 추가하는 것이므로 `POST`요청을 받은 딕셔너리들의 키값을 써야만 한다. `POST`요청을 한 딕셔너리의 키(username,password1,password2)에 대한 딕셔너리 값을 인스턴스에 할당

```
if User.objects.filter(username=username).exists():
	return HttpResponse('아이디가 이미 존재합니다.')
```

: 이미 존재하는 값을 확인하기 위해서는 `exists()`함수를 사용한다. 이미 존재하는 username일경우에는 `HttpResponse`을 사용하여 이미존재한다고 알려줌.

```
elif password1 != password2:
	return HttpResponse('패스워드 확인값이 틀립니다. 다시써주세요.')
```

: 아이디값은 같으나,비밀번호확인값이 다른경우는 `HttpResponse`을 사용하여 패스워드 확인값을 틀리다고 알려줌

```
user = User.objects.create_user(
	username=username,
	password=password1,
	)
```

: 위의 두경우 (아이디가 이미 존재하거나, 비밀번호 확인이 틀릴경우)를 제외하고 완벽할경우, `User.objects.create_user`을 사용하여 `User`객체를 새로 생성한다. 생성하려면 `username`과 `password`값만 주면 생성되고 자동으로 저장이 된다.

```
django_login(request,user)
return redirect('post:post_list')
```

: 로그인을 시킨후, post_list으로 redirect

```
else:
	return render(request,'member/signup.html')
```

: `get`요청으로 왔을경우, `member/signup,html`을 보여준다.

`member/signup.html`

```

{% extends 'common/base.html' %}

{% block content %}
<div>
	<form action='',method='post'>
	{% csrf_token %}
	<div>
		<label for='id-username">Username</label>
		<input id='id-username' type='text', name='username'>
	</div>
	<div>
	<label for="id-password1">password1</label>
	<input id='id-password1' type='password' name='password1'>
	</div>
	<div>
	<label for='id-password2">password2</label>
	<input id='id-password2' type='password' name='password2'>
	</div>
	<button type="submit>가입하기</button>
	</div>
	</form>
	</div>
{% endblock %}

```

![](/Users/mac/projects/images/스크린샷 2017-06-15 오후 9.10.34.png)

: <label>과 <input>요소의 id값을 일치시켜준다면 `username`이라는 `label`요소를 클릭하면 `input`요소로 커서가 들어간다.



`commont/base.html`의 이부분을

```
<nav>
				<div>
					{% if user.is_authenticated %}
						<span>{{ user }}로 로그인 중</span>

					<a href="{% url 'member:logout' %}" class="btn">로그아웃</a>
					{% else %}
					<a href="{% url 'member:login' %}" class="btn">로그인</a>
					{% endif %}
				</div>
			</nav>
```



`scss/_variables.scss`

```
$color-text-common: rgb(38, 38, 38);
$color-border: rgba(0,0,0,.0975);
```
:`_variables.scss`와 같이 `_`언더스코어를 앞에 단 이름은 컴파일 되지 않는다


`scss/layout.scss` 수정부분

```
@import 'variables';

수정된 부분



header.top-header > nav:nth-child(1) {
max-width:1010px;
margin: 0 auto;
padding : 0 10px;
#처음쓰인 nav로 바뀜
# padding 추가

header.top-header > nav:nth-child(2) {
	border-top :1px solid $color-border;
	text-align: right;
	
	> div {
	max-width:1010px;
	margin : 0 auto;
	}
}
# nav안에 div요소를 만들어 최대크기 지정

```

`scss/common.scss`

```
@import 'variables';

.btn {
	padding: 4px 10px;
	border : 1px solid $color-border;
	border-radius:3px;
	cursor:pointer;
	text-decoration : none;
}

#common/base.html에서 a요소 class명을 btn으로 주었다. a요소 class명을 btn으로 주면 박스가 만들어진다.

```

: 현재 만들어진 회원가입기능, 로그인기능을 `member/forms`에 다 넣어서 쓰기에, 구분해서 `Form`을 동적으로 구성하기위해서 파일을 구분한다.

# User에 관한 `Form`동적구성하기

`member/forms/`안에 `__init__.py`,`login.py`를 넣는다.
`__init__.py`안에 `from .login import LoginForm`을 넣는다.

* `LoginForm` 만들기

```
from django import forms
from django.contrib.auth import authenticate
```

: forms는 `django`에서 import한다

```
class LoginForm(forms.Form):
	username = forms.CharField(
		max_length=30,
		widget = forms.TextInput(
			attrs={
				'placeholder' : '사용자 아이디를 입력하세요',
				}
			)
		)
	password = forms.CharField(
		widget=forms.PasswordInput(
		attrs={
			'placeholder' : '비밀번호를 입력하세요',
			}
		)
	)
```

: username의 필드는 html파일에서 `<input type='text' name='username',placeholder='~'>
`
password의 필드는 html파일에서 `<input type='password' name='password',placeholder='~'>`

* `is_valid`메서드

: `Form내부`의 모든 Field에 대한 `유효성 검증`을 실행하는 메서드를 만들기

```
def clean(self):
	cleaned_data = super().clean()
	username= cleaned_data.get('username')
	password = cleaned_data.get('password')

```
: `cleaned_data = super().clean()`을 선언하는것보다 `self`인스턴스내에 `clean()`메서드가 들어있기에, `self`인스턴스.속성으로 실행시키는것이 효율적이다.

```
def clean(self):
	username=self.cleaned_date.get('username')
password=self.cleaned_date.get('password')
```

: `self.cleaned_date`는 `self`라는 인스턴스는 `clean()`메서드가 들어있기에 직접 실행시켜 제정된 결과를 `get()`메서드를 활용하여 가져온다. `get()`메서드를 사용하는 이유는 빈값이 올경우(아이디나 비밀번호가 빈값으로 올경우) `False`로 인식하여 함수를 실행하지 않기때문에, 빈값으로 올경우에 대한 함수를 `if not`으로 따로 지정해주지 않아서 편하다.

```
user = authenticate(
	username=username,
	password=password,
	)
```

: username과 password를 이용해 `authenticate()`함수를 실행시켜 저장되어 있는 세션의 딕셔너리에 있는 username과 password의 키에 대한 값을 비교를 실시한다.

```
if user in not None:
	self.cleaned_data['user'] =user
```

: 대조한결과 요청한 user의 값들이 세션에 저장되어 있는 키에대한 값과 일치할경우, 인증된 user을 `self.cleaned_data['user']`에 할당한다. `self.cleaned_data['user']` : `user`라는 키의 이름으로 되어있는 정제된 모든 값들을 인증된 user인스턴스(안에 인증된 키에대한 값)로 대체한다. 물론 다른 정제된 모든 필드요소도 같이 들어있다.

```
else:
	raise forms.ValidationError(
		'login credentials not vaild'
		)
	return self.cleanded_data
```

: 인증에 실패할경우. `is_valid`를 통과하지 못하도록 `forms.ValidationError`를 발생시키고, `return self.cleaned_data`를 리턴한다.

`member/views.py`

```
def login(request):
	if request.method =='POST':
		form = LoginForm(data=request.POST)
```

: `Bound form`을 생성하여 `LoginForm`안에 `data=request.POST`라는 `POST`요청을 받은 딕셔너리를 넣고, `form`이라는 인스턴스에 할당한다.

```
if form.is_valid():
	user = form.cleaned_data['user']
```

: `LoginForm`함수에서 리턴값은 `self.cleaned_data`였다. 유효성검사할때의 `self`인스턴스는 `POST요청을한 딕셔너리`를 담은 `form`이다. `self.cleaned_data`는 `LoginForm`에서 `self.cleaned_data['user']=user`로 이미 인증된 user을 넣어놨다. 그렇기때문에 `form.cleaned_data['user']`은 `form`에서 `cleaned_data['user']`을 실행시키기에  `요청한 user`가 `인증된 user`가 되며 그걸 `user`인스턴스에 할당

`django_login(request,user)`

: `인증된 user`로 로그인시킨다.

* `post`를 새로만들고 싶을때, 로그인상태(`login.requires=True`)가 아닐경우`(get)요청`이 올경우에 장고에서는 url을 `/?next/`라는 상대경로로 보내기에 이에 대한 처리를 해줘야한다. 

![](/Users/mac/projects/images/스크린샷 2017-06-16 오후 6.36.37.png)

```
next = request.GET.get('next')
if next:
	return redirect(next)
```
: `next`키에 매치되는 값 `/?next=/`을 가져와서 `next`라는 인스턴스에 할당한다. `if next:` `next`에 일치하는 값이 왔을경우, `return redirect(next)`으로 리다이렉트 될경우  `next`키에 대한 값은 자동적으로 `member:login`이 들어간다.

`conf/settings.py`

`LOGIN_URL = 'member:login'`으로  설정해놓는다. 이 뜻은 `로그인 url`의 설정해둔 페이지로 갔을경우에, 자동적으로 `next`키에 대한 값은 `LOGIN_URL ='member:login'`이 들어간다.



```
else:
	if request.is_authenticated:
		return redirect('post:post_list')
```

: `GET`요청이 왔을경우, `request.is_authenticated`: 이미  로그인한 user일경우, post_list.html로 redirect

```
if request.user.is_authenticated:
	return redirect('post:post_list')
	form = LoginForm()
context = {
	'form' : form,
}
```

: 이미 로그인한 유저가 아닌경우인데 `get`요청이 왔다면 username이나 password가 빈값으로 구성되어 있기때문이다. 그렇기 때문에 `LoginForm()`을 비워두고 `member/login.html`로 다시 렌더링해준다.

`return render(request,'member/login.html',context)`

* html파일 동적으로 구성하기

`include/field_set.html`을 만들어서 `login.html`과 `signup.html`에 공통으로 적용할 `html`내용을 동적으로 구성한다.

`include/field_set.html`



```
{% if form.non_field_errors %}
<ul class='errors'>
	{% for error in form.non_field_errors %}
	<li>{{ error }}</li>
	{% endfor %}
</ul>
{% endif %}

```

: 여기에서 쓰는 `Form`은 `class LoginForm(forms.Form):`에서 `forms.Form`을 의미한다.


`form.non_field_errors`는 `form`자체에 대한 `error`를 의미하며, 정확히는 `Form`의 `clean()`메서드에 발생한 `ValidationError`를 의미한다. 만약 form자체에 대한 error가 있다면, `{% if form.non_field_errors %}`, `for`문을 이용하여 `error`을 꺼내서 사용한다. `<li>{{ error }}</li>` for 반복문이 끝난다면, `{% endfor %}`, if문이 끝난다면, `{% endif %}`을 의미한다.

```
{% for field in form %}
```

: `form문을 순회`하면 `form이 가진 각 필드`를 매 루프마다 제공한다.

```
{% for field in form %}
<div class="field-wrapper">
	{{ field.label_tag }}
	{{ field }}
```

: `label태그(label for ="id")`을 `field`에서 불러서 실행한다. `form`클래스안에는 각 `form이 가진 각 필드를 모두 리턴`해주기에, 그 필드내에도 `label_tag`속성이 정의되어 있다. `{{ field }}`으로 `input`생성한다. 즉 html에서

```
 <laber for='username-id' name='username',type='text'>
<input id='username-id',name='username'>

<laber for='password-id' name='password',type='text'>
<input id='password-id',name='password'>

```
이것을 

```
{{ field.label_tag }}
{{ field }}
```
: {{ field }}에서 이미 `for문`을 통해 `form문`에서 `각 필드를 하나씩 꺼내주기에` `username`,`password` 2개의 필드가 생성될것이다.

`scss/common.scss`

```
.field-wrapper {
	margin-bottom : 10px;
	
	ul.errors {
		font-size:11px;
		color : red;
		font-weight:bold;
		padding-left:5px;
	}
	label {
		font-size:11px;
		}
	input[type=text],input[type=password] {
	width:100%
	#input요소 한줄차지하기
	box-sizing:border-box;
	#테두리를 포함한 크기로 인식하도록 설정
	}
	# 
```
![](/Users/mac/projects/images/스크린샷 2017-06-16 오후 7.10.47.png)


* `LoginForm`전체를 동적으로 활용하기

: 지금까지는 `LoginForm`에서 `field`를 꺼내서 `html`에 그려줬다면, `LoginForm`자체를 쓸수 있도록 해주자.

`member/context_processor`

```
from .forms import LoginForm

def forms(request):
	context = {
		'login_form' : LoginForm(),
		return context
```

: 이렇게 설정해두면, `login_form`이라는 키값에 `LoginForm`함수자체를 넣었기에 html에서 사용하려면 `context`딕셔너리의 `키`값을 이용하여 변수로 사용한다.

`common/base.html`

![](/Users/mac/projects/images/스크린샷 2017-06-16 오후 7.20.02.png)



```
{% if user.is_authenticated %}
<span>{{ user }}로 로그인중</span>
<a href="{% url 'member:logout' %}" class="btn">로그아웃</a>
```

: 이미 인증된 user면 span요소를 이용해서 {{ user }}은 `member/views.py`에서  선언한 `user = form.cleaned_data['user']`을 의미한다.

```
{% else %}
<form action="{% url 'member:login' %}" method="POST" class="form-inline-login">
	{% csrf_token %}
```
: 이미 인증된 user가 아니라면 `POST`요청을 받아 로그인 기능을 만들어 줘야 한다.

* form동적구성 하기전의 코드


```
<label for="id-login-username">ID:</label>
<input id="id-login-username" type='text' name="username">

<label for='id-login-password">PW:</label>
<input id='id-login-password' type='password' name='password'>
```
==

`{{ login_form }}`으로 대체

```
<button type="submit" class="btn">로그인</button>
```

: 로그인버튼기능 구현

```
<a href="{% url 'member:login' %}" class="btn">회원가입</a>
</form>
{% endif %}
```

: a요소에서 `class="btn"`으로 주면 버튼이 생성된다.

`<form action="{% url 'member:login' %}" method="POST" class="form-inline-login">`

`scss/layout.scss`

```
form.form-inline-login {
  font-size: 11px;
}

form.form-inline-login label {
  margin-right: 5px;
}

form.form-inline-login input {
  padding: 5px 8px;
  font-size: 11px;
  margin-right: 6px;
  #padding은 input박스의 
  내부여백을 의미한다.
  
```
	
* `forms/signup.py`

: 회원가입을 동적으로 구성해보자

```
from django import forms
from django.contrib.auth import get_user_model

User = get_user_model()

class SignupForm(forms.Form):
	username = forms.CharField(
		help_text='signup help text text',
		widget=forms.TextInput)
	
	nickname= forms.CharField(
		widget=forms.TextInput,
		help_text='닉네임은 유일해야 합니다.,
		max_length =24,
		
	password1 = forms.CharField(
		widget=forms.PasswordInput
		)
	password2 = forms.CharField(
		widget=forms.PasswordInput
		)
```

: `SignupForm`을 구성하고 해당 form을 view에서 사용하도록 설정, html파일의 input요소등을 동적으로 필드를 통해 구성 , `help_text`는 오류가 발생했을 경우 전달할 메세지


* `is_valid`유효성검사 메서드 만들기


(1) username일치 여부


```
def clean_username(self):
	username = self.cleaned_data.get('username')
```

: `clean_<fieldname>`메서드를 이용해서 `username`필드에 대한 유효성 검증을 실행 , 장고에서는 유효성검사를 할때 순서대로 실행되는 함수들이 있는데??, 3가지중 마지막으로 `clean_<fieldname>`이다. 즉 모든 함수들이 다 순서대로 실행되기에 전의 정제된 필드들을 모두 들고 올수 있다.

`self.cleaned_date.get('username')`은 username을 키값으로 하는 값들을 가져온다. `get()`메서드를 사용하는 이유는 빈값이 올경우 `if not`으로 오류를 처리할필요없이 빈값은 `False`라고 선언되면 실행되지 않기에 쓴다. 빈값이 올수도 있다는 가정

```
if username and User.object.filter(username=username).exists():
	raise forms.ValidationError(
	'유저아이디 이미 존재'
	)
	return username
	
```

`username and` : username을 전달받았을 경우라고 가정할경우, `User.objects.filter(username=username).exist()` : 이미 `User`메서드에있는 세션에 저장되어 있는 `username`키가 동일하다면, `raise forms.ValidationError`을 실행시킨다. 유효성검사를 넘기지 못하도록 오류를 발생시킨다.

```
    def clean_username(self):
        username = self.cleaned_data.get('username')
        if username and User.objects.filter(username=username).exists():
            raise forms.ValidationError(
                'Username already exist'
            )
        return username
```
: 이미존재하는 username이 아닐경우 `username`을 리턴한다.


(2) password1,2 일치여부

```
def clean_password2(self):
	password1 = self.cleaned_data.get('password1')
	password2 = self.cleaned_data.get('password2')
	
```

: `password2`필드에 `clean_<fieldname>`을 재정의한 이유는  `cleaned_data에 password1이 이미 들어와 있기 때문이다.`

`password1 = self.cleaned_data.get('password1')` : password1라는 키에 대한값을 가져온다.

`password2 = self.cleaned_data.get('password2')` : password2라는 키에 대한 값을 가져온다. 

```
if password1 and password2 and password1 != password2:
```

: `and`요소로 연결한 경우는 `password1`과 `password2`의 값이 온다는 가정을 한것이고, password1과 password2가 일치하지 않을경우,

```
raise forms.ValidationError(
	'password mismatch',
	)
return password2

```

: 유효성검사를 통과하지 못하도록 , `ValidationError`을 일으킨다. `return password2`로 `password2`를 리턴한다.

* 폼적으로 회원가입 기능 구성


```
def create_user(self):
	username= self.cleaned_data['username']
	password=
	self.cleaned_data['password2']
```

: form에서 설정한 모든 필드요소에서 `username`,`password2`키에 대한 값을 가져온다.

```
return User.objects.create_user(
	username=username,
	password,password
	)
```

`User.objects.create_user`안에 `form`에서 만든 모든 필드요소중username,password인스턴스를 넣어서  만든 결과를 리턴한다. 

`member/views.py`

```
def signup(request):
	if request.method == 'POST':
	
	form = SignupForm(data=request.Post)
```

: `POST`요청이 올 경우, `data=request.POST`를 인자로 넣어서 `SignupForm`함수를 실행한 결과를 `form`인스턴스에 할당한다.

```
if form.is_valid():
	user = form.create_user()
	django_login(request,user)
	return redirect('post:post_list')
```

: `form.create_user()`를 실행한 결과는 return값이 `User.object.create_user`에서 만든 하나의 `user`이다. `django_login(request,user)`에 `user`로 넣어서 로그인처리를 한다. 로그인 처리를 한후 `post_list`로 `redirect`

```
else:
	form = SignupForm()
context = {
	'form' : form,
}
return render(request, 'member/signup.html', context)

```

: else : `get`요청이 올경우(빈값이 전달될경우), `SignupForm`의 함수에 빈값을 넣어서 `form`인스턴스에 할당하고 `context`의 딕셔너리에 넣어서, 다시 로그인창으로 회원가입창으로 돌아가도록 `return render(request, 'member/signup.html',context)`로 렌더링해준다.

`include/field_set.html`

```
{% for field in form %}
<div class="field-wrapper">
	{{ field.label_tag }}
	{{ field }}
	
	{% if field.help_text %}
	<p class='help'>{{ field.help_text }}</p>
	
	{% if field.errors %}
	<ul class='errors'>
		{% for error in field.errors %}
		<li>{{ error }}</li>
		{% endfor %}
	</ul>
	{% endif %}
</div>
{% endfor %}
```
![](/Users/mac/projects/images/스크린샷 2017-06-16 오후 10.21.02.png)

: 빨간색의 오류메세지가 `member/signup.py`에서 설정한 필드속성인 `help_text`이다.

실행할때는 `{% if field.help_text %}` ,`{{ field.help_text }}`,`{% if field.errors %}`. `{% for error in field.errors %}` , `{{ error }}`


# Model 동적구성

(1) Post만들기 기능

`post/forms`안에 `__init__.py`와 `post.py`를 넣어준다. `__init__.py`에 `from . import post`넣어주고, `post.py`를 생성해준다. 

```
from django import forms
from ..models import post
```

`models.py`에 있는 `Post`함수를 가져온다.

```
class PostForm(forms.ModelForm):
	def __init__(self,*args,**kwargs):
		super().__init__(*args, **kwargs)
		
```

: 꼭 `ModelForm`을 사용해야 한다. `PostForm`을 생성해서 실제 Post함수의 photo필드는 `photo = models.ImageField(upload_to='post', blank=True)`처럼 `blank=True`가 지정되어 있다. 원래 `member/views.py`에는 `photo=request.FILES['files']`로 구성되어 있다.`forms/post.py`에서  동적으로 파일을 가져오려면, 

`self.fields['photo'].required=True`

: `fields`내에 `request.FILES`가 설정되어 있기에, `self.fields['photo']`가 가능하다. 즉 `photo`라는 딕셔너리의 키에 대한 값을 가져와서 `.required=True`로 무조건 요구하도록 설정한다. 


```
comment = forms.CharField(
	required=True,
	widget=forms.TextInput
	)
	
	class Meta:
		model = Post
		fields = (
			'photo',
			'comment',
		)
```

: `required=False`는 무조건이 아님, `required=True`는 무조건이다. `comment`필드를 생성해주고, `class Meta`속성에서 `model = Post`로 정해주고, `fields = ('photo','comment')`로 `사용할 fields`들만 써준다.


`post/views.py`

```
@login_required
def post_create(request):
	if request.method == 'POST':
	
```

`@login_required`는 무조건 로그인한상태로 들어가도록 하는 `데코레이터`

```
form = PostForm(data=request.POST, files=request.FILES)

```

: `POST`요청을 한 데이터와 파일들을 인자로 `PostForm`에 넣어서 실행한 결과를 `form`인스턴스에 할당

```
if form.is_valid():
	post = form.save(commit=False)
	post.author = request.user
	post.save()
```

`post = form.save(commit=False)`는 `post/views.py`에서 `post_create`함수의 속성들은 `author`,`photo`,`comment`3가지 인데, 이미 `ModelForm`에서 `comment`와 `photo`필드를 생성해 놨기에, 일단 `POST`요청을 한 `photo`와 `comment`는 `ModelForm`의 `save()`메서드를 사용해서 `Post`객체를 가져온다. **데이터베이스에는 저장되지 않고**, 페이크로 저장하는 `save()`메서드를 실행하는것이다. 즉, 기존에 만들어진 `post`객체에 `오버라이드` 하기위해서, 저장은 하지않고, 일단 객체만 가져오는것이다. 
`post.author = request.user` : 현재 요청한(`request.user`)를 `post.author`에 할당하고`post.save()`를 다시한다. 

* `save()`메서드

: `save()`메서드는 `commit=?` 키워드인자를 선택적으로 갖는다. 기본값은 `commit=True`이다.

(1) `save(commit=False)`

: 데이터베이스에 저장되지 않은 객체를 반환한다. `ManyToMany`관계에서, `commit=False`인 경우, 장고는 즉시 `MTM`관계에서 폼데이터를 저장할수 없다. 왜냐면 데이터베이스에 인스턴스가 존재하지 않기 때문이다. 이 문제를 해결하기 위해서, 장고는 `save_m2m()`을 더한다.

```
f = AuthorForm(request.POST)
#  POST data의 폼 인스턴스를 만든다.

new_author = f.save(commit=False)

# new author인스턴스를 저장하지 않고, 인스턴스만 만든다.

new_author.some_field ='some_value`
# new_author의 속성을 호출하여, 값을 할당한다. 수정

new_author.save()
# 새로운 인스턴스를 저장한다.

f.save_m2m()
# 폼에있는 mtm 데이터를 지금 저장한다.

```
`commit=False`로 저장할때만, `save_m2m()`을 호출할수 있다. 

* 추가적인 method호출없이 `save()`메서드를 사용할 경우,

```
a = Author()
f = AuthorForm(request.POST, instance =a)

# POST data를 받는 form instance를 만든다.

new_author = f.save()
# new author인스턴스를 저장하고 만든다.

```

: `ModelForm`은 `save()`와 `save_m2m`을 사용하는것과 똑같이 작용한다.  `a=Author()`은 `commit=False`로 `save()`메서드를 실행한것과 동일하다.



`return redirect('post:post_detail',post_pk =post.pk)`

```
else:
	form = PostForm()
context = {
	'form' : form,
}
return render(request, 'post:post_create.html',context)
``` 

: `GET`요청이 왔을경우, 빈 form (`form=PostForm()`)을 넣어주고 `post_create.html`로 다시 렌더링해준다.


`post_create.html`

`{% include 'include/field_set.html' with form=form %}`

: `field_set.html`을 구성할때,`form`을 변수로 사용한것을 전달한다는 의미, 즉 `form`을 변수로 사용한 `html파일 `이라는것을 보여줌

* `post`만들기

`post/views.py`

(1) `PostForm을 쓰지 않는 경우`

```
@login_required
def post_create(request):
	if request.method == 'POST':
	user = User.objects.first()
	post = Post.objects.create(
		author=user,
		photo =request.FILES['files'],
		)
	comment_string = request.POST.get('comment','')
	if comment_string:
		post.comment_set.create(
			author=user,
			content=comment_string,
			)
```

(2) `PostForm을 쓰는 경우`

```
@login_required
def post_create(request):
	if request.method == 'POST':
	form = PostForm(data=request.POST,files=request.FILES)
	if form.is_valid():
		post = form.save(commit=False)
		post.author = request.user
		post.save()
				
```

: `ModelForm`의 `save()`메서드를 사용해서 `Post`객체를 가져옴. 현재 `Modelform`에 형성된 필드들은 `comment`와 `photo`이다. `photo`의 경우, `ModelForm`안에 `self.fields['photo'].required = True`로 설정해놨다. `comment`와 `author`만 처리하면 된다.

```
comment_string = form.cleaned_data['comment']
if comment_string:
	post.comment_set.create(
		content=comment_string,
		author = post.author
		)
```

: `comment_string`에 `form`에서 정제된 데이터의 속성(`cleaned_data`)에서 `comment`를 키로 하는 값을 가져온다. `form.cleaned_data['comment']` `if comment_string` : 현재 존재하는 문자열이 있을경우 `post.comment_set.create` 역참조를 이용해서 생성한다. `content=comment_string`,`author=post.author`를 넣어준다.

`return redirect('post:post_detail',post_pk=post.pk)`

* `ModelForm`에서 한번에 처리하기

: `ModelForm`에서 처리할 수 없는 것은 어떤값이 올지 모르는 `author`이다. `author=request.user`만 외부에서 `request`요청으로 받고, 나머지는 `ModelForm`에서 처리해준다.

```
def post_create(request):
	if request.method == 'POST':
	form = PostForm(data=request.POST, files=request.FILES)
	if form.is_valid():
		post = form.save(author=request.user)
		return redirect('post:post_detail',post_pk=post.pk)

	else:
		form = PostForm()
		context = {
			'form' : form,
			}
	return render(request, 'post/post_create.html',context)
```

`/post/forms/post.py`

```
from django import forms
from django.contrib.auth import get_user_model

from ..models import Post,Comment

User = get_user_model()

class PostForm(forms.ModelForm):
	def __init__(self,*args,**kwargs):
	super().__init__(*args,**kwargs)
	self.fields['photo'].required = True

	comment = forms.CharField(
		required=False,
		widget=forms.TextInput)
	
	clsss Meta:
		model = Post
		fields = (
			'photo',
			'comment',
			)
```

: `class Meta`에서 `사용할 모델메서드`와 `fields`를 설정할수 있다. `post/models.py`에 있는 `Post`모델을 사용하고, `photo`와 `comment`필드만 사용하겠다.

* `save()`메서드를 사용한다.

`def save(self,**kwargs):`

: `self`는 여기서 `boundform`이다. 
`form = PostForm(data=request.POST,files=request.FILES)`

`commit = kwargs.get('commit',True)`

: 전달된 키워드 인수중 `commit`키 값을 가져옴 , `get()`메서드의 경우, 매치되는`comment`키가 없을경우, `commit=True`기본값을 가진다.

`author=kwargs.pop('author',None)`

: 전달된 키워드인수중 `author`키 값을 가져오고, 기존 키워드인자 딕셔너리에서 제외한다. 그 이유는 `save()`메서드는 `한개의 키워드인자`밖에 못받기 때문이다.

`self.instance.author = author`

: `ModelForm`안에서 `instance`가 있는경우나 없는경우에 항상 `self.instance`를 반환한다.

```
class BaseModelForm(BaseForm):
	opts=self._meta
	
	
	if opts.model is None:
		raise ValueError('modelform has no model class)
		
	if instance is None:
		self.instance = opts.model()
		# class Meta에 있는 Post메서드에 선언한 필드값 photo,comment를 가지는 post객체를 만든다.
		
	else:
		self.instance = instance
	# instance가 이미 존재할경우도 self.instance에 할당한다.
```

: 즉 `instance`가 이미 존재하거나 없는경우 모두 `self.instance`를 반환하며, `self.instance`는 `photo`와 `comment`를 속성으로 갖는 `Post`객체이다. 즉 `post = Post.objects.create(photo=request.FILES['photo'],comment=request.POST`는 `self.instance`에 들어있다. `author`값을 넣어야 한다.

`self.instance.author = author`

: 처음에는 `instance`가 생성되지 않으므로, `self.instance`가 반환되었을 것이고, 자동으로 `class Meta`에서 설정한 필드값들로 만든 `post`객체를 가지고 있고, `post객체.author = author`이고 `author`은 `request.user`를 받은 `author`를 `post.author`하여 `post객체에 넣는것`이다. 일단 `Post`를 만들기 위해 필요한 요소들을 다 넣어야


`instance=super().save(*kwargs)`

: `super().save()`가 실행되는


`ModelForm`에서 
`commit =True`인경우 `DB에 저장`하고(`post.objects.create(인자)`) `commit=False`일 경우 `p=post()`와 같기 때문에, 인스턴스만 남겨둔다.

`commit=True`이고, `instance`가 `pk`와 `author`를 정상적으로 가진경우에만 `Comment`생성 로직을 실행한다. pk를 왜가지는가??

`comment_string = self.cleaned_data['comment']
if commit`

```
if commit and comment_string:
	instance.comment_set.create(
	author=instance.author
	content=comment_string)

return instance
```

: `ModelForm`의 `save()`에서 반환해야 하는 `model`의 `instance`리턴한다.


* `post/models/`에 `my_comment`필드 추가 및 `PostForm`에서 해당 필드 사용하도록 수정

```
class Post(models.Model):
	my_comment = models.OneToOneField(
	'Comment',
	blank=True,
	null=True,
	related_name='+'
	)
```

`post/forms/post.py`

```
from django import forms
from ..models import Post,Comment

class PostForm(forms.ModelForm):
	comment = forms.CharField(
		required=False,
		widget=forms.TextInput)
		
		class Meta:
			model = Post
			fields = (
				'photo',
				'comment',
				)
		def save(self,**kwargs):
		# save메서드 커스터마이징
		commit = kwargs.get('commit',True)
		author = kwargs.pop('author',None)
		
		self.instance.author = author
		instance = super().save(**kwargs)
		
		comment_string = self.cleaned_data['comment']
		if commit and comment_string:
```

: `commit=True`이고, `comment_string`이 있을경우,

```
	if instance.my_comment:
		instance.my_comment.content=comment_string
		instance.my_comment.save()
```

: `instance`는 `DB`에 모든 키워드 인자를 저장한 인스턴스고, 거기다 오버라이드 해준다. `instance.my_comment`는 `post객체.my_comment`가 있다면, `instance.my_comment.content` = `comment_string`을 할당하고, `instance.my_comment.save()`를 한다.

```
else:
	instance.my_comment = Comment.objects.create(
		post=instance,
		author=author,
		content=comment_string
		)
	instance.save()
```

: `my_comment`가 없을경우, 새로 `comment`를 만든다. `Comment.objects.create(
	post=instance,
	author=author,
	content=comment_string)
instance.save()
`

`return instance`

: `ModelForm`의 `save()`에서 반환해야하는 `model`의 `instance`리턴

* `post_modify` 동적으로 구현

`post/urls.py`

`url(r'^(?P<post_pk>/modify/$',views.post_modify,name='post_modify'),`


`post/forms/post.py`

```
from django import forms
from ..models import Post,Comment

class PostForm(forms.ModelForm):
	def __init__(self,*args,**kwargs):
	super().__init__(*args,**kwargs)
	self.fields['photo'].required = True
```

```
if self.instance.my_comment:
	self.fields['comment'].initial = self.instance.my_comment.content
```

: `self.instance`는 `instance`가 없을때 생기는 `post`객체이고, `if.self.instance.my_comment:` `my_comment`(내가 쓴 코멘트)가 있을경우, `self.fields['comment'].initial`처음 `comment`의 키에 대한 값은 `self.instance.my_comment.content`의 값을 넣는다.

* `initial`

`Field.initial` 는 `unbound Form`안에 필드를 렌더링할때, 처음값을 구체화 하기위해 사용한다. 즉 `self.fields['comment'].initial = self.instance.my_comment.content` 처음 빈값에 `comment`라는 딕셔너리의 키에 대한 값은 `self.instance.my_comment.content`를 넣어놓겠다라는 의미이다.

```
comment = forms.CharField(
	required=False,
	
class Meta:
	model = Post
	fields = (
		'photo',
		'comment',
		)
def save(self, **kwargs):
	commit = kwargs.get('commit',True)
	author = kwargs.pop('author,None)
	
	self.instance.author = author
	instance = super().save(**kwargs)
```

```
comment_string = self.cleaned_data['comment']
```

: 정제된 데이터 `cleaned_data`속성에서 `comment`를 키로 갖는 딕셔너리 값을 가져와서 `comment_string`에 할당한다.


`if commit and comment_string:` : `my_comment`가 이미 있는 경우(update의 경우)

```
	if instance.my_comment:
		instance.my_comment.content = comment_string
		instance.my_comment.save()
```

`my_comment`가 없는 경우, `Comment`객체를 생성해서 `my_comment` `OTO field`에 할당한다.

```
	else:
		instance.my_comment = Comment.objects.create(
			post=instance,
			author=author,
			content=comment_string,
			)
		instance.save()
```

: `OTO`필드의 저장을 위해 `Post`(`instance`)의 `save()`를 호출한다. 여기서 instance는 `post`객체이며, `post=instance`처럼 `post`객체이기에 `instance`를 `post`에 넣는다.

`return instance`

: `ModelForm`의 `save()`에서 반환하는 `model`의 `instance`를 리턴한다.


`post/views.py`

```
def post_modify(request,post_pk):
	post = Post.objects.get(pk=post_pk)
```

: 현재 수정하고자 하는 `Post`의 객체를 `get(pk=post_pk)`를 통해 가져온다.

```
if request.method == 'POST':
	form = PostForm(data=request.POST,files=request.FIELS, instance=post)
```

: `instance=post`는 `instance= super().save(**kwargs)`이다. `post`객체(`post=Post.objects.get(pk=post_pk)`를 넣고, `super()`메서드의 `save`메서드를 실행한다.

```
if request.method == 'POST':
	form = PostForm(data=request.POST,files=request.FILES,instance=post)
```
: `boundform`을 생성해서, 요청받은 `request.POST`,`files=request.FILES,instance=post`를 넣는다. 여기서 `post`는 `post=Post.objects.get(pk=post_pk)`이다.

```
	form.save()
	return redircet('post:post_detail',post_pk=post.pk)
	
else:
	form = PostForm(instance=post)
context = {
	'form',form,
	}
	return render(request,'post/post_create.html',context)
```

: `get`요청으로 올경우, 수정한것이 없거나, 빈값으로 올수도 있다는 뜻이므로, `post=Post.objects.get(pk=post_pk)`를 그대로 넣어서, `post_create.html`로 렌더링해준다.



* `post_modify`에서 `instance.author`값을 채울때 발생하던 오류수정, 

: 이유는 `self.instance.author = author`때문이다. `post_create`에서는 `author`값이 `request`요청에 무조건 전달되어야 생성되기에, `self.instance.author`에 `author`을 할당해주었지만, 수정할경우, `author`가 없을수도 있고`(author=None)`, `author=User`인경우도 고려해줘야한다,


```
from django import forms
from django.contrib.auth import get_user_model
from django.contrib.auth.models import AnonymousUser

from .models import Post,Comment,

User = get_user_model()
```

: `User`객체를 가져온다. `author=User`가 있을수도 있기 때문이다.

```
class PostForm(forms.ModelForm):
	super().__init__(*args,**kwargs)
	self.fields['photo'].required = True,
	
	if self.instance.my_comment:
		self.field['comment'].initial = self.instance.my_comment.content
		
comment = forms.CharField(
	required=False,
	widget=forms.TextInput
	)

class Meta:
	model = Post
	fields = (
		'photo',
		'comment',
		)
def save(self,**kwargs):
	commit = kwargs.get('commit',True)
	author = kwargs.pop('author',None)
	
```
* 수정한 부분

`self.instance.author=author`을 바로 쓰지 않고, `self.instance.pk`가 존재하지 않거나(새로 생성하거나), `author`가 `User인스턴스`일경우, 두가지 중 하나이면 `self.instance.author`에 `author`값으로 할당

```
if not self.instance.pk or ininstance(author,User):
	self.instance.author = author
	instance = super().save(**kwargs)
```

: 위의 코드는 `post_create`할때나 `post_modify`할때 모든것을 고려해주는 경우로, `if not self.instance.pk`는 `post_create`를할때 `author`값을 안적어서 요청할수도 있으니, 존재하지않을경우, `author`을 `self.instance.author`에 할당해주고, 또는 `post_modify`할떄, `isinstance`로 `author`와 `User`의 타입을 비교해서 일치할경우, 내가 쓴 글에 대한 수정은 나만이 할수 있으므로, `isinstace(author,User)` 해서 `author=User`과 같다면, 이 `author`를 `self.instance.author`에 할당한다.

# post_modify,post_create form최종본


```
def save(self,**kwargs):

	commit = kwargs.get('commit',True)
	author = kwargs.get('author',None)
	
	if not self.instance.pk or isinstance(author,User)
		self.instance = author
		
	instance = super().save(**kwargs)
	
	comment_string = self.cleanded_data['comment']
	if commit and comment_string
		if instance.mycomment:
			instance.mycomment.content = comment_string
			instance.my_comment.save()
		else:
			instance.my_comment = Comment.object.create(
			post=instance,
			author=instance.author
			content=comment_string,
		)
	instance.save()
return instance

```

`post/views.py`

```
@login_required
def post_modify(request,post_pk):
	post = Post.objects.get(pk=post_pk)
	
	if request.method == 'POST':
		form = PostForm(data=request.POST,files=request.FILES,instance=post)
		form.save()
		return redirect('post:post_detail',post_pk=post.pk)
	else:
		form=PostForm(instance=post)
		context = {
			'form' : form,
		return render(request,'post/post_modify.html',context)
```

* `post_modify.html`수정

`templates/post/post.modify.html`

```
{% extends 'common/base.html` %}

{% block content %}
<div class="content">
	<form action="" method='POST' enctype="multipart/form-data">
		{% crsf_token %}
		<img src="{{ form.instance.photo.url }}
		{% include 'include/field_set.html' with form=form %}
		<button type="submit" class="btn">등록</button>
	</form>
</div>
{% endblock %}

```




* `/`로 접속시 `post_list`로 이동

(1)  

`config/views.py`

```
from django.shortcuts import redirect

def indet(request):
	return redirect('post:post_list')
```

`config/urls.py`

```
from . import views

urlpattenrs = [
	url(r'^$', views.index, name='index')
	
```

(2)

`config/urls.py`

```
from django.views.generic import RedirectView

url('r^$', RedirectView.as_view(pattern_name='post:post_list')),
```

* `post_owner_decorator`구현 및 `post_modify`에서 사용

: 포스트를 쓴 author만이 수정할 수 있도록. 데코레이터를 생성

`django_app/post/decorators.py`

```
from django.core.exceptions import PermisstionDenied

from post.models import Post

def post_owner(f):
	def wrap(request,*args,**kwargs):
		post = Post.objects.get(pk=kwargs['post_pk'])
		if request.user == post.author:
			return f(request, *args, **kwargs)
		raise PermissionDenied
	return wrap
```

: 일단 구체적인 `post`를 구별하기위해 `(pk=kwargs['post_pk'])`에서 `kwargs` 키워드인자모음에서 `post_pk`키에 해당하는 값을 가져와서 pk에 할당하고, `현재 요청한 user = request.user`가 `post.author` : `포스트를 쓴 author`일 경우, return `f(request, *args, **kwargs)` 함수를 리턴하고, 아닐경우 `PermissionDednied`를 발생시킨다.
`return = wrap`을 함수를 리턴하면 `return f(request,*args,**kwargs)`여기에서 `post_modify`나 `post_create`가 들어가서 `return wrap`으로 `wrap`함수로 리턴되고, 그값이 `wrap`함수를 다시돌아가서 실행


* `post_html`에서 수정,삭제 버튼은 작성자만 보이도록 함

`include/post.html`

```
<div class="btn-rignt">
	{% if user == post.user %}
	<a href="{% url 'post:post_modify' post_pk=post.pk %}" class="btn">수정하기<a/>
	<a href="" class="btn">삭제하기</a>
	{% endif %}
</div>

```

`{% if user == post.user %}`을 추가하면, 현재 `user`객체가 `post.user`즉, post를 쓴 user와 같다면,이라는 조건을 추가한다.

* `post_delete`구현

`post/urls.py`

```
ex) post/3/delete

urlpattern = [
	url('r^(?P<post_pk>\d+)/delete/$', views.post_delete, name='post_delete'),
	
```

`post/views.py`

```
@post_ownder
@login_required


def post_delete(request,post_pk)
	post = get_objects_or_404(Post,pk=post_pk)
	if request.method == 'POST':
		post.delete()
		return redirect('post:post_list')
	else:
		context = {
			'post':post,
			}
		return render(request,'post/post_delete.html',context)
		
```
	
* `get_object_or_404()`

```
from django.shortcuts import get_object_or_404

def post_delete(request,post_pk):
	post = get_object_or_404(Post,pk=post_pk)

```
==

```
def post_delete(request,post_pk):
	try:
		post = Post.objects.get(pk=post_pk)
	except Post.DoesNotExist as e:
	(1) return HttpResponse('post not found,detail : '{}'.format(e))
	(2) return redirect('post:post_list')
	(3) return HttpResponseNotFound('post not found')
					
```
		
: 이경우는  `/post/124214124`와 같이 아무숫자나 올수 있기때문에 404 에러가 발생한다. 

`templates/include/post.html`

```
<div class="btn-right">
{% if user == post.user %}
<a href ="{% url 'post:post_modify' post_pk=post.pk %}" class="btn">수정하기<a/>


<form action="{% url 'post:post_delete' post_pk=post.pk %}" method='POST' class="form-inline">
	{% csrf_token %}
	<button type="submit class="btn">삭제하기</button>
</form>
{% endif %}
</div>
</div>

```

: `삭제하기 기능`을 구현하기 위해서는 , 삭제할 `post_pk`가 필요하기에 `POST`요청을 받아야한다.

* `post_delete` 확인페이지 구현

`include/post.html`

```
<div class="btn-right">
	{% if user == post.author %}
	<a href="{% url 'post:post_modify' post_pk=post.pk %}" class="btn">수정하기</a>
	<form action="{% url 'post:post_delete' post_pk=post.pk %}" method="POST" class="form-inline">
          {% csrf_token %}
          <button type="submit" class="btn">삭제하기</button>
        </form>
        {% endif %}
       </div>
     </div>
```


`templates/post/post_delete.html`

```
{% extends 'common/base.html' %}

{% block content %}
<div class="content">
	<img src="{{ post.photo.url }}" alt="" width="200">
	<p>Post (pk: {{ post.pk }}를 삭제하시겠습니까?</p>
	<form action="" method="POST" class="form-inline">
	{% csrf_token %}
	<button type="submit" class="btn">삭제</button>
	</form>
	<a href="{% url 'post:post_detail' post_pk=post.pk %}" class="btn">돌아가기</a>
</div>
{% endblock %}

```

: `post_delete`페이지는 `get`요청할때, 즉 어떤 페이지를 보여주는 용도로 사용할때, 사용한다. 삭제할때의 페이지를 보여줄용도로 `get`요청을 받아서 `post_delete`렌더링해준다. `get`요청은 `post/3/delete`와 같이, 직접 url주소를 쳐서 들어가는경우,

![](/Users/mac/projects/images/스크린샷 2017-06-20 오후 1.45.24.png)




* `comment_create`를 만들기(`CommentForm`)을 만들기



`forms/__init__.py`

```
from .post import PostForm
from .comment import CommentForm

```

`post/forms/comment.py`

```
from django import forms
from ..models import Comment

class CommentForm(forms.ModelForm):
	class Meta:
		model=Comment
		fields = [
			'content',
			]
			widget = {
				'content' : forms.TextInput(
				attrs={
					'class':'input-comment',
					'placeholder': '댓글 입력',
					}
				)
			}
```

: `<input type='text' name='content' class="input-comment'>`를 형성해주는것과 같으며, `comment`를 만들기위한 요소(어떤`post`,`author`,`comment`3가지중) `comment`만 request요청을 받은것을 `models.py`의 `Comment`객체로 만든다. 즉, `comment = Comment(comment.content="")`와 비슷한 의미이다. `ModelForm`에서 동적으로 `html`요소를 생성하기 위해서는 `class Meta`안에다 선언해줘야 한다.

`post/urls.py`

```
ex) post/3/comment/create
urlpatterns = [
	url(r'^(?P<post_pk>\d+)/comment/create/$',views.comment_create, name='comment_create'
```

* `views.py`에 있는 내용을 패키지를 만들어 `post`와 `comment`로 구분한다.

![](/Users/mac/projects/images/스크린샷 2017-06-20 오후 1.53.31.png)

`views/__init__.py`

```
from .post import *
from .comment import *
```

`views/comment.py`

```
__all__ = (
    'comment_create',
    'comment_modify',
    'comment_delete',
)
```
와 같이 `__all__`에 `메서드들`을 튜플로모아서 할당하고 , `import *` 모두 `import`해준다.

* `clean_<fieldname>`메서드 사용


```
from django import forms
from ..models import Comment

class CommentForm(forms.ModelForm):
	class Meta:
		model = Comment
		fields = [
			'content',
			]
			widgets = {
				'content' : forms.TextInput(
				attrs={
					'class' : 'input-comment',
					'placeholder' : '댓글 입력',
					}
				)
			}
```

+

```
def clean_content(self):
	content = self.cleaned_data['content']
	if len(content) < 3:
		raise ValidationError(
			'댓글은 최소 3자이상'
			)
		return content
```

`clean_<fieldname>`메서드를 사용해서,`Comment`의 `content필드`에 대한 유효성 검증을 실행한다. 

`post/views/comment.py`

```
from ..forms import CommentForm
from ..models import Post

__all__ = (
	'comment_create',
	'comment_modify',
	'comment_delete',
)

@require_POST
# POST요청일경우만 쓴다.(내부함수)
@login_required
def comment_create(request,post_pk)
	post = get_objects_or_404(Post, pk=post_pk)
	# post객체를 하나 들고온다. 
	
	form = CommentForm(request.POST)
	if form.is_valid():
		form.save()
		return redirect('post:post_detail', post_pk=post.pk)
```

: 일단 미완성인 `comment_create`메서드를 만든다. 일단 `comment`를 만드려면, 어떤 `post`에  `comment`를 만드는가가 중요하므로, 일단  `post=get_objects_or_404(Post,pk=post_pk)`를 가져온다. `comment`를 만들기 위해서는 `post`,`content`,`author`이 필요하다.

* `post_list`에서 각 `post`에 댓글달기 기능을 추가하기위해서, `context`에 `CommentForm`을 추가한다.

`views/post.py`

```
def post_list(request):
	posts = Post.objects.all()
	context = {
		'posts':posts,
		'comment_form':CommentForm(),
		}
	return render(request, 'post/post_list.html',context)
	
```

: `context`딕셔너리에 `'comment_form' : CommentForm()`를 추가해준다. context 딕셔너리의 키를 변수로 사용할 수 있다. -> `comment_form`

`include/post.html`

```
중간생략
<form action="{% url 'post:comment_create' post_pk=post.pk %}" method="POST">
{% csrf_token %}
{{ comment_form.content }}
</form>
</div>

```

`post.html`에서 댓글달기 기능을 지우고 `form`형식으로 `comment_create.html`로 보내고, `POST`방식을 받아야, 내용을 추가할수 있다. `{{ comment_form.content }}`을 써서 댓글의 내용을 출력할 수 있다.

`post/views/comment.py`

```
@require_POST
# POST요청만 받을때 사용하는것이다.
@login_required
def comment_create(request,post_pk):
	post = get_objects_or_404(Post,pk=post_pk)
	form = CommentForm(request.POST)
```
: `URL`에 전달되어온, `post_pk`로 특정 `Post`객체를 가져온다.

`next = request.GET.get('next')`

: `URL`의 `GET인자`의 `next`키에 대한 값을 가져옴, 이걸해주는 이유는 각 `post`에 해당하는 `comment`이기 때문에, 댓글을 달고 리턴할경우, 댓글을 수정한 `post`로 되돌아가는 기능을 구현하기 위해서다.

```
if form.is_valid():
# form이 유효할 경우, Comment생성
	comment = form.save(commit=False)
	
```

: `form=CommentForm(request.POST)` : `POST`요청을 받은 `CommentForm`객체가 `form`인데, `CommentForm`에서 `input`요소로 만들어준것은 오직 `comment`객체이다.
그러므로, `comment = form.save(commit=False)` : `form`객체(CommentForm내제)에서 `save()`메서드를 호출하고, `commit=False`로 데이터베이스에는 저장하지 않고, `comment`라는 객체만을 만든다. 즉 `comment=Comment(comment=?)`하고 비슷한 의미이다. 여기다 `comment`를 생성할때 필요한 `author`과 `content`를 넣어줘야 한다.

```
comment = form.save(commit=False)
comment.author = request.user
comment.post = post
comment.save()
```
: `comment.save()`는 `Comment.objects.create(content=?,author=?,post=?)`와 같다. 

`form`이 유효하지 않을경우, 현재 `request`에 `error메세지`를 추가한다.

```
else:
	result = '<br>'.join['<br>'.join(v) for v in form.errors.values()])
	message.error(request,result)
```

: `form`이 유효하지 않을경우, `form.errors`가 발생하는 키에 대한 각각의 오류에 대한 모든 값을 `<br>`(줄바꿈) 기준으로 더해주고, 또 더해준다.

`result='<br>'.join['<br>'.join(v) for v in form.errors.values()])`

`messages.error(request,result)`

```
if next:
	return redirect(next)
return redirect('post:post_detail',post_pk=post.pk)
```

`next`값이 존재할경우, `next=request.GET.get('next')`즉 `next`키에 대한 값의 url로 보낸다. 없을경우, `comment`를 수정한 `post_detail`로 리다이렉트 해준다.

* 오류메세지 표현

`common/base.html`

```
<body>
	{% if messages %}
	<div class="messages">
		{% for message in messages %}
		<div>{{ message }}</div>
		{% endfor %}
	</div>
	{% endif %}
```

`include/post.html`

```
{% load static %}
<article class="post">
<article id="post-{{ post.pk }}" class="post">
```

`post.html`의 전체 `id`값에 각각의 `post`의 루트가 보이도록 설정해야한다.

: `<article id="post-{{ post.pk }}">` 라는 `id`값을 선언해주면, 각 `post.html`은 `http://127.0.0.1:8000/post/#post-4`라는 값을 가진다. 

`include/post.html`

`<form action="{% url 'post:comment_create' post_pk=post.pk %}?next={{ request.path }}#post-{{ post.pk }}" method = "POST">
`
댓글 달기기능의 링크를 바꿔준다. `?next=`를 이용하여 `next`의 키로 사용하고, `next`키는 `?next=`에서 매칭이되고, `?next=`키에 대한 값은 `{{ request.paht }}#post-{{ post.pk }}`와 같다. 여기서 `{{ request.path }}`는 `http://127.0.0.1:8000/post/`이고, 거기다 `#post-{{ post.pk }}`가 이어진다.

![](/Users/mac/projects/images/스크린샷 2017-06-20 오후 7.27.43.png)

`views/comment.py`

```
@require_POST
@login_required
def comment_create(self, post_pk)
	post = get_objects_or_404(Post,post_pk=post.pk)
	form = CommentForm(request.POST)
	next = request.GET.get('next')
	# next키에 대한 값을 next에 할당
	# next는 {{ request.path }}#post-
	# {{ post.pk }}가 될것이다.
	
if form.is_valid():
# form이 유효할 경우, Comment생성
	comment = form.save(commit=False)
	comment.author = request.user
	comment.post = post
	comment.save()
	
else:
	result='<br>'.join(['<br>'.join(v) for v forms.errors.values()])
	messages.error(request,result)

if next:
	return redirect(next)
	# next는 url
	
return redirect('post:post_detail', post_pk=post.pk)

```

* `comment_modify`구현

`post/urls.py`

`url(r'comment/(?P<comment_pk>\d+)/modify/$', views.comment_modify, name='comment_modify')`

: post/comment/3(comment_pk)/modify 키워드 인자를 `<comment_pk>`로 준다.

`views/comment.py`

```
def comment_modify(request,comment_pk):
	comment = get_objects_or_404(Comment,pk=comment_pk)
	form = CommentForm(request.POST,instance=comment)
	if request.method == 'POST':
		pass
	else:
		form = CommentForm(instance=comment)
	context = {
		'form' : form,
		}
	return render(request,'post/comment_modify.html',context)
	
```
: 일단 `comment_modify` 최소구현, 수정할 `comment`객체를 `get_objects_or_404(Comment,pk=comment.pk)`로 가져온다.

`include/post_comment.html`

```
<a href='' class="comment-author">{{ comment.author }}</a>
<span class="comment-content">{{ comment.content }}</span>
<a href='' class="comment-tag"></a>

{% if request.user == comment.author %}
# 현재 요청받은 user가 comment.author과 같을때만 수정하게 만든다.

<a href="{% url 'post:comment_modify' comment_pk=comment.pk %}" class="btn btn-xs">수정</a>
{% endif %}

```

`templates/post/comment_modify.html`

```
{% extends 'common/base.html' %}

{% block content %}
<div class="content">
	<h3>Comment modify</h3>
	<form action="" method="post">
		{% include 'include/field_set.html' %}
		<button type="submit" class="btn">수정</button>
		</form>
	</div>
{% endblock %{

```

* `comment_owner`데코레이터 생성

`post/decorators.py`

```
def comment_owner(f):
	def wrap(request,*args,**kwargs):
	comment = Comment.objects.get(pk=kwargs['comment_pk])
	if request.user == comment.author:
		return f(request,*args,**kwargs)
	raise PermissionDenied
return wrap
```

* `comment_modify`완성

`views/comment.py`

```
@comment_owner
@login_required
def comment_modify(request, comment_pk):
	 comment = get_objects_or_404(Comment,comment_pk=comment.pk)
	 next = request.GET.get('next')
	 # next키에 대한 값을 next에 할당
	 # comment_modify하고, 다시 post로(원래위치)로 되돌려주는 기능
	 
	 if request.method == "POST":
	 	form = CommentForm(data=request.POST, instance=comment)
	 	form.save()
	 	
```

: `Form`을 이용해 객체를 업데이트 시킨다.(data에 포함된 부분만 update된다.) `CommentForm`을 이용하여 만든 `comment`객체안에 `data=request.POST`의 모든데이터와, `instance=comment`를 다 넣어주고, `form.save()`한다. 이미 `instance`에는 가져온 객체 자체가 만들어진 `comment`가 있고, 가져온 `comment`(수정할 댓글)을 `instance=comment`로 오버라이드 해주고, `request.POST`에는 `post`와 `author`가 들어있기에, 다 넣어서 `form.save()`를 활용해서 오버라이드해준다.

```
if next:
	return redirect(next)
return redirect('post:post_detail', post_pk=comment.post.pk)
```

: `post_pk=comment.post.pk`로 `comment`의 객체에서 `post`를 불러오고, `post`에서 `pk`를 호출

```
else:
	#CommentForm의 기존 comment인스턴스의 내용을 채운 bound Form
	form = CommentForm(instance=comment)
	context = {
		'form':form,
			}
		return render(request,'post/comment_modify.html',context)
		
```

`get`요청은 어떤 `url`로  보내기위해서 사용한다. `return render(request,'post/comment_modify.html',context)`

* `comment_modify`를 한후 원래위치로 리턴

`include/post_comment.html`


```
{% if request.user = comment.author %}
<a href="{% url 'post:comment_modify' comment_pk=comment.pk %}?next={{ request.path }}#post-{{ comment.pk }}" class="btn btn-xs">수정</a>
{% endif %}
```

![](/Users/mac/projects/images/스크린샷 2017-06-20 오후 8.12.05.png)

`post/comment_modify.html`

```
{% extends 'common/base.html' %}

{% block content %}
{% comment %}
	#template안에서 request.GET의 내용을 쓰는법
	{{ request.GET.[key] }}
{% endcomment %}

<div class='content'>
	<h3>Comment modify</h3>
	<form action="" method="POST">
		{% csrf_token %}
		{% include/field_set.html' %}
		<button type="submit" class="btn">수정</button>
	</form>
```


![](/Users/mac/projects/images/스크린샷 2017-06-20 오후 8.15.15.png)

* `comment_delete`구현

`post/urls.py`

```
ex) post/comment/3/delete
url(r'comment/(?P<comment_pk>\d+)/delete/$', views.comment_delete, name='comment_delete')

```

`post/views/comment.py`

```
@comment_owner
@require_POST
@login_required

next = request.GET.get('next')
def comment_delete(request,comment_pk):
	comment = get_objects_or_404(Comment,pk=comment_pk)
	post=comment.post
	comment.delete()
	if next:
		return redirect(next)
	return redirect('post:post_detail',post_pk=post.pk)
```

: 여기서 중요한것은 `comment_delete`이후에, 원래페이지로 돌아갈수 있도록, `post`객체를 만들어놔야 한다. `post=comment.post`

`templates/include/post_create.html`

```
<form action="{% url 'post:comment_delete' comment_pk=comment.pk %}?next={{ request.path }}#post-{{ comment.post.pk }]" method="POST">
	{% csrf_token %}
	<button type="submit" class="btn btn-xs">삭제</button>
</form>
{% endif %}
```
: 다시 돌아오는 이유는 `{{ request.path }}`이후로 `#post-{{ comment.post.pk }}`역참조로 `comment에서 post를 참조하고 그 post의 pk로 참조하기 때문이다.`

* hashtag 만들기

`post/models.py`

: `Comment`메서드의 속성에 `tags=models.ManyToManyField('Tag')`로 해서, `Tag`와 `Comment`메서드를 다대다 관계로 맺는다.


`tags = models.ManyToManyField('Tag',blank=True)`

```
def add_tag(self,tag_name):

tag,tag_created = Tag.objects.get_or_create(name=tag_name)
if not self.tags.filter(id=tag.id).exists():
	self.tags.add(tag)
```
: tags에 tag매개변수로 전달된 값(문자열)을 name으로 갖는 Tag객체를 (이미존재하면)가져오고, 없으면 생성하여, 자신의 tags에 추가한다. 위의 예를 활용하여 `Comment`에서 `save()`메서드를 활용하여 해쉬태그를 만들어야 한다.

`post/models.py`

```
class Comment(modles.Model):
	post = models.ForeignKey(Post)
	author = models.ForeignKey(settings.AUTH_USER_MODEL)
	content = models.TextField()
	html_content = models.TextField(blank=True)
	tags = models.ManyToManyField('Tag')
	created_data = models.DataTimeField(auto_now_add=True)
	modified_data = models.DateTimeField(auto_now=True)
	like_users = models.ManyToManyField(
		settings.AUTH_USER_MODEL,
		through='CommentLike',
		related_name='like_comments',
		)
```

`Comment`클래스를 만든다. 추가해야할것은 `html_content = models.TextField(blank=True)`
`tags=models.ManyToManyField('Tag')` 

`Comment`클래스 안에 `save()`메서드를 실행하여 해쉬태그를 만든다.

```
import re

def save(self,*args,**kwargs):
	super().save(*args,**kwargs)
	self.make_html_content_and_add_tags()
	
```

: 여기서 `self`는 `Comment`메서드다. `Comment.속성`으로 실행하는것이다.??



```
def make_html_content_and_add_tags(self):
	p = re.compile(r'(#\w+)')
	tag_name_list = re.findall(p,self.content)
	ori_content = self.content
	for tag_name in tag_name_list:
	tag,_ = Tag.objects.get_or_create(name=tag_name.replace('#',''))
	ori_content = ori_content.replace(tag_name,'<a href="#" class="hash-tag">{}</a>'.format(tag_name))
	
	if not self.tags.filter(pk=tag.pk).exists():
		self.tags.add(tag)
	self.html_content = ori_content
	
```

(1) 기존의 self.comment내용을 `#댓글내용`에 url링크를 걸고 싶다.

![](/Users/mac/projects/images/스크린샷 2017-06-20 오후 9.19.39.png)에서 링크가 걸린 해쉬태그를 누르면, 해쉬태그와 일치하는 post를 가져와야 한다. 즉 `해쉬태그링크로 동일한 해쉬태그를 쓴 post의 리스트를 가져와야 한다.`

(2) `Tag`를 사용하여, comment가 담긴 tag를 생성해줘야하는데, 중복되는것을 제외해야한다.

`def make_html_content_and_add_tags(self):`

: `self`는 요청받은 `Comment`객체이다.

`p=re.compile(r'(#\w+)')`

: `#문자열`을 컴파일해서 패턴화하여 `p`에 할당

`tag_name_list = re.findall(p,self.content)`

: `import re`하여, `findall`메서드로 `self.content`에서 p라는 패턴을 찾아 `tag_name_list`에 할당한다.

`ori_content = self.content`

: 요청받은 `comment`객체.content를 원래의 `ori_content = self.content`에 할당한다. 

`for tag_name in tag_name_list:`

:`tag_name_list`에는 해쉬태그의 모음이 있다. 그 모음안에서 `tag_name`을 꺼내서, `tag_name`으로 `태그`를 생성한다.

`tag,_ = Tag.objects.get_or_create(name=tag_name.replace('#',''))`

: tag는 해쉬태그를 없애고, 문자열로 저장해야 하므로, `replace('#','')`로 바꾸고, `tag,_`는 Tag객체를 가져오거나 생성, 생성여부는 신경쓰지 않으므로 변수를 `_`로 처리한다. `Tag.object.get_or_create(name=tag_name.replace('#',''))`로 `tag_name`을 `name`으로 `태그`를 만든다.

`ori_content = ori_content.replace(
tag_name,'<a href="#" class="hash-tag">{}</a>'.format(tag_name))` : 즉 기존에 해쉬태그와 문자열과 섞여있는 `comment`를 `tag_name`만 링크가 걸린 `tag_name`으로 바꾼다. 즉  `<a href="#" class="hash-tag">{}</a>'.format(tag_name))`

`if not self.tags.filter(pk=tag.pk).exists():
	self.tags.add(tag)`
	
: `content`에 포함된 `Tag`목록을 자신의 `tags`필드에 추가한다. 중복되는것을 제외하기 위해서, `self.tags.filter(pk=tag.pk).exists()`로 이미 존재하는 `pk=tag.pk`값으로 필터링하여, `in not`없다면, `self.tags.add(tag)`를 넣어준다. `tag`는 `Tag.objects.get_or_create()`로 만들어준 인스턴스이고, 그것을 `Comment객체.Comment속성(tags).add(tag)`를 실행하여 넣어준다.

`self.html_content = ori_content`

: 편집이 완료된(모두링크가걸린해쉬태그)를 `self.html_content`는 `Comment객체.html_content(Comment속성)`을 불러와서 할당한다.

`include/post_comment.html`

`<p class="comment-content'>{{ comment.html_content|safe }}</p>
`

: `|safe`는 중복되지 않도록 `html`을 표시하는 것이다. 

![](/Users/mac/projects/images/스크린샷 2017-06-20 오후 9.48.54.png)

: 즉 이렇게 `comment.content`를 해쉬태그가 걸린 링크로 보여준다.

* `hashtag`로 `post_list`를 검색하는기능

:`hashtag_post_list`를 `post.py`에 만든다. 

* 태그생성 정규표현식 수정

```
-            ori_content = ori_content.replace(
 -                tag_name,
 -                '<a href="#" class="hash-tag">{}</a>'.format(
 -                    tag_name
 -                )

 ```
을 지우고 새롭게 생성

```
change_tag = '<a href="{url}" class="hash-tag">{}</a>'.format(tag_name)

ori_content = re.sub(r'{}(?![<\w])'.format(tag_name).change_tag,ori_content,count=1)

```
`re.sub('바뀔문자열','바꿀문자열','source(어디서)')` 

: `re.sub(정규표현식,change_tag,ori_content,count=1)`

정규표현식 `(r'{}(?![\w])` : ??

`post/views/post.py`

: `해쉬태그로 검색했을때의 페이지 구현`

```
def hashtag_post_list(request,tag_name):
	tag = get_object_or_404(Tag,name=tag_name)
	
	posts = Post.objects.filter(my_comment__tags=tag)
	post_count = post.count()
	
	context = {
		'tag' : tag,
		'posts' : posts,
		'post_count' : posts_count,
		}
	return render(request, 'post/hashtag_post_list.html',context)
```

: 특정 `tag_name`이 해당 `Post`에 포함된 `Comment`의 `tags`에 포함되어 있는 `Post`목록 쿼리를 생성한다.

`tag = get_object_or_404(Tag,name=tag_name)` 특정한 `tag_name`를 `name`에 넣어서 만든 `tag`객체를 가져온다.

`posts=Post.objects.filter(my_comment__tags=tag)` : 역쿼리네임을 활용하여 `Post`에 있는 `my_comment`속성은 `Comment`와 `MTM`관계로 이어져있으므로, 역쿼리네임을 이용하여 `__tags`, 즉 `Comment`객체에 있는 속성을 호출할 수 있다. `my_comment__tags=tag` 가져온 `tag`객체를 넣어준다. `context`딕셔너리에 어떤 태그,posts는 역쿼리네임으로 참조한 포스트, post_counts, post_counts의 숫자를 넣어준다.

```
def hashtag_post_list(request,tag_name):
	tag= get_object_or_404(Tag,name=tag_name)
	posts = Post.objects.filter(my_comment__tags=tag)
	post_counts = post.count()
	
	context = {
		'tag' : tag,
		'post_counts':post_counts
		'posts':posts
		}
		
	return render(request, 'post/hashtag_post_list.html', context)
	
```

`post/hashtag_post_list.html`

```
{% extends 'common/base.html' %}

{% block content %}
<div class="content">
	<h3>{{ tag.name }}</h3>
	<h5>게시물 총 {{ posts_count }]개</h5>
	
	<ul class="post_list">
		{% for post in posts %}
		<li>
			<img src="{{ post.photo.url }} alt="" width='33%">
		</li>
		{% endfor %}
	</ul>
</div>
{% endblock %}

```

![](/Users/mac/projects/images/스크린샷 2017-06-20 오후 10.17.02.png)

* 정규표현식에서 `change_tag`링크거는부분 변경

`post/models.py`

```
def make_html_content_and_add_tags(self):
	tag,_ = Tag.objects.get_or_create(name=tag_name.replace('#','')
```

+

* 수정할 부분

```
change_tag = '<a href="{url}" class="hash-tag">{tag_name}</a>'.format(url=reverse('post:hastag_post_list', kwargs={'tag_name' : tag_name.replace('#','')}),tag_name=tag_name)
```

`url=reverse('post:hashtag_post_list', args=[tag_name.replace('#','')]`

: {url}에 들어갈 내용 url = reverse를 써야 urls.py에 있는 패턴을 쓸 수 있다. 어떤 인자로, 보낼것은 위치인자 `args=[]`, `kwargs={}`로 줄수 있다. `args=[tag_name.replace('#','')]`, 위치인자의 이름은 `#`을 빼야하므로 `replace`를 함수를 써주었다. 두번째 들어갈 내용은 {tag_name}이고, `tag_name=tag_name`을 넣어주면된다. 그대로 `tag_name`은 요청받은 `해쉬태그의 이름`이다.



	 
	 











	

























```



    def save(self,*args,**kwargs):
        #'박보영 <a href="#">#여신</a>
        p = re.compile(r'(#\w+)')
        tag_name_list = re.findall(p,self.content)
        super().save(*args,**kwargs)
        
```


```
문자열 바꾸기(replace)

>>> a = "Life is too short"
>>> a.replace("Life", "Your leg")
'Your leg is too short'
replace(바뀌게 될 문자열, 바꿀 문자열)
```



