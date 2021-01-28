---
title: TIL.45) DB indexing
date: 2020-09-08 03:09:31
category: Database
thumbnail: '../TIL/images/database.png'
draft: false
---

![](../TIL/images/database.png)

# 왜 DB indexing을 할까?

index란 색인 기능이며 **원하는 결과를 빠르게 찾기** 위해 사용한다. 즉 `CRUD`에서 `Read`성능을 향상 시키기 위해서 사용한다. 만약 index가 되어 있지 않은 테이블에서 특정 값을 찾으려고 한다면 데이터베이스는 **테이블의 모든 값에서 내가 원하는 값을 찾으려고 할텐데** 이를 `Full table scan`이라 한다. 이러한 scan은 테이블의 데이터가 수십만개, 혹은 수백만개라면 그리고 심지어 이런 조회가 자주 일어난다면 심각한 성능 저하가 발생할 것이다.

그래서 데이터베이스에서 이러한 문제를 방지하고자 인덱스라는 기능을 지원하며 이 기능을 통해 검색 수행시 **어떤 값에 대해서도 같은 시간의 결과를 얻을 수 있는 균일성을 유지 할 수 있게 된다.**

![https://yongineer.duckdns.org/database/til45_03.png](https://yongineer.duckdns.org/database/til45_03.png)

# DB index?

- **내가 원하는 값과 그 값의 위치 (즉, `Primary Key`)를 따로 테이블로 관리**
- 읽기 성능을 향상 시키기 위한 일종의 Key-Value 자료구조
- 인덱스는 원본 테이블과 별도로 저장되며 **index로 설정한 컬럼값이 수정, 삭제 된다면 인덱스도 반영된다.**
- Primary Key에는 데이터베이스가 자동으로 인덱스 기능을 설정하여 관리한다.

![https://yongineer.duckdns.org/database/til45_01.png](https://yongineer.duckdns.org/database/til45_01.png)

## 인덱싱한 테이블에서 어떻게 값을 찾을까?

인덱스 테이블은 다음과 같은 순서로 데이터를 찾는다.

1. 인덱스 테이블에서 원하는 값을 검색한다.
2. 해당 값의 위치 (PK)들을 가져온다.
3. 가져온 위치들의 값으로 원본 테이블의 row를 조회한다.

# 데이터베이스의 인덱스 알고리즘

데이터베이스는 다양한 알고리즘으로 인덱스를 관리하는데 **MySQL에선 일반적으로 사용하는 알고리즘은 B+Tree 알고리즘**이다.

## B+Tree 알고리즘

![https://yongineer.duckdns.org/database/til45_02.png](https://yongineer.duckdns.org/database/til45_02.png)

### B+Tree 알고리즘의 구성

1. 리프노드
    - 실제 데이터가 저장되는 노드
    - Key와 data를 저장
2. 브랜치노드
    - 리프 노드까지의 경로 노드
    - Key만 저장
3. 루트노드
    - 출발점 노드

### B+ Tree의 장점

리프노트를 제외한 나머지는 데이터를 저장하지 않기 때문에 메모리가 더 여유롭다 따라서 **더 많은 Key들을 저장**할 수 있으며 하나의 노드에 더 많은 key를 저장할 수 있기에 **트리의 높이는 낮아진다** 때문에 cache hit를 높일 수 있다. 

또한 Full scan시 B+Tree는 리프 노드에 모든 데이터가 저장되어 있기 때문에 **한번의 선형 탐색만 수행** 하면 되기 때문에 B-Tree보다 빠르다 (B-Tree는 모든 노드를 확인 해야 한다.)

# 인덱스의 종류

## Clustered Index

- 원본 테이블당 인덱스 테이블은 하나만 생성 가능하다.
- 행 데이터를 인덱스로 지정한 열에 맞춰서 자동 정렬되어 있기 때문에 순차적 데이터를 접근 할때 유리하다.
- 사전과 같이 내용 자체가 순서대로 정렬되어 있어 인덱스 자체가 내용과 같다.

![https://yongineer.duckdns.org/database/til45_04.gif](https://yongineer.duckdns.org/database/til45_04.gif)

## Non Clustered Index

- 원본 테이블당 여러개의 인덱스 테이블을 생성 할 수 있다.
- 데이터가 정렬되어 있지 않기 때문에 인덱스에 의해 원본 데이터까지 찾아가야 한다.
- 찾아보기가 있는 일반 책과 같다.

![https://yongineer.duckdns.org/database/til45_05.gif](https://yongineer.duckdns.org/database/til45_05.gif)
