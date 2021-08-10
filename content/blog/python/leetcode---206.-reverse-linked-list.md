---
title: Leetcode - 206. Reverse Linked List
date: 2021-08-11 01:08:15
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

연결 리스트를 뒤집어라

### Example 1.

![https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```python
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

### Example 2.

![https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```python
Input: head = [1,2]
Output: [2,1]
```

### Example 3.

```python
Input: head = []
Output: []
```

# 나의 풀이

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        to_list = []
        while head:
            to_list.append(head.val)
            head = head.next

        to_list.reverse()

        rev = ListNode()
        res = rev

        for i in to_list:
            res.next = ListNode(i, None)
            res = res.next

        return rev.next
```

![https://yongineer.duckdns.org/python/leetcode_206_1.png](https://yongineer.duckdns.org/python/leetcode_206_1.png)

주어진 연결 리스트의 값들을 배열에 담은 다음 해당 배열을 뒤집었다. 이후 `rev`에 `ListNode()`인스턴스를 생성하고 이를 `res`가 참조하게 했다. 이후 뒤집은 리스트에서 값을 하나씩 꺼내와 새로운 연결 리스트를 생성하고 이를 반환하였다.

# 풀이 : 재귀 구조로 뒤집기

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        def reverse(node: ListNode, prev: ListNode = None):
            if not node:
                return prev
            next, node.next = node.next, prev
            return reverse(next, node)

        return reverse(head)
```

![https://yongineer.duckdns.org/python/leetcode_206_2.png](https://yongineer.duckdns.org/python/leetcode_206_2.png)

다음 노드 `next`와 현재 노드 `node`를 파라미터로 `reverse`함수를 계속해서 재귀 호출한다. `node.next`에는 이전 `prev`리스트를 계속 연결해주면서 `node`가 `None`이 욀때까지 재귀 호출하면 마지막에는 백트래킹 되면서 연결 리스트가 거꾸로 연결된다.

여기서 맨 처음에 리턴된 `prev`는 뒤집힌 연결 리스트의 첫번째 노드가 된다.

> **💡 백트래킹 (Back Traking)**
>
> - 탐색을 하다가 더 갈 수 없으면 왔던 길을 되돌아가 다른 길을 찾는다는 데서 유래했다.
> - DFS와 같은 방식으로 탐색하는 모든 방법을 뜻한다.
> - 주로 재귀로 구현된다.
> - 가보고 되돌아오고를 반복한다. 운이 좋으면 시행착오를 덜 거치고 목적지에 도착할 수 있지만 최악의 경우 모든 경우를 다 거친 다음에 도착할 수 있다. 이때문에 브루트 포스와 유사하다.
> - 하지만 한번 방문 후 가능성이 없는 경우에는 즉시 후보를 포기한다는 점에서 매번 같은 경로를 방문하는 브루트 포스보다 훨씬 유리하다.

# 풀이 : 반복 구조로 뒤집기

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node, prev = head, None

        while node:
            next, node.next = node.next, prev
            prev, node = node, next

        return prev
```

![https://yongineer.duckdns.org/python/leetcode_206_3.png](https://yongineer.duckdns.org/python/leetcode_206_3.png)

`node.next`를 이전 `prev`리스트로 계속 연결하면서 끝날 때까지 반복한다. `node`가 `None`이 될때 `prev`는 뒤집힌 연결 리스트의 첫 번째 노드가 된다.

`prev`에 `node`를, `node`에 `next`를 별도로 세팅하며 이를이용해 `node`가 `None`이 될 때까지 계속 `while`문을 돌게 된다.
