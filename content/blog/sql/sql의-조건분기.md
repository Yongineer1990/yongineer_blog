---
title: SQL의 조건분기
date: 2020-11-02 00:11:04
category: SQL
thumbnail: ../TIL/images/sql.png
draft: false
---

![](../TIL/images/sql.png)

# SQL과 조건분기

SQL은 코드를 절차적으로 기술하는 것이 아니므로 조건 분기를 '문장' 단위로 하지 않고 '식'기준으로 조건 분기를 수행한다. 그리고 이런 식의 분기를 실현하는 기능이 바로 `CASE`식이다.

## 1. CASE식의 구문

```sql
CASE WHEN [평가식] THEN [식]
		 WHEN [평가식] THEN [식]
		 WHEN [평가식] THEN [식]
				...
		 ELSE [식]
END
```

`CASE`식의 구문에는 단순 `CASE`식과 검색 `CASE`식이라는 두 종류가 있다. 다만 검색 `CASE` 식은 단순 `CASE`식의 기능을 모두 포함하고 있으므로 검색 `CASE`만 다루도록 하겠다.

여기서 `WHEN`구의 평가식이라는 것은 `WHERE`의 조건식과 동일하다.

## 2. CASE 식의 작동

처음에 있는 `WHEN`구의 평가식부터 평가되고 조건이 맞으면 `THEN`구에서 지정한 식이 리턴되며 `CASE`식 전체가 종료 된다. 만약 조건이 맞지 않으면 다음 `WHEN`구로 이동해 같은 처리를 반복한다. 마지막 `WHEN`구까지 반복했는데도 조건이 맞는 경우가 없다면 ELSE`에서` 지정한 식이 리턴되며 끝난다.

절차 지향형 언어의 조건 분기와 SQL 조건 분기 사이에 가장 큰 차이점은 바로 리턴값이다. 절차 지향형 언어의 조건 분기는 문장을 실행하고 딱히 리턴하지는 않는다. 반면 SQL의 조건 분기는 특정한 값(상수)를 리턴한다.

