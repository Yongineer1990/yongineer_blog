---
title: TIL.13) [Django] Tutorial, Part.1
date: 2020-07-27 18:07:88
category: Django
thumbnail: './images/django.png'
draft: false
---

![](./images/django.png)

이 튜토리얼은 간단한 설문조사(Polls) 어플리케이션을 만드는 과정이며 다음과 같은 파트로 구성되어 있다.

- 설문을 보고 투표를 하는 사용자 사이트
- 관리자가 설문을 추가, 변경, 삭제할 수 있는 관리용 사이트

# 프로젝트 만들기

Django에서는 프로젝트를 자동으로 생성해주며 데이터베이스 설정, Django를 위한 옵션들, 어플리케이션을 위한 설정 등이 자동으로 생성된다.

터미널에서 아래의 코드를 입력한다.

```python
$ django-admin startproject mysite
```

현재 디렉토리에 `mysite`라는 디렉토리가 생성된 것을 확인한다. 그럼 Django가 무엇을 생성 했는지 확인 해보자.

```python
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

1. **바깥`mysite/`**
    - 프로젝트의 Root directory
    - 이름은 사용자가 원하는대로 지정 가능하다.
    - 처음 프로젝트 생성 시 프로젝트 이름과 동일하여 이름을 변경하여 착오를 방지 하는게 좋다.
2. **`manage.py`**
    - Django 프로젝트와 다양한 방법으로 상호작용하는 유틸리티이다.
3. **`mysite/`**
    - 프로젝트를 위한 실제 Python 패키지들이 저장된다.
    - 이 디렉토리의 이름을 이용하여 (`mysite.urls`와 같이) 프로젝트 어디서나 Python 패키지들을 Import 할 수 있다.
4. **`mysite/__init__.py`**
    - Python에게 이 디렉토리를 패키지 처럼 다루라고 알려주는 용도의 파일
5. **`mysite/setting.py`**
    - 현재 Django의 프로젝트 환경 및 구성을 저장한다.
6. **`mysite/urls.py`**
    - `URL` 선언을 저장한다.
    - Django로 작성된 사이트의 목차와 같다.
7. **`mysite/asgi.py`**
    - 비 동기식 웹 소켓 요청을 처리하는 거라는데 잘 모르겠다.
8. **`mysite/wsgi.py`**
    - 이것도 비 동기식 웹 소켓 요청을 처리하는거 라는데 역시나 잘 모르겠다.

# 개발 서버

개발 서버를 시작하기 위해 `mysite` 디렉토리로 이동 후 다음 명령어를 실행한다.

```python
$ python manage.py runserver
```

터미널에 다음과 같은 메시지가 출력된다.

```markdown
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

6월 08, 2020 - 15:50:53
Django version 3.0, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

이제 브라우저에서 [`localhost:8000`](http://localhost:8000) 또는 [`http://127.0.0.1:8000/`](http://127.0.0.1:8000/)로 접속하여 개발 서버를 확인해 보자.
![](https://images.velog.io/images/yongineer1990/post/7c14ed83-8b9e-42f1-a22f-c4a161d3315a/image.png)

위와 같은 화면이 나온다면 성공.

> 💡**Port 변경하기**
> 
>기본적으로 포트는 8000번을 사용하지만 사용자가 특정 포트를 사용하고 싶을 때는 서버 시작 시 사용할 포트를 인수로 전달 해주면 된다.
```python
$ python manage.py runserver 8080
```

# 설문조사 앱 만들기

앱을 생성하기 위해서는 `manage.py`가 존재하는 디렉토리에서 다음과 같은 명령어를 사용한다.

```python
$ python manage.py startapp polls
```

그럼 이제 Django가 생성한 polls 어플리케이션의 구조를 살펴 보자

```python
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

# 첫 번째 뷰 작성하기

`polls/views.py` 파일을 열어 다음과 같은 함수를 작성한다.

```python
# polls/views.py
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

이 `view`의 `index`함수는 `Clinet`로 부터 `Request`를 받으면 "Hello, world. You're at the polls index." 라는 문구를 다시 `Response` 해주는 함수이다.

문제는 이 `index`함수를 호출 하려면 이와 연결된 `URL`이 있어야 하는데 이를 위해 `URLconf`가 사용된다.

Django에서는 이 `URLconf`를 생성 하려면 어플리케이션 디렉토리에 `urls.py`을 생성해야 한다.

```python
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py ---------------> # polls의 URLconf
    views.py
```

이 `polls/urls.py`에는 다음과 같은 코드를 작성한다.

```python
# polls/urls.py

from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

다음은 위 `polls/urls.py`를 프로젝트의 `URLconf`가 바라볼수 있게 `mysite/urls.py`파일에 다음과 같이 코드를 작성한다.

```python
# mysite/urls.py

from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

먼저 `include()`함수를 사용하기 위해 `django.urls`에서 `include`모듈을 import해준다.

그 후 `urlpatterns` 안에 `path`로 정의 되어야 한다.

> 💡**include( )**
>
다른 `URLconf`들을 참조할 수 있도록 해주는 함수이다. `include()`함수는 `URL`의 해당하는 시점까지 잘라내고 남은 문자열 부분을 후속 처리를 위해 `include`된 `URLconf`로 전달한다.
>
예를 들어 `[localhost:8000/polls/vote/result](http://localhost:8000/polls/vote/result)` 이렇게 `URL`이 전달된다면 `include()`함수는 `[localhost:8000/polls](http://localhost:8000/polls까지만)`까지만 자르고 나머지 부분 `vote/result`은 `include()`함수의 인자로 전달된 어플리케이션의 `URLconf`, 위 코드에서는 `'polls.urls'`에게 전달한다. 따라서 나머지 `URL`의 처리를 해당 어플리케이션의 `URLconf`에서 이어서 처리할 수 있다.

> 💡**path( )**
>
`path()`함수에는 2개의 필수 인수인 `route`와 `view` 2개의 선택 인수인 `kwargs`와  `name`이 있다.

1. **route**
    - `URL`패턴을 가진 문자열
    - Django는 `urlpatterns`의 첫번째 패턴부터 시작하여 일치하는 패턴을 찾는다.
    - 도메인 이름은 검색하지 않으므로 인수로 전달하지 않는다
       
       👉 [`http://localhost:8000/polls/`](http://localhost:8000/polls/)이 요청된 경우 `URLconf`는 오직 `polls/`부분만 바라보기 때문에 도메인 이름은 인수로 전달하지 않는다.

2. **view**
    - Django에서 일치하는 `urlpatterns`를 찾으면 `views.py`의 함수를 호출한다.

3. **kwargs**
    - `views.py`에 사전형으로 전달되는 인수이다.
    - 이 튜토리얼에서는 사용하지 않는다.

4. **name**
    - `URL`에 이름을 짓는다.
    - 이름을 지음으로 써 Django내에서 어디서나 명확하게 참조가 가능하다.

이제 `index`함수가 `URLconf`와 연결되었다 잘 작동하는지 확인하기 위해 서버를 실행한다.

> 💡runserver의 자동 변경 기능
>
기존 코드에 수정사항을 반영하기 위해서 굳이 서버를 재기동하지 않아도 된다. 왜냐하면 `runserver`는 요청이 들어올때 마다 (저장 할 때마다) 자동으로 다시 서버를 재기동한다. 그러나 파일을 추가하는 등 몇몇의 동작은 자동으로 인식하지 못하기 때문에 이런 상황에서는 재기동 해줘야 한다.
