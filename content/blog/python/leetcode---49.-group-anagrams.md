---
title: Leetcode - 49. Group Anagrams
date: 2021-08-07 22:08:72
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

문자열 배열을 받아 애너그램 단위로 그룹화 하라

## input

```python
strs = ["eat","tea","tan","ate","nat","bat"]
```

## output

```python
[["bat"],["nat","tan"],["ate","eat","tea"]]
```

# 나의 풀이

```python
import collections

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        anagrams = collections.defaultdict(list)

        for word in strs:
            key = ''.join(sorted(word))
            anagrams[key].append(word)

        return anagrams.values()
```

인자로 주어진 `strs`에서 하나 꺼내 정렬한다. 정렬을 수행하면 `ate`, `eat`모두 모두 같은 문자로 이뤄져 있으므로 모두 `['a', 'e', 't']`로 정렬이 된다. 이 리스트를 키로 삼는 dictionary자료형인 `anagrams`를 하나 생성한다.

이때, 없는 키를 생성하려 할 경우 `KeyError`가 발생하므로`defaultdict()`로 선언해준다. 이제 정렬된 `key`를 dictionary의 `key`로 삼고 어를 하나씩 list에 삽입시켜주고 `anagrams`의 값만 리턴시켜준다.

![https://yongineer.duckdns.org/python/leetcode01.png](https://yongineer.duckdns.org/python/leetcode01.png)
