---
title: 자료형 - Map
date: 2022-03-28 03:03:65
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

### Sample

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 * Map
 */
public class Map {
    public static void main(String[] args) {
        HashMap<String, String> map = new HashMap<>();
        map.put("people", "Lewis Hamilton");
        map.put("team", "Mercedes AMG Petronas");

        System.out.println(map);
        System.out.println(map.get("people"));  // people 키에 해당하는 값 반환
        System.out.println(map.get("number"));  // 없는 키 접근시 null 리턴
        System.out.println(map.getOrDefault("number", "NoNumber"));  // 디폴트를 지정한다면 없는 키 접근시 디폴트 반환
        System.out.println(map.containsKey("team"));  // 전달된 키가 있는지 조사하여 그 결과를 boolean으로 반환
        System.out.println(map.size());  // HashMap size (키 갯수) 반환

        System.out.println(map.keySet());  // HashMap의 모든 키를 모아서 Set으로 반환
        List<String> keyList = new ArrayList<>(map.keySet());  // List로 바꿔서 반환도 가능하다.
        System.out.println(keyList);

        System.out.println(map.remove("team"));  // 전달된 키에 해당하는 데이터를 삭제하고 삭제된 value를 반환
        System.out.println(map);
    }
}

/*
Output

{team=Mercedes AMG Petronas, people=Lewis Hamilton}
Lewis Hamilton
null
NoNumber
true
2
[team, people]
[team, people]
Mercedes AMG Petronas
{people=Lewis Hamilton}
*/
```

`Python`의 `Dictionary`와 비슷하다.

## HashMap

### put

```java
map.put(key, value);
```

데이터 삽입

### get

```java
map.get(key);
```

`key`에 해당하는 `value`를 얻는다.

### getOrDefault

```java
map.getOrDefault(key, default);
```

기본적으로 `value`가 없는 키에 접근시 null이 반환된다 이때 `getOrDefault`메서드를 사용하면 기본값을 지정하여 `value`가 없더라도 지정한 기본값이 반환된다.

### containsKey

```java
map.containsKey(key);
```

주어진 `key`가 존재하는지에 대한 여부를 `boolean`으로 반환한다.

### remove

```java
map.remove(key);
```

주어진 `key`에 대한 아이템을 삭제하고 삭제된 `value`를 반환한다.

### size

```java
map.size();
```

`Map`의 개수 (`key` 갯수)를 반환한다.

### keySet

```java
map.keySet();
```

`Map`의 모든 `key`를 `Set` 자료형으로 반환한다.

```java
List<String> keyList = new ArrayList<>(map.keySet());
```

또한 위와 같이 `List`자료형으로 바꿔서 사용도 가능하다.

> ### 💡 `LinkedHashMap` 과 `TreeMap`
>
> - `LinkedHashMap`은 입력된 **순서대로** 데이터를 저장하는 특징을 가지고 있다.
> - `TreeMap`은 입력된 **key의 오름차순 순서**로 데이터를 저장하는 특징을 가지고 있다.
