---
title: Leetcode - 5. Longest Palindrome Substring
date: 2021-08-08 15:08:78
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)


# 문제

가장 긴 팰린드롬 부분 문자열을 출력하라

### Example 1

```python
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

### Example 2

```python
Input: s = "cbbd"
Output: "bb"
```

# 나의 풀이

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        word = ''
        for i in range(len(s)):
            for j in range(len(s), i, -1):
                if len(word) >= j-i:
                    break
                elif s[i:j] == s[i:j][::-1]:
                    word = s[i:j]
                    break

        return word
```

가장 긴 팰린드롬을 담을 `word`변수를 하나 초기화 한다. 주어진 단어의 길이만큼 반복문을 수행한다. 그리고 반복문이 수행된 시점부터 주어진 단어의 길이의 역순 (`range(start, stop, step)`)으로 반복을 수행한다. 이때 `word`길이가 탐색 종료지점(`j`)과 시작지점(`i`)의 차이보다 크거나 같다면 `len(word) >= j=i` 탐색을 중단시키고 다음 시작지점으로 탐색을 이어나간다. 만약 해당 조건이 아니라면 아니라면 주어진 단어의 탐색 시작지점과 종료지점을 자른 단어와 이 단어를 뒤집었을때 일치한다면 `word`에 저장하고 다음 시작지점으로 탐색을 이어나간다.

![https://yongineer.duckdns.org/python/leetcode_5_1.png](https://yongineer.duckdns.org/python/leetcode_5_1.png)

# 풀이 : Expand Around Center

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:

        def expand(left: int, right: int) -> str:
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            return s[left+1:right]

        if len(s) < 2 or s == s[::-1]:
            return s

        result = ''
        for i in range(len(s) - 1):
            result = max(result, expand(i, i+1), expand(i, i+2), key=len)

        return result
```

![https://yongineer.duckdns.org/python/leetcode_5_2.png](https://yongineer.duckdns.org/python/leetcode_5_2.png)

일단 먼저 주어진 단어의 길이가 1이거나 주어진 단어를 뒤집었을때도 같아면 제일 길이가 긴 팰린드롬이므로 바로 해당 단어를 리턴하여 전체적인 풀이속도를 향상시키는 조건 처리를 하나 만든다.

```python
if len(s) < 2 or s == s[::-1]:
		return s
```

그 다음 중첩합수 `expand()`를 생성한다 이 함수는 홀수, 짝수 2개의 투 포인터가 펠린드롬여부를 판별하고 그 결과를 리턴한다.

```python
def expand(left: int, right: int) -> str:
		while left >= 0 and right < len(s) and s[left] == s[right]:
				left -= 1
				right += 1
		return s[left+1:right]
```

이제 이 `expand()`를 이용하여 주어진 단어를 계속 우측으로 이동시켜 홀수, 짝수의 단어중 길이를 기준으로 가장 긴 단어를 `result`에 담는다.

```python
result = ''
for i in range(len(s) - 1):
		result = max(result, expand(i, i+1), expand(i, i+2), key=len)
```
