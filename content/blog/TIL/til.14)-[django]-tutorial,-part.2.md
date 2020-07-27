---
title: TIL.14) [Django] Tutorial, Part.2
date: 2020-07-27 18:18:47
category: TIL 
thumbnail: './images/django.png'
draft: false
---

![](./images/django.png)

# 데이터베이스 설치

Django에서는 기본적으로 `SQLite`를 제공한다. 하지만 만약 다른 데이터베이스를 사용하고자 한다면 `mysite/settings.py`를 열어 다음의 항목을 수정한다.

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

1. `**ENGINE**`
    - Django에서 지원하는 데이터베이스를 입력해준다.
    - `'django.db.backends.sqlite3'`, `'django.db.backends.postgresql'`, `'django.db.backends.mysql'`, `'django.db.backends.oracle'` 등이 있다.
2. `**NAME**`
    - 데이터베이스의 이름
    - `os.path.join()`로 경로와 파일명을 지정한다.
    - 기본값은 프로젝트 디렉토리 (`BASE_DIR`)에 `db.sqlite3`이다.

만약 `SQLite`를 데이터베이스로 사용하지 않는 경우 `USER`, `PASSWORD`, `HOST`같은 추가 설정이 반드시 필요하다.

데이터베이스 설정이 완료 되었으면 `TIME_ZONE`값을 설정한다. 해당 값은 서비스할 국가에 맞게 설정한다.

그 다음은 `INSTALLED_APPS`를 설정하는데 이 변수는 다음의 어플리케이션을 함께 포함하고 있다.

1. **`[django.contrib.admin]`** : 관리용 사이트
2. **`[django.contrib.auth]`** : 인증 시스템
3. **`[django.contrib.contenttypes]`** : 컨텐츠 타입을 위한 프레임워크
4. **`[django.contrib.sessions]`** : 세션 프레임워크
5. **`[django.contrib.messages]`** : 메세징 프레임워크
6. **`[django.contrib.staticfiles]`** : 정적 파일을 관리하는 프레임워크

이 기본 어플리케이션중 일부는 최소한 하나 이상의 데이터베이스 테이블을 사용한다. 따라서 데이터베이스에서 테이블을 미리 만들 필요가 있으므로 터미널에서 다음의 명령을 실행한다.

```python
$ python manage.py migrate
```

# 모델 만들기

모델이란 부가적인 메타데이터를 가진 데이터베이스의 구조를 말한다.

