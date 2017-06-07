# Model field reference

: `field options`와 `field types`를 모두 포함하는 `Field`의 `API`를 설명한다.

: `django.db.models.fields`에 이러한 모델들은 정의되어 있으며, `django.db.moelds`에서 `import` 할 수 있다. 

# Field options

* null(Field.null)

: `null=True`,장고는 텅빈값을 데이터베이스에 `NULL`로써 저장한다. `no data:NULL`이거나 `텅빈문자열`을 의미한다. 예외로, `CharField`에  `unique=True`나 `blank=True`을 동시에 가진다면, 이러한 상황에서 	`null=True`는  텅빈값으로 다양한 객체를 저장할때, `unique constratint violations`(독특한 제한 위반?)을 피하게 된다.??

: 문자열기반이건 아니건 폼에서 텅빈값을 원한다면 꼭`blank=True`을 추가해야한다. `null`은 단지 **데이터베이스에 저장할때 영항**을 미치기 때문이다.

: `BooleanField`에서 `null`값을 넣고 싶다면  `NullBolleanField`를 대신에 사용해라.

: `BooleanField`는 `true/false` 필드이며, 이 필드의 폼 widget은 `checkboxinput`으로 변한다. 기본값은 `None`이다.

* blank

: `blank=True`일때 텅빈값을 허용한다. 

* choices

: choice필드를 사용하기 위해서 두개의 항목으로 이루어진 튜플을 사용해야 한다. 폼 widget은 `selectbox`로 주어진다. `model class`에 `choices`를 정의하는것이 최고다. 

```
from django.db import models

class Student(models.Model):
	FRESHMAN = 'FR'
    SOPHOMORE = 'SO'
    JUNIOR = 'JR'
    SENIOR = 'SR'
    YEAR_IN_SCHOOL_CHOICES = (
    	(FRESTHMAN, 'freshman'),
    	(SOPHOMORE, 'sophomore'),
    	(JUNIOR, 'junior'),
    	(SENIOR, 'senior'),
    	)
    	
    year_in_school = models.CharField(
    max_length=2
    choices = YEAR_IN_SCHOOL_CHOICES,
    default=FRESHMAN,
    )
    
    def is_upperclass(self):
    	return self.year_in_school in (self.JUNIOR,self.SENIOR)
```

: 일반적으로, 튜플의 첫번째 요소는 모델의 실제값이고, 두번째 요소는 인간이 읽을수 있는 이름이다.

: 비록 너가 모델밖에 choice list를 정의했을지라도, `Student.SOPHOMORE`은 `Student`모델이 `import`된다면 어디든 사용 가능하다.

```

MEDIA_CHOICES - (
('Audio',(
		('vinyl','Vinyl')
		('cd','CD'),
	)
),
('Video', (
		('vhs','VHS Tape')
		('dvd','DVD'),
	)
),
('unknown',Unknown'),

)
```

: 각각의 튜플의 첫번째 요소는 그룹에 적용될 이름이다. 두번째 요소는 순환할수 있는 2개의 튜플인다. 2개의 튜플 각각 첫번째 요소는 값(소문자), 두번째 요소는 인간이 읽을수 있는 이름(앞에 대문자),이다. `Grouped options`는 `ungrouped options`와 결합될수 있다. 각각의 모델은 `choices` set을 가지고 있ㅇ며, 장고는 인간이 읽을수 있는 이름은 데이터베이스에는 저장되지 않으며, `get_함수명_display()`로 볼수 있다. 함수명은 `choices = 선택값이 적용된 튜플의 인스턴스`가 설정된 함수명이다. `get_year_in_school_display()`로 각 튜플의 두번째요소(인간이 읽을수있는 이름)을 불러 낼수 있다.

: `choices`는 순환할수 있는 객체이고, 꼭 튜플이나 리스트가 필요하지는 않다. `blnak=True`라면, `selectbox`에서 "------"으로 표현되기에, 이행위를 재정의하기 위해서는 `choices`의 튜플에 (None, 'your string for display')라고 더해줘야 한다.

