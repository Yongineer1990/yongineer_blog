---
title: Leetcode - 706. Design HashMap
date: 2021-08-12 00:08:75
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

다음의 기능을 제공하는 해시맵을 디자인하라

- `put(key, value)` : 키, 값을 해시맵에 삽입한다. 만약 이미 존재하는 키라면 업데이트 한다.
- `get(key)` : 키에 해당하는 값을 조회한다. 만약 키가 존재하지 않는다면 `-1`을 리턴한다.
- `remove(key)` : 키에 해당하는 키, 값을 해시맵에서 삭제한다.

### Example 1.

```python
Input
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
Output
[null, null, null, 1, -1, null, 1, null, -1]

Explanation
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // The map is now [[1,1]]
myHashMap.put(2, 2); // The map is now [[1,1], [2,2]]
myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]]
myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]]
myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value)
myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]]
myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]]
myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]]
```

# 나의 풀이

```python
class MyHashMap:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.d = {}

    def put(self, key: int, value: int) -> None:
        """
        value will always be non-negative.
        """
        self.d[key] = value

    def get(self, key: int) -> int:
        """
        Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key
        """
        try:
            return self.d[key]
        except KeyError:
            return -1

    def remove(self, key: int) -> None:
        """
        Removes the mapping of the specified value key if this map contains a mapping for the key
        """
        try:
            del self.d[key]
        except KeyError:
            return None

# Your MyHashMap object will be instantiated and called as such:
# obj = MyHashMap()
# obj.put(key,value)
# param_2 = obj.get(key)
# obj.remove(key)
```

![https://yongineer.duckdns.org/python/leetcode_706_1.png](https://yongineer.duckdns.org/python/leetcode_706_1.png)

기본적인 Python의 Dictionary를 다루는 문제같아서 따로 설명할건 없는것 같다.
