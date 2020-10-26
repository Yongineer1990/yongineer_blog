---
title: Django ORM - Aggregation
date: 2020-10-27 02:10:53
category: Django
thumbnail: '../TIL/images/django.png'
draft: false
---

![](../TIL/images/django.png)

# 들어가기 전에

먼저 다음과 같이 온라인 서점 모델이 존재한다고 가정하자.

```python
from django.db import models

class Author(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()

class Publisher(models.Model):
    name = models.CharField(max_length=300)

class Book(models.Model):
    name = models.CharField(max_length=300)
    pages = models.IntegerField()
    price = models.DecimalField(max_digits=10, decimal_places=2)
    rating = models.FloatField()
    authors = models.ManyToManyField(Author)
    publisher = models.ForeignKey(Publisher, on_delete=models.CASCADE)
    pubdate = models.DateField()

class Store(models.Model):
    name = models.CharField(max_length=300)
    books = models.ManyToManyField(Book)
```

# Cheat sheet

## 모든 책의 수 - 조건 없는 Count

```python
>>> Book.objects.count()
2452
```

## BaloneyPress가 발행한 책의 수 - 조건 있는 Count

```python
>>> Book.objects.filter(publisher__name='BaloneyPress').count()
73
```

`filter()`메서드로 발행인의 조건을 건 후 `count()`메서드를 사용하여 구한다.

## 모든 도서의 평균 가격 - Average

```python
>>> from django.db.models import Avg
>>> Book.objects.all().aggregate(Avg('price'))
{'price__avg': 34.35}
```

`aggregate()`메서드의 `Avg`함수를 통해 구할 수 있다. 이때 기준이 되는 필드명을 지정해야 한다.

## 모든 도서 중 최고 가격 - Max

```python
>>> from django.db.models import Max
>>> Book.objects.all().aggregate(Max('price'))
{'price__max': Decimal('81.20')}
```

`aggregate()`메서드의 `Max`함수를 통해 구할 수 있다. 이때 기준이 되는 필드명을 지정해야 한다.

## 최고가의 도서와 평균가의 도서의 차이 - 집계함수 연산

```python
>>> from django.db.models import FloatField
>>> Book.objects.aggregate(
...     price_diff=Max('price', output_field=FloatField()) - Avg('price'))
{'price_diff': 46.85}
```

## 각 출판사가 발행한 도서의 수를 'num_books'라는 속성으로 지정

```python
>>> from django.db.models import Count
>>> pubs = Publisher.objects.annotate(num_books=Count('book'))
>>> pubs
<QuerySet [<Publisher: BaloneyPress>, <Publisher: SalamiPress>, ...]>
>>> pubs[0].num_books
73
```

## 각 출판사를 평점 5점 이상, 5점 이하로 나누고 각각의 그룹에서 발행한 책의 수

```python
>> from django.db.models import Q
>>> above_5 = Count('book', filter=Q(book__rating__gt=5))
>>> below_5 = Count('book', filter=Q(book__rating__lte=5))
>>> pubs = Publisher.objects.annotate(below_5=below_5).annotate(above_5=above_5)
>>> pubs[0].above_5
23
>>> pubs[0].below_5
12
```

## 가장 책을 많이 출간한 출판사 TOP 5

```python
>>> pubs = Publisher.objects.annotate(num_books=Count('book')).order_by('-num_books')[:5]
>>> pubs[0].num_books
1323
```

# 각 항목에 대한 집계 생성

`annotate()`메서드를 사용하여 객체 별 요약을 생성 할 수 있다. 이 때 `annotate()`메서드내의 집계함수의 인자로 필드명을 사용해야 한다.

예를들어 해당 도서의 저자 수를 `count()`로 구한다고 가정한다면 다음과 같다.

