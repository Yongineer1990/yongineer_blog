---
title: TIL.41) [DRF] Tutorial Part6. ViewSets & Routers
date: 2020-08-31 00:08:38
category: Django
thumbnail: 'https://yongnas.iptime.org/django/drf.png'
draft: false
---

![](https://yongnas.iptime.org/django/drf.png)

해당 내용은 Django REST Framework의 공식문서를 참고하여 작성되었으며. 해당 공식문서는 [여기](https://www.django-rest-framework.org/tutorial/6-viewsets-and-routers/)에서 확인 가능하다.

Django REST Framework에서는 개발자가 API의 상태 및 상호 작용을 모델링하는데 더 집중하고 공통 규칙에 따라 URL구성이 자동으로 처리되도록 하는 `ViewSet`을 처리하기 위한 추상화클래스가 제공된다.

`ViewSet`클래스는 **`GET`또는 `PUT`과 같은 메서드 처리가 아니라 읽기 또는 업데이트와 같은 작업을 제공한다는 점**을 제외하면 `View`클래스와 거의 동일하다.

`ViewSet`클래스는 일반적으로 `URL conf`에 복잡하게 정의된 메서드를 처리하는 `Router`클래스를 사용하여 `ViewSet`클래스를 사용하는 View에 인스턴스화 되는 마지막 순간에 메서드 처리기 집합에만 바인딩된다.

# Refactoring to use ViewSets

이제 현재 작성한 `snippets/views.py`를 `ViewSet`으로 리팩토링 해보자.

먼저 `rest_framework`의 `ViewSets` 모듈을 import해준다. 그 후 `UserList`와 `UserDetail`View를 단일 `UserViewSet`으로 변경하기 위해 기존에 작성했던 두 View를 제거한다.

```python
from rest_framework import viewsets

class UserViewSet(viewsets.ReadOnlyModelViewSet):
    """
		ReadOnlyModelViewSet은 자동으로 'list' 및 'detail' 작업을 제공한다.
    """
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

여기서는 기본적인 Read only작업을 자동으로 제공하기 위해  `viewsets`의 `ReadOnlyModelViewSet`을 사용하였다. 그리고 일반 View를 사용할 때와 똑같이 `queryset`과 `serailzer_class`속성을 설정한다. 다만, 기존과 차이점은 더 이상 `UserList`와 `UserDetail`이라는 두개의 개별 클래스 View에 동일한 정보를 제공할 필요가 없다는 것이다.

다음으로 `SnippetList`, `SnippetDetail`, `SnippetHighlight` View클래스를 제거하고 `ViewSet`클래스로 바꿔주겠다.

```python
from rest_framework.decorators import action
from rest_framework.response import Response

class SnippetViewSet(viewsets.ModelViewSet):
    """
    ModelViewSet은 자동으로 `list', 'create', 'retrieve',
    'update', 'destroy' 작업을 처리해주며

    또한 'highlight' 액션도 제공한다.
    """
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly,
                          IsOwnerOrReadOnly]

    @action(detail=True, renderer_classes=[renderers.StaticHTMLRenderer])
    def highlight(self, request, *args, **kwargs):
        snippet = self.get_object()
        return Response(snippet.highlighted)

    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)
```

이번에는 기본 읽기 및 쓰기 작업을 자동으로 제공하기 위해 `viewsets`의 `ModelViewSet`클래스를 사용하였다.

또한 `@action`Decorator를 사용하여 `highlight`라는 사용자 지정 작업을 생성하였고 이 데코레이터는 **일반적인 `create`, `update`, `delete`처리와 맞지 않는 사용자 생성 Endpoint를 추가하는데 사용**할 수 있다.

`**@action`Decorator는 기본적으로 `GET`요청에 응답**한다. `POST`요청에 응답하는 작업을 원한다면 `methods`인수를 사용한다.

`@acition`Decorator의 URL은 메서드 이름을 사용한다. (위 예제의 URL은 highlight이다.) 만약 url의 구성을 변경하려면 `url_path`를 데코레이터 키워드 인수로 지정해줘야 한다,

> ⚠️ 만약 `@aciton`에서 `POST`요청에 응답하면서 사용자 지정 URL을 사용할 경우
>```python
># url : /movies/{pk}/release-today 개봉일을 오늘 날짜로 바꿔줌
>   @action(methods=['post'], detail=True, url_path='release-today', url_name='release_today')
>```

전체적인 `snippets/views.py`의 모습은 다음과 같다.,

```python
# snippets/views.py