[Part.1](http://yonineer.netlify.app/TIL/til.13\)[django]-tutorial,-part.1/)에서 만들었던 polls 어플리케이션에서 사용할 질문(Question)과 선택(Choice)에 대한 모델을 만들자.

먼저 **Question**모델의 구성은 다음과 같다.

- 질문내용
- 질문을 생성한 날짜

그 다음 **Choice**모델의 구성은 다음과 같다.

- 선택지
- 총 득표수

또한 Choice모델은 Question모델의 각 질문 내용과 연결되어 있도록 한다.

그럼 이제 `polls/models.py`파일을 열어 모델을 작성한다.

```python
# polls/models.py

from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

이제 `Question`모델과 `Choice`모델은 `django.db.models.Model`을 인수로 받는 클래스가 되었다. 또한 각 모델의 변수는 데이터베이스의 `Field`가 된다.

이 `Field`명을 Python 코드에서 사용할 수 있으며 Database에서는 컬럼명으로 사용할 것이다. 또한, 데이터베이스 `Field`명은 사용자가 직접 지정할 수도 있는데 위 코드의`pub_date`의 경우 `Field`클래스의 생성자에 첫번째 인수로 사용자가 이름을 직접 지정하여 사용한다. (`'date published'`)

다만, 이는 필수가 아닌 선택이며 만약 첫번째 인수로 사용자 이름을 지정하지 않는다면 Django는 자동으로 변수명을 데이터베이스의 `Field`명으로 사용하고 이를 기계가 읽기 좋은 형식 (`machine-friendly format`)이라고 한다.

`CharField`와 `DateTimeField`,`IntegerField` 는 해당 필드의 자료형인데 `CharField`는 문자형(character), `DateTimeField`는 날짜와 시간 (Datetime), `IntegerField`는 정수형 (Integer)자료를 표현한다. Django는 더 많은 자료형을 지원하며 자세한 내용은 [여기](https://docs.djangoproject.com/en/3.0/ref/models/fields/)를 참고하자.

또한, 자료형은 다양한 인수를 받을 수 있는데 이 인수들은 필수적일수도 있고 선택적일수도 있다. 

예를 들어`CharField`의 경우 `max_length`를 필수 인수로 받아야 하는 반면 `IntegerField`의 경우 `default`인수는 필수가 아닌 선택 인수이다.

마지막으로 `ForeignKey`를 통해 모델 간 관계를 다음과 같이 설정 할 수 있다. 위 예제는 각각의 `Choice`가 하나의 `Question`과 관계된다 것을 Django에게 알려준다.

# 모델의 활성화

모델을 활성화 함으로써 Django는 다음과 같은 작업을 수행 한다.

- 어플리케이션을 위한 데이터베이스 스키마 생성 (`CREATE TABLE`)
- `Question`과 `Choice`객체에 접근하기 위한 Python 데이터베이스 접근 API를 생성

하지만 위 작업을 수행하기 위해 먼저 프로젝트에 `polls`어플리케이션이 있다는 것을 알려야 한다.

어플리케이션을 프로젝트에 포함시키기 위해서는 프로젝트의 구성 클래스 즉, `mysite/settings.py`파일에 `INSTALLED_APPS` 설정을 추가 해야 한다. 그러기 위해 먼저 `polls/apps.py`파일을 열어 보자

```python
# polls/apps.py
from django.apps import AppConfig

# Application Config
class PollsConfig(AppConfig):
    name = 'polls'
```

위 예제코드를 보면 어플리케이션 설정 클래스 (`PollsConfig`)가 존재하는걸 알 수 있는데 이 설정을 `mysite/settings.py`에 `INSTALLED_APPS`설정에 추가하면 된다.

```python
# mysite/settings.py
...
INSTALLED_APPS = [
    'polls.apps.PollsConfig', # polls config
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
...
```

> 💡 **polls.apps.PollsConfig?**
>
`polls/apps.py` 파일 내에 있는 `PollsConfig`라는 뜻 변수, 함수, 패키지 모듈 호출할 때 경로 지정하는 것과 동일하다.

이제 해당 모델들을 `migration`으로 저장하기 위해 터미널에 다음 명령을 입력한다.

```python
$ python manage.py makemigrations polls
```

그러면 다음과 같이 출력된다.

```python
Migrations for 'polls':
  polls/migrations/0001_initial.py
    - Create model Question
    - Create model Choice
```

`makemigrations`명령을 실행함으로써 모델이 변경 되었다거나 새로 모델이 만들어졌다던가 하는 변경사항을 `migration`하고 싶다는 것을 Django에게 알려주게 된다. 이에 따라 장고는 `migrate`명령으로 `migration`을 수행하는데 이 전에 먼저 `sqlmigrate`명령을 통해 `migration`이 내부적으로 어떤 `SQL`문장을 실행 하는지 알아보기 위해 터미널에 다음 명령을 입력한다.

```python
$ python manage.py sqlmigrate polls 0001
```

그러면 다음과 같이 출력된다.

```python
BEGIN;
--
-- Create model Question
--
CREATE TABLE "polls_question" ( # 1️⃣
    "id" serial NOT NULL PRIMARY KEY, # 2️⃣
    "question_text" varchar(200) NOT NULL, # 3️⃣
    "pub_date" timestamp with time zone NOT NULL # 3️⃣
);
--
-- Create model Choice
--
CREATE TABLE "polls_choice" ( # 1️⃣
    "id" serial NOT NULL PRIMARY KEY, # 2️⃣
    "choice_text" varchar(200) NOT NULL, # 3️⃣
    "votes" integer NOT NULL, # 3️⃣
    "question_id" integer NOT NULL # 3️⃣
);
ALTER TABLE "polls_choice" # 4️⃣
  ADD CONSTRAINT "polls_choice_question_id_c5b4b260_fk_polls_question_id" # 5️⃣
    FOREIGN KEY ("question_id") # 5️⃣
    REFERENCES "polls_question" ("id") # 5️⃣
    DEFERRABLE INITIALLY DEFERRED; # 5️⃣
CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id"); # 5️⃣

COMMIT;
```

위 과정을 풀어서 설명하면 다음과 같다.

1️⃣먼저 어플리케이션의 이름인 `polls`와 모델명의 소문자 표기명을 합쳐 `CREATE TABLE`을 통해 테이블을 생성한다.

2️⃣기본키(`ID`)가 자동으로 추가된다.

3️⃣`polls/models.py` 에서 생성한 `Field`클래스가 데이터베이스의 `Field`로 추가된다.

4️⃣ 외래키 (`Foreign Key`) 가 존재하는 `Choice`테이블과 `Question`의 관계를 연결시키기 위해 `ALTER TABLE`을 통해 두 테이블을 수정해 준다.

5️⃣ `choice`테이블의 `question`과 `question`의 `id`를 외래키로 연관시켜 준다.

`makemigrations`가 완료되면 이제 `migrate`를 실행시켜 `models.py`에서 만든 모델을 테이블로 생성한다.

```powershell
$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, polls, sessions
Running migrations:
  Rendering model states... DONE
  Applying polls.0001_initial... OK
```

`migrate`명령은 아직 적용되지 않은 마이그레이션을 모두 수집해 이를 실행하며 이 과정을 통해 모델에서의 변경 사항들과 데이터베이스 스키마의 동기화가 이루어진다.

> **💡모델을 생성/변경 하는 방법 (정리)**
>
>1. `models.py`에서 모델을 생성 또는 변경한다.
>2. `python [manage.py](http://manage.py) makemigrations`를 통해 생성된 모델의 마이그레이션을 만든다.
>3. `python [manage.py](http://manage.py) migrate`명령을 통해 모델을 테이블로 생성한다.

# API 가지고 놀기

`shell`에 접근하기 위해 다음의 명령을 입력한다.

```powershell
python manage.py shell
```

`shell`은 Django에서 동작하는 모든 명령을 python 쉘에서 그대로 사용해 볼 수 있다.

```python
# 앞서 작성한 polls 어플리케이션의 모델 Choice, Question을 Import 한다.
>>>> from polls.models import Choice, Question  

# Question.objects.all() : Question의 모든 데이터를 호출한다.
# 아직은 Question에 데이터가 없는 상태로 Queryset은 빈 리스트로 호출된다.
>>> Question.objects.all()
<QuerySet []>

# 질문발행(pub_date) 시간을 기록하기 위해 django.utils에서 timezone을 import 한다.
>>>> from django.utils import timezone

# Question 테이블에 question_text 필드 값 What's new?
# qub_date 필드 값은 현재시간으로 변수 q에 선언
>>>> q = Question(question_text="What's new?", pub_date=timezone.now())

# 데이터테이블에 위에서 선언한 값을 저장
>>>> q.save()

# ID는 생성하지 않아도 django 에서 자동 생성 해준다 따라서 q의 id값은 1이 된다.
>>>> q.id
1

# 변수 q에는 위에서 선안한 필드값이 속성으로 생성되었으며 해당 속성을 호출시 속성이 가진 값이 출력된다.
>>>> q.question_text
"What's new?"
>>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)

# 또한 해당 속성을 다음과 같이 수정할 수 있다
>>>> q.question_text = "What's up?"
>>>> q.save()

# objects.all()은 데이터 베이스의 저장된 모든 값을 호출 한다. 
>>>> Question.objects.all()
<QuerySet [<Question: Question object (1)>]>
```

여기서 `Question.objects.all()`의 결과인 `<QuerySet [<Question: Question object (1)>]>`는 어떤 데이터인지 알아보기 힘든데 이때 `__str__()`method를 추가하여 데이터를 알아볼 수 있게 해줄 수 있으며 이는 admin 사이트에서도 객체의 표현으로 사용된다.

```python
# polls/models.py

from django.db import models

class Question(models.Model):
    # ...
    def __str__(self):
        return self.question_text

class Choice(models.Model):
    # ...
    def __str__(self):
        return self.choice_text
```

또한 django는 커스텀 method를 지원하며 아래는 어제자로 부터 발행된 질문을 출력하는 커스텀 메소드이다.

```python
# polls/models.py
import datetime

from django.db import models
from django.utils import timezone

class Question(models.Model):
    # ...
    def was_published_recently(self): # 커스텀 메소드
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
```

변경사항을 마이그레이션 한 후 다시 `shell`에서 위에 추가된 변경사항을 확인해 보자.

```python
>>>> from polls.models import Choice, Question

# objects.all() 사용시 __str__메소드에서 작성한 question_text를 반환한다.
>>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

# objects.filter()를 통해 데이터 조회가 가능하다.
# <Table name>.objects.filter(<field name> = <value>
# 조회시 다양한 옵션을 지원한다 (__startswith : 시작문자를 포함한 데이터를 조회)
>>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>

# objects.get()을 통해서도 데이터 조회가 가능하다.
>>>> from django.utils import timezone
>>>> current_year = timezone.now().year # 올해 값을 current_year변수에 선언
# __year : 시간 타입일때 연도만 조회
# 발행일 중 올해 발행된 데이터만 가져온다.
>>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>

# objects.get()에서는 찾고자 하는 조건이 없을때 에러를 발생 시킨다.
>>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.

# django에서는 데이터를 기본키로 조회하는 것이 일반적이며 아래의 코드는 다음과 같다
# Question.objects.get(id=1) 
>>>> Question.objects.get(pk=1)
<Question: What's up?>

# 변수 q에 데이터를 가져왔을때 앞서 작성한 커스텀 메소드도 담기게 된다. 
>>>> q = Question.objects.get(pk=1)
>>>> q.was_published_recently()
True

# 앞서 models.py에서 Choice 테이블의 question을 ForeignKey로 지정하였는데
# 따라서 변수 q에 Question의 데이터를 가져오면 Choice 테이블의 값을 같이 가져오게 된다.
>>>> q = Question.objects.get(pk=1)
>>>> q.choice_set.all()
<QuerySet []>

# 따라서 Question 데이터를 담은 변수 q에서 Choice 테이블의 데이터를 생성 할 수 있다.
>>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

# 또한 Choice객체는 관련된 Question객체에 대한 API 접근 권한을 갖는다.
>>>> c.question
<Question: What's up?>

# 반대의 경우인 Question 객체도 Choice객체에 대한 접근 권한을 갖는다.
>>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>>> q.choice_set.count()
3

# API는 자동으로 우리가 필요한 관계를 자동으로 생성한다.
# 이 관계는 이중 밑줄 (__)로 분리하여 사용 할 수 있다.
# Choice객체에서 올해 발행된 모든 질문을 찾기 위해 다음과 같이 사용 할 수 있다.
# question__pub_date__year
# question : 외래키로 Question이 지정된 필드
# __pub_date : Question이 갖고 있는 발행일 필드
# __year : 그 중 연도의 값만 갖는다. 
>>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# 데이터를 삭제하기 위해 delete()를 사용한다.
>>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>>> c.delete()
```

> 💡 **`filter()` vs `get()`**

 - `filter()`
    1. 특정 조건을 만족하는 **여러개의 객체**를 포함하는 `QuerySet`을 반환한다.
    2. 따라서 `filter()`의 결과를 확인하기 위해서는 `for` 와 같은 반복문을 사용해 확인해야 한다. (`QuerySet`은 `list`형태이기 때문
```python
>>>> m = Menu.objects.filter(id=1)
>>>> m
<QuerySet [<Menu: Menu object (1)>]> # 쿼리셋은 리스트의 형태로 반환
>>>> for i in m:
...     i.name
'음료'
```
   3. 만약 일치하는 조건이 없다면 빈 `QuerySet`을 반환한다.
```python
>>>> Drink.objects.filter(name="엄마는 외계인")
<QuerySet []> # 빈 쿼리셋을 반환
```
>
- `get()`
    1. 특정 조건을 만족하는 하나의 객체를 포함하는 `object`를 반환한다.
    2. 따라서 Unique 한 값을 가지고 검색하는 것이 좋다.
    3. 만약 조건 검색 결과로 여러개의 값이 조회되면 `MultipleObjectsReturned`에러가 발생한다.

```python
# Category는 두개의 객체가 있다.
>>> Category.objects.all()
<QuerySet [<Category: Category object (1)>, 
<Category: Category object (2)>]>
>>>> c = Category.objects.all()
>>>> for i in c:
...     i.name
...
'콜드브루커피'
'브루드 커피'
# 만약 위와 같은 객체에서 name에 "커피"를 포함한 객체를 반환 받으려 한다면
# MultipleObjectsReturned 에러가 발생하게 된다.
>>>> c.get(name__icontains="커피")
modeler.models.Category.MultipleObjectsReturned:
get() returned more than one Category -- it returned 2!
```
4. 결과는 오직 하나의 객체를 갖고 오게 되어 `filter()`와 같이 반복문을 사용할 필요 없이 해당 객체를 호출하는 것만으로 값을 확인 할 수 있다.
```python
>>>> c = Category.objects.all()
>>>> get_result=c.get(id=2)
>>>> get_result
<Category: Category object (2)>
>>>> get_result.name
'브루드 커피'
```

# Django 관리자 소개

## 관리자 생성하기

관리자 생성을 위해 터미널에서 다음 명령을 수행한다.

```python
$ python manage.py createsuperuser
```

그런 다음 계정을 생성 하게 되며 이 계정은 관리자 계정이 된다.

```python
Username: admin # 사용자 지정 이름도 가능하다.
Email address: admin@example.com
Password: **********
Password (again): *********
Superuser created successfully.
```

## 개발 서버 시작

서버는 다음의 명령으로 시작할 수 있다.

```python
$ python manage.py runserver
```

이제 로컬 도메인 `/admin/`으로 이동하면 관리자 페이지에 접근 할 수 있으며 다음과 같은 로그인 화면이 노출된다.

![](https://images.velog.io/images/yongineer1990/post/4f9e7798-7f2e-4feb-bcd3-1e3d77878224/image.png)

## 관리자 사이트 들어가기

위에서 생성한 슈퍼유저 계정으로 로그인하면 다음과 같은 관리자 페이지가 노출된다.
![](https://images.velog.io/images/yongineer1990/post/14a54823-1d95-4e86-a97c-49cba685244b/image.png)

현재는 편집 가능한 그룹과 사용자와 같은 컨텐츠만 노출되고 앞서 생성한 polls 어플리케이션에 관한 관리 메뉴가 보이지 않는다 이는 아직 polls 어플리케이션을 관리하도록 설정하지 않아서 노출되지 않는다.


## 관리자 사이트에서 poll 어플리케이션을 변경하도록 만들기

먼저 관리자에게 Question 객체가 있다는것을 알려줘야 한다. 따라서 `polls/admin.py`에 다음과 같이 코드를 장성한다.

```python
# polls/admin.py

from django.contrib import admin # 관리자 모듈인 admin import

from .models import Question

admin.site.register(Question) # 관리자 사이트에 models.py에서 작성한 Question을 등록한다.
```


## 관리 기능 탐색 하기

Question을 등록하고 관리자 사이트에 재접속 해본다.
![](https://images.velog.io/images/yongineer1990/post/a1336e38-e6af-4aa6-b0ca-583ccb4063ef/image.png)

앞서 만든 polls 어플리케이션의 Question 모델이 관리자페이지에서 노출되는것을 확인 할 수 있다.

이제 Questions를 클릭해보면 해당 모델의 관리 페이지로 이동하게 된다.
![](https://images.velog.io/images/yongineer1990/post/cfa1defd-ec93-4595-ab9e-3c406c198636/image.png)

질문인 What's up? 을 클릭해 들어가 보면 해당 질문의 관리 페이지로 이동하게 된다.
![](https://images.velog.io/images/yongineer1990/post/2f4645de-cf5b-4be1-8f74-64c4dbe34f17/image.png)

여기선 다음과 같은 기능을 수행한다.

1. 데이터 수정 (현재는 `question_text`, `pub_date`를 수정 할 수 있다.)
2. 저장 (Save) : 이 객체에 대한 변경사항을 저장한다.
3. 저장 및 편집 계속 (Save and continue edition) : 이 객체에 대한 변경사항을 저장하고 현재 편집 페이지를 갱신 한다.
4. 저장 및 다른 이름으로 추가 (Save and add another) 이 객체에 대한 변경사항을 저장하고 이 유형의 객체에 대한 비어있는 새로운 입력창을 불러온다.
5. 삭제 (Delete) : 삭제를 확인하는 페이지를 호출한다.
