---
title: TIL.23) Part 2 - 테이블에서 데이터 검색
date: 2020-07-28 17:07:42
category: SQL
thumbnail: '../TIL/images/sql.png'
draft: false
---

![](../TIL/images/sql.png)

# Hello World 실행하기

```sql
SELECT * FROM 테이블명;
```

## 1. 'SELECT * FROM 테이블명' 실행하기

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7146ad91-65bd-42ef-a9e5-f26d2d50981f/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7146ad91-65bd-42ef-a9e5-f26d2d50981f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T154543Z&X-Amz-Expires=86400&X-Amz-Signature=3825b66769080c76becd10640ef9cd95442e47c5e050bab5e67e3695291a3483&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- `SELECT`와 `*`그리고 `FROM`사이에는 Space를 넣어 구분
- `FROM`과 `sample21`사이에도 Space가 필요
- `;`은 명령의 마지막을 나타낸다.

> 만약 `;`을 붙이지 않고 명령 실행 시 아직 입력 중인 것으로 간주 된다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4fdee886-307e-4de2-9c9f-a66e77a27c49/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4fdee886-307e-4de2-9c9f-a66e77a27c49/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T154635Z&X-Amz-Expires=86400&X-Amz-Signature=8ee838124328c353d9f737b01da8b93e89591aadfb1fb364cbfd91ec82c09075&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## 2. SELECT 명령 구문

- `DML`에 속하는 명령
- 데이터베이스에서 데이터를 읽어 올 수 있다.
- **'질의'** 또는 **'쿼리'**라 불린다.

```sql
SELECT * FROM sample21;
```

- 여기서 맨 앞의 `SELECT`는 SQL 명령의 한 종류로 `SELECT`명령을 실행하라는 의미
- 애스터리스크 (`*`)는 '모든 열'을 의미
- `FROM`은 처리 대상 테이블을 지정하는 키워드
- `SQL`명령은 키워드에 의해 '구'라는 단위로 나눌 수 있다.

    💡위 명령은 `SELECT`구 와 `FROM`구 라는 단위로 나눌 수 있다.

## 3. 예약어와 데이터베이스 객체명

- `SELECT`와 `FROM`이 구를 결정하는 키워드이자 **예약어**이다.
- 데이터베이스에는 테이블 외에 다양한 데이터를 저장하거나 관리하는 '어떤 것'을 만들 수 있으며 이를 **데이터베이스 객체**라 한다.
- 같은 이름으로 다른 데이터베이스 객체를 만들 수 없다.

    💡위 예제에서 `sample21`은 기존 데이터베이스 내에 존재하는 테이블이므로 동일한 이름인 `sample21`이 라는 동일한 이름의 새 테이블을 만들 수 없다.

- 만약 같은 이름으로 객체 생성 시 '이미 테이블이 정의되어 있다'는 에러가 발생하게 된다.
- 데이터베이스 객체명에는 예약어와 동일한 이름을 사용할 수 없다.

    💡예약어인 `SELECT`과 이름이 같은 테이블을 생성 할 수 없다.

### 대소문자 구별

- 예약어와 데이터베이스 객체명은 대소문자를 구별 하지 않는다.

```sql
select * from sample21;
Select * From Sample21;
SELECT * FROM SAMPLE21;
```

*위 예제는 모두 동일하게 동작 한다.*

- `SQL`명령과 달리 많은 데이터베이스 제품들은 데이터의 대소문자를 구별한다.

## 4. Hello World를 실행한 결과 = 테이블

- 표 형식의 데이터는 **'행(레코드)'**과 **'열(컬럼/필드)'**로 구성된다.
- 열마다 이름이 지정되어 있다.
- 행과 열이 만나는 부분을 '셀'이라고 한다.
- 셀에는 하나의 데이터값이 저장되어 있다.
- 숫자만으로 구성된 데이터를 **'수치형'** 데이터라고 한다.
- 문자로 구성된 데이터를 **'문자열형'** 데이터라고 한다.
- 날짜와 시각을 나타내는 데이터를 **'날짜시간형'** 데이터라고 한다.
- 열은 하나의 자료형만 가질수 있다.

    💡수치형의 열에 문자형의 데이터를 저장 할 수 없음

