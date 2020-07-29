---
title: TIL.24) Part 3 - 정렬과 연산
date: 2020-07-30 01:45:32
category: SQL
thumbnail: '../TIL/images/sql.png'
draft: false
---

![](../TIL/images/sql.png)

# 정렬 - ORDER BY

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명;
```

- `SELECT`명령에 `ORDER BY`구를 지정하면 검색 결과의 행 순서를 바꿀수 있다.
- `ORDER BY`구를 지정하지 않는 경우에는 데이터베이스 내부에 저장된 순서로 반환

## 1. ORDER BY로 검색 결과 정렬하기

- `ORDER BY`구로 정렬하고 싶은 열을 지정
- `ORDER BY`구는 `WHERE`구 뒤에 지정한다.
- 검색 조건이 필요 없는 경우에는 `WHERE`구를 생략하고 이때 `ORDER BY`구는 `FROM`구 뒤에 지정

    ```sql
    SELECT 열명 FROM 테이블명 ORDER BY 열명;
    ```

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ebfeb03-3a12-4124-886a-6a6c4e26fb4c/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0ebfeb03-3a12-4124-886a-6a6c4e26fb4c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T165047Z&X-Amz-Expires=86400&X-Amz-Signature=31d5279f024f98413bcbae462a0070d2d323c338d43b14e774da112d57f186eb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ef14195-2b78-467d-bd4d-449bc283e44e/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6ef14195-2b78-467d-bd4d-449bc283e44e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T165126Z&X-Amz-Expires=86400&X-Amz-Signature=3e2c49168a31cb948a2bbccb4f1d899541f7ca56203ea4fc5f342dcb48a7048b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## 2. ORDER BY DESC로 내림차순으로 정렬하기

```sql
# 내림차순
SELECT 열명 FROM 테이블명 ORDER BY 열명 DESC;

# 오름차순
SELECT 열명 FROM 테이블명 ORDER BY 열명 ASC;
```

- 오름차순은 내림차순과 달리 생략 가능
- `DESC` : **DESC**endant
- `ASC` : **ASC**endant
- 정렬방법을 지정하지 않은 경우에는 `ASC`로 간주하며 `ORDER BY`의 기본 정렬방법은 오름차순

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b82c4fb8-c10e-43ed-8fc5-8b2f29603e0a/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b82c4fb8-c10e-43ed-8fc5-8b2f29603e0a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T165240Z&X-Amz-Expires=86400&X-Amz-Signature=504aa5f95e716c4d0ffcc56f98a2652f7affc2cb5215ff031799b9dc7dcd7291&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*`age`열의 값을 `DESC`로 내림차순 정렬*

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/58578eae-d55b-484c-994c-99beb300f945/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/58578eae-d55b-484c-994c-99beb300f945/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T165755Z&X-Amz-Expires=86400&X-Amz-Signature=efe2e52b689aa4db8884136e28e127073bc8356438510bbbba55d8c22c97a6b2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*`age`열의 값을 `ASC`로 오름차순 정렬*

## 3. 대소관계

- 수치형 데이터의 대소관계는 숫자의 크기로 판별
- 날짜시간형 데이터의 대소관계는 숫자의 크기로 판별
- 문자열형 데이터의 대소관계는 사전식 순서에 의해 결정

    💡 **알파벳 → 한글** 순이며 **한글은 자음 → 모음** 순

> ⚠️ **한글의 정렬 순서**
>
예를 들어 '나비', '가방', '가족'을 정렬 할 경우
>
> - 가방
> - 가족
> - 나비

### 사전식 순서에서 주의할 점

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7eeb25cd-1b65-4123-97ae-f83200b9d7a6/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7eeb25cd-1b65-4123-97ae-f83200b9d7a6/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T165923Z&X-Amz-Expires=86400&X-Amz-Signature=f0e7f69c8fdbfca78d421890f5828382c6a6967ce0d839c66de729966921b84c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*`a`열을 오름차순으로 정렬*

`Sample311`은 `a`열이 문자열형 (`VARCHAR`)으로 `b`열이 수치형(`INTEGER`)으로 이루어진 테이블이다. 하지만 결과를 보면 이상한데 1보다 2가 큰 것은 당연한데 10과 11이 2보다 작은 숫자로 판단되어 정렬 되었다.

이는 `a`열이 문자열형으로 되어 있어 대소관계를 사전식 순서로 비교 했기 때문에 발생하는 현상이다.

문자열형 열에 숫자를 저장하면 문자로 인식되어 대소관계의 계산 방법이 달라진다.

## 4. ORDER BY는 테이블에 영향을 주지 않는다.

- 클라이언트로 행 순서를 바꾸어 결과를 반환하는 것뿐, 저장장치에 저장된 데이터의 행 순서를 변경하는 것이 아니다.
- `SELECT`명령은 데이터를 검색하는 명령이며 테이블의 데이터를 참조만 할 뿐 변경하지는 않는다.

# 복수의 열을 지정해 정렬하기

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명1 [ASC|DESC], 열명2 [ASC|DESC]...
```

