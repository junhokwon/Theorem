#QuerySet API reference

: `API`를 통해 소스공개는 하지 않으면서, 특정권한으로 파일을 업로드/다운로드 할 수 있는 인터페이스기능을 제공하는것이 `API`가 하는일이다. 웹에서의 `API`는 데이터를 요청하고 응답하는것이 전부이다.

## QuerySets are evaluated

: 쿼리셋은 만들어지고,필터링되고,슬라이스되고,일반적으로 데이터베이스에 충돌되는일 없이 우회된다. 데이터베이스의 활동과 관련이 없다.

## Pickling querysets

```
>>> users = {'kim':'3kid9', 'sun80':'393948', 'ljm':'py90390'}
>>> f = open('d:/python21/exam/users.txt', 'w')
>>> import pickle
>>> pickle.dump(users, f)
>>> f.close()
```

: 회원의 ID와 비밀번호를 파일에 저장하는것을 생각해본다면, 처음에 ID와 비밀번호를 users라는 인스턴스에 담았고, f라는 인스턴스에 `users.txt`파일을 새로 열어서 `w` 를 선언하여 수정하다고 하였다. `import pickle`을 사용하여, `pickle.dump(users,f)`를 선언하면 `dump`를 통해 모든 `users`에 담아있는 내용을 `f`라는 인스턴스에 넣어버리는것이다. 즉 **`pickle`모듈은 파이썬에서 만들어지는 것은 뭐든지 파일에 적을 수 있다고 한다.**

```
import pickle
query = pickle.loads(s)
qs = MyModel.objects=all()
s.query = query

```

# Queryset API

: 쿼리셋의 공통된 2가지 속성

`ordered`

: `ordered=True`

`db`

: 지금 쿼리가 실행된다면 데이터베이스가 사용될것이라고 선언

## Methods that return new QuerySets

(1) `filter(키워드인자)`

(2) `exclude(키워드인자)`

` Entry.objects.exclude(pub_date__gt=datetime.date(2005,1,3),headline='Hello')`

: 여기서 `,`는 `AND`를 의미한다.

`Entry.objects.exclude(pub_date__gt=datetime.date(2005,1,3)).exclude(headline='Hello')

: 2005.1.3일 이후의 게시물중 headline이 안녕인 게시물을 필터링해라

(3) `annotate(위치인자,키워드인자)`

```
from django.db import Count
q = Blog.objects.annotate(Count('entry'))

