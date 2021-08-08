---
title: Leetcode - 238. Product of Array Except Self
date: 2021-08-09 01:08:38
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)


# 문제

배열을 입력받아 `output[i]`가 자신을 제외한 나머지 모든 요소의 곱셈 결과가 되도록 출력하라.

단, 나눗셈을 하지 않고 $O(n)$에 풀이하라

### Example 1.

```python
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

### Example 2.

```python
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

# 나의 풀이 : 실패

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        left = []
        right = []

        val = 1
        for i in range(0, len(nums)):
            left.append(val)
            val = val * nums[i]

        val = 1
        for i in range(0, len(nums)):
            right.append(val)
            val = val * nums[::-1][i]

        result = [left[i] * right[::-1][i] for i in range(len(left))]

        return result
```

![https://yongineer.duckdns.org/python/leetcode_238_2.png](https://yongineer.duckdns.org/python/leetcode_238_2.png)

나눗셈을 하지 않아야 한다는 조건 덕분에(?) 미리 전체 곱셈을 구해 놓고 각 항목 별로 자기 자신을 나눠서 풀이하는 방법을 써먹지 못하게 되었으므로 자기 자신을 제외하고 왼쪽의 곱셈 결과와 오른쪽의 곱셈 결과를 곱해서 풀어야 할거 같다.

따라서 왼쪽의 곱셈 결과를 저장할 `left`와 오른쪽의 곱셈 결과를 저장할 `right`를 미리 빈 배열(`[]`)로 생성하였다. 이후 `left`에는 배열의 순서대로 곱셈의 결과를 저장하였고 `right`에는 배열의 순서를 뒤집어(`[::-1]`) 곱셈의 결과를 저장하였고

이 두 배열중 오른쪽의 곱셈결과를 뒤집어 `left`와 `right`를 곱하여 결과를 생성하였다. `Run Code`상으로는 통과가 되었지만... 결과적으로는 `Timeout`이 발생하였다.

![https://yongineer.duckdns.org/python/leetcode_238_1.png](https://yongineer.duckdns.org/python/leetcode_238_1.png)

# 풀이 : 공간 복잡도 $O(1)$로 풀기

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        out = []

        val = 1
        for i in range(0, len(nums)):
            out.append(val)
            val = val * nums[i]

        val = 1
        for i in range(len(nums) - 1, 0 - 1, -1):
            out[i] = out[i] * val
            val = val * nums[i]

        return out
```

![https://yongineer.duckdns.org/python/leetcode_238_3.png](https://yongineer.duckdns.org/python/leetcode_238_3.png)

나의 풀이와 거의 비슷하지만 한가지 다른 점은 좌, 우측 리스트 변수를 생성하지 않고 하나의 리스트 변수 `out`을 재활용함으로 써 공간 복잡도가 나의 코드에서는 $O(n)$이었지만 이 솔루션에서는 $O(1)$에 풀이했다는 점이다.