## 5. 값이 없는 데이터 = NULL

- 특별한 데이터 값
- `NULL`이라는 데이터가 저장되어 있는 것이 아닌 **'아무것도 저장되어 있지 않은 상태'**

# 테이블 구조 참조하기

## 1. DESC

```sql
DESC 테이블명;
```

- 테이블에 어떤 열이 정의되어 있는지 알 수 있다.
- `Field` : 열의 이름이 표시된다.
- `Type` : 해당 열의 '자료형'
- `Null` : `NULL`값을 허용할 것인지 아닌지를 나타내는 **제약사항**
- `Key` : 해당 열이 '키'로 지정되어 있는지를 나타냄

    💡`Key`: 행을 특정하기 위해 사용할 수 있는 열

- `Default` : 그 열에 주어진 '기본값' 즉

## 2. 자료형

### INTEGER

- 수치형의 하나
- 정수값을 저장할 수 있는 자료형
- 소수점은 포함할 수 없다.

### CHAR

- 문자열형의 하나
- 문자열을 저장할 수 있는 자료형
- 열의 최대길이를 지정 해야 한다. (`CHAR(10)`)
- 언제나 고정된 길이로 데이터가 저장
- '고정 길이 문자열' 자료형
- 최대 길이보다 작은 문자열을 저장할 경우 공백문자로 나머지를 채운 후 저장

### VARCHAR

- 문자열을 저장할 수 있는 자료형
- `CHAR`와 같이 최대 길이를 지정해야 한다.
- 단, **데이터 크기에 맞춰 저장공간의 크기도 변경된다.**
- '가변 길이 문자열' 자료형

### DATE

- 날짜값을 저장할 수 있는 자료형
- 연월일의 데이터를 저장

### TIME

- 시간을 저장할 수 있는 자료형
- 시분초의 데이터를 저장

# 검색 조건 지정하기

- 행을 선택할 때는 `WHERE`구를 사용한다.
- 열을 선택할 때는 `SELECT`구를 사용한다.

## 1. SELECT 구에서 열 지정하기

```sql
SELECT 열1, 열2 ... FROM 테이블명;
```

- 열은 위 처럼 Comma(`,`)로 구분하며 여러 개를 지정 할 수 있다,
- 지정된 열만 결과값으로 표시된다.
- `SELECT FROM sample21;`과 같이 열을 전혀 지정하지 않으면 구문 에러가 발생한다.
- 존재하지 않는 열을 지정하면 구문 에러가 발생 한다.
- 테이블에서 열이 정의된 순서와 동일한 순으로 지정할 필요는 없다.
- 결과는 지정한 열의 순서대로 표시된다.
- 동일한 열을 중복해서 지정해도 무관하다. (`SELECT no, no ,no ,no FROM sample21;`)

## 2. WHERE 구에서 행 지정하기

```sql
SELECT 열 FROM 테이블명 WHERE 조건식;
```

- `WHERE`구는 `FROM`구의 뒤에 표기
- 예약어 `WHERE`뒤에 검색 조건을 표기
- 조건에 일치하는 행만 `SELECT`의 결과로 반환

### 구의 순서와 생략

- 구의 순서가 정해져 있어 바꿔 적을 수 없다.

    💡SELECT 구 → WHERE 구 → FROM 구의 순으로 적으면 **에러**

- `WHERE`처럼 생략 가능한 것도 있다.

### WHERE 구

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/74eb94db-42a4-4043-82ab-88a824af05d8/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/74eb94db-42a4-4043-82ab-88a824af05d8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T154749Z&X-Amz-Expires=86400&X-Amz-Signature=9b23f6a7e18ee06a83ce95fe1c22416487252631ed129eacc9858c43834a9dc0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 조건식 : 열과 연산자, 상수로 구성되는 식
- 조건식에 일치하는 행만 `SELECT`의 결과로 반환

### 조건식

