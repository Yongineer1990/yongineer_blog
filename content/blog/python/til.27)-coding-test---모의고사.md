---
title: TIL.27) Coding Test - 모의고사
date: 2020-08-01 07:08:56
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# Question

### **문제 설명**

수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...

2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...

3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한 조건

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

# 나의 해결방식

```python
import math

def solution(answers):
    a = [1, 2, 3, 4, 5]
    b = [2, 1, 2, 3, 2, 4, 2, 5]
    c = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]
    
    a = a * math.ceil(len(answers)/len(a))
    b = b * math.ceil(len(answers)/len(b))
    c = c * math.ceil(len(answers)/len(c))
    
    a_point = 0
    b_point = 0
    c_point = 0
    
    for i in range(len(answers)):
        if a[i] == answers[i]:
            a_point += 1
        if b[i] == answers[i]:
            b_point += 1
        if c[i] == answers[i]:
            c_point += 1
    
    answer = [a_point, b_point, c_point]
    top = (max(answer))
    result = (idx+1 for idx, point in enumerate(answer) if top == point)
    return list(result)
```

수포자들의 찍는 방식을 보면 패턴을 알 수 있는데

1. 수포자1번의 경우 `[1, 2, 3, 4, 5]` 를 반복한다.
2. 수포자2번의 경우 `[2, 1, 2, 3, 2, 4, 2, 5]`를 반복한다.
3. 수포자3번의 경우 `[3, 3, 1, 1, 2, 2, 4, 4, 5, 5]`를 반복한다.

하지만 이것은 정답의 패턴일뿐 실제 문제의 수는 최대 10,000개 이므로 각 문제와 답이 1:1로 일치하지 않는다. 따라서 이 패턴을 문제의 개수에 맞게 세팅 할 필요가 있고 이는 곳 수포자들의 패턴이 몇번 반복되느냐와 같다. 

즉, `수포자들의 정답 패턴 * 반복횟수`가 되며 이를 구하기 위해 `문제의 수 / 수포자들의 정답 패턴 수`가 된다. 그러나 만약 문제의 수가 수포자들의 정답 패턴수와 일치하게 되면 반복횟수는 0이 되고 이를 계산하면 빈 리스트를 반환한다 따라서 `math`모듈의 올림함수인 `ceil`을 사용했다 이러면 최소값은 1이 되고 만약 일치한다면 원래의 정답 패턴 리스트가 반환된다.

그 후 수포자들의 점수를 0으로 초기화 한 후 수포자들이 찍은 정답과 문제의 정답을 비교해 같다면 점수를 누적시키고 이를 `answer`변수에 리스트로 저장하였다. 이중 가장 큰 값을 찾아 `top`변수에 저장 후 `enumerate`함수를 이용하여 인덱스와 값을 나열 한 후 최고 점수와 같은 인덱스들을 `result`에 저장 후 문제에서 요구하는 `list`타입으로 반환하였다.

# enumerate( )

- 반복문에서 해당 반복문의 인덱스와 요소를 `tuple`타입으로 반환한다.
- `enumerate`는 반복 가능한 객체를 인자로 받는다 (`list`, `tuple`, `string`, `dictionary` 등)

```python
mylist = [10,20,30]

for idx, num in enumerate(mylist):
  print(f'{idx} : {num}')

# Result
0 : 10
1 : 20
2 : 30
```

만약 `dictionary`형태로 인자를 받는다면 반환되는 값은 `key`이다. 따라서 값이 필요하다면 `enumerate`에서 반환된 키값을 통해 인자로 전달된 `dictionary`를 조회해야 한다.

```python
mydict = {
  1 : 'one', 
  2 : 'two',
  3 : 'three'
  }

for idx, key in enumerate(mydict):
  print(f'{idx} : {key} - {mydict[key]}')

# Result
0 : 1 - one
1 : 2 - two
2 : 3 - three
```

# Itertools

```python
import itertools
```

반복 가능한 데이터 원소들을 처리하는 함수와 제네레이터가 포함된 모듈이다.

## 1. cycle

```python
from itertools import cycle

for number, letter in zip(cycle(range(2)), ['a', 'b', 'c', 'd', 'e']):
  print(f'{number} : {letter}')

# Result
0 : a
1 : b
0 : c
1 : d
0 : e
```

반복 가능한 객체에서 요소를 반복적으로 생성한다. 

다만 연산의 대상이 되는 반복 가능한 객체가 끝날 때 생성을 중단한다. 

## 2. chain

```python
from itertools import chain

letters = ['a', 'b', 'c', 'd', 'e', 'f']
booleans = [1, 0, 1, 0, 0, 1]
decimals = [0.1, 0.7, 0.4, 0.4, 0.5]

print(list(chain(letters, booleans, decimals)))

# Result
['a', 'b', 'c', 'd', 'e', 'f', 1, 0, 1, 0, 0, 1, 0.1, 0.7, 0.4, 0.4, 0.5]
```

반복 가능한 객체를 연결 한다.

## 3. count

```python
from itertools import count

for number, letter in zip(count(0, 10), ['a', 'b', 'c', 'd', 'e']):
  print (f'{number} : {letter}')

# Result
0 : a
10 : b
20 : c
30 : d
40 : e
```

`count(시작 값 : 증가 값)`로 반복하고자 하는 최대 수를 알지 못해도 시작 값과 증가 값으로 연산의 대상이되는 반복 가능한 객체가 끝날때마다 수를 증가시킨다.
