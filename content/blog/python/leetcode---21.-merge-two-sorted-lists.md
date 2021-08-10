---
title: Leetcode - 21. Merge Two Sorted Lists
date: 2021-08-10 23:08:85
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

정렬되어 있는 두 연결 리스트를 병합하고 정렬하여 반환하라

### Example 1.

![https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```python
Input: l1 = [1,2,4], l2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

### Example 2.

```python
Input: l1 = [], l2 = []
Output: []
```

### Example 3.

```python
Input: l1 = [], l2 = [0]
Output: [0]
```

```python
ListNode{val: 0, next: None}
ListNode{val: 0, next: None}
```

# 나의 풀이

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        rev = ListNode()
        res = rev

        while l1 and l2:
            if l1.val < l2.val:
                res.next = l1
                l1 = l1.next
            else:
                res.next = l2
                l2 = l2.next

            res = res.next

        res.next = l1 if l1 else l2

        return rev.next
```

![https://yongineer.duckdns.org/python/leetcode_21_1.png](https://yongineer.duckdns.org/python/leetcode_21_1.png)

`rev`에 `ListNode`클래스 인스턴스를 생성해주고 이를 `res`가 참조한다. `l1`과 `l2`가 `None`이 될때가지 반복을 돌려주고 `l1`과 `l2`를 비교하여 `l1`이 작다면 `res`의 다음 노드(`res.next`)에 `l1`을 할당하고 `l1`을 다음 노드로 이동시켜 준다.

만약 `l2`가 작다면 `rex`의 다음 노드에 `l2`를 할당하고 `l2`를 다음 노드로 이동시켜준다. 비교 과정이 끝나면 `res`를 다음 노드로 이동 시켜 주고 이를 반복한다.

`l1`과 `l2`모두 끝 지점까지 도달하였으면 반복을 종료하고 `res`의 다음 노드에 `l1`또는 `l2`의 나머지를 연결 하고 `res`가 참조하고 있는 `rev`의 `next`를 리턴해준다.

# 풀이 : 재귀 구조로 연결

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        if (not l1) or (l2 and l1.val > l2.val):
            l1, l2 = l2, l1

        if l1:
            l1.next = self.mergeTwoLists(l1.next, l2)

        return l1
```

먼저 `l1`의 `None`이 아니거나 `l2`가 `None`이 아니면서 `l1`의 값이 `l2`의 값보다 작을때 `l1`과 `l2` 변수를 스왑한다. 이후에 `l1`이 `None`이 아니라면서 `l1`의 `next`를 재귀호출하면서 다음번 연결 리스트가 계속 스왑될 수 있게 한다.

이렇게 스왑하면서 그 다음 값이 엮이도록 계속 재귀호출 하면 연결 리스트가 점점 하나로 병합된다. 마지막에는 `l1`이 `None`이 되면서 재귀가 끝나고 리턴을 시작한다.

## 연산자 우선순위

<div class="page-body"><table class="collection-content"><thead><tr><th>우선순위</th><th>연산자</th><th>설명</th></tr></thead><tbody><tr id="a8d4e810-c987-4c1e-8f6b-11641f961eb3"><td class="cell-title"><a href="https://www.notion.so/1-a8d4e810c9874c1e8f6b11641f961eb3">1</a></td><td class="cell-unYw"><code>()</code></td><td class="cell-uSR~">괄호</td></tr><tr id="93a98d43-1334-4480-a8ac-4abadf03fe92"><td class="cell-title"><a href="https://www.notion.so/2-93a98d4313344480a8ac4abadf03fe92">2</a></td><td class="cell-unYw"><code>f(args...)</code></td><td class="cell-uSR~">함수 호출</td></tr><tr id="d2244f37-526c-4e17-a1f3-d84018188ccc"><td class="cell-title"><a href="https://www.notion.so/3-d2244f37526c4e17a1f3d84018188ccc">3</a></td><td class="cell-unYw"><code>x[index:index]</code></td><td class="cell-uSR~">슬라이싱</td></tr><tr id="dd21f8f4-5b7b-4928-b3a6-b90730b519f8"><td class="cell-title"><a href="https://www.notion.so/4-dd21f8f45b7b4928b3a6b90730b519f8">4</a></td><td class="cell-unYw"><code>x[index]</code></td><td class="cell-uSR~">배열</td></tr><tr id="8f713735-58b9-4070-aed9-56503626e5d2"><td class="cell-title"><a href="https://www.notion.so/5-8f71373558b94070aed956503626e5d2">5</a></td><td class="cell-unYw"><code>x.attribute</code></td><td class="cell-uSR~">속성 참조</td></tr><tr id="aad3346e-521d-4f44-8974-f1fd7c1b962f"><td class="cell-title"><a href="https://www.notion.so/6-aad3346e521d4f448974f1fd7c1b962f">6</a></td><td class="cell-unYw"><code>**</code></td><td class="cell-uSR~">지수</td></tr><tr id="05192a69-ecfb-46d7-aff8-1ad790673c11"><td class="cell-title"><a href="https://www.notion.so/7-05192a69ecfb46d7aff81ad790673c11">7</a></td><td class="cell-unYw"><code>~x</code></td><td class="cell-uSR~">비트 연산 NOT</td></tr><tr id="fab3950b-4c20-4efc-826b-163fc32453d2"><td class="cell-title"><a href="https://www.notion.so/8-fab3950b4c204efc826b163fc32453d2">8</a></td><td class="cell-unYw"><code>+x</code>, <code>-x</code></td><td class="cell-uSR~">양수, 음수</td></tr><tr id="ce205945-a037-49d6-98bb-a034d01088b6"><td class="cell-title"><a href="https://www.notion.so/9-ce205945a03749d698bba034d01088b6">9</a></td><td class="cell-unYw"><code>*</code>, <code>/</code>, <code>%</code></td><td class="cell-uSR~">곱, 나눗셈, 나머지</td></tr><tr id="cf6eada6-b5f7-4f7f-badd-39e6694bdcae"><td class="cell-title"><a href="https://www.notion.so/10-cf6eada6b5f74f7fbadd39e6694bdcae">10</a></td><td class="cell-unYw"><code>+</code>, <code>-</code></td><td class="cell-uSR~">덧셈, 뺄셈</td></tr><tr id="76361fa4-e7e4-463b-a9db-e30e2984ab2d"><td class="cell-title"><a href="https://www.notion.so/11-76361fa4e7e4463ba9dbe30e2984ab2d">11</a></td><td class="cell-unYw"><code>&lt;&lt;</code>, <code>&gt;&gt;</code></td><td class="cell-uSR~">비트 연산 시프트</td></tr><tr id="4803c1cc-1b43-4ef1-9ff5-48dd83970b39"><td class="cell-title"><a href="https://www.notion.so/12-4803c1cc1b434ef19ff548dd83970b39">12</a></td><td class="cell-unYw"><code>&amp;</code></td><td class="cell-uSR~">비트 연산 AND</td></tr><tr id="24018f98-450d-4376-858f-8147981c12c3"><td class="cell-title"><a href="https://www.notion.so/13-24018f98450d4376858f8147981c12c3">13</a></td><td class="cell-unYw"><code>^</code></td><td class="cell-uSR~">비트 연산 XOR</td></tr><tr id="8b9797aa-e64f-4d27-9066-21a438f982a7"><td class="cell-title"><a href="https://www.notion.so/14-8b9797aae64f4d27906621a438f982a7">14</a></td><td class="cell-unYw"><code>|</code></td><td class="cell-uSR~">비트 연산 OR</td></tr><tr id="407fe14f-d89b-43c4-af4b-782fc0abc35f"><td class="cell-title"><a href="https://www.notion.so/15-407fe14fd89b43c4af4b782fc0abc35f">15</a></td><td class="cell-unYw"><code>in</code>, <code>not in</code>, <code>is</code>, <code>is not</code>, <code>&lt;</code>, <code>≤</code>, <code>&gt;</code>, <code>≥</code>,<code> &lt;&gt;</code>, <code>≠</code>, <code>==</code></td><td class="cell-uSR~">비교 연산</td></tr><tr id="d9be87f2-dab4-4c1e-a144-761d595e8b67"><td class="cell-title"><a href="https://www.notion.so/16-d9be87f2dab44c1ea144761d595e8b67">16</a></td><td class="cell-unYw"><code>not x</code></td><td class="cell-uSR~">Bool 연산 NOT</td></tr><tr id="9579be8b-ace0-48b1-939b-6887eecfbf76"><td class="cell-title"><a href="https://www.notion.so/17-9579be8bace048b1939b6887eecfbf76">17</a></td><td class="cell-unYw"><code>and</code></td><td class="cell-uSR~">Bool 연산 AND</td></tr><tr id="39c66842-511b-4f12-8ee5-eb01ace23411"><td class="cell-title"><a href="https://www.notion.so/18-39c66842511b4f128ee5eb01ace23411">18</a></td><td class="cell-unYw"><code>or</code></td><td class="cell-uSR~">Bool 연산 OR</td></tr><tr id="1f6124b3-d02e-4495-af8a-47b71b79553a"><td class="cell-title"><a href="https://www.notion.so/19-1f6124b3d02e4495af8a47b71b79553a">19</a></td><td class="cell-unYw"><code>lambda</code></td><td class="cell-uSR~">람다 표현</td></tr></tbody></table></div>

## 변수 스왑

두 변수를 스왑하는 가장 일반적이고 널리 사용되는 방법은 임시 변수를 사용하는 방법이다.

```python
temp = a
a = b
b = temp
```

이 방식은 거의 모든 언어에서 사용할 수 있는 가장 기본적인 방식이다. 그러나 Python에서는 다중 할당을 통해 이 변수 스왑이 가능하며 가독성 또한 좋아 별다른 이슈가 없는 한 이 방식으로 스왑하는게 가장 좋다.

```python
a: int = 1
b: int = 2
a, b = b, a
```

숫자형인 경우 간단한 덧셈, 뺄셈을 통해 임시 변수 없이 스왑을 할 수 있는 방법이 있고 다음과 같이 가능하다.

```python
x += y
y = x - y
x -= y
```

여기서 만약 입력값이 `x=9`, `y=4`라고 가정한다면 결과는 다음과 같다.

1. `x += y`의 결과 `x=13`
2. `y = x -y`의 결과 `y=9`
3. `x -= y`의 결과 `x=4`

```python
>>> x = 9
>>> y = 4
>>> x += y
>>> y = x-y
>>> x -= y
>>> x
4
>>> y
9
```

이 방식은 추가 공간을 필요로 하지 않는다는 점에서 공간 복잡도 또한 필요하지 않은 좋은 방법이나 숫자여야 하는 등의 제약사항으로 실용적인 면은 다소 부족하다. 다만 간혹 오프라인 인터뷰에서 이런 질문이 나오기도 하여 알아둘 필요는 있다.
