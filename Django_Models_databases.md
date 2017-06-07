# Models and databases

: 하나의 모델은 데이터에 대한 소스이고, 본질적인 필드(제목열)들과 우리가 저장하는 데이터에 대한 동작이 포함되며, 각 모델은 단일 데이터베이스 테이블(관련데이터 항목의 모음이며 열과 행으로 구성)에 매핑된다.

# 항목

* Models
* Making queries(쿼리셋은 전달받은 모델의 객체 목록)
* Aggregation(집합체)
* Search
* Managers
* Performing raw SQL queries
* Database transactions
* Multiple databases
* Tablespaces
* Database access optimization
* Examples of model relationship
* API usage

# Models

: 각각의 모델은 `django.db.models.Model` 에 속하는 하위 파이썬 클래스다.

: 모델의 각각의 속성은 `데이터 필드`를 나타낸다.

: 장고는 자동적으로 만들어지는 데이터베이스에 대한 접근(API)를 제공한다.

* API(응용 프로그램 프로그래밍 인터페이스)는 응용프로그램에서 사용할 수 있도록, 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 말한다(파일제어,창제어,화상처리,문자제어등등)

```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

: 이모델은 사람이라는 클래스를 정의하고 필드명은 성과 이름이다. 각각의 `필드`는 `클래스의 속성`에 대해 구체화 되며, 각각의 `속성`은 데이터베이스의 `컬럼`에 맵핑된다.

```
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```

: override(재정의), `primary key`를 따로 지정하면 `id`라는 필드가 생기지 않는다.

![](/Users/mac/projects/images/스크린샷 2017-06-04 오후 12.52.19.png)

: 테이블의 네임은 `myapp_person`, `id`필드는 자동적으로 생성, 

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

: `sqlite3`라고 지정해놨다.

## using models

: 모델을 정의했다면, 장고에게 이 모델을 사용할 것이라고 알려줘야할 필요가 있다. `settings.py`에 있는 `INSTALLED_APPS`세팅에 만든 모듈의 이름을 더해줘야 한다. 

`python manage.py startapp '모듈명'`


```
INSTALLED_APPS = [
    #...
    'myapp',
    '모듈명',
    #...
]
```
: `INSTALLED_APPS`에다 `모듈명`을 추가해줘야한다.

새로운 모듈명을 추가할때 마다, `./manage.py makemigrations`으로 변경사항을 저장한후, `./manage.py migrate`로 실제 데이터베이스에 모델을 반영

## Fields

: 모델에서 가장 중요한 부분으로, 데이터베이스 필드의 목록을 만드는것이다. **필드들은 클래스 속성**에 따라 구체화되며, `clean`,`save`,`delete`는 모델 api하고 충돌하므로, 이름을 잘지어야 한다.

```
from django.db import models

class Musician(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)

class Album(models.Model):
    artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()
```

: `artist = models.ForeignKey(Musician,on_delete=modles.CASCADE)`에서 `Album`클래스에서 외래키를 사용하여 `Musician`과 `one to many`연계를 해주었다. 그래서 `모듈명_Album`테이블에서 `artist_id`가 생긴것을 볼 수 있다.

![](/Users/mac/projects/images/스크린샷 2017-06-04 오후 1.03.53.png)

: 모델의 각각의 필드는 적절한 필드 클래스의 `인스턴스명`을 가지며, `column type`은 저장하는 데이터베이스종류를 말한다. (`INTERGER`,`VARCHAR`,`TEXT`)

: `HTML WIDGET`에서 `models.py`에서 `forms.py`가 없어도, `textfield``charfield`를 사용하여 `form`요소를 만들 수 있다. `form field`를 렌더링 하는것은 (`input type = text`,`select`)

# Writing custom model fields

: 전화번호,주소 목록을 만들어서 온라인과 연동하여 실제로 이주소와 일치하는지 유효성검사 필드를 만들 수 있다.

## Field options

: 필드명을 구성하는데 공통된 인자(arguments)가 있다. 선택사항이다.

```
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```


* null

: if True, 장고는 텅빈값을 `NULL`로써 데이터베이스에 저장한다. `NULL=True`로 선언

* blank

: `null`과 다른점은 `blank`는 빈 문자열을 의미하며, `NULL`은 아예 없다는것을 의미. `datetime field`는 빈값을 넣을 수가 없다. 문자열이 아니기때문이다.`blank=True` 로 선언하여, 빈 값으로 줄 수 있다.

* choices

: 순환할수 있는 객체(튜플의 리스트) 필드명에서 사용할때는 두개씩 괄호로 묶어서 선택지를 만든다. `textfield`대신 `charfield`를 준다면, `select box`를 사용하여 렌더링한다.

```
YEAR_IN_SCHOOL_CHOICES = (
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
)
```

: 각각의 튜플에서 첫번째 요소는 데이터베이스에 저장되고, 두번째 요소는 폼요소 widget이나 `ModelChoiceField`안에 있을 수 있다. 주어진 모델의 인스턴스는 선택지를 보여주고, 선택지에 대한 값을 보려면, **`get_FOO_display()`메서드**를 사용한다. 예를 들어)

```
from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
    
```

![](/Users/mac/projects/images/스크린샷 2017-06-05 오전 11.26.20.png)

: `SHIRT_SIZES`라는 인스턴스에 튜플로 선택지를 만들고, `shirt_size`라는 인스턴스에 `choices=SHIRT_SIZES`를 넣는다.
주의할점은 `choices`

```
>>> p = Person(name="Fred Flintstone", shirt_size="L")
>>> p.save()
>>> p.shirt_size
'L'
>>> p.get_shirt_size_display()
'Large'