* Field.db_column

: `column`의 데이터베이스 이름이 주어지지 않는다면 필드명을 사용하거나, 파이썬의 속성이름과 같이 허용되지 않는 문자여도 장고에서 `column`이나`table`이름을 알아서 인용해준다.

* Field.db_index

: db_index = True, 데이터베이스 index가 필드에 생성될것이다.??

* Field.db_tablespace

: `database tablespace`라는 이름이 필드의 `index`에서 사용되고, 이필드가 `indexed`라면, `DEFAULT_INDEX_TABLESPACE` setting이 기본값이다. 만약 백엔드에서 `indexes`를 위한 `tablespaces`를 지원하지 않는다면, `db_tablespace`는 무시된다.

* default

: 필드의 default 값을 의미하며, 값이나 객체이다. `defalut`는 가변성의 객체가 될수 없다.(model instance,list,set)

```
def contact_default():
	return {"email" : "oilvers@gmial.com'}
	
contact_info = JSONfield("ContactInfo",default=contact_default)
```

: lambda는 `default`에서는 사용될수 없다.

: `ForeignKey`로 모델인스턴스와 연결하는 필드의 경우, `default`는 모델인 인스턴스 대신에 **그들이 참조한 필드의 값이 되어야 한다.**

: `default` 값은 새로운 인스턴스가 만들어질 때나 필드에서 값을 제공하지 않을때, 필드가 `primary key`라면, `default`는 필드가 `None`으로 될때 사용된다??


* Field.editable

: `editable=False`라면, 필드는 `admin`이나 다른 `ModelForm`을 보여주지 않는다. `model validation`하는동안 스킵한다.

* error_message

: `error_message` 인자는 기본메세지를 재정의 하도록 해준다. 딕셔너리에서, 에러메세지 `key`는 `null`,`blank`,`invalid`,`invalid_choice`,`unique`를 포함하며, 추가적인 에러메세지 `key`는 각각의 필드타입에 의해 구체화된다.

```
from django.forms import ModelForm
from django.core.exceptions import NON_FIELD_ERRORS

class ArticleForm(ModelForm):
	class Meta:
		error_message = {
			NON_FIELD_ERRORS : {
				'unique_together' : "%(model_name)s's % (field_label)s are not unique.",}
				}
```

: 에러메세지는 `Meta`폼에서 정의된다. `error_messages`는 인스턴스이며, 딕셔너리의 키값은 `NON_FIELD_ERRORS`이며 값은 전하고자 하는 에러메세지이다.

* Field.help_text

: `help` text는 폼의 widget에서 보인다. 폼에서 필드가 사용되지 않았을 경우에 유용하다. 

`help_text = "다음양식을 사용하세요 : <em>YYYY-MM--DD</em>` : `django.utils.html.escape()`를 사용하여 `HTML`의 특별한 문자를 넣을 수 있다.

## primary_key

: `primary_key = True`는 `primary_key`의 행위를 재정의 해줄때 필요하다. `AutoField`에서 자동적으로 `primary_key`를 더한다.  `primary_key=True`는 `null=False`,`unique=True`를 의미하고 있다. 오직 하나의 `primary_key`는 하나의 객체만을 허용한다. `read_only`이며, 만약 `primary_key`의 값을 바꾸고 싶다면, 기존의 값을 저장하고, 새롭게 만든다. `(새로운값,기존값)`이렇게 보인다.

* Field.unique

: `unique=True`면 테이블전체가 `unique` 하게 된다. `ManyToManyField`와 `OneToOneField`를 제외하고는 모두 가능하다. `unique=True`일때, `db_index`는 구체화할 필요가 없다. 

* unique_for_date/unique_for_year/unique_for_month

