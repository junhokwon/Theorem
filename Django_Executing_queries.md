# Executing quries


```
from django.db improt models

class Blog(models.Model):
	name = models.CharField(max_length=100)
	tagline = models.TextField()
	
	def __str__(self):
		return self.name
		
class Author(models.Model):
	name = models.CharField(max_length=200)
	email = models.EmailField()
	
	def __str__(self):
		return self.name
		
class Entry(models.Model):
	blog = models.ForeignKey(Blog)
	headline = models.CharField(max_length=255)
	body_text = models.TextField()
	pub_date = models.DateField()
	mod_date = models.DateField()
	
	authors = models.ManyToManyField(Author)
	n_comments = models.IntergerField()
	n_pingbacks = models.IntergerField()
	rating = models.IntegerField()
	
	def __str__(self):
		return self.headline
```

: 1대 다 관계는 `Blog` 대 `Entry`이며 다쪽에 `ForeignKey`를 걸어준다. 1대다 관계는 `entry_set`으로 역참조 가능,

: 다대다 관계는 `Author` 대 `Entry`이며, 역참조 `related_name`은 `entry_set`이나 `authors` 인스턴스 그대로 불러서 쓸 수 있다.

`save()`는 : `primary_key`를 생성하고 누적한다.

`manytomany`일때는 `add()`만 하면 `save()`저절로 된다.

`쿼리셋은 ` sql의 `select`와 같고, `filter`는 sql의 `where,limit`와 같다.

: 매니저를 통한것은 테이블 수준
: 인스턴스를 통한것은 low 수준



## Chainging filters

```
Entry.objects.filter(
	headline__startswith='What').exclude(pub_date__gte = datetime.date.today()).filter(pub_date__gte=datetime(2005,1,30)
```

: 헤드라인이 `What`으로 시작하며, 오늘 이후의 게시날짜를 배제하고, (2005,1,30)이후의 게시물만 가져와라

## Limiting QuerySets

: `print`,`for`문등 하기 전까지, 쿼리셋을 만드는것은 데이터베이스에 가져오지 않는다. 단순히 `SQL`문을 작성하는것과 같다.

: **하나의 객체를 가져올때는 `get`을 쓴다.** 매치되는 객체가 없을때는 `클래스명.DoesNotExist`가 발생한다. 

* 쿼리셋 슬라이스

`Entry.objects.all()[:5]`

: 처음부터 5개의 객체를 가져와라

`Entry.objects.all()[5:10]`

: 5번쨰부터 10번째까지 가져와라

