---
title: Django에서 MongoDB 사용해보기
date: 2021-02-07 22:02:46
category: Django
thumbnail: ../TIL/images/django.png
draft: false
---

![](../TIL/images/django.png)

# Django에서 MongoDB 사용하기

## 요구사항
- django 3.1
- mongoDB 4.4.3
- mongoDB Atlas
- djongo 1.3.3
- pymongo 3.11.2

### pymongo

- `Python`에서 `MongoDB`서버(`mongoDB Atals`포함)연결 및 조작을 지원하는 모듈

### djongo

- `django ORM`을 확장하여 `Python`개체를 `mongoDB`문서에 매핑해주는 모듈
- `pymongo`에 의존성을 갖는다.

## django와 mongoDB 연결

### 다중 데이터 베이스 설정

- 데이터베이스 추가

```python
# settings.py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'db_name',
        'USER': 'user',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '3306',
    },
    'test_mongo': {
        'ENGINE': 'djongo',
        'NAME': 'db_name',
        'CLIENT': {
            'host': 'localhost',
            'port': 27017,
            'username': 'user',
            'password': 'password',
            'authSource': 'admin',
            'authMechanism': 'SCRAM-SHA-1'
        }
    }
}
```

- 데이터베이스 라우팅 (`router.py`)

```python
# router.py

class Router:
    def db_for_read(self, model, **hints):
        if model._meta.app_label == 'app_name':
            return 'app_name'
        return None

    def db_for_write(self, model, **hints):
        if model._meta.app_label == 'app_name':
            return 'app_name'
        return None

    def allow_relation(self, obj1, obj2, **hints):
        if obj1._meta.app_label == 'app_name' or obj2._meta.app_label == 'app_name':
            return True
        return None

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        if app_label == 'app_name':
            return db == 'app_name'
        return None
```

```python
# settings.py

DATABASES_ROUTERS = ['project_name.router.Router']
```

### model 작성

```python
# app/models.py

from djongo import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
		...
```

- `django`의 models class가 아닌 `djongo`의 models class를 이용하여 작성

# Django에서 mongoDB 사용시 주의사항

## DB서버

- Amazon Web Service의 RDS를 이용할 수 없다. (AWS DocumentDB 사용해야 함)
- 따라서, `mongoDB Atlas`를 이용해야하며 비용이 발생한다. (free tier 존재)

## ORM

- `Django ORM`을 지원해주는 `djongo`모듈이 있지만 온전히 지원되지 않는다.

    다음은 `boolean` 타입의 데이터를 검색할때 `djongo ORM`, `mongoDB CLI`, `pymongo`의 결과 비교이다.

    - `djongo ORM`

        ![https://yongineer.duckdns.org/django/01.png](https://yongineer.duckdns.org/django/01.png)

    - `mongoDB CLI`

        ![https://yongineer.duckdns.org/django/02.png](https://yongineer.duckdns.org/django/02.png)

    - `pymongo`

        ![https://yongineer.duckdns.org/django/03.png](https://yongineer.duckdns.org/django/03.png)