![https://yongineer.duckdns.org/sql/055.png](https://yongineer.duckdns.org/sql/055.png)

예를들어 위와 같은 결과를 도출할때 `district`필드는 다음과 같이 `CASE` 식으로 구할 수 있다.

```sql
SELECT name, address,
	CASE WHEN address = '서울시' THEN '경기'
	CASE WHEN address = '인천시' THEN '경기'
	CASE WHEN address = '부산시' THEN '영남'
	CASE WHEN address = '속초시' THEN '관동'
	CASE WHEN address = '서귀포시' THEN '호남'
	ELSE NULL END AS district
FROM Address;
```

**`CASE` 식의 강력한 점은 식이라는 것이다. 따라서 식을 적을 수 있는 곳이라면 어디든지 적을 수 있다.** `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`구와 같은 곳 어디에나 적을 수 있으므로 다양한 기법으로 활용할 수 있다. `CASE`식은 SQL의 성능과도 굉장히 큰 관련이 있다.

# UNION을 사용한 쓸데없이 긴 표현

UNION을 사용한 조건분기는 성능적인 측면에서 굉장히 큰 단점을 가지고 있다. 외부적으로는 하나의 SQL 구문을 실행하는 것처럼 보이지만, 내부적으로는 여러 개의 `SELECT` 구문을 실행하는 실행 계획으로 해석되기 때문이다. 따라서 테이블에 접근하는 횟수가 많아져서 `I/O` 비용이 크게 늘어난다.

## 1. UNION을 사용한 조건 분기와 관련된 간단한 예제

![https://yongineer.duckdns.org/sql/056.png](https://yongineer.duckdns.org/sql/056.png)

일단 상품을 관리하는 items 테이블이 있다. 이 테이블은 각각의 상품에 대해서 세금이 포함된 가격과 세금이 포함되지 않은 가격을 모두 저장한다. 그런데 2002년부터 법이 개정되면서 세금이 포함된 가격을 표시하는게 의무가 되었다. 따라서 2001년까지는 세금이 포함되지 않은 가격(price\_tax\_ex)을, 2002년 부터는 세금이 포함된 가격(price\_tax\_in)을 price필드로 표시하게 되어 구하고자 하는 결과는 다음과 같다.

![https://yongineer.duckdns.org/sql/057.png](https://yongineer.duckdns.org/sql/057.png)

이때 조건 분기에 year필드를 사용해야 하겠다는 건 쉽게 알 수 있을 것이다. UNION을 사용하면 다음과 같이 이런 조건 분기를 만들 수 있다.

```sql
# UNION을 사용한 조건 분기
SELECT item_name, year, price_tax_as AS price
		FROM items
	WHERE year <= 2001
SELECT item_name, year, price_tax_in AS price
		FROM items
WHERE year >= 2002;
```

다만, 이 코드는 굉장히 큰 문제점을 안고 있다. 첫 번째 문제는 쓸데없이 길다는 것이다, 거의 같은 두 개의 쿼리를 두 번이나 실행하고 있다. 이는 SQL을 쓸데없이 길고 읽기 힘들게 만들 뿐이다. 또한 두 번째 문제는 성능이다.

### UNION을 사용했을 때의 실행 계획 문제

![https://yongineer.duckdns.org/sql/058.png](https://yongineer.duckdns.org/sql/058.png)

`UNION` 쿼리는 items 테이블에 2회 접근한다는 것을 알 수 있다. 그리고 그때마다 Table full scan이 발생하므로 (`type : ALL`) 읽어들이는 비용도 테이블의 크기에 따라 선형으로 증가하게 된다.

### 정확한 판단 없는 UNION 사용 회피

정확한 판단 없이 SELECT 구문 전체를 여러 번 사용해서 코드를 길게 만드는 것은 쓸데없는 테이블 접근을 발생시키며 SQL의 성능을 나쁘게 만든다. 또한 물리 자원(저장소의 I/O비용)도 쓸데없이 소비하게 된다.

## 2. WHERE 구에서 조건 분기를 하는 사람은 초보자

위 예시를 SELECT 구만으로 조건 분기를 하면 다음과 같이 최적화 할 수 있다.

```sql
#SELECT 구문에서 CASE 식을 사용한 조건 분기
SELECT item_name, year,
		CASE WHEN year <= 2001 THEN price_tax_ex
				 WHEN year >= 2002 THEN price_tax_in END AS price
	FROM items;
```

위 쿼리도 UNION을 사용한 쿼리와 같은 결과를 출력한다. 하지만 성능적으로 위 쿼리가 훨씬 좋다.

## 3. SELECT 구를 사용한 조건 분기의 실행 계획

![https://yongineer.duckdns.org/sql/059.png](https://yongineer.duckdns.org/sql/059.png)

item테이블에 대한 접근이 1회로 줄어든 것을 확인할 수 있다. 뿐만 아니라 SQL 구문 자체의 가독성도 굉장히 좋았졌다.

> 💡`UNION`과 `CASE` 비교
>
`UNION`을 사용한 분기는 `SELECT` 구문을 기본 단위로 분기하고 있다. 구문을 기본 단위로 사용하고 있다는 점에서, 아직 절차 지향형의 발상을 벗어나지 못한 방법이라고 말할 수 있다.
>
반면 `CASE`식을 사용한 분기는 문자 그대로 '식'을 바탕으로 하는 사고이다. 이렇게 '구문'에서 '식'으로 사고를 변경하는 것이 SQL을 마스터하는 열쇠 중 하나이다.

# 집계와 조건 분기

먼저 다음과 같은 population테이블이 있다고 가정해 보자

![https://yongineer.duckdns.org/sql/060.png](https://yongineer.duckdns.org/sql/060.png)

이 테이블을 아래와 같은 레이아웃으로 변경하는 방법을 생각해 보자 성별은 1은 남성 2는 여성을 의미한다고 가정한다.

![https://yongineer.duckdns.org/sql/061.png](https://yongineer.duckdns.org/sql/061.png)

## 1. 집계 대상으로 조건 분기

### UNION을 사용한 방법

일단 남성의 인구를 지역별로 구하고, 여성의 인구를 지역별로 구한 뒤 머지(`merge`)하는 방법을 생각할 것이다.

```sql
# UNION을 사용한 방법

SELECT prefecture, SUM(pop_men) AS pop_men, SUM(pop_wom) AS pop_wom
	FROM (SELECT prefecture, pop AS pop_men, null AS pop_wom
				FROM population
			WHERE sex = 1 # 남성
			UNION
		SELECT prefecture, NULL AS pop_men, pop AS pop_wom
				FROM population
			WHERE sex = 2) TMP #여성
GROUP BY prefecture;
```

이러한 쿼리의 가장 큰 문제는 `WHERE`구에서 sex필드로 분기를 하고, 결과를 `UNION`으로 머지한다는 절차 지향적인 구성에 있다.

### UNION의 실행 계획

![https://yongineer.duckdns.org/sql/062.png](https://yongineer.duckdns.org/sql/062.png)

실행 계획을 보면 popolation테이블에 테이블 풀 스캔이 2회 수행되는 것을 확인할 수 있다.

### 집계의 조건 분기도 CASE 식을 사용

이 문제는 CASE식의 응용 방법으로 굉장히 유명한 표측/표두 레이아웃 이동 문제이다. CASE식을 집약 함수 내부에 포함시켜서 '남성 인구'와 여성 인구' 필터를 만든다.

```sql
# CASE 식을 사용한 방법
SELECT prefecture,
		SUM(CASE WHEN sex = 1 THEN pop ELSE 0 END) AS pop_men,
		SUM(CASE WHEN sex = 2 THEN pop ELSE 0 END) AS pop_wom
	FROM population
GROUP BY prefecture
```

> 💡표두, 표측
>
>- 표두(head, column)는 이차원 표의 상부 제목
>- 표측(stub, row)는 이차원 표의 좌측의 제목

### CASE식의 실행 계획

![https://yongineer.duckdns.org/sql/063.png](https://yongineer.duckdns.org/sql/063.png)

population 테이블로의 풀 스캔이 1회로 감소한 것을 확인할 수 있다. 따라서 UNION을 사용한 경우의 2회에 비해 (캐시 등을 고려하지 않는다면) I/O비용이 절반으로 감소한다는 의미이다.

## 2. 집약 결과로 조건 분기

예를들어 직원과 직원이 소속된 팀을 관리하는 employees테이블이 있다고 가정하자

![https://yongineer.duckdns.org/sql/064.png](https://yongineer.duckdns.org/sql/064.png)

여기서 다음과 같은 조건에 맞춰 결과를 만드는 것을 생각해보자 

1. 소속된 팀이 1개라면 해당 직원은 팀의 이름을 그대로 출력한다.
2. 소속된 팀이 2개라면 해당 직원인 '2개를 겸무' 라는 문자열을 출력한다.
3. 소속된 팀이 3개 이상이라면 해당 직원은 '3개 이상을 겸무'라는 문자열을 출력한다.

![https://yongineer.duckdns.org/sql/065.png](https://yongineer.duckdns.org/sql/065.png)

### UNION을 사용한 조건 분기

직원을 조건 1️⃣~3️⃣에따라 분류한다면 다음과 같다.

1. jim, carl
2. kim
3. bree, joe

이러한 분류를 UNION을 사용한다면 다음과 같다.

```sql
# UNION으로 조건 분기

SELECT emp_name, MAX(team) AS team
		FROM employees
	GROUP BY emp_name
HAVING COUNT(*) = 1
UNION
SELECT emp_name, '2개를 겸무' AS team
		FROM employees
	GROUP BY emp_name
HAVING COUNT(*) = 2
UNION
SELECT emp_name, '3개 이상을 겸무' AS team
		FROM employees
	GROUP BY emp_name
HAVING COUNT(*) >= 3
```

이 문제는 조건 분기가 레코드 값이 아닌, 집합의 레코드 수에 적용된다는 것이다. 따라서 조건 분기가 WHERE구가 아니라 HAVING구에 지정되었다. 하지만 UNION으로 머지하고 있는 이상, 구문 레벨의 분기일 뿐이다. 따라서 WHERE구를 사용할 때와 크게 다르지 않다.

### UNION의 실행 계획

![https://yongineer.duckdns.org/sql/066.png](https://yongineer.duckdns.org/sql/066.png)

3개의 쿼리를 머지하는 쿼리이므로 예상대로 employees테이블에 대한 접근도 3회이며 모두 테이블 풀 스캔을 수행하는 것을 확인할 수 있다.

### CASE식을 사용한 조건 분기

```sql
# SELECT구와 CASE식을 사용
SELECT emp_name,
		CASE WHEN COUNT(*) = 1 THEN MAX(team)
				 WHEN COUNT(*) = 2 THEN '2개를 겸무'
				 WHEN COUNT(*) >= 3 THEN '3개 이상을 겸무'
		END AS team
	FROM employees
GROUP BY emp_name; 
```

### CASE 식을 사용한 조건 분기의 실행 계획

![https://yongineer.duckdns.org/sql/067.png](https://yongineer.duckdns.org/sql/067.png)

`CASE`식을 사용하면 테이블에 접근 비용을 3/1로 줄일수 있다. 추가적으로 `GROUP BY`의 `HASH` 연산도 3/1로 줄어든다. 이를 가능하게 하는 것은 집약 결과(`COUNT` 함수의 리턴값)을 `CASE` 식의 입력으로 사용했기 때문이다. `COUNT` 또는 `SUM`과 같은 집약 함수의 결과는 1개의 레코드로 압축된다. 다르게 말하면 집약함수의 결과가 스칼라값이 되는 것이다. 따라서 `CASE`식의 매개변수에 집약함수를 넣을 수 있다.

앞서 `WHERE`구에서 조건 분기를 하는 것은 초보자라고 했는데 마찬가지로 `HAVING`구에서 조건분기를 하는것도 조보자라는 것을 명심하자.

# 그래도 UNION이 필요한 경우

## 1. UNION을 사용할 수밖에 없는 경우

머지 대상이 되는 `SELECT` 구문들에서 사용하는 테이블이 다른 경우가 대표적이다. 쉽개 말하면 여러 개의 테이블에서 검색한 결과를 머지하는 경우다.

```sql
# 다른 테이블의 결과를 머지
SELECT col_1 FROM table_a WHERE col_2 = 'A'
UNION ALL
SELECT col_3 FROM table_b WHERE col_4 = 'B';
```

물론 그렇다고 `CASE`식을 사용할 수 없다는 것은 아니다. `FROM`구에서 테이블을 결합한다면 `CASE` 식을 사용해 원하는 결과를 구할 수도 있다. 하지만 그렇게 하면 필요 없는 결합이 발생해서 성능적으로 악영향을 발생한다. (`UNION`을 사용한다면 발생하지 않는다). 따라서 실행 계획 등을 확인해서 어떤 것이 더 좋은지 명확하게 확인해줘야 한다.

## 2. UNION을 사용하는 것이 성능적으로 더 좋은 경우

인덱스와 관련된 경우, `UNION`을 사용했을 때 좋은 인덱스(압축을 잘 하는 인덱스)를 사용하지만, 이외의 경우에는 테이블 풀 스캔이 발생한다면 `UNION`을 사용한 방법이 성능적으로 더 좋을 수 있다.

![https://yongineer.duckdns.org/sql/068.png](https://yongineer.duckdns.org/sql/068.png)

three_elements 테이블에 저장된 레코드를 보면 레코드는 date, flg이라는 3개의 짝에서 하나의 짝에만 값이 있고, 다른 짝은 모두 `NULL`이다.

이 테이블에서 date\_1 ~ date\_3이 특정 날짜 (예를 들어 2013년 11월 1일)를 값으로 갖고 있고 대칭되는 플래그 필드의 값이 '`T`'인 레코드를 선택한다고 가정하자.

![https://yongineer.duckdns.org/sql/069.png](https://yongineer.duckdns.org/sql/069.png)

### UNION을 사용한 방법

```sql
# UNION을 사용한 방법

SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
	FROM three_elements
		WHERE date_1 = '2013-11-01' AND flg_1 = 'T'
UNION
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
	FROM three_elements
		WHERE date_2 = '2013-11-01' AND flg_2 = 'T'
UNION
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
	FROM three_elements
		WHERE date_3 = '2013-11-01' AND flg_3 = 'T'
```

머지되는 3개의 `SELECT` 구문에서 다른 것은 `WHERE`구 뿐이다. 쿼리는 원하는 결과를 만들므로 기능적으로 충반하다. 그렇다면 문제는 바로 성능과 실행계획이다.

이때 포인트가 인덱스이다. 이 쿼리를 최적의 성능으로 수행하려면 다음과 같은 필드 조합에 인덱스가 필요하다.

```sql
CREATE INDEX idx_1 ON three_elements (date_1, flg_1);
CREATE INDEX idx_2 ON three_elements (date_2, flg_2);
CREATE INDEX idx_3 ON three_elements (date_3, flg_3);
```

이제 실행계획을 살펴보면 다음과 같다.

![https://yongineer.duckdns.org/sql/070.png](https://yongineer.duckdns.org/sql/070.png)

3개의 `SELECT`구문 모두 idx\_1, idx\_2, idx\_3이라는 인덱스가 사용되는 것을 확인할 수 있다. 이렇게되면 three_elements테이블의 레코드 수가 많고, 각각의 `WHERE`구의 검색 조건에서 레코드 수를 많이 압축할수록, 테이블의 풀 스캔보다도 훨씬 빠른 접근 속도를 기대할 수 있다.

### OR을 사용한 방법

```sql
# OR을 사용한 방법
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
	FROM three_elements
		WHERE (date_1 = '2013-11-01' AND flg_1 = 'T')
		OR (date_2 = '2013-11-01' AND flg_2 = 'T')
		OR (date_3 = '2013-11-01' AND flg_3 = 'T');
```

이러한 쿼리의 결과는 `UNION`을 사용했을 때와 같다. 하지만 실행 계획이 크게 다르다. (`Oracle`)

![https://yongineer.duckdns.org/sql/071.png](https://yongineer.duckdns.org/sql/071.png)

`SELECT` 구문이 하나로 줄어들었기 때문에 three_elements테이블에 대한 접근이 1회로 줄었다. 하지만 이때 인덱스가 사용되지 않고, 그냥 테이블 풀 스캔이 수행된다. 이렇게 `WHERE`구문에서 `OR`을 사용하면 해당 필드에 부여된 인덱스를 사용할 수 없다.

![https://yongineer.duckdns.org/sql/072.png](https://yongineer.duckdns.org/sql/072.png)

다만 `MySQL`에서는 `Index merge`가 수행되는데 테이블 풀 스캔보단 성능적으로는 더 낫지만 각각의 조건으로 검색한 데이터를 병합하는 과정이 생기기 때문에 인덱스만으로 연산을 먼저 수행하고 그 결과를 바탕으로 데이터를 가져오는게 더 효율적이다.

`UNION`과 `OR`의 성능 비교는 결국 **3회의 인덱스 스캔 VS 1회의 테이블 풀 스캔** 중에서 어떤 것이 더 빠른지에 대한 문제가 된다. 하지만 테이블이 크고 `WHERE` 조건으로 선택되는 레코드의 수가 충분히 작다면 `UNION`이 더 빠르다. 따라서 `UNION`을 사용하는 경우가 더 빠를 수도 있다.

### IN을 사용한 방법

```sql
# IN을 사용한 방법

SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
	FROM three_elements
		WHERE ('2013-11-01', 'T')
			IN ((date_1, flg_1), (date_2, flg_2), (date_3, flg_3));
```

![https://yongineer.duckdns.org/sql/073.png](https://yongineer.duckdns.org/sql/073.png)

이는 다중 필드(multiple fields) 또는 행식(row expression)이라는 기능을 사용한 방법이다. `IN`의 매개변수로는 단순한 스칼라뿐만 아니라 이렇게 (a,b,c)와 같은 값의 리스트(배열)을 입력할 수도 있다.

`OR`을 사용했을때 보다 간단하고 이해하기 쉬울 수 있지만 `OR`과 마찬가지로 테이블 풀 스캔을 수행하며 성능적인 문제도 같다.

### CASE를 사용한 방법

```sql
# CASE를 사용한 방법
SELECT key, name, date_1, flg_1, date_2, flg_2, date_3, flg_3
	FROM three_elements
			WHERE CASE WHEN date_1 = '2013-11-01' THEN flg_1
								 WHEN date_2 = '2013-11-01' THEN flg_2
								 WHEN date_3 = '2013-11-01' THEN flg_3
								 ELSE NULL END = 'T';
```

![https://yongineer.duckdns.org/sql/073.png](https://yongineer.duckdns.org/sql/073.png)

이 쿼리도 원하는 결과를 만들어 내나 실행 계획은 `OR`, `IN`을 사용할 때와 같다. 따라서 성능적으로 같은 문제를 안게된다.

# 절차 지향형과 선언형

## 1. 구문 기반과 식 기반

SQL초보자가 `UNION`을 사용해 조건 분기를 하는 이유는 간단하다. `UNION`으로 연결하는 대상은 `SELECT`구문이다. 따라서 절차 지향형 프로그래밍 언어에 익숙한 사람들에게 굉장히 익숙한 방식이다.

하지만 SQL의 기본적인 체계는 선언형이다. 절차 지향형 언어가 `CASE`구문으로 분기하는 것을, SQL은 `CASE`식으로 분기한다. SQL 구문의 각 부분(`SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`)에 작성하는 것은 모두 식이다. 열 이름 또는 상수만 기술하는 경우에도 마찬가지이다. SQL 구문 내부에서는 식을 작성하지, 구문을 작성하지는 않는다.

## 2. 선언형의 세계로 도약

절차 지향형 세계에서 선언형 세계로 도약하는 것이 곧 SQL능력 향상의 핵심이다.

# 정리

- SQL의 성능은 저장소의 `I/O`를 얼마나 감소 시킬지 있을지가 키포인트
- `UNION`에서 조건 분기를 표현한다면 "내가 지금 쓸데없이 길게 쓰고 있는 것은 아닐까?"라는 것을 항상 의식할 것
- `IN` 또는 `CASE`식으로 조건 분기를 표현할 수 있다면, 테이블에서의 스캔을 크게 감소 시킬 가능성이 있다.
- 이를 위해서라도, 구문에서 식으로의 패러다임 전환을 연습할 필요가 있다.
