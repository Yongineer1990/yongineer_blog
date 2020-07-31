---
title: TIL.25) Coding Test - 완주하지 못한 선수
date: 2020-07-31 18:07:34
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# Question

### **문제 설명**

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

# 나의 해결방식

```python
def solution(participant, completion):
    answer = ''
    for name in completion:
        if name in participant:
            participant.remove(name)
    answer = ''.join(participant)
    return answer
```

인자로 전달받은 `completion`의 요소를 하나씩 `participant` 리스트에 있는지 확인하고 있다면 `remove`를 통해 제거한 후 `answer`에 담아 반환했다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b86f243-91b0-4892-8f3e-2b9770200654/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5b86f243-91b0-4892-8f3e-2b9770200654/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200731%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200731T093957Z&X-Amz-Expires=86400&X-Amz-Signature=9676043da156fa19016a5145ba332f564a82e6d01f44f05bf6414fac0896c27c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

그러나 정확성 테스트에서는 통과했으나 매우 효율적이지 않다는 결과가 나왔다.

그래서 조금 방식을 바꿔 리스트로 들어온 두 인자를 정렬 후에 요소를 하나씩 비교 하다 같지 않다면 반환하는 방식으로 바꿨다.

```python
def solution(participant, completion):
    participant.sort()
    completion.sort()
    for i in range(len(completion)):
        if participant[i] != completion[i]:
            return participant[i]
    return participant[-1]
```

`participant`와 `completion`모두 정렬 후에 `completion`의 크기 만큼 for loop을 실행하여 `participant`와 `completion`을 비교하다 만약 같지 않는 결과가 나온다면 해당 요소를 반환하고 만약 `completion`의 모든 요소와 비교를 끝냈으나 아직 비교를 하지 않은 `participant` 요소가 있다면 해당 요소가 완주하지 못한 선수이므로 해당 요소를 반환했다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/861e113a-c671-4a23-a65c-94fc28f9081d/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/861e113a-c671-4a23-a65c-94fc28f9081d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200731%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200731T094104Z&X-Amz-Expires=86400&X-Amz-Signature=df66bcc24bad89a72b84ebc456818f9c9fbd55a7b5c677a4c36b4e84bee421d1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

# 놀라운 해답

정말 깜짝 놀라운 해답을 보았다.

```python
import collections

def solution(participant, completion):
    answer = collections.Counter(participant) - 
										collections.Counter(completion)
    return list(answer.keys())[0]
```

...

일단 코드 자체가 너무 깔끔하고 군더더기가 없었다. 아름다웠다 진심...

게다가 `collections`모듈을 처음봐서 당황스러웠다.

이 `collections.Counter()` 는 컨테이너에 동일한 값의 자료가 몇개인지 파악하는데 사용하는 객체이며 결과는 `dictionary`형태로 반환된다고 한다.

따라서 이 `collections`모듈을 이용하여 들어온 인자들을 모두 `dictionary`로 변환 해준다음 두 객체를 `-`연산해준다. 그렇게 하면 `participant`에서 `completion`에 없는 요소만 남게되는데 이 요소의 키값을 반환해준다.

결과는?

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ffd7ceae-7a30-4588-ae57-5ee1e3e61775/Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ffd7ceae-7a30-4588-ae57-5ee1e3e61775/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200731%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200731T094206Z&X-Amz-Expires=86400&X-Amz-Signature=88de268af4808b1838894dcbe8057e6cd71df99cea3ccca6115ad7a70b070716&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

모든 테스트에서 나의 해결방법보다 더 빠르게 결과를 반환하는 것을 볼 수 있다. 대단하다...

그럼 지금부터 `collections`모듈에 대해 좀 더 공부해보자

# Collections 모듈

## 1.  Counter()의 입력값

### List

```python
import collections

mylist = ['aa', 'cc', 'dd', 'aa', 'bb', 'ee']
print(collections.Counter(mylist))
```

**Result**

```python
Counter({'aa': 2, 'cc': 1, 'dd': 1, 'bb': 1, 'ee': 1})
```

`mylist = ['aa', 'cc', 'dd', 'aa', 'bb', 'ee']`의 요소 개수를 `collections.Counter()`를 이용하여 구할 수 있다. 출력의 결과는 `Dictionary`형태이다.

### Dictionary

```python
import collections

mydict = {'가': 3, '나': 2, '다': 4}
print(collections.Counter(mydict))
```

**Result**

```python
Counter({'다': 4, '가': 3, '나': 2})
```

또한 `Dictionary`를 인자로 전달 할 수 있으며 결과를 보면 알 수 있듯 요소의 개수가 많은 것 부터 출력 해준다.(`list`동일)

### Tuple

```python
import collections

my_tuple = ('aa', 'cc', 'dd', 'aa', 'bb', 'ee')
print(collections.Counter(my_tuple))
```

**Result**

```python
Counter({'aa': 2, 'cc': 1, 'dd': 1, 'bb': 1, 'ee': 1})
```

### 값 = 개수 형태

```python
import collections

c = collections.Counter(a=2, b=3, c=2)
print(collections.Counter(c))
print(sorted(c.elements()))
```

**Result**

```python
Counter({'b': 3, 'a': 2, 'c': 2})
['a', 'a', 'b', 'b', 'b', 'c', 'c']
```

`값=개수`형태로 전달이 가능하며 이때 `collections.Counter(a=2, b=3, c=2)`는 `['a', 'a', 'b', 'b', 'b', 'c', 'c']`과 같다.

### String

```python
import collections

my_string = 'aabcdeffgg'
print(collections.Counter(my_string))
```

**Result**

```python
Counter({'a': 2, 'f': 2, 'g': 2, 'b': 1, 'c': 1, 'd': 1, 'e': 1})
```