`Entry.objects.all()[:10:2]

: 0번째에서 10번째까지 2번째마다

`Entry.objects.order_by('headline')[0]`

: `headline`의 알파벳순서로정렬한후, 첫번째의 `entry`값을 가져와라.

`Entry.objects.filter(blog_id=4)` : 데이터베이스에서는 `blog_id`로 필드명이 저장

* `Field__속성 = value`


`Entry.objects.get(headline__exact ="Cat bites dog")`

: 일치하는것

`Blog.objects.get(id__exact=14)
Blog.objects.get(id=14)` 

: `id__exact`  두개구문은 같은의미이다.

`Blog.objects.get(name__iexact="beatles blog")`

: `name_iexact` 대소문자 구분없이 다 가져온다.

`Entry.objects.get(headline__contains='Lennon')`

: `Entry`모델에서 가져오는것

`Entry.objects.filter(blog__name='Beatles Blog')`

: **`blog`을 클래스를 들어가서 `name`속성을 사용한다.**

`Blog.objects.filter(entry__headline__contains='Lennon')`

`Blog.objects.filter(entry__authors__name="Lennon")`

: `related_query_name = entry`로 사용하는경우, **`Blog`모델에서 필터링하려면**, `(entry__headline__contains=?)`라고 참조하고자 하는 속성의 클래스명`entry`를 넣어야한다. `역 쿼리네임`으로 참조하는것이다. 

`Blog.objects.fitler(entry__headline__contains='Lennon',entry__pub_date__year=2008)`

: and 연산

`Blog.objects.filter(entry__headline__contains='Lennon').filter(entry__pub_date__year=2008)`

: 필터링을 2번 하는것이다. `as well as` ~뿐만 아니라의 의미



`Blog.objects.exclude(
	entry__in = Entry.objects.filter(
	headline__contains='Lennon',
	pub_date__year = 2008,
	),
)`

: `filter`and 연산을 하고 `exclude`연산을 해야한다. 블로그 중에서 and연산으로 속하는 entry(`entry__in`)을 제외하고 검색하는것.

`Blog.objects.filter(entry__authors__name__isnull=True)`

`Blog.objects.filter(entry__authors__name=None)`과 같은 의미

: `authors__name`이 없을 경우, `authors__name`에는 빈값이 있어도 `null`값으로 나타낸다.(오류발생하지않고, 그냥 `null`값으로 설정해라)

## Filters can reference fields on the model

: `F expreesions`은 **같은모델에서 두가지의 다른필드(속성) 값을 비교**해준다. 같은 모델에서 필드안의 값을 참조하고 싶을때 사용한다.


`from django.db.models import F`
`Entry.objects.filter(n_comments__gt=F('n_pingbacks'))`

: `n_comments` 가`n_pingbacks`보다 많을때


또한 `F()`을 사용하면, 다양한 연산이 가능하다.

`Entry.objects.filter(n_comments__gt=F('n_pingbacks')*2)`

`Entry.objects.filter(rating__lt=F('n_comments') + F('n_pingbacks'))`

`Entry.objects.filter(authors__name=F('blog__name'))`

: 작가의이름과 블로그의 이름이 같은 Entry

`from datetime import timedelta
Entry.objects.filter(mod_date__gt=F('pub_date') + timedelta(days=3))`

: 수정한 날짜가 게시한날짜 3일 이후의 entry만 가져오기.

`F('somefield').bitand(16)`

## Escaping percent signs and underscores

`Entry.objects.filter(headline__contains='%')`

: `%`은 문자열이 들어가는 값을 찾는다.

## Caching and QuerySet

: : 처음에 쿼리셋을 생성할경우, 캐쉬값은 비어있다. 데이터베이스 쿼리를 처음 실행할경우, 장고는 쿼리셋 캐쉬에다가 쿼리의 결과를 저장한다.

```
>>> print([e.headline for e in Entry.objects.all()])
>>> print([e.pub_date for e in Entry.objects.all()])
```

: 위의 예에서는 같은 데이터베이스 쿼리를 2번 실행한 결과를 의미한다.

```
>>> queryset = Entry.objects.all()
>>> [entry for entry in queryset] # Queries the database
>>> print(queryset[5]) # Uses cache
>>> print(queryset[5]) # Uses cache
```
: 쿼리셋이라는 인스턴스를 만들어 실행하면, 캐쉬를 재활용한다.

```
>>> queryset = Entry.objects.all()
>>> print(queryset[5]) # Queries the database
>>> print(queryset[5]) # Queries the database again
```

: 슬라이스연산이나 인덱스 연산은 캐쉬를 발생하지 않는다.

```
>>> queryset = Entry.objects.all()
>>> [entry for entry in queryset] # Queries the database
>>> print(queryset[5]) # Uses cache
>>> print(queryset[5]) # Uses cache
```

: 하지만, 전체 쿼리셋이 이미 평가되었다면, 캐쉬가 남는다.


## Complex lookups with Q objects

: filter()연산은 and연산과 비슷, `Q 객체`를 사용해야한다.

```
from django.db.models import Q
Q(question__startswith='What')
```

`Q(question__startswith='Who') | Q(question__startswith='What')`

: `|` = or

`Q(question__startswith='Who') | ~Q(pub_date__year=2005)`

: `~Q` : NOT을 의미

```
Poll.objects.get(
    Q(question__startswith='Who'),
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6))
)
```

: `Q` 객체에서 `,`은 `and`연산을 의미한다 `who` and ((2005,5,2) or (2005,5,6)), **항상 `Q`객체가 먼저 쓰여야한다.**

## Comparing objects

: 비교할경우 `==`을 사용한다. 비교할때 같다는 기준은 `primary_key`를 기준으로 한다.

```
>>> some_entry == other_entry
>>> some_entry.id == other_entry.id
```

```
>>> some_obj == other_obj
>>> some_obj.name == other_obj.name
```

## Deleting objects

`Entry.objects.all().delete()`
`b = Blog.objects.get(pk=1)`
`b.delete()`

: `on_delete = models.CASCADE`일경우 참조된 객체 필드까지 삭제된다.

: `objects.delete()`는 실행되지 않는다.

## Copying model instances

```
blog = Blog(name='My blog', tagline='Blogging is easy')
blog.save() # blog.pk == 1

