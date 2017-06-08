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
	
	

	
	