```python
>>> from django.db.models import Count
>>> q = Book.objects.annotate(Count('authors'))
>>> q[0]
<Book: The Definitive Guide to Django>
>>> q[0].authors__count
2
>>> q[1]
<Book: Practical Django Projects>
>>> q[1].authors__count
1
```

이때 `count()`가 생성하는 필드의 이름은 Django가 자동으로 생성하게 되는데 이 이름은 다음과 같이 재정의가 가능하다.

```python
>>> q = Book.objects.annotate(num_authors=Count('authors'))
>>> q[0].num_authors
2
>>> q[1].num_authors
1
```

한편, `annotate()`는 `aggregate()`와 다르게 터미널절에 해당하지 않는다. 따라서 `annotate()`의 결과는 `QuerySet`이며 따라서 `filter()`, `order_by()`등과 같이 다른 `QuerySet` 대한 추가 호출이 가능하다.

# 여러 집계 결합

여러 집계를 `annotate()`와 결합하게 되면 `Subquery`대신 `Join`이 사용되기 때문에 잘못된 결과가 생성된다.

```python
>>> book = Book.objects.first()
>>> book.authors.count()
2
>>> book.store_set.count()
3
>>> q = Book.objects.annotate(Count('authors'), Count('store'))
>>> q[0].authors__count
6
>>> q[0].store__count
6
```

다만 `Count`집계에는 이를 피할 수 있는 고유한 매개 변수가 존재한다. (`distinct`)

```python
>>> q = Book.objects.annotate(Count('authors', distinct=True), Count('store', distinct=True))
>>> q[0].authors__count
2
>>> q[0].store__count
3
```

# 결합과 집계

집계 함수에서 집계할 필드를 지정할 때 Django는 `filter()`에서 연관 테이블의 필드를 참조 할 때 사용하는 이중 밑줄 (`__`)표기법을 사용할 수 있다. 이때 Django는 관련 값을 검색하고 집계하는 데 필요한 모든 테이블을 `Join`한다.

예를들어 각 상점에서 제공되는 책의 가격 범위를 찾기위해 다음과 같이 사용할 수 있다.

```python
>>> from django.db.models import Max, Min
>>> Store.objects.annotate(min_price=Min('books__price'), max_price=Max('books__price'))
```

위 예시는 Django에게 Store모델을 검색하고 (M:M관계) Book 모델과 결합하고 가격 필드에서 집계하여 최소값 및 최대값을 찾도록 지시한다.

이 규칙은 `aggregate()`에서도 적용된다. 모든 상점에서 판매 할 수 있는 책의 최저 가격과 최고 가격을 알고 싶다면 다음과 같이 사용할 수 있다.

```python
>>> Store.objects.aggregate(min_price=Min('books__price'), max_price=Max('books__price'))
```

Join연결은 필요한 만큼 깊을 수 있다. 예를 들어 판매 가능한 책의 최연소 저자의 나이를 찾는다면 다음과 같이 사용할 수 있을 것이다.

```python
>>> Store.objects.aggregate(youngest_age=Min('books__authors__age'))
```

# 역참조 관계에서의 집계

결합관계에서의 조회와 유사한 방식으로 역참조 관계에서도 집계함수를 사용할 수 있다. 이때 관련 모델의 소문자 이름과 이중 밑줄 (`__`)이 사용된다.

예를들어 각각의 장부 재고 카운터로 주석이 달린 모든 출판사를 요청할 수도 있다. 다만, 출판사 → 도서의 외래키hop을 지정하기 위해 `book`을 사용하는 방법에 유의해야 한다.

```python
>>> from django.db.models import Avg, Count, Min, Sum
>>> Publisher.objects.annotate(Count('book'))
```

이때 결과 `QuerySet`의 모든 게시자는 `book__count`라는 추가 속성을 갖는다.

또한 우리는 모든 출판사가 발행한 책 중 가장 오래된 책의 결과를 얻을 수도 있다.

```python
>>> Publisher.objects.aggregate(oldest_pubdate=Min('book__pubdate'))
```