: `DateField`나 `DateTiemField`에서 옵션으로 사용할 수 있다. 이 속성은 필드의 독자성(`unique`)이 요구된다. 만약 `title`이라는 필드에 `unique_for_date = "pub_date"`라고 정의할 경우 장고는 같은 `title` 과 `pub_date`를 가지고 있는 같은 기록을 허용하지 않는다. 객체가 저장된 시점이 = `current timezone`에서 시행된다.


: `Model.validate_unique()`에 의해 정의되며, 만약 `unique_for_date`제한이 `ModelForm`에서 정의한 속성(`exclude`,`editable=False`)이 부분이 아닌 필드를 포함한다면, `Model.validate_unique()`는 유효성검사를 스킵하게 할것이다. ??

* verbose_name

: 필드에서 인간이 읽을수 있는 이름, 따로 정의하지 않는다면 필드의 인스턴스 이름으로 필드명을 저장한다. 

* validators

: `validators`은 값을 가질수 있고, `ValidationError`오류를 제기 할 수 있다. 만약 몇가지 기준에 충족되지 않는다면, `Validators`는 유효성로직에서 유용하다.

```

from django.cor.exceptions import ValidationError

from django.utils.translation import ugettext-lazy as _

def validate_even(value):
	if value % 2 !=0
		raise ValidationError(
		_('%(value)s is not an even number'),params={'value':value},)
		
from django.db import models

class MyModel(models.Model):
	even_field = models.IntegerField(validatiors=[valitate_even])
	
```

: vaildator가 실행되기 전에 파이썬으로 값이 변환될 필요가 있기 때문이다. 


# Field types

* AutoField

`id = models.AutoField(primary_key=True)` : 자동적으로 `primary key`가 증가된다. `primary key`를 커스텀하고 싶으면, `primary_key=True`를 선언할때, `AutoFIeld(primary_key=True)`를 사용하면 된다.

* BigAutoField

: 1 to 9223372036854775807의 숫자를 `primary_key`에 부여한다.

* BigIntegerField

: 9223372036854775808 to 9223372036854775807의 숫자를 `primary_key`에 부여한다. 기본 폼 widget은 `TextInput`이다. `<input type ="text"~>`

* BinaryField

: 2진수의 데이터를 저장하는 필드, 오직 `bytes`만 할당한다. `BinaryField`값을 쿼리셋으로 필터링이 불가능 하다. 

* BooleanField

: true/false 필드이며, 기본 form widget는 `CheckboxInput`이다. `null`값을 받아드릴 필요가 있다면, `NullBooleanField`가 필요하다.

* CharField

`class CharField(max_length=None, **options)`

: 작고큰 문자열 필드, 많은 텍스트양일 경우 `TextField`를 사용한다. 기본 form widget은 `TextInput`이다.

* commaSeparatedIntergerField

`class CommaSeparatedIntegerField(max_length=None, **options`

: 콤마로 분리된 정수형 필드가 있을경우, `CharField`와 `max_length`인자가 요구된다. 

* DateField

`class DateField(auto_now=False, auto_now_add=False, **options)`

: `datetime.date`인스턴스에 의해 파이썬으로 표현할수 있는 data이며 몇가지 인자가 필요하다

* `DateField.auto_now`

: 객체가 저장될때마다의 시간을 자동적으로 정의해준다. 현재 date는 항상 사용된다. 재정의할수 있는 기본값이 아니다.

: `Model.save()`로 자동저그로 저장된다. `QuerySet.update()`와 같은 다른방식으로 필드가 업데이트 되지 않는다. 업데이트할 필드의 `custom value`를 구체화 해야한다.??

* `DateField.auto_now_add`

: 객체가 처음 생성될때, 자동적으로 지금으로 필드가 정의된다. 객체가 생성될때 이필드의 값을 설정할 경우, 무시된다. 이 필드를 수정한다면, `auto_now_add=True`대신에

` DateField:default=date.today from datetime.date.today()`

` DateTimeField:default=timezone.now from django.utils.timezone.now()`