- 위 예제에서 `no=2`는 조건식이다.
- `no`는 열의 이름
- `=`는 연산자로 서로 같은 값이면 참을, 같지 않으면 거짓을 반환하며 '비교 연산자'라 한다.
- 조건식에 일치하는 행이 전혀 없는 경우에는 아무것도 반환되지 않는다.

### 값이 서로 다른 경우 '<>'

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27e7fc8a-1b1a-4954-9c3d-45544204b5d2/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/27e7fc8a-1b1a-4954-9c3d-45544204b5d2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T154818Z&X-Amz-Expires=86400&X-Amz-Signature=86289edc57feecd77444ec0b386f0647b1a026627719e8923a40f2a00e124be5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- `<>`연산자는 값이 서로 다른 경우 참이 되는 조건식

## 3. 문자열형의 상수

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f196b5d5-9914-44cb-8359-9ac1251fc247/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T154908Z&X-Amz-Expires=86400&X-Amz-Signature=f31fd91b0cd1ef9d8d2450b9f56c5498df2ff8f079dbec5617f99850c893ec95&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- **싱글쿼트(`' '`)로 둘러싸 표기**
- 날짜시간형의 경우에도 `' '`로 둘러싸 표기하며 이때 연월일은 하이픈(`-`)으로 구분하고 시각의 시분초는 콜론(`:`)으로 구분하여 표기
- 조건으로 지정할 값도 그 자료형에 맞춰 지정해야 한다.
- 자료형에 맞게 표기한 상수값을 `Literal`이라 한다.

## 4. NULL값 검색

- `=`연산자로 검색할 수 없다.

### IS NULL

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2f2fde3-e195-4404-9e06-6735f5f524af/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c2f2fde3-e195-4404-9e06-6735f5f524af/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T154937Z&X-Amz-Expires=86400&X-Amz-Signature=a2cb0883382c90142f404cdcf37b402ac3be9477d8ce2589e464550f46114e56&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- `IS NULL`을 사용하여 검색
- 반대로 `NULL`이 아닌 값을 행을 검색할 때는 `IS NOT NULL`

## 5. 비교 연산자

- `=`: 좌변과 우변의 값이 같을 경우 참이 된다.
- `<>`: 좌변과 우변의 값이 같지 않을 경우 참이 된다.
- `>`: 좌변의 값이 우변의 값보다 클 경우 참이 된다. 같거나 작을 경우는 거짓이 된다.
- `>=`: 좌변의 값이 우변의 값보다 크거나 같을 경우 참이 된다. 작을 경우는 거짓이 된다.
- `<`: 좌변의 값이 우변의 값보다 작을 경우 참이 된다. 같거나 클 경우는 거짓이 된다.
- `<=`: 좌변의 값이 우변의 값보다 작거나 같을 경우 참이 된다. 클 경우는 거짓이 된다.

# 조건 조합하기

