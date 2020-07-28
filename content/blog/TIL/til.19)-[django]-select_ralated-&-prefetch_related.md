---
title: TIL.19) [django] select_ralated & prefetch_related
date: 2020-07-27 19:07:42
category: Django
thumbnail: './images/django.png'
draft: false
---

![](./images/django.png)

# select\_related & prefetch\_related

- 하나의 Query set을 가져올때 미리 연관된 객체들까지 모두 불러오는 함수
- DB에 접근을 줄여 성능을 향상시킨다.
- 다만, `select_related`와 `prefetch_related`는 차이점이 있다.

## select\_related

- `SQL`의 `JOIN`을 사용하기 때문에 `foreign-key`, `one-to-one`와 같은 Single-valued relationship에서만 사용 가능하다.

```python
from django.db import models

class Job(models.Model):
    name = models.CharField(max_length=10)
    def __str__(self):
        return self.name

class Person(models.Model):
    Job = models.ForeignKey(Job)
    name = models.CharField(max_length=10)
 
    def __str__(self):
        return self.name
        
class Salary(models.Model):
    person = models.ForeignKey(Person)
    salary= models.IntegerField(default = 0)
 
    def __str__(self):
        return self.name
```

여기서 Salary id가 1인 인스턴스의 Person과 Job데이터를 조회하기 위해선 아래와 같은 방법을 사용할 것이다.

```python
# Not use select_related()

salary = Salary.objects.get(id=1)
person = salary.person
job = person.job
```

결과는 정상적이나 DB에 총 세번 접근하여 비효율적으로 DB를 사용하게 된다.

1️⃣ : Salary 모델에서 id가 1번인 데이터를 가져오기 위해

2️⃣ : 1️⃣의 데이터에서 person을 가져오기 위해

3️⃣ : 2️⃣의 데이터에서 job을 가져오기 위해

반면, `select_related`를 사용하면 이런 비효율적인 DB접근을 단 한번에 끝낼수 있다.

```python
# Use select_related()

salary = Salary.objects.select_related('person__job').get(id=1) #1️⃣
person = salary.person
job = person.job
```

위 방법에선 1️⃣에서 이미 관계된 person과 job모두 조인해서 가져왔으며 그 다음 person과 job의 정보를 가져오기 위해 다시 DB에 접근하지 않고 이미 가져온 salary에서 꺼내 쓴다.

## prefetch\_related

- `foreign-key`, `one-to-one`, `many-to-many`, `many-to-one`등 모든 relationship에서 사용 가능하다.

```python
class Topping(models.Model):
	pizza_set = models.ManyToManyField(Pizza)
	name = models.CharField(
		max_length=10,
	)
	
	def __str__(self):
		return self.name
```

여기서 Pizza의 모든 인스턴스와 그 인스턴스들의 topping_set들을 아래처럼 모두 가져와야 한다고 가정해보자

*치즈피자 : 치즈, 토마토 소스*

*하와이안피자 : 치즈, 파인애플, 피망, 토마토 소스*

*포테이토피자 : 치즈, 포테이토, 페페로니, 피망*

먼저 `prefetch_related`를 사용하지 않고 가져온다면 다음과 같을수 있다.

```python
# Not use prefetch_related()

pizzas = Pizza.objects.all()
for pizza in pizzas:
	print(pizza.name+' : ', end='')
	for topping in pizza.topping_set.all():
		print(topping.name+' : ', end='')
	print('')
```

마찬가지로 결과에는 이상이 없지만 `Pizza.objects.all()`안에 있는 pizza마다 `pizza.topping_set.all()`이라는 Query를 날리게된다. 즉 pizza가 100개, 1000개, 1000개 있다면 그 만큼 `pizza.topping_set.all()`Query를 DB에 날리게 되는것이다. 하지만 위 코드를 `prefetch_related`를 사용한다면 효율적으로 개선 할 수 있다.

```python
# Use prefetch_related()

pizzas = Pizza.objects.all().prefetch_related('topping_set') #1️⃣
for pizza in pizzas:
	print(pizza.name+' : ', end='')
	for topping in pizza.topping_set.all():
		print(topping.name+' : ', end='')
	print('')
```

`prefetch_related`를 사용하게 된다면 1️⃣에서 `Pizza.objects.all()`이 실행되는 동시에 `self.topping_set.all()`이라는 별도의 Query가 실행되 모두 `cache`에 저장하게 된다. 그래서 pizza의 수만큼 `pizza.topping_set.all()`이 실행되더라도 DB에 접근하지 않고 cache에 저장된 데이터를 찾아서 쓰게 된다. 결과적으로 2개의 쿼리로 1000개의 pizza를 처리할 수 있게 되었다.