상품코드와 해당 상품의 하위번호까지 함께 고려했을 때 비로소 하나의 행을 특정지을 수 있는 명세서등 복수의 열을 지정해 정렬해야 할때 사용

## 1. 복수 열로 정렬 지정

같은 값을 가진 행의 순서나 `ORDER BY`를 생략 했을때의 순서는 일정하지 않다. 따라서 언제나 같은 순서로 결과를 얻고 싶다면 반드시 `ORDER BY`구로 순서를 지정해야 한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b24fed7-aa5c-4803-88e8-8771bcb3dc89/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5b24fed7-aa5c-4803-88e8-8771bcb3dc89/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T170519Z&X-Amz-Expires=86400&X-Amz-Signature=0733de02990efef804612e74bacb66e60887aa457649f839c75c99c68e10024c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

위 테이블은 `a`와 `b`모두 `INTEGER`형이며 `b`열은 `a`열의 하위 번호이다 이를 `a`열만을 이용하여 '1-1, 1-2, 1-3, 2-1, 2-2'와 같이 정렬하려 한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed8023d9-2bea-4995-bec4-305e932cb883/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ed8023d9-2bea-4995-bec4-305e932cb883/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T170547Z&X-Amz-Expires=86400&X-Amz-Signature=f8544045cc2a5e7186cf2ea9f2cb8b2fc04c287e5fd1e354e307ec603e6601a7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*`a`열만으로 정렬하기*

`a`열은 순서대로 정렬되었으나 `b`열의 정렬 순서는 일정하지 않다. 원하는대로 정렬하기 위해서는 `**ORDER BY`구에 복수 열을 지정해야 한다.**