다만 이 예제에서는 `oldest_pubdate`라는 별칭을 사용했지만 별칭을 지정하지 않으면 `book__pubdatae__min`과 같은 다소 긴 필드명이 생성되므로 유의 해야한다.

또한 역참조 관계에서의 집계는 `M:M`관계에서도 적용된다. 예를들어 저자가 공동 또는 혼자서 쓴 모든 책을 고려하여 저자가 작성한 총 페이지수를 구할 수 도 있다.

```python
>>> Author.objects.annotate(total_pages=Sum('book__pages'))
```

또는 우리가 등록한 저자가 쓴 모든 책의 평균 등급을 구할 수도 있다.

```python
>>> Author.objects.aggregate(average_rating=Avg('book__rating'))
```

# 다른 QuerySet과의 집계

## filter( )와 exclude( )

집계는 필터의 인자로 사용할 수도 있다. 일반 모델 필드에 적용된 모든 `filter()` (또는 `exclude()`)는 집계 대상으로 고려되는 객체를 제한하는 효과가 있다.

`annotate()`와 함께  `filter()`를 사용하면 `annotate()`가 계산되는 객체를 제한하는 효과가 있다. 예를들어 제목이 'Django'로 시작하는 모든 도서를 쓴 저자를 `Count`할 수 있다.

```python
>>> from django.db.models import Count
>>> Book.objects.filter(name__startswith="Django").annotate(num_authors=Count('authors'))
```

마찬가지로 `aggregate()`와 함께 `filter()`를 사용해도 마찬가지의 효과를 기대할 수 있다. 예를들어 'Django'로 시작하는 모든 도서의 평균가를 구할 수 있다.

```python
>>> Book.objects.filter(name__startswith="Django").aggregate(Avg('price'))
```

### annotate( )의 filter

`annotate()`의 결과 값 역시 `filter()`를 사용할 수 있으며 이때 `annotate()`가 생성한 필드명을 기준으로 필터링 한다.

예를들어 저자가 두 명 이상인 도서 목록을 생성한다면 다음과 같이 사용 할 수 있다.

```python
>>> Book.objects.annotate(num_authors=Count('authors')).filter(num_authors__gt=1)
```

위 예제는 `annotate()`의 결과를 먼저 생성 한 후 이 결과를 바탕으로 `filter()`가 실행된다.

만약 두 개의 개별 필터가 있는 `annotate()`결과 가 필요하다면 모든 집계와 함께 `filter()`인수를 사용할 수 있다. 예를들어, 높은 평가를 받은 책의 수로 저자 목록을 생성하려면 다음과 같이 사용 할 수 있다.

```python
>>> highly_rated = Count('book', filter=Q(book__rating__gte=7))
>>> Author.objects.annotate(num_books=Count('book'), highly_rated_books=highly_rated)
```

이때 `QuerySet`결과의 각 작성자는 `num_books`및 `highly_rated_books` 속성을 갖는다.

### annotate( )와 filter( )의 질의 순서

`annotate()`와 `filter()`를 모두 사용하는 복잡한 쿼리를 개발할때는 질의 순서에 특히 주의해야한다. `annotate()`가 `QuerySet`에 적용되면 결과가 요청된 지점 까지 쿼리 상태에서 `annotate()`결과가 계산된다. 이것의 의미는 즉, `filter()`와 `annotate()`가 교환적이지 않다는 것이다.

다음과 같은 조건에서 `Count` 집계의 예를 살펴보자

- 출판사 A에는 등급 4와 5인 두 권의 책이 있다.
- 출판사 B에는 등급 1과 4의 책이 두 권 있다.
- 출판사 C에는 등급 1의 책이 한 권 있다.

