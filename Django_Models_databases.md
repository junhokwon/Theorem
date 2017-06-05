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

`Article.objects.filter(reporter__in=[1,2]).distinct()` : ??

`Article.objects.filter(reporter__in=[r,r2]).distinct()` : ??

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

`p_supersupreme.toppings.create(name='야채')` : 아예 `Many manage`를 사용하여 야채를추가하고 저장한다. `toppings`는 인스턴스로 `1대다`의 관계에서는 인스턴스로 접근 할수 없다. 

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

: `TradeInfo`가 중간모델이고, 	`Club`과 `Person`의 공통요소로 쓸것은 `date_joined`와 `date_leave`다.


![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 4.38.57.png)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 4.39.19.png)



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

: `intermediate model`은 `Membershop`이고, `source model`은 `Group`이다. 여기서 `members = models.ManyToManyField(Person,through='Membership')`를 사용하여 소스모델인 `Group`과 `Person`클래스를 `ManyToManyField`로 설정해줘야 한다. 설정해주지 않는다면 `Person`클래스 하위요소가 `Membership`이라고 장고에서 착각할수 있기 때문이다.  또한 `ForeiginKey`의 위치인자를 같은 모델명(클래스명)에 주는것도 가능하다. 하지만 두가지관계는 연관이 없다면, `source model`에서 `through`를 사용하여 관계를 구체화해줘야 한다.

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

: `ForeiginKey`로 다 연관시켜서 `Membership`클래스의 속성에 `Person`클래스의 인스턴스 `person`과 `Group`클래스의 인스턴스 `group`을 만들어 놓았기 때문에, `Membership`클래스에서 객체를 `create`함수를 통해서 만들 수 있다. `beatles.members.all()`가 가능한 이유는 `members = models.ManyToManyField(Person,through='Membership')` `Person`클래스클 연결해 놓았기 때문이다.

: `many-to-many fields`에서 `add(), create(), or set() or remove()`는 사용할 수 없다. 왜냐면 `Person`클래스와 `Group`클래스 사이를 연관시키지 않았기 때문이다. 방법은 `intermediate model`즉,  `Membership`클래스에서 인스턴스를 만들어 만드는 방법밖에 없다.

: 삭제하기 위해서는 `clear()`메서드를 사용하면 된다.`beatles.members.clear()
`


```
# Find all the groups with a member whose name starts with 'Paul'
>>> Group.objects.filter(members__name__startswith='Paul')
<QuerySet [<Group: The Beatles>]>
```

: 중간모델에서 인스턴스를 만듬으로써 다대다 관계가 선언된다면, 쿼리셋을 사용할수 있다. 다대다 모델의 속성을 사용한다.

```
Person.objects.filter(
	group__name ='The beatles',
	membership__date__joined__gt=date(1961,1,1))
```

: `Person`인스턴스에서 시간대를 불러올려면 `Person.objects.filter(membership__date_joined__gt=date(year,?,?)` 즉 `membership__`이 중요하다.

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

: `OneToOneField`를 사용하여 관련된 모델의 위치인자가 필요하다.

```
from django.db import models
class Place(models.Model):
	name = models.CharField(max_length=50)
	address = models.CharField(max_length=80)
	
class Restaruant(models.Model):
	place = models.OneToOneField(
		Place,
		on_delete=models.CASCADE,
		primary_key =True,
		)
	serves_hot_dogs = models.BooleanField(default=False)

class Waiter(models.Model):
	restaurant = models.ForeignKey(Restaruant,on_delete=models.CASCADE)
	name = models.CharField(max_length=50)
```

: ` class Restaruant(models.Model):
	place = models.OneToOneField(
		place,
		on_delete=models.CASCADE,
		primary_key =True,
		)
`에서 위치인자로 `Place`, `primary_key`(또다른 객체로 확장될때, 객체의 주요한 키로 유용하다), 상단의 예에서 `Place`클래스를 상속받아 `Restaruant`클래스에서 사용하는것이다.  `Restaurant` model은 `OneToOneField` to `Place`모델을 가진다.


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

: metadata는 `ordering`,`db_table`,`verbose_name`,`verbose_name_plural`와 같은 것으로 필드가 아니다. 모델에 대한 설명을 추가하는것이다. ??

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

```
from django.db import models

class Blog(models.Model):
	name = models.CharField(max_length=100)
	tagline = models.TextField()
	
	def save(self, *args, **kwargs):
		if self.name == 'yoko ono's blog':
		return 
		else:
			super(Blog,self).save(*args, **kwargs)
```

: 슈퍼클래스 메서드를 기억하는것이 중요. `super(Blog,self).save(*args, **kwargs)` 


## Model inheritance

(1) Abstract base classes

: 다른 모델들에게 공통된 정보를 넣을때 유용하다. 상속할 정보가 담겨있는 베이스 클래스의 메타클래스에 `abstract = True`를 선언하면 된다. 

```
from django.db import models

class CommonInfo(models.Model):
	name = models.CharField(max_legnth=100)
	age = models.PositiveIntegerField()
	
	class Meta:
		abstract = True
		
class Student(CommonInfo):
	home_group = models.CharField(max_length=5)
	
```

: `Student`모델은 개개의 필드를 가지고있다. `name`,`age`,`home_group`

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

## `be careful with related_name and related_query_name` ??

