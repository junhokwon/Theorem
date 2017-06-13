
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
	
: request.FILES(딕셔너리이며,각각 `FileField또는 ImageField의 "키"를 포함하고 있다)에서 파일 업로드 방법(파일을 가져와야한다)

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

: `request.FILES`는 request.method가 POST이고, form속성중 `enctype="multipart/form_data"`를 가진다면, 데이터만 포함오직 포함할 것이다. 다른말로 하면 `request.FILES`에는 텅빌것이다.

`photo = request.FILES['file']` : [키] ==값을 반환

`comment_string = request.POST.get('comment','')`

: `request.POST`는 딕셔너리이며, POST요청시 name이 `comment`인 input에서 전달한 값을 가져옴. `<input type="text" name="comment" placeholder="댓글">` 즉 `request.POST`자체가 dict(딕셔너리)이고 `dict.get(key, (default=None or 키가 존재하지 않을때의 value))` 즉 `request.POST.get('comment','')`

:  빈 문자열('')이나 None 모두 False으로 평가되므로, if not으로 댓글로 쓸 내용 또는 comment키가 전달되지 않을 경우를 검사해야 한다. ??

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


`<img src="{{ post.photo.url }}" alt="">` 

: `Post`클래스의 `ImageField`로 만든   `photo`속성에다 `.url`을 실행하면 이미지가 나타난다. `post.photo.url`

![](/Users/mac/projects/images/스크린샷 2017-06-12 오후 4.35.12.png)

`photo = models.ImageField(upload_to='post',blank=True` 추가

![](/Users/mac/projects/images/스크린샷 2017-06-12 오후 4.36.12.png)

: media/post/안에 설정된것을 볼 수 있다.


# URL CONf

: 정규표현식 하나가 일치하면 views.py에 보낸다. 요청이 views.py에 보내진다.

: 정규표현식중 그룹지정을한것이 키워드 인자로 주어진다.

# atom실행후 css 파일작업

![](/Users/mac/projects/images/스크린샷 2017-06-13 오후 12.30.30.png)