```sql
조건식1 AND 조건식2;
조건식1 OR 조건식2;
NOT 조건식1
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e41c030-025a-44d9-9203-4263ab62128c/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1e41c030-025a-44d9-9203-4263ab62128c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155014Z&X-Amz-Expires=86400&X-Amz-Signature=e0354aa2429d1ab87a00ce3ef002cfe5c3bc74af5c9ae03603420a66cd4af1cb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## 1. AND로 조합하기

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3c5d8d17-c947-4f54-ab5b-51d4b5c5ae83/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3c5d8d17-c947-4f54-ab5b-51d4b5c5ae83/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155045Z&X-Amz-Expires=86400&X-Amz-Signature=fb7e2704a934186adc6ba4fb174f759ba1689f2741bf63ac7e4d25e7ea4f34c3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- **좌우의 식이 모두 참일 경우** AND연산자는 참을 반환한다.
- '및'에 해당
- '교집합'으로 계산할 수 있다.
- 논리곱을 계산하는 논리 연산자.

## 2. OR로 조합하기

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e5b38c1f-9c9b-48c2-8d62-f4c68ed7b32d/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e5b38c1f-9c9b-48c2-8d62-f4c68ed7b32d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155108Z&X-Amz-Expires=86400&X-Amz-Signature=ba6765ceff8b294cf02cb7524a6347e805173ce6478023225a57fa6a77ac6c2c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- **어느 쪽이든 조건을 만족하면 결과는 참이 된다.**
- '또는'에 해당
- '합집합'으로 계산할 수 있다.
- 논리합을 계산하는 논리 연산자

## 3. AND와 OR를 사용할 경우 주의할 점

- 'no'열의 값이 1 또는 2인 행을 추출하고 싶을 경우, 다음과 같은 조건식을 사용하면 **올바른 결과를 얻을 수 없다.**

```sql
SELECT * FROM sample24 WHERE no=1 OR 2;
```

- 상수 '2'는 논리연산에서 항상 참이 되기 때문에 결과적으로 모든 행을 반환
- 올바른 조건식은 다음과 같다.

```sql
SELECT * FROM sample24 WHERE no=1 OR no=2;
```

### AND와 OR를 조합해 사용하기

위 AND예제의 조건식 `SELECT * FROM sample24 WHERE a<>0 AND b<>0;`을 `OR`연산자를 활용하여 바꾸면 다음과 같이 될 수 있다.

```sql
SELECT * FROM sample24 WHERE a=1 OR a=2 AND b=1 OR b=2;
```

그러나 앞서 AND연산자 예시의 결과와는 다른 것을 확인 할 수 있다. 

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9db60c46-7b3b-402d-b3d8-71233f7ba3e0/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9db60c46-7b3b-402d-b3d8-71233f7ba3e0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155148Z&X-Amz-Expires=86400&X-Amz-Signature=767f85b0197769144bec15d340b1523a3b48083c86cd696e4e4f96a79a278e6d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

이는 **연산자의 계산 우선 순위가 다르기 때문**이다.

### 연산자의 우선 순위

- AND의 우선순위가 높다. 따라서 위 예제에서는 `a=2 AND b=1`이 먼저 계산된다.
- 즉, 위 예제는 결국 3개의 `OR`로 연결된 것과 같아진다.
    - `a=1`
    - `a=2 AND b=1`
    - `b=2`
- **괄호(`()`)를 통해 우선순위를 변경 할 수 있다.**

```sql
SELECT * FROM sample24 WHERE (a=1 OR a=2) AND (b=1 OR b=2);
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a772c3b6-3c3a-41c9-9134-3001808b65e9/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a772c3b6-3c3a-41c9-9134-3001808b65e9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155218Z&X-Amz-Expires=86400&X-Amz-Signature=94dffb44aea5c96b536bd980192e4df6574ec9ad668b58dc3646881f2402d5ea&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 일반적으로 `OR`조건식은 괄호로 묶어 지정하는 경우가 많다.

> ⚠️ AND는 OR에 비해 우선 순위가 높다.

## 4. NOT으로 조합

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a3fdc75-111d-49d9-ac2c-e428bbc1740c/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0a3fdc75-111d-49d9-ac2c-e428bbc1740c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155314Z&X-Amz-Expires=86400&X-Amz-Signature=754752e75d3e708dd595df14262ab0c1baccb95e74584ad15c0c0da665bd2f85&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 단항 연산자
- 조건식의 반대 값을 반환
- '~아닌', '~외에', '~를 제외한 나머지' 등의 조건을 지정할 경우 사용
- 집합에 포함되지 않는 나머지 부분을 결과로서 반환

# 패턴 매칭에 의한 검색

- 특정 문자나 문자열이 포함되어 있는지를 검색할 때 사용
- **'패턴 매칭'** 또는 **'부분 검색'**이라고 한다.

## 1. LIKE로 패턴 매칭하기

```sql
열명 LIKE '패턴'
```