```python
# Query 1
>>> a, b = Publisher.objects.annotate(num_books=Count('book', distinct=True)).filter(book__rating__gt=3.0)
>>> a, a.num_books
(<Publisher: A>, 2)
>>> b, b.num_books
(<Publisher: B>, 2)

# Query 2
>>> a, b = Publisher.objects.filter(book__rating__gt=3.0).annotate(num_books=Count('book'))
>>> a, a.num_books
(<Publisher: A>, 2)
>>> b, b.num_books
(<Publisher: B>, 1)
```

두 쿼리 모두 평점 3.0을 초과하는 도서가 하나 이상 있는 출판사 목록을 반환하므로 출판사 C는 제외된다. 다만 첫번째 쿼리를 보면 `annotate()`가 `filter()`앞에 있으므로 `filter()`가 `annotate()`에 영향을 주지 않는다. 이를 방지하려면 `distinct=True`가 필요하다. 

두 번째 쿼리는 각 출판사에 대해 평점이 3.0을 초과하는 도서 수를 계산한다. `filter()`가 `annotate()`앞에 있으므로 `filter()`는 `annotate()`가 계산할 때 고려되는 객체를 제한 할 수 있다.

다음은 평균 집계를 사용한 또 다른 예시이다.

```python
>>> a, b = Publisher.objects.annotate(avg_rating=Avg('book__rating')).filter(book__rating__gt=3.0)
>>> a, a.avg_rating
(<Publisher: A>, 4.5)  # (5+4)/2
>>> b, b.avg_rating
(<Publisher: B>, 2.5)  # (1+4)/2

>>> a, b = Publisher.objects.filter(book__rating__gt=3.0).annotate(avg_rating=Avg('book__rating'))
>>> a, a.avg_rating
(<Publisher: A>, 4.5)  # (5+4)/2
>>> b, b.avg_rating
(<Publisher: B>, 4.0)  # 4/1 (등급 1이 제외된 책)
```

첫 번째 쿼리는 평점이 3.0을 초과하는 책이 하나라도 있는 출판사에 대한 평균 등급이다. 두번째 쿼리는 평점이 3.0을 초과하는 책만을 집계하여 출판사에 대한 평균 등급을 구한 것이다.

## order_by( )

`annotate()`의 결과는 `order_by()`의 기준으로 사용할 수 있으며 별칭이 적용된 결과라도 해당 별칭을 기준으로 사용할 수 있다. 예를 들어 책을 작성한 저자 수에 따라 책 테이블을 정렬하고자 한다면 다음과 같이 사용할 수 있다.

```python
>>> Book.objects.annotate(num_authors=Count('authors')).order_by('num_authors')
```

## values( )

일반적으로 `annotate()`의 결과는 객체별로 생성되고 생성된 결과는 `QuerySet`의 각 객체에 대해 하나의 결과를 반환한다. 그러나 `values()`를 사용하여 결과 집합에 반환되는 열을 제한하면 `annotate()`결과를 생성하는 방법이 달라진다. 원본 `QuerySet`의 각 결과에 대해 `annotate()`결과를 반환하는 대신 `values()`에 지정된 필드의 고유 조합에 따라 그룹화 된다. 그런 다음 각 고유 그룹에 대한 `annotate()`결과가 제공된다. `annotate()`그룹은 모든 객체에 대해 계산된다. 이렇게 `group by`를 사용 가능하다.

예를 들어, 각 저자가 쓴 도서의 평균 등급을 찾으려고 한다면 다음과 같이 사용 할 수 있다.

```python
>>> Author.objects.values('name').annotate(average_rating=Avg('book__rating'))
```

위 예제에서 저자는 이름별로 그룹화되므로 고유한 각 저자 이름에 대한 `annotate()`결과만 얻을 수 있다. 즉, 동일한 이름을 가진 두 명의 작성자가 있는 경우 해당 결과가 단일한 결과로 병합된다. 평균은 두 저자가 쓴 책의 평균으로 계산된다.

### annotate( )와 values( )의 질의 순서