from django.contrib.auth.models import User
from rest_framework import (
    generics,
    permissions,
    renderers,
    viewsets
)
from rest_framework.decorators import (
    api_view,
    action
)
from rest_framework.response import Response
from rest_framework.reverse import reverse
from .models import Snippet
from .serializers import (
    SnippetSerializer,
    UserSerializer
)
from .permissions import IsOwnerOrReadOnly

@api_view(['GET'])
def api_root(request, format=None):
    return Response({
        'users' : reverse('user-list', request=request, format=format),
        'snippets' : reverse('snippet-list', request=request, format=format)
    })

class SnippetViewSet(viewsets.ModelViewSet):
    """
    ModelViewSet은 자동으로 `list', 'create', 'retrieve',
    'update', 'destroy' 작업을 처리해주며

    또한 'highlight' 액션도 제공한다.
    """
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly,
                          IsOwnerOrReadOnly]

    @action(detail=True, renderer_classes=[renderers.StaticHTMLRenderer])
    def highlight(self, request, *args, **kwargs):
        snippet = self.get_object()
        return Response(snippet.highlighted)

    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class UserViewSet(viewsets.ReadOnlyModelViewSet):
		"""    
		ReadOnlyModelViewSet은 자동으로 'list' 및 
		'detail' 작업을 제공한다.
		"""    
		queryset = User.objects.all()
    serializer_class = UserSerializer
```

# Binding ViewSets to URLs explicitly

핸들러 메서드는 `URLConf`를 정의 할 때만 액션에 바인딩된다. 내부에서 무슨일이 일어나고 있는지 보기 위해 먼저 `ViewSets`에서 View 집합을 명시적으로 생성해 보자.

```python
# snippets/urls.py

from django.urls import path
from rest_framework import renderers
from rest_framework.urlpatterns import format_suffix_patterns
from .views import (
    api_root,
    SnippetViewSet,
    UserViewSet
)

