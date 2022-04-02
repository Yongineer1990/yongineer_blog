---
title: 자료형 - Set
date: 2022-04-03 03:04:67
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)


### Sample

```java
import java.util.Arrays;
import java.util.HashSet;

/**
 * Set
 */
public class Set {

    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>(Arrays.asList("H", "e", "l", "l", "o"));
        System.out.println(set);
    }
}

// Output
[e, H, l, o]
```

# 특징

- 중복을 허용하지 않는다.
- 순서가 없다

# 교집합, 합집합, 차집합

### 교집합

```java
import java.util.Arrays;
import java.util.HashSet;

/**
 * Set
 */
public class Set {

    public static void main(String[] args) {
        HashSet<Integer> s1 = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5, 6));
        HashSet<Integer> s2 = new HashSet<>(Arrays.asList(4, 5, 6, 7, 8, 9));

        // 교집합
        HashSet<Integer> intersection = new HashSet<>(s1);
        intersection.retainAll(s2);
        System.out.println(intersection);
    }
}

// Output
[4, 5, 6]
```

- `retainAll`을 통해서 구할 수 있다.

### 합집합

```java
import java.util.Arrays;
import java.util.HashSet;

/**
 * Set
 */
public class Set {

    public static void main(String[] args) {
        HashSet<Integer> s1 = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5, 6));
        HashSet<Integer> s2 = new HashSet<>(Arrays.asList(4, 5, 6, 7, 8, 9));

        // 합집합
        HashSet<Integer> union = new HashSet<>(s1);
        union.addAll(s2);
        System.out.println(union);
    }
}

// Output
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- `addAll` 메서드를 통해서 구할 수 있다.

### 차집합

```java
import java.util.Arrays;
import java.util.HashSet;

/**
 * Set
 */
public class Set {

    public static void main(String[] args) {
        HashSet<Integer> s1 = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5, 6));
        HashSet<Integer> s2 = new HashSet<>(Arrays.asList(4, 5, 6, 7, 8, 9));

        // 차집합
        HashSet<Integer> substract = new HashSet<>(s1);
        substract.removeAll(s2);
        System.out.println(substract);
     }
}

// Output
[1, 2, 3]
```

- `removeAll` 메서드를 통해서 구할 수 있다.

## 관련 메서드

### add

```java
import java.util.HashSet;

/**
 * Set
 */
public class Set {

    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();

        set.add("Hello");
        set.add("world!");

        System.out.println(set);
    }
}

// Output
[Hello, world!]
```

- `Set` 자료형에 값을 추가 할 때는 `add` 메서드를 사용한다.

### addAll

```java
import java.util.Arrays;
import java.util.HashSet;

/**
 * Set
 */
public class Set {

    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();
        set.addAll(Arrays.asList("Hello", "World!"));
        System.out.println(set);
    }
}

// Output
[Hello, world!]
```

- `Set` 자료형에 값을 여러개 추가 할 때는 `addAll` 메서드를 사용한다.

### remove

```java
import java.util.Arrays;
import java.util.HashSet;

/**
 * Set
 */
public class Set {

    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>(Arrays.asList("Hello", "world!"));
        set.remove("world!");
        System.out.println(set);
    }
}

// Output
[Hello]
```

- `Set` 자료형에서 값을 삭제하고 싶을 때는 `remove` 메서드를 사용한다.


> **💡 `TreeSet` & `LinkedHashSet`**
>
> - `TreeSet` : 오름차순으로 값을 정렬하여 저장하는 특징이 있다.
> - `LinkedHashSet` : 입력한 순서대로 값을 정렬하는 특징이 있다.
