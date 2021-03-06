---
title: TIL.47) 데이터베이스 객체 작성과 삭제
date: 2020-09-14 18:09:99
category: SQL
thumbnail: '../TIL/images/sql.png'
draft: false
---

![](../TIL/images/sql.png)

# 데이터베이스 객체

## 1. 데이터베이스 객체

데이터베이스 객체란 테이블이나 뷰, 인덱스 등 데이터베이스 내에 정의하는 모든 것을 말한다. 객체는 데이터베이스 내의 실체를 가지는 어떤 것을 말한다. 객체의 종류에 따라 데이터베이스에 저장되는 내용도 달라진다.

객체는 이름을 가진다. 데이터베이스 내에서 객체를 작성할 때는 이름이 겹치지 않도록 해야한다. 이름을 붙일 때는 다음과 같은 제약 사항을 따른다.

- 기존 이름이나 예약어와 중복하지 않는다.
- 숫자로 시작할 수 없다.
- `_`외의 기호는 사용할 수 없다.
- 한글을 사용할 때는 `" "`(`MySQL`에서는 `` ``)로 둘러 싼다.
- 시스템이 허용하는 길이를 초과하지 않는다.

객체의 이름은 중요하다. 어떤 데이터가 저장되어 있는지 파악하는 기준이 되는 경우가 많기 때문에 **의미 없는 번호 등으로 이름을 붙이지 않도록 한다.**

한편, 이름은 객체의 종류와는 관계없다는 것에 주의 한다. 예를 들어 `foo`라는 이름의 테이블을 한번 만들면, 같은 종류의 테이블은 물론이고 뷰와 같은 다른 종류의 객체 역시 똑같은 이름으로 작성할수 없다.

## 2. 스키마

데이터베이스 객체는 스키마라는 그릇 안에 만들어진다. 따라서 객체의 이름이 같아도 스키마가 서로 다르다면 상관없다. 이와 같은 특징 때문에 데이터베이스 객체는 '스키마 객체' 라 불리기도 하며 스키마는 SQL명령의 DDL을 이용하여 정의한다.

어떤 것이 스키마가 되는지는 데이터베이스 제품에 따라 달라진다. `MySQL`에서는 `CREATE DATABASE` 명령으로 작성한 '데이터베이스'가 스키마가 된다. 한편, `Oracle`들에서는 데이터베이스와 데이터베이스 사용자가 계층적 스키마가 된다.

**이름이 충돌하지 않도록 기능하는 그릇을 '네임스페이스 (namespace)'라고 부르기도 한다.**

# 테이블 작성﹒삭제﹒변경

`SELETE`, `INSERT`, `DELETE`, `UPDATE`는 SQL의 명령 중에서도 `DML`로 분류된다. `DML`은 데이터를 조작하는 명령이다. 

한편, `DDL`은 데이터를 정의하는 명령이다. `DDL`은 모두 같은 문법을 사용한다. `CREATE`로 작성, `DROP`으로 삭제, `ALTER`로 변경 할수 있다. 뒤이어 어떤 종류의 객체를 작성, 삭제, 변경 할지 지정한다. 예를들어 테이블을 작성한다면 `CREATE TABLE`, 뷰를 작성한다면 `CREATE VIEW`와 같이 지정한다.

## 1. 테이블 작성

```sql
CREATE TABLE 테이블명 (
	열 정의1,
	열 정의2,
	...
)
```

작성하고 싶은 테이블의 이름을 지정하고 테이블 명 뒤에는 `()`로 묶어 열을 정의할 수 있다. 열을 정의할 때는 테이블에 필요한 열을 `,`로 구분하여 연속해 지정한다.

열명은 열에 붙이는 이름이다. 자료형은 `INTEGER`나 `VARCHAR`등을 지정한다. 특히나 `CHAR`나 `VARCHAR`와 같은 문자열형으로 지정할 때는 최대길이를 `()`로 묶어줘야 한다.

기본값을 설정할 때는 `DEFAULT`로 지정하되 자료형에 맞는 리터럴로 기술한다. 또는 기본값은 생략 가능하다.

`NULL`을 허용할 것인지 지정한다. 기본값은 허용이나 `NOT NULL`을 지정하면 `NULL`을 허용하지 않는다.

