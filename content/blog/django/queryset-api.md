---
title: QuerySet API
date: 2020-11-06 18:11:58
category: Django
thumbnail: ../TIL/images/django.png
draft: false
---

![](../TIL/images/django.png)

# filter( )

```python
filter(**kwargs)
```

지정된 조회 매개 변수와 일치하는 객체를 포함하는 새로운 `QuerySet`을 반환한다.

조회 매개 변수 `(**kwargs)`는 필드 조회 형식이어야 한다. 여러 매개 변수는 기본 SQL문에서 `AND`를 통해 결합된다.

더 복잡한 쿼리 (예를들어 `OR`문이 있는 쿼리)를 실행해야 하는 경우 `Q objects`를 사용할 수 있다.

# exclude( )

```python
exclude(**kwargs)
```

지정된 조회 매개 변수와 일치하지 않는 객체를 포함하는 새로운 `QuerySet`을 반환한다.

조회 매개 변수 `(**kwargs)`는 필드 조회 형식이어야 한다. 여러 매개 변수는 기본 SQL문에서 `AND`를 통해 결합되며 전체가 `NOT()`으로 묶여 있다.

아래 예제는 `pub_date`가 2005-01-03이후 이며(`AND`) 제목이 'Hello'모든 항목을 제외한다.

```python
Entry.objects.exclude(pub_date__gt=datetime.date(2005, 1, 3), headline='Hello')
```

SQL문으로는 다음과 같다.

```sql
SELECT ...
WHERE NOT (pub_date > '2005-01-03' AND headline = 'Hello')
```

아래 예제는 `pub_date`가 2005-01-03이후 이고(`OR`) 제목이 'Hello'모든 항목을 제외한다.

```python
Entry.objects.exclude(pub_date__gt=datetime.date(2005, 1, 3)).exclude(headline='Hello')
```

SQL문으로는 다음과 같다.

```sql
SELECT ...
WHERE NOT pub_date > '2005-1-3'
AND NOT headline = 'Hello'
```

두번째 예제는 더 제한적이며 더 복잡한 쿼리 (예를들어 `OR`문이 있는 쿼리)를 실행해야 하는 경우 `Q objects`를 사용할 수 있다.

# annotate( )

```python
annotate(*args, **kwargs)
```

제공된 쿼리 식 목록을 사용하여 `QuerySet`의 각 객체에 주석을 추가한다. 식은 단순 값, 모델의 필드에 대한 참조 (또는 모든 관계 모델), 또는 객체와 관련된 객체에 대한 계산된 집계 식 (평균, 합계 등)일 수 있다.

`annotate()`의 각 인수는 반환되는 `QuerySet`의 각 객체에 추가되는 주석이다.

`**kwargs`를 사용하여 지정된 주석은 키워드 주석의 별칭으로 사용된다. 익명 인수에는 집계 함수의 이름과 집계되는 모델 필드에 따라 생성된 별칭이 있다. 단일 필드를 참조하는 집계식만 익명 인수가 될 수 있다. 그 밖에 모든 것은 키워드 인수여야 한다.

예를들어 블로그 목록을 조작하는 경우 각 블로그에 작성된 항목 수를 확인할 수 있다.

```python
>>> from django.db.models import Count
>>> q = Blog.objects.annotate(Count('entry'))
# 첫번째 블로그 이름
>>> q[0].name
'Blogasaurus'
# 첫번째 블로그의 항목 수
>>> q[0].entry__count
42
```

Blog 모델은 자체적으로 `entry__count`속성을 정의하지 않지만 `**kwargs`를 사용하여 집계 함수를 지정하면 주석의 이름을 사용자가 생성할 수 있으며 제어 할 수 있다.

```python
>>> q = Blog.objects.annotate(number_of_entries=Count('entry'))
# 제공된 이름 number of entries을 사용하는 첫번째 블로그의 항목 수
>>> q[0].number_of_entries
42
```

