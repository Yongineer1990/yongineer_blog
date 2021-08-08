---
title: Leetcode - 42. Trapping Rain Water
date: 2021-08-08 19:08:87
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

높이를 입력받아 비 온 후 얼마나 많은 물이 쌓일 수 있는지 계산하라.

### Example 1

![https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

```python
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

### Example 2

```python
Input: height = [4,2,0,3,2,5]
Output: 9
```

# 풀이 : Two Pointer

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        left = 0
        right = len(height) - 1
        rain = 0

        lm = height[left]
        rm = height[right]

        while left < right:
            lm = max(height[left], lm)
            rm = max(height[right], rm)

            if lm <= rm:
                rain += lm - height[left]
                left += 1
            else:
                rain += rm - height[right]
                right -= 1

        return rain
```

![https://yongineer.duckdns.org/python/leetcode_42_1.png](https://yongineer.duckdns.org/python/leetcode_42_1.png)

문제의 예시에서 보이는 가장 높은 막대는 막대의 높이와 무관하게 단순히 왼쪽과 오른쪽을 나누는 역할을 한다. 따라서 가장 높은 막대까지 각각 좌우 기둥의 최대 높이 (`lm`, `rm`)와 현재 높이의 차이 만큼 물 높이(`rain`)을 더해 나간다.

또한 비교시 낮은 쪽은 그만큼 항상 채워질 것이기 때문에, 좌우 어느 쪽이든 낮은 쪽은 높은 쪽을 향해서 포인터가 가운데로 점점 이동한다. 오른쪽이 크다면 왼쪽으로 이동하고 (`left += 1`) 그렇지 않다면 오른쪽이 이동한다. (`right -= 1`) 이렇게 하면 가장 높은 막대에서 좌우 포인터가 서로 만나게 된다.

```python
if lm <= rm:
		rain += lm - height[left]
		left += 1
else:
		rain += rm - height[right]
		right -= 1
```

# 풀이 : Stack

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        stack = []
        volume = 0

        for i in range(len(height)):
						# 변곡점을 만나는 경우
            while stack and height[i] > height[stack[-1]]:
                # 스택에서 꺼낸다
								top = stack.pop()

                if not len(stack):
                    break

								# 이전과의 차이만큼 물 높이 계산
                distance = i - stack[-1] - 1
                waters = min(height[i], height[stack[-1]]) - height[top]

                volume += distance * waters

            stack.append(i)

        return volume
```

![https://yongineer.duckdns.org/python/leetcode_42_2.png](https://yongineer.duckdns.org/python/leetcode_42_2.png)

현재 높이가 이전 높이보다 높을 때, 즉 변곡점을 만났을때 변곡점을 기준으로 격차만큼 물 높이(`volume`)를 채운다. 이전 높이는 고정된 형태가 아니라 가변적이기 때문에 계속 스택을 채워 나가다가 변곡점을 만날때마다 스택에서 하나씩 꺼내면서 이전과의 차이만큼 물 높이를 채워 나간다.