* `"%(class)s"` 
* `"%(app_label)s"`

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

: `common.ChildA.m2m`필드는 `common_chlida_related`이 될것이고 쿼리셋의 이름은 `common_childsas`이 된다. `rare.ChildB.m2m`필드는 `rare_childb_related`가 되고, 쿼리셋의 이름은 `rare_chlidbs`가 된다.???

## Multi-table inheritance

```
from django.db import models

class Place(models.Model):
	name = models.CharField(max_length=50)
	address = models.CharField(max_length=80)
	
class Restaurant(Place):
	servers_hot_dogs = models.BooleanField(default=False)
	servers_pizza = models.BooleanField(default=False)
```

```
place_ptr = models.OneToOneField(
	place,on_delete=models.CASCADE,parent_link=True,)
```
	
: `Restaurant`클래스에서 `parent_link=True`을 사용하여 `OneToOneField`를 선언하여 오버라이드 할수 있다. ??

## Meta and multi-table inheritance

: 멀티테이블 상황에서, 부모의 메타클래스를 자식클래스가 상속받는게 이해가 안된다. 모든 메타 옵션들은 이미 적용된 부모클래스가 있다. 그래서 자식모델들은 부모의 메타클래스에 접근할 수 없다. 하지만 자식모델이 `ordering`속성이나 `get_latest_by`속성을 구체화 하지 않는다면, 부모의 메타클래스를 상속 받을수 있다.

`class ChildModel(ParentModel):
	class Meta:
		# 부모의 ordering effect없애기
		ordering = []`

## inheritance and reverse relations

: `OneToField`는 명백히 부모클래스에서 -> 자식클래스의 방향대로 가능하다. 자식클래스 -> 부모클래스 역방향 이라면?

`class Supplier(Place):
	customers = models.ManyToManyField(place)`
	
```
Reverse query name for 'Supplier.customers' clashes with reverse query
name for 'Supplier.place_ptr'.

HINT: Add or change a related_name argument to the definition for
'Supplier.customers' or 'Supplier.place_ptr'.
```

: 이런 오류가 발생한다. 해결방법은 `models.ManyToField(Place, related_name ='provider`) : customers필드에 `related_name`을 붙인다.

# Proxy models

: `mutli-table-inheritance`를 사용할때, 새로운 데이터베이스가 각각의 모델의 서브클래스마다 생긴다. 서브클래스는 각각의 데이터를 저장할 장소가 필요하다. 하지만, 오직 모델의 행위만 변하고 싶을 경우는, `proxy model inheritance`를 사용하면 된다.  `Proxy models`은 `normal models`와 같이 선언된다. 

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

: `MyPerson`클래스는 부모클래스인 `Person`클래스의 같은 데이터베이스에서 작동한다. 

`p = Person.objects.create(first_name="foobar")`
`MyPerson.objects.get(first_name="foobar")`

```
class OrderedPerson(Person):
	class Meta:
		ordering = ["last_name"]
		proxy = True
```
: 다른 기준으로 순서화 하고 싶을경우도 `proxy` 를 사용한다.

: `proxy model`은 `one non-abstract model class`로부터 상속받아야 한다. 즉 하나의 클래스에서만 사용할 수 있다.

## Proxy model managers

: 부모클래스의 `managers`를 상속받는다. `proxy`모델의 매니저를 선언하고 싶다면,

```
from django.db import models

class Newmanager(models.Manager):
	pass
	
class MyPerson(Person):
	objects = Newmanager()
	
	class Meta:
		proxy = True
```

: 새로운 매니저를 포함하는 베이스 클래스를 만들면 된다.

```
새로운 매니저 abstract class 만들기

class ExtraManagers(models.Model):
	secondary = NewManger()
	
	class Meta:
		abstract = True
		
class MyPerson(Person, ExtraMangers):
	class Meta:
		proxy = True
```

(1) `Meta.managed=False`는 장고의 컨트롤하에 있지 않은 데이터베이스 컨트롤러나 테이블을 모델링할때 유용하다.

(2) `Meta.proxy=True`는 모델에서 파이썬의 행위만 바꾸고 싶고, 기존의 같은 필드를 유지하고 싶을때 사용한다. 데이터가 저장될때,   오리지널 모델의 저장 구조를 정확히 카피한다.

## Multiple inheritance

: 다양한 부모모델에서 상속받을경우,

```
class Article(models.Model):
	article_id = models.AutoField(primary_key=True)

class Book(models.Model):
	book_id = models.AutoField(primary_key=True)
	
class BookReview(Book,Article):
	pass
```

: `AutoField`를 사용하여 공통된 부모요소를 나타내야 한다.
: `BookReview`클래스에 `Book,Ariticle`두개를 상속받았다.


# Organizing models in package

: `manage.py startapp` 명령은 어플리케이션 구조를 만들고, `models.py`파일을 포함시킨다. 많은 `모델`을 가지고 싶다면, 각각의 파일을 조직화하는것은 매우 유용하다. `models package`를 만드는것은 `models.py`를 제거하고, `myapp/models/` 디렉토리에 `__init__.py`를 포함시키고 저장한다. 무조건 `__init__.py` 파일을 `import`해야 한다.

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 12.56.17.png)

![](/Users/mac/projects/images/스크린샷 2017-06-05 오후 12.57.16.png)

: `__init__.py`에다 자기가 쓸 클래스명을 `import`




