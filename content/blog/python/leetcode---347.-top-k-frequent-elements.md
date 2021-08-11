---
title: Leetcode - 347. Top K Frequent Elements
date: 2021-08-12 02:08:55
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

상위 K번 이상 등장하는 요소를 추출하라.

### Example 1.

```python
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

### Example 2.

```python
Input: nums = [1], k = 1
Output: [1]
```

# 나의 풀이

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        commons = collections.Counter(nums).most_common(k)

        return [common[0] for common in commons]
```

![https://yongineer.duckdns.org/python/leetcode_347_1.png](https://yongineer.duckdns.org/python/leetcode_347_1.png)

`collections.Counter.most_common`을 활용하여 문제를 풀이했다. 이때 리턴되는 값은 빈도수가 높은 순으로 상위 n개를 `List`안의 `Tuple`형태로 리턴되고 `(요소, 빈도수)`의 형태로 반환된다 때문에 이중 요소만 따로 `List comprehension`진행하여 반환 하였다.

# 풀이 : Pythonic Way

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        return list(zip(*collections.Counter(nums).most_common(k)))[0]
```

![https://yongineer.duckdns.org/python/leetcode_347_2.png](https://yongineer.duckdns.org/python/leetcode_347_2.png)

마찬가지로 `collections.Counter.most_common`을 활용하였지만 추가적으로 파이썬 내장 함수인 `zip`과 `*`를 통해 나의 풀이에서 사용한 `List Comprehension`없이 깔끔하게 해답을 리턴했다.

## zip( ) 함수

- 2개 이상의 시퀀스를 짧은 길이를 기준으로 일대일 대응하는 튜플 시퀀스를 만드는 역할을 한다.

    ```python
    >>> a = [1,2,3,4,5]
    >>> b = [2,3,4,5]
    >>> c = [3,4,5]
    >>> zip(a, b)
    <zip object at 0x7fa22f346190>
    ```

- Python 2에서는 `zip()`의 결과로 바로 `List`가 리턴되었다.
- Python 3+ 에서는 `Generator`를 반환한다. 따라서 실제값을 추출하기 위해서는 `list()`로 한번 더 묶어 주면 된다.

    ```python
    >>> list(zip(a, b))
    [(1, 2), (2, 3), (3, 4), (4, 5)]
    >>> list(zip(a, b, c))
    [(1, 2, 3), (2, 3, 4), (3, 4, 5)]
    ```

- `zip()`의 결과 자체는 리스트 시퀀스가 아닌 튜플 시퀀스를 만들기 때문에 값을 변경하는게 불가능하다 즉, 불변(immutable) 객체이다. (`tuple`은 불변 객체이기 때문)

    ```python
    >>> d = list(zip(a, b, c))
    >>> d[0]
    (1, 2, 3)
    >>> d[0][0]
    1
    >>> d[0][0] = 0
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment
    ```

## Asterisk(`*`)

```python
>>> a = ['a1', 'a2']
>>> b = ['b1', 'b2']
>>> c = ['c1', 'c2']
>>> d = ['d1', 'd2']
>>> list(zip(a))
[('a1',), ('a2',)]
>>> list(zip(a, b))
[('a1', 'b1'), ('a2', 'b2')]
>>> list(zip(a, b, c))
[('a1', 'b1', 'c1'), ('a2', 'b2', 'c2')]
>>> list(zip(a, b, c, d))
[('a1', 'b1', 'c1', 'd1'), ('a2', 'b2', 'c2', 'd2')]
```

- 앞서 살펴본 `zip()`의 파라미터는 1개가 될수도 있고 10개가 될수도 있다 이것이 가능한 이유는 Asterisk(`*`)를 활용하기 때문이다.
- `C`에서는 이 `*`가 포인터 변수이지만 Python에서는 포인터가 존재 하지 않는다. 또한 생김새만 비슷할 뿐 전혀 다른 기능을 수행한다.
- Python에서 `*`는 언팩(Unpack)이다. 시퀀스 언패킹 연산자로 말 그대로 시퀀스를 풀어헤치는 연산자를 뜻하며 주로 `tuple`이나 `list`를 언패킹 하는데 사용한다.

    ```python
    >>> fruits = ['lemon', 'pear', 'watermelon', 'tomato']
    >>> fruits
    ['lemon', 'pear', 'watermelon', 'tomato']
    >>> print(fruits[0], fruits[1], fruits[2], fruits[3])
    lemon pear watermelon tomato
    >>> print(*fruits)
    lemon pear watermelon tomato
    ```

- 함수의 파라미터가 되었을때는 반대로 패킹이 가능하다.

    ```python
    >>> def f(*params):
    ...     print(params)
    ...
    >>> f('a', 'b', 'c')
    ('a', 'b', 'c')
    ```

- 변수 할당시 사용도 가능하다. 일반적인 변수는 값을 하나만 취하지만 `*`로 처리하면 나머지 모든 값을 취하게 된다.

    ```python
    >>> a, *b = [1,2,3,4]
    >>> a
    1
    >>> b
    [2, 3, 4]
    >>> *a, b = [1,2,3,4]
    >>> a
    [1, 2, 3]
    >>> b
    4
    ```

- `*`를 하나가 아닌 2개(`**`)를 사용하는 경우 key/value pair를 언패킹하는데 사용한다.

    ```python
    >>> date_info = {'year' : '2021', 'month' : '08', 'day' : '12'}
    >>> new_info = {**date_info, 'day' : '14'}
    >>> new_info
    {'year': '2021', 'month': '08', 'day': '14'}
    ```
