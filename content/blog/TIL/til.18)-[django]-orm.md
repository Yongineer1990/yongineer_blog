---
title: TIL.18) [Django] ORM
date: 2020-07-27 18:55:44
category: TIL 
thumbnail: './images/django.png'
draft: false
---

![](./images/django.png)

# 데이터 가져오기 - all() & get() & filter()

## `all()`

- 테이블의 모든 데이터를 조회한다.
- `<table>.objects.all()`
- 💡SQL : `SELECT * FROM <table>`

> 💡**`all()` + `limit`**
>
>- 전체 데이터 중 원하는 개수 만큼 조회 할 수 있다.
>- 처음 부터 원하는 개수를 조회 할 때 : `<table>.objects.all()[:10]`
>
>    👉 전체 결과에서 처음 부터 10개의 결과만 조회
>- 원하는 위치에서 부터 원하는 개수를 조회 할 때 : `<table>.objects.all()[5:10]`
>
>    👉 전체 결과 중 5번째 부터 10개의 결과만 조회

## `get()`

- 테이블의 데이터 중 **조건에 맞는 하나의 값**을 가져온다.
- `<table>.objects.get(<field> = <value>)`
- 만약 조회된 값이 다수일 경우 에러가 발생하기 되니 주의한다.
    - 이럴 경우 `filter()`를 사용 할 수 있다.

## `filter()`

- 테이블의 데이터 중 **조건에 맞는 모든 값**을 가져 온다.
- `<table>.objects.filter(<field> = <value>)`
- 💡SQL : `SELECT * FROM <table> WHERE <column> = <value>`

> 💡**`filter()` + `exists()`**
>
>- 만약 테이블에 데이터가 있는지 확인하려면 `exists()`을 사용한다.
>- `<table>.objects.filter(<field> = <value>).exists()`
>- 만약 찾고자 하는 데이터가 없다면 에러를 반환한다.

# 데이터 추가하기 - `create()`

## `create()`

- 테이블의 데이터를 추가한다.
- `<table>.objects.create(<field> = <value>)`
    - 또는 다음과 같이 추가 가능하다.
        ```python
        variable = Table(field = value)
        variable.save()
        ```
        
# 데이터 삭제하기 - delete()

## `delete()`

- 테이블에서 데이터를 제거한다.
- 삭제한 데이터를 지정해줘야 한다.
    
    👉 `<table>.objects.get(<field> = <value>).delete()`
- 만약 모든 데이터를 삭제하고 싶다면 다음과 같이 사용한다.
    
    👉 `<table>.objects.all().delete()`
- 또는 다음과 같이 사용한다.
    ```python
    variable = Table.objects.get(field = value)
    variable.delete()
    ```

# 데이터 수정하기 - update()

## `update()`

- 이미 생성된 테이블의 데이터를 수정할때 사용한다.
- `<table>.objects.update(<field> = <value>)`
- 또한 `save()`를 통해서도 수정이 가능하며 다음과 같이 사용 할 수 있다.
    ```python
    # 단일 값을 수정
    var1 = Table.objects.get(field = value)
    var1.field = update_value
    var1.save()
	
    # 모든 값을 수정
    var2 = Table.objects.all()
    for i in var2:
    	var2.field = update_value
    	var2.save()
    		# 각 Model Instance 별로 DB에 수정요청을 하여 성능저하 이슈가 있음
    ```
- 다만, 위의 방법중 특히 모든 값을 수정하는 경우 각 모델 인스턴스별로 DB에 수정요청을 하기때문에 성능저하를 일으킬 수 있으니 모든 값을 수정할때는 다음과 같이 `update()`를 사용한다.
    ```python
    var = Table.objects.all()
    var.update(field = update_value)
    ```