집계에 대한 자세한 내용은 [다음](https://yongineer.netlify.app/django/django-orm-:-aggregation/)을 참고한다.

# order_by( )

```python
order_by(*fields)
```

기본적으로 `QuerySet`에 의해 반환된 결과는 모델의 `Meta`클래스에 있는 `ordering`옵션에 의해 제공되는 `ordering tuple`에 따라 정렬된다. 하지만 `order_by`메소드를 사용하여 `QuerySet`별로 이를 재정의 할 수 있다.

```python
Entry.objects.filter(pub_date__year=2005).order_by('-pub_date', 'headline')
```

위의 결과는 `pub_date`를 내림차순으로 정렬한 다음 제목의 오름차순으로 정렬된다. `pub_date`앞의 `-`기호는 내림차순을 의미한다. 무작위로 정렬하려면 다음과 같이 `?`를 사용한다.

```python
Entry.objects.order_by('?')
```

참고로 `order_by('?')`는 성능이 매우 안좋으니 사용을 자제하는 것이 좋다.

다른 모델의 필드별로 정렬하려면 모델 관계에서 쿼리할 때와 동일한 구문을 사용한다. 즉, 필드 이름, double underscore(`__`), 새 모델의 필드 이름등이 결합하려는 모델 수만큼 계속된다. 예를들면 아래와 같다.

```python
Entry.objects.order_by('blog__name', 'headline')
```

다른 모델과 관련된 필드를 기준으로 정렬하려고 하면 `Django`는 관련 모델의 기본 순서를 사용하거나 `Meta`클래스의 `ordering`이 지정되지 않은 경우 관련 모델의 `Primary Key`를 기준으로 정렬한다. 예를들어, Blog모델에는 기본순서가 지정되어 있지 않기때문에

```python
Entry.objects.order_by('blog')
```

이것은 다음과 같다.

```python
Entry.objects.order_by('blog__id')
```

만약 Blog모델에 `ordering = ['name']`이 지정되어 있는 경우는 다음과 같다.

```python
Entry.objects.order_by('blog__name')
```

또한 표현식에서 `asc()` 또는 `desc()`를 호출하여 쿼리 표현식별로 정렬 할 수 도 있다.

```python
Entry.objects.order_by(Coalesce('summary', 'headline').desc())
Entry.objects.order_by(Coalesce('summary', 'headline').asc())
```

`asc()`와 `desc()`에서는 `null`값을 정렬하는 방법을 제어하는 인수 `nulls_first`및 `nulls_last`가 있다.

> **💡M:M 또는 역참조관계에서의 `order_by()`**
> 
결과를 정렬하기 위해 다중 값 필드를 지정할 수 있다.
>
 >```python
 ># Model
 >class Event(Model):
 >   parent = models.ForeignKey(
 >       'self',
 >       on_delete=models.CASCADE,
 >       related_name='children',
 >   )
 >   date = models.DateField()
 >```
 >
 >```python
 >Event.objects.order_by('children__date')
 >```
>
여기에서 각 이벤트에 대해 잠재적으로 여러 주문 데이터가 있을 수 있다. **여러 하위 항목이 있는 각 이벤트는 `order_by()`가 생성하는 새 `QuerySet`에 여러번 반환된다.** 즉, `QuerySet`에서 `order_by()`를 사용하면 처음에 작업했던 것 보다 더 많은 항목을 반환 할 수 있다. 이는 아마도 예상하지도 쓸모있지도 않은 데이터가 될 것이다.
>
따라서 다중 값 필드를 사용하여 결과를 정렬 할 때는 주의해야 한다. 주문하는 각 항목에 대해 하나의 주문 데이터만 있다고 확신할 수 있다면 이 방법은 문제되지 않는다. 그렇지 않은 경우 결과가 예상 한 결과인지 확인해야 할 필요가 있다.

> **⚠️ `order_by()`는 쿼리를 발생시킨다.**
 >
 >`order_by()`에 추가되는 각 필드는 쿼리를 발생시킨다. 추가하는 외래키 역시 포함된다.
 >
 >쿼리에 순서가 지정되지 않은 경우 데이터베이스에서 지정되지 않은 순서로 결과가 리턴된다. 특정 순서로 정렬할때는 결과에서 각 객체를 고유하게 식별하는 필드 집합으로 `order_by()`할때만 보장된다. 예를 들어 이름 필드가 고유하지 않은 경우 이름을 기준으로 정렬한다고 해서 이름이 같은 객체가 항상 같은 순서로 표시되는 것은 아니다.

# reverse( )

```python
reverse()
```

`reverse()`메서드를 사용하면 `QuerySet`의 요소가 리턴하는 순서를 반대로 만들수 있다. `reverse()`를 두번 호출한다면 다시 정상 방향으로 되돌아 온다.

쿼리셋에서 마지막 5개 항목을 검색하려면 다음과 같이 사용 할 수 있다.

```python
my_queryset.reverse()[:5]
```

이것은 `Python`에서 시퀀스 끝에 슬라이스 하는것과 완전히 같지 않다. 위의 예는 마지막 항목을 먼저 반환한 다음 두번째로 5개를 반환하는 절차를 진행한다. `Python`에서는 `seq[-5:]`와 같이 시퀀스의 순서를 반대로 뒤집지 않아도 마지막 5개의 항목을 가져올 수 있지만 `Django`에서는 이런 과정을 SQL에서 효율적으로 수행 할 수 없기때문에 끝에서 슬라이스와 같은 엑세스 모드를 지원하지 않는다.

또한 `reverse()`는 일반적으로 정의 된 순서가 있는 쿼리셋에서만 호출되어야 한다. (기본 순서를 정의하는 모델에 대해 쿼리 할 때 또는 `order_by()`를 사용 할 때), 만약 주어진 쿼리셋에 대해 이러한 순서가 정의되어 있지 않은 경우 `reverse()`를 호출 해도 효과가 없다.

# distinct( )

```python
distinct()

# Only PostgreSQL
distinct(*fields)
```

SQL문법에서 `SELECT DISTINCT`를 사용하는 새로운 QuerySet을 리턴한다. 이렇게 하면 쿼리 결과에서 중복 행이 제거된다.

기본적으로 `QuerySet`은 중복행을 제거하지 않는다. 실제로 `Bolg.objects.all()`과 같은 간단한 쿼리는 결과 행이 중복 될 가능성이 없기 때문에 이는 거의 문제가 되지 않는다. 그러나 쿼리가 여러 테이블을 걸쳐야 한다면 중복되는 결과를 얻을 수 있다 이럴때 `distinct()`를 사용한다.

PostgreSQL에서는 `distinct()`에 Positional arguments를 전달 할 수 있다. 이때 인자 값은 필드 값이다. 이것은 `SELECT DISTINCT ON`과 같은 SQL쿼리로 변환된다 하지만 Raw SQL과 차이점이 존재하는데 일반적인 `distinct()`호출의 경우 데이터 베이스는 구별되는 행을 결정할 때 각 행의 각 필드를 비교한다. 하지만 ORM에서는 지정된 필드를 인자로 갖는  `distinct()`호출의 경우 데이터베이스가 지정된 필드 이름만 비교한다.

> **💡필드를 인수로 갖는 `distinct()`**
 >
 >필드 이름을 지정할 때 QuerySet에는 `order_by()`가 필요하며 `order_by()`의 필드는 `distinct()`에 제공된 필드와 동일하게 지정되어야 한다.
 >
 >예를들어 `SELECT DISTINCT ON (a)`는 a열의 각 값에 대한 첫 번째 행을 제공한다. 만약 ordering이 되어 있지 않다면 임의의 행이 리턴된다.
 >
 >예시 (PostgreSQL에서만 가능)
 >
 >```python
 >>>> Author.objects.distinct()
 >
 >>>> Entry.objects.order_by('pub_date').distinct('pub_date')
 >
 >>>> Entry.objects.order_by('blog').distinct('blog')
 >
 >>>> Entry.objects.order_by('author', 'pub_date').distinct('author', 'pub_date')
 >
 >>>> Entry.objects.order_by('blog__name', 'mod_date').distinct('blog__name', 'mod_date')
 >
 >>>> Entry.objects.order_by('author', 'pub_date').distinct('author')
 >```

> **⚠️ `order_by()`와 `distinct()`**
 >
 >`order_by()`호출에 사용된 모든 필드는 SQL `SELECT`컬럼에 포함된다. 이로 인해 `distinct()`와 함께 사용하면 예기치 않은 결과가 발생할 수 있다. 만약 연계된 모델의 필드를 기준으로 정렬하면 해당 필드가 선택한 열에 추가되고 그렇지 않으면 중복된 행이 구별되는 것 처럼 보일 수 있다. 추가 열은 반환된 결과에 표시되지 않기 때문에 (순서 지정을 지원하기 위해 존재하는 경우에만 리턴값에 표시) 때때로 명확하지 않은 결과가 반환되는 것처럼 보인다.
 >
 >마찬가지로 `values()`를 사용하여 선택한 열을 제한하는 경우 `order_by()`(또는 기본 ordering)에 사용된 열이 계속 관련되며 결과의 고유성에 영향을 미칠수 있다.
 >
 >여기서 중요한것은 **여러 모델이 연관되어 있는 쿼리셋에  `distinct()`를 사용하는 경우 정렬 기준이 관련 모델의 필드라면 주의**해야 한다는 것이다. 마찬가지로, `**distinct()`및 `values()`를 함께 사용할 때는 `values()`의 필드가 아닌 이외의 필드로 정렬 할 때 주의**해야 한다.
 >
 >또한 `order_by()`는 정의된 기본 관련 모델 순서를 사용한다. `DISTINCT ON`식이 `ORDER BY`절의 시작 부분에 있는 식과 일치하는지 확인하려면 관계의 `_id`또는 참조된 필드를 기준으로 명시적으로 정렬해야 할 수도 있다. 예를 들어 Blog모델이 이름으로 정렬을 정렬한 경우
 >
 >```python
 >Entry.objects.order_by('blog').distinct('blog')
 >```
 >
 >위 예시는 쿼리가 `blog__name`으로 정렬되어 `DISTINCT ON`표현식과 일치하지 않기 때문에 작동하지 않는다. 두 표현식이 일치하는지 확인하려면 관계 `_id`필드 (이 경우 `blog_id`)또는 참조 된 필드 (`blog_pk`)를 기준으로 명시적으로 정렬해야 한다.

# values( )

```python
values(*field, **expressions)
```

iterable로 사용될 때 모델 인스턴스가 아닌 `dictionary`을 반환하는 `QuerySet`을 반환한다.

각 `dirctionary`는 모델 객체의 속성 이름에 해당하는 키가 있는 객체를 나타낸다. 다음의 예제는 `values()`가 반환하는 `dictionary`와 일반 모델 객체를 비교한다.

```python
# 이 목록에는 Blog 객체가 포함되어 있다.
>>> Blog.objects.filter(name__startswith='Beatles')
<QuerySet [<Blog: Beatles Blog>]>

# 이 목록에는 dictionary가 포함되어 있다.
>>> Blog.objects.filter(name__startswith='Beatles').values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
```

`values()` 메서드는 `SELECT`가 제한되어야 하는 필드 이름을 지정하는 optional positional arguments 인 `*fields`를 사용한다. 필드를 지정하면 각 `dictionary`에는 지정한 필드에 대한 key와 대응하는 value만 포함된다. 필드를 지정하지 않으면 `dictionary`에 데이터베이스 테이블의 모든 필드에 대한 key와 value가 포함된다.

```python
>>> Blog.objects.values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
>>> Blog.objects.values('id', 'name')
<QuerySet [{'id': 1, 'name': 'Beatles Blog'}]>
```

`values()`메서드는 `annotate()`로 전달되는 optional keyword arguments인 `**expresstions`을 사용한다.

```python
>>> from django.db.models.functions import Lower
>>> Blog.objects.values(lower_name=Lower('name'))
<QuerySet [{'lower_name': 'beatles blog'}]>
```

`values()`메서드는 정렬된 사용자 지정조회를 사용할 수 있다.

```python
>>> from django.db.models import CharField
>>> from django.db.models.functions import Lower
>>> CharField.register_lookup(Lower)
>>> Blog.objects.values('name__lower')
<QuerySet [{'name__lower': 'beatles blog'}]>
```

`values()`절 내의 집계는 동일한 `values()`절 내의 다른 인수 보다 우선순위가 높다. 다른 값으로 그룹화 해야하는 경우 `value()`절과 분리하여 집계함수를 사용해야 한다.

```python
>>> from django.db.models import Count
>>> Blog.objects.values('entry__authors', entries=Count('entry'))
<QuerySet [{'entry__authors': 1, 'entries': 20}, {'entry__authors': 1, 'entries': 13}]>
>>> Blog.objects.values('entry__authors').annotate(entries=Count('entry'))
<QuerySet [{'entry__authors': 1, 'entries': 33}]>
```

사용 가능한 일부 필드의 값만 필요하고 모델 인스턴스 객체의 기능이 필요하지 않다는 것을 알고있을때 유용하다. 사용해야 하는 필드만 선택하는 것이 효율적이다.

마지막으로 `values()`호출 후에 `filter()`, `order_by()`등을 호출 할 수 있다. 이는 이 두 호출이 동일하다는 것을 의미한다.

```python
Blog.objects.values().order_by('id')
Blog.objects.order_by('id').values()
```

`OneToOneField`, `ForeignKey`및 `ManyToManyField`속성을 통해 역참조가 있는 관련 모델의 필드를 참조 할 수도 있다.

```python
>>> Blog.objects.values('name', 'entry__headline')
<QuerySet [{'name': 'My blog', 'entry__headline': 'An entry'},
     {'name': 'My blog', 'entry__headline': 'Another entry'}, ...]>
```

> **⚠️ `ManyToManyField`에서 `values()`사용시 주의 사항**
>
`ManyToManyField`속성 및 역참조 관계에는 여러 행이 있을 수 있으므로 이를 포함하여 결과 집합의 리턴값이 배로 늘어나는 결과를 얻을 수 있다. 이는 `values()`쿼리에 이러한 필드를 여러 개 포함하는 경우 특히 자주 발생하며, 이 경우 가능한 모든 조합이 반환된다. 

### 몇가지 유의 사항

- 만약 외래키인 `foo`라는 필드가 있는 경우 기본적으로 `values()`호출은 실제 값을 저장하는 숨겨진 모델 속성의 이름이기 때문에 `foo_id`라는 `dictionary` key를 반환하게 된다. (`foo` 속성은 연관 모델을 나타낸다) `values()`를 호출하고 필드 이름을 전달할 때 `foo` 또는 `foo_id`를 전달할 수 있으며 동일한 결과를 얻을 수 있다. (`dictionary`key는 전달한 필드 이름과 일치한다.)

    ```python
    >>> Entry.objects.values()
    <QuerySet [{'blog_id': 1, 'headline': 'First Entry', ...}, ...]>

    >>> Entry.objects.values('blog')
    <QuerySet [{'blog': 1}, ...]>

    >>> Entry.objects.values('blog_id')
    <QuerySet [{'blog_id': 1}, ...]>
    ```

- `distinct()`와 함께 `values()`를 사용할 때 사용하는 순서가 결과에 영향을 줄 수 있다.
- `extra()`호출 후에 `values()`절을 사용하는 경우 `extra()`에서 선택인자로 정의된 모든 필드가 `values()`호출에 명시적으로 포함되어야 한다. `values()`호출 이후에 수행된 `extra()`호출은 추가로 선택된 필드가 무시된다.
- `values()`뒤에 `only()`및 `defer()`를 호출하는 것은 의미가 없으므로 `TypeError`가 발생한다.
- 변환과 집계를 결합하려면 명시적으로 또는 `values()`에 대한 키워드 인수로 두 개의 `annotate()`호출을 사용해야 한다. 위와 같이 해당 필드 유형에 변환이 등록된 경우 첫번째 `annotate(0`를 생략 할 수 있으므로 다음 예제는 동일한 결과를 갖는다.

    ```python
    >>> from django.db.models import CharField, Count
    >>> from django.db.models.functions import Lower
    >>> CharField.register_lookup(Lower)
    >>> Blog.objects.values('entry__authors__name__lower').annotate(entries=Count('entry'))
    <QuerySet [{'entry__authors__name__lower': 'test author', 'entries': 33}]>
    >>> Blog.objects.values(
    ...     entry__authors__name__lower=Lower('entry__authors__name')
    ... ).annotate(entries=Count('entry'))
    <QuerySet [{'entry__authors__name__lower': 'test author', 'entries': 33}]>
    >>> Blog.objects.annotate(
    ...     entry__authors__name__lower=Lower('entry__authors__name')
    ... ).values('entry__authors__name__lower').annotate(entries=Count('entry'))
    <QuerySet [{'entry__authors__name__lower': 'test author', 'entries': 33}]>
    ```

# values_list( )

```python
values_list(*fields, flat=False, named=False)
```

반환 값이 `values()`와 다르게 튜플을 반환한다는 점을 제외하면 `values()`와 유사하다. 각 튜플에는 `values_list()`호출에 전달된 각 필드 또는 표현식의 값이 포함된다. 따라서 첫번째 항목은 첫번째 필드이다. 예를들면 다음과 같다.

```python
>>> Entry.objects.values_list('id', 'headline')
<QuerySet [(1, 'First entry'), ...]>
>>> from django.db.models.functions import Lower
>>> Entry.objects.values_list('id', Lower('headline'))
<QuerySet [(1, 'first entry'), ...]>
```

단일 필드만 전달하는 경우 `flat`매개 변수도 전달할 수 있다. `True`면 리턴된 결과가 단일 튜플이 아니라 단일 값임을 의미한다.

```python
>>> Entry.objects.values_list('id').order_by('id')
<QuerySet[(1,), (2,), (3,), ...]>

>>> Entry.objects.values_list('id', flat=True).order_by('id')
<QuerySet [1, 2, 3, ...]>
```

단, 필드가 두개 이상인 경우 `flat`은 에러를 발생 시킨다.

`named = True`를 전달하여 `namedtuple()`을 결과로 얻을 수 있다.

```python
>>> Entry.objects.values_list('id', 'headline', named=True)
<QuerySet [Row(id=1, headline='First entry'), ...]>
```

`namedtuple()`을 이용하면 결과를 더 읽기 쉽게 만들 수 있지만 `namedtuple()`형으로 변환되면서 약간의 성능 저하가 발생된다.

`values_list()`에 값을 전달하지 않으면 선언된 순서대로 모델의 모든 필드가 반환된다.

일반적으로 `values_list()`를 사용하는것은 특정 모델 인스턴스의 특정 필드의 값을 가져올 때 사용한다. 이를 위해 `values_list()`다음 `get()`을 호출한다.

```python
>>> Entry.objects.values_list('headline', flat=True).get(pk=1)
'First entry'
```

`values()`및 `values_list()`는 모두 특정 사용 사례에 대한 최적화를 위한 것이다. 모델 인스턴스를 만드는 오버헤드 없이 데이터 하위 집합을 검색한다. 따라서 **M:M 및 기타 다중 관계 (예 : 1:M관계에서 역참조)를 다룰 때 "하나의 행, 하나의 객체"가 적용되지 않고 모두 분리 된다.**

다음은 `ManyToManyField`를 쿼리 할 때의 동작이다.

```python
>>> Author.objects.values_list('name', 'entry__headline')

<QuerySet [('Noam Chomsky', 'Impressions of Gaza'),
 ('George Orwell', 'Why Socialists Do Not Believe in Fun'),
 ('George Orwell', 'In Defence of English Cooking'),
 ('Don Quixote', None)]>
```

다수의 책을 작성한 작성자는 여러번 표시되고 항목이 없는 작성자는 `entry__headline`이 `None`으로 표시되는 것을 확인 할 수 있다.

마찬가지로 역참조를 쿼리할 때 작성자가 없는 항목에 대해 `None`으로 표시된다.

```python
>>> Entry.objects.values_list('authors')

<QuerySet [('Noam Chomsky',), ('George Orwell',), (None,)]>
```

# date( )

```python
date(field, kind, order='ASC')
```

`QuerySet`의 내용 내에서 특정 종류의 사용 가능한 모든 날짜를 나타내는 `datetime.date` 객체 목록으로 평가되는 `QuerySet`을 리턴한다.

필드는 모델의 `DateField`어야 한다. `kind`는 `year`, `month`, `week`, `day`어야 한다. 결과 목록의 각 `datetime.date` 객체는 주어진 유형으로 데이터를 잘라낸다.

- `year`는 필드에 대한 모든 고유 연도 값이 담긴 list를 반환한다.
- `month`는 필드에 대한 고유한 연도/월 값이 담긴 list를 반환한다.
- `week`는 필드에 대한 모든 고유한 연도/주 값이 담긴 list를 반환하며 모든 날짜는 월요일이다.
- `day`는 필드에 대한 모든 고유한 연/월/일 값이 담긴 list를 반환한다.

기본적으로 정렬되는 순서는 `ASC`(오름차순)이며 `order`를 지정할 때는 `ASC`또는 `DESC`여야 한다.

```python
>>> Entry.objects.dates('pub_date', 'year')
[datetime.date(2005, 1, 1)]
>>> Entry.objects.dates('pub_date', 'month')
[datetime.date(2005, 2, 1), datetime.date(2005, 3, 1)]
>>> Entry.objects.dates('pub_date', 'week')
[datetime.date(2005, 2, 14), datetime.date(2005, 3, 14)]
>>> Entry.objects.dates('pub_date', 'day')
[datetime.date(2005, 2, 20), datetime.date(2005, 3, 20)]
>>> Entry.objects.dates('pub_date', 'day', order='DESC')
[datetime.date(2005, 3, 20), datetime.date(2005, 2, 20)]
>>> Entry.objects.filter(headline__contains='Lennon').dates('pub_date', 'day')
[datetime.date(2005, 3, 20)]
```

# datetimes( )

```python
datetimes(field_name, kind, order='ASC', tzinfo=None, is_dst=None)
```

`QuerySet`의 내용 내에서 특정 종류의 사용 가능한 모든 날짜를 나타내는 `datetime.datetime` 객체 목록으로 평가되는 `QuerySet`을 리턴한다.

`field_name`은 모델의 `DateTimeField`여야 한다. `kind`는 `year`, `month`, `week`, `day`, `hour`, `minute`, `second`어야 한다. 결과 목록의 각 `datetime.datetime` 객체는 주어진 유형으로 데이터를 잘라낸다.

기본적으로 정렬되는 순서는 `ASC`(오름차순)이며 `order`를 지정할 때는 `ASC`또는 `DESC`여야 한다.

`tzinfo`는 자르기 전에 `datetime`이 변환되는 시간대를 정의한다. 실제로 주어진 `datetime`은 사용중인 시간대에 따라 다른 표현을 갖는다. 이 매개 변수는 `datetime.tzinfo`객체여야 한다. `None`이면 Django의 현재 시간대를 사용한다. `USE_TZ`가 `False`면 아무런 변화가 없다.

`is_dst`는 `pytz`가 서머타임을 사용하는지에 대한 여부를 나타낸다. 기본적으로 (`is_dst = None`) `pyzt`는 이러한 날짜 시간에 대해 예외를 발생시킨다.

 > **💡데이터베이스의 시간대를 사용하는 `datetime()`**
 >
 >`datetime()`은 데이터베이스에서 직접 시간대 변환을 수행한다. 따라서 데이터베이스는 `tzinfo.tzname (None)`의 값을 해석 할 수 있어야 한다. 따라서 데이터베이스는 다음과 같은 요구사항이 충족되어야 한다.
 >
 >- SQLite : 요구 사항이 없다. 변환은 `Python`에서 `pytz`를 사용하여 수행되며 이 패키지는 Django를 설치할때 이미 설치되어 있는 패키지이다.
 >- PostgreSQL : 요구사항 없음 ([참조](https://www.postgresql.org/docs/current/datatype-datetime.html#DATATYPE-TIMEZONES))
 >- Oracle : 요구사항 없음 ([참조](https://docs.oracle.com/en/database/oracle/oracle-database/18/nlspg/datetime-data-types-and-time-zone-support.html#GUID-805AB986-DE12-4FEA-AF56-5AABCD2132DF))
 >- MySQL : `mysql_tzinfo_to_sql`을 사용하여 시간대 테이블을 가져온다. ([참고](https://dev.mysql.com/doc/refman/8.0/en/mysql-tzinfo-to-sql.html))

# none( )

```python
none()
```

`none()`을 호출하면 객체를 반환하지 않는 쿼리셋이 생성되고 결과에 엑세스 할 때 쿼리가 실행되지 않는다. `qs.none()`쿼리셋은 `EmptyQuerySet`의 인스턴스가 된다.

```python
>>> Entry.objects.none()
<QuerySet []>
>>> from django.db.models.query import EmptyQuerySet
>>> isinstance(Entry.objects.none(), EmptyQuerySet)
True
```

# all( )

```python
all()
```

현재 `QuerySet` (또는 `QuerySet` 하위 클래스)의 복사본을 반환한다. 이는 모델 관리자 또는 `QuerySet`에 전달하고 결과에 대한 추가 필터링을 수행하려는 상황에서 유용할 수 있다. 두 객체 모두 `all()`을 호출 하면 작업할 `QuerySet`을 확실하게 갖게된다.

`QuerySet`이 평가 될 때 일반적으로 결과를 캐시한다. `QuerySet`이 평가 된 이후 데이터베이스의 데이터가 변경된 경우 이전에 평가된 `QuerySet`에서 `all()`을 호출하여 동일한 쿼리에 대한 업데이트 된 결과를 얻을 수 있다.

# union( )

```python
union(*other_qs, all=False)
```

SQL의 `UNION`을 사용하여 둘 이상의 `QuerySet` 결과를 결합한다.

```python
>>> qs1.union(qs2, qs3)
```

UNION 연산자는 기본적으로 고유한 값만 선택한다. (`distinct()`가 기본적으로 수행됨.) 이 때 중복 값을 허용하려면 `all = True`인수를 사용한다.

`union()`, `intersection()`, `difference()`는 인수가 다른 모델의 `QuerySet`인 경우에도 첫번째 `QuerySet`유형의 모델 인스턴스를 반환한다. `SELECT` 목록이 모든 `QuerySet`에서 동일하면 다른 모델을 전달하는 것이 작동한다. (최소한 유형, 유형이 동일한 순서면 이름이 중요하지 않다.) 이러한 경우 `QuerySet`에 적용된 `QuerySet`메서드의 첫번째 `QuerySet`의 열 이름을 사용해야 한다.

```python
>>> qs1 = Author.objects.values_list('name')
>>> qs2 = Entry.objects.values_list('headline')
>>> qs1.union(qs2).order_by('name')
```

또한 결과 QuerySet에는 `LIMIT`, `OFFSET`, `COUNT(*)`, `ORDER BY`및 열 지정 (슬라이싱, `count()`, `order_by()`, `value()`, `values_list()`)만 허용된다. 데이터베이스는 결합 된 쿼리에서 허용되는 작업에 제한을 둔다. 예를 들어 대부분의 데이터베이스는 결합 된 쿼리에서 `LIMIT`또는 `OFFSET`을 허용하지 않는다.

# intersection( )

```python
intersection(*other_qs)
```

SQL의 `INTERSECT`를 사용하여 둘 이상의 `QuerySet`의 교집합을 반환한다.

```python
>>> qs1.intersection(qs2, qs3)
```

`intersection()`의 제한사항은 `union()`과 동일하다.

# difference( )

```python
difference(*other_qs)
```

SQL의 `EXCEPT`연산자를 사용하여 둘 이상의 `QuerySet`의 차집합을 반환한다.

```python
>>> qs1.difference(qs2, qs3)
```

`difference()`의 제한사항은 `union()`과 동일하다.

# extra( )

```python
extra(select=None, where=None, params=None, tables=None, order_by=None, select_params=None)
```

때때로 Django 쿼리 구문 만으로는 복잡한 `WHERE`절을 쉽게 표현할 수 없다. 이러한 상황을 위해 Django는 `extra()QuerySet`한정 자를 제공한다. 이것은 `QuerySet`에 의해 생성된 SQL에 특정 절을 삽입하기 위한 방법이다.

> **💡이 방법은 최후의 수단으로 사용할 것**
 >
 >`extra()`는 향후 Django에서 지원을 중단하려는 오래된 API이다. 다른 쿼리셋 메서드를 사용하여 쿼리를 표현할 수 없는 경우에만 사용할 것을 권장한다. Django는 더 이상 `extra()`에 대한 버그를 개선하거나 수정하지 않는다.
 >
 >```python
 >>>> qs.extra(
 >...     select={'val': "select col from sometable where othercol = %s"},
 >...     select_params=(someparam,),
 >... )
 >```
 >
 >이것은 `RawSQL`을 사용하는 다음과 동일하다.
 >
 >```python
 >>>> qs.annotate(val=RawSQL("select col from sometable where othercol = %s", (someparam,)))
 >```
 >
 >`RawSQL`사용시 주요 이점은 필요한 경우 `output_field`를 설정할 수 있다는 것이다. 주된 단점은 raw SQL에서 쿼리셋의 일부 테이블 별칭을 참조하면 Django가 해당 별칭을 변경할 수 있다는 것이다. (예: 쿼리셋이 또 다른 쿼리에서 서브쿼리로 사용되는 경우)

> **⚠️ SQL injection에 취약한 `extra()`**
 >
 >`extra()`를 사용할 때는 SQL injection공격에 매우 취약하므로 사용자가 매개 변수를 사용하여 제어할 수 있는 모든 매개 변수를 이스케이프해야 한다.
 >
 >또한 SQL문자열에서 자리 표시자를 인용하지 않아야 한다. 이 예제는 `%s`주위의 `''`사용으로 이해 SQL injection에 취약하다.
 >
 >```python
 >SELECT col FROM sometable WHERE othercol = '%s'  # unsafe!
 >```
 >
 >Django의 SQL injection 보호 작동 방식에 대해선 [여기](https://docs.djangoproject.com/en/3.1/topics/security/#sql-injection-protection)를 참고 하도록 하자.

# defer( )

```python
defer(*fields)
```

일부 복잡한 데이터 모델링에서는 모델에 많은 필드가 포함되며, 그중 일부에는 많은 데이터(ex. `TextField`)가 포함될 수 있거나 Python 객체로 변환하기 위해 무거운 처리가 필요할 수 있다. 데이터를 처음 가져올 때 특정 필드가 필요한지 여부를 알 수 없는 상황에서 쿼리셋의 결과를 사용하는 경우 Django에 데이터베이스에서 검색하지 않도록 지시 할 수 있다.

이는 로드 되지 않도록 필드이름을  `defer()`에 전달하여 수행된다.

```python
# body와 headline필드를 모두 검색에서 제외 시킨다.
Entry.objects.defer("body").filter(rating=5).defer("headline")
```

지연된 집합에 필드가 추가되는 순서는 중요하지 않다. 이미 검색하지 않도록 제거된 필드 이름으로 `defer()`를 호출하는 것은 해가 되지 않는다. (필드는 여전이 검색에서 제외되어 있다.)

double underscore (`__`)를 사용하여 관련 필드를 구분하여 관련된 모델의 필드를 검색에서 제외시킬 수 있다.(`select_related`를 통해 쿼리셋을 가져올 경우)

```python
Blog.objects.select_related().defer("entry__headline", "entry__body")
```

검색에서 제외된 필드 집합을 해제하려면 `defer()`의 매개변수로 `None`을 전달한다.

```python
# 모든 필드를 가져온다.
my_queryset.defer(None)
```

모델의 일부 필드는 검색에서 제외되지 않는다. 특히 Primary Key는 검색에서 제외 시킬 수 없다. `select_related()`를 사용하여 관련 모델을 검색하는 경우 기본 모델에서 관련모델로 연결되는 필드를 검색에서 제외시키면 안된다. 만약 검색에서 제외된다면 에러가 발생한다.

검색에서 제외된 필드가 있는 인스턴스에 대해 `save()`를 호출하면 검색된 필드만 저장된다.

# only( )

```python
only(*fields)
```

`only()`메서드는 `defer()`와 반대되는 개념이다. 모델을 검색할 때 반드시 포함되어야 하는 필드를 사용하여 호출한다. 거의 모든 필드를 검색에서 제외해야 하는 모델이 있는 경우 `only()`를 사용하면 코드가 더 간결해 질 수 있다.

이름, 나이, 생애전기가 있는 모델이 있다고 가정하고 다음 두 개의 쿼리셋은 검색되는 필드의 제한에 있어서 동일하다.

```python
Person.objects.defer("age", "biography")
Person.objects.only("name")
```

`only()`를 호출 할 때마다 즉시 로드할 필드 세트를 대체 한다. 메서드 이름은 `mnemonic`이다. 해당 필드 만 즉시 로드되며 나머지는 검색에서 제외된다. 따라서 `only()`를 연속적으로 호출하면 마지막 필드만 고려된다.

```python
# 최종 결과는 `headline`을 제외한 모든 것이 검색에서 제외 된다.
Entry.objects.only("headline", "body").defer("body")

# 최종 결과는 `headline`과 `body`를 즉시 로드한다. (only()는 기존 필드 세트를 대체한다.)
Entry.objects.defer("body").only("headline", "body")
```

`defer()`의 모든 주의 사항은 `only()`에도 적용된다. 다른 옵션을 다 사용한 후에만 조심스럽게 사용해야 한다.`only()`를 사용하고 `select_related()`를 사용하여 요청 된 필드를 생략하는 것도 에러가 발생한다.

검색에서 제외된 필드가 있는 인스턴스에 대해 `save()`를 호출하면 검색된 필드만 저장된다.

# using( )

```python
using(alias)
```

이 메소드는 둘 이상의 데이터베이스를 사용하는 경우 쿼리셋이 평가 될 데이터베이스를 제어하기 위한 것이다. 이 메소드의 매개변수는 `DATABASES`에 정의된 데이터베이스 별명이다.

```python
# 'default'라고 지정된 데이터베이스를 사용한다.
>>> Entry.objects.all()

# 'backup'라고 지정된 데이터베이스를 사용한다.
>>> Entry.objects.using('backup')
```

# select\_for\_update( )

```python
select_for_update(nowait=False, skip_locked=False, of=())
```

트랜잭션이 끝날 때까지 행에 락을 걸고 지원되는 데이터베이스에서 `SELECT ... FOR UPDATE` SQL문을 생성하는 쿼리셋을 반환한다.

```python
from django.db import transaction

entries = Entry.objects.select_for_update().filter(author=request.user)
with transaction.atomic():
    for entry in entries:
        ...
```

쿼리셋을 평가 될 때 (이 경우 항목의 항목에 대해) 일치하는 모든 항목은 트랜잭션 블록이 끝날 때까지 락이 걸린다. 즉, 다른 트랜잭션이 해당 항목에 대한 락을 변경하거나 획득하지 못하도록 한다.

일반적으로 다른 트랜잭션이 선택한 행 중에 하나에 대해 이미 락이 걸린 경우 락이 해제될 때 까지 쿼리가 차단된다. 이것이 원하는 동작이 아닌 경우 `select_for_update(nowait = True)`를 호출한다. 이렇게하면 호출이 차단되지 않는다 충돌하는 락이 이미 다른 트랜잭션에 의해 획득 된 경우 쿼리셋이 평가 될 때 데이터베이스 에러가 발생한다. 대신 `select_for_update(skip_locked = True)`를 사용하여 락이 걸린 행을 무시할 수도 있다. `nowait`과 `skip_locked`는 상호 배타적이며 두 옵션을 모두 활성화 한 상태에서 `select_for_update()`를 호출하면 `ValueError`가 발생한다.

기본적으로 `select_for_update()`는 쿼리에서 선택한 모든 행에 락을 건다. 예를 들어 `select_related()`에 지정된 관련 객체의 행은 쿼리 셋 모델의 행과 함께 잠긴다. 원하지 않는 경우 `select_related()`와 동일한 필드 구문을 사용하여 `select_for_update(of = (...))`를 통해 락을 걸려는 관련 객체를 지정한다. 검색어 세트의 모델을 참조하려면 `self`를 사용한다.

> **💡`select_for_update(of=(...))`에서 상위 모델 락 걸기**
 >
 >다중 테이블 상속을 사용할 때 상위 모델에 락을 걸려면 `of`인수에 상위 링크 필드 (기본적으로 `<parent_model_name>_ptr`)를 지정해야 한다. 예를 들면
 >
 >```python
 >Restaurant.objects.select_for_update(of=('self', 'place_ptr'))
 >```

`nullable`관계에는 `select_for_update()`를 사용할 수 없다.

```python
>>> Person.objects.select_related('hometown').select_for_update()

Traceback (most recent call last):
...
django.db.utils.NotSupportedError: FOR UPDATE cannot be applied to the nullable side of an outer join
```

이러한 제한을 회피하기 위해 `null`이 필요 없는 경우 `null`객체를 제외 시킬 수 있다.

```python
>>> Person.objects.select_related('hometown').select_for_update().exclude(hometown=None)
<QuerySet [<Person: ...)>, ...]>
```

현재 PostgreSQL, Oracle, MySQL데이터베이스는 `select_for_update()`를 지원한다. 그러나 MariaDB 10.3+는 `nowait`만 지원되며 MySQL 8.0.1+는 `nowait`및 `skip_locked`를 지원한다. MySQL과 MariaDB는 `of`인수를 지원하지 않는다.

MySQL과 같이 이러한 옵션을 지원하지 않는 데이터베이스를 사용하여 `nowait=True, skip_locked = True`또는 `select_for_update(of=(...))`를 전달하면 `NotSupportedError`가 발생한다.

`SELECT ... FOR UPDATE`를 지원하는 데이터베이스의 자동 커밋 모드에서 `select_for_update()`를 사용하여 쿼리세트를 평가하는 것은 이 경우 행에 락이 걸리지 않기 때문에 `TransactionManagementError`가 발생한다. 만약 에러가 발생하지 않게 허용한다면 트랜잭션외부의 트랜잭션에서 실행 될 것으로 예상되는 코드를 호출하여 데이터 손상이 쉽게 발생할 수 있다.

`SELECT ... FOR UPDATE`를 지원하지 않는 데이터베이스 (ex. SQLite)에서 `select_for_update()`를 사용하면 효과가 없다. `SELECT ... FOR UPDATE`는 쿼리에 추가되지 않으며 자동 커밋 모드에서 `select_for_update()`를 사용하면 오류가 발생하지 않는다.

> **⚠️ TestCase에서 `select_for_update()`사용시 주의 사항**
 >
 >`select_for_update()`는 일반적으로 자동 커밋 모드에서 실패하지만 TestCase는 트랜잭션의 각 테스트를 자동으로 래핑하므로 `atomic()`블록 외부에서도 TestCase에서 `select_for_update()`를 호출하면 `TransactionManagementError`를 발생시키지 않고 (아마도 예기치 않게) 통과된다.`select_for_update()`를 제대로 테스트 하려면 `TransactionTestCase`를 사용해야 한다.

> **💡지원되지 않는 표현 : Window function**
>
`select_for_update()`PostgreSQL의 Window function expression을 지원하지 않는다.

# raw( )

```python
raw(raw_query, params=None, translations=None)
```

raw SQL 쿼리를 받아서 실행하고 `django.db.models.query.RawQuerySet`인스턴스를 반환한다. 이 `RawQuerySet`인스턴스는 객체 인스턴스를 제공하기 위해 일반 `QuerySet`처럼 반복 될 수 있다.

자세한 내용은 [여기](https://docs.djangoproject.com/en/3.1/topics/db/sql/)를 참조한다.

> **⚠️ `raw()`사용시 주의 사항**
>
`raw()`는 항상 새 쿼리를 트리거하고 이전 필터링을 고려하지 않는다. 따라서 일반적으로 Manager 또는 새로운 쿼리셋 인스턴스에서 호출해야 한다.