![https://yongineer.duckdns.org/sql/til47_01.png](https://yongineer.duckdns.org/sql/til47_01.png)

> ⚠️ 열 정의
>
>```sql
>열명 자료형 [DEFAULT 기본값] [NULL|NOT NULL]
>```

## 2. 테이블 삭제

```sql
DROP TABLE 테이블명
```

주의할 점은 많은 데이터베이스가 SQL명령을 실행할 때 확인을 요구하지 않는다는 것이다. 따라서 실수로 테이블을 삭제하지 않도록 신중하게 테이블을 삭제해야 한다.

### 데이터 행 삭제

```sql
TRUNCATE TABLE 테이블명
```

테이블의 정의는 그대로 둔 채 데이터만 삭제할 때는 `TRUNCATE TABLE` 명령을 사용한다. `DELETE`명령을 사용하여 삭제할 수도 있지만 `DELETE`명령은 행 단위로 여러가지 내부처리가 일어나브로 삭제할 행이 많으면 처리속도가 상당히 늦어진다.

`TRUNCATE TABLE`명령은 행을 지정할 수 없고 `WHERE`구를 지정할 수 없지만 모든 행을 삭제해야 할 때 빠른 속도로 삭제할 수 있다.

## 3. 테이블 변경

```sql
ALTER TABLE 테이블명 변경명령
```

`ALTER TABLE`명령을 사용하면 테이블에 저장되어 있는 데이터는 그대로 남긴 채 구성만 변경 할 수 있는데 `ALTER TABLE`명령이 수행하는 일은 다음 두 가지와 같다.

- 열 추가, 삭제, 변경
- 제약 추가, 삭제

### 열 추가

```sql
ALTER TABLE 테이블명 ADD 열 정의
```

열을 추가 할 때는 `ADD` 하부 명령을 통해 실행 한다. 여기에서의 열 정의는 `CREATE TABLE`의 경우와 동일하다. 즉, 열 이름, 자료형, 기본값, `NULL`제약을 지정한다. 물론 열의 이름이 중복 되면 열을 추가 할 수 없다.

![https://yongineer.duckdns.org/sql/til47_02.png](https://yongineer.duckdns.org/sql/til47_02.png)

`ALTER TABLE ADD`로 열을 추가 할 때, 기존 데이터행이 존재하면 추가한 열의 값이 모두 `NULL`이 된다. 물론 기본 값이 지정되어 있으면 기본값으로 데이터가 저장된다. **한편, `NOT NULL` 제약을 붙인 열을 추가하고 싶다면 먼저 `NOT NULL`로 제약을 건 뒤에 `NULL` 이외의 값으로 기본값을 지정할 필요가 있다.**

### 열 속성 변경

```sql
ALTER TABLE 테이블명 MODIFY 열 정의
```

열 속성을 변경할 경우 `MODIFY` 하부 명령을 통해 실행한다. 이때도 열 정의는 `CREATE TABLE`의 경우와 동일하다. `MODIFY`로 열 이름은 변경할 수 없지만, 자료형이나 기본값, `NULL`제약 등의 속성은 변경할 수 있다.

![https://yongineer.duckdns.org/sql/til47_03.png](https://yongineer.duckdns.org/sql/til47_03.png)

기존의 데이터 행이 존재하는 경우, 속성 변경에 따라 데이터 역시 변환된다. 이때 만약 자료형이 변경되면 테이블에 들어간 데이터의 자료형 역시 바뀐다. 다만 그 처리과정에서 에러가 발생하면 `ALTER TABLE`명령은 실행되지 않는다.

`MODIFY`는 `MySQL`과 `Oracle`에서 사용할 수 있는 `ALTER TABLE`의 하부명령이다.

### 열 이름 변경

```sql
ALTER TABLE 테이블명 CHANGE [기존 열 이름][신규 열 정의]
```

열 이름을 변경할 때는 `CHANGE` 하부 명령으로 변경한다. `CHANGE`는 열 이름 뿐만 아니라 열 속성도 변경할 수 있다. 한편, `Oracle`에서는 `RENAME TO` 하부 명령을 사용한다.

![https://yongineer.duckdns.org/sql/til47_04.png](https://yongineer.duckdns.org/sql/til47_04.png)

### 열 삭제

```sql
ALTER TABLE 테이블명 DROP 열명
```

열을 삭제할때는 `DROP` 하부명령을 사용한다. `DROP`뒤에 삭제하고 싶은 열 명을 지정한다. 물론, 존재하지 않는 열이 지정되면 에러가 발생한다.

![https://yongineer.duckdns.org/sql/til47_05.png](https://yongineer.duckdns.org/sql/til47_05.png)

## 4. ALTER TABLE로 테이블 관리

### 최대 길이 연장

대규모 데이터베이스에서는 데이터의 크기가 매우 커지는 경향이 있다. 이때 데이터 하나으 ㅣ크기를 최적화하는 것만으로도 저장공간을 절약할 수 있다.

특히, 문자열형의 경우 최대길이를 지정하는데, 이 최대길이를 ALTER TABLE로 늘릴 수 있다.

```sql
ALTER TABLE sample MODIFY col VARCHAR(30)
```

반대로 저장공간을 늘리기 위해 최대길이를 줄이고 싶은 경우도 있을텐데 이때 유의해야 할 점은 기존의 행에 존재하는 데이터의 길이보다 작게 지정할 수는 없다. 만약 작게 지정하게 되면 저장된 데이터의 일부가 잘려나가므로 에러가 발생하기 때문이다. 또한 열의 최대길이를 줄였다고 해서 실제 저장공간이 늘어나는 경우도 작다. 일반적으로 최대길이를 늘리는 경우는 많지만 줄이는 경우는 별로 없다.

### 열 추가

테이블에 열을 추가하는 일은 자주 일어난다.

```sql
ALTER TABLE sample ADD new_col INTEGER
```

보통 열을 추가하는 정도로는 시스템 쪽에 미치는 영향이 적을 것 같지만, 테이블 정의가 바뀌어 버리는 일인 만큼 꽤 영향을 준다. 적어도 변경한 테이블에 행을 추가하는 `INSERT`명령은 확인해야 한다. 열을 추가하면 해당 열에 대해 데이터 값을 지정해야 하기 때문이다.

만약 기존 시스템의 `INSERT`명령에서 열 지정이 생략되어 있다면, 열을 추가한 후 그대로 실행했을 때 열의 개수가 맞지 않아 에러가 발생한다.

# 제약

테이블에 제약을 설정함으로써 저장될 데이터를 제한할 수 있다. 예를 들어 NOT NULL제약은 NULL값이 저장되지 않도록 제한한다. 이 외에도 기본키 (Primary Key)제약이나 외부참조(정합) 제약등이 있다.

> ⚠️ 외부참조
>
복수의 테이블 사이에서 정합성을 유지하기 위해 설정하는 것으로 테이블 간에는 부모 자식과 같은 관계를 가지며 정합성을 유지한다.
>
자식 테이블 측에는 외부키(Foreign Key)를 지정해 부모 테이블을 참조한다. 부모 테이블에서 참조될 열은 반드시 유일성(Unique Key, Primary Key)을 가진다.

## 1. 테이블 작성시 제약 정의

```sql
CREATE TABLE sample631 (
	a INTEGER NOT NULL,
	b INTEGER NOT NULL UNIQUE,
	c VARCHAR(30)
);
```

위와 같이 열에 대해 정의하는 제약을 '열 제약'이라 한다. 한편, 한 개의 제약으로 복수의 열에 제약을 설명하는 경우를 '테이블 제약'이라 한다.

```sql
CREATE TABLE sample632 (
	no INTEGER NOT NULL,
	sub_no INTEGER NOT NULL,
	name VARCHAR(30),
	PRIMARY KEY (no, sub_no)
);
```

제약에는 이름을 붙일 수 있다. 제약 이름은 `CONSTRAINT`키워드를 사용하여 지정한다.

```sql
CREATE TABLE sample632 (
	no INTEGER NOT NULL,
	sub_no INTEGER NOT NULL,
	name VARCHAR(30),
	CONSTRAINT pkey_sample PRIMARY KEY (no, sub_no)
);
```

## 2. 제약 추가

기존 테이블에도 나중에 제약을 추가 할 수 있다. 이때 열 제약과 테이블 제약은 조금 다른 방법으로 추가한다.

### 열 제약 추가

열 제약을 추가할 경우 `ALTER TABLE`로 열 정의를 변경 한다. 기존 테이블을 변경할 경우에는 제약을 위반하는 데이터가 있는지 먼저 검사한다. 만약 `NOT NULL`제약을 추가하려는 열에 `NULL`값이 존재한다면 에러가 발생한다.

제약을 위반하는 데이터가 없다면 다음과 같이 열 제약을 추가할 수 있다.

```sql
ALTER TABLE sample631 MODIFY c VARCHAR(30) NOT NULL;
```

### 테이블 제약 추가

`ALTER TABLE`의 `ADD` 하부 명령으로 추가할 수 있으며 **기본키는 테이블에 하나만 설정**할 수 있다. 이미 키본키가 설정되어 있는 테이블에 추가로 기본키를 작성 할 수 는 없다, 또. 열 제약을 추가할 때와 마찬가지로 기존의 행을 검사해 추가할 제약을 위반하는 데이터가 있으면 에러가 발생한다.

```sql
ALTER TABLE sample631 ADD CONSTRAINT pkey_sample631 PRIMARY KEY(a);
```

## 3. 제약 삭제

테이블 제약은 나중에 삭제할 수도 있다. **제약을 추가할 때와 동일하게 열 정의를 변경한다.**

```sql
ALTER TABLE sample631 MODIFY c VARCHAR(30);
```

한편, 테이블 제약은 `ALTER TABLE`의 `DROP` 하부 명령으로 삭제할 수 있다. 삭제할 때는 제약명을 지정한다. (`MySQL`에서는 제약명 생략) 단, 기본키는 테이블당 하나만 설정할 수 있기 때문에 제약명을 지정하지 않고도 삭제할 수 있다.

```sql
ALTER TABLE sample631 DROP CONSTRAINT pkey_sample631;

# MySQL
ALTER TABLE sample631 DROP CONSTRAINT PRIMARY KEY;
```

## 4. 기본키

```sql
CREATE TABLE sample634(
	p INTEGER NOT NULL,
	a VARCHAR(30),
	CONSTRAINT pkey_sample634 PRIMARY KEY(p)
);
```

이때 열 `p`가 `sample634` 테이블의 기본키이며 기본키는 `NOT NULL`제약이 설정되어 있어야 한다.

데이터베이스에는 '검색키'와 같이 '키(key)'라는 단어가 자주 등장한다 이때 검색키는 검색할 때의 키워드라고 하면 이해하기 쉽다. 즉, 대량의 데이터에서 원하는 데이터를 찾아낼 때 키가 되는 요소를 지정해 검색하는 것이다.

기본키는 테이블의 행 한 개를 특정할 수 있는 검색키이다. 기본키로 검색했을 때 복수의 행이 일치하는 데이터를 작성할 수 없다. 즉, 기본키로 설정된 열이 중복하는 데이터 값을 가지면 제약에 위반된다.

```sql
INSERT INTO sample634 (1, '첫째줄');
INSERT INTO sample634 (2, '두째줄');
INSERT INTO sample634 (3, '셋째줄');
```

`INSERT`를 이용해 앞서 생성항 `sample634` 테이블에 세 개의 행을 추가 한 후 이미 존재하는 값인 2로 다시 한 번 행을 추가한다면 기본키 제약에 위반되어 행을 추가 할 수 없다. 기본키 `p`열의 값이 중복되기 때문이다.

![https://yongineer.duckdns.org/sql/til47_06.png](https://yongineer.duckdns.org/sql/til47_06.png)

또한, `UPDATE`명령 역시 기본키가 중복된다면 에러가 발생한다.

![https://yongineer.duckdns.org/sql/til47_07.png](https://yongineer.duckdns.org/sql/til47_07.png)

이처럼 **열을 기본키로 지정해 유일한 값을 가지도록 하는 구조가 바로 기본키 제약**이다. 행이 유일성을 필요로 한다는 다른 의미에서 '유일성 제약'이라 불리는 경우도 있다.

> 💡 기본키 제약이 설정된 열에는 중복된 값을 저장할 수 없다.

### 복수의 열로 기본키 구성하기

기본키를 구성하는 열은 복수라도 상관 없다. 다만, 키를 구성하는 모든 열을 사용해서 중복하는 값이 있는지 없는지를 검사한다.

![https://yongineer.duckdns.org/sql/til47_08.png](https://yongineer.duckdns.org/sql/til47_08.png)

위와 같은 테이블 구성에서 키가 완전히 동일한 데이터 값으로 `INSERT`명령을 실행하면 기본키 제약에 위반되어 에러가 나지만 `a`, `b`열의 데이터 중 하나라도 다르다면 키 전체로는 중복되지 않으므로 에러가 발생하지 않는다.

![https://yongineer.duckdns.org/sql/til47_09.png](https://yongineer.duckdns.org/sql/til47_09.png)

# 인덱스 구조

'색인'이라고도 불리는 인덱스는 데이터베이스 객체 중 하나이다.

## 1. 인덱스

**인덱스는 테이블에 붙여진 색인**이라 할 수 있다. **인덱스의 역할은 검색속도의 향상**이다. 테이블에 인덱스가 지정되어 있으면 효율적으로 검색할 수 있으므로 `WHERE`로 조건이 지정된 `SELECT`명령의 처리 속도가 향상된다.

책의 목차나 색인 역시 인덱스라고 생각하면 좀 더 이해하기 쉽다. 인덱스의 구조도 목차나 색인과 비슷하다. 목차나 색인에 제목, 키워드별 페이지 번호가 적혀있듯, 데이터베이스의 인덱스에는 검색 시 에 쓰이는 키워드와 대응하는 데이터 행의 장소가 저장되어 있다.

인덱스는 테이블과는 별개로 독립된 데이터베이스 객체로 작성된다. 하지만 인덱스는 테이블에 의존하는 개체이다. 대부분의 데이터베이스에서는 테이블을 삭제하면 인덱스도 같이 삭제 된다.

## 2. 검색에 사용하는 알고리즘

데이터베이스의 인덱스에 쓰이는 대표적인 검색 알고리즘으로는 '이진 트리'가 있으며 그 다음으로 '해시'가 유명하다.

이진 트리는 정확히 말하면 탐색 방법이라기 보다 데이터 구조에 가깝다. 탐색 방법으로 말하자면 '이진 탐색'이 된다. 이때 이진 탐색에서 검색하기 쉬운 구조로 한 것이 이진 트리이다.

### 풀 테이블 스캔 (Full table scan)

인덱스가 지정되지 않은 테이블을 검색할 때는 풀 테이블 스캔이라 불리는 검색 방법을 사용하며 테이블에 저장된 모든 값을 처음부터 차례로 조사해나가는 것이다.

### 이진 탐색 (binary search)

이진 탐색은 차례로 나열된 집합에 대해 유효한 검색 방법이며 집합을 반으로 나누어 조사한다.

![https://yongineer.duckdns.org/sql/til47_10.png](https://yongineer.duckdns.org/sql/til47_10.png)

만약 위와 같은 집합에서 34의 위치를 찾는다고 가정한다면 먼저 이진 탐색에서는 집합의 가운데 부터 조사한다. 위 예시에서 가운데 값은 27인데 그렇다면 현재 찾고있는 34보다 작으며 수치는 정렬되어 있으므로 27위치 오른쪽에서 다시 중심위치를 찾는다.

![https://yongineer.duckdns.org/sql/til47_11.png](https://yongineer.duckdns.org/sql/til47_11.png)

27의 오른쪽 가운데 값은 38이다. 34<38이기 때문에 이번에는 중심위치의 왼쪽에 원하는 숫자가 있을것이다.

![https://yongineer.duckdns.org/sql/til47_12.png](https://yongineer.duckdns.org/sql/til47_12.png)

이번에는 30이 중심위치다 따라서 34>30 이기 때문에 오른쪽에서 원하는 숫자를 찾게 된다.

![https://yongineer.duckdns.org/sql/til47_13.png](https://yongineer.duckdns.org/sql/til47_13.png)

이번에는 중심위치가 34이며 이 숫자가 지금 우리가 찾는 숫자와 일치하게 된다. 이렇게 이진 탐색을 사용하면 총 네번의 비교를 통해 목표를 찾을 수 있다.

만약 풀 테이블 스캔으로 찾았다면 최대 열 번을 비교해야 했겠지만, 이진 탐색이라면 4회로 끝나기 때문에 더 효율적이다.

### 이진 트리 (binary tree)

이진 탐색은 고속으로 검색할 수 있는 탐색 방법이지만 데이터가 미리 정렬되어 있어야 한다. 하지만 테이블 내의 행을 언제나 정렬된 상태로 두는 것은 힘든 작업이다.

일반적으로 테이블에 인덱스를 작성하면 테이블 데이터와 별개로 인덱스용 데이터가 저장장치에 만들어진다. 이때 이진 트리라는 데이터 구조로 작성된다. 이진 트리의 구조는 다음과 같다.

![https://yongineer.duckdns.org/sql/til47_18.png](https://yongineer.duckdns.org/sql/til47_18.png)

트리는 노드라는 요소로 구성된다. 각 노드는 두개의 가지로 나뉜다. 노드의 왼쪽 가지는 작은 값으로, 오른쪽 가지는 큰 값으로 나뉘어져 있다. 두 개의 가지로 분기하는 구조라서 '이진 트리'라 불리는 것이다.

이진 드리의 경우에는 트리의 루트 노드부터 시작한다. 원하는 수치와 비교해서 더 크면 오른쪽 가지를, 작으면 왼쪽의 가지를 조사해 나간다.

위 트리에서 12라는 값을 검색하는 예는 다음과 같다.

![https://yongineer.duckdns.org/sql/til47_14.png](https://yongineer.duckdns.org/sql/til47_14.png)

먼저 루트 노드에서 찾을 값과 루트 노드의 값을 비교한다. 20 > 12 이기 때문에 루트노드에서 왼쪽의 가지로 이동한다.

![https://yongineer.duckdns.org/sql/til47_15.png](https://yongineer.duckdns.org/sql/til47_15.png)

다음은 15이다. 15 > 12 이기때문에 이번에도 왼쪽 가지로 이동한다.

![https://yongineer.duckdns.org/sql/til47_16.png](https://yongineer.duckdns.org/sql/til47_16.png)

다음은 10이다. 12 > 10 이기때문에 이번에는 오른쪽 가지로 이동한다.

![https://yongineer.duckdns.org/sql/til47_17.png](https://yongineer.duckdns.org/sql/til47_17.png)

이렇게 원하는 값인 12를 찾을 수 있었다.

## 3. 유일성

이진 트리에서는 집합 내 중복하는 값을 가질 수 없다. 즉, 노드의 가지는 큰 쪽과 작은 쪽의 두 가지로 나뉘며, 같은 값을 허용하기 위해서는 '같은' 이라는 제 3의 가지를 가질 필요가 있다.

하지만, 이진 트리에서 '같은 값을 가지는 노드를 여러개 만들 수 없다'라는 특성 은 키에 대하여 유일성을 가지게 할 경우에만 유용하다. 그래서 기본키 제약은 이진 트리로 인덱스를 작성하는 데이터가 많다.

> 💡 이진 트리에는 중복하는 값을 등록 할 수 없다.

# 인덱스 작성과 삭제

인덱스는 데이터베이스 객체의 하나로 `DDL`을 사용해서 작성하거나 삭제한ㄷ.

## 1. 인덱스 작성

```sql
CREATE INDEX 인덱스명 ON 테이블명 (열명1, 열명2, ...)
```

인덱스는 `CREATE INDEX` 명령으로 만든다. 데이터베이스 객체가 될지 테이블의 열처럼 취급할지는 데이터베이스 제품에 따라 다르다. `Oracle`이나 `DB2` 등에서 인덱스는 스키마 객체가 된다. 따라서 스키마 내에 이름이 중복하지 않도록 지정해 관리한다. 한편 `SQL Server`나 `MySQL`에서 인덱스는 테이블 내의 객체가 된다. 따라서 테이블 내에 이름이 중복되지 않도록 지정해 관리한다.

인덱스를 작성할 때는 해당 인덱스가 어느 테이블의 어느 열에 관한 것인지 지정할 필요가 있다. 이때 열은 복수로도 지정할 수 있다.

```sql
CREATE INDEX isample65 ON sample62(no);
```

위 명령은 `sample62` 테이블의 `no`열에 `isample65`라는 인덱스를 지정하는 것이다.인덱스를 작성할 때는 저장장치에 색인용 데이터가 만들어진다. 테이블 크기에 따라 인덱스 작성시간도 달라지는데, 행이 대량으로 존재하면 시간도 많이 걸리고 저장공간도 많이 소비한다.

## 2. 인덱스 삭제

```sql
# 스키마 객체인 경우
DROP INDEX 인덱스명

# 테이블 내 객체인 경우
DROP INDEX 인덱스명 ON 테이블명
```

인덱스는 `DROP INDEX` 명령으로 삭제한다. `DROP`할때는 다른 객체와 동일하게 인덱스 이름만 지정하면 된다. 다만, 테이블 내 객체로서 작성하는 경우에는 테이블 이름도 지정한다. 이때 인덱스를 구성하는 열은 지정할 필요가 없다.

인덱스는 테이블에 의존하는 객체이다. `DROP TABLE`로 테이블을 삭제하면 테이블에 저장된 인덱스도 자동으로 삭제된다. 인덱스만 삭제하는 경우에는 `DROP INDEX`를 사용한다.

```sql
DROP INDEX isample65 ON sample62;
```

인덱스를 작성해두면 검색이 빨라진다, 작성한 인덱스의 열을 `WHERE`구로 조건을 지정하여 `SELECT`명령으로 **검색하면 처리속도가 향상**된다. 한편, **`INSERT`명령의 경우에는 인덱스를 최신 상태로 갱신하는 처리가 늘어나므로 처리속도가 조금 떨어지게 된다.**

> ⚠️ `SELECT`명령에서의 인덱스 사용
>
>먼저 다음과 같은 명령으로 인덱스를 작성했다고 가정한다.
>
>```sql
>CREATE INDEX isample65 on sample62(a);
>```
>
>`WHERE`구에 `a`열에 대한 조건식을 지정한 경우 `SELECT`명령은 인덱스를 사용해 빠르게 검색할 수 있다.
>
>```sql
>SELECT * FROM sample62 WHERE a = 'a';
>```
>
>그러나 `WHERE`구의 조건식에 `a`열이 전혀 사용되지 않으면 `SELECT`명령은 `isample62`라는 인덱스를 사용할 수 없다.
>
>```sql
>SELECT * FROM sample62 WHERE b = 'b';
>```

## 3. EXPLAIN

```sql
EXPLAIN SQL명령
```

실제로 인덱스를 사용해 검색하는지를 확인하려면 `EXPLAIN`명령을 사용한다. 이 SQL명령은 실제로 실행되지 않는다. 어떤 상태로 실행되는지를 데이터베이스가 설명해줄 뿐이다. 단, `MySQL`의 경우 상황에 따라 다르지만 필요한 정보를 얻기 위해 SQL명령의 일부분을 실제로 실행하는 경우도 있다.

`EXPLAIN`은 표준 SQL에는 존재하지 않는, 데이터베이스 제품 의존형 명령이다. 하지만 어떤 데이터베이스 제품이라도 이와 비슷한 명령을 지원한다.

![https://yongineer.duckdns.org/sql/til47_19.png](https://yongineer.duckdns.org/sql/til47_19.png)

`sample62`의 `a`열에는 `isample65`라는 인덱스가 작성되어 있다. `possible_keys`라는 곳에 사용될 수 있는 인덱스가 표시되며, `key`는 사용된 인덱스가 표시된다.

![https://yongineer.duckdns.org/sql/til47_20.png](https://yongineer.duckdns.org/sql/til47_20.png)

`WHERE`의 조건을 바꿔 인덱스가 작성된 `a`열을 조건식에 사용하지 않는다면 인덱스를 사용할 수 없다 (`possible_keys`와 `key`가 `NULL`로 표기된다.)

## 4. 최적화

`SELECT`명령을 실행할 때 인덱스의 사용 여부를 선택한다. 이는 데이터베이스 내부의 최적화에 의해 처리되는 부분이다. **내부처리에는 `SELECT`명령을 실행하기에 앞서 실행계획을 세운다.** 실행계획에서는 '인덱스가 지정된 열이 `WHERE`조건으로 지정되어 있으니 인덱스를 사용하자'와 같은 처리가 이루어 진다. `EXPLAIN`명령은 이 실행계획을 확인하는 명령이다.

실행계획에서는 인덱스의 유무뿐만 아니라 인덱스를 사용할 것인지 여부에 대해서도 데이터베이스 내부의 최적화 처리를 통해 판단한다. 이때 판단 기준으로 인덱스의 품질도 고려된다.

예를들어, '예' 또는 '아니오'라는 값만 가지는 열이 있다면, 해당 열에 인덱스를 지정해도 다음과 같은 이진트리가 되어 좋은 구조를 가지지 못한다.

![https://yongineer.duckdns.org/sql/til47_21.png](https://yongineer.duckdns.org/sql/til47_21.png)

이는 단순한 리스트와 별다른 차이가 없는 구조로 이진 탐색에 의한 효율화를 기대하기 어렵다. 위 예시는 극단적인 예시지만 **데이터의 종류가 적으면 적을수록 인덱스의 효율도 떨어진다.** 반대로 서로 다른 값으로 여러 종류의 데이터가 존재하면 그만큼 효율은 좋아진다. 이렇게 인덱스의 품질을 고려해 실행계획이 세워지는 것이다.

# 뷰 작성과 삭제

서브쿼리에 이름을 붙이고 데이터베이스 객체화하여 쓰기 쉽게 한 것을 뷰라고 한다.

## 1. 뷰

데이터베이스 객체중 하나이다. **본래 데이터베이스 객체로 등록할 수 없는 `SELECT`명령을 객체로서 이름을 붙여 관리할 수 있도록 한 것이 뷰이다.** 따라서 뷰를 참조하면 그에 정의된 `SELECT`명령의 실행결과를 테이블 처럼 사용할 수 있다.

```sql
SELECT * FROM (SELECT * FROM sample54) sq;
```

서브쿼리 부분을 '뷰 객체'로 만들면 다음과 같은 `SELECT`명령이 된다.

```sql
SELECT * FROM sample_view67
```

뷰를 정의할 때는 이름과 `SELECT`명령을 지정한다. 이후 뷰를 사용할 `SELECT`명령에서 뷰의 이름을 지정하면 참조할 수 있다.

실제 업무에서는 `WHERE`구로 조건을 지정하거나 `GROUP BY`구로 집계하는 등 좀 더 복잡한 명령으로 이루어지는 경우가 많다. 이런 경우 서브쿼리 부분을 뷰로 대체하여 `SELECT`명령을 간략하게 표현 할 수 있다. 또한 복잡한 `SELECT`명령을 데이터베이스에 등록해 두었다가 나중에 간단히 실행할 수도 있다. 즉, **자주 사용하거나 복잡한 `SELECT`명령을 뷰로 만들어 편리하게 사용할 수 있는것이다.**

### 가상 테이블

뷰는 테이블처럼 취급할 수 있지만 '실체가 존재하지 않는다'라는 의미로 **'가상 테이블'**이라 불리기도 한다. 뷰는 테이블 처럼 데이터를 쓰거나 지울 수 있는 저장공간을 가지지 않는다. 이 때문에 **뷰는 `SELECT` 명령에서만 사용하는 것을 권장한다.** `INSERT`나 `UPDATE`, `DELETE`명령에서도 조건이 맞으면 가능하지만 사용에 주의할 필요가 있다.

## 2. 뷰 작성과 삭제

뷰는 데이터베이스 객체이기 때문에 `DDL`로 작성하거나 삭제한다.

- 작성 : `CREATE VIEW`
- 삭제 : `DROP VIEW`

### 뷰의 작성

```sql
CREATE VIEW 뷰명 AS SELECT 명령
```

CREATE VIEW 다음에 뷰의 이름을 지정하고 AS로 SELECT명령을 지정한다. 이때 AS별명을 붙일 때 사용하는 AS와는 달리 생략 불가능 하다.

![https://yongineer.duckdns.org/sql/til47_22.png](https://yongineer.duckdns.org/sql/til47_22.png)

뷰를 작성 한 뒤 `SELECT`명령의 `FROM`구에 지정해 사용할 수 있다. 뷰는 필요에 따라 열을 지정할 수도 있다. 이 경우에는 이름 뒤에 `()`로 묶어 열을 나열한다.

```sql
CREATE VIEW 뷰명 (열명1, 열명2, ...) AS SELECT 명령
```

뷰의 열 지정을 생략한 경우 SELECT명령의 SELECT구에서 지정하는 열 정보가 수집되어 자동적으로 뷰의 열로 지정된다. 반대로 열을 지정한 경우에는 SELECT명령의 SELECT구에 지정한 열보다 우선된다.

SELECT명령의 모든 열을 사용할 경우에는 열을지정하지 않는 편이 낫다. 또한 열 이외에는 정의할 수 없으며, 테이블의 열을 정의할 때처럼 자료형이나 제약도 지정할 수 없다.

![https://yongineer.duckdns.org/sql/til47_23.png](https://yongineer.duckdns.org/sql/til47_23.png)

### 뷰 삭제

```sql
DROP VIEW 뷰명
```

뷰를 삭제할 경우에는 DROP VIEW를 사용한다. 일단 뷰를 삭제하면 더 이상 뷰를 참조하여 사용할 수 없다.

## 3. 뷰의 약점

뷰는 데이터베이스 객체로서 저장장치에 저장된다. 하지만 **테이블과 달리 대량의 저장공간을 필요로하지 않는다. 데이터베이스에 저장되는 것은 `SELECT`명령뿐이기 때문이다. 대신 CPU의 자원을 사용한다.** 뷰의 실행 결과는 일시적으로 보존되며, 뷰를 참조할 때마다 `SELECT`명령이 실행된다.

### 머티리얼라이즈드 뷰 (Materialized View)

뷰의 근원이 되는 테이블에 보관하는 데이터양이 많은 경우, 집계처리를 할 때도 뷰가 사용된다면 처리속도가 많이 떨어질 수 밖에 없다. 뷰를 중첩해서 사용하는 경우에도 처리속도가 떨어지기 쉽다.

이 같은 상황을 회피하기 위해 사용할 수 있는 것이 머티리얼라이즈드 뷰이다. 머티리얼라이즈드 뷰는 데이터를 일시적으로 저장해 사용하는 것이 아니라 테이블처럼 저장장치에 저장해두고 사용한다.

머티리얼라이즈드 뷰는 처음 참조되었을 때 데이터를 저장해둔다. 이후 다시 참조할 때 이전에 저장해두었던 데이터를 그대로 사용한다. 일반적인 뷰처럼 매번 `SELECT`명령을 실생 할 필요가 없다. 다만, 뷰에 지정된 테이블의 데이터가 변경된 경우에는 `SELECT`명령을 재실행하여 데이터를 다시 저장한다. 변경 유무를 확인하여 재실행 하는것은 `RDBMS`가 자동으로 실행한다.

뷰에 지정된 테이블의 데이터가 자주 변경되지 않는 경우라면 머티리얼라이즈드 뷰를 사용하여 뷰의 약점을 어느정도 보완할 수 있다. 다만, `MySQL`에서는 머티리얼라이즈드 뷰를 사용할 수 없다. `Oracle`과 `DB2`에서만 사용할 수 잇는 데이터베이스 객체이다.

### 함수 테이블

**상관 서브쿼리의 경우 뷰의 `SELECT`명령으로 사용할 수 없다.** 대신 이 같은 뷰의 약점을 함수테이블을 사용하여 회피할 수 있다. 함수 테이블은 테이블을 결과값으로 반환해주는 사용자정의 함수이다.

함수에는 인수를 지정할 수 있기 때문에 인수의 값에 따라 `WHERE`조건을 붙여 결과값을 바꿀 수 있다. 그에 따라 상관 서브쿼리처럼 동작 할 수 있다.

# Question

## 1. 테이블을 작성할 때 사용하는 명령어

- `CREATE TABLE`

## 2. 저장장치의 저장공간을 거의 사용하지 않는 객체

- 뷰

## 3. 인덱스를 사용하면 어떤 효과를 얻을 수 있는가?

- 쿼리의 성능 향상
