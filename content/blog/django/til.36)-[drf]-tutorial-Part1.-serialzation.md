---
title: TIL.36) [DRF] Tutorial Part1. Serialzation
date: 2020-08-25 03:08:40
category: Django
thumbnail: '../TIL/images/django.png'
draft: false
---

![](../TIL/images/django.png)

해당 내용은 Django REST Framework의 공식문서를 참고하여 작성되었으며. 해당 공식문서는 [여기](https://www.django-rest-framework.org/tutorial/1-serialization/)에서 확인 가능하다.

# 소개

이 튜토리얼에서는 code Highlighting 서비스읜 Pastebin의 간단한 Web API를 만들어보는 과정을 진행한다. 이 과정에서 Django REST Framework를 다루면서 RESTful API를 제작하는데 있어 제공되는 다양한 컴포넌트를 소개하고 이 컴포넌트들이 어떻게 동작하는지 제공한다.

# 환경 설정

공식문서에서는 `venv`를 사용하였으나 나는 `miniconda`를 사용하였다.

```bash
conda create -n drf_tutorial python=3.8
conda activate drf_tutorial
```

이제 프로젝트에 필요한 패키지들을 설치해준다.

```python
pip install django
pip install djangorestframework
pip install pygments # code highlighting에 필요한 패키지
```

# 시작하기

`django` 프로젝트를 만들어 준다.

```bash
django-admin startproject tutorial
```

이제 snippets이라는 애플리케이션을 생성해준다.

```python
python manage.py startapp snippets
```

앱이 성공적으로 생성되었으면 생성한 `snippets`앱과 `rest_framework`앱을 프로젝트의 `settings.py`에 추가해야 한다.

```python
# tutorial/settings.py

INSTALLED_APPS = [
		...
    'rest_framework',
    'snippets'
]
```

# 모델 작성하기

code snippet을 저장하기 위해 모델을 작성한다.

```python
# snippets/models.py

from django.db import models
from pygments.lexers import get_all_lexers
from pygments.styles import get_all_styles

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

    def __str__(self):
        return self.title

    class Meta:
        db_table = 'snippets'
        ordering = ('created',)
```

모델 작성이 완료되었다면 `migration`해준다.

```python
python manage.py makemigrations
python manage.py migrate
```

# Serializer 클래스 만들기

`serializer`는 `queryset`과 `model instance`같은 데이터를 `Json`이나 `xml`과 같은 데이터 형태로 쉽게 랜더링 할 수 있게 해주며 **직렬화** 라고 한다.

반대로 `deserializer`는 **역직렬화**라고 하며 `Json`이나 `xml`와 같은 형태의 데이터를 python 자료형으로 변환하는 것을 말한다.

`serializer`를 사용하기 위해서는 먼저 애플리케이션 경로에 `serializer.py`를 생성하고 `Django`의 `forms`와 비슷한 역할을 하는 `serializer`를 정의하여 사용할 수 있다.

```python
from rest_framework import serializers
from .models import (
    Snippet,
    LANGUAGE_CHOICES,
    STYLE_CHOICES
)

class SnippetSerializer(serializers.ModelSerializer):

# -------> 직렬화 또는 역직렬화 될 필드를 정의
		id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    code = serializers.CharField(style={'base_template': 'textarea.html'})
    linenos = serializers.BooleanField(required=False)
    language = serializers.ChoiceField(choices=LANGUAGE_CHOICES, default='python')
    style = serializers.ChoiceField(choices=STYLE_CHOICES, default='friendly')
    
# -------> serializer.save()가 호출될때 

    def create(self, validated_data):
        # 검증된 데이터(validated_data)가 주어지면 새 snippet 인스턴스를 만들고 반환한다.
        return Snippet.objects.create(**validated_data)

    def update(self, instance, validated_data):
        # 검증된 데이터(validated_data)가 주어지면 기존 snippet 인스턴스를 업데이트하고 반환한다.
        instance.title = validated_data.get('title', instance.title)
        instance.code = validated_data.get('title', instance.code)
        instance.linenos = validated_data.get('title', instance.linenos)
        instance.language = validated_data.get('title', instance.language)
        instance.style = validated_data.get('title', instance.style)
        instance.save()

        return instance
```

`serializer`의 첫 부분은 직렬화 또는 역직렬화 되는 필드를 정의하며 이는 models작성하는 것과 매우 유사하다. 따라서 `required`, `max_length`, `default`와 같이 필드 옵션도 `serializer`에서 사용 가능하다.

`create()`와 `update()`는 `serializer.save()`가 호출 되었을 때 인스턴스를 생성되거나 수정되는 방법을 정의한다.

필드 플래그는 `HTML`을 렌더링할 때와 같이 특정 상황에서 `serializer`가 어떻게 출력되는지 관리해준다. 위의 `{'base_template': 'textarea.html'}`는 `Django Form`의 `widget=widgets.Textarea`과 동일하다.

# Serializer로 작업하기

좀 더 `serializer`와 친숙해지기 위해 `shell`에서 작업한다.

```python
python manage.py shell
```

먼저 앞서 생성한 모델과 serializers를 import하고 `JSONRenderer`와 `JSONParser`를 import 한다.

```python
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser

snippet = Snippet(code='foo = "bar"\n')
snippet.save()

snippet = Snippet(code='print("hello, world")\n')
snippet.save()
```

이제 앞서 만든 `snippet`인스턴스 중 하나를 serializer해본다.

```python
serializer = SnippetSerializer(snippet)
serializer.data

>>> {'id': 2, 'title': '', 'code': 'print("hello, world")\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}
```

결과로 나타난 데이터는 Python의 기본 데이터 유형이다 따라서 이를 `Json`형식으로 변환해준다.

```python
content = JSONRenderer().render(serializer.data)
content

>>> b'{"id": 2, "title": "", "code": "print(\\"hello, world\\")\\n", "linenos": false, "language": "python", "style": "friendly"}'
```

`Json`형식의 데이터를 Deserializer (역직렬화) 하는 것도 가능하다. 그러기 위해선 먼저 Bytes 타입인 `Json`을 구문 분석할 필요가 있기 때문에 BytesIO 모듈을 import 해야한다.

```python
from io import BytesIO

stream = io.BytesIO(content)
data = JSONParser().parse(stream)
serializer = SnippetSerializer(data=data)
serializer.is_valid()
>>> True
serializer.validated_data
>>> OrderedDict([('title', ''), ('code', 'print("hello, world")\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])
serializer.save()
>>> <Snippet: Snippet object>
```

이렇게 역직렬화된 데이터는 Object instance 형태로 저장도 가능하다.

또한, `queryset`도 직렬화 가능하며 이를 위해 `serializer`의 인수로 `many=True`를 추가하는 것만으로 가능하다.

```python
serializer = SnippetSerializer(Snippet.objects.all(), many=True)
serializer.data
>>> [OrderedDict([('id', 1), ('title', ''), ('code', 'foo = "bar"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 2), ('title', ''), ('code', 'print("hello, world")\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 3), ('title', ''), ('code', 'print("hello, world")'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])]
```

# ModelSerializers 사용하기

앞서 `serializers.py`에서 작성했듯 `serializer`를 생성하기 위해서 `model`을 생성하는 것과 같이 유사한 과정을 거쳤었다. 이 코드를 좀더 간결하게 작성 및 유지하고 싶다면 `ModelSerializers`를 사용하면 된다.

미리 작성해 둔 `snippets/serializers.py`를 열어 다음과 같이 리팩토링 한다.

```python
# snippets/serializers.py

class SnippetSerializer(serializers.ModelSerializer):
    class Meta:
        model = Snippet
        fields = ('id', 'title', 'code', 'linenos', 'language', 'style')
```

이미 작성한 각 필드마다 `Serializer`를 정의하지 않고 `model`의 값으로 작성한 `Snippet`모델을 지정하고 `fields`의 속성으로 `Serializer`의 대상이 되는 필드를 지정하면 아까와 같이 반복적인 코드 없이 직렬화가 가능하다.

> ⚠️ exclude
>
`fields`와 다르게 직렬화를 제외시킬 필드를 지정 가능하다. 만약 값이 없다면 모든 필드가 직렬화 된다.

이제 shell에서 직렬화가 잘 되었는지 확인 할 수 있다.

```python
from snippets.serializers import SnippetSerializer
serializer = SnippetSerializer()
print(repr(serializer))

# SnippetSerializer():
#    id = IntegerField(label='ID', read_only=True)
#    title = CharField(allow_blank=True, max_length=100, required=False)
#    code = CharField(style={'base_template': 'textarea.html'})
#    linenos = BooleanField(required=False)
#    language = ChoiceField(choices=[('Clipper', 'FoxPro'), ('Cucumber', 'Gherkin'), ('RobotFramework', 'RobotFramework'), ('abap', 'ABAP'), ('ada', 'Ada')...
#    style = ChoiceField(choices=[('autumn', 'autumn'), ('borland', 'borland'), ('bw', 'bw'), ('colorful', 'colorful')...
```

> ⚠️ ModelSerializer 클래스가 특별한 것을 해주는 것은 아니다. 단지 다음과 같은 도움을 주는것이다.
>
> - 필드들을 자동으로 결정해준다.
> - `create()`와 `update()`함수를 기본적으로 실행시켜 준다.

# Serializer를 사용하여 Django view 작성

Serializer 클래스를 사용하여 API View를 작성하는 방법을 알아보겠다. 다만 지금은 DRF의 다른 기능을 사용하지 않고 일반적인 Django 뷰를 작성한다.

`snippets/views.py`에 다음과 같이 작성한다.

```python
# snippets/views.py

from django.http import (
    HttpResponse,
    JsonResponse
)
from django.views.decorators.csrf import csrf_exempt
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer

@csrf_exempt
def snippet_list(request):
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)
        return JsonResponse(serializer.data, safe=False)

    elif request.method == 'POST':
        data = JSONParser().parse(request)
        serializer = SnippetSerializer(data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data, status=201)
        return JsonResponse(serializer.errors, status=400)

@csrf_exempt
def snippet_detail(request, id):
    try:
        snippet = Snippet.objects.get(id=id)

    except Snippet.DoesNotExist:
        return HttpResponse(status=404)

    if request.method == 'GET':
        serializer = SnippetSerializer(snippet)
        return JsonResponse(serializer.data)

    elif request.method == 'PUT':
        data = JSONParser().parse(request)
        serializer = SnippetSerializer(snippet, data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data)
        return JsonResponse(serializer.errors, status=400)

    elif request.method == 'DELETE':
        snippet.delete()
        return HttpResponse(status=204)
```

여기서 `csrf_exempt`는 `CSRF Token`이 없는 클라이언트에서 view에 `POST`할 수 있기를 원하기 때문에 추가하였으며 일반적으로 처리하지 않는 방법이다.

view 작성이 완료 되었으면 url을 연결해야 한다.

```python
# tutorial/urls.py

from django.urls import (
    path,
    include
)

urlpatterns = [
    path('api/v1/snippets', include('snippets.urls'))
]
```

```python
# snippets/urls.py

from django.urls import path
from .views import snippet_list, snippet_detail

urlpatterns = [
    path('', snippet_list),
    path('/<int:id>', snippet_detail)
]
```

# Web API 테스트하기

먼저 서버를 실행 시켜준다.

```python
python manage.py runserver
```

`httpie`또는 `postman`에서 테스트 할수 있으며 이 포스팅에선 이미 사전에 설치되어있다고 가정 한 뒤 httpie에서 테스트 진행 하겠다.

```bash
http -v GET localhost:8000/api/v1/snippets
```

**Result**

```bash
Accept: */*
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: localhost:8000
User-Agent: HTTPie/2.1.0

HTTP/1.1 200 OK
Content-Length: 350
Content-Type: application/json
Date: Mon, 24 Aug 2020 18:39:45 GMT
Referrer-Policy: same-origin
Server: WSGIServer/0.2 CPython/3.8.5
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

id를 통해 특정 snippet을 가져올 수도 있다.

```bash
http -v GET localhost:8000/api/v1/snippets/2
```

**Result**

```bash
Accept: */*
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: localhost:8000
User-Agent: HTTPie/2.1.0

HTTP/1.1 200 OK
Content-Length: 118
Content-Type: application/json
Date: Mon, 24 Aug 2020 18:41:12 GMT
Referrer-Policy: same-origin
Server: WSGIServer/0.2 CPython/3.8.5
X-Content-Type-Options: nosniff
X-Frame-Options: DENY

{
    "code": "print \"hello,world\"\n",
    "id": 2,
    "language": "python",
    "linenos": false,
    "style": "friendly",
    "title": ""
}
```