으로 설정해야 한다. 기본 필드의 기본 form widget은 `TextInput`이다. `auto_now_add`,`auto_now`,`default`는 서로 양립할 수 없다.

: `auto_now`,`auto_now_add=True`를 쓸때는 옵션에 `editable=False`와 `blank=True`를 써야한다.

* DatetimeField

`class DateTimeField(auto_now=False, auto_now_add=False, **options)`

: `admin`은 두개의 분리된 `TextInput`위젯을 사용한다. 

: `auto_now`,`auto_now_add=True`옵션은 항상 `default timezone`을 사용하기에 다른시간을 지정하고 싶다면, `DateField`대신에 `DateTimeField`을 사용하고, 지정하고자 하는 `default`값을 불러오고 `save()`을 사용하여 덮어쓴다. 

* DecimalField(실수표현)

`class DecimalField(max_digits=None, decimal_places=None, **options)`

: 2개의 인자를 가질수 있다.

`class Decimal(value="0", context=None)` : 값에 `Decimal`객체를 만든다. 값은 정수,문자열,튜플,`float`객체를 가질수 있다.

`DecimalField.max_digits`

: `digits`의 최대 숫자를 허용한다. `digits`는 자릿수,`decimal_places`와 같거나 큰 숫자를 정의해야 한다.??

`DecimalField.decimal_places`

: `decimal_place`에 저장하는 숫자

예를들어, 2 decimal places에 999까지의 숫자를 저장하고 싶다면,

`models.DecimalField(...,max_digits=5,decimal_places=2)`

10 decimal places에 1조의 숫자를 저장하고 싶다면,

`models.DecimalField(...,max_digits=19,decimal_places=10)`

: 기본 form widget은 `NumberInput`이다. `<input type = number>`

* FloatField

`class FloatField(**options)`

: 기본 form widget은 `NumberInput`이다.

* DurationField

`class DurationField(**options)`

: 시간의 기간을 저장하는 필드로, `timedelta`모듈이다. 

* EmailField

`class EmailField(max_length=254, **options`

* ImageField

`class ImageField(upload_to=None, height_field=None, width_field=None, max_length=100, **options)`

: `ImageField`는 또한 `height`와 `width`속성을 가진다.

: `ImageField`인스턴스는 `varchar`컬럼으로 100자의 문자를 가지고 데이터베이스에서 생서된다. `max_lenght`인자를 사용하여 최대 길이를 조정 할 수 있다.

: form widget은 `ClearableFileInput`이다. <input type = "file"...>

`IntegerField`

`class IntegerField(**options)`

: 정수형값을 저장하는 필드 , form widget은  `NumberInput`이다.

* GenericIPAddressField

`class GenericIPAddressField(protocol=’both’, unpack_ipv4=False, **options)`

: `IPv4`, `IPv6` 줏조에서, 문자열 포맷안에서, form widget은 `TextInput`이다.

: `IPv6` 주소는 ??

`GenericeIPAddressField.protocol`

: 받아드리는 값은 `both`, `'IPv4' or 'IPv6'`

`GenericIPAddressField.unpack_ipv4`

: 이 옵션은 `protocol = both`일때만 사용 가능하다.

* `NullBooleanField`

`class NullBooleanField(**options)`

: `null=True`와 `BooleanField`대신에 사용한다. 기본 form widget은 `NullBooleanSelect`이다.

* PositiveIntegerField

: 0 to 2147483647 범위의 정수값을 받아드린다.

* PositiveSmallIntegerField

: 0 to 32767범위의 정수값을 받아드린다.

* SlugField

`class SlugField(max_length=50, **options)`

: `Slug`는 뉴스페이퍼 용어이다.  짧은 문자,숫자,하이픈등 url에 사용하는 것을 의미한다.

: `max_length`을 설정하지 않으면 장고는 기본값으로 `length of 50`으로 설정한다.

: `Field.db_index = True`을 내포한다.