### ORDER BY로 복수 열 지정하기

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명1 [ASC|DESC], 열명2 [ASC|DESC]...
```

- `,`로 열명을 구분해 지정할 수 있다.
- 정렬 순서는 지정한 열명의 순서를 따른다.
- 값이 같아 순서를 결정할 수 없는 경우에는 다음으로 지정한 열명을 기준으로 정렬한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3cfb03c7-1aaf-417f-a52d-ec807233589f/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3cfb03c7-1aaf-417f-a52d-ec807233589f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T170640Z&X-Amz-Expires=86400&X-Amz-Signature=d9095a051bac85b9b80563a91b86abb73af4a9c0b217e585a3926b531c766c5f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*`a`열과 `b`열로 정렬하기*

> ⚠️ 위 예제에서 `a`와 `b`의 순서를 바꾸면
>
![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2a223c51-3ebf-46ec-99e9-91b6fb4d3741/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2a223c51-3ebf-46ec-99e9-91b6fb4d3741/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T170722Z&X-Amz-Expires=86400&X-Amz-Signature=8bbb4f923e3c8888f1f4df954c5aa7bcf174e0bba036707428d0e743082c613e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
>
*`b`열과 `a`열로 정렬하기*
>
`b`열에서 값의 크기 순서대로 정렬되고, 값이 같은 부분은 `a`열로 정렬된다.

## 2. 정렬방법 지정하기

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명1 [ASC|DESC], 열명2 [ASC|DESC]...
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3c0a89f5-4903-4af6-b617-f446b2acd68c/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3c0a89f5-4903-4af6-b617-f446b2acd68c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T171720Z&X-Amz-Expires=86400&X-Amz-Signature=5f7c19596f76ae0be4d8a3507cc29bca298afe25a48b6ed4fb3a7ee0bf70a87d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*`a`열은 `ASC`로 `b`열은 `DESC`로 정렬*

- 복수 열을 지정한 경우에도 각 열에 대해 개별적으로 정렬방법을 지정 할 수 있다.
- 복수 열을 지정하는 경우에도 정렬방법을 생략하면 기본값은 `ASC`가 된다.

    💡`ORDER BY a DESC, b`

## 3. NULL 값의 정렬순서

- `ORDER BY`로 지정한 열에서 `NULL`값을 가지는 행은 가장 먼저 표시되거나 가장 나중에 표시된다.
- MySQL에서는 `NULL`값을 가장 작은 값으로 취급하여 `ASC`에서는 가장 먼저, `DESC`에서는 가장 나중에 표시된다.

# 결과 행 제한하기 - LIMIT

```sql
SELECT 열명 FROM 테이블명 LIMIT 행수 [OFFSET 시작행];
```

## 1. 행수 제한

- `LIMIT`은 표준 SQL은 아니며 MySQL과 PostgreSQL에서 사용할 수 있다.
- `LIMIT`은 `SELECT`명령 마지막에 지정하는 것으로 `WHERE`나 `ORDER BY`뒤에 지정 한다.

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명 LIMIT 행수 [OFFSET 시작행];
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2bc7c8d-3713-4f48-af90-14f859271746/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a2bc7c8d-3713-4f48-af90-14f859271746/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T171815Z&X-Amz-Expires=86400&X-Amz-Signature=fdc2ea3d2ad67c01b89152d05ce25a7155e92939c7d4ac9712336712b625d9ed&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- `LIMIT`으로 지정하는 것은 최대 행수이다.
- 만약 테이블에 하나의 행만 있다면 `LIMIT 3`으로 지정해도 1개의 행이 반환된다.

### 정렬한 후 제한하기

`LIMIT 3`과 동일한 결과를 얻기 위해 `WHERE no <= 3`과 같은 조건식을 붙인다면 동일한 결과를 얻을 수 있으나 `**ORDER BY`로 정렬한 뒤 결과를 얻고자 한다면 내부 처리 순서가 달라 원하는 결과를 얻을 수 없다.**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/016738bb-d9f0-43ec-9862-597e9d41a059/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/016738bb-d9f0-43ec-9862-597e9d41a059/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T171900Z&X-Amz-Expires=86400&X-Amz-Signature=274c1317ae66e20cc443c22074ab79cc09cb6ecb32e4fbeef9d25614008c422e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*테이블을 정렬 후 `LIMIT 3`으로 상위 3건만 취득하기*

### LIMIT을 사용할 수 없는 데이터베이스에서의 행 제한

- SQL Server : `TOP`

    💡`SELECT TOP 3 * FROM sample33;`

- Oracle : `ROWNUM`

    💡`SELECT * FROM sample33 WHERE ROWNUM <=3;`

## 2. 오프셋 지정

- n번째 행부터 라는 표현은 결과값으로 부터 데이터를 취득할 위치를 가리키는 것으로, `LIMIT`구에 `OFFSET`으로 지정할 수 있다.
- `OFFSET`은 생략 가능하며 기본값은 0이다.
- 시작할 행 - 1로 지정한다.

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명 LIMIT 행수 OFFSET 위치;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c229ba5-1e4f-4b00-8550-d440e48c8183/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6c229ba5-1e4f-4b00-8550-d440e48c8183/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T171933Z&X-Amz-Expires=86400&X-Amz-Signature=e1ffbcec506527ed10712b7f878e607a27e9c5b5435f5908c1c0ef75a2855563&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*`LIMIT 3 OFFSET 3`으로 데이터 표시*

# 수치 연산

## 1. 사칙 연산

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/932dee5f-8bc6-4681-ab1d-f3cf0eebcc54/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/932dee5f-8bc6-4681-ab1d-f3cf0eebcc54/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172011Z&X-Amz-Expires=86400&X-Amz-Signature=5774f36caa8adbe60eda316e2bc007c02e2b567bfad29bf3927650ef52e3cf48&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- `%` 대신 `MOB`함수를 사용하는 경우도 있다.

### 연산자의 우선 순위

- 곱셈, 나눗셈, 나머지 → 덧셈, 뺄셈
- 왼쪽 → 오른쪽

## 2. SELECT 구로 연산하기

```sql
SELECT 식1, 식2 ... FROM 테이블명
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/994d3cbb-33e7-4673-b37b-41b3c0ffd0ff/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/994d3cbb-33e7-4673-b37b-41b3c0ffd0ff/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172041Z&X-Amz-Expires=86400&X-Amz-Signature=7f3fce0dd40be23af01bcde4b2b73a79a26776762212b0ea2f5133d5bcbf5539&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*가격 * 수량으로 금액 계산 하기*