`String`으로 전달했을 경우 `{문자 : 개수}`의 `Dictionary`로 반환해준다.

## 2. Method of collections.Counter( )

### update( )

```python
import collections

# String
a = collections.Counter()
print(a)
a.update("abcdefg")
print(a)

# Dictionary
a.update({'f':3, 'e':2})
print(a)
```

**Result**

```python
# String
Counter()
Counter({'a': 1, 'b': 1, 'c': 1, 'd': 1, 'e': 1, 'f': 1, 'g': 1})

# Dictionary
Counter({'f': 4, 'e': 3, 'a': 1, 'b': 1, 'c': 1, 'd': 1, 'g': 1})
```

`update()`는 `Counter`의 값을 갱신하는 것을 의미한다. `Dictionary`의 `update`와 비슷하지만 입력값을 `String`으로 받을수 있다는 차이점이 있다. 또한 갱신하면서 요소의 크기에 따라 정렬하는 것도 확인 할 수 있다.

### elements( )

```python
import collections

# String
c1 = collections.Counter("Hello Python")
print(list(c1.elements()))
print(sorted(c1.elements()))

# List
c2 = collections.Counter(['aa', 'cc', 'dd', 'aa', 'bb', 'ee'])
print(list(c2.elements()))
print(sorted(c2.elements()))

# Dictionary
c3 = collections.Counter({'가': 3, '나': 2, '다': 4})
print(list(c3.elements()))
print(sorted(c3.elements()))

# 값=개수
c4 = collections.Counter(a=4, b=2, c=0, d=-2)
print(list(c4.elements()))
print(sorted(c4.elements()))
```

**Result**

```python
# String
['H', 'e', 'l', 'l', 'o', 'o', ' ', 'P', 'y', 't', 'h', 'n']
[' ', 'H', 'P', 'e', 'h', 'l', 'l', 'n', 'o', 'o', 't', 'y']

# List
['aa', 'aa', 'cc', 'dd', 'bb', 'ee']
['aa', 'aa', 'bb', 'cc', 'dd', 'ee']

# Dictionary
['가', '가', '가', '나', '나', '다', '다', '다', '다']
['가', '가', '가', '나', '나', '다', '다', '다', '다']

# 값=개수
['a', 'a', 'a', 'a', 'b', 'b']
['a', 'a', 'a', 'a', 'b', 'b']
```

전달받은 값을 풀어서 반환한다. 이때 요소는 **무작위로 반환되며 요소 수가 1보다 작을 경우 반환되지 않는다.**

정렬되지 않은 결과는 `sorted()`를 이용해 정렬할 수 있다.

### most_common( )

```python
import collections

mylist = ['aa', 'cc', 'dd', 'aa', 'bb', 'ee']
c = collections.Counter(mylist)
print(c.most_common())
print(c.most_common(3))
```

**Result**

```python
[('aa', 2), ('cc', 1), ('dd', 1), ('bb', 1), ('ee', 1)]
[('aa', 2), ('cc', 1), ('dd', 1)]
```

`most_common(n)`은 `Counter()`에 전달된 인자의 요소 중 **빈도수가 높은 순으로 상위 n개를 `list`안의 `tuple`형태로 반환**한다.

만약 `most_common`의 인자를 입력하지 않은 경우 오소 전체를 반환한다.

### subtract( )

```python
import collections

c = collections.Counter(a=4, b=2, c=0, d=-2)
d = collections.Counter(a=1, b=2, c=3, d=4)
c.subtract(d)
print(c)
```

**Result**

```python
Counter({'a': 3, 'b': 0, 'c': -3, 'd': -6})
```

`subtract()`는 **요소를 빼는 기능을 하며 만약 요소가 없는 경우 음수로 계산**된다.

## 3. Counter( ) 연산

### 덧셈

```python
import collections

c1 = collections.Counter([1, 2, 3])
c2 = collections.Counter([1, 2, 3, 4])
print(c1)
print(c2)
print(c1+c2)
```

**Result**

```python
Counter({1: 1, 2: 1, 3: 1})
Counter({1: 1, 2: 1, 3: 1, 4: 1})

# c1+c2
Counter({1: 2, 2: 2, 3: 2, 4: 1})
```

### 뺄셈

```python
import collections

c1 = collections.Counter({'a' : 1, 'b' : 2, 'c' : 5})
c2 = collections.Counter({'a' : 1, 'b' : 1, 'c' : 3})
print(c1)
print(c2)
print(c1-c2)
```

**Result**

```python
Counter({'c': 5, 'b': 2, 'a': 1})
Counter({'c': 3, 'a': 1, 'b': 1})

# c1-c2
Counter({'c': 2, 'b': 1})
```

`Counter()`의 뺄셈에서 결과가 음수값이라면 반환되지 않는다

### 교집합

```python
import collections

c1 = collections.Counter([2, 4, 6, 8, 10])
c2 = collections.Counter([4, 8, 12, 16, 20])
print(c1)
print(c2)
print(c1&c2)
```

**Result**

```python
Counter({2: 1, 4: 1, 6: 1, 8: 1, 10: 1})
Counter({4: 1, 8: 1, 12: 1, 16: 1, 20: 1})

# c1&c2
Counter({4: 1, 8: 1})
```

### 합집합

```python
import collections

c1 = collections.Counter('aabbccdd')
c2 = collections.Counter('aabbbce')
print(c1)
print(c2)
print(c1|c2)
```

**Result**

```python
Counter({'a': 2, 'b': 2, 'c': 2, 'd': 2})
Counter({'b': 3, 'a': 2, 'c': 1, 'e': 1})

# c1|c2
Counter({'b': 3, 'a': 2, 'c': 2, 'd': 2, 'e': 1})
```