```

: p라는 인스턴스에 `Person`클래스에 속성값 `name`과 `shirt_size`에 대한 값을 넣고 `p.save()`로 저장하고, `p.get_shirt_size_display()`을 사용하여 `L`라는 값 `Large`를 표현한다.

* default

: 필드에서의 객체의value(값)이다.

* help_text

: 너의 필드에서 폼 요소가 사용되지 않을지라도, 유용하다. 폼 widget에서 보여질수 있다. 

* primary_key(read-only)

: 필드는 각 모델에 대한 primary key이며, 장고에서는 자동적으로 `primary_key=True`를 생성해준다. 만약 `primary_key`의 값을 바꾸고 싶다면, 새로운 객체는 예전의 값을 놔두고 새롭게 만든다. 예를들어)

```
from django.db import models
class Fruit(models.Model):
	name = models.CharField(max_length=100, primary_key = True)
```
```
fruit = Fruit.objects.create(name='Apple')
fruit.name = 'Pear'
fruit.save()
**Fruit.objects.value_list('name', flat=True)
>>> ['Apple','pear']**
```


* unique

: 필드 속성에 대한 공통적인 짧은 설명이다. 

## automatic primary key fields

: 장고는 기본적으로 필드에 해당하는 각각의 모델에 `id = models.AutoField(primary_key=True)`를 부여한다. 하나씩 추가할때마다 값이 하나씩 증가, 각각의 모델은 명백히 선언되었거나, 자동적으로 부여된 `primary_key =True`를 가진 하나의 필드만을 요구한다.

: 만약 `primary key`를 커스텀하고 싶다면, 너의 필드중 하나에서 `primary_key=True`를 구체화 하면 된다. `name = models.CharField(max_length=100, primary_key = True)`와 같은 예

## verbose field names

: 각각의 필드타입에서, `ForeignKey`,`ManyToManyField`와 `OneToOneField`를 제외하고, **첫번째 오는 위치인자를 verbose field name으로 지정한다.** 예를 들어)

필드명을 `"person's first name"`하고 싶다면, `first_name = models.CharField("person's first name", max_length=30)`이며, 따로 위치인자를 주지 않을경우, `first_name = models.CharField(max_length=30)` **인스턴스이름이 필드명이 된다.**

: `ForeignKey`,`ManyToManyField`와 `OneToOneField`을 쓸경우 필드명을 설정할때는 따로 키워드 인자로 ** `verbose_name : 필드명`을 설정해줘야 한다. 예를 들어)

```
polls = models.ForeginKey(poll,on_delete=models.CASCADE,verbose_name='the related poll',)

sites = models.ManyToManyField(site, verbose_name='list of sites')