## 3. 열의 별명

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/262723d2-2724-468a-8845-c8d05076eb18/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/262723d2-2724-468a-8845-c8d05076eb18/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172104Z&X-Amz-Expires=86400&X-Amz-Signature=4d6730a3b40a2e03af675f97befb27bbbfe9b3c3e35954e14f6314a8e7fe9fcb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 예약어 `AS`를 사용해 지정
- 복수의 식이 존재 할 경우 각각의 식에 별명을 붙일 수 있다.
- MySQL에서는 별명을 중복해서 지정해도 에러는 발생하지 않지만 프로그래밍 언어에서 결과값의 처리 방식에 따라 문제가 발생할 수도 있다. 따라서 **기본적으로 중복되지 않도록 지정 한다.**
- `AS`는 생략 가능하다
- 영어, 숫자, 한글 등으로 지정 가능하다.
- 단, 한글로 지정할 경우 Doublequote(`" "`)를 사용해 지정한다.

    💡`SELECT price * quantity "금액" FROM sample34;`

- `" "`로 지정하면 테이터베이스 객체의 이름이라고 간주한다.

    💡`' '`는 문자열 상수

- 예약어도 `" "`로 지정하면 사용 가능

    💡`SELECT price * quantity "SELECT" FROM sample34;`

- 숫자로 시작하는 별명은 사용 불가능하다.

    💡단, MySQL에서는 `" "`로 묶으면 사용 가능하나 사용하지 않는것이 좋다. 또한 Oracle에선 불가능하다.

## 4. WHERE 구에서 연산하기

`SELECT`구에서 `amount`라는 별멍을 붙였으므로 `WHERE`구에서도 `amount`로 지정하면 되지 않을까 생각할 수도 있지만 실제로 `SELECT price * quantity "SELECT" FROM sample34 WHERE**amount>=2000**;` 실행해보면 다음과 같은 에러가 발생한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d90a0da0-d524-4400-bfe9-67df832471a1/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d90a0da0-d524-4400-bfe9-67df832471a1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172155Z&X-Amz-Expires=86400&X-Amz-Signature=90cf5a5cccff9934dfa0b3d7b443a51bb7ee8054d4ed6990c789062f2e731bfd&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

이는 **내부처리 순서에 의해 불가능**하다.

### WHERE 구와 SELECT 구의 내부처리 순서

- `WHERE` → `SELECT`
- 별명은 `SELECT`구문을 내부 처리 할때 붙여진다. 즉, `WHERE`구의 처리는 `SELECT`구보다 선행되므로 `WHERE`구에서 사용한 별칭은 아직 내부적으로 지정되지 않은 상태라 에러가 발생하게 된다.
- 따라서, `**SELECT`구에서 지정한 별명은 `WHERE`구 안에서 사용할 수 없다.**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/30e049df-fd7f-4e06-9dce-a026658e30ab/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/30e049df-fd7f-4e06-9dce-a026658e30ab/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172232Z&X-Amz-Expires=86400&X-Amz-Signature=d95da87732987c02d0f4d6f2eabbee94036cfc086dbbb7ff2b19ec5efeb878ba&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*`WHERE`구에서 금액을 계산하고 2000원 이상인 행 검색하기*

## 5. NULL 값의 연산

- 프로그래밍 언어에서는 `NULL`이 0으로 처리되지만 S**QL에서는 `NULL`값이 0으로 처리되지 않는다.**
- `NULL + 1`의 결과값은 1이 아닌 `NULL`이다.

> ⚠️ 아래의 결과 값은 모두 `NULL`이다.
>
> - `NULL + 1`
> - `1 + NULL`
> - `1 + 2 + NULL`
> - `1 / NULL`
>
*통상적인 연산에서는 0을 1로 나누면 `division by zero`에러가 발생하지만 `1 / NULL`의 결과는 계산해도 0으로 처리되지 않아 에러가 발생하지 않고 `NULL`이 된다.*

