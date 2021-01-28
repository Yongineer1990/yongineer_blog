---
title: TIL.29) Coding Test - 두 수의 합
date: 2020-08-04 00:08:17
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

이 문제는 LeetCode에 소개된 문제이며 [여기](https://leetcode.com/problems/two-sum/)에서 문제를 풀어 볼 수 있습니다. 풀이 과정은 '파이썬 알고리즘 인터뷰'에 소개된 방법입니다.

# Question

덧셈하여 타겟을 만들 수 있는 배열의 두 숫자 인덱스를 리턴하라.

**Example:**

```python
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

# 나의 해결법

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        
        for i in range(len(nums)):
            num1 = nums[i]
            for j in range(i+1, len(nums)):
                num2 = nums[j]
                if num1 + num2 == target:
                    return [i, j]
        
        return []
```

![https://yongineer.duckdns.org/python/til29_1.png](https://yongineer.duckdns.org/python/til29_1.png)

배열의 첫번째 값과 (`nums[i]`), 그 이후 값들(`nums[j]`)을 대입하여 `target`에 맞으면 해당 값의 인덱스를 리턴했다.

여담으로 책에서 이 방법이 제일 효율성이 떨어진다 한다...

이와 같은 방법을 무차별 대입 방식 `Brute Force`라고 한다.

> ⚠️ Brute Force

- 시간 복잡도는 $O(n^2)$이다.

# 풀이

## 1. in을 이용한 탐색

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i, n in enumerate(nums):
            complement = target - n
            
            if complement in nums[i+1:]:
                return nums.index(n), nums[i+1:].index(complement) + (i+1)
```

![https://yongineer.duckdns.org/python/til29_2.png](https://yongineer.duckdns.org/python/til29_2.png)

모든 조합을 비교하지않고 정답. 즉, 타겟에서 첫번째 값을 뺀 값 (`target - n`)이 존재하는지 탐색하는 방법으로 해결했다. 여기서 `in`의 시간복잡도는 $O(n)$이고 따라서 전체 시간 복잡도는 나의 해결법과 동일한 $O(n^2)$이다. 그러나 `in`연산은 파이썬 내부 함수로 `Brute Force`에 비해 훨씬 빠르다.

## 2. 첫 번째 수를 뺀 결과 키 조회

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        nums_map = {}
				# 키와 값을 바꿔서 딕셔너리로 저장
        for i, num in enumerate(nums):
            nums_map[num] = i
        
				# 타겟에서 첫 번째 수를 뺀 결과를 키로 조회
        for i, num in enumerate(nums):
            if target - num in nums_map and i != nums_map[target - num]:
                return nums.index(num), nums_map[target - num]
```

![https://yongineer.duckdns.org/python/til29_3.png](https://yongineer.duckdns.org/python/til29_3.png)

위 풀이는 타겟에서 첫 번째 수를 빼면 두 번째 수를 바로 알아낼 수 있다. 그렇다면 두 번째 수를 키로 하고 기존의 인덱스는 값으로 바꿔서 딕셔너리로 저장해두면, 나중에 두 번째 수를 키로 조회해서 정답을 즉시 찾아낼 수 있다.

이제 타겟에서 첫 번째 수를 뺀 결과를 키로 조회해보면 두 번째 수의 인덱스를 즉시 조회할 수 있다. 딕셔너리의 조회는 평균적으로 $O(1)$이며 전체는 $O(n)$이다. 앞선 방식이 $O(n^2)$에 비해 훨씬 더 빠른 속도를 보여준다.

### Refactoring

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        nums_map = {}
        
        #하나의 for문으로 통합
        for i, num in enumerate(nums):
            if target - num in nums_map:
                return [nums_map[target - num], i]
            nums_map[num] = i
```

딕셔너리 저장과 조회를 2개의 `for loop`에서 각각 처리하던걸 하나의 `for`문으로 합칠수 있다.