place = models.OneToOneField(place,on_delete=models.CASCADE,verbose_name="related place',)

```

: `verbose name`에서 첫번째 문자가 대문자가 올 필요는 없다. 장고는 자동적으로 `verbose name`을 대문자화 해준다.

## Relationships

: 데이터베이스 관계요소를 설정하고 싶을떄, 새개의 공통된 속성을 제공한다.

* Many-to-one-relationships

: `Many-to-one-relationships`을 사용한다면, `django.db.models.ForeignKey.`을 사용해야 한다. 

`ForeignKey`은 `many쪽`에다 걸어준다.

```
from django.db import models

class Reporter(models.Model):
	first_name = models.CharField(max_length=30)
	last_name = models.CharField(max_length=30)
	email = models.EmailField(blank=True)
	
	def __str__(self):
	 	return '{} {}'.format(
	 	self.first_name,
	 	self.last_name,
	 	)
	 
class Article(models.Model):
	headline = models.CharField(max_length=30)
	pub_date = models.DateField()
	reporter = models.ForeignKey(Reporter,on_delete=models.CASCADE)
	
	def __str__(self):
		return self.headline
	class Meta:
		ordering = ('headline',)
```


```
In [1]: r = Reporter(first_name='junho', last_name='kwon')

In [2]: r.save()

In [3]: r2 = Reporter(first_name='wonho',last_name='kwon')

In [4]: r2.save()

```

: `r`과 `r2`에 `Reporter`클래스를 이용하여, 리포터들을 만든다.

```
In [6]: a = Article(id=None, headline="This is a test",pub_date=date(2005,3,3), 
   ...: reporter = r)
   
In [8]: a.reporter.id
Out[8]: 2

In [9]: Reporter.objects.all()
Out[9]: <QuerySet [<Reporter: john smith>, <Reporter: junho kwon>, <Reporter: wonho kwon>]>

In [10]: a.reporter
Out[10]: <Reporter: junho kwon>
```

: 항상 `save()`로 저장해야한다. `Article`클래스에서 필요한 위치인자는 `headline`,`pub_date`,`reporter` 3개이다. `reporter` 는 `Foreign`키로 `Many`인 `Reporter`이다.

```
In [11]: r
Out[11]: <Reporter: junho kwon>

In [12]: new_article = r.article_set.create(headline='good', pub_date=date(2017,
    ...: 3,3))

In [13]: new_article
Out[13]: <Article: good>

```

: 리포트이름을 저장한 `r`인스턴스를 활용하고, 역참조 `article_set`매니저명을 활용하여 `new_article`라는 인스턴스에 새로운 기사를 만들었다. `a = Article(id=None, headline="This is a test",pub_date=date(2005,3,3), 
   ...: reporter = r)`와 같은 의미이다.
   
```
>>> new_article2 = Article(headline="Paul's story", pub_date=date(2006, 1, 17))
>>> r.article_set.add(new_article2)
>>> new_article2.reporter
```

: `new_article2`라는 인스턴스를 만들고 add(새로운기사)를 통해 추가할 수 있다.

`Article.objects.filter(reporter__in=[1,2]).distinct()` : reporter id가 1,2인값을 가져와라,

`Article.objects.filter(reporter__in=[r,r2]).distinct()` : 리포터 인스턴스 r과 r2인 기사를 가져와라

`Reporter.objects.order_by('first_name')` : `first_name`순으로 정렬

`Reporter.objects.filter(article__reporter__first_name__startswith='John').distinct()` : 기사를 쓴 리포트이름이 john으로 시작하는 리포터를 찾아라.
	
	


* **ForeignKey**

: 연계하고자 하는 모델클래스를 위치인자로 받는다. `ForeiginKey`를 쓰는 클래스는 하위모델에 쓴다. 하위모델이 `1`, 소스모델이 `Many`다.

```
from django.db import models

class Manufacturer(models.Model):
	pass
class Car(models.Model):
	manufacturer = models.Foreignkey(Manufacturer,on_delete=models.CASCADE)
```

: `Recursive relationship` : 자기자신의 클래스를 다시 가지는것 : `models.ForeignKey('self',on_delete=models.CASCADE)`

```
class Person(models.Model):

 PERSON_TYPES =(
        ('student','학생'),
        ('teacher','선생'),
    )

    person_type = models.CharField(
        '유형',
        max_length=10,
        choices=PERSON_TYPES,
        default=PERSON_TYPES[0][0]
    )
    teacher = models.ForeignKey(
        'self',
        null=True,
        blank=True,
        on_delete=models.CASCADE,)
```

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 12.38.00.png)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 12.38.41.png)

: `null=True`,`blank=True`을 사용하면 `null`값으로 보인다.

## Following relationshop backward(역참조)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 12.45.59.png)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 12.48.42.png)

: `Foreignkey`를 가지는 모델이 있다면, `foreign-key model`의 인스턴스는 `Manager`로 접근이 가능하다. `하위모델명(소문자)_set`

```
class Car(models.Model):
    name = models.CharField(max_length=40)
    manufacturer = models.ForeignKey(Manufacturer,on_delete=models.CASCADE)

    def __str__(self):
        return self.name

class Manufacturer(models.Model):
    name = models.CharField(max_length=40)

    def __str__(self):
        return self.name

```
`m = Manufacturer(name="bmw")`
`Car.objects.create(name="520d",manufacturer = m)` : 생성방법

`Car.objects.filter(name__startswith='5')`이 아닌, `m=Manufacturer(name="bmw")`, `m.car_set.filter(name__startswith='5')`

: `car_set`은 `foreign-key model`의 인스턴스는 `manufacturer`이며, 이 인스턴스는 `Manager`로 접근이 가능하기에 `Manager`명 `car_set`으로 접근한다.


## Many-to-Many relationships

: 다대다 관계를 설정하기 위해서는 `ManyToManyField`를 사용하여 위치인자를 주어야 한다. pizza클래스가 topping 클래스를 가지는것이 자연스럽다. 그래서 pizza폼에서 유저가 토핑을 선택하도록 한다.

```
from django.db import models


class Topping(models.Model):
    name = models.CharField(max_length=30)

    def __str__(self):
        return self.name

    class Meta:
        ordering =('name',)

class Pizza(models.Model):
    name = models.CharField(max_length=30)
    toppings = models.ManyToManyField(Topping)

    def __str__(self):
        return self.name

    class Meta:
        ordering = ('name',)

```

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 3.14.12.png)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 3.15.26.png)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 3.20.08.png)
`t_cheese.toppings.add(t_vegetable)`과 같이 `다대다관계`에서 `toppings`라는 인스턴스를 활용하여 접근할 수 있다.(1대다일경우는 `함수명_set.`으로 접근만 가능)

`t_cheese.pizza_set.all()` : 치즈토핑이 있는 피자를 모두 가져와라 (역참조)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 3.23.21.png)

: `Pizza`와 `Topping` 필드가 아닌 새로 `Pizza_and_Topping` 테이블이 새로 생긴다.

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 3.26.50.png)

`p_supersupreme.toppings.create(name='야채')` : 아예 `Many manage`를 사용하여 야채를 추가하고, 저장한다. `toppings`는 인스턴스로 `1대다`의 관계에서는 인스턴스로 접근 할수 없다. 

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 3.29.37.png)

: 상태보기(`print(q.query)`)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 3.32.20.png)

: `topping_id`가 1이거나 2(`lte=2`)인 피자를 가져와라

![]( /Users/mac/projects/images/스크린샷 2017-06-05 오후 3.35.37.png)

`Topping.objects.filter(pizza__name__startswith="치즈")` 와 `p_cheese.toppings.all()`은 같다.

`t_cheese.pizza_set.create(name="야채") >>> <Pizza: 야채 (치즈)>`

`p_vegetable.toppings.create(t_cheese)` 과 같은 의미이다.


* 피자명 옆에 토핑명을 같이 쓰고 싶을때,

```
from django.db import models

class Topping(models.Model):
	name=models.CharField(max_length=30)
	def __str__(self):
		return self.name
	class Meta:
		ordering('name',)
		
class Pizza(models.Model):
	name = models.CharField(max_length=30)
	toppings = models.ManyToManyField(Topping)
	
	def __str__(self):
		toppings_string = ""
		for topping in self.toppings.all():
		toppings_string += topping.name
		topping_strign +=','
		
		topping_string = toppings_string[:-2]
		return '{} ({})'.format(self.name,toppings_string)
		
return '{} ({})'.format(self.name,','.join[(t.name for t in self.toppings.all())])
```
 
 



# Extra fields on many to many relationships(중간모델)

: 두 모델사이의 관계를 설정하고 싶을때 필요하며, 예를 들어) 이 피자에 이 토핑이 언제부터 만들어졌나?라는 디테일 요구할때,구단에서 축구선수가 이적하거나 들어올때의 디테일을 요구할경우, `intermediate model`에서 `extra fields`를 추가해야한다. `intermediate` 모델은 `ManyToManyField`와 연관되어 있으며 `through`를 사용하여 중계자역할을 하게끔한다. 중간자 모델은 `Foreginkey`는 하나만 쓸 수 있다. `중간모델(intermediate model)`을 설정하는 이유는 두 모델간의 공통점을 표현하기 위한것이다.

```
from django.db import models

class Player(models.Model):
    name = models.CharField(max_length=30)

    def __str__(self):
        return self.name

class Club(models.Model):
    name = models.CharField(max_length=40)
    players = models.ManyToManyField(
        Player,
        through='TradeInfo',
    )

    def __str__(self):
        return self.name

class TradeInfo(models.Model):
    player = models.ForeignKey(Player,on_delete=models.CASCADE)
    club = models.ForeignKey(Club,on_delete=models.CASCADE)
    date_joined = models.DateField()
    date_leave = models.DateField(null=True,blank=True)

```

`players = models.ManyToManyField(player,through ='TradeInfo',)`은 

**`Player`,`Club`과 `TradeInfo`를 연결해주는 선언이다. `Club`과 `Player`을 `ManyToManyField`로 연결해주고, `through`를 통해 중간자 모델을 `TradeInfo`로 연결해준다. **


: 중간자모델 : `TradeInfo`

: 소스모델 : `Club`

: 대상모델 : `players`

: `TradeInfo`가 중간모델이고, 	`Club`과 `Person`의 공통요소로 쓸것은 `date_joined`와 `date_leaved`다.


![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 4.38.57.png)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 4.39.19.png)

* property를 사용하여 현재 속하는 TradeInfo을 리턴

`Player 모델에서 `

```

(1) @property
def current_tradeinfo(self):
	return TradeInfo.objects.get(
	player_id = self.id,
	trade__date_joined_lte = timezone.now(),
	trade__date_leaved = None,)
	
(2) @property
def current_tradeinfo(self):
	return self.tradeinfo_set.get(date_leaved__isnull=True)
```


* property를 사용하여 현재 속하는 Club을 리턴(해당선수의 가입일자가 현재보다 이전이고, 떠난적이 없을때)

`Player 모델에서 `

```

(1) @property
def current_club(self):
	return self.club_set.get(
	tradeinfo__date_joined_lte = timezone.now(),
	tradeinfo__date_leaved = None)
	
(2) @property
def current_club(self):
	return current_tradeinfo.club
	
```
	
* squad메서드에 현직선수들만 리턴, 인수로 년도를 받아 해당년도의 선수들을 리턴, 주어지지 않는다면, 현재를 기준으로 함

2015년도에 현직이었던 선수일경우,

들어온날짜는 2016,1,1보다는 작아야한다.

나간날짜는 2015,1,1보다는 커야 한다.



`Club모델에서 `


```
def squad(self,year=None):
	if year:
		return Q(self.players.filter(tradeinfo__date_joined__lte=date(year+1,1,1)) & Q(self.players.filter(tradeinfo__date_leaved__gte =date(year,1,1) | Q(self.players.filter(tradeinfo_date_leaved__isnull)
	else:
		return self.players.filter(tradeinfo__date_leaved_isnull = True)
		
```

:`lte`는 이전, `gte`는 이후
: Q 함수는 a or b처럼 연산관계를 리턴해주는것이다.



* Tradeinfo에서 recommender와 prev_club을 활성화 시키고, Club의 MTM필드에 through_fields를 활성화

: `TradeInfo`에서 `recommender`에도 외래키로 `Player`로 연결되있고, `player`에도 `Player`로 연결되어 있기에, 역참조로 불러올때, 만약 `player's name.tradeinfo_set`일때, `TradeInfo`테이블에서 어떤 필드를 참조해야할지 모르기 때문에 `through_fields`를 설정해 줘야 한다.`through_fields = (club,player)`로 설정하여 `club`인스턴스와 `player`인스턴스는 `tradeinfo_set`을 쓸 수 있지만, `recommender`,`prev_club`의 역참조 이름을 새로 정해줘야 한다. `TradeInfo`메서드에서 `related_name ="tradeinfo_set_by_recommender"`
`related_name = "tradeinfo_set_by_prev_club"`을 설정해줘야 한다.



```
(1)
class Club(models.Model):	players=models.ManyToMany(Player,through='TradeInfo',through_fields=('club','player'),)


(2) class TradeInfo(model.Model):

recommender = models.ForeignKey(
	Player,
	on_delete = models.PROTECT,
	related_name = 'tradeinfo_set_by_recommender',
	null=True,
	blank=True,
	)
	
prev_club = models.ForeignKey(
	Club,
	on_delete = models.PROTECT,
	related_name ='+'
	null =True,
	blank= True,)
```


* property로 is_current속성이 TradeInfo가 현재 현직 선수를 가지고 있는지 리턴



```
@property

def is_current(self):
	return self.date_leaved is None
	#return
not self.date_leaved
```

: 여기서 `self`는 `TradeInfo.objects.create`나 `tradeinfo_set.create`로 만든 인스턴스다.

* 선수이름,구단명(시작일자~종료일자)로 나타내라

`TradeInfo`메서드에서 `TradeInfo.objects.create(~)`으로 만들어진 인스턴스


```
def __str__(self):
	return '{},{} ({}~{})'.format(
	self.player.name,
	self.club.name,
	self.date_joined,
	self.date_leaved or "현직"
	
	#self.date_leaved if self.date_leaved else "현직"

```

: `인스턴스.속성`으로 `self.player`,`self.club`,`self.date_joined`,`self.date_leaved`



```
from django.db import models

class Person(models.Model):
	name = models.CharField(max_length=128)
	
	def __str__(self):
		return self.name
		
class Group(models.Model):
	name = models.CharField(max_length=128)
	members = models.ManyToManyField(Person,through='Membership')
	
	def __str__(self):
	return self.name
	
class Membership(models.Model):
	person = models.ForeignKey(person,on_delete=models.CASCADE)
	group = models.Foreignkey(Group, on_delete=models.CASCADE)
	date_joined = models.DateField()
	invite_reason =  models.CharField(max_length=64)
```



: `intermediate model`은 `Membershop`이고, `source model`은 `Group`이다. 여기서 `members = models.ManyToManyField(Person,through='Membership')`를 사용하여 소스모델인 `Group`과 `Person`클래스를 `ManyToManyField`로 설정해줘야 한다. 설정해주지 않는다면 `Person`클래스 하위요소가 `Membership`이라고 장고에서 착각할수 있기 때문이다.  또한 `ForeiginKey`의 위치인자를 같은 모델명(클래스명)에 주는것도 가능하다. 하지만  `source model`에서 `through`를 사용하여 관계를 쓰고자하는 인스턴스를 구체화 해줘야하고, 나머지 같은모델을 참조하는 인스턴스는 속성으로 `related_name`을 설정해줘야 한다.



```
ringo = Person.objects.create(name='ringo')
paul = Person.objects.create(name='paul')
beatles = Group.objects.create(name='beatles')

m1 = Membership(person=ringo, group =beatles, date_joined=date(1962,8,16),invite_reason="needed a new drummer.")

m1.save()

beatles.members.all()
>> <QuerySet [<Person: Ringo Starr>]>
# members라는 인스턴스로 실행(MTOM)
ringo.group_set.all()
>> <QuerySet [<Group: The Beatles>]>
# ringo라는 Person클래스를 담은 인스턴스.group_set(역참조)

m2 = Membership.objects.create(person=paul,group=beatles,date_joined=date(1960,8,1),invite_reason="wanted to form a band.")

beatles.members.all()
>><QuerySet [<Person: Ringo Starr>, <Person: Paul McCartney>]>
```

: `ForeignKey`로 다 연관시켜서 `Membership`클래스의 속성에 `Person`클래스의 인스턴스 `person`과 `Group`클래스의 인스턴스 `group`을 만들어 놓았기 때문에, `Membership`클래스에서 객체를 `create`함수를 통해서 만들 수 있다. 


: `many-to-many fields`에서 `add(), create(), or set() or remove()`는 사용할 수 없다. 왜냐면 `Person`클래스와 `Group`클래스 사이를  직접 연관시키지 않았기 때문이다. 방법은 `intermediate model`즉,  `Membership`클래스에서 인스턴스를 만들어 만드는 방법밖에 없다.


: 삭제하기 위해서는 `clear()`메서드를 사용하면 된다.`beatles.members.clear()
`


```
# Find all the groups with a member whose name starts with 'Paul'
>>> Group.objects.filter(members__name__startswith='Paul')
<QuerySet [<Group: The Beatles>]>
```
```
Person.objects.filter(
	group__name ='The beatles',
	membership__date__joined__gt=date(1961,1,1))
```

: `Group`과 `Person`이 다대다 관계가 설정되었고, `Membership`과 `Many to one`관계가 설정되었기에, `Person`에서 필터링을하여, `group__name`,`membership__date__joined__gte=date(1961,1,1))`이 가능하다.



: `Person`인스턴스에서 시간대를 불러올려면 `Person.objects.filter(membership__date_joined__gte=date(year,?,?)` 즉 `Person`이라는 모델은 `Membership`의 속성을 가져다 쓸 수 있다.



```
(1) ringos_membership = Membership.objects.get(group=beatles,person=ringo)

(2) ringos_membership = ringo.membership_set.get(group=beatles)

```


: (2)번은 membership_set(역참조)를 사용 `person`이나 `group`인스턴스를 그대로 사용하지 못하고 `ForeignKey` 를 사용하여 `1대 Many`를 사용했기에, `membership_set`을 사용해야 한다.


```
ringos_membership.date_joined
>>> datetime.date(1962,8,16)

ringos_membership.invite_reason
>>> 'needed a new drummer
```


```
>>> ringos_membership = Membership.objects.get(group=beatles, person=ringo)
>>> ringos_membership.date_joined
datetime.date(1962, 8, 16)
>>> ringos_membership.invite_reason
'Needed a new drummer.'

```



: `인스턴스.date_joined`에서 인스턴스는 `Membership.objects.get(group=beatles,person=ringo)`


## one to one relationships

: `OneToOneField`를 사용하여 관련된 모델의 위치인자가 필요하다. `User`테이블에 어떠한 요소를 넣고 싶으면, 기존의 테이블은 바꾸지 않고, 확장시킨다.

```
from django.db import models


class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

    def __str__(self):
        return '{} the place'.format(self.name)


class Restaruant(models.Model):
    place = models.OneToOneField(
        Place,
        on_delete=models.CASCADE,
        primary_key=True,
    )
    serves_hot_dogs = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)

    def __str__(self):
        return '{} the restaurant'.format(self.place.name)
 

class Waiter(models.Model):
    restaurant = models.ForeignKey(Restaruant, on_delete=models.CASCADE)
    name = models.CharField(max_length=50)

    def __str__(self):
        return '{} the waiter'.format(self.restaurant)
```

`r = Restaurant(place=p1, serves_hot_dogs=True, serves_pizza=False)`

`r.place`,`p1.restaurant`를 실행해도 `OneToOneField`를 설정해놨기에, `place`,`restaurant`인스턴스로 참조가 가능하다. 여기서 중요한것은 `Restaurant`모델안에 `onetoone`관계를 설정해서 `Place`모델과 연결했다. `place`인스턴스를 활용하여 `r.place`는 원래 `manytomany`에서도 가능했으나,  `Place`모델에서도 `restaurant`를 불러올수 있다. `p1.restaurant`

![](/Users/mac/projects/images/스크린샷 2017-06-07 오후 8.34.00.png)

* p2가 관련된 레스토랑을 가지지 않는다면, 검사하는 방법이 있다.

(1)

```

>>> from django.core.exceptions import ObjectDoesNotExist
>>> try:
>>>     p2.restaurant
>>> except ObjectDoesNotExist:
>>>     print("There is no restaurant here.")
There is no restaurant here.

```
(2)
`hasattr(p2,'restaurant')>>> False`

`Waiter.objects.filter(restaurant__place=p1)` 에서 `Waiter`모델안에는 `restaurant`(외래키 Restaurant연결)과 `name` 두개의 속성이 있다. `restaurant__place`가 가능한 이유는 `restaurant`인스턴스는 외래키로 `Restaurant`와 연결해놨고, `Restaurant`모델안에서는 `Place`와 1대1관계를 맺었기 때문에 가능하다.




## Models across files

```
class Restaruant(models.Model):
	place = models.OneToOneField(
		place,
		on_delete=models.CASCADE,
		primary_key =True,
		)

```

: 관련된 모델을 `import`해야 한다. `alt+enter`을 사용하여 모듈을 `import`한다.

## Field name restrictions

: 이름을 지을때, 파이썬 문법과 관련된 언어로 지으면 안되고(`pass`), 하나의 언더스코어이상의 줄로 된 이름은 지을수 없다.(`for__bar`)

## crating custom fields

: 기본적으로 제공하는 클래스 속성(`VARCHAR and INTEGER`)이 아닌 애매모호한 객체도 만들고 싶다면,

```
class Hand(object):

def __init__(self,north,east,south,west):
	self.north = north
	self.east = east
	self.south = south
	self.west = west
```
: 인스턴스를 선언하지 않더라고, 우리는 hand라는 속성은 Hand클래스의 인스턴스라고 가정할 수 있다.

```
example = MyModel.objects.get(pk=1)
print(example.hand.north)

new_hand = Hand(north,east,south,west)
example.hand = new_hand
example.save()
```

## Meta options

: `class Meta`를 안에 사용함으로써 `metadata`를 모델에 줄 수 있다.

```
from django.db import models
class Ox(models.Model):
	horn_length = models.IntegerField()
	class Meta:
		ordering = ["horn_length"]
		verbose_name_plural = "oxen"
```

: `metadata`는 `ordering`,`db_table`,`verbose_name`,`verbose_name_plural`와 같은것으로 옵션을 넣을때 사용한다.

## Model attributes

* objects

```
from django.db import models

class Person(models.Model):
    #...
    people = models.Manager()
```
 
 
 
: 모델속성에서 가장 중요한 것은 `Manager`이다. 데이터베이스 쿼리셋 운영의 인터페이스며, 데이터베이스로 부터 인스턴스를 가져와 사용하곤 한다. 기본적으로, 장고는 모든 장고모델 클래스마다 객체이름에 따라 매니저이름을 만든다. `people = models.Manager()`은 `Manager`을 위한 객체 이름을 새로 만든것이다. 

## Model methods

: `Manager`메서드는 `table`단위이고, `model`메서드는 특정한 모델 인스턴스에서 실행된다.

```
from django.db import models

class Person(models.Model):
	first_name = models.CharField(max_length=50)
	last_name = models.CharField(max_length=50)
	birth_date = models.DateField()
	
def baby_boomer_status(self):
	import datetime
	if self.birth_date < datetime.date(1945,8,1):
	return "pre-boomer'
	elif self.birth_date < datetime.date(1965,1,1):
	return "baby boomer"
	else:
		return "post-boomer"
	@property
	def full_name(self):
		return '%s %s % (self.first_name, self.last_name)
```

: `Person`클래스의 속성으로 `first_name`,`last_name`,`birth_date`를 사용하고, `baby_boomer_status`라는 메서드(클래스안의 함수)를 만들어서, if,elif문으로 태어난 날짜에 따라 미숙아인지, 정상으로 태어났는지를 구분하고 `property`를 사용하여, `full_name`함수를 만든다. 

## overriding predefined model methods

: 얼마든지 기존의 메서드를 재정의 할수 있다. 개체를 저장할때마다 다른 행위가 나오길 원할때,

```
from django.db import models

class Blog(models.Model):
	name = models.CharField(max_length=100)
	tagline = models.TextField()
	
	def save(self, *args, **kwargs):
		if self.name == "Yoko Ono's blog":
            return
		else:
			super(Blog,self).save(*args, **kwargs)
```

: `save`메서드는 이미 존재하고, `def save(self,*args,**kwargs):` : 기존의 `save`메서드는 인자가 많기에 `*args,**kwargs`로 넣어두고, `if self.name == "Yoko Ono's blog":`은 새롭게 추가하고자 하는 기능, 부모모델 
`super(Blog,self).save(*args, **kwargs)` 
: 실제 `save`메서드를 가져와서 `다시 어떠한 기능`을 실행한 후에 저장해라 `.save(*args,**kwagrs)`

* Bulk operations 

: 몇만건엑셀파일을 넣을경우 사용

# Model inheritance(모델상속)

: 세가지 타입은 데이터베이스 테이블을 어떻게 처리하는가가 기준이다.



## Abstract base classes

: 다른 모델들에게 공통된 정보를 다른모델에 넣을때 유용하다. 상속할 정보가 담겨있는 베이스 클래스의 `메타클래스`에 `abstract = True`를 선언하면 된다. 이 모델은 데이터베이스 테이블을 만드는데 사용되지 않는다.



```
from django.db import models

class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()

    class Meta:
        abstract = True

class Student(CommonInfo):
    home_group = models.CharField(max_length=5)

    def __str__(self):
        return "Homegroup {}\'s student ({},{})".format(
                self.home_group,
                self.name,
                self.age,
        )

class Teacher(CommonInfo):
    classes = models.CharField(max_length=20)

    def __str__(self):
            return 'class {} \'s teacher ({},({})'.format(
                self.classes,
                self.name,
                self.age
            )
            class Meta:
                db_table = "introduction_to_models_abc_teacher"

	
```

: `Student`모델,`Teacher`모델은 `Commoninfo`에서 상속받은 `name`,`age`를 쓸 수 있다.

![](/Users/mac/projects/images/스크린샷 2017-06-07 오후 2.40.39.png)

: **`CommonInfo`테이블은 만들어 지지않고, 상속받은 각 모델에 `name`,`age`속성이 보인다.**



### Meta inheritance

: `abstract` base클래스가 만들어졌을때, 장고는 선언한 베이스 클래스 안에 `Meta inner class`를 만든다.	 자식 클래스에서 메타클래스를 선언하지 않았을 경우, 부모의 메타데이터는 상속되며, 자식클래스에서 선언된 메타클래스가 확장된다.

```
from django.db import models

class CommonInfo(models.Model):
	class Meta:
		abstract = True
		ordering = ['name']
		
class Student(CommonInfo):
	class Meta(CommonInfo.Meta):
		db_table = 'student_info'
```

: 만약 `abstract = False`라고 설정할 경우, `abstract base class`의 `Meta`클래스를 만들지 않기 때문에, `abstract = True`가 중요하다. 

## `be careful with related_name and related_query_name` 



```
from django.db import models

class Manufacturer(models.Model):
    name = models.CharField(max_length=30)

    def __str__(self):
        return self.name

class Car(models.Model):
    name = models.CharField(max_length=40)
    manufacturer = models.ForeignKey(
        'Manufacturer',
        on_delete=models.CASCADE,
        related_name ='cars',
        related_query_name='manufacturer_car',

    )

    def __str__(self):
        return self.name
```

: `related_name`은 역참조할때 쓸 이름 관련매니저의 이름 `car_set`을 `cars`로 바꾼다.

`m.cars` : 역참조

`Manufacturer.objects.filter(car__name__contains='B')`은 원래 역 쿼리셋 검색이면,

: `related_query_name`은 
`Manufacturer.objects.filter(manufacturer_car__name="B")` : `Manufacturer`모델에서 쿼리셋으로 자동차 이름을 찾고싶을경우,  `related_query_name`에 지정해둔 `manufacturer_car__name`으로 찾는다.




* `related_name`과 `related_query_name`을 동적이름으로 구성하는 예

```
from django.db import models

class Base(models.Model):
	m2m = models.ManyToManyField(
		otherModel,
		related_name = "%(app_label)s_%(class)s_related",
		related_query_name= "%(app_label)_%(class)ss",
		
		class Meta:
			abstract = True
			
class ChildA(Base):
	pass
class ChildB(Base):
	pass
```

```
from common.models import Base

class ChildB(Base):
    pass
```

: `related_name = "%(app_label)s_%(class)s_related"` : `(app_label)`은 처음 `django-admin startproject 모듈명` : 에서 모듈명이고,`(class)` : 말그대로 클래스명이다. 즉 쿼리셋에서 `django_modelss_Bases_related` 가 역참조 `base_set`을 대신하는 것이다.

: `"%(app_label)_%(class)ss"`: 역쿼리셋이름은 `django_models_Basess`이다.


## Multi-table inheritance

```
from django.db import models


class CommonInfo2(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()

    class Meta:
        ordering = ('-name',)
        db_table = 'django_models_mti_commoninfo2'


class Student2(CommonInfo2):
    home_group = models.CharField(max_length=5)

    def __str__(self):
        return "Homegroup {}\'s student ({},{})".format(
            self.home_group,
            self.name,
            self.age,
        )

    class Meta:
        db_table ='django_models_mti_student2'


class Teacher2(CommonInfo2):
    classes = models.CharField(max_length=20)

    def __str__(self):
        return 'class {} \'s teacher ({},({})'.format(
            self.classes,
            self.name,
            self.age
        )

    class Meta:
        db_table = 'django_models_mti_teacher2'

```


![](/Users/mac/projects/images/스크린샷 2017-06-07 오후 3.41.49.png)

![](/Users/mac/Desktop/스크린샷 2017-06-07 오후 3.45.24.png)


![](/Users/mac/projects/images/스크린샷 2017-06-07 오후 3.45.30.png)

![](/Users/mac/projects/images/스크린샷 2017-06-07 오후 3.45.37.png)

: 각각의 모델의 `Meta class`에서 `db_table` = `mti`로 설정한 테이블이 따로 생성되고, 각각의  `mti`로 생성된 테이블과 `CommonInfo2`테이블과  1대1 관계로 맺어있으며, `teacher2테이블`은 `classes`만을 가지고 있고, `student2테이블`은 `home_group`을 가지고 있다.

: `mti`테이블안에 `ptr`이라는 1대1 관계의 필드가 생성되나 데이터베이스에서는 보이지 않는다. `abstract model`과의 차이는 **추상모델의 경우는 `Commoninfo`테이블이 만들어지지 않고, `Commoninfo`의 속성들은 상속되어 각각의 상속받은 자식테이블에 들어가기에 `Commoninfo`에서 쿼리셋을 할 수 없으나,** `multi_table`은 `Commoninfo`테이블이 만들어지고 각각의 자식모델과 `ptr_id`로 1대1관계로 맺어지기에 `Commoninfo`에서 쿼리셋을 실행시킬수 있다.



```
from django.db import models

class Place(models.Model):
	name = models.CharField(max_length=50)
	address = models.CharField(max_length=80)
	
class Restaurant(Place):
	servers_hot_dogs = models.BooleanField(default=False)
	servers_pizza = models.BooleanField(default=False)
```
`p = Place.object.get(id=12)`
`p.restaurant`이 가능한이유는

```
place_ptr = models.OneToOneField(
	place,on_delete=models.CASCADE,parent_link=True,)
```

:  `Restaurant`에서 `place_ptr`이라는 `Place`와 1대1 관계인 테이블을 자동적으로 만들기 때문에 가능하다.
	

## Meta and multi-table inheritance

: 멀티테이블 상황에서, 부모모델의 메타클래스에 선언된 속성들은 이미 부모모델에 적용했다. 그래서 자식모델들은 부모의 메타클래스에 접근할 수 없지만, 자식모델이 `ordering`속성이나 `get_latest_by`속성을 구체화하지 않는다면,부모의`meta class`속성을 물려받는 경우가 있다. 

: 부모의 `meta`클래스를 물려받기 원하지 않는경우,

```
class ChildModel(ParentModel):
	class Meta:
		# 부모의 ordering effect없애기
		ordering = []
```


## inheritance and reverse relations(상속과 역참조)

: 멀티테이블 상속의 경우, 역참조명(`related_name`)을 따로 지정해줘야 한다.

`class Supplier(Place):
	customers = models.ManyToManyField(Place)`
	
: 지정해주지 않을경우 `Suppliers.customers(인스턴스호출)`와`Supplier.place_ptr`중 역참조2개기 때문이다. 이 두개중에 정의를 해줘야한다.
	
: `models.ManyToField(Place, related_name ='provider`) : customers필드에 `related_name`를 새로 설정한다.

# Proxy models

: 멀티테이블 상속을 사용할경우, 새로운 데이터베이스가 각각의 모델의 서브클래스마다 생긴다. 서브클래스는 각각의 데이터를 저장할 장소가 필요하다. 하지만, **오직 모델의 메서드만 변하고 싶을 경우는**, `proxy model inheritance`를 사용하면 된다.  `Proxy models`은 `normal models`와 같이 선언된다. 테이블을 만들지 않고, **메서드만 추가해서 사용** class Meta에 `proxy=True`만 설정

```
from django.db import models

class Person(models.Model):
	first_name = models.CharField(max_length=30)
	last_name = models.CharField(max_length=30)
	
class MyPerson(person):
	class Meta:
		proxy = True
		
	def do_something(self):
		pass
```

: `proxy=True`라고 설정할 경우, `MyPerson`클래스는 부모클래스인 `Person`클래스의 같은 데이터베이스에서 작동한다.  자식클래스에서 `proxy=true`로 설정

```
p = Person.objects.create(first_name="foobar")`
`MyPerson.objects.get(first_name="foobar")
```

: 같은 `Person`데이터베이스를 쓰기에, `MyPerson`에서 접근해도 똑같은 결과가 나온다. 단지 `proxy=True`를 통해 메서드를 추가하거나, 속성을 추가하거나 바꿀때 사용.  

```
class OrderedPerson(Person):
	class Meta:
		ordering = ["last_name"]
		proxy = True
```

: `ordering`이라는 옵션만 추가


: 다른 기준으로 순서화 하고 싶을경우도 `proxy` 를 사용한다.

: 추상클래스는 여러번 상속 받을 수 있다. 

: `proxy model`은 `one non-abstract model class`로부터 상속받아야 한다. 즉 하나의 클래스에서만 사용할 수 있다.

## Proxy model managers

: 부모클래스의 `managers`를 상속받는다. `proxy`모델의 매니저를 선언하고 싶다면, `objects`가 `Manager`를 의미하므로,

```
from django.db import models

class Newmanager(models.Manager):
	pass
	
class MyPerson(Person):
	objects = Newmanager()
	
	class Meta:
		proxy = True
```

: `NewManager`이라는 모델을 만들고, `MyPerson`이라는 모델에서 `objects= NewManager()`이라고 선언 `Meta class`에 `proxy=True`라고 선언. `Manager`만 커스텀하고 싶을뿐이므로 `proxy`모델을 사용한다.


* 새로운 매니저 abstract class 만들기 : 기존의 `Manager`을 대체하지 않고, `NewManager`을 추가하고 싶다면,


```

class ExtraManagers(models.Model):
	secondary = NewManger()
	
	class Meta:
		abstract = True
		
class MyPerson(Person, ExtraMangers):
	class Meta:
		proxy = True
```

: `abstract=True`를 선언한 모델을 상속받는다면, 따로 테이블이 생기지 않고, 상속받은 자식모델에게 속성값만 들어간다. `MyPerson`클래스에 `Person`과 `ExtraManagers` 두가지를 상속받는다.

## `proxy 상속`과 `unmanaged models`의 차이

(1) `Meta.managed=False`는 장고의 컨트롤하에 있지 않은 데이터베이스 컨트롤러나 테이블을 모델링할때 유용하다. 즉 어떤모델의 `objects`가 없다는 의미이다. 

(2) `Meta.proxy=True`는 모델에서 파이썬의 행위만 바꾸고 싶고, 기존의 같은 필드를 유지하고 싶을때 사용한다. 데이터가 저장될때,   오리지널 모델의 저장 구조를 정확히 카피한다.

## Multiple inheritance

: 다양한 부모모델에서 여러개를 상속받을경우, 상속순서중에서 첫번째 상속받은 부모클래스의 `Meta` 클래스를 상속받는다. 

* `mix-in` : 중복된 필드속성들을 새로운 클래스를 만들어서 넣어두고, 속성을 모아둔 클래스를 상속받는다. 단 속성을 모아둔 클래스의 메타클래스에는 `abstract = True`를 설정해야한다. `mixin`을 사용하는 이유는 멀티테이블을 사용하는것이 아닌 `abstract=True`로 사용하여, 테이블을 새로 생성하는것이 아닌 단순히 다른 필드에 속성을 추가하고 싶은것이다.

`models/mixin.py`

```

from django.db import models
from util.models.mixins import TimeStampedMixin


class Post(TimeStampedMixin):
    author = models.ForeignKey('User')
    title = models.CharField(max_length=50)
    content = models.TextField()


class Comment(TimeStampedMixin):
    post = models.ForeignKey(Post)
    author = models.ForeignKey('User')
    content = models.TextField()


class User(TimeStampedMixin):
    name = models.CharField(max_length=50)
```
`util/models/mixins.py`

```
from django.db import models


class TimeStampedMixin(models.Model):
    created_date = models.DateTimeField(auto_now_add=True)
    modified_date = models.DateTimeField(auto_now=True)

    class Meta:
        abstract = True
```

: util/models안에 `__init__.py`파일을 추가하여 패키지화 해야한다. 


: 중복되는 필드의 속성이나, 메서드를 추가하고, `Meta class`에서 `abstract=True`를 선언한다.


```
class Article(models.Model):
	article_id = models.AutoField(primary_key=True)

class Book(models.Model):
	book_id = models.AutoField(primary_key=True)
	
class BookReview(Book,Article):
	pass
```

: 동일한 `primary key` 를 가지는 다양한 모델들을 상속받는 것은 에러를 일으킨다. 그러므로, 다양한 상속을 받기 위해서는 상속할 부모모델의 `AutoField`와 `primary_key`를 이용하여 `primary_key`를 다시 설정해줘야 한다. 

 `article_id = models.AutoField(primary_key=True)`,
 
 `book_id= models.AutoField(primary_key=True)`




# Organizing models in package

: `manage.py startapp` 명령은 어플리케이션 구조를 만들고, `models.py`파일을 포함시킨다. 많은 `모델`을 가지고 싶다면, 각각의 파일을 조직화하는것은 매우 유용하다. `models package`를 만드는것은 `models.py`를 제거하고, `myapp/models/` 디렉토리에 `__init__.py`를 포함시키고 저장한다. 무조건 `__init__.py` 파일을 `import`해야 한다.

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 12.56.17.png)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 12.57.16.png)

: `__init__.py`에다 자기가 쓸 클래스명을 `import`