- 열 값이 부분적으로 일치하는 경우에도 참이 반환된다.
- 왼쪽에는 매칭 대상을 지정하고 오른쪽에는 패턴을 문자열로 지정
- 패턴을 정의할 때는 `**%`나 `_`같은 메타문자를 사용한다.**
- 메타문자는 '와일드카드'라고 불리기도 한다.
- **임의의 문자 또는 문자열에 매치하는 부분을 지정하기 위해 쓰이는 특수문자**
- `%`는 임의의 문자열을 의미
- `_`는 임의의 문자 하나를 의미
- 메타문자를 전혀 정의하지 않아도 문제는 없지만, 완전 일치로 검색되므로 정의하는 의미가 없다.
- `*`는 `LIKE`에서는 사용 불가능

### Example

'SQL'이라는 문자를 포함한 데이터를 검색한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56aa7d51-4dcc-4c3f-9207-b21bcf303d8e/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/56aa7d51-4dcc-4c3f-9207-b21bcf303d8e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155350Z&X-Amz-Expires=86400&X-Amz-Signature=6433bb5dde0bce0abaa84613bc7932e55e9c09f28bdfdf34d48351893f3f4d7a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

하지만 `no`가 3인 행에도 `text`에 'SQL'이 포함되어 있으나 검색되지 않은걸 알 수 있는데 이는 시작문자가 'SQL'이 아니기 때문이다. 

따라서 문자열 도중에 존재하는 'SQL'을 검색하고자 한다면 다음과 같이 패턴을 바꿀 필요가 있다.

```sql
SELECT * FROM sample25 WHERE text LIKE '%SQL%';
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/378d84fb-f3f8-431e-84db-db7f1e12fbfe/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/378d84fb-f3f8-431e-84db-db7f1e12fbfe/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155414Z&X-Amz-Expires=86400&X-Amz-Signature=89159e198ba1c28bdd293e96eb5339b068089fd154d4e038645b1beeccbb2a67&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

여기서 핵심은 메타문자 %가 빈 문자열과도 매치 되는 것이다. `no`의 값 1번의 행의 `text`의 경우 'SQL'앞에 `%`와 매치되는 문자열은 없는데도 검색이 되었다.

> ⚠️ %는 임의의 문자열과 매치하며, 빈 문자열에도 매치한다.

### 전방일치, 중간일치, 후방일치

- **전방일치**
    - `SQL%`과 같이 문자열 앞쪽에 지정한 문자와 일치하는 것
    - 지정한 문자 뒤로 임의의 문자열이 존재
- **중간일치**
    - `%SQL%`과 같이 지정 문자열이 중간에 있는 것
    - 지정한 문자 앞뒤로 임의의 문자열이 존재
- **후방일치**
    - `%SQL`과 같이 문자열 뒤쪽에 지정한 문자와 일치하는 것
    - 지정한 문자 앞쪽에 임의의 문자열이 존재

## 2. LIKE로 %를 검색하기

- `\%`와 같이 `\`을 `%`앞에 붙인다.

```sql
WHERE text LIKE '%\%%';
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f2c240fe-65e4-416b-a50c-d410537528c1/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f2c240fe-65e4-416b-a50c-d410537528c1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200728T155521Z&X-Amz-Expires=86400&X-Amz-Signature=5e6f6507ea72ffb634ea0af1a455ec35c466f305a88198433b319203e36d57cc&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 맨 앞의 `%`는 메타문자
- 그 다음 `%`는 앞에 `\`가 있어 이스케이프 되어 일반 문자 '%"가 되었다.
- 맨 뒤의 `%`도 메타문자

> ⚠️  요약
>
>- '%' →`\%`
>- '\_' → `\_`

## 3. 문자열 상수 '의 이스케이프

- '를 2개 연속해서 기술하는 것으로 이스케이프 처리
- It's → `'It''s'`
- ' → `''''`
- 더 복잡한 패턴을 매칭하는 경우는 정규 표현식 (Regular Expression)을 사용하는 편이 낫다

# Question

### Q1. WHERE 구로 조건식을 지정하는 것?

- 행

### Q2. NULL 값을 가지는 행을 검색할 경우 사용하는 연산자

- `IS NULL`

### Q3. '나이가 20세인 여성'만 검색할 경우 사용하는 논리 연산자

- `AND`
