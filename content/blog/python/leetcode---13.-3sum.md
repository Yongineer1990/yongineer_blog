---
title: Leetcode - 13. 3Sum
date: 2021-08-09 00:08:50
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)


# 문제

배열을 입력받아 합으로 0을 만들 수 있는 3개의 엘리먼트를 출력하라.

### Example 1.

```python
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

### Example 2.

```python
Input: nums = []
Output: []
```

### Example 3.

```python
Input: nums = [0]
Output: []
```

# 풀이 : Two Pointer

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if not nums:
            return nums

        if len(nums) < 3:
            return []

        nums.sort()
        add = 0
        result = []
        for i in range(len(nums) - 2):
						# 중복된 값 건너띄귀
            if i > 0 and nums[i] == nums[i - 1]:
                continue

						# start와 end를 좁혀가며 합계 계산
            start = i + 1
            end = len(nums) - 1

            while start < end:
                add = nums[i] + nums[start] + nums[end]
                if add < 0:
                    start += 1
                elif add > 0:
                    end -= 1
                elif add == 0:
										# add = 0인 경우 요소 추가 및 스킵처리
                    result.append([nums[i], nums[start], nums[end]])

                    while start < end and nums[start] == nums[start + 1]:
                        start += 1
                    while start < end and nums[end] == nums[end - 1]:
                        end -= 1

                    start += 1
                    end -= 1

        return result
```

![https://yongineer.duckdns.org/python/leetcode_15_1.png](https://yongineer.duckdns.org/python/leetcode_15_1.png)

먼저 조건에서 빈 배열(`[]`)이 들어오거나 길이가 3보다 작을때는 즉시 `[]`을 리턴시켜 문제 해결 시간을 단축 시킨다.

```python
if not nums:
		return nums

if len(nums) < 3:
		return []
```

그런 다음 주어진 배열을 정렬하고 일치하는 값을 저장할 빈 배열(`result`)을 하나 선언한다. 또한 `i`를 축으로 중복된 값을 건너뛰게(`continue`) 한다.

```python
nums.sort()
add = 0
result = []
		for i in range(len(nums) - 2):
				if i > 0 and nums[i] == nums[i - 1]:
						continue
```

이제 `i`의 다음 지점(`start`)과 마지막 지점(`end`) 사이의 간격을 좁혀 가면서 `i`, `start`, `end`를 더한다.

이때 만약 합산의 결과 (`add`)가 0보다 작다면 **값을 더 키워야 하므로** `start`가 우측으로 이동(증가)하고 0보다 크다면 **값을 줄여야 하므로** `end`가 좌측으로 이동(감소)한다.

```python
while start < end:
		add = nums[i] + nums[start] + nums[end]
		if add < 0:
				start += 1
		elif add > 0:
				end -= 1
```

만약 `add`의 결과가 0과 같다면 각 요소를 앞서 생성한 배열, `result`에 추가한다. 그 다음 `start`와 `end`양 옆으로 동일한 값이 존재 할 수 있으므로 각각 우측과 좌측으로 이동시켜 스킵시킨다.

마지막으로 `start`를 한 칸 우측으로, `end`를 한 칸 좌측으로 이동 시키고 다음으로 넘긴다.

```python
while start < end:
...
elif add == 0:
		result.append([nums[i], nums[start], nums[end]])

		while start < end and nums[start] == nums[start + 1]:
		    start += 1
		while start < end and nums[end] == nums[end - 1]:
				end -= 1

		start += 1
		end -= 1

return result
```
