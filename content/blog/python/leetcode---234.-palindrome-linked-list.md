---
title: Leetcode - 234. Palindrome Linked List
date: 2021-08-09 20:08:01
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)


# 문제

연결 리스트가 팰린드롬 구조인지 판별하라

### Example 1.

![https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```python
Input: head = [1,2,2,1]
Output: true
```

### Example 2.

![https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```python
Input: head = [1,2]
Output: false
```

# 나의 풀이

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        to_list = []
        while True:
            to_list.append(head.val)
            if head.next:
                head = head.next
            else:
                break

        if to_list == to_list[::-1]:
            return True

        return False
```

![https://yongineer.duckdns.org/python/leetcode_234_1.png](https://yongineer.duckdns.org/python/leetcode_234_1.png)

`ListNode`의 연결된 모든 요소를 `List`로 변환하였다. 이때 최소 한번의 실행은 보장해야 하기 때문에 `while`의 조건은 `True`로 주고 만약 연결된 리스트가 없을때 (`head.next is None`) `while`문을 탈출 하도록 로직을 설계하였다.

```python
to_list = []
while True:
		to_list.append(head.val)
		if head.next:
				head = head.next
		else:
				break
```

이제 이렇게 생성된 리스트를 뒤집었을때도 동일하다면 팰린드롬이므로 `True`를 리턴하고 아니라면 `False`를 리턴하도록 로직을 설계하였다.

# 풀이 : Deque

```python
import collections

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        q: Deque = collections.deque()

        if not head:
            return True

        node = head
        while node is not None:
            q.append(node.val)
            node = node.next

        while len(q) > 1:
            if q.popleft() != q.pop():
                return False

        return True
```

![https://yongineer.duckdns.org/python/leetcode_234_2.png](https://yongineer.duckdns.org/python/leetcode_234_2.png)

만약 `List`로 변환 후 `list.pop(0)`을 통해 첫 번째 아이템을 가져오려 했다면 이때 병목이 발생한다. 동적 배열로 구성된 리스트는 맨 앞 아이템을 가져오기에 적합한 자료형이 아니기 때문이다.

왜냐하면 맨 앞의 값을 가져오려면 모든 요소들이 한 칸 씩 시프팅되어야 하며 이는 시간 복잡도 $O(n)$이 발생하기 때문이다.

반면 `Deque`는 이중 연결 리스트 구조로 양쪽 모두 추출하는데 시간 복잡도 $O(1)$에 실행된다.

# 풀이 : Runner

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        rev = None
        slow = fast = head

        # 런너를 이용해 역순 연결 리스트 구성
        while fast and fast.next:
            fast = fast.next.next
            rev, rev.next, slow = slow, rev, slow.next

        if fast:
            slow = slow.next

        # 팰린드롬 여부 확인
        while rev and rev.val == slow.val:
            slow, rev = slow.next, rev.next

        return not rev
```

![https://yongineer.duckdns.org/python/leetcode_234_3.png](https://yongineer.duckdns.org/python/leetcode_234_3.png)

`Linked list`를 `List`나 `Deque`로 변환하지 않고 푸는 방법 중 하나이며 `Linked list`문제를 해결할때 유용하게 사용하는 방법인 `Runner`방법을 이용한 풀이이다.

빠른런너(`fast`)와 느린런너(`slow`)를 각각 출발시키면 빠른 런너가 끝에 도달할때 느린 러너가 정확히 중간 지점에 도달하게 된다.

느린 런너는 중간까지 이동하면서 연순으로 연결 리스트 `rev`를 만들어 나간다. 중간에 도달한 느린 런너가 나머지 경로를 이동할 때, 역순으로 만든 연결 리스트의 값들과 일치하는지 확인하면 된다.

빠른 런너와 느린 런너의 초기값은 모두 `head`에서 시작한다.

```python
slow = fast = head
```

다음은 런너를 이동시킨다. 이때 `next`가 존재하지 않을때까지 `fast`는 `slow`의 두배씩 이동 시킨다.

```python
while fast and fast.next:
		fast = fast.next.next
		...
		slow = slow.next
```

그러면서 역순으로 연결 리스트 `rev`를 생성하는 로직을 `slow`앞에 덧 붙인다. 첫 `rev`의 값은 `None`이 되고 `slow`가 이동하면서 `1->None`, `2->1->None`으로 점점 이전 값으로 연결되는 구조가 된다.

```python
while fast and fast.next:
		fast = fast.next.next
		rev, rev.next, slow = slow, rev, slow.next
```

다중 할당을 보면 역순 연결 리스트(`rev`)는 현재 값을 `slow`로 교체하고 `rev.next`는 `rev`가 된다. 즉, 앞에 계속 새로운 노드가 추가되는 형태이다. 결국 이 리스트는 `slow`의 역순 연결 리스트가 될 것이다.

입력값이 홀수 일때와 짝수일 때 마지막 처리가 조금 다른데, 홀수일때는 `slow`가 한 칸 더 앞으로 이동하여 중앙의 값을 빗겨 나가야 한다. 왜냐하면 여기서 `3`은 중앙에 위치한 값으로 팰린드롬 체크에서 배제되어야 하기 때문이다.

이는 `fast`가 아직 `None`이 아니라는 경우로 판단할 수 있다. 따라서 `fast`가 `None`이 아닐때 `slow`를 한칸 더 이동해준다.

```python
if fast:
		slow = slow.next
```

이제 팰린드롬 여부를 확인한다. `slow`런너가 생성한 `slow`와 `slow`의 역순 연결 리스트 `rev`를 서로 하나씩 풀어가면서 비교한다.

```python
while rev and rev.val == slow.val:
		slow, rev = slow.next, rev.next
```

정상적으로 비교가 종료되었다면 `slow`또는 `rev`는 모두 끝까지 이동해 `None`이 되었을것이다. 따라서 `return not rev` 또는 `return not slow`를 반환해준다. (`None` : `True` / `not None` : `False`)

## Runner 기법

![https://yongineer.duckdns.org/python/leetcode_234_4.png](https://yongineer.duckdns.org/python/leetcode_234_4.png)

- `Linked list`를 순회 할때 2개의 포인터를 동시에 사용하는 기법
- 한 포인터가 다른 포인터보다 앞서게 하여 병합 지점, 중간위치, 길이 등을 판별할때 유용하게 사용할 수 있다.
- 보통 빠른런너가 2칸씩, 느린런너가 1칸씩 이동하게 된다. 이때 빠른 런너가 연결 리스트 끝에 도달하면 느린런너는 정확히 중간 지점에 위치하게 도니다.
- 이 같은 방식으로 중간 위치를 찾아내면 거기서 부터 값을 비교하거나 뒤집기를 시도하는 등 여러가지 활용이 가능하여 연결 리스트 문제에서 자주 쓰이는 기법이다.

## 다중할당

2개 이상의 변수에 동시에 할당하는 것을 말한다.

```python
a, b = 1, 2
```

위 풀이에서는 아래와 같이 3개의 변수에 3개의 값을 할당한 부분이 있다.

```python
rev, rev.next, slow = slow, rev, slow.next
```

코드의 가독성을 위해 세줄로 정리할 수는 있지만 이렇게 될 경우 문제가 풀리지 않는다.

```python
fast = fast.next.next
rev, rev.next = slow, rev
slow = slow.next
```

이렇게 세 줄로 늘어트릴 경우 `rev, rev.next = slow, rev`라는 구문에서는 `slow`와 `rev`가 동일한 참조가 된다. 구문 중간의 `rev = slow`가 있으니 서로 같은 값을 참조 하게 된다. 왜 `=`구문이 값을 할당하는 것이 아닌 서로 같은 값을 참조하게 되는지는 Python의 특징에 기인한다.

Python에서는 원시 타입이 존재하지 않는다. 대신 모든 것이 객체다. 여러가지 자료형은 물론 숫자와 문자 또한 모두 마찬가지다. 문자와 숫자의 경우 불변 객체라는 점만 다를 뿐 `=`연산자를 이용해 할당을 하면 값을 할당 하는 것이 아니라 이 불변 객체에 대한 참조를 할당하게 된다.

```python
>>> id(5)
4439139584
>>> a = 5
>>> id(a)
4439139584
>>> b = 5
>>> id(b)
4439139584
```

5라는 숫자에 대해 숫자 5와 변수 `a`, `b`모두 동일한 ID를 갖는다 즉 5라는 값은 메모리 상 단 하나만 존재하며, `a`, `b`두 변수는 각각 이 값을 가리키는 참조라는 의미다. 만약 5가 6으로 변경된다면 `a`, `b` 두 변수도 값이 6으로 변경될 것이라고 생각할 수 있지만 그런 일을 일어나지 않는다. 앞서 설명한 것과 같이 숫자는 불변 객체이기 때문이다.

반면, `List`와 같은 자료형이라면 내부의 값은 얼마든지 변할 수 있다. 이 경우 이 리스트를 참조하는 모든 변수의 값도 따라서 함께 바뀌게 된다.

만약 `rev = 1`, `slow = 2->3`이라고 가정해보자 여기서 `slow`는 연결리스트이며, `slow.next`는 3이라는 의미이다.

```python
rev, rev.next, slow = slow, rev, slow.next
```

이 경우 `rev = 2->3`, `rev.next = 1`, `slow = 3`이 되고 `rev.next = 1`이므로 최종적으로 `rev = 2->1`, `slow = 3`이 된다. 다중할당은 작업이 동시에 일어나기 때문에 이 모든 작업은 중간 과정 없이 한번의 트랜잭션으로 끝나게된다.

그러나 코드를 분기처리 하는 경우를 생각해보자.

```python
rev, rev.next = slow, rev
slow = slow.next
```
첫 줄을 실행한 결과, `rev = 2->3`, `rev.next = 1`따라서 `rev = 2->1`이 되는데 여기서 중요한 점은 `rev = slow`라는 점이다.

즉 동일한 참조가 되었으며 `rev = 2->1`이 되었기 때문에 `slow = 2->1`도 함께 되어 버린다. 따라서 이후에 `slow = slow.next`의 결과는 `slow = 1`이 된다. 결국 최종 결과는 `rev = 2->1`, `slow = 1`로 앞서 다중 할당으로 처리한 것과 다른 결과가 된다.
