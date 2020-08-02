---
title: TIL.28) Coding Test - 유효한 팰린드롬
date: 2020-08-03 02:08:20
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

이 문제는 LeetCode에 소개된 문제이며 [여기](https://leetcode.com/problems/valid-palindrome)에서 문제를 풀어 볼 수 있습니다. 풀이 과정은 '파이썬 알고리즘 인터뷰'에 소개된 방법입니다.

# Question

주어진 문자열이 팰린드롬인지 확인하라. 대소문자를 구분하지 않으며, 영문자와 숫자만을 대상으로 한다.

**Example 1:**

```python
Input: "A man, a plan, a canal: Panama"
Output: true
```

**Example 2:**

```python
Input: "race a car"
Output: false
```

> ⚠️ Palindrome?
>
앞뒤가 똑같은 단어나 문장으로, 뒤집어도 같은 말이 되는 단어나 문장

# 나의 해결법

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        texts = list(text.lower() for text in s if text.isalnum())
        
        for i in texts:
            if i != texts[-1]:
                return False
            texts.pop()
        
        return True
```

`texts`는 입력값인 `s`가 문자 또는 숫자인지 판별하고 (`isalnum()`) 대소문자를 구분하지 않으므로 소문자로 변환해(`lower()`) `generator expression`으로 생성한 `list`이다.

이를 `for loop`을 돌면서 첫 문자인 `i`와 마지막 문자인 `texts[-1]`와 비교하면서 만약 다르다면 `False`를 반환하고 같다면 마지막 문자를 삭제시킨다. (`pop()`)

만약 `for loop`이 모두 수행했는데 통과 되었다면 이는 팰린드롬이므로 `True`를 반환한다.

## 결과

![https://yongnas.iptime.org/python/1.png](https://yongnas.iptime.org/python/1.png)

# 풀이

## 1. 데크 자료형을 이용한 최적화

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
				# 자료형을 데크로 선언
        strs: Deque = collections.deque()
        
        for char in s:
            if char.isalnum():
                strs.append(char.lower())
        
        while len(strs) > 1:
            if strs.popleft() != strs.pop():
                return False
        
        return True
```

![https://yongnas.iptime.org/python/2.png](https://yongnas.iptime.org/python/2.png)

Runtime은 내 해결방식과 비슷한거 같은데 전처리 한 문자를 담은 `strs`변수의 자료형을 `Deque`로 선언했다.

### Deque

`Double Ended Queue`의 줄임말로 양쪽 끝을 모두 추출할 수 있는, 큐를 일반화한 형태의 추상 자료형(ADT)이다.

![https://yongnas.iptime.org/python/deque.jpeg](https://yongnas.iptime.org/python/deque.jpeg)

`stack`과 `queue`의 특징을 모두 갖고 있다. 이 추상 자료형(ADT)의 구현은 배열이나 연결 리스트 모두 가능하지만, 특히 이중 연결 리스트 (Doubly Linked List)로 구현하는게 가장 좋다.

![https://yongnas.iptime.org/python/doubly_linked_list.png](https://yongnas.iptime.org/python/doubly_linked_list.png)

이중 연결 리스트로 구현하면 양쪽으로 `head`와 `tail`이라는 이름의 두 포인터를 갖고 잇다가 새로운 아이템이 추가될 때마다 앞쪽 또는 뒤쪽으로 연결시켜 주기만 하면 된다. 당연히 연결 후에는 포인터를 이동하면 된다.

```python
import collections

d = collections.deque()
print(type(d))

# Result
<class 'collections.deque'>
```

Python에서는 데크 자료형을 `collections`모듈에서 `deque`라는 이름으로 지원한다.

# 2. 슬라이싱 사용

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:        
        s = s.lower()
        s = re.sub('[^a-z0-9]', '', s)
        
        return s == s[::-1]
```

![https://yongnas.iptime.org/python/3.png](https://yongnas.iptime.org/python/3.png)

일단 코드가 굉장히 간결하다.

파이썬의 문자열을 조작하는 `Slicing`을 사용했다. 앞서 나의 해결법에서는 `isalnum()`을 이용하여 모든 문자의 영숫자를 판단했는데 이 풀이에서는 정규식을 활용하여 처리 했다. (`s = re.sub('[^a-z0-9]', '', s)`)

또한 파이썬의 문자열 처리 기능인 `[::-1]`을 이용하여 문자열을 뒤집어서 처리했다.

## 문자열 슬라이싱

파이썬에서는 문자열 슬라이싱 기능을 제공하며 내부적으로 매우 빠르게 동작한다. **문자열을 조작할 때는 항상 슬라이싱을 우선으로 사용하는 편이 속도 개선에 유리**하다. 문자열을 별도로 리스트로 매핑하는 등의 처리는 데이터 구조를 다루는 입장에서 좋은 방법이지만, **별도 자료형으로 매핑하는 과정에서 상당한 연산 비용이 필요하므로 전체적인 속도에서는 오히려 손해를 볼 수 있다.**

![httpss://yongnas.iptime.org/python/4.png](https://yongnas.iptime.org/python/4.png)

### 문자열 슬라이싱 해보기

![https://yongnas.iptime.org/python/5.png](https://yongnas.iptime.org/python/5.png)

```python
s = '안녕하세요'

print(s[1:4])
print(s[1:-2])
print(s[1:])
print(s[:])
print(s[1:100])
print(s[-1])
print(s[-4])
print(s[:-3])
print(s[-3:])
print(s[::1])
print(s[::2])

# Result
녕하세
녕하
녕하세요
안녕하세요
녕하세요
요
녕
안녕
하세요
안녕하세요
안하요
```

- `s[1:4]` : 인덱스 1에서 4이전 까지 자른다.
- `s[1:-2]`: 인덱스 1에서 -2이전까지 자른다. 음수는 뒤에서부터 접근한다.
- `s[1:]` : 문자열의 시작 또는 끝은 생략 가능하다.
- `s[:]` : 시작과 끝 모두 생략하면 사본을 리턴한다. 이 방식은 문자열이나 리스트를 복사하는 파이썬 다운 방식이다.
- `s[1:100]` : 인덱스가 클 경우 문자열의 최대 길이만큼 자른다, `s[1:]`과 같아진다.
- `s[-1]` : 마지막 문자
- `s[-4]` : 뒤에서 4번째 문자
- `s[:-3]` : 뒤에서 3번째 문자 앞까지
- `s[-3:]` : 뒤에서 3번째 문자에서 마지막까지
- `s[::1]` : 1은 기본값으로 동일하다.
- `s[::-1]` : 뒤집는다.
- `s[::2]` : 2칸씩 앞으로 이동한다.
