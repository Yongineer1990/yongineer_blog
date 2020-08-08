---
title: TIL.33) Coding Test - 문자열 뒤집기
date: 2020-08-09 03:08:95
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

이 문제는 LeetCode에 소개된 문제이며 [여기](https://leetcode.com/problems/reverse-string/)에서 문제를 풀어 볼 수 있습니다. 풀이 과정은 '파이썬 알고리즘 인터뷰'에 소개된 방법입니다.

# Question

문자열을 뒤집는 함수를 작성하라, 입력값은 문자 배열이며, 리턴 없이 리스트 내부를 직접 조작하라.

**Example 1:**

```python
Input: ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

**Example 2:**

```python
Input: ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

# 나의 해결법

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s.reverse()
        print(s)
```

리스트를 뒤집는 함수인 `reverse()`를 이용하여 풀었다.

# 풀이

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anythin, modify s in-place instead.
        """
        s = s[::-1]
        print(s)
```

나의 해결법도 책에서 소개한 좋은 방식이지만 이 문제는 문자열 슬라이싱을 이용하여 풀 수도 있다. 다만, LeetCode에서는 오류를 발생시킨다... (사실 나도 원래는 이 방식으로 풀려고 했었다.)

만약 온라인 코딩테스트를 치뤄야 한다면 테스트 전 코딩 플랫폼에대해 충분히 숙지가 필요할 것 같다.