: `SlugField.allow_unicode=True`를 쓴다면, 유티코드문자를 넣을 수 있다.

* SmallIntegerField

`class SmallIntegerField(**options)`

: Values from -32768 to 32767

* TextField

: 큰 텍스트 필드일 경우, 기본 form widget은 `Textarea`이다. 

* TimeField

`class TimeField(auto_now=False, auto_now_add=False, **options)`

: `datetime.time`에서 표현되는 시간, 기본 widget은 `TextInput`이다. 

* URLField

`class URLField(max_length=200, **options)`

: `URL`의 `CharField`이다. 기본 form widget은 `TextInput`이다. `URLField`는 `max_length`인자를 선택적으로 가져야한다. 정의하지 않는다면 기본값은 200이다.

* UUIDField

: 보편적인 `identifiers`를 필드에 저장할때 사용한다. `AutoField`와 `primary_key`가 필요하며, 데이터베이스는 자동적으로 `UUID`를 생성해주지 않는다.

```
import uuid
from django.db import models

class MyUUIDModel(models.Model):
	id = models.UUIDField(primary_key=True,default=uuid.uuid4, editable=False)
```

# Relationship fields

## Foreignkey

`class ForeignKey(othermodel, on_delete, **options)`

: `manytoone` 관계에서 첫번째 인자로, 부모모델이 필요하다. 자기자신과의 관계를 설정할경우 `ForeignKey('self',on_delete=models.CASCADE)`를 써야 한다.

```
from django.db import models

class Car(models.Model):
	manufacuter = models.ForeignKey('Manufacturer',
	on_delete = models.CASCADE,)
```

`abstract models`: 수많은 다른 모델에 공통된 정보를 넣고 싶을때 유용하며, `abstract=True`를 사용 한 모델을 통해 공유한다.

`products/models.py`

```
from django.db import models

class AbstractCar(models.Model):
	manufacturer = models.ForeignKey('Manufacturer',on_delete=models.CASCADE)
	
	class Meta:
		abstract = True
```

`production/models.py`

```
from django.db import models
from products.models import AbstractCar

class Manufacturer(models.Model):
	pass
clasS Car(AbstractCar):
	pass
	
```

만약 `Manufacturer`모델이 다른 aplication에서 정의도니다면 명칭을 바꿔서 정의해야 한다.

```
class Car(models.Model):
	manufacturer = models.ForeignKey('production.Maunufacturer',on_delete=models.CASCADE,)
```

: 데이터베이스 `index`는 자동적으로 `ForeginKey`에 의해 만들어 지는데 `db_index=False`를 하면  `index`가 만들어지지 않는다.??

`Database Representation`

: 필드의 이름 +`"_id"`은 데이터베이스 컬럼의 이름을 만든다. 위의 예에서, `Car`모델은 `manufacturer_id`컬럼을 가질것이다. 

* Arguments

: `ForeignKey`는 다른 인자를 받아드린다.

(1) `ForeignKey.on_delete`

: 외래자에 참조된 객체가 삭제된다면(부모모델),객체의 값이 `null`값을 만들어준다. 아래의 예와 같다.

```
user = models.ForeignKey(
	User,
	models.SET_NULL,
	blank=True,
	null=True,
	)
```

`on_delete = ?`

* CASCADE : ??

* PROTECT : `ProtectedError`가 발생했을때, 참조한 객체의 삭제를 막는다. `django.db.IntegrityError`

* SET_NULL : null값으로 외래자를 설정하고 싶을떄,  `null=True`

* SET_DEFAULT : `ForeignKey`의 값을 셋팅한다.

* SET() 

```
from django.conf import settings
from django.contrib.auth import get_user_model
from django.db import models

def get_sentinel_user():
	return
get_user_model().objects.get_or_create(username='deleted')[0]

class MyModel(models.Model):
	user = models.ForeignKey(
		settings.AUTH_USER_MODEL,
		on_delte=models.SET(get_sentinel_user),
		)
		
```