## 6. ORDER BY 구에서 연산하기

```sql
SELECT 열명, 식1 FROM 테이블명 ORDER BY 식1 [ASC|DESC]
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb81fbb5-6a78-4a82-a2f4-9327ce06ccef/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cb81fbb5-6a78-4a82-a2f4-9327ce06ccef/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172345Z&X-Amz-Expires=86400&X-Amz-Signature=a9706ca5477ba2f2d3bf30523cb6bf5456e2ef9ddaa9bb0e3882cd77d4252712&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- `ORDER BY`는 서버에서 내부적으로 가장 나중에 처리된다.
- 즉, `SELECT` 구 보다 나중에 처리되기 때문에 `**SELECT`구에서 지정한 별명을 `ORDER BY`에서도 사용할 수 있다.**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a00e7319-8bd1-4dab-b2e4-01c13f5cafaa/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a00e7319-8bd1-4dab-b2e4-01c13f5cafaa/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172411Z&X-Amz-Expires=86400&X-Amz-Signature=e4a64b0282bf7cd43373c5754887b80a3de57de8c851ffd8810b092572d621b3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> ⚠️ WHERE, SELECT, ORDER BY의 처리 순서
>
![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36f3de8a-85b0-4e54-9127-bd9b63eb72cb/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/36f3de8a-85b0-4e54-9127-bd9b63eb72cb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172435Z&X-Amz-Expires=86400&X-Amz-Signature=9f029b4059947c834d9a25f58b954be478fbcee358f3df87b7e56cc7394e75d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## 7. 함수

```sql
함수명(인수1, 인수2 ... )
```

- 함수는 함수명에 따라 연산 방법이 결정된다.
- 계산 대상을 인수로 지정한다.
- 함수명 뒤에 괄호로 묶어 표기한다.
- 대부분의 함수는 1개 이상의 인수를 가진다.
- 인수는 `Parameter`라고도 부른다.
- 함수는 결과값을 반환하며 이를 '함수의 반환값'이라 한다.

## 8. ROUND 함수

**반올림**을 하는 함수

```sql
SELECT 열명, ROUND(열명) FROM 테이블명;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b1af78f5-be06-4fb3-bec2-c7ac02814131/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b1af78f5-be06-4fb3-bec2-c7ac02814131/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172507Z&X-Amz-Expires=86400&X-Amz-Signature=5b0e04e8c20c493d0550713f71b740d86a0d6c739e79611d6a904227764f3252&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> ⚠️ DECIMAL 형
>
위 예제의 `amount`열은 `DECIMAL`형으로 정의되어 있는데 `DECIMAL`형은 정수부와 소수부의 자릿수를 지정할 수 있는 자료형이다. 즉, 소수점을 포함하는 수치를 저장하는 자료형이다.

### 반올림 자릿수 지정

- `ROUND`함수는 기본적으로 소수점 첫째 자리를 기준으로 반올림한 값을 반환한다.
- `ROUND`함수의 두번째 인수로 반올림 할 자릿수를 지정할 수 있으며 생략할 경우는 0으로 간주되어 소수점 첫째 자리를 반올림 한다.
- `1`을 지정하면 소수점 둘째 자리를 반올림 한다.
- 음수로 지정해 정수부의 반올림할 자릿수도 지정할 수 있다.
- `-1`을 지정하면 1단위, `-2`을 지정하면 10단위를 반올림할 수 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/389b4664-5f6b-4717-bb91-d0ca14eac800/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/389b4664-5f6b-4717-bb91-d0ca14eac800/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172538Z&X-Amz-Expires=86400&X-Amz-Signature=a92f92bca4af5a74b4e7ad58dd1c8a983d7d3ac28164ac89ca4f1fcef495dce6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> ⚠️ 버림 : `TRUNCATE`

# 문자열 연산

## 1. 문자열 결합

