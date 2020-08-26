---
title: TIL.39) [DRF] Tutorial Part4. Authentication & Permissions
date: 2020-08-27 06:08:97
category: Django
thumbnail: 'https://yongnas.iptime.org/django/drf.png'
draft: false
---

![](https://yongnas.iptime.org/django/drf.png)

해당 내용은 Django REST Framework의 공식문서를 참고하여 작성되었으며. 해당 공식문서는 [여기](https://www.django-rest-framework.org/tutorial/4-authentication-and-permissions/)에서 확인 가능하다.

지금까지 만들었던 API에는 Snippet을 편집하거나 삭제할 때 따로 권한이 없었다. 따라서 이번 과정을 통해 다음과 같은 기능을 추가한다.

- Code Snippet 작성 시 작성자를 연결한다.
- 인증된 사용자만 Snippet을 작성 할 수 있다.
- 사용자가 생성한 Snippet만 수정하거나 삭제 할 수 있다.
- 인증되지 않은 사용자는 모든 Snippet을 조회만 할 수 있다.

# Adding information to our model

먼저 `snippets/models.py`의 `Snippet`클래스에 다음과 같은 사항을 추가한다.

- Snippet을 만든 사용자를 나타내는 `owner` 필드 추가
- Snippet Code의 Highlight된 HTML을 저장하는 `highlighted` 필드 추가

```python
# snippets/models.py

owner = models.ForeignKey('auth.User', related_name='snippets', on_delete=models.CASCADE, null=True)
highlighted = models.TextField(null=True)
```

또한, 모델이 저장될 때 (`.save()`) `pygments`라이브러리를 사용하여 하이라이팅된 필드를 채우는 지 확인하기 위해 다음과 같은 모듈을 import 해준다

```python
# snippets/models.py

from pygments.lexers import (
    get_all_lexers,
    get_lexer_by_name
)
from pygments.styles import get_all_styles
from pygments.formatters.html import HtmlFormatter
from pygments import highlight
```

이제 `Snippet`클래스에 `.save()`메서드를 추가해준다.

```python
# snippets/models.py

def save(self, *args, **kwargs):
        lexer = get_lexer_by_name(self.language)
        linenos = 'table' if self.linenos else False
        options = {'title': self.title} if self.title else {}
        formatter = HtmlFormatter(style=self.style, linenos=linenos, full=True, **options)
        self.highlighted = highlight(self.code, lexer, formatter)
        super(Snippet, self).save(*args, **kwargs)
```

전체적인 `snippets/models.py`는 다음과 같다.

```python
# snippets/models.py

from django.db import models
from pygments.lexers import (
    get_all_lexers,
    get_lexer_by_name
)
from pygments.styles import get_all_styles
from pygments.formatters.html import HtmlFormatter
from pygments import highlight

LEXERS = [item for item in get_all_lexers() if item[1]]
LANGUAGE_CHOICES = sorted([(item[1][0], item[0]) for item in LEXERS])
STYLE_CHOICES = sorted((item, item) for item in get_all_styles())

class Snippet(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    code = models.TextField()
    linenos = models.BooleanField(default=False)
    language = models.CharField(choices=LANGUAGE_CHOICES, default='python', max_length=100)
    style = models.CharField(choices=STYLE_CHOICES, default='friendly', max_length=100)
    owner = models.ForeignKey('auth.User', related_name='snippets', on_delete=models.CASCADE, null=True)
    highlighted = models.TextField(null=True)

    class Meta:
        db_table = 'snippets'
        ordering = ('created',)

    def __str__(self):
        return self.title

    def save(self, *args, **kwargs):
        lexer = get_lexer_by_name(self.language)
        linenos = 'table' if self.linenos else False
        options = {'title': self.title} if self.title else {}
        formatter = HtmlFormatter(style=self.style, linenos=linenos, full=True, **options)
        self.highlighted = highlight(self.code, lexer, formatter)
        super(Snippet, self).save(*args, **kwargs)
```

`models.py`작성이 완료 되었으면 이제 migration작업을 진행 해야 한다.

```python
python manage.py makemigrations
python manage.py migrate
```

이제 API테스트에 필요한 사용자를 생성해야 하는데 이 사용자를 만드는 가장 빠른 방법은 `createsuperuser`명령을 사용하는 것이다.

```python
python manage.py createsuperuser
```

# Adding endpoints for our User models

이제 Snippet을 작성할 때 사용자를 사용하므로 해당 사용자에 대한 Serializer를 생성해주는 것이 좋다.

`snippets/serializers.py`에 `UserSerializer`를 추가해주자

```python
from django.contrib.auth.models import User

class UserSerializer(serializers.ModelSerializer):
		# snippets는 User모델의 역관계이므로 ModelSerializer 사용시 포함되지 않아 명시적으로 지정
    snippets = serializers.PrimaryKeyRelatedField(many=True, queryset=Snippet.objects.all())

    class Meta:
        model = User
        fields = ['id', 'username', 'snippets']
```

여기서 `snippets`는 `User` 모델의 역관계이므로 `ModelSerializer`클래스를 사용할 때 기본적으로 포함되지 않으므로 명시적으로 지정해줘야 한다.

이제 `snippets/views.py`에 사용자 관련 view를 추가할 것이다. 추가할 view의 기능은 다음과 같다.

- 사용자 리스트
- 사용자 상세보기

다만, 이 view들은 읽기 전용으로 수정 및 삭제는 불가능하게 제한을 걸 예정인데 이를 위해 `generics`의 `ListAPIView`와 `RetrieveAPIView`를 사용할 것이다.

일단 먼저 `snippets/views.py`에 `UserSerializer`클래스를 import하자.

```python
# snippets/views.py

from .serializers import UserSerializer
```

이제 `UserLIst`, `UserDetail`뷰를 작성하자.

```python
# snippets/views.py

class UserList(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer

class UserDetail(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

전체적인 `snippets/views.py`는 다음과 같다.

```python
# snippets/views.py

from django.contrib.auth.models import User
from rest_framework import generics
from .models import Snippet
from .serializers import (
    SnippetSerializer,
    UserSerializer
)

class SnippetList(generics.ListCreateAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer

class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer

class UserList(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer

class UserDetail(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

마지막으로 `snippets/urls.py`에 User관련 API들을 추가해준다.

```python
# snippets/urls.py

from django.urls import path
from rest_framework.urlpatterns import format_suffix_patterns
from .views import (
    SnippetList,
    SnippetDetail,
    UserList,
    UserDetail
)

urlpatterns = [
    path('', SnippetList.as_view()),
    path('/<int:pk>', SnippetDetail.as_view()),
    path('/users', UserList.as_view()),
    path('/users/<int:pk>', UserDetail.as_view())
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

# Associating Snippets with Users

여기까지 따라왔다면, 이제 Code snippet을 생성하려 한다면 해당 Code snippet을 사용자와 연결할 방법이 없을것이다. 사용자 속성은 Serializer의 일부로 전송되지 않고 대신 request를 통해 들어오는 속성이기 때문이다. 

이를 처리하는 방법은 `snippets/views.py`에서 `.perform_create()`를 override하여 인스턴스가 저장을 관리하고 들어오는 request 또는 요청된 URL에 암시적인 정보를 처리 할 수 있도록 하는것이다.

이제 `snippets/views.py`의 `SnippetList`View 클래스에 다음과 같은 코드를 추가 하자.

```python
def perform_create(self, serializer):
    serializer.save(owner=self.request.user)
```

`serializer`의 `create()`메서드는 이제 요청에서 확인된 데이터와 함께 `owner`필드의 정보를 `self.request.user`에서 가져와 함께 전달한다.

# Updating our serializer

이제 Code snippet을 생성할 때 사용자를 연결하므로 이를 반영하도록 `SnippetSerializer`를 업데이트 해야한다.

`snippets/serializers.py`를 열고 `SnippetSerializer`에 다음과 같은 내용을 추가한다.

```python
owner = serializers.ReadOnlyField(source='owner.username')
```

또한 `Meta`클래스의 `fields`에도 `owner`를 추가해야 한다.

```python
class Meta:
        model = Snippet
        fields = ('id', 'title', 'code', 'linenos', 'language', 'style', 'owner')
```

`source`의 인수는 필드를 채우는 데 사용되는 속성을 제어하며 직렬화된 어떤 속성이든 가리킬 수 있다. 또한, 점 표기법 (`.`)을 사용하는데 이는 `Django`의 템플릿 언어에서 사용되는 것과 유사한 방식으로 주어진 인수의 속성을 탐색한다.

`ReadOnlyField`는 `CharField`, `BooleanField`와 같은 필드와 달리 유형이 지정되어 있지 않은 클래스이다. 형식화되지 않은 `ReadOnlyField`는 항상 읽기 전용이며 직렬화된 표현에는 사용되지만 역직렬화 할 때는  모델 인스턴스를 업데이트 하는데 사용되지 않는다.

또한, `ReadOnlyField`는 `CharField(read_only = True)`와 같은 형식으로 사용할 수도 있다.

# Adding required permissions to views

이제 Code snippet이 작성자와 연결되었으므로 인증된 사용자만 Code snippet을 생성, 수정, 삭제할 수 있도록 `snippets/views.py`를 수정할 것이다.

Django REST Framework에는 View에 접근 권한을 제한하는데 사용할 수 있는 여려 권한 클래스가 제공된다 여기서는 `IsAuthenticatedOrReadOnly`클래스를 사용할껀데 이는 인증된 사용자는 읽기, 쓰기가 가능하지만 인증되지 않은 사용자는 읽기 전용 권한만 부여해준다.

먼저 이를 사용하기 위해 `snippets/views.py`에 `permissions`모듈을 import해준다.

```python
# snippets/views.py

from .permissions import IsOwnerOrReadOnly
```

그리고 `SnippetList`, `SnippetDetail`View클래스에 다음과 같이 추가해준다.

```python
# snippets/views.py

permission_classes = [permissions.IsAuthenticatedOrReadOnly]
```

전체적인 `snippets/views.py`는 다음과 같다.

```python
# snippets/views.py

from django.contrib.auth.models import User
from rest_framework import (
    generics,
    permissions
)
from .models import Snippet
from .serializers import (
    SnippetSerializer,
    UserSerializer
)

class SnippetList(generics.ListCreateAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]

class UserList(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer

class UserDetail(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

# Adding login to the Browsable API

이제 브라우저를 열고 API주소로 이동하면 더 이상 새 Code snippet을 만들 수 없을 것이다. 왜냐하면 이제 새 Code snippet은 로그인이 되어야 생성 가능하기 때문이다. 이를 위해 로그인 View를 추가 해줘야 한다.

프로젝트의 `urls.py`(이하. `tutorial/urls.py`)에 `URLconf`를 편집하여 로그인 View를 추가 할 수 있다.

```python
# tutorial/urls.py

from django.conf.urls import include
from django.urls import (
    path,
    include
)

urlpatterns = [
    path('api/v1/snippets', include('snippets.urls'))
]

urlpatterns += [
    path('api/v1/snippets', include('rest_framework.urls'))
]
```

이제 브라우저를 다시 열고 페이지를 새로 고침 하면 오른쪽 상단에 'login' 버튼이 생긴것을 확인 할 수 있을 것이다.

![https://yongnas.iptime.org/django/til39_1.png](https://yongnas.iptime.org/django/til39_1.png)

앞서 생성한 Superuser로 로그인 하면 다시 Code snippet을 생성 할 수 있다.

몇개의 Code snippet을 생성 한 후 users endpoint로 이동하면 각 사용자가 생성한 Code snippet의 ID목록을 확인 할 수 있다.

![https://yongnas.iptime.org/django/til39_2.png](https://yongnas.iptime.org/django/til39_2.png)

# Object level permissions

우리의 API는 Code snippet이 모든 사용자(인증되지 않은 사용자라도)에게 표시되기를 원하지만 Code snippet을 생성한 사용자(작성자)만 Code snippet을 수정하거나 삭제할 수 있어야 한다. 이를 위해 사용자 지정 권한을 생성해야 한다.

`snippets/permissions.py`를 생성하고 다음과 같이 작성한다.

```python
# snippets/permissions.py

from rest_framework import permissions

class IsOwnerOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        # 모든 요청에 대한 읽기 권한을 허용한다.
        # 따라서, GET, HEAD, OPTION 요청을 항상 허용한다.
        if request.method in permissions.SAFE_METHODS:
            return True

        # 쓰기 권한은 Snippet 소유자에게만 허용된다.
        return obj.owner == request.user
```

이제 `snippets/views.py`의 `SnippetDetail`View클래스 에서 `permission_classes`속성을 편집하여 사용자 지정 권한을 추가 할 수 있다. 이를 위해 먼저 `snippets/permissions.py`에서 생성한 `IsOwnerOrReadOnly`클래스를 import해주자.

```python
# snippets/views.py

from .permissions import IsOwnerOrReadOnly
```

그리고 이제 `SnippetDetail`View클래스 에서 `permission_classes`속성을 편집한다.

```python
# snippets/views.py

class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
...
permission_classes = [permissions.IsAuthenticatedOrReadOnly, IsOwnerOrReadOnly]
```

전체적인 `snippets/views.py`는 다음과 같다.

```python
# snippets/views.py

from django.contrib.auth.models import User
from rest_framework import (
    generics,
    permissions
)
from .models import Snippet
from .serializers import (
    SnippetSerializer,
    UserSerializer
)
from .permissions import IsOwnerOrReadOnly

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

이제 브라우저를 다시 열면 `DELETE`와 `PUT`작업은 Code snippet을 생성한 사용자만이 가능한 것을 확인 할 수 있다.

![https://yongnas.iptime.org/django/til39_3.png](https://yongnas.iptime.org/django/til39_3.png)

*로그인 하지 않은 사용자 or Code snippet의 작성자가 아닐때*

![https://yongnas.iptime.org/django/til39_4.png](https://yongnas.iptime.org/django/til39_4.png)

*Code snippet의 작성자일때*

# Authenticating with the API

이제 API에 대한 인증 및 권한 절차가 생성 되었으므로 Code snippet을 편집하려면 요청을 인증해야 한다. 아직 인증 클래스를 따로 설정하지 않았으므로 현재는 기본값인 `SessionAuthentication`및 `BasicAuthentication`이 적용된다.

웹 브라우저를 통해 API와 상호 작용할 때 로그인 할 수 있으며 브라우저 세션은 요청에 필요한 인증을 제공한다. 기존에 테스트 했었던 `httpie`또는 `postman`에서 API와 상호 작용하려는 경우 각 요청에 인증 자격 증명을 명시적으로 제출해야 한다.

만약 인증하지 않고 Code snippet을 생성하려고 하면 오류가 발생한다.

```bash
http -v POST http://localhost:8000/api/v1/snippets code='print(12304)'

HTTP/1.1 403 Forbidden
Allow: GET, POST, HEAD, OPTIONS
Content-Length: 58
Content-Type: application/json
Date: Wed, 26 Aug 2020 19:14:55 GMT
Referrer-Policy: same-origin
Server: WSGIServer/0.2 CPython/3.8.5
Vary: Accept, Cookie
X-Content-Type-Options: nosniff
X-Frame-Options: DENY

{
    "detail": "Authentication credentials were not provided."
}
```

앞서 생성한 사용자의 이름과 비밀번호를 포함하여 요청하면 생성이 가능하다.

```bash
http -a admin:password123 POST http://localhost:8000/api/v1/snippets code="print(789)"

{
    "id": 1,
    "owner": "admin",
    "title": "foo",
    "code": "print(789)",
    "linenos": false,
    "language": "python",
    "style": "friendly"
}
```