: 쿼리셋으로 불러오고 싶은 명령어를 `get-sentinel_user`에 저장해 놓고, `on_delete=models.SET(모델명)`으로 쓴다.

* DO_NOTHING : ??

(2) `ForeignKey.limit_choices_to`

: 부모모델중 선택사항인것을 참조하겠다.

```
staff_member = models.ForeignKey(User,
	on_delete = models.CASCADE,
	limit_choices_to ={'is_staff' :True},)
```

: 위의 예에서 `Users`라는 모델에서 `is_staff=True`를 가지는 일치하는 필드만 반환하겠다.

```
def limit_pub_date_choices():
	return {'pub_date__lte' : datetime.date.utcnow()}
	
limit_choices_to = limit_pub_date_choices

```

: 모델을 따로 만들어서, `limit_choices_to`를 인스턴스로 하여 함수명을 넣을수 있다.

(3) `ForeignKey.related_name`

: `related_name`을 설정하는 이유는 중간자모델에서 똑같은 소스모델을 많은 외래자로 참조함고, `through_fields =('field1','field2)`에서 설정한 필드를 를 제외한 속성을 역참조하고 싶으면, `related_name`을 재설정해줘야 한다.

```

user = models.ForeignKey(
	User,
	on_delete=models.CASCADE,
	related_name = '+',
	)
```
	
: `related_name='+'`을 쓰면, 부모모델인 `User` 모델은 외래자를 설정한 모델과 `backend relation`(역참조)를 할필요가 없을경우

(4) `ForeignKey.related_query_name`

: 쿼리셋 이름을 `tag`로 하여 찾을수 있다.

```
related_query_name으로 외래자를 선언하기

class Tag(models.Model):
	article = models.ForeignKey(
		Article,
		on_delete=models.CASCADE,
		related_name = "tags",
		related_query_name ="tag",
	)
	name = models.CharField(max_length=255)
	
Article.objects.filter(tag__name="important")
```

: `related_query_name="tag"`는 `tag`문자열로 하여 필터링하겠다는 의미

(5) `ForeignKey.to_field` 

: 관련된객체(부모모델)의 `primary_key`를 사용하는데, 다른필드를 참조할경우 그필드는 `unique=True`값을 가지게 된다.

(6) `Foreignkey.db_constraint`

: `db_constraint=True`가 기본값, 외래자의 데이터베이스에서 만들어진 제한들을 컨트롤 할때, ??

(7) `ForeginKey.swappable`

: 외래자가 `swappable model`를 참조하고 있다면, `swappable=True`일때 외래자는 `settings.AUTH_USER_MODEL`의 현재의 값을 매치하여 가르킨다. 이러한 관계는 `migration`에 저장된다.

: `swappable=False`라면, swappable 모델을 참조할수 있다.??

# ManyToManyField

`class ManyToManyField(othermodel, **options)`

* Arguments

(1) `related_name`

(2) `related_query_name`

(3) `limit_choices_to`

(4) `symmetrical`

```
from django.db import models

class Person(models.Model):
	friends = models.ManyToManyField("self")
```

: 자기자신을 `ManyToManyField`로 가진다. `Person`클래스에 `person_set`이라는 속성을 더하지 않는다. 만약 다대다 관게에서 `symmentry`(대칭)을 원하지 않는다면 `symmetrical = False`로 지정하면 된다.

(5) `through`

: 사용하길 원하는 중간모델을 나타내는 장고모델을 구체화 할 수 있다. 중간자 모델을 따로 설정할때 중간자 모델에 해당하는 모델명을 `through=중간자모델명`으로 설정할 수 있다. ??