- 문자열 결합은 데이터베이스 제품마다 방언이 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f25636b-8367-4ad7-a000-54eb46072d55/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0f25636b-8367-4ad7-a000-54eb46072d55/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172617Z&X-Amz-Expires=86400&X-Amz-Signature=8e31d1158c8763ed8beac6cf2929fe97410d7f3d4004e3258cb2ba13f1f32fd2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 2개의 열 데이터를 모아서 1개의 열로 처리하고 싶은 경우에 자주 사용

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fad16b21-271e-4f24-90a3-aa5e5aee5dab/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fad16b21-271e-4f24-90a3-aa5e5aee5dab/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172638Z&X-Amz-Expires=86400&X-Amz-Signature=5b93e1bb38ee4ee0436dd95611ba685c32c7bb98e741a9118aa6f9064a92d58a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*문자열 결합으로 단위를 연결해 결과 얻기*

여기서 `unit`열은 문자열형이고 `quantity`는 `INTEGER`형이다. 이때 `CONCAT`함수를 이용해 두 열을 결함하면 수치 데이터인 `quantity`도 문제없이 연산 할 수 있다. 단, 문자열로 결합한 결과는 문자열형이 된다.

## 2. SUBSTRING 함수

```sql
# 앞 4자리(년) 추출
SUBSTRING('20140125001', 1, 4) -> '2014'

# 5째 자리부터 2자리(월) 추출
SUBSTRING('20140125001', 5, 2) -> '01'
```

- 문자열의 일부분을 계산해서 반환해주는 함수이며 **문자열을 추출한다.**

## 3. TRIM 함수

```sql
# TRIM으로 스페이스 제거하기
TRIM('ABC   ') -> 'ABC'
```

- 문자열의 앞뒤로 여분의 Space가 있을 경우 이를 제거해준다.
- 문자열 도중에 존재하는 Space는 제거되지 않는다.
- 인수를 지정하는 것으로 Space 이외의 문자를 제거할 수도 있다.

## 4. CHARACTER_LENGTH 함수

- 문자열의 길이를 계산해 돌려주는 함수이며 **문자열의 길이를 계산한다.**
- 문자 단위로 계산되어 수치로 반환
- 함수명은 `CHAR_LENGTH`로 줄여서 사용 가능

> ⚠️ `OCTET_LENGTH` 함수
>
> - 문자열의 길이를 **바이트 단위로 계산해 돌려주는 함수**
> - 문자 하나의 데이터가 몇 Byte의 저장공간을 필요로 하는지 '인코드 방식'에 따라 결정 된다.
> - 문자를 수치화(인코드) 하는 방식에 따라 필요한 저장공간의 크기가 달라진다
> - `VARCHAR`형의 최대 길이 역시 Byte 단위로 지정한다.
> - **문자세트 (Character set)에 따라 길이가 문자 수로 간주되기도** 하니 주의할 필요가 있다.

### 문자세트

- 알파벳은 반각문자, 한글은 전각문자
- 저장용량은 전각문자 쪽이 더 크다.
- 반각의 알파벳이나 숫자, 기호는 `ASCII`문자 라고 불린다. 한글의 경우 `EUC-KR`, `UTF-8`등의 인코드 방식을 주로 사용
- 인코드 방식은 데이터베이스나 테이블을 정의할 때 변경 할 수 있고 이를 `RDBMS`에서는 **'문자세트'**라고 부른다.
- **한 문자가 몇 Byte인지는 쓰이는 문자세트에 따라 다르다.**
- 특히, `OCTET_LENGTH`함수를 사용하는 경우는 문자 수가 아닌 Byte단위로 길이를 계산하므로 주의가 필요

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70eff676-ca1a-4552-9565-672225d2f503/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/70eff676-ca1a-4552-9565-672225d2f503/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172732Z&X-Amz-Expires=86400&X-Amz-Signature=ef2a7e793307f9d679603c593a23584089cee9a0fb5aec3fc5b23e5d90ab3467&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- `EUC-KR`에서 `ASCII`문자는 1Byte, 한글은 2Byte의 용량을 가진다.
- `UTF-8`에서 `ASCII`문자는 1Byte, 한글은 3Byte의 용량을 가진다.
- **문자열 조작 함수로 문자 단위가 아닌 Byte단위로 지정할 경우 문자세트에 주의할 필요가 있다.**

# 날짜 연산

## 1. SQL에서의 날짜

- 날짜나 시간 데이터는 수치 데이터와 같이 사칙 연산을 할 수 있다.
- 날짜시간 유형의 데이터를 반환하는 경우도 있고 기간(간격)의 차를 나타내는 기간형(`interval`)데이터를 반환하는 경우도 있다.
- 기간형은 '10일간', '2시간10분'과 같이 시간의 간격을 표현

