---
title: TIL.37) [DRF] Tutorial Part2. Requests and Responses
date: 2020-08-26 01:08:53
category: Django
thumbnail: 'https://yongnas.iptime.org/django/drf.png'
draft: false
---

![](https://yongnas.iptime.org/django/drf.png)

해당 내용은 Django REST Framework의 공식문서를 참고하여 작성되었으며. 해당 공식문서는 [여기](https://www.django-rest-framework.org/tutorial/2-requests-and-responses/)에서 확인 가능하다.

# Request objects

Django REST Framework에서는 기존 `HttpRequest`의 확장 오프젝트인 `Request`를 제공하며 `Request`는 `HttpRequest`보다 유연하게 Requet 구문을 분석한다. 

`Request`의 핵심 속성은 `request.POST`와 Web API들과 좀 더 유용하게 동작하는 `request.data`이다.

```python
request.POST  # 양식이 있는 데이터 (form data)만 처리한다. 'POST' Method에서만 작동한다.
request.data  # 임의의 데이터를 처리한다. 'POST', 'PUT', 'PATCH' Method에서 작동한다.
```

# Response objects

Django REST Framework는 렌더링 되지 않은 컨텐츠를 가져와 클라이언트에 반환할 올바른 컨텐츠 형식으로 변환하는 기능을 제공하는 `TemplateResponse`인 `Response`가 있다.

```python
return Response(data)  # 클라이언트가 요청한 컨텐츠 유형으로 렌더링 해준다.
```

# Status codes

view에서 HTTP Status code를 사용한다고 해서 무조건 읽을 수 있는 것은 아니며 잘못된 Status code를 사용한다면 차라리 사용하지 않는 것이 더 낫다. 

Django REST Framework의 `status`모듈은 `HTTP_400_BAD_REQUEST`와 같은 기존보다 명확한 식별자를 제공한다.

`status`모듈을 사용하기 위해서는 다음과 같이 모듈을 import 해야 한다.

```python
from rest_framework import status
```

# Wrapping API views

API view를 만들기 위해 Django REST Framework에서는 두가지의 wrapper를 제공한다.

1. 함수기반 view를 위해 Decorator 형태로 제공되는 `@api_view`
2. 클래스기반 view를 위해 제공되는 `APIView`

wrapper는 뷰에서 `Request`인스턴스를 수신하고 `Response`객체에 컨텍스트를 추가하여 컨텐츠 협상을 할 수 있도록 몇가지 기능을 제공한다.

wrapper는 상황에 맞춰 `405 Method Not Allowed`와 같은 메시지를 반환해주고 손상된 입력으로 `request.data`에 접근했을 때 `ParseError`를 반환해준다.

# Pulling it all together

이제 새로운 요소들을 앞서 작성해둔 `snippets/views.py`에 적용해보자.

리팩토링이 필요한 부분은 다음과 같다.

1. `Response` 오브젝트로 인해 더 이상 `JSONResponse`와 `HttpResponse`는 필요없으므로 삭제
2. `request` 오프젝트가 입력된 데이터를 유연하게 받아들이므로 이제 데이터를 `JSON`형태로 변환하는 `JSONParser`는 사용하지 않을 것이다.
3. 함수형 view에 `@api_view` wrapper를 적용한다.

```python
# snippets/views.py

from django.http import (
    HttpResponse,
    JsonResponse
)
from django.views.decorators.csrf import csrf_exempt
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer

@api_view(['GET','POST'])
def snippet_list(request, format=None):
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = SnippetSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

@api_view(['GET', 'PUT', 'DELETE'])
def snippet_detail(request, id, format=None):
    try:
        snippet = Snippet.objects.get(id=id)

    except Snippet.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)

    if request.method == 'GET':
        serializer = SnippetSerializer(snippet)
        return Response(serializer.data)

    elif request.method == 'PUT':
        serializer = SnippetSerializer(snippet, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':
        snippet.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

더 이상 request나 response들을 특정 컨텐츠 타입으로 분류하지 않아도 되며, `**request.data`가 입력된 `json request`를 제어할 수 있고 필요하다면 다른 형식으로도 처리 할 수 있다.**

`**Response`는 `serializer.data`를 반환하지만 `Response` 오브젝트가 응답할 올바른 컨텐츠 유형으로 렌더링 해준다.**

또한, **좀 더 명확하게 식별되어 있는 `status code`를 사용한다.**

# Adding optional format suffixes to our URLs

더 이상 하나의 컨텐츠 타입에 종속되지 않게 하기 위해 API Endpoint에 format에 대한 접미사를 지원한다.

format에 대한 접미사를 사용하면 명시적으로 지정된 format이 참조하는 URL이 제공되므로 `http://example.com/api/items/4.json`와 같은 URL을 제어할 수 있다.

이를 위해 view 함수에 다음과 같은 `format`키워드를 인자로 부여한다.

```python
def snippet_list(request, format=None):
```

```python
def snippet_detail(request, id, format=None):
```

이제 `snippets/urls.py`에 `format_suffix_patterns`를 기존 URL에 추가한다.

```python
# snippets/urls.py

from django.urls import path
from rest_framework.urlpatterns import format_suffix_patterns
from .views import snippet_list, snippet_detail

urlpatterns = [
    path('', snippet_list),
    path('/<int:id>', snippet_detail)
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

# How's it looking?

[Tutorial Part 1](https://yongineer.netlify.app/django/til.36\)-[drf]-tutorial-Part1.-serialzation/)에서 했던 거 처럼 httpie 또는 postman을 통해 API에 접근해보자. 여기서는 httpie를 사용하였다.

```bash
http -v GET localhost:8000/api/v1/snippets

GET /api/v1/snippets HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: localhost:8000
User-Agent: HTTPie/2.1.0

HTTP/1.1 200 OK
Allow: POST, OPTIONS, GET
Content-Length: 315
Content-Type: application/json
Date: Tue, 25 Aug 2020 15:58:07 GMT
Referrer-Policy: same-origin
Server: WSGIServer/0.2 CPython/3.8.5
Vary: Accept, Cookie
X-Content-Type-Options: nosniff
X-Frame-Options: DENY

[
    {
        "code": "foo = \"bar\"\n",
        "id": 1,
        "language": "python",
        "linenos": false,
        "style": "friendly",
        "title": ""
    },
    {
        "code": "print \"hello,world\"\n",
        "id": 2,
        "language": "python",
        "linenos": false,
        "style": "friendly",
        "title": ""
    },
    {
        "code": "print \"hello,world\"",
        "id": 3,
        "language": "python",
        "linenos": false,
        "style": "friendly",
        "title": ""
    }
]
```

`Accept` 헤더를 사용해 포맷이 정의된 응답을 제어 할 수도 있다.

```bash
http -v GET localhost:8000/api/v1/snippets Accept:application/json # Request JSON
http -v GET localhost:8000/api/v1/snippets Accept:text/html        # Request HTML
```

또는 format 접미사를 붙여서도 제어 가능하다.

```bash
http -v GET localhost:8000/api/v1/snippets.json    # JSON suffix
http -v GET localhost:8000/api/v1/snippets.api     # Browsable API suffix
```

마찬가지로 `Content-Type` 헤더를 사용하여 POST형식의 요청도 처리 할 수 있다.

```bash
# 양식(form)데이터를 사용한 POST

http --form POST localhost:8000/api/v1/snippets code='print(123)'

HTTP/1.1 201 Created
Allow: POST, OPTIONS, GET
Content-Length: 94
Content-Type: application/json
Date: Tue, 25 Aug 2020 16:00:28 GMT
Referrer-Policy: same-origin
Server: WSGIServer/0.2 CPython/3.8.5
Vary: Accept, Cookie
X-Content-Type-Options: nosniff
X-Frame-Options: DENY

{
    "code": "print(123)",
    "id": 4,
    "language": "python",
    "linenos": false,
    "style": "friendly",
    "title": ""
}
```

```bash
# JSON 데이터를 사용한 POST

http --json POST localhost:8000/api/v1/snippets code="print(456)"

HTTP/1.1 201 Created
Allow: POST, OPTIONS, GET
Content-Length: 94
Content-Type: application/json
Date: Tue, 25 Aug 2020 16:00:56 GMT
Referrer-Policy: same-origin
Server: WSGIServer/0.2 CPython/3.8.5
Vary: Accept, Cookie
X-Content-Type-Options: nosniff
X-Frame-Options: DENY

{
    "code": "print(456)",
    "id": 5,
    "language": "python",
    "linenos": false,
    "style": "friendly",
    "title": ""
}
```
