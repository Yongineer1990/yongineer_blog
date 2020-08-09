---
title: TIL.35) Coding Test - 가장 흔한 단어
date: 2020-08-09 22:08:18
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

이 문제는 LeetCode에 소개된 문제이며 [여기](https://leetcode.com/problems/most-common-word/)에서 문제를 풀어 볼 수 있습니다.

# Question

금지된 단어를 제외한 가장 흔하게 등장하는 단어를 출력하라. 대소문자 구분을 하지 않으며, 구두점 또한 무시한다.

**Example:**

```python
Input: 
paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]
Output: "ball"
Explanation: 
"hit" occurs 3 times, but it is a banned word.
"ball" occurs twice (and no other word does), so it is the most frequent non-banned word in the paragraph. 
Note that words in the paragraph are not case sensitive,
that punctuation is ignored (even if adjacent to words, such as "ball,"), 
and that "hit" isn't the answer even though it occurs more because it is banned.
```

# 나의 해결법

```python
class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        paragraph = re.sub('[^A-Za-z\s]',' ',paragraph).lower()
        countword = collections.Counter(paragraph.split())
        
        for word in list(countword):
            if word in banned:
                del countword[word]
        
        return countword.most_common(1)[0][0]
```

![https://yongnas.iptime.org/python/til35_1.png](https://yongnas.iptime.org/python/til35_1.png)

먼저 입력값에 구두점을 정규식을 이용하여 제거했다. (`re.sub()`) 그 다음 모두 소문자로 바꿔 주었다. (`lower()`) 

그 다음 공백을 기준으로 나눈 리스트를 `collections`의 `Counter()`객체로 등장 횟수를 모두 구해 준 다음 금지 단어가 포함된 요소는 제거 해주었다.

전 처리가 완료된 `countword`에서 `most_common()`을 사용하여 가장 많이 등장한 단어만 가져와 해당 단어를 반환해주었다.

# 풀이

## List Comprehension, Counter 객체 사용

```python
class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        words = [word for word in re.sub(r'[^\w]', ' ', paragraph).lower().split()
                    if word not in banned]
        
        counts = collections.Counter(words)
        return counts.most_common(1)[0][0] 
```

`paragraph`의 전처리를 List Comprehension를 활용하여 나와는 다르게 한번에 끝냈다. 이때 정규식으로 `re.sub(r'[^\w]')`을 사용하였는데 정규식에서 `\w`는 단어 문자를 뜻한다.

그 다음 해당 단어가 금지단어인지 확인하고 아니라면 리스트에 추가했다.