### 시스템 날짜

- `CURRENT_TIMESTAMP`라는 함수로 기준 시간을 표시한다.

    💡Oracle : `SYSDATE`

    💡SQL Server : `GETDATE`

- 인수는 지정할 필요 없으며 따라서 `()`를 사용하지 않는 특수한 함수다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98fa2802-513f-48b5-a4bc-d31328b18398/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/98fa2802-513f-48b5-a4bc-d31328b18398/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172825Z&X-Amz-Expires=86400&X-Amz-Signature=d07ca16dbc81e66fc08b116ece14050111681061ce0b3543234651f4a9b1e328&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

`CURRENT_TIMESTAMP`로 시스템 날짜 확인

위 예제에서는 `FROM`구를 생략하였으나 Oracle같은 전통적인 데이터베이스에서는 생략할 수 없다.

### 날짜 서식

- 앞서 시스템 날짜를 저장할 경우에는 `CURRENT_TIMESTAMP`함수를 사용했으나 **임의의 날짜를 저장하고 싶을 경우에는 직접 날짜 데이터를 지정해야 한다.**
- 날짜 서식은 국가별로 다르다.

> ⚠️ 국가별 날짜 서식
>
> - `2014/01/25` : 🇰🇷, 🇯🇵
> - `2014-01-25` : 🇰🇷, 🇯🇵
> - `25 Jan 2014` : 🇺🇸

## 2. 날짜의 덧셈과 뺄셈

- 날짜시간형 데이터는 기간형 수치데이터와 덧셈 및 뺄셈을 할 수 있다.
- 예를들어 특정일로 부터 1일 후를 계산할 수도 있고 1일 전을 계산할 수도 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2fc51a19-4981-487c-af8a-6bfb0dc84cac/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2fc51a19-4981-487c-af8a-6bfb0dc84cac/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T173031Z&X-Amz-Expires=86400&X-Amz-Signature=396d382d3b7bd2743ffb26c0ae738189c20c65c64a51e9d6dbb13e8445866c68&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*시스템 날짜에서 1일 후와 1일전 계산*

> ⚠️ 여기서 `INTERVAL 1 DAY`는 '1일' 이라는 의미의 기간형 상수이다.

### 날짜형 간의 뺄셈

- Oracle : `'2014-02-28' - '2014-01-01'`
- MySQL : `DATEDIFF('2014-02-28', '2014-01-01')`

# CASE 문으로 데이터 변환하기

```sql
CASE WHEN 조건식1 THEN 식1
	[WHEN 조건식2 THEN 식2 ... ]
	[ELSE 식3]
END
```

## 1. CASE 문

- 기존 연산자나 함수만으로는 처리할 수 없는 것을 처리하고 싶을때

    💡 `NULL`값을 0으로 간주하고 계산할때

```sql
CASE WHEN 조건식1 THEN 식1
	[WHEN 조건식2 THEN 식2 ... ]
	[ELSE 식3]
END
```

- `WHEN`절에는 참과 거짓을 반환하는 조건식을 기술
- 조건을 만족하여 참이되는 경우는 `THEN`절에 기술한 식이 처리
- `WHEN`절의 조건식을 차례로 평가해 나가다가 가장 먼저 조건을 만족한 `WHEN`절과 대응하는 `THEN`절 식의 처리결과를 `CASE`문의 결과값으로 반환
- 그 어떤 조건식도 만족하지 못한 경우에는 `ELSE`절에 기술한 식이 실행된다.
- `ELSE`는 생략 가능하며 생략한 경우는 `ELSE NULL`로 간주된다.

### COALESCE

```sql
SELECT a, COALESCE(a,0) FROM sample37;
```

- `NULL`값을 변환하는 함수
- 해당열에 `NULL`이 아니면 그대로 출력하고 그렇지 않으면 0이 출력된다.

## 2. 또 하나의 CASE 문

- **디코드** : 문자화 하는것
- **인코드** : 수치화 하는것

### 검색 CASE