`filter()`와 마찬가지로 `annotate()`와 `values()`의 질의 순서도 중요하다. `values()`가 `annotate()`앞에 오는 경우 `annotate()`의 결과는 `values()`에서 정의한 그룹화를 사용하여 계산된다.

하지만 `anotate()`가 `values()`앞에 오면 전체 결과에 대한 `annotate()`결과가 생성된다. 이 경우 `values()`는 출력시 생성되는 필드만 제한하게 된다.

앞선 예제에서 `annotate()`와 `values()`의 순서를 반대로 하게 된다면 다음과 같다.

```python
>>> Author.objects.annotate(average_rating=Avg('book__rating')).values('name', 'average_rating')
```

이렇게 사용한다면 이제 각 저자에 대한 하나의 `annotate()`결과가 생성된다. 그러나 작성자의 이름과 `annotate()`가 생성한 `average_rating` 결과만 출력 데이터에 반환된다.

또한 `average_rating`이 반환 될 값 목록에 명시적으로 포함되어 있다는 점도 유의해야 한다. 이는 `values()` 및 `annotate()`의 질의 순서 때문에 필요하다.

만약 `values()`가 `annotate()`앞에 오게 되면 모든 `annotate()`결과가 `values()`에 자동으로 포함된다. 그러나 `annotate()`뒤에 `values()`가 오게되는 경우 `annotate()`가 생성한 결과 필드를 명시적으로 포함해줘야 한다.

### 기본 순서와의 상호작용 또는 order_by( )

`order_by()`에 사용된 (또는 모델의 기본 순서로 사용 지정)한 필드는 `values()`호출에서 별도로 지정하지 않아도 데이터를 반환할 때 적용된다. 이러한 추가 필드는 '유사'결과를 함께 그룹화 하는 데 사용되며 그렇지 않으면 동일한 결과 행이 분리 된 것처럼 보일 수 있다. 특히 계산할 때 두드러진다.

예를들어 다음과 같은 모델이 있다고 가정하자.

```python
from django.db import models

class Item(models.Model):
    name = models.CharField(max_length=10)
    data = models.IntegerField()

    class Meta:
        ordering = ["name"]
```

여기서 중요한 부분은 `Meta`클래스에서 `ordering`된 `name`필드이다. 각 고유 데이터 값이 나타나는 횟수를 계산하려면 다음과 같이 시도 할 수 있다.

```python
# Warning: 정확하지 않음
Item.objects.values("data").annotate(Count("id"))
```

item 객체를 공통 데이터 값으로 그룹화 한 다음 각 그룹의 ID 값 수를 계산한다. 물론 이것은 작동하지 않는다. (정확한 쿼리 아님) ordering된 name필드도 그룹화에서 역할을 하므로 이 쿼리는 사용자가 원하지 않는 (data, name )쌍으로 그룹화 된다. 이를 해결하면 다음과 같이 작성해야 한다.

```python
Item.objects.values("data").annotate(Count("id")).order_by()
```

ordering되는 name을 order_by()를 통해 지운다면 ordering의 영행 없이 data를 정렬 할 수 있다. data는 이미 쿼리에서 역할을 하고 있기 때문이다.

## Aggregating annotations

`annotate()`결과에 대한 `aggregate()`결과를 생성 할 수도있다. `aggregate()`를 정의할 때 제공하는 집계는 쿼리에서 `annotate()`결과의 일부로 정의 된 별칭을 참조 할 수 있다.

예를 들어, 책당 평균 저자 수를 계산 하려면 먼저 저자 수로 책 테이블을 `Count`한 다음 이 결과를 참조하여 저자의 평균 값을 계산해야 할 것이다. 이럴때는 다음과 같이 사용할 수 있다.

```python
>>> from django.db.models import Avg, Count
>>> Book.objects.annotate(num_authors=Count('authors')).aggregate(Avg('num_authors'))
{'num_authors__avg': 1.66}
```
