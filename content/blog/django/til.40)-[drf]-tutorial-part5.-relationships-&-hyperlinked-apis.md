---
title: TIL.40) [DRF] Tutorial Part5. Relationships & Hyperlinked APIs
date: 2020-08-27 22:08:71
category: Django
thumbnail: 'https://yongineer.duckdns.org/django/drf.png'
draft: false
---

![](https://yongineer.duckdns.org/django/drf.png)

해당 내용은 Django REST Framework의 공식문서를 참고하여 작성되었으며. 해당 공식문서는 [여기](https://www.django-rest-framework.org/tutorial/5-relationships-and-hyperlinked-apis/)에서 확인 가능하다.

현재까지 작성한 API의 관계는 `Primary Key`를 사용하여 표시하였다. 이제 Django REST Framework에서 제공하는 `API Hyperlink`기능을 사용하여 관계를 표현하고 API의 응집력과 검색가능성을 개선하겠다.

# Creating an endpoint for the root of our API

지금까지는 Snippet과 User에 대한 Endpoint는 있었지만 Root 진입에 대한 Endpoint는 없었다. 먼저 Root 진입에 대한 Endpoint를 `snippets/views.py`에 FBV 패턴으로 추가할 것이며 이를 위해 `@api_view`Decorator를 사용할 것이다.

```python
# snippets/views.py

from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework.reverse import reverse

@api_view(['GET'])
def api_root(request, format=None):
    return Response({
        'users': reverse('user-list', request=request, format=format),
        'snippets': reverse('snippet-list', request=request, format=format)
    })
```

전체적인 `snippets/views.py`는 다음과 같다.

```python
# snippets/views.py

from django.contrib.auth.models import User
from rest_framework import (
    generics,
    permissions,
    renderers
)
from rest_framework.decorators import api_view
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

class SnippetList(generics.ListCreateAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly, IsOwnerOrReadOnly]

class UserList(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer

class UserDetail(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

여기서 확인해야 할 사항이 두가지 있다.

1. 정규화된 URL을 반환하기 위해 Django REST Framework에서 제공하는 `reverse`기능을 사용한다.
2. URL패턴은 나중에 `snippets/urls.py`에서 선언할 `name`으로 식별한다.

# Creating an endpoint for the highlighted snippets

지금까지 작성한 API에서 아직 구현이 안된 부분은 Code highlight기능이다.

다른 API Endpoint와 달리 `JSON`을 사용하지 않고 `HTML`표현을 사용한다. Django REST Framework에서 제공되는 `HTML Renderer`에는 두가지 스타일이 있다.

1. 템플렛을 사용하여 렌더링된 `HTML`을 처리
2. 미리 렌더링된 `HTML`을 처리

여기는 두번째 스타일을 사용 할 것이다.

Code Highlight View를 생성 할 때 고려해야할 또 다른 사항은 사용할 수 있는 기존의 구체적인 `Generic View`가 없다는 것이다. 왜냐하면 객체 인스턴스를 반환하지 않고 객체 인스턴스의 속성을 반환하기 때문이다.

`snippets/views.py`에 구체적인 `Generic View`를 사용하는 대신 인스턴스를 나타내는 `CBV`패턴을 사용하고 자체적인 `GET`동작을 구현하기 위해 `.get()`메서드를 생성한다.

```python
# snippets/views.py

from rest_framework import renderers
from rest_framework.response import Response

class SnippetHighlight(generics.GenericAPIView):
    queryset = Snippet.objects.all()
    renderer_classes = [renderers.StaticHTMLRenderer]

    def get(self, request, *args, **kwargs):
        snippet = self.get_object()
        return Response(snippet.highlighted)
```

전체적인 `snippets/views.py`의 모습은 다음과 같다

```python
# snippets/views.py

from django.contrib.auth.models import User
from rest_framework import (
    generics,
    permissions,
    renderers
)
from rest_framework.decorators import api_view
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

class SnippetList(generics.ListCreateAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly, IsOwnerOrReadOnly]

class UserList(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer

class UserDetail(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer

class SnippetHighlight(generics.GenericAPIView):
    queryset = Snippet.objects.all()
    renderer_classes = [renderers.StaticHTMLRenderer]

    def get(self, request, *args, **kwargs):
        snippet = self.get_object()
        return Response(snippet.highlighted)
```

이제 API Root에 대한 URL과 Code Highlight에 대한 URL을 `snippets/urls.py`에 추가해준다.

```python
# snippets/urls.py

...
urlpatterns = [
    path('', api_root),
    path('/snippets', SnippetList.as_view()),
    path('/snippets/<int:pk>', SnippetDetail.as_view()),
    path('/snippets/<int:pk>/highlight', SnippetHighlight.as_view()),
    path('/snippets/users', UserList.as_view()),
    path('/snippets/users/<int:pk>', UserDetail.as_view()),
]
...
```

# Hyperlinking our API

`Entity` 간의 관계를 다루는 것은 WEB API 디자인에서 가장 어려운 측면 중 하나이다. 관계를 표현하기 위해 선택 할 수 있는 여러 옵션은 다음과 같다.

- `Primary Key`사용
- `Entity` 간 하이퍼링크 사용
- 관련 `Entity`에서 고유한 식별 슬러그 필드 사용
- 관려 `Entity`의 기본 문자열 표현을 사용
- 상위 표현 내 관련 `Entity` 중첩
- 다른 사용자 지정 표현

Django REST Framework는 이러한 모든 옵션을 지원하고 정참조 또는 역참조 관계에 적용하거나 Generic foreign key와 같은 사용자 지정 관리자에 적용 할 수 있다.

여기서는 `Entity`간에 `Hyperlinked style`을 사용 할 것이다. 이를 위해 `snippets/serializers.py`에 기존 `ModelSerializer`를 `HyperlinkedModelSerializer`로 변경하면서 이와 관련된 수정도 함께 진행 할 것이다.

> ⚠️ `HyperlinkedModelSerializer` 와 `ModelSerializer`의 차이점
>
>- 기본적으로 `id`필드는 포함되지 않는다.
>- `HyperlinkedIdentityField`를 사용하는 `url`필드를 포함한다.
>- 관계는 `PrimaryKeyRelatedField`대신 `HyperlinkedRelatedField`를 사용한다.

```python
# snippets/serializers.py

from django.contrib.auth.models import User
from rest_framework import serializers
from .models import (
    Snippet,
    LANGUAGE_CHOICES,
    STYLE_CHOICES
)

class SnippetSerializer(serializers.HyperlinkedModelSerializer):
    owner = serializers.ReadOnlyField(source='owner.username')
    highlight = serializers.HyperlinkedIdentityField(view_name='snippet-highlight', format='html')

    class Meta:
        model = Snippet
        fields = ['url', 'id', 'highlight', 'title',
                  'code', 'linenos', 'language', 'style', 'owner']

class UserSerializer(serializers.HyperlinkedModelSerializer):
    snippets = serializers.HyperlinkedRelatedField(many=True, view_name='snippet-detail', read_only = True)

    class Meta:
        model = User
        fields = ['url', 'id', 'username', 'snippets']
```

`highlight`필드도 추가되었다. 이 필드는 `snippet-detail`이라는 `url`패턴 대신 `snippet-highlight`라는 `url`패턴을 가리키는 점을 제외하고 기본적으로 `url field`와 동일한 유형이다.

`snippets/urls.py`에서 `format_suffix_patterns`를 통해 `.json`과 같은 타입을 포함했기 때문에 `highlight`가 반환하는 타입을 명시적으로 지정해야 한다 따라서 `format`의 인수로 `.html`을 사용하겠다고 명시해야할 필요가 있다.

# Making sure our URL patterns are named

하이퍼링크 된 API를 사용하려면 `urlpatterns`에 URL이름을 지정해야 한다. 먼저 지정해야할 URL패턴들을 살펴보면 다음과 같다.

- Root API는 `user-list`와 `snippet-list`를 참조한다.
- `SnippetSerializer`에는 `snippet-highlight`를 참조하는 `highlight`필드가 있다.
- `UserSerializer`에는 `snippet-detail`을 참조하는 `snippets`필드가 있다.
- `SnippetSerializer`와 `UserSerializer`에는 `{model_name}-detail`을 참조하는 `url`필드가 포함되어 있으며 이 경우 `snippet-detail` 및 `user-detail`이 해당된다.

이제 `snippets/urls.py`에 위와 같은 사항을 참조하여 `name`을 부여하자.

```python
# snippets/urls.py

from django.urls import path
from rest_framework.urlpatterns import format_suffix_patterns
from .views import (
    SnippetList,
    SnippetDetail,
    UserList,
    UserDetail,
    SnippetHighlight,
    api_root
)

urlpatterns = [
    path('', api_root),
    path('/snippets', SnippetList.as_view(), name='snippet-list'),
    path('/snippets/<int:pk>', SnippetDetail.as_view(), name='snippet-detail'),
    path('/snippets/<int:pk>/highlight', SnippetHighlight.as_view(), name='snippet-highlight'),
    path('/snippets/users', UserList.as_view(), name='user-list'),
    path('/snippets/users/<int:pk>', UserDetail.as_view(), name='user-detail'),
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

# Adding pagination

User 및 Code snippet에 대한 List는 많은 인스턴스를 반환 할 수 있으므로 결과에 페이지 번호를 매기고 API 클라이언트가 각 ㅂ개별 페이지를 단계별로 접근하도록 만들 수 있다.

이를 위해 프로젝트 단의 `settings.py`에 Django REST Framework가 제공하는 pagination을 추가 할 수 있다. 여기서는 `tutorial/settings.py`에 추가하도록 하겠다.

```python
# tutorial/settings.py

...
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
```

Django REST Framework의 설정은 모두 `REST_FRAMEWORK`네임 스페이스에 추가하여 다른 프로젝트 설정과 구분하도록 만들어야 한다.

필요한 경우 pagination은 사용자 지정설정으로 추가할 수 있지만 여기서는 일단 기본값을 그대로 사용하겠다.

# Browsing the API

이제 브라우저를 열고 Code Snippets의 Detail로 이동하면 해당 Code Snippet의 Highlight링크가 생성되었으며 이를 클릭하면 Code Highlight된 `HTML`페이지로 이동된다.

![https://yongineer.duckdns.org/django/til40_1.png](https://yongineer.duckdns.org/django/til40_1.png)


**결과**
___
<p align="center"><img src="https://yongineer.duckdns.org/django/til40_2.png"></p>
