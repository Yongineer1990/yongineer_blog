---
title: TIL.38) [DRF] Tutorial Part3. Class-based Views
date: 2020-08-27 02:08:83
category: Django
thumbnail: 'https://yongineer.duckdns.org/django/drf.png'
draft: false
---

![](https://yongineer.duckdns.org/django/drf.png)

해당 내용은 Django REST Framework의 공식문서를 참고하여 작성되었으며. 해당 공식문서는 [여기](https://www.django-rest-framework.org/tutorial/3-class-based-views/)에서 확인 가능하다.

FBV가 아닌 CBV를 이용해 API Views를 작성할 수 있다. 공통적인 함수를 재사용 할 수 있고, DRY를 유지하는데 도움이 된다.

> ⚠️ FBV? CBV?
>
>- FBV : 함수 기반 Views, Function Based Views
>- CBV : 클래스 기반 Views, Class Based Views

> ⚠️ DRY?
>
Don't repeat yourself의 약어로 모든 형태의 정보 중복을 지양하는 원리이다. 프로그래밍에 있어 중복을 지양한다는 것은 소스코드의 중복을 의미한다.

# Rewriting our API using class-based views

앞서 작성한 `snippets/views.py`는 FBV로 구현하였다 이를 다시 CBV로 리팩토링 하겠다.

```python
# snippets/views.py

from django.http import Http404
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer

class SnippetList(APIView):

    def get(self, request, format=None):
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)

        return Response(serializer.data)

    def post(self, request, format=None):
        serializer = SnippetSerializer(data=request.data)

        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

class SnippetDetail(APIView):

    def get_object(self, id):
        try:
            return Snippet.objects.get(id=id)
        except Snippet.DoesNotExist:
            raise Http404

    def get(self, request, id, format=None):
        snippet = self.get_object(id)
        serializer = SnippetSerializer(snippet)
        return Response(serializer.data)

    def put(self, request, id, format=None):
        snippet = self.get_object(id)
        serializer = SnippetSerializer(snippet, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def delete(self, request, id, format=None):
        snippet = self.get_object(id)
        snippet.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

전체적인 코드가 FBV와 다르지 않다. 다만 Decorator로 `@api_view`를 사용하여 `HTTP Method`에 맞는 동작을 수행했던 이전 코드와 달리 `APIView`클래스를 상속받아 `HTTP Method`명과 동일하게 사용하고 있는 `Method`에 해당 동작을 처리하여 코드가 좀 더 간결하고 명확해졌다. 하지만 사실 이것은 `Django`의 `django.views`모듈에서 제공하는 `View`클래스와 크게 다르지 않다.

이제 CBV에 맞게 `snippets/urls.py`도 수정해야 한다.

```python
# snippets/urls.py

from django.urls import path
from rest_framework.urlpatterns import format_suffix_patterns
from .views import (
    SnippetList,
    SnippetDetail
)

urlpatterns = [
    path('', SnippetList.as_view()),
    path('/<int:id>', SnippetDetail.as_view())
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

CBV에서는 API URL에 접근했을때 `APIView`클래스의 클래스 메서드인 `as_view()`를 호출한다.

이 `as_view()`는 다시 `dispatch()`메서드를 호출하는데 이 `dispatch()`메서드가 요청을 분석하여 어떤 `HTTP Method`요청인지 확인한다. 만약 `GET`요청이라면 앞서 생성한 CBV패턴에서 `get`메서드로 해당 요청을 처리할 수 있게 연결해준다.

# Using mixins

CBV 패턴의 장점 중 하나는 재사용이 가능한 동작의 조합을 쉽게 구성할 수 있다는 것이다.

지금까지 사용했던 `CRUD`작업은 모델 기반 API Views와 유사하다. 이런 일반적인 동작은 `Django REST Framework`의 `mixins`클래스에 `CRUD`와 같은 공통적인 함수가 구현되어 있다.

이제 CBV로 리팩토링한 `snippets/views.py`를 `mixin`클래스를 사용하여 리팩토링 하겠다.

```python
# snippets/views.py

from rest_framework import mixins
from rest_framework import generics
from .models import Snippet
from .serializers import SnippetSerializer

class SnippetList(
    mixins.ListModelMixin,
    mixins.CreateModelMixin,
    generics.GenericAPIView):

    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer

    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)

class SnippetDetail(
    mixins.RetrieveModelMixin,
    mixins.UpdateModelMixin,
    mixins.DestroyModelMixin,
    generics.GenericAPIView):

    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer

    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

리팩토링 한 `snippets/views.py`를 보면 모든 snippet 리스트를 조회, 생성하는 `SnippetList`클래스에는 `ListModelMixin`과 `CreateModelMixin`을, 각 snippet을 조회, 수정, 삭제하는 `SnippetDetail`클래스에는 `RetrieveModelMixin`, `UpdateModelMixin`, `DestroyModelMixin`을 상속받았고 공통적으로 `generics`모듈의 `GenericAPIView`를 상속받았다.

`GenericAPIView`클래스는 핵심 함수를 제공하고 각 `mixin`은 `.create()`, `.retrieve()`, `.update()`, `.destroy()`등의 기능을 제공한다.

# Using generic class-based views

이미 `mixins`클래스를 사용하여 이전보다 적은 코드를 사용하면서 같은 동작을 수행하는 view를 작성했지만 여기서 한단계 더 나아가 `views.py`의 코드를 훨씬 더 줄이는데 사용할 수 있는 `mixin`을 포함한 `view set`을 제공한다.

다시 `snippets/views.py`를 열어 리팩토링 해보자.

```python
# snippets/views.py

from rest_framework import generics
from .models import Snippet
from .serializers import SnippetSerializer

class SnippetList(generics.ListCreateAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer

class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
```

`Django REST Framework`의 `generics`모듈로 부터 `Generic view`를 상속받아 사용하는데, 사용하려는 기능에 따라 필요한 `Generic View`를 선택하여 상속한다.

예를 들어, `GET`와 `POST` 메소드의 기능이 필요하다면 `ListCreateAPIView`를 상속받아 사용할 수 있고, `GET`, `PUT(PATCH)`, `DELETE`의 기능이 필요하다면 `RetrieveUpdateDestroyAPIView`를 상속받아 사용하면 된다.

보다 자세한 `Generic View`는 [여기](https://www.django-rest-framework.org/api-guide/generic-views/#concrete-view-classes)를 참고하자.
