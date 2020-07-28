---
title: TIL.10)[Django] 시작하기전 알아두어야 할 개념들
date: 2020-07-27 14:07:18
category: Django
thumbnail: './images/django.png'
draft: false
---

![](./images/django.png)

# GET / POST / PUT / DELETE

## GET

- `Server` 혹은 `DB`에게 `resource`를 보내달라고 요청하는 것.
- `resource`는 `Client`에게 전달만 될 뿐 변경되지 않는다.
- `Browser` 주소창에 주소를 입력하면 이 신호는 항상 `get`으로 요청된다.
- SQL = `read`

## POST

- `Server` 혹은 `DB`에게 `resource`를 보내면서 생성해 달라고 요청하는 것
    - ex) 회원가입을 하면 DB에 새로운 회원 정보가 등록 되면서 프로필 사진을 업로드하면 그 사진이 해당 회원의 프로필 사진으로 등록되는 것.
- **Django에서 주로 사용**
- SQL = `create`

## PUT

- `Server` 혹은 `DB`에게 `resource`의 업데이트를 하거나 `resource`가 없다면 새로운 `resource`를 생성해 달라고 요청하는 것
- 회원 정보 수정 등에 사용된다.
- SQL = `update`

## PATCH

- `Server` 혹은 `DB`에게 `resource`의 업데이트를 요청한다.
- 회원 정보 수정 등에 사용된다.

> 💡**PUT vs PATCH**
- **PUT**
    - Data에서 하나의 필드만 업데이트 하더라도 항상 모든 필드값을 가져와서 모든 필드를 항상 새로운 값으로 교체
- **PATCH**
    - Data에서 하나의 필드값을 업데이트 시킬때 해당 필드값만 가져와서 해당 부분만 업데이트 한다.

## DELETE

- `Server` 혹은 `DB`에게 `resource`의 삭제를 요청한다.
- SQL = `delete`
![](https://images.velog.io/images/yongineer1990/post/ed1fc967-ced2-4074-921c-80b0ef8b7a23/image.png)

# REST 와 Django url 맵핑

- `REST URL`이 들어오면 `URLconf`에 사전 정의된 규칙에 의해 Django에서 함수를 호출한다.
    - 💡URLconf : `django`에서는 `URLconf`를 `urls.py`에서 관리한다.
![](https://images.velog.io/images/yongineer1990/post/71ad1252-c150-431f-8366-0d0076de641d/image.png)

# MVC vs MVT

## MVC (Model View Controller)

1. **Model**
    - 데이터를 처리하며 데이터베이스와 상호 작용하는 인터페이스 역할을 한다.
    - 기본적으로 데이터베이스의 복잡성에 얽매이지 않고 데이터와 상호작용 할 수 있다.
2. **View**
    - 웹 응용 프로그램인 브라우저에 실제 사용자에게 표시되는 프레젠테이션 로직을 처리
    - 실제 브라우저라는 응용 프로그램의 UI를 나타낸다.
3. **Controller**
    - View에서 핸들러의 흐름을 처리하거나 Model의 데이터를 업데이트 처리하는 로직을 제공한다.
![](https://images.velog.io/images/yongineer1990/post/71b19e7d-6ef2-447b-a90f-f9d942c5f030/image.png)

### MVT (Model View Template)

- 위의 `MVC`를 `Django`에서는 `MVT`라고 한다.
- `MVT`는 `MVC`와 각각 다음과 같이 매칭된다.
![](https://images.velog.io/images/yongineer1990/post/d41eb5a3-3581-45ee-b648-dccd69740c31/image.png)

# Django Workflow
![](https://images.velog.io/images/yongineer1990/post/17296c1e-ced0-4bdf-8b91-a979d3b20c4b/image.png)

## 1️⃣ Client ➡️ URLconf(urls.py)

- 브라우저 또는 Application 사용자가 `request`를 보내면 Django에서 해당 REST URL을 `URLconf(urls.py)`에서 사전 정의된 함수가 있는지 확인하는 과정
- 만약, 해당 REST URL과 매칭되는 `URL`이 없다면 `Status Code : 404`를 반환한다.

## 2️⃣ URLconf(urls.py) ➡️ View(views.py)

- `URLconf(urls.py)`와 매칭되는 `VIew(views.py)`함수를 실행한다.

## 3️⃣ View(views.py) ➡️ Model(models.py)

- 사용자에게서 받은 `request`를 확인하고 만약, 데이터베이스에서 해당 요청을 처리해야 할 경우 데이터 관리를 하는 `Model(models.py)`에게 해당 요청을 처리할 것을 지시한다.
    - 💡만약 데이터베이스 접근이 필요 없는 요청일 경우 위 과정을 거치지 않고 바로 `Template`에서 적절한 페이지를 찾아 `response`해준다.
- `Client`요청에 따른 `Application` 실행 결과를 `Template` 혹은 `Error msg`등을 이용해서 다시 `Client`에게 `response`해준다.

## 4️⃣ Model(models.py) ➡️ Database

- `View(views.py)`에서 받은 요청을 `Database`에서 처리하여 그 결과를 다시 `View(views.py)`에게 전달해준다.
    - 💡대표적인 `Database` 처리
        1. Create : 생성
        2. Read : 읽기
        3. Update : 갱신
        4. Delete : 삭제
- `ORM (Object Relational Mapping)`를 사용한다.
- `Class`를 이용하여 맵핑한다.

> 💡ORM?
- 객체 관계 데이터베이스
- 소프트웨어 개발자가 스스로 데이터 형과 메서드(=객체의 `class`)를 자유롭게 정의하여 데이터베이스를 개발할 수 있는 데이터베이스 관리 시스템
- Django에서는 `Model Calss`를 통해서 객체를 만들고 이 객체를 통해서 `DB`에 접근하도록 도와준다.
![](https://images.velog.io/images/yongineer1990/post/32e52696-b6b9-464f-8d19-b06c32eec6f9/image.png)

## 5️⃣ View(views.py) ➡️ Template (*.html)

- `Model(models.py)`에서 처리된 결과를 Template에 전달하여 적절한 페이지 (`html`)를 찾는다.

## 6️⃣ Template(*.html) ➡️ Client

- `html`파일과 `Database`에서 처리된 결과를 조합하여 페이지를 사용자에게 전달한다.