blog.pk = None
blog.save() # blog.pk == 2
```

: `pk`값을 없애고 `blog.pk = None`하고 다시 `blog.save()`하면 새로운 pk값이 부여되고 복사된다.

```
class ThemeBlog(Blog):
    theme = models.CharField(max_length=200)

django_blog = ThemeBlog(name='Django', tagline='Django is easy', theme='python')
django_blog.save() # django_blog.pk == 3
```

```
django_blog.pk = None
django_blog.id = None
django_blog.save() # django_blog.pk == 4
```

```
entry = Entry.objects.all()[0] # some previous entry
old_authors = entry.authors.all()
entry.pk = None
entry.save()
entry.authors.set(old_authors)
```

: `Entry`와 `Author`와 같은 다대다관계인경우는 새로운 테이블을 생성하기에, entry를 복사하기 위해서는,`entry.authors.all()`을 `old_authors`에 넣고, `pk`값을 없앤후, 저장하고 `entry.authors.set(old_authors)`을 넣으면 복사된다.

```
detail = EntryDetail.objects.all()[0]
detail.pk = None
detail.entry = entry
detail.save()
```

: 1대 1 관계일경우, `detail = EntryDetail.objects.all()[0]`과 같이 한개의 primary key에 하나씩 연결되어있기에, 세세히 구분해줘야한다.

## Updating multiple objects at once

: 모든객체의 특별한 값을 한번에 바꾸고 싶을때, `update()`를 사용한다.

```
# Update all the headlines with pub_date in 2007.
Entry.objects.filter(pub_date__year=2007).update(headline='Everything is the same')
```

: 2007년도에 게시된 모든 헤드라인을 바꾸고 싶을때,

```
>>> b = Blog.objects.get(pk=1)

# Change every Entry so that it belongs to this Blog.
>>> Entry.objects.all().update(blog=b)
```

```
>>> b = Blog.objects.get(pk=1)

# Update all the headlines belonging to this Blog.
>>> Entry.objects.select_related().filter(blog=b).update(headline='Everything is the same')
```

`update()`시에는 `save()`가 호출되지 않기에, 각각에 업데이트하고 `save()`해라

```
for item in my_queryset:
    item.save()
```

```
Entry.objects.all().update(n_pingbacks=F('n_pingbacks') + 1)
```

: `F expreesions`도 `update()`할때 사용이 가능하다.

# using a custom reverse manager

: 역참조에서 사용되는 `RelatedManager`는 `default manager`의 서브클래스이다.

: **`selected_related()`에서 가져오게되면 연결되어 있는 외래자의 모델의 모든 정보를 가져온다.**

```
from django.db import models

class Entry(models.Model):
    #...
    objects = models.Manager()  # Default Manager
    entries = EntryManager()    # Custom Manager

b = Blog.objects.get(id=1)
b.entry_set(manager='entries').all()
```

: `objects` 는 기본 매니저, `objects=models.Manager()`로 선언

: `entries`는 커스텀 매니저이다. `entires = EntryManager()`로 선언.

: `entries`라고 매니저이름을 바꾸었으나, 역참조이름은 못바꾸기 때문에 `b.entry_set(manager='entries')`라고 선언하여 사용해야 한다. 커스텀매니저를 사용하는이유는 필터링할수는 있지만, 애초에 어떠한 조건을 포함하고 있는 메서드로 만들면 편할 수 있다.

## Additional methods to handle related objects

* add(obj1,obj2,...)

* create(키워드인자)

: 새로운 객체를 만들때 저장한다.

* remove(obj1,obj2,...)

* clear() 

: 관련된 모든 객체 삭제

* set(objs)

: 관련된 객체를 대체

`b = Blog.objects.get(id=1)`
`b.entry_set.set([e1,e2])`







	

	
	