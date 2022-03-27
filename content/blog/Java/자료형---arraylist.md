---
title: 자료형 - ArrayList
date: 2022-03-28 02:03:45
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

# ArrayList

```java
public class ArrayListTutorial {
    public static void main(String[] args) {
        ArrayList<String> pitches = new ArrayList<>();
        pitches.add("138");  // 데이터 삽입
        pitches.add("129");
        pitches.add("141");

        pitches.add(0, "133");  // 첫번째 위치에 133 삽입

        System.out.println(pitches);
        System.out.println(pitches.get(1));  // 특정 인덱스 값 접근
        System.out.println(pitches.size());  // 리스트 요소 갯수 반환
        System.out.println(pitches.contains("142"));  // 해당 요소가 있는지 확인하여 boolean 반환

        // remove
        System.out.println(pitches.remove("129"));  // "129" 요소를 삭제하고 그 결과 (bool) 리턴
        System.out.println(pitches.remove(0));  // 인덱스 0 항목을 삭제하고 삭제한 요소를 리턴
    }
}

/*
[133, 138, 129, 141]
138
4
false
true
133
*/
```

## Get

```java
System.out.println(pitches.get(1));
```

`ArrayList`의 특정 인덱스의 값을 가져온다.

## Size

```java
System.out.println(pitches.size());
```

`ArrayList`의 크기 (요소의 갯수)를 반환한다.

## Contains

```java
System.out.println(pitches.contains("142"));
```

인수로 주어진 요소가 있는지 확인하고 그 결과를 반환한다.

## Remove

### remove(객체)

```java
System.out.println(pitches.remove("129"));
```

객체에 해당하는 항목을 삭제하고 그 결과(`boolean`)를 반환한다.

### remove(index)

```java
System.out.println(pitches.remove(0));
```

인덱스에 해당하는 항목을 삭제하고 삭제된 요소를 반환한다.

## ArrayList 생성하기

```java
import java.util.ArrayList;
import java.util.Arrays;

/**
 * ArrayListCreate
 */
public class ArrayListCreate {
    public static void main(String[] args) {
        String[] data = {"138", "117", "145"};
        ArrayList<String> pitches = new ArrayList<>(Arrays.asList(data));

        System.out.println(pitches);
    }
}
```

이미 데이터가 존재하는 경우에는 보다 편하게 `ArrayList`를 생성할 수 있다.

```java
import java.util.ArrayList;
import java.util.Arrays;

/**
 * ArrayListCreate
 */
public class ArrayListCreate {
    public static void main(String[] args) {
        ArrayList<String> pitches = new ArrayList<>(Arrays.asList("138", "117", "145"));

        System.out.println(pitches);
    }
}
```

이미 데이터가 존재하지 않더라도 `java.util.Arrays`의 `asList`메서드를 사용하여 여러개의 문자를 전달하여 `ArrayList`를 생성할 수 있다.

## String.join

```java
import java.util.ArrayList;
import java.util.Arrays;

/**
 * ArrayListCreate
 */
public class ArrayListCreate {
    public static void main(String[] args) {
        ArrayList<String> pitches = new ArrayList<>(Arrays.asList("138", "117", "145"));
        String result = String.join(",", pitches);

        System.out.println(result);
    }
}

// 138,129,142
```

`ArrayList`의 각 요소를 이어 붙인다.

## 정렬

### 오름차순

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;

/**
 * ArrayListCreate
 */
public class ArrayListCreate {
    public static void main(String[] args) {
        ArrayList<String> pitches = new ArrayList<>(Arrays.asList("138", "117", "145"));
        pitches.sort(Comparator.naturalOrder());

        System.out.println(pitches);
    }
}

// [117, 138, 145]
```

### 내림차순

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;

/**
 * ArrayListCreate
 */
public class ArrayListCreate {
    public static void main(String[] args) {
        ArrayList<String> pitches = new ArrayList<>(Arrays.asList("138", "117", "145"));
        pitches.sort(Comparator.reverseOrder());

        System.out.println(pitches);
    }
}

// [145, 138, 117]
```

# Generics

`J2SE 5.0` 이후 부터 아래와 같이 자료형을 선언하면 경고가 발생한다.

```java
ArrayList pitches = new ArrayList()
```

따라서 `ArrayList`객체에 어떤 자료형이 담기는지 명확하게 명시하는것을 권장하고 있다.

```java
ArrayList<String> pitches = new ArrayList<>();
```

다만, `ArrayList<String> pitches = new ArrayList<String>();`과 같이 앞뒤 모두 자료형 선언시 일부 IDE에서는 경고를 발생시킨다.

```java
ArrayList pitches = new ArrayList();
aList.add("138");
aList.add("129");

String one = (String) pitches.get(0);  // Object 자료형을 String으로 캐스팅
String two = (String) pitches.get(1);
```

만약 위와 같이 제네릭스를 사용하지 않고 `ArrayList`를 만든다면 `ArrayList`에 추가된 요소는 모두 `Object` 자료형으로 인식되어 요소를 가져올때 원하는 자료형으로 캐스팅이 되어야 한다. 또한 제네릭스를 사용하지 않은 `ArrayList`에는 내가 원하는 자료형만 있는것은 아니기 때문에 캐스팅시 오류가 발생할 수 있다.
