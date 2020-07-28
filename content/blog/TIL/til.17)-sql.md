---
title: TIL.17) SQL
date: 2020-07-27 18:49:58
category: SQL 
thumbnail: './images/sql.png'
draft: false
---

![](./images/sql.png)

# SQL?

- Structured Query Language
- MySQL같은 관계형 데이터베이스에서 데이터를 읽거나 생성 및 수정하기 위해 사용하는 언어
- CRUD

> 💡**CRUD?**
>
>**C**reate
>
>**R**ead
>
>**U**pdate
>
>**D**elete

# SELECT

- 데이터를 **읽어** 들일 때 사용

```sql
SELECT 
    id,
    name,
    age,
    gender
FROM users
-> users테이블에서 id, name, age, gender를 읽어온다.
```

- `WHERE` 구문을 사용해 **검색**도 가능

```sql
SELECT 
    id,
    name,
    age,
    gender
FROM users
WHERE name = "아이유"
-> users테이블에서 name의 값이 "아이유"인 row의 id, name, age, gender를 읽어온다.
```

# INSERT

- 데이터를 **생성**할 때 사용

```sql
INSERT INTO users (
    id,
    name,
    age,
    gender
) VALUES (
    1,
    "아이유",
    27,
    "여자"
), (
    2,
    "제인",
    19,
    "여자"
)
-> users테이블의 id,name,age,gender column에 VALUES의 값을 각각 대입하여 생성한다.
-> id=1, name="아이유", age=27, gender="여자"
-> id=2, name="제인", age=19, gender="여자"
```

# UPDATE

- 데이터를 **수정**할 때 사용

```sql
UPDATE users SET age = 25 WHERE name = "아이유"
-> users테이블의 name값이 "아이유" row의 age를 25로 수정한다.
```

# DELETE

- 데이터를 **삭제**할 때 사용

```sql
DELETE FROM users WHERE gender = "남자"
-> users테이블에서 gender값이 "남자"인 row를 삭제한다.
```

# Exist

- 어떠한 조건이 존재하는지 여부를 확인 할 때 사용하는 SQL문
- 예를 들어, 특정 `user_name`이 이미 존재하는지 확인 하고 싶을 때

```sql
SELECT 1
FROM users
WHERE EXISTS (SELECT * 
              FROM users
              WHERE user_name = '아이유');
-> users테이블에서 만약 users테이블의 user_name이 "아이유"인 row의 개수만큼 1을 반환하라
```

> 💡 **SELECT 1**
>
>- SQL에서 1은 `True`를 의미한다.
>- 만약 만족하는 테이블의 row의 수가 N개면 N개 만큼 1을 반환하게 된다.

> 💡 `NOT EXIST`
>- `EXISTS`의 반대의 경우로 체크 할 수 있다.
```sql
SELECT lname, fname
FROM customer
WHERE NOT EXISTS (SELECT * 
                  FROM orders 
                  WHERE customers.customer_id = orders.c_id);
```

# JOIN

- Foreign Key(외부키)로 걸려있는 2개의 table들을 join(연결)해서 양쪽 table에서 모두 row를 읽어 들이고 싶을 때는 `join`문을 사용해야 함.
- 즉, `join`은 교집합이라고 생각하면 된다.

## Join 기본 문법

```sql
SELECT
  테이블별칭.조회할칼럼,
  테이블별칭.조회할칼럼
FROM 기준테이블 별칭
INNER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키
```

## Join 사용 예

```sql
SELECT 
    users.id,
    users.name,
    users.age,
    users.gender,
    accounts.account_type
FROM users
JOIN accounts ON accounts.id = users.account_id
```

## Join의 여러 유형들

### `(INNER) JOIN`

- 일반적인 join문
- 기준이 되는 테이블(left table)과 join이 걸리는 테이블 (right table) 양쪽 모두에 matching되는 row만 select가 됨

### `LEFT (OUTER) JOIN`

- 기준이 되는 테이블 (left table)의 모든 row와 join이 걸리는 테이블 (right table)중 left table과 matching되는 row만 select가 됨.

### `RIGHT (OUTER) JOIN`

- join이 걸리는 테이블 (right table)의 모든 row와 기준이 되는 테이블 (left table)에서right table과 matching되는 row만 select가 됨.

### `FULL (OUTER) JOIN`

- 기준이 되는 테이블 (left table)과 join이 걸리는 테이블 (right table) 양쪽 모두의 모든 row를 select 한다.
![](https://images.velog.io/images/yongineer1990/post/cd6551f3-f40a-47d7-9462-13fd63a5fb65/image.png)

# CREATE TABLE

- 테이블을 **생성**할 때 사용

```sql
CREATE TABLE accounts(
   id INT NOT NULL AUTO_INCREMENT,
   account_type VARCHAR(100) NOT NULL,
   PRIMARY KEY (id)
);

CREATE TABLE users(
   id INT NOT NULL AUTO_INCREMENT,
   name VARCHAR(255) NOT NULL,
   email VARCHAR(255) NOT NULL,
   hashed_password VARCHAR(255) NOT NULL,
   account_id INT NOT NULL,
   PRIMARY KEY (id),
   UNIQUE KEY email (email),
   CONSTRAINT `users_account_fkey` FOREIGN KEY (`account_id`) 
		REFERENCES `accounts` (`id`)
);
```