```sql
CASE WHEN 조건식 THEN 식 ...
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5d04bc81-e2ca-48d3-8c23-dec006ad6717/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5d04bc81-e2ca-48d3-8c23-dec006ad6717/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T172943Z&X-Amz-Expires=86400&X-Amz-Signature=c4b12c81f1d98e5bb53862a16c0afb0de1b817b6afa57104f77757a1c0965c8c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*검색 CASE를 이용한 성별 코드 변환하기*

### 단순 CASE

```sql
CASE 식1 
	WHEN 식2 THEN 식3
		[WHEN 식4 THEN 식5 ... ]
		[ELSE 식6]
END
```

- 단순 CASE에서는 CASE뒤에 식을 기술하고 WHEN뒤에 조건식이 아닌 식을 기술
- 식 1의 값이 `WHEN`의 식2의 값과 동일한지 비교하고 값이 같다면 식3의 값이 `CASE`문의 결과값이 된다.
- 값이 같지 않으면 그 뒤에 기술한 `WHEN`절과 비교하고 그래도 일치하는 `WHEN`절이 없다면 `ELSE`절이 적용

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a467d4db-8182-45b9-8a81-b5329c52c07a/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a467d4db-8182-45b9-8a81-b5329c52c07a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T173146Z&X-Amz-Expires=86400&X-Amz-Signature=4ef5b46fccae1c2960a1dd931a001ab7371312e49221bfcc5b1b70ec26584144&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*단순 CASE를 이용한 성별 코드 변환하기*

> 검색 CASE의 경우 `WHEN`에 `a=1, a=2`처럼 식을 상세하게 기술해야 하지만 단순 CASE에서는 CASE문에서 비교할 항목인 `a`를 따로 지정하므로 `WHEN`에는 `1, 2`처럼 비교할 값만 기술할 수 있다.

## 3. CASE를 사용할 경우 주의사항

- `CASE`는 어디에나 사용할 수 있다.

    💡 `WHERE`구에서 조건식의 일부로 사용될 수도 있고 `ORDER BY`구나 `SELECT`구에서도 사용할 수 있다.

### ELSE 생략

- `**ELSE`를 생략하면 `ELSE NULL`이 된다.**
- 따라서 생략하지 않는 편이 낫다.

### WHEN에 NULL 지정하기

- `WHEN NULL THEN '데이터 없음'`과 같이 지정해도 문법적으로 문제는 없으나 정상적으로 처리되지 않는다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7918ab4b-581b-4285-9561-b5c8cb8bc20e/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7918ab4b-581b-4285-9561-b5c8cb8bc20e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T173217Z&X-Amz-Expires=86400&X-Amz-Signature=a15827c86f080fc2352825ac4137597eb3e708e4398f68ce5d15a8670d04f3c2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*단순 CASE문에서 `WHEN`절에 `NULL`지정하기*

위 예제에서는 다음과 같은 순서로 조건식을 처리한다.

1. `a=1`
2. `a=2`
3. `a=NULL` → 비교 연산자로는 NULL값과 같은지 아닌지 비교 할 수 없음

따라서 값이 `NULL`이라고 해도 `a=NULL`이 참이 되지 않는다. 따라서 `'데이터 없음'`은 `'미지정'`이라는 결과값이 나온다.

`**NULL`값인지 아닌지를 판정하기 위해서는 `IS NULL`을 사용해야 하며 단순 CASE문 특성상 `=`연산자로 비교하는 만큼 검색 CASE문을 사용해야 한다.**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/345c3849-8287-4639-8365-3144e10d5f2d/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/345c3849-8287-4639-8365-3144e10d5f2d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200729T173242Z&X-Amz-Expires=86400&X-Amz-Signature=beab0a2ba1aaca8c8573703bf5c9ebe70d40b81be55216f2aefc659c49245a6d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*검색 CASE문으로 `NULL`판정하기*

### DECODE NVL

- Oracle에는 이 같은 디코드를 수행하는 `DECODE`함수가 내장되어 있다.
- 다만 `DECODE`함수는 Oracle에서만 지원하는 함수다.
- `NULL`값을 반환 함수
    - Oracle : `NVL`
    - SQL Server : `ISNULL`
    - 표준 SQL : `COALESCE`

# Question

### Q1. ORDER BY구에 의해 정렬되는 것

- 행

### Q2. NULL이 포함된 식의 연산결과

- `NULL`

### Q3. SELECT구로 식에 붙일 수 잇는 것

- 별명
