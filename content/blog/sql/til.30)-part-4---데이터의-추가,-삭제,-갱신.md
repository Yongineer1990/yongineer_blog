---
title: TIL.30) 데이터의 추가, 삭제, 갱신
date: 2020-08-04 03:08:31
category: SQL
thumbnail: '../TIL/images/sql.png'
draft: false
---

![](../TIL/images/sql.png)

# 행 추가하기 - INSERT

## 1. INSERT로 행 추가하기

```sql
INSERT INTO 테이블명 VALUES(값1, 값2 ... );
```

- `**INSERT**`: 테이블의 행 단위로 데이터를 추가
- `INSERT INTO`뒤에 추가할 테이블을 지정
- 행의 데이터는 `VALUES`구를 이용해 지정
- 값을 지정할 때는 열의 데이터 형식에 맞도록 지정
- `INSERT`명령은 데이터가 클라이언트에서 서버로 전송되므로 반환되는 결과가 없음

    ![https://yongineer.duckdns.org/sql/til30_1.png](https://yongineer.duckdns.org/sql/til30_1.png)

## 2. 값을 저장할 열 지정하기

```sql
INSERT INTO 테이블명(열1, 열2 ... ) VALUES (값1, 값2 ... );
```

- `INSERT INTO 테이블명`뒤에 `()`로 묶어 열명을 나열하고 `VALUES`구에 값을 지정할 경우 지정한 열과 동일한 개수로 값을 지정한다.

    ![https://yongineer.duckdns.org/sql/til30_2.png](https://yongineer.duckdns.org/sql/til30_2.png)

    *no 열과 a열만 지정해 행 추가하기*

> ⚠️ 별도의 값을 지정하지 않았던 `b`열의 기본값인 `NULL (b 열의 default 값)`이 저장된다.

## 3. NOT NULL 제약

- 행을 추가할 때 유효한 값이 없는 상태로 두고 싶을 경우에는 `VALUES`구에서 `NULL`로 값을 지정할 수 있다.

    ![https://yongineer.duckdns.org/sql/til30_3.png](https://yongineer.duckdns.org/sql/til30_3.png)

- 그러나 추가하려는 열에 `NULL`값을 허용하지 않는 `NOT NULL`제약이 걸려있다면 에러가 발생한다.
- 테이블에 저장하는 데이터를 설정으로 제한하는 것을 통틀어 '제약'이라 한다.
- 따라서 `NULL`을 허용하고 싶지 않다면 `NOT NULL`제약을 걸면 된다.

    ![https://yongineer.duckdns.org/sql/til30_4.png](https://yongineer.duckdns.org/sql/til30_4.png)

    *no 열에 3을 지정해 `NOT NULL`제약을 피해 행 추가*

## 4. DEFAULT

- 명시적으로 값을 지정하지 않았을 경우 사용하는 초기값
- `DEFAULT`는 명시적으로 저장하는 방법과 암묵적으로 저장하는 방법이 있다.

### 명시적으로 저장하는 방법

![https://yongineer.duckdns.org/sql/til30_5.png](https://yongineer.duckdns.org/sql/til30_5.png)

`VALUES`구에서 명시적으로 `DEFAULT`키워드를 사용하면 초기값이 저장된다.

### 암묵적으로 저장하는 방법

![https://yongineer.duckdns.org/sql/til30_6.png](https://yongineer.duckdns.org/sql/til30_6.png)

`DEFAULT`를 저장할 열을 `INSERT`명령문에서 별도 지정하지 않으면 초기값이 저장된다.

# 삭제하기 - DELETE

```sql
DELETE FROM 테이블명 WHERE 조건식;
```

## 1. DELETE로 행 삭제하기

- `**DELETE`는 행단위로 수행된다.**
- 삭제할 행은 `WHERE`구로 지정할 수 있으나 (조건식을 사용하면 조건에 맞는 행만 삭제) 생략하면 모든행을 대상으로 동작하여 테이블의 모든 데이터가 삭제된다.

```sql
DELETE FROM 테이블명;
```

![https://yongineer.duckdns.org/sql/til30_7.png](https://yongineer.duckdns.org/sql/til30_7.png)

- 삭제는 행 단위로 수행되기 때문에 `DELETE no FROM sample41`과 같이 열을 지정하여 삭제할수는 없다.
- `WHERE`구에서 조건식을 잘못 지정하면 의도하지 않은 데이터가 삭제되니 주의해야 한다.

## 2. DELETE 명령 구

- `WHERE`구에서 대상이 되는 행을 검색하는 것은 `SELECT`명령에서도 `DELETE`명령에서도 같다.

    ![https://yongineer.duckdns.org/sql/til30_8.png](https://yongineer.duckdns.org/sql/til30_8.png)

*앞선 예제에서 `WHERE`구를 `no = 1 OR no = 2`로 바꾸면 열이 1이나 2인 행을 삭제한다.*

- MySQL에서는 `DELETE`명령에서 `ORDER BY`구와 `LIMIT`구를 사용해 삭제할 행을 지정할 수 있으나 다른 제품에서는 불가능하다.

# 데이터 갱신하기 - UPDATE

```sql
UPDATE 테이블명 SET 열1 = 값1, 열2 = 값2, ... WHERE 조건식
```

## 1. UPDATE로 데이터 갱신하기

- `UPDATE`명령으로 데이터를 갱신할 수 있다.
- **셀 단위로 데이터를 갱신 할 수 있다.**
- `WHERE`구에 조건을 지정하면 그에 일치하는 모든 행을 갱신할 수 있다. 따라서 `DELETE`와 마찬가지로 의도하지 않은 데이터가 갱신될 수 있으므로 주의가 필요하다.
- `WHERE`구를 생략하면 테이블의 모든 행이 갱신된다.
- `**SET`구를 사용하여 갱신할 열과 값을 지정**하며 문법은 `SET 열명 = 값`이다. 이때 `=`는 비교연산자가 아닌 대입연산자이다.
- 테이블에 존재하지 않는 열을 지정하면 에러가 발생한다.
- 값은 상수로 표기하며 자료형에 맞는 값을 지정해야 한다.
- 갱신해야할 열과 값이 복수인경우 `,`로 구분한다.

    ![https://yongineer.duckdns.org/sql/til30_9.png](https://yongineer.duckdns.org/sql/til30_9.png)

## 2. UPDATE로 갱신할 경우 주의사항

![https://yongineer.duckdns.org/sql/til30_10.png](https://yongineer.duckdns.org/sql/til30_10.png)

만약 위와 같은 테이블에서 아래의 명령을 수행하면 어떻게 될까?

```sql
UPDATE sample41 SET no=no+1;
```

`WHERE`구가 지정되어 있지 않으므로 대상은 모든 행이된다. `SET`구에서 `no`열의 값을 갱신하는데 갱신후의 값은 원래 값에서 1을 더한 결과가 된다.

![https://yongineer.duckdns.org/sql/til30_11.png](https://yongineer.duckdns.org/sql/til30_11.png)

이때 해당 열이 갱신 대상이 되는 열이라 해도 상관없이 갱신되는 것을 알 수 있다. 갱신은 행 단위로 처리되므로 '현재의 no값'은 그 행이 갱신되기 전의 값에 해당한다.

## 3. 복수열 갱신

```sql
# 두 구문으로 나누어 UPDATE 명령 실행
UPDATE sample41 SET a = 'xxx' WHERE no = 4;
UPDATE sample41 SET b = '2020-08-04' WHERE no = 4;

# 하나로 묶어서 UPDATE 실행
UPDATE sample41 SET a = 'xxx', b = '2020-08-04' WHERE no = 4;
```

- `SET`구에서 `,`로 구분하여 갱신할 열을 여러개 지정 할 수 있다.

### SET구의 실행 순서

```sql
UPDATE sample41 SET no = no+1, a = no; -> 1
UPDATE sample41 SET a = no, no = no+1; -> 2
```

위와 같은 명령이 있을때 데이터베이스 마다 처리 방식이 달라진다. MySQL에서는 두 값이 다른 결과값으로 나오지만 Oracle에서는 같은 결과 값이 나온다.

![https://yongineer.duckdns.org/sql/til30_12.png](https://yongineer.duckdns.org/sql/til30_12.png)

*명령1️⃣ 실행*

1️⃣명령 실행시 `no`열과 `a`열이 서로 같아진다. `no`열의 값에 1을 더하여 no 열에 저장한 뒤, 그 값이 다시 `a`열에 대입되기 때문이다.

![https://yongineer.duckdns.org/sql/til30_13.png](https://yongineer.duckdns.org/sql/til30_13.png)

*명령2️⃣ 실행*

반면, 2️⃣명령 실행 시 `no`열과 `a`열이 서로 달라지는데 그 이유는 `no`열의 값이 `a`열에 대입 된 후 `no`열의 값을 +1 증가시키기 때문이다.

따라서 MySQL의 `SET`구는 기술된 순서대로 처리가 진행되기 때문에 갱신식 안에서 열을 참조할때는 처리 순서를 고려해야 한다.

## 4. NULL로 갱신하기

```sql
UPDATE 테이블명 SET 열 = NULL;
```

- `NULL`로 갱신하는 것을 보통 'NULL 초기화'라고 한다.

    ![https://yongineer.duckdns.org/sql/til30_14.png](https://yongineer.duckdns.org/sql/til30_14.png)

- 단, `NOT NULL`제약이 설정되어 있는 열은 `NULL`이 허용되지 않는다.

# 물리삭제와 논리삭제

## 1. 두 종류의 삭제방법

### 물리삭제

`DELETE`명령을 사용해 직접 데이터를 삭제하는것

### 논리삭제

- 테이블에 '삭제플래그'와 같은 열을 미리 준비해두고 테이블에서 실제로 행을 삭제하는 대신 `UPDATE`명령을 이용해 '삭제플래그'의 값을 유효하게 갱신하는 것
- 실제 테이블 안에 데이터는 남아있지만 참조할때 삭제플래그가 삭제로 설정된 행을 제외하는 `SELECT`명령을 실행해 결과적으로는 해당 행이 삭제된 것처럼 보인다.

> ⚠️논리삭제의 장점 및 단점
>
**논리삭제의 장점**
>
>- 삭제되기 전의 상태로 간단히 되돌릴수 있다.
>
**논리삭제의 단점**
>
>- 삭제해도 데이터베이스의 저장공간이 늘어나지 않는다.
>- 따라서 데이터베이스의 크기가 증가함에 따라 검색속도가 떨어진다.

## 2. 삭제방법 선택하기

- 사용자의 개인 정보를 다루는 시스템에서는 물리삭제를 하는 편이 안전하다.
- 쇼핑 사이트의 사용자 주문과 같은 정보는 논리삭제 방법을 많이 사용한다.
- 한편, 논리삭제는 하드웨어의 제한으로 인해 물리삭제를 할 수밖에 없는 경우도 있다. 논리삭제는 실제 데이터가 삭제되지 않기 때문에 데이터베이스의 사용량은 일방적으로 늘어난다. 따라서 결국 물리삭제 방법으로 데이터를 지운다.

# Question

### Q1. 테이블에 행을 추가할 때 사용하는 SQL 명령

- `INSERT`

### Q2. DELETE 명령에서 WHERE 구를 지정하지 않고 실행하는 경우, 테이블의 데이터는 어떻게 변화하는가?

- 모두 지워진다.

### Q3. UPDATE 명령으로 갱신할 수 있는 것은?

- 셀