snippet_list = SnippetViewSet.as_view({
    'get' : 'list',
    'post' : 'create'
})
snippet_detail = SnippetViewSet.as_view({
    'get' : 'retrieve',
    'put' : 'update',
    'patch' : 'partial_update',
    'delete' : 'destroy'
})
snippet_highlight = SnippetViewSet.as_view({
    'get' : 'highlight'
}, renderer_classes=[renderers.StaticHTMLRenderer])
user_list = UserViewSet.as_view({
    'get' : 'list'
})
user_detail = UserViewSet.as_view({
    'get' : 'retrieve'
})
```

`HTTP Method`를 각 View에 필요한 작업에 바인딩 하여 각 `ViewSet`클래스에서 여러 View를 만들 수 있다.

이제 리소스를 구체적인 View에 바인딩 했으므로 평소와 같이 `URL Conf`로 View를 등록할 수 있다.

```python
# snippets/urls.py
...
urlpatterns = [
    path('', api_root),
    path('/snippets', snippet_list, name='snippet-list'),
    path('/snippets/<int:pk>', snippet_detail, name='snippet-detail'),
    path('/snippets/<int:pk>/highlight', snippet_highlight, name='snippet-highlight'),
    path('/snippets/users', user_list, name='user-list'),
    path('/snippets/users/<int:pk>', user_detail, name='user-detail'),
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

전체적인 `snippets/urls.py`는 다음과 같다.

```python
# snippets/urls.py

from django.urls import path
from rest_framework import renderers
from rest_framework.urlpatterns import format_suffix_patterns
from .views import (
    api_root,
    SnippetViewSet,
    UserViewSet
)

snippet_list = SnippetViewSet.as_view({
    'get' : 'list',
    'post' : 'create'
})
snippet_detail = SnippetViewSet.as_view({
    'get' : 'retrieve',
    'put' : 'update',
    'patch' : 'partial_update',
    'delete' : 'destroy'
})
snippet_highlight = SnippetViewSet.as_view({
    'get' : 'highlight'
}, renderer_classes=[renderers.StaticHTMLRenderer])
user_list = UserViewSet.as_view({
    'get' : 'list'
})
user_detail = UserViewSet.as_view({
    'get' : 'retrieve'
})

urlpatterns = [
    path('', api_root),
    path('/snippets', snippet_list, name='snippet-list'),
    path('/snippets/<int:pk>', snippet_detail, name='snippet-detail'),
    path('/snippets/<int:pk>/highlight', snippet_highlight, name='snippet-highlight'),
    path('/snippets/users', user_list, name='user-list'),
    path('/snippets/users/<int:pk>', user_detail, name='user-detail'),
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

# Using Routers

`View`클래스가 아닌 `ViewSet`클래스를 사용하기 때문에 앞서 생성했던 `URL conf`와 같이 직접 생성 할 필요 없이 `Router`클래스를 사용하여 `ViewSet`을 등록해주면 나머지는 Django REST Framework가 자동으로 URL 연결 규칙을 처리해준다.

이제 다시 `snippets/urls.py`를 `Router`클래스를 활용하여 변경해 주자.

```python
# snippets/urls.py

from django.urls import (
    path,
    include
)
from rest_framework.routers import DefaultRouter
from .views import (
    SnippetViewSet,
    UserViewSet
)

# router를 만들고 viewset을 등록한다.
router = DefaultRouter()
router.register(r'snippets', SnippetViewSet)
router.register(r'users', UserViewSet)

# API URL은 router에 의해 자동으로 결정된다.
urlpatterns = [
    path('', include(router.urls))
]
```

Router에 Viewset을 등록(registering)하는 것은 `urlpattern`을 제공하는 것과 유사하다. 이때 `router.register`는 URL주소와 `ViewSet`클래스를 인수로 받는다.

또한 `DefaultRouter`를 사용함으로써 API Root View를 생성했으므로 이제 `snippets/views.py`에서 FBV로 작성한 `api_root`는 삭제해도 무방하다.

```python
# snippets/views.py

from django.contrib.auth.models import User
from rest_framework import (
    generics,
    permissions,
    renderers,
    viewsets
)
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.reverse import reverse
from .models import Snippet
from .serializers import (
    SnippetSerializer,
    UserSerializer
)
from .permissions import IsOwnerOrReadOnly

class SnippetViewSet(viewsets.ModelViewSet):
    """
    ModelViewSet은 자동으로 `list', 'create', 'retrieve',
    'update', 'destroy' 작업을 처리해주며

    또한 'highlight' 액션도 제공한다.

    """
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly,
                          IsOwnerOrReadOnly]

    @action(detail=True, renderer_classes=[renderers.StaticHTMLRenderer])
    def highlight(self, request, *args, **kwargs):
        snippet = self.get_object()
        return Response(snippet.highlighted)

    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class UserViewSet(viewsets.ReadOnlyModelViewSet):
    # ReadOnlyModelViewSet은 자동으로 'list' 및 'detail' 작업을 제공한다.
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

# Trade-offs between views vs viewsets

`ViewSet`클래스를 사용하는 것은 유용한 추상화가 될 수 있다. 이는 URL규칙이 API전체에게 일관될 수 있게 하는데 도움이 되며 작성해야하는 코드의 양을 최소화 하며 `URL conf`의 세부 사항이 아닌 API가 제공하는 동작 및 표현에 집중할 수 있게 도와준다.

다만, 그렇다고 해서 항상 올바른 접근 방식을 취하는 것은 아니며 FBV대신 CBV를 사용할 때 고려할 유사한 단점이 있다, **`ViewSet`클래스를 사용하는 것은 View를 개별적으로 구축하는 것 보다 덜 명확하다.**