```
from django.db import models

class Person(models.Model):
	name = models.CharField(max_length=50)

class Group(models.Model):
	name = models.CharField(max_length=128)
	members = models.ManyToManyField(
	Person,
	through='Membership',
	through_fields=('group','person')
	)
class Membership(models.Model):
	group = models.ForeginKey(Group,on_delete=models.CASCADE)
	person = models.ForeignKey(Person,on_delete=models.CASCADE)
	inviter = models.ForeignKey(
		Person,
		on_delete=models.CASCADE,
		related_name="membership_invites",)
		invite_reason = models.CharField(max_length=64)
```

: `Membership`은 두개의 외래자 를 가지며 `Person`,`Group`이며(`person`,`inviter`)은 `Person`모델을 참조하고 있다. 이렇게 같은 모델을 참조하는 외래자의 경우가 2개일경우 `through_fields`를 사용해서 구체화 해줘야한다.

: `through_fields`는 튜플('field1','field2')를  받아드린다.

: 중간자 모델을 사용한 자기자신의 관계를 설정할경우, `symmetrical=False`이며, `field1`은 소스모델(부모모델), `field2`는 타겟모델(자식모델)로 변한다.

(6) `db_table`

(7) `db_constraint` : `True`가 기본값, 컨트롤러에 관한것 ??

(8) `swappable` : `ManyToManyField`가 `swappable model`를 참조한다면 `True`라면, `settings.AUTH_USER_MODEL`의 현재의 값을 매치하여 참조한다.

# OneToOneField

`class OneToOneField(othermodel, on_delete, parent_link=False, **options)`

: `unique=True`이 포함된`ForeignKey`는 `OneToOneField`와 같다. 또다른 모델로 확장하는것이 매우 유용한 방식이다. `Multi-table-상속`도 부모모델과 자식모델의 내포된 1대1 관계를 가지고 있다. 1대1관계에서, `relate_name`을 설정해주지 않을경우, 장고는 `현재의 모델의 소문자를 역참조` 명으로 사용한다.

```
from django.db import settings
from django.db import models

class MySpecialUser(models.Model):
	user = models.OneToOneField(
		settings.AUTH_USER_MODEL,
		on_delete = models.CASCADE,
		)
		
	supervisor = models.OneToOneField(
	settings.AUTH_USER_MODEL,
	on_delete=models.CASCADE,
	related_name='supervisor_of',
	
)

```

```
>>> user.supervisor_of
Traceback (most recent call last):
    ...
DoesNotExist: User matching query does not exist.
```

: user.역참조명을 실행했는데도 쿼리셋이 존재하지 않는다고 하는 이유는 `user`인스턴스는 단순히 `User`와 일대일 관계일뿐,`supervisor`과의 관계는 설정되있지 않기 때문이다. `parent_link=True`를 쓴다면 ??

# Field API reference

`class Field`

: 필드는 추상적인 클래스고 데이터베이스 테이블의 컬럼을 나타낸다. 필드는 모델(클래스)에서 클래스 속성(인스턴스)으로 만들어지고, 특별한 테이블의 컬럼을 나타낸다. `meta class`에서 쓰이는 필드 옵션(`null`,`unique`)나 메서드를 가지고 있다.

: 필드는 `QuerySet`에서 `Transform`이나 `Lookup`이 등록될수 있다.??

* description

: 필드의 `verbose description`이며,
`description = _("strung (up to %(max_length)s")

* get_internal_type

: 클래스 name을 리턴해준다.

* db_type(connection)

: 어떤필드의 데이터베이스의 컬럼데이터의 타입을 반환한다.

* rel_db_type

: `외래자`와 `1대1관계`가 참조하고 있는 어떤필드의 데이터베이스 컬럼을 반환한다.

* 장고가 데이터베이스 백엔드와 필드를 관계시킬 필요가 있을 세가지 경우

(1) 파이썬 값에서 데이터베이스 백엔드 값으로변환(데이터베이스에서 쿼리셋한다.)

(2) 데이터베이스에서 데이터를 읽을때(load) , 데이터베이스값에서 파이썬 값으로 변환

(3) 데이터베이스에 저장할때, 파이썬 값에서 데이터베이스 백엔드 값으로

-> 데이터베이스에서 쿼리할때, `get_db_prep-``value(),get_prep_value()`가 사용된다. ??

* get_prep_value(value)

: `value`는 모델속성의 현재의 값이다. 쿼리에서 인자로 사용될 준비가 된 포맷의 데이터를 메서드는 반환해야 한다.??

* get_db_prep_value(value,connection,prepared=False)

: `value`를 백엔드 `value`로 바꾼다. 기본적으로 `prepared=True`거나 `get_prep_value()`라면 기본값이 리턴된다.

```
def get_db_prep_value(self, value, connection, prepared=False):
    value = super(BinaryField, self).get_db_prep_value(value, connection, prepared)
    if value is not None:
        return connection.Database.Binary(value)
    return value
    
