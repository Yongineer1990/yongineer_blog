---
title: 자료형 - Array
date: 2022-03-28 02:03:36
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)


```java
String[] weeks = {"월", "화", "수", "목", "금", "토", "일"};
int[] odds = {1, 3, 5, 7, 9};
```

- 자료형의 집합
- `[]`로 표현된다. (`int`형의 배열은 `int[]`로 표현된다.)

## 특징

### Sample

```java
public class Array {
    public static void main(String[] args) {
        String[] weeks = new String[7];
        weeks[0] = "mon";
        weeks[1] = "tue";
        weeks[2] = "wed";
        weeks[3] = "thu";
        weeks[4] = "fri";
        weeks[5] = "sat";
        weeks[6] = "sun";

        System.out.println(weeks[3]);
    }
}

// thu
```

### 배열의 길이는 고정되어 있다.

따라서

```java
String[] weeks = new String[];
```

위 처럼 초기값 없이 배열 변수 생성시 컴파일 오류가 발생한다.

### 인덱싱을 사용하여 접근 할 수 있다.

```java
System.out.println(weeks[3]);

// thu
```

### 길이만큼 for loop이 가능하다.

```java
String[] week = {"월", "화", "수", "목", "금", "토", "일"};
for (int i=0; i<weeks.length; i++) {
    System.out.println(weeks[i]);
}

/*
mon
tue
wed
thu
fri
sat
sun
*/
```

### 자주 만날 수 있는 오류

```java
ArrayIndexOutOfBoundsException
```

배열의 길이를 초과하여 접근시도시
