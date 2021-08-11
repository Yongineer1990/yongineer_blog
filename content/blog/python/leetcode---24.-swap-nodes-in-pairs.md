---
title: Leetcode - 24. Swap Nodes in Pairs
date: 2021-08-11 20:08:17
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

연결 리스트를 입력받아 페어 단위로 스왑하라.

### Example 1.

![https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```python
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

### Example 2.

```python
Input: head = []
Output: []
```

### Example 3.

```python
Input: head = [1]
Output: [1]
```

# 나의 풀이

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        rev = head

        while rev and rev.next:
            rev.val, rev.next.val = rev.next.val, rev.val
            rev = rev.next.next

        return head
```

![https://yongineer.duckdns.org/python/leetcode_24_1.png](https://yongineer.duckdns.org/python/leetcode_24_1.png)

변수 `rev`에 주어진 연결리스트 `head`를 할당하고 `while`문을 수행하면서 현재값 (`rev.val`)과 현재의 다음값 (`rev.next.val`)을 서로 바꿔주었다. 그리고 `rev`의 위치를 두 칸 이동시켰다.

이렇게 변경된 연결리스트 `head`를 반환시켰다.

# 풀이 : 반복 구조로 스왑

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        root = prev = ListNode(None)
        prev.next = head

        while head and head.next:
            # b가 a(head)를 가리키도록 할당
            b = head.next
            head.next = b.next
            b.next = head

            # prev가 b를 가리키도록 할당
            prev.next = b

            # 다음번 비교를 위해 이동
            head = head.next
            prev = prev.next.next

        return root.next
```

![https://yongineer.duckdns.org/python/leetcode_24_2.png](https://yongineer.duckdns.org/python/leetcode_24_2.png)

연결리스트의 값을 변경하는 일에 비해 연결리스트 자체를 바꾸는 일은 생각보다 복잡하다. 왜냐하면 `1->2->3->4->5->6`의 연결리스트에서 `3->4`를 `4->3`으로 바꾸는 일은 둘만 바꾼다고 끝나느 문제가 아니라 그 앞인 2가 가리키는 연결리스트와 5로 연결되는 연결리스트도 다 같이 변경해야 하기 때문이다.

`b`는 `head`를 가리키고 `head`는 `b`의 다음(`b.next`)를 가리키도록 변경한다.

```python
...
while head and head.next
		b = head.next
		head.next = b.next
		b.next = head
```

그러나 아직 `head`의 이전 노드가 `b`를 가리키지는 못한다. 따라서 `head`의 이전 노드 (`prev`)가 `b`를 가리키게 하고 다음번 비교를 위해 `prev`는 두 칸 앞으로 이동한다. 그리고 다시 다음번 교환을 진행할 것이다.

```python
...
while head and head.next:
		...
		prev.next = b

		head = head.next
		prev = prev.next.next
```

연결 리스트의 `head`를 가리키는 노드가 직접 바뀌는 풀이이므로 `head`를 리턴하지 못하고 그 이전 값을 `root`로 별도로 설정한 다음 `root.next`를 리턴한다.

# 풀이 : 재귀 구조로 스왑

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head and head.next:
            p = head.next

            # 스왑된 값을 리턴 받음
            head.next = self.swapPairs(p.next)
            p.next = head
            return p

        return head
```

![https://yongineer.duckdns.org/python/leetcode_24_2.png](https://yongineer.duckdns.org/python/leetcode_24_2.png)

반복 풀이와 달리 포인터 역할을 하는 `p`변수 하나만 있어도 충분하며, 더미 노드를 만들 필요도 없이 `head`를 바로 리턴할 수 있어 공간 복잡도가 낮다.

`p`는 `head.next`가 되고 `p.next`는 `head`가 된다.

```python
if head and head.next:
		p = head.next
		...
		p.next = head
```

그 사이 재귀 호출로 계속 스왑된 값을 리턴받게 된다.

```python
if head and head.next:
		...
		head.next = self.swapPairs(p.next)
		...
```

이후 백트래킹 되면서 연결 리스트가 생성되게 된다.
