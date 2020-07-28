---
title: TIL.12) [Django] 시작하기 및 Django의 구조
date: 2020-07-27 17:59:35
category: Django
thumbnail: './images/django.png'
draft: false
---

![](./images/django.png)

# Django 설치

- pip 명령어를 사용하여 설치
- `pip install django`

# Project 생성

- Working directory로 이동 후
- `django-admin startproject <Project name>`

# Application 생성

- Django에서 생성한 Project directory로 이동 후
- `python [manage.py](http://manage.py) startapp <App name>`
    - 💡 만약 에러가 난다면 해당 위치에 `manage.py`가 있는지 확인

# settings.py

- 프로젝트의 전체적인 설정을 담당한다.

## Application 등록

- 모든 Application 은 해당 파일의 `INSTALLED_APPS` 내에 등록되어야 한다.
- Application 등록 시 해당 Application의 설정 클래스를 등록하여야 하며 경로는 다음과 같다.
    - `<Project directory>/<Application directory>/app.py`
- `INSTALLED_APPS`에 등록시 경로는 다음과 같이 한다.
    - `'<Application name>.apps.<App setting class name>'`

## Debug Mode

- 기본적으로 Django는 개발모드 상태로 생성된다.
- 그러나 개발이 완료되고 운영모드에 들어갈때는 `DEBUG`변수의 값을 `False`로 주어 운영모드로 전환시켜야 한다.
- 또한, 운영모드에선 `ALLOWED_HOST`의 값으로 서버의 ip주소를 할당해줘야 한다.
- 개발모드에서 `ALLOWED_HOST`의 값은 아무 값을 할당하지 않아도 기본적으로 `localhost`이다.

# urls.py

- `Client`로 부터 전달받은 `**URL`을 `view`함수와 연결시켜주는 역할**
- 기본적으로 Project에 생성되나 Application내에 생성하고 Project에서 해당 위치로 연결시켜주는것이 관리 측면에서 더 좋다.
- 모든 `URL`은 `urlpatterns`내 `path`함수로 정의 되어야 한다.
- `path( "URL", views.<func name> or <method name>)`

```python
from django.urls import path
from .views  import MainView # views.py의 MainView 클래스를 사용함

urlpatterns = [
    path('', MainView.as_view()) # path 설정
]
```

> 💡**as_view method**
>
`class`형 view에서 `class`로 진입하기 위한 진입 method이다. 또한 호출을 한 `http method`가 `GET`인지 `POST`인지 `DELETE`인지 `PUT`인지 판별해서 그에 맞는 함수를 실행 시켜 준다.

# views.py

- 기본적으로 Application내 위치하고 있다.
- `urls.py` 와 연결된 함수 또는 메서드가 실제 작성되는 파일로 **로직을 담당하는 파일**
- 또한 `Template`에게 `Client`에게 보여질 페이지를 전달하는 역할도 하며 잘못된 `url`로 접근 시 에러 메시지를 반환하는 역할도 한다.
- 함수 또는 메서드의 **이름은 반드시 `urls.py`내 `path`함수에 정의된 이름과 동일하게 작성되어야 한다.**

```python
import json # json 데이터를 처리하기 위해 사용
from django.views import View # django 프레임 워크에 내장된 view 클래스 사용
from django.http  import JsonResponse # Json으로 response 하기 위해 사용

class MainView(View): # django의 내장 view 클래스를 상속받음
    def get(self, request): 
		    return JsonResponse({"Hello":"World"}, status=200)
```

# models.py

- `Database`의 `Table`을 정의하는 파일
- `class`를 이용하여 `Table` 를 생성 하게 된다.

> 💡 **Class를 통한 Table 생성**
>
> - `Table`을 만들기 위해서 `class`는 반드시 `models.Model`을 상속받아야 한다.
```python
class TableName(models.Model)
```
>
>- `Field`의 정의는 `class`내에서 이루어 지며 `Field Name = model.datatype(option)`으로 정의한다.
>
```python
class Users(models.Model):
	# Field Name = models.datatype(option)
  name = models.CharField(max_length = 50)
  email = models.CharField(max_length = 50)
  password = models.CharField(max_length = 300)
  created_at = models.DateTimeField(auto_now_add = True)
  updated_at = models.DateTimeField(auto_now = True)
```

# migrations 디렉토리

- `models.py`에서 정의한 `Table` 구조를 `manage.py`의 `makemigrations` 옵션을 통해 생성되는 파일이 저장되는 디렉토리
