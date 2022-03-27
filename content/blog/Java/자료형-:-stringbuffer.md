---
title: 자료형 - StringBuffer
date: 2022-03-28 00:03:43
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

`StringBuffer`는 문자열을 추가하거나 변경 할 때 주로 사용하는 자료형이다.

## Append

### Sample

```java
public class StringBf {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer();
        sb.append("hello");
        sb.append(" ");
        sb.append("world!");

        String result = sb.toString();
        System.out.println(result);
    }
}

// hello world!
```

- `append` 메서드를 사용하여 문자열을 추가할 수 있다.
- `toString()`메서드를 사용하여 String 자료형으로 변환 가능하다.

```java
String result = "";
result += "hello";
result += " ";
result += "world!";
System.out.println(result);

// hello world!
```

위와 같이 `String` 자료형만 갖고 동일한 로직을 작성 할 수 있지만 메모리를 사용하는 과정은 다르다.

`StringBuffer` 객체는 한번만 생성되는 반면 `String` 자료형에 `+`연산이 발생할 때마다 새로운 String 객체가 생성된다.

또한, `String` 자료형은 한번 값이 생성되면 그 값을 변경할 수 없다. 이렇게 값을 변경할 수 없는 것을 `Immutable`이라고 한다. 그러나 `StringBuffer`는 이와 다르게 값을 변경 할 수 있다. (`Mutable`)

> ### 💡 StringBuffer VS StringBuilder
>
>
> `StringBuilder`의 사용법은 `StringBuffer`와 동일하다.
>
> ```java
> StringBuilder sb = new StringBuilder();
> sb.append("hello");
> sb.append(" ");
> sb.append("world!");
> String result = sb.toString();
> System.out.println(result);
> ```
>
> 다만, `StringBuffer`는 멀티 스레드 환경에서 안전하다는 장점이 있고 `StringBuilder`는 `StringBuffer`보다 성능이 우수하다는 장점이 있다. 따라서 동기화가 필요없다면 `StringBuilder`를 사용하는 것이 유리하다.

## Insert

### Sample

```java
public class StringBf {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer();

        sb.append("world!");
        sb.insert(0, "hello ");

        System.out.println(sb.toString());
    }
}

// hello world!
```

- 특정 위치에서 원하는 문자열을 삽입 할 수 있다.

## Substring

### Sample

```java
public class StringBf {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer();

        sb.append("hello world!");

        System.out.println(sb.substring(0, 5));
    }
}

// hello
```

- `substring(start, end)`로 `StringBuffer` 객체 시작 위치부터 끝 위치 까지의 문자를 가져온다.
