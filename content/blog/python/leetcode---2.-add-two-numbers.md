---
title: Leetcode - 2. Add Two Numbers
date: 2021-08-11 02:08:77
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

역순으로 저장된 연결 리스트의 숫자를 더하라

### Example 1.

![https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

```python
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

### Example 2.

```python
Input: l1 = [0], l2 = [0]
Output: [0]
```

### Example 3.

```python
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

# 나의 풀이

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        num1 = ''
        num2 = ''

        while l1:
            num1 += ''.join(str(l1.val))
            l1 = l1.next

        while l2:
            num2 += ''.join(str(l2.val))
            l2 = l2.next

        res = str(int(num1[::-1]) + int(num2[::-1]))
        rev = None

        for i in res:
            rev = ListNode(int(i), rev)

        return rev
```

![https://yongineer.duckdns.org/python/leetcode_2_1.png](https://yongineer.duckdns.org/python/leetcode_2_1.png)

주어진 연결 리스트 `l1`과 `l2`의 값들을 모두 `string`으로 변환하여 `num1`, `num2`변수에 이어 붙였다. 이렇게 이어 붙인 `num1`과 `num2`를 뒤집어서 `integer`로 변환 후 합산하여 다시 `string`으로 변환하였다. (`res`)

이 결과를 하나씩 꺼내어 새로운 `ListNode`(`rev`)에 `integer`로 변환하여 연결리스트를 생성하고 이를 반환시켰다.

# 풀이 : Full Adder

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        root = head = ListNode(0)

        carry = 0
        while l1 or l2 or carry:
            sum = 0

            # 두 입력값의 합 계산
            if l1:
                sum += l1.val
                l1 = l1.next
            if l2:
                sum += l2.val
                l2 = l2.next

            # 몫(자리올림수)과 나머지(값) 계산
            carry, val = divmod(sum + carry, 10)
            head.next = ListNode(val)
            head = head.next

        return root.next
```

![https://yongineer.duckdns.org/python/leetcode_2_2.png](https://yongineer.duckdns.org/python/leetcode_2_2.png)

먼저, 두 입력값의 합을 구한다.

```python
while l1 or l2 or carry:
		sum = 0

		# 두 입력값의 합 계산
		if l1:
				sum += l1.val
				l1 = l1.next
		if l2:
				sum += l2.val
				l2 = l2.next
```

만약 자릿수가 넘어갈 경우에는 자리 올림수를 설정한다.

```python
carry, val = divmod(sum + carry, 10)
```

가산 결과가 두 자릿수가 될 경우 몫은 자리올림수로 설정해 다음번 연산에 사용하게 되고 나머지는 값으로 취한다. 이제 이 값을 연결리스트로 만들어준다.

```python
head.next = ListNode(val)
head = head.next
```

> **💡 `divmod()`**
>
> - Python 내장 함수
> - 몫과 나머지로 구성된 `tuple`을 반환한다.
> - 즉, (a // b, a % b) 와 동일한 결과를 출력한다.
>
> ```python
> >>> divmod(10, 3)
> (3, 1)
> >>> (10 // 3, 10 % 3)
> (3, 1)
> ```

## 전가산기

![https://yongineer.duckdns.org/python/leetcode_2_3.gif](https://yongineer.duckdns.org/python/leetcode_2_3.gif)

- 입력값 A와 B, 이전의 자리올림수 (Carry in)이렇게 3가지 입력으로 합과 다음 자리 올림수 (Carry out)여부를 결정한다.
- 입력값 A, B는 오른쪽으로는 `XOR`게이트를 통과한 결과가 나아가고
- 아래로는 `AND`게이트를 통과한 결과가 나아간다.
- 그렇게 합과 다음 자리올림수 위치에 도달한 결과가 그림 좌측의 진리표이다.
- 실제로 덧셈 연산을 수행하는 논리 회로의 원리이며
- 산술 논리 장치(Arithmetic Logic Unit)를 구성하는 디지털 회로의 일부분이다.