q[0].name
q[0].entry__count
```

: `entry__count`를 `Count('entry')`로 바꾸어 쓴것이다. 

`q = Blog.objects.annotate(number_of_entires=Count('entry))` : `annotate(키워드인자)`를 한것이다.

`q[0].number_of_entries`

: 첫번째 블로그의 기사의 수

(4) `order_by(fields)`

: 기본적으로, `ordering`옵션으로 ㅠ플로 주어진 경우 정렬할때 사용한다. 

`Entry.objects.filter(pub_date__year=2005).order_by('-pub_date','headline')`

: 게시일을 내림차순, 헤드라인을 오름차순으로 정렬

`Entry.objects.order_by('blog')`와 `Entry.objects.order_by('blog__id)`는 같은 의미이다

: `Meta클래스에 ordering`옵션을 주지 않았다면, 관련된 모델의 `primary_key`에 의해 정렬하는것과 같다.

만약 블로그 클래스에서 `ordering=['name']`을 가지고 있다면, `Entry.objects.order_by('blog__name')`과 같다.

* query expressions : `asc()`,`desc()`

`Entry.objects.order_by(Coalesce('summary','headline').desc())`

`Entry.objects.order_by(Lower('headline').desc())`

```
from django.db.models.functions import Lower
Author.objects.create(name='Margaret Smith')

author = Author.objects.annotate(name_lower=Lower('name')).get()

print(author.name_lower)
```

: `Lower(expression)`으로 간단한 텍스트필드를 받아드리고, 소문자화 한다.

`Entry.objects.order_by('headline').order_by('pub_date')`

: 전순위의 `ordering`은 무시된다. 위의예에서는 `headline`으로 정렬됨.

(5) `reverse()`

`my_queryset.reverse()[:5]`

(6) `distinct(fields)`

: 쿼리결과로부터 중복된 줄을 제거한다. 쿼리가 `mutible tables`인 경우, 중복된 결과를 얻을수 있으므로 `distinct()`를 사용한다.

```

Authors.objects.distinct()
>>> ...
Entry.objects.order_by('pub_date').distinct('pub_date')
>>> ...
Entry.objects.order_by('blog').distinct('blog')
>>> ...
Entry.objects.order_by('author','pub_date').distinct('author','pub_date')
```

: `order_by`는 이미 선언된 `model ordering`의 기본값을 사용한다.

(7) `values(fields,expresstions)`

:  딕셔너리를 반환한다. 각각의 딕셔너리는 객체를 의미하고, 모델객체의 속성과 일치하는 `key`값을 반환한다.

`Blog.objects.filter(name__staratswith='Beatles')`

`Blog.objects.fitler(name__startswith= 'Beatles').values()`

`<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>`

: `values()`메서드는 위치인자를 가지고 있다. `fields`를 넣는다면, 각각의 딕셔너리는 너가 구체화한 키나 값을 포함할 것이다. 

`Blog.objects.values()` >>>

`<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>`

`Blog.objects.values('id','name')` >>> `<QuerySet [{'id': 1, 'name': 'Beatles Blog'}]>`

`values()`메서드는 키워드인자를 받기도 한다. 

`from adjango.db.models.functions import Lower`

```
Blog.objects.values(lower_name=Lower('name')) 
>>>
<QuerySet [{'lower_name': 'beatles blog'}]>
```

```
from django.db.models import Count

Blog.objects.values('author',entires=Count('entry'))

>>> <QuerySet [{'author': 1, 'entries': 20}, {'author': 1, 'entries': 13}]>

Blog.objects.values('author').annotate(entries=Count('entry'))

>>> <QuerySet [{'author': 1, 'entries': 33}]>
```

: 결과값이 다르게 나온것을 볼 수 있다. 


```
Entry.objects.values()
>>> <QuerySet [{'blog_id': 1, 'headline': 'First Entry', ...}, ...]>
Entry.objects.values('blog')
>>> <QuerySet [{'blog': 1}, ...]>
Entry.objects.values('blog_id')
>>><QuerySet [{'blog_id': 1}, ...]>
```

: ` values()`의 사용법

`Blog.objects.values().order_by('id')`와 `Blog.objects.order_by('id').values()`는 같은 의미이다.

`Blog.objects.values('name','entry__headline')`

: 관련된모델의 필드속성도 같이 쓸수 있다. 


(8) `values_list(fields)`

: 딕셔너리를 반환하는것을 제외하고는 `values()`와 같다. 이것은 `순환가능한 튜플`을 반환단다.

```
Entry.objects.values_list('id','headline')

[(1, 'First entry'), ...]

from django.db.models.functions import Lower

Entry.objects.values_list('id','Lower('headline'))

[(1, 'first entry'), ...]
```

* `flat=True`속성

: 반환되는 결과가 하나의 값만을 표현

```
Entry.objects.values_list('id').order_by('id')
>>> [(1,), (2,), (3,), ...]
Entry.objects.values_list('id',flat=True).order_by('id')
>>> [1,2,3,...)
```


: 구체적인 필드의 값을 얻기 원할때는 `get()`을 사용해라,

```
Entry.objects.values_list('headline',flat=True).get(pk=1)
>>> 'First entry'
```

(9) `dates(field,kind,order=ASC)`

```
Entry.objects.dates('pub_date','year')
>[datetime.date(2005, 1, 1)]
Entry.objects.dates('pub_date','month')
>[datetime.date(2005, 2, 1), datetime.date(2005, 3, 1)]
Entry.objects.dates('pub_date','day')
> [datetime.date(2005, 2, 20), datetime.date(2005, 3, 20)]
Entry.objects.dates('pub_date','day',order='DESC')
> [datetime.date(2005, 3, 20), datetime.date(2005, 2, 20)]
Entry.objects.filter(headline__contains='Lennon').dates('pub_date','day')
>[datetime.date(2005, 3, 20)]
```

(10) `datetimes(field_name,order=ASC,izinfo=None)`

:`datetimes`에서는 `year,month,day,hour,minute,second`를 사용

: `ASC`는 오름차순,내림차순(`ASC`,`DESC`)을 의미

: `tzinfo` : ?? ,`tzinfo=None`은 `current time zone`을 의미한다.

(11) `none()`

: 아무거솓 반환하지 않는다. 결과를 확인하고 싶을때, `EmptyQuerySet`인스턴스를 사용한다.

```
Entry.objects.none()
> <QuerySet []>
from django.db.models.query import EmptyQuerySet
> isinstance(Entry.objects.none(),EmptyQuerySet)
> True
```

(12) `all()`

: 현재 쿼리셋의 복사본을 반환한다. 이전의 평가된 쿼리셋에서 `all()`을 호출하여 같은 쿼리셋의 결과를 가져올수 있다.

(13) `union()`

: 두개이상의 쿼리셋들의 결과를 합치는것

`qs1.union(qs2,qs3)`

: 분명한 값만 인자로 받을수 있다. 중복된 값을 넣기를 원한다면  `all=True`를 인자를 ㅆ면 된다.

(14) `intersection()`

`qs1.intersection(qs2,qs3)`

: qs2와 qs3의 공통된 요소를 반환한다.

(15) `difference()`

: ??

(16) `select_related(fields)`

: 외래키나 관련된 객체의 모든 데이터를 가져오는것이다.

```
e = Entry.objects.get(id=5)

b = e.blog
```
를 

```
e = Entry.objects.select_related('blog').get(id=5)

b = e.blog
```

로 표현하면 데이터베이스에 접촉하는것을 1번으로 줄일 수 있다.

```
from django.utils import timezone

blogs = set()
# 미래에 게시될 모든 기사목록의 블로그를 발견하라

for e in Entry.objects.filter(pub_date__gt=timezone.now()).select_related('blog')

	blogs.add(e.blog)
```



```
Entry.objects.fitler(pub_date__gt=timezone.now()).select_related('blog')

Entry.objects.select_related('blog').filter(pub_date__gt = timezone.now())
```

: `filter`과 `select_related`의 순서는 중요하지 않다.

```
from django.db import models

class City(models.Model):
	pass
class Person(models.Model):
	
	hometown = models.ForeignKey(
	City,
	on_delete = models.SET_NULL,
	blank=True,
	null=True,
	)
class Book(models.Model):
	author = models.ForeignKey(Person,on_delte=models.CASCADE)
```

```
b = Book.objects.select_related('author__hometown').get(id=4)
## 외래키로 연결된 Person클래스의 hometown속성을 사용한다.

p = b.author
c = p.hometown

## 데이터베이스에 접촉하지 않는다.

b = Book.objects.get(id=4)
p = b.author
c = p.hometown

## 데이터베이스에 접촉한다.

```

: `ForeignKey`와 `OneToOneField`의 관계는 `select_related()`로 사용할 수 있다.

`without_relations = queryset.select_related(None)`으로 설정할경우, 관련된 객체의 리스트가 `clear`된다.

(17) `prefetch_related()`

: `manytomany`관계에서 관련된 객체의 데이터를 가져오는 방법

```
from django.db import models

class Topping(models.Model)
	name = models.CharField(max_length=30)
	
class Pizza(models.Model)
	name = models.CharField(max_length=50)
	toppings = models.ManyToManyField(Topping)
	
	def __str__(self):
		return "%s (%s)" % (self.name,','.join(topping.name for topping in self.toppings.all()),
		)
```


`Pizza.objects.prefetch_related('toppings')`

: 각각의 피자에 `self.toppings.all()`을 의미한다. 데이터베이스에 직접가서 찾는것을 대신한다.

```
class Restaurant(models.Model):
	pizzas = models.ManyToManyField(Pizza,related_name='restaurants')
	best_pizza = models.ForeignKey(Pizza,related_name='championed_by')
```

` Restaruant.objects.prefetch_related('pizzas__toppings')`

: 레스토랑,피자,토핑 즉 세개의 데이터베이스 쿼리의 모든 결과를 가져올 것이다.

`Restaurant.objects.prefetch_related('best_pizza__toppings')`

: 각강의 레스토랑의 베스트 피자의 모든 토핑과 베스트피자를 가져올것이다. 

`Restaurant.objects.select_related('best_pizza').prefetch_related('best_pizza__toppings')`

: `ForeignKey`로 연결된 `best_pizza`는 `selected_related`를 사용할 수 있다. 이것이 가능한 이유는 `best_pizza`의 객체는 이미 `fetched`되었다. ??

`non_prefetched = qs.prefetch_related(None)` 을 통해 `prefetch_related`행위를 초기화 할 수 있다.

```
from django.db.models import Prefetch

Restaurant.objects.prefetch_related(Prefetch('pizzas__toppings'))
```

: `Prefetch`객체를 사용할수 있다.

`Restaurant.objects.prefetch_related(Prefetch('pizzas__toppings',queryset=Toppings.objects.order_by('name')))`

`Pizza.objects.prefetch_related(Prefetch('restaurants',queryset=Restaurant.objects.select_related('best_pizza')))`

```
vegetarian_pizzas = Pizza.objects.filter(vegetarian=True)

Restaurant.objects.prefetch_related(Prefetch('pizzas', to_attr='menu'),Prefetch('pizzas',queryset=vegetarian_pizzas,to_attr='vegetarian_menu'))

```

: `to_attr`인자를 사용하여, 선택적인 결과를 가져올수 있다.

```
vegetarian_pizzas = Pizza.objects.filter(vegetarian=True)
Restaurant.objects.prefetch_related(Prefetch('pizzas',queryset=vegetarian_pizzas,to_attr='vegetarian_menu','vegetarian_menu__toppings')

```

```
queryset = Pizza.objects.filter(vegetarian=True)

##추천하는 방식

restaurants = Restaurant.objects.prefetch_related(Prefetch('pizzas',queryset=queryset,to_attr='vegetarian_pizzas'))

vegetarian_pizzas = restaurants[0].vegetarian_pizzas

```

(18) `extra()`

: 복잡한 `WHERE`,`tables`sql문을 상세히 표현 하기 위해서 `extra()`를 사용하여 쿼리문에서 `sql`문처럼 표현할 수 있다.

```
qs.extra(select = {'val' : select~~},select_params=(someparam,),
)
```

==

`qs.annotate(val=RawSQL("select ~ = %s",(someparam,)))`

* extra와 같이 쓸수 있는 속성

* select

`Entry.objects.extra(select = {'is_recent' : "pub_date > '2006-01-01'"})`

```
Blog.objects.extra(
	select=OrderedDict([('a','%s'),('b','%s')]),
select_params =('one','two'))
```

* where/tables

`Entry.objects.extra(where=["foo='a' OR bar = 'a'", "baz = 'a'"])`

sql문에서는

`SELECT* FROM blog_entry WHERE (foo='a' or bar ='a') AND (baz='a')`

* order_by

```
q = Entry.objects.extra(select = {'is_recent': "pub_date>'2006-01-01'"})

q = q.extra(order_by = ['-is_recent'])
```

* params

`Entry.objects.extra(where=['headline=%s'],params=['Lennon'])`

: `WHERE`에 들어갈 값을 포함하는것 대신에 `params`을 사용하는것이 좋다.

(19) `defer(fields)`

: 필드를 배제하는것

`Entry.objects.defer("headline","body")`

`Entry.objects.defer("body").filter(rating=5).defer("headline")`


`Blog.objects.select_related().defer("entry__headline","entry__body')`


```
class CommonlyUsedModel(models.Model):
    f1 = models.CharField(max_length=10)

    class Meta:
        managed = False
        db_table = 'app_largetable'

class ManagedModel(models.Model):
    f1 = models.CharField(max_length=10)
    f2 = models.CharField(max_length=10)

    class Meta:
        db_table = 'app_largetable'

# Two equivalent QuerySets:
CommonlyUsedModel.objects.all()
ManagedModel.objects.all().defer('f2')

```


(20) `only(fields)`

: `defer()`의 반대이다. 그 필드속성들만 가져오는것.

```
Person.objects.defer("age","biography")

Person.objects,only("name')
```

: 같은 의미이다.

`Entry.objects.only("body","rating").only("headline")`

`Entry.objects.only("headline", "body").defer("body")`

: ??

`Entry.objects.defer("body").only("headline","body")

: ??


(21) `using(alias)`

: 별칭을 사용할때,

`Entry.objects.using('backup')`

(22) `select_for_update()`

`entries = Entry.objects.select_for_update().filter(author=request.user)`


(23) `raw()`

: SQL 쿼리문을 가져와 사용하고 리턴해준다. 


## Methods that dont return QuerySets

: 어떤것을 반환해주지 않는 메서드

(1) `get(키워드인자모음)`

: 하나의 값만 가져온다.

```
entries = Entry.objects.select_for_update().filter(author=request.user)

```

`entry = Entry.objects.filter(...).exclude(...).get()`


(2) `create(키워드인자모음)`

`p = Person.objects.create(first_name=??)`

(3) `get_or_create()`

```
obj, created = Person.objects.get_or_create(
    first_name='John',
    last_name='Lennon',
    defaults={'birthday': date(1940, 10, 9)},
)
```

: `defaults`에 넣는 인자는 `get`요청할 것이고, `get_or_create()`는 객체의 튜플로 반환된다.

```
params = {k: v for k, v in kwargs.items() if '__' not in k}
params.update({k: v() if callable(v) else v for k, v in defaults.items()})
obj = self.model(**params)
obj.save()
```


`Foo.objects.get_or_create(defaults__exact='bar', defaults={'defaults': 'baz'})`


(4) `update_or_create()`

: (object,created)로 반환된다.

```
default = {'first_name':'bob'}
try:

	obj = Person.objects.get(first_name='john',last_name='lennon')
	for key,value in defaltes.items():
	setattr(obj,key,value)
	obj.save()
	
except Person.DoesNotExist:
	new_values= {'first_name': 'john','last_name' : 'lennon'}
	new_values.update(defaults)
	obj = Person(**new_values)
	obj.save()
```

```
obj,created = Person.objects.update_or_create(first_name='john',last_name='lennon',defaults={'first_name':'bob'},)

```

(5) `bulk_create()`

```
Entry.objects.bulk_create([
	Entry(headline='this is a test'),
	Entry(headline='this is only a test'),])
```
	
	
	
(6) `count()`

`Entry.objects.filter(headline__contains='Lennon').count()`

(7) `in_bulk()`

: `primary_key` 의 값을 딕셔너리로 가져온다.

```
>>> Blog.objects.in_bulk([1, 2])
{1: <Blog: Beatles Blog>, 2: <Blog: Cheddar Talk>}
```


(8) `iterator()`

??

(9) `latest()`

: `field_name`,date을 사용하여 최근의 객체를 반환한다.

`Entry.objects.latest('pub_date')`

(10) `earliest()`

: ??

(11) `first()`

: 쿼리셋에서 처음 매칭된 객체를 반환한다.

`p = Article.objects.order_by('title','pub_date').first()`

```
try:
    p = Article.objects.order_by('title', 'pub_date')[0]
except IndexError:
    p = None
```

(12) `last()` 

: 마지막 객체를 반환한다.

(13) `aggregate()`

: 평균,합등등 연산 값을 딕셔너리로 반환한다.

```
from django.db.models. import Count

q = Blog.objects.aggregate(Count('entry'))
>{'entry__count': 16}
```

```
q = Blog.objects.aggregate(number_of_entires = Count('entry'))
```

(14) `exists()`

```
entry = Entry.objects.get(pk=123)
if some_queryset.filter(pk=entry.pk).exists():
    print("Entry contained in queryset")
```

(15) `update(키워드인자모음)`

`Entry.objects.filter(pub_date__year=2010).update(comments_on=False)`

`Entry.objects.filter(pub_date__year=2010).update(comments_on=False, headline='This is old')`

`Entry.objects.filter(blog__id=1).update(comments_on=True)`


```
Entry.objects.filter(id=64).update(comments_on=True)
> 1
Entry.objects.filter(slug='nonexistent-slug').update(comments_on=True)
>0
Entry.objects.filter(pub_date__year=2010).update(comments_on=False)
>132
```

```
e = Entry.objects.get(id=10)
e.comments_on = False
e.save()
```

`Entry.objects.filter(id=10).update(comments_on=False)`

: `save()`가 포함된다.

(16) `delete()`

```
b = Blog.objects.get(pk=1)
Entry.objects.filter(blog=b).delete()
>(4, {'weblog.Entry': 2, 'weblog.Entry_authors': 2})
```


# Field lookups

(1) `exact`

```
Entry.objects.get(id__exact=14)
Entry.objects.get(id__exact=None)
```

(2) `iexact`

```
Blog.objects.get(name__iexact='beatles blog')
Blog.objects.get(name__iexact=None)
```


(3) `contains`

`Entry.objects.get(headline__contains='Lennon')`

(4) `icontains`

`Entry.objects.get(headline__icontains='Lennon')`

(5) `in`

`Entry.objects.filter(id__in=[1, 3, 4])`

```
inner_qs = Blog.objects.filter(name__contains='Cheddar')
entries = Entry.objects.filter(blog__in=inner_qs)
```

```
inner_qs = Blog.objects.filter(name__contains='Ch').values('name')
entries = Entry.objects.filter(blog__name__in=inner_qs)
```

(6) `gt`

: ~보다 큰

`Entry.objects.filter(id__gt=4)` : 4보다 큰

(7) `gte`

: 같거나 큰

(8) `lt`

: ~보다 적은

(9) `lte`

: ~보다 같거나 작은

(10) `startswith` 

: 어떠한 패턴으로 시작하는 값

`Entry.objects.filter(headline__startswith='Lennon')`

(11) `istartwith`

`Entry.objects.filter(headline__istartswith='Lennon')`

(12) `endswith`

`Entry.objects.filter(headline__endswith='Lennon')`

(13) `iendswith`

`Entry.objects.filter(headline__iendswith='Lennon')`

(14) `range`

```
mport datetime
start_date = datetime.date(2005, 1, 1)
end_date = datetime.date(2005, 3, 31)
Entry.objects.filter(pub_date__range=(start_date, end_date))
```

(15) `date`

```
Entry.objects.filter(pub_date__date=datetime.date(2005, 1, 1))
Entry.objects.filter(pub_date__date__gt=datetime.date(2005, 1, 1))
```


(16) `year`

```
Entry.objects.filter(pub_date__year=2005)
Entry.objects.filter(pub_date__year__gte=2005)
```

(17) `month`

```
Entry.objects.filter(pub_date__month=12)
Entry.objects.filter(pub_date__month__gte=6)
```

(18) `day`

```
entry.objects.filter(pub_date__day=3)
Entry.objects.filter(pub_date__day__gte=3)
```


(19) `week`

```
Entry.objects.filter(pub_date__week=52)
Entry.objects.filter(pub_date__week__gte=32, pub_date__week__lte=38)
```

(20) `week_day`

: 1(sunday) to 7(saturday)

```
Entry.objects.filter(pub_date__week_day=2)
Entry.objects.filter(pub_date__week_day__gte=2)
```

(21) `time`

```
Entry.objects.filter(pub_date__time=datetime.time(14, 30))
Entry.objects.filter(pub_date__time__between=(datetime.time(8), datetime.time(17)))
```

(22) `hour`

```
Event.objects.filter(timestamp__hour=23)
Event.objects.filter(time__hour=5)
Event.objects.filter(timestamp__hour__gte=12)
```

(23) `minute`

```
Event.objects.filter(timestamp__minute=29)
Event.objects.filter(time__minute=46)
Event.objects.filter(timestamp__minute__gte=29)
```

(24) `second`

```
Event.objects.filter(timestamp__second=31)
Event.objects.filter(time__second=2)
Event.objects.filter(timestamp__second__gte=31)
```

(25) `isnull`

: `True`또는 `False`를 반환한다. 

`Entry.objects.filter(pub_date__isnull=True)`

(26) `search`

`Entry.objects.filter(headline__search="+Django -jazz Python")`

(27) `regex`

: 정규표현식을 통해 표현하고 싶을떄,

`Entry.objects.get(title__regex=r'^(An?|The) +')`

(28) `iregex`

`Entry.objects.get(title__iregex=r'^(an?|the) +')`

# Aggregation functions

(1) `Q() objects` : | (OR) and & (AND) operators를 사용할수 있으며 , 사용할때 항상 Q를 앞에 붙여야 한다.

(2) `Prefetch() objects`

```
from django.db.models import Prefetch

Question.objects.prefetch_related(Prefetch('choice_set')).get().choice_set.all()

>>> <QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> 
Question.objects.prefetch_related(Prefetch('choice_set')).all() 
>>> <QuerySet [<Question: What's up?>]>
```

```
voted_choices = Choice.objects.fitler(votes__gt=0)

voted_choices
> <QuerySet [<Choice: The sky>]>

prefetch = Prefetch('choice_set',queryset=voted_choices)

> Question.objects.prefetch_related(prefetch).get().choice_set.all()
> <QuerySet [<Choice: The sky>]>
```

```
prefetch = Prefetch('choice_set',queryset=voted_choices,to_attr='voted_choices')

Question.objects.prefetch_related(prefetch).get().voted_choices

><QuerySet [<Choice: The sky>]>

Question.objects.prefetch_related(prefetch).get().choice_set.all()

> <QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
```

(3) `prefetch_related_objects()`

```
from django.db.models import prefetch_related_objects

restaurants = fetch_top_restaurants-form_cache() 
# 레스토랑의 리스트를 가져와라

prefetch_related_objects(restaurants,'pizzas__toppings')
```



ASDFSADFASDF