```

: ??

* from_db_value(value,expression,connection,context)

: 파이선객체에서 데이터베이스에 의해 반환된 값을 변환한다. `get_prep_value()`의 반대작용을 한다. 

* `pre_save()`,`get_db_prep_save()`는 저장할때 사용한다.

* get_db_prep_save(value,connection)

: `get_db_prep_value()`와 같은 의미이나, 필드값이 데이터베이스에 저장되어야만 한다. 기본적으로 `get_db_prep_value()`을 리턴한다.

* pre_save(model_instance,add)

: `get_db_prep_save()`이전의것을 불러서 저장한다. `model_instance`는 필드가 속한 인스턴스이며, `add`는 인스턴스가 처음으로 데이터베이스에 저장될때  더한다.??

: 모델인스턴스로 부터 적절한 속성의 값을 반환해야한다. 그속성의 이름은 `self.attname`이다.

* to_python(value)

: 올바른 파이썬 객체로 값을 바꾼다. `value_to_string()`과 반대의 작용을 한다. 또한 `clean()`을 호출한다. 데이터베이스에 저장하는것외에도 필드는 값을 계속 연속으로 반환하는 방법을 알필요가 있다. ??

* value_to_string(obj)

: `obj`를  `string`으로 바꾼다. `See Converting field data for serialization for usage.을 참조`

: `model forms`을 사용할때, 필드는 어느 폼필드가 표현되는지 알필요가 있다.

`formfield(form_class=None,choices_form_class=None,**kwargs)`

: `ModelForm`의 필드의 `django.forms.Field`를 반환한다. ??

: 기본적으로, `form_class`와 `choices_form_class`은 `None`이다. 만약 `choices`와 `choices_form_class`를 구체화하지 않았다면, `TypedChoiceField`를 사용한다.??

* deconstruct()

: 필드를 다시만들 충분한 정보의 4개의 튜플을 반환한다. 모델의 필드명, 위치인자의 목록, 키워드 인자의 딕셔너리 반환등

# Field attibulte reference

: 모든 `Field`인스턴스는 몇가지의 속성을 포함한다. `Model_meta API`로 함께 이러한 속성을 사용해라.??

## 필드의 속성

* Field.auto_created

: 필드가 자동적으로 만들어지는것, 모델 상속에 의해 사용되는 `onetoonefield`와 같다.??

* Field.concrete

: 어떤것과 관련되어 있는 데이터베이스 컬럼을 가지는 필드를 가르킨다.??

* Field.hidden

: ??

: `Option.get_fields()`는 기본적으로 `hidden fields`를 배제한다. `include_hidden =True`는 결과값에서 `hidden fields`를 리턴한다.

* Field.is_relation

: 하나 혹은 더많은 모델을 참조한 필드를 가르킨다.

* Field.model

: 필드가 정의될때, 모델을 반환한다. 

## Attributes for fields with relations(관계설정에서 사용되는 필드속성)

* Field.many_to_many

: `many_to_many=True`이면 필드는 다대다관계를 가진다. 

* Field.many_to_one

* Field.one_to_many

* Field.one_to_one

* Field.related_model : 필드가 관련되있는 모델을 가르킨다.
















