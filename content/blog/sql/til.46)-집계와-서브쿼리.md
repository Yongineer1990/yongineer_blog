---
title: TIL.46) 집계와 서브쿼리
date: 2020-09-11 03:09:50
category: SQL
thumbnail: '../TIL/images/sql.png'
draft: false
---

![](../TIL/images/sql.png)

# 행 개수 구하기 - COUNT

## 1. COUNT로 행 개수 구하기

```sql
COUNT(집합)
```

![https://yongineer.duckdns.org/sql/til46_01.png](https://yongineer.duckdns.org/sql/til46_01.png)

SQL은 집합을 다루는 **집계함수**를 제공한다. 집계함수는 복수의 값(집합)에서 하나의 값을 계산해내는 것이며 집합으로 부터 하나의 값을 계산하는 것을 '집계'라 부른다. 이러한 이유로 집계함수를 `SELECT`구에 쓰면 `WHERE`구의 유무와 상관없이 결과값으로 하나의 행을 반환하게 된다.

- COUNT함수는 인수로 주어진 집합의 **'행 개수'를 구해 반환**한다.
- 인수로 `*`가 지정되어 있는데 이는 `SELECT`구에서 '모든 열'을 나타낼 때 사용하는 메타문자와 같다.

### WHERE구 지정하기

![https://yongineer.duckdns.org/sql/til46_02.png](https://yongineer.duckdns.org/sql/til46_02.png)

`SELECT`구는 `WHERE`구보다 나중에 내부적으로 처리된다. 따라서 `WHERE`구로 조건을 지정하면 테이블 전체가 아닌, 검색된 행이 `COUNT`로 넘겨진다. 즉, `WHERE`구의 조건에 맞는 행의 개수를 구할 수 있다.

위 예제에서도 검색된 행은 두개 였지만, 최종으로 하나의 행이 결과값으로 반환 되었다.

## 2. 집계함수와 NULL값

- `COUNT`의 인수로 **열명을 지정할 수 있으며 열명을 지정하면 그 열에 한해서 행의 개수를 구할 수 있다.**
- **메타문자 `*`을 인수로 사용할 수 있는 것은 `COUNT`함수 뿐이며** 다른 집계함수에서는 열명이나 식을 인수로 지정한다.
- 집계함수는 **집합 안에 NULL값이 있을 경우 이를 제외하고 처리한다.**

![https://yongineer.duckdns.org/sql/til46_03.png](https://yongineer.duckdns.org/sql/til46_03.png)

`name`열에는 NULL값을 가지는 행이 하나 존재하므로 이를 제외한 개수는 4가 된다. 다만, `COUNT(*)`의 경우 모든 열의 행수를 카운트하기 때문에 `NULL`값이 있어도 해당 정보가 무시되지 않는다.

## 3. DISTINCT로 중복 제거

SQL의 `SELECT`명령은 중복된 값을 제거하는 `DISTINCT`함수가 제공된다.

![https://yongineer.duckdns.org/sql/til46_04.png](https://yongineer.duckdns.org/sql/til46_04.png)

`DISTINCT`는 예약어로 열명이 아니다. `SELECT`구에서 `DISTINCT`를 지정하면 중복된 데이터를 제외한 결과를 클라이언트로 반환한다. 위 예제에서 첫번째 `SELECT`명령에서는 `DISTINCT`가 아닌 ALL을 지정했다. 이렇게하면 중복 유무와 관계없이 문자 그대로 모든행을 반환한다.

즉, `SELECT`구에서 지정하는 `ALL`또는 `DISTINCT`는 중복된 값을 제거할 것인지 설정하는 스위치와 같은 역할을 한다. 만약 이를 지정하지 않고 생략할 경우 `ALL`로 간주되어 처리된다.

## 4. 집계함수에서 DISTINCT

앞선 예제에서 `name`열에서 `NULL`을 생략하고 중복되지 않는 데이터의 개수만 구한다고 가정한다면 (A, B, C 만 집계되어 3을 반환) 집계함수의 인수로 `DISTINCT`를 사용한 수식을 지정하는 것이다. `DISTINCT`는 집계함수의 인수에 수식자로 지정할 수 있다. `DISTINCT`를 이용해 집합에서 중복을 제거한 뒤 `COUNT`로 개수를 구할 수 있다.

![https://yongineer.duckdns.org/sql/til46_05.png](https://yongineer.duckdns.org/sql/til46_05.png)

# COUNT 이외의 집계함수

## 1. SUM으로 합계 구하기

```sql
SUM([ALL|DISTINCT] 집합)
```

![https://yongineer.duckdns.org/sql/til46_06.png](https://yongineer.duckdns.org/sql/til46_06.png)

집합의 **합계를 구하며 지정되는 집합은 수치형 뿐**이다. 문자열형이나 날짜시간형의 집합에서 합계를 구할 수는 없다. 위 예제와 같이 `name`열은 문자열형이므로 `SUM`함수로 합계를 구할 수 없다.

한편, `SUM` 집계함수도 `COUNT`와 마찬가지로 `NULL`값을 무시하며 `NULL`값을 제거한 뒤에 합계를 계산한다.

## 2. AVG로 평균내기

```sql
AVG([ALL|DISTINCT] 집합)
```

![https://yongineer.duckdns.org/sql/til46_07.png](https://yongineer.duckdns.org/sql/til46_07.png)

집합의 **평균값을 구하며 `SUM`과 마찬가지로 수치형만 가능**하다. 또한 `NULL`값은 무시되는데 이때 `NULL`을 0으로 간주해서 평균을 내고 싶다면 `CASE`를 사용해 `NULL`을 0으로 변환한 뒤에 `AVG`함수로 계산하면 된다.

![https://yongineer.duckdns.org/sql/til46_08.png](https://yongineer.duckdns.org/sql/til46_08.png)

## 3. MIN, MAX로 최소값, 최대값 구하기

```sql
MIN([ALL|DISTINCT] 집합)
MAX([ALL|DISTINCT] 집합)
```

![https://yongineer.duckdns.org/sql/til46_09.png](https://yongineer.duckdns.org/sql/til46_09.png)

- `MIN` : 최소값
- `MAX` : 최대값

이들 함수는 **문자열형과 날짜시간형에도 사용 할 수 있으며** 다른 집계함수들과 마찬가지로 `NULL`값을 무시한다.

# 그룹화 - GROUP BY

```sql
SELECT * FROM 테이블명 GROUP BY 열1, 열2, ...
```

## 1. GROUP BY 로 그룹화

그룹으로 나눌때 `GROUP BY`구를 사용한다. 이때 `GROUP BY`구에는 그룹화할 열을 지정한다. 또한 복수로도 지정할 수 있다.

![https://yongineer.duckdns.org/sql/til46_10.png](https://yongineer.duckdns.org/sql/til46_10.png)

위 예제를 보면 `DISTINCT`를 지정했을 때와 같은 결과가 반환된것을 확인 할 수 있는데 **`GROUP BY`구에 열을 지정하여 그룹화하면 지정된 열의 값이 같은 행이 하나의 그룹으로 묶인다.**

각 그룹으로 묶인 값들은 서로 동일하다. 즉, 결과적으로는 각각의 그룹 값이 반환되는 것이다. 따라서 `GROUP BY`를 지정해 그룹화 하면 `DISTINCT`와 같이 중복을 제거하는 효과가 있다.

다만, **`GROUP BY`구를 지정하는 경우에 집계함수와 함계 사용하지 않으면 별 의미가 없다.** `GROUP BY`구로 그룹화된 각각의 그룹이 하나의 집합으로서 집계함수의 인수로 넘겨지기 때문이다.

![https://yongineer.duckdns.org/sql/til46_11.png](https://yongineer.duckdns.org/sql/til46_11.png)

위 예제를 보면 `GROUP BY name`에 의해 `name`열 값이 A, B, C, `NULL`이렇게 네 개 그룹으로 나뉜다. A 그룹에는 두 개의 행이 있는데, `COUNT`는 행의 개수를 반환 하므로 2라는 값을 가지게 된다. A 그룹에 해당하는 2개의 행의 `quantity`열 값은 각각 1과 2이므로 SUM함수의 결과는 3이 된다.

> ⚠️ 실무에서 `GROUP BY`를 사용하는 경우
>
점포별 매출실적을 집계해 어떤 점포가 매출이 올라가는지, 어떤 상품이 인기가 있는지 등을 분석할 때 사용된다. 여기에서 점포별, 상품별, 월별, 일별 등 특정 단위로 집계할 때 `GROUP BY`를 자주 사용한다.

## 2. HAVING구로 조건 지정

![https://yongineer.duckdns.org/sql/til46_12.png](https://yongineer.duckdns.org/sql/til46_12.png)

**집계함수는 `WHERE`구의 조건식에서는 사용 할 수 없다.** 왜냐하면 `GROUP BY`와 `WHERE`구의 내부처리 순서와 관계가 있다. 즉, `WHERE`구로 행을 검색하는 처리가 `GROUP BY`로 그룹화하는 처리보다 순서상 앞서기 때문이다. 따라서 집계한 결과에서 조건에 맞는 값을 따로 걸러낼 수 있는데 이때 사용하는 것이 `HAVING`구다.

`HAVING`구는 `GROUP BY`구의 뒤에 기술하며 WHERE구와 동일하게 조건식을 지정 할 수 있다. 조건식에는 그룹별로 집계된 열의 값이나 집계함수의 계산결과가 전달된다 이때 조건식이 참인 그룹값만 클라이언트에게 반환된다.

결과적으로 `WHERE`구와 `HAVING`구에 지정된 조건으로 검색하는 2단 구조가 된다.

![https://yongineer.duckdns.org/sql/til46_13.png](https://yongineer.duckdns.org/sql/til46_13.png)

그룹화 보다도 나중에 처리되는 `ORDER BY`구에서는 문제없이 집계함수를 사용 할 수 있다. 즉, `ORDER BY COUNT(name)`과 같이 지정할 수 있다.

> ⚠️ 내부처리 순서
>
`WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY`

`SELECT`구보다도 먼저 처리되므로 **별명을 사용 할 수는 없다.** `COUNT(name)`에 cn이라는 별명을 붙이면 `ORDER BY`구에는 사용 할 수 있지만 `GROUP BY`구나 `HAVING`구에서는 사용할 수 없다.

```sql
SELECT name AS n, COUNT(name) AS cn FROM sample51 GROUP BY n HAVING cn = 1;
```

다만, `MySQL`과 같은 RDBMS에서는 조금 융통성 있게 별명을 사용 할 수 있는데 위와 같은 명령은 `Oracle`에서는 에러가 나지만 `MySQL`에서는 실행이 가능하다.

## 3. 복수열의 그룹화

GROUP BY에 지정한 열 이외의 열은 집계함수를 사용하지 않은 채 SELECT구에 기술해서는 안된다.

![https://yongineer.duckdns.org/sql/til46_14.png](https://yongineer.duckdns.org/sql/til46_14.png)

위 예제에서 `GROUP BY name`으로 `name`열을 그룹화 했다. 이 경우 `SELECT`구에 `name`을 지정하는 것은 문제 없지만 `no`열이나 `quantity`열을 `SELECT`구에 그대로 지정하면 에러가 발생한다.

`GROUP BY`로 그룹화하면 클라이언트로 **반환되는 결과는 그룹당 하나의 행**이된다, 하지만 `name`열 값이 A인 그룹의 `quantity`열 값은 1과 2로 두 개의 값이 존재한다. 이때 **그룹마다 하나의 값만을 반환해야 하므로 어느 것을 반환하면 좋을지 몰라 에러**가 발생하게 된다. 이때 집계함수를 사용하면 집합은 하나의 값으로 계산되므로 그룹마다 하나의 행을 출력할 수 있게 된다.

![https://yongineer.duckdns.org/sql/til46_15.png](https://yongineer.duckdns.org/sql/til46_15.png)

> 💡`GROUP BY`에서 지정한 열 이외의 열은 집계함수를 사용하지 않은채 `SELECT`구에 지정할 수 없다.

## 4. 결과값 정렬

GROUP BY로 그룹화 해도 실행결과 순서를 정렬할 수 없다. 이때 **ORDER BY구를 사용해 결과를 정렬할 수 있다.**

![https://yongineer.duckdns.org/sql/til46_16.png](https://yongineer.duckdns.org/sql/til46_16.png)

# 서브쿼리

서브쿼리는 SQL명령문 안에 지정하는 하부 `SELECT`명령으로 `()`로 묶어 지정한다. 서브쿼리는 SQL명령의 `WHERE`구에서 주로 사용되며 `WHERE`구를 사용할 수 있는 `SELECT`, `DELETE`, `UPDATE`구에서도 물론 사용이 가능하다.

## 1. DELETE의 WHERE구에서 서브쿼리 사용하기

![https://yongineer.duckdns.org/sql/til46_17.png](https://yongineer.duckdns.org/sql/til46_17.png)

위 테이블에서 `a`열의 값이 가장 작은 행을 삭제 하려 한다. 위 예제는 행 개수가 몇개 없으므로 `a`가 20인 행이 가장 작다는 것을 한눈에 알아볼 수 있다. 따라서 간단하게 `DELETE FROM sample54 WHERE a = 20;`이라는 명령으로 가장 작은 행을 삭제 할 수 있을 것이다.

하지만 `a`열의 값이 가장 작은 행이 어느 행인지 전혀 파악할 수 없이 데이터가 크다면 아마 먼저 `SELECT`명령으로 가장 작은 값을 찾는 `MIN`함수를 사용하여 검색할 것이다.

![https://yongineer.duckdns.org/sql/til46_18.png](https://yongineer.duckdns.org/sql/til46_18.png)

이 `SELECT`명령을 `DELETE`명령의 `WHERE`구에서 사용할 수 있다면 하나의 `DELETE`명령으로 원하는 행을 삭제 할 수 있을것이다. 이때 **서브쿼리를 사용하여 `DELETE`와 `SELECT`를 결합시킬 수 있다.** `()`둘러싼 서브쿼리를 먼저 실행 한 후 `DELETE`명령을 실행한다고 생각하면 이해하기 쉽다.

![https://yongineer.duckdns.org/sql/til46_19.png](https://yongineer.duckdns.org/sql/til46_19.png)

단, `MySQL`에서는 위와 같이 에러가 발생하는데 이는 **데이터를 추가하거나 갱신할 경우 동일한 테이블을 서브쿼리에서 사용할 수 없도록 되어 있기 때문**이다 에러를 발생하지 않고 실행하려면 다음과 같이 인라인 뷰로 임시 테이블을 만들도록 처리하면 된다.

![https://yongineer.duckdns.org/sql/til46_20.png](https://yongineer.duckdns.org/sql/til46_20.png)

한편, SQL에서 변수를 사용할 수 있다고 한다면, 다음과 같이 정리해 표현 할 수 있다.

```sql
변수 = (SELECT MIN(a) FROM sample54);
DELETE FROM sample54 WHERE a = 변수;
```

> ⚠️ `MySQL`에서의 변수
>
`MySQL`에서는 다음과 같이 변수를 구현 한다. 이때 `@a`가 변수가 되고 `set`이 이 변수에 대입하는 명령이 된다.
>
>    ```sql
>    set @a = (SELECT MIN(a) FROM sample54;
>    DELETE FROM sample54 WHERE a = @a;
>    ```

## 2. 스칼라 값

다음과 같은 네 가지가 일반적인 서브쿼리 패턴이다.

1. **하나의 값을 반환하는 패턴**

    ![https://yongineer.duckdns.org/sql/til46_21.png](https://yongineer.duckdns.org/sql/til46_21.png)

2. **복수의 행이 반환되지만 열은 하나인 패턴**

    ![https://yongineer.duckdns.org/sql/til46_22.png](https://yongineer.duckdns.org/sql/til46_22.png)

3. **하나의 행이 반환되지만 열이 복수인 패턴**

    ![https://yongineer.duckdns.org/sql/til46_23.png](https://yongineer.duckdns.org/sql/til46_23.png)

4. 복수의 행, 복수의 열이 반환되는 패턴

    ![https://yongineer.duckdns.org/sql/til46_24.png](https://yongineer.duckdns.org/sql/til46_24.png)

위와 같은 패턴들 중에서 1️⃣만 다른 패턴과 다르다. 이는 다른 패턴과 **달리 하나의 값을 반환**하기 때문에 '단일 값'으로도 통용되지만 데이터베이스에서는 **'스칼라 값'**이라 불린다.

> 💡`SELECT` 명령이 하나의 값만 반환하는 것을 '스칼라 값을 반환한다'고 한다.
>
스칼라 값을 특별 취급 하는 이유는 서브쿼리로서 사용하기 쉽기 때문이다. 스칼라 값을 반환하도록 `SELECT` 명령을 작성하고자 한다면 `SELECT`구에서 단일 열을 지정한다.

`SELECT`구에서 하나의 열을 지정하고 `GROUP BY`를 지정하지 않은 채 집계함수를 사용하면 결과는 단일한 값이 된다. 만약 `GROUP BY`로 그룹화를 하면 몇 가지 그룹으로 나뉘어져 버릴 가능성이 있기 때문에 결과적으로 단일한 값이 반환되지 않을 수 있다. 또한, `WHERE` 조건으로 하나의 행만 검색할 수 있다면 단일 값이 되므로 스칼라 값을 반환하는 `SELECT` 명령이 된다.

서브쿼리로 사용하기 간편한 통상적인 이유는 특정한 두 가지가 서로 동일한지 여부를 비교할 때는 서로 단일한 값으로 비교한다. 즉, `WHERE`구에서 스칼라 값을 반환하는 서브쿼리는 `=` 연산자로 비교 할 수 있다는 뜻이다.

```sql
DELETE FROM sample54 WHERE a = (SELECT MIN(a) FROM sample54);
```

위와 같은 명령에서 서브쿼리 부분은 스칼라 값을 반환하는 `SELECT`명령으로 되어 있으므로 `=` 연산자를 사용해 열 `a`의 값과 비교 할 수 있다.

> 💡`=`연산자를 사용하여 비교할 경우에는 스칼라 값끼리 비교할 필요가 있다.
>
스칼라 값을 반환하는 서브쿼리를 특별히 **'스칼라 서브쿼리'**라 부르기도 한다. '스칼라 서브쿼리'라면 `WHERE`구에 사용할 수 있으므로 집계함수를 사용해 집계한 결과를 조건식으로 사용할 수 있다. 또한, 하나의 그룹에 다른 값이 여러 개 존재할 경우는 스칼라 값이라고 할 수 없다.

## 3. SELECT 구에서 서브쿼리 사용하기

서브쿼리는 `SELECT`구, `UPDATE`의 `SET`구등 다양한 구 안에서 지정 할 수 있다. 왜냐하면 문법적으로 서브쿼리는 '하나의 항목'으로 취급한다. 단, 서브쿼리를 사용할 때는 스칼라 서브쿼리로 되어 있는지 확인해야 한다. 

`SELECT`구에서 서브쿼리를 지정할 때는 스칼라 서브쿼리가 필요하다.

![https://yongineer.duckdns.org/sql/til46_25.png](https://yongineer.duckdns.org/sql/til46_25.png)

서브쿼리가 아닌 상부의 `SELECT`명령에는 `FROM`구가 없다는 것이다. `MySQL`에선 `FROM`구를 생략할수 있다 하지만 `Oracle`과 같은 RDBMS에서는 `FROM`을 생략할 수 없다.

> ⚠️ `Oracle`에서 `SELECT`구에서 서브쿼리 사용하기
>
>    ```sql
>    SELECT
>        (SELECT COUNT(*) FROM sample51) AS sq1,
>        (SELECT COUNT(*) FROM sample54) AS sq2 FROM DUAL;
>    ```

## 4. SET 구에서 서브쿼리 사용하기

`SET`구에서 서브 쿼리를 사용해 갱신한다고 가정 했을때 다음과 같은 명령어를 사용할 수 있다.

```sql
UPDATE sample54 SET a = (SELECT MAX(a) FROM sample54);
```

**`SET`구에서 서브쿼리를 사용할 경우에도 스칼라 값을 반환하도록 스칼라 서브쿼리를 지정할 필요가 있다.** 단, 위 명령은 `UPDATE`명령을 실행하면 `a` 열 값이 모두 `a`열의 최대값으로 갱신된다 사실 이런 명령은 잘 쓰이지 않으며 서브쿼리는 상부의 `UPDATE`명령과 관련이 있는 조건식으로 지정되지 않으면 별 의미가 없다.

## 5. FROM 구에서 서브쿼리 사용하기

`FROM`구에 서브쿼리를 지정하는 경우에도 서브쿼리의 기술방법은 같다. `()`로 `SELECT`명령을 묶으면 된다. 다만 `FROM`구에는 기본적으로 테이블을 지정하는 만큼 다른 구와는 조금 상황이 다르다.

`SELECT`구나 `SET`구에서는 스칼라 서브쿼리를 지정해야 하지만 **`FROM`구에서는 스칼라 값을 반환하지 않아도 된다.** 물론 스칼라 값이라도 상관은 없다.

![https://yongineer.duckdns.org/sql/til46_26.png](https://yongineer.duckdns.org/sql/til46_26.png)

`SELECT`명령 안에 `SELECT`명령이 들어있는 듯 보입니다. 이를 **'nested 구조', '중첩구조' 또는 '내포구조'라 부른다.** 또한 `FROM`구에서는 테이블이나 **서브쿼리에 별명을 붙일 수 있다.** 이렇게 별명을 붙이는 것으로 비로소 서브쿼리의 이름을 지정한다. 이 때도 `SELECT`구에서 별명을 붙일 때처럼 `AS`키워드를 사용한다.

![https://yongineer.duckdns.org/sql/til46_27.png](https://yongineer.duckdns.org/sql/til46_27.png)

중첩구조는 몇 단계로든 구성 할 수 있다.

![https://yongineer.duckdns.org/sql/til46_28.png](https://yongineer.duckdns.org/sql/til46_28.png)

## 6. INSERT명령과 서브쿼리

`INSERT`명령과 서브쿼리를 조합해 사용할 수 있는데 INSERT명령에는 `VALUES`구의 일부로 서브쿼리를 사용하는 경우와, `VALUES`구 대신 `SELECT`명령을 사용하는 두 가지 방법이 있다.

먼저 `VALUES`구의 값으로 서브쿼리를 사용하는 예를 살펴보겠다. 이때 서브쿼리는 스칼라 서브쿼리로 지정할 필요가 있다. 물론 자료형도 일치해야 한다.

![https://yongineer.duckdns.org/sql/til46_29.png](https://yongineer.duckdns.org/sql/til46_29.png)

### INSERT SELECT

VALUES구 대신 SELECT명령을 사용하는 예를 살펴보겠다.

![https://yongineer.duckdns.org/sql/til46_30.png](https://yongineer.duckdns.org/sql/til46_30.png)

위 예제는 흔히 **`INSERT SELECT`**라 불리는 명령으로 `INSERT`와 `SELECT`을 합친 것과 같은 명령이 되었다. `SELECT`가 결과값으로 1과 2라는 상수를 반환하므로 이는 다음과 같다.

```sql
INSERT INTO sample541 VALUES (1,2)
```

이때 `SELECT`명령이 반환하는 값이 꼭 스칼라 값일 필요는 없으며 열 수 와 자료형이 `INSERT`대상 테이블과 일치하기만 하면 된다.

`INSERT SELECT`명령은 `SELECT`명령의 결과를 `INSERT INTO`로 지정한 테이블에 전부 추가 된다. 이 때문에 **데이터의 복사나 이동할 때 자주 사용하는 명령이다.**

열 구성이 똑같은 테이블 사이에는 다음과 같은 INSERT SELECT 명령으로 행을 복사 할 수도 있다.

```sql
INSERT INTO sample542 SELECT * FROM sample543;
```

# 상관 서브쿼리

## 1. EXISTS

```sql
EXISTS (SELECT 명령)
```

서브쿼리를 사용해 검색할 때 **'데이터가 존재하는지 아닌지' 판별하기 위해 조건을 지정**할 수도 있다. 이런 경우 `EXISTS`술어를 사용해 조사 할 수 있다.

![https://yongineer.duckdns.org/sql/til46_31.png](https://yongineer.duckdns.org/sql/til46_31.png)

위와 같이 `sample551`테이블에는 1~5까지의 데이터가 저장되어 있다. 한편 `sample552`에는 3과 5가 저장되어 있다. 여기서 `sample551`테이블의 `a`열은 문자열형이지만 모두 `NULL`이다. 이 중 `sample552`테이블의 `no2`열의 값과 같은 행이 있다면 `a`열을 '있음'이라는 값으로 `UPDATE`하려 한다. 이 때 다음과 같이 `EXISTS`를 사용하면 조건에 맞는 행을 갱신할 수 있다.

![https://yongineer.duckdns.org/sql/til46_32.png](https://yongineer.duckdns.org/sql/til46_32.png)

서브쿼리 부분이 `UPDATE`의 `WHERE`구로 행을 검색할 때 마다 차례로 실행된다. 서브쿼리의 `WHERE`구는 `no2 = no`라는 조건식으로 되어 있는데 `no2`는 `sample552`테이블의 열이고 `no`는 `sample551`테이블의 열이다. 이때 `no`가 3과 5일때만 서브쿼리가 행을 반환하게 된다.

EXISTS술어에 서브쿼리를 지정하면 서브쿼리가 행을 반환할 경우 참을 돌려준다. 결과가 한 줄이라도 그 이상이라도 참이된다. 반면 반환되는 행이 없을 경우에는 거짓이 된다.

## 2. NOT EXISTS

이번에는 위 예제의 반대로 `sample552`테이블의 `no2`열의 값과 다른 값이 있다면 `sample551`테이블의 `a`열의 값을 '없음'으로 `UPDATE`하려 한다. 이 때 다음과 같이 `NOT EXISTS`를 사용하여 거짓을 반환하는 행을 갱신 할 수 있다.

![https://yongineer.duckdns.org/sql/til46_33.png](https://yongineer.duckdns.org/sql/til46_33.png)

## 3. 상관 서브쿼리

서브쿼리에는 명령 안에 중첩구조로 된 SELECT 명령이 존재한다. 앞선 예제 명령을 다시 살펴 보자

```sql
UPDATE sample551 SET a = '있음' WHERE
	EXISTS (SELECT * FROM sample552 WHERE no2 = no);
```

`UPDATE` 명령 (부모)에서 `WHERE`구에 `()`로 묶은 부분이 서브쿼리(자식)이 되며 부모 명령에서는 `sample551`테이블을 갱신하고 자식 명령에서는 `sample552`테이블의 `no2`열 값이 부모의 `no`열 값과 일치하는 행을 검색한다.

이처럼 **부모 명령과 자식인 서브쿼리가 특정 관계를 맺는 것을 '상관 서브쿼리'**라 부른다.

```sql
DELETE FROM sample54 WHERE a = (SELECT MIN(a) FROM sample54);
```

앞선 예제 명령인 위 명령의 경우 상관 서브쿼리가 아니다. 상관 서브쿼리가 아닌 단순한 서브쿼리는 단독 쿼리로 실행 할 수 있기 때문이다. 하지만 **상관 서브쿼리에서는 부모 명령과 연관되어 처리되기 때문에 서브쿼리 부분만 따로 떼어내어 실행 시킬 수 없다.**

![https://yongineer.duckdns.org/sql/til46_34.png](https://yongineer.duckdns.org/sql/til46_34.png)

### 테이블명 붙이기

앞선 예제에서 `no`와 `no2`의 열 명이 모두 같은 이름이라면 즉, `WHERE no = no`라고 건을 지정하면 제대로 동작하지 않는다. 특히 `MySQL`에서는 `WHERE sample552.no = sample552.no`가 되어 조건식은 항상 참이되어 결과적으로 `sample551`의 `a`열의 모든 값은 '있다'로 갱신될 것이다.

따라서 열이 어느 테이블의 것인지 명시적으로 나타낼 필요가 있다. 예를들어 `no`열이 `sample551`의 것이라면 `sample551.no`라고 지정해야 한다. 마찬가지로 `no2`의 경우에도 `sample552.no2`라고 지정해야 한다. 이렇게 하면 열 이름이 같아도 구별되므로 문제없이 실행할 수 있다.

```sql
UPDATE sample551 SET a = '있음' WHERE
	EXISTS (SELECT * FROM sample552 WHERE sample552.no2 = sample551.no);
```

## 4. IN

```sql
열명 IN (집합)
```

`IN`은 집합 안의 값이 존재하는지 조사 할 수 있다. 특정 열의 값이 **'무엇 또는 (`OR`) 무엇'이라는 조건식**을 지정하는 경우 `IN`을 사용하면 간단하게 지정할 수 있다.

`IN`에서는 오른쪽에 집합을 지정한다. 왼쪽에 지정된 값과 같은 값이 집합안에 존재하면 참을 반환한다. 집합은 상수리스트를 `()`로 묶어 기술한다.

![https://yongineer.duckdns.org/sql/til46_35.png](https://yongineer.duckdns.org/sql/til46_35.png)

집합 부분은 서브쿼리로도 지정 할 수 있으며 서브쿼리로 지정 할 때는 스칼라 서브쿼리가 될 필요는 없다. 다만,  **스칼라 값을 설명한 패턴에서 1️⃣ 과 2️⃣ 패턴은 사용 할 수 있지만 열이 복수로 반환되는 3️⃣ 과 4️⃣ 는 비교할 수 없게되어 사용이 불가능 하다.**

또한 `NOT IN`으로 지정하면 집합에 값이 포함되어 있지 않을 경우 참이 된다.

### IN과 NULL

`IN`은 집합안에 `NULL`값이 있어도 무시하지 않는다. 다만, `NULL = NULL`을 제대로 계산할 수 없으므로 `IN`을 사용해도 `NULL`값은 비교할 수 없다. 즉, `NULL`을 비교할 때는 `IS NULL`을 사용해야 한다.

또한 `NOT IN`의 경우 집합안에 `NULL`값이 있으면 왼쪽 값이 집합 안에 포함되어 있지 않아도 참을 반환하지 않으며 그 결과는 `UNKNOWN`이 된다. 

다만, `MySQL`에서의 `IN`은 집합에 `NULL`이 포함되어 있는 경우, 조건식 `IN`은 왼쪽 값이 집합에 포함되어 있으면 참을, 그렇지 않으면 `NULL`을 반환한다. `NOT IN`의 경우 왼쪽 값이 집합에 포함되어 있으면 거짓을, 그렇지 않으면 `NULL`을 반환하게 된다. 결국 `NOT IN`의 경우 집합에 `NULL`이 포함되어 있다면 그 결과값은 0건이 된다. `NULL`을 반환한다는 것은 비교할 수 없다는 것을 의미한다. 왼쪽의 값이 `NULL`인 경우에도 오른쪽의 값과 관계없이 비교할 수 없으므로 조건식은 참 또는 거짓이 아닌 `NULL`을 반환한다.

# Question

## 1. 테이블의 행 개수를 알고 싶을 때 사용하는 집계함수

- COUNT

## 2. SELECT명령에 GROUP BY를 지정한 경우 데이터베이스 내부에서 수행되는 처리

- 그룹화

## 3. SELECT구에서 지정할 수 있는 서브쿼리

- 스칼라 서브쿼리
