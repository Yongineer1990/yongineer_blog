---
title: Casting & Final
date: 2022-04-03 04:04:77
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

# Casting

## String → Integer

```java
/**
 * Casting
 */
public class Casting {

    public static void main(String[] args) {
        String num = "123";
        int n = Integer.parseInt(num);

        System.out.println(n);  // 123
    }
}
```

## Integer → String

### Case 1.

```java
/**
 * Casting
 */
public class Casting {

    public static void main(String[] args) {
        int num = 123;
        String n = "" + num;

        System.out.println(n);  // "123"
    }
}
```

### Case 2.

```java
/**
 * Casting
 */
public class Casting {

    public static void main(String[] args) {
        int num = 123;
        String n = String.valueOf(num);

        System.out.println(n);  // "123"
    }
}
```

### Case 3.

```java
/**
 * Casting
 */
public class Casting {

    public static void main(String[] args) {
        int num = 123;
        String n = Integer.toString(num);

        System.out.println(n);  // "123"
    }
}
```

## String → Double

```java
/**
 * Casting
 */
public class Casting {

    public static void main(String[] args) {
        String num = "123.456";
        double n = Double.parseDouble(num);

        System.out.println(n);  // 123.456
    }
}
```

## Interger → Double

```java
/**
 * Casting
 */
public class Casting {

    public static void main(String[] args) {
        int num = 123;
        double n = num;

        System.out.println(n);  // 123.0
    }
}
```

## Double → Integer

```java
/**
 * Casting
 */
public class Casting {

    public static void main(String[] args) {
        double num = 123.456;
        int n = (int) num;

        System.out.println(n);  // 123
    }
}
```

## Exception

```java
/**
 * Casting
 */
public class Casting {

    public static void main(String[] args) {
        String num = "123.456";
        int n = Integer.parseInt(num);
    }
}

/* Output
Exception in thread "main" java.lang.NumberFormatException: For input string: "123.456"
```

- 실수 형태의 문자열을 정수로 변환하려고 하면 `NumberFormatException`가 발생한다.

# Final

- 자료형에 값을 단 한번만 설정할 수 있게 강제하는 키워드
- 즉, 값을 한번 설정하면 그 값을 다시 설정할 수 없다.

```java
/**
 * Final
 */
public class Final {
    public static void main(String[] args) {
        final int n = 123;
        n = 456;  // 컴파일 에러
    }
}
```

- 리스트의 경우에도 동일하게 적용한다.

```java
import java.util.ArrayList;
import java.util.Arrays;

/**
 * Final
 */
public class Final {

    public static void main(String[] args) {
        final ArrayList<String> a = new ArrayList<>(Arrays.asList("a", "b"));
        a = new ArrayList<>(Arrays.asList("c", "d"));  // 컴파일 에러
    }
}
```

- 다만, 리스트의 경우 값을 더하거나(`add`), 빼는것(`remove`)는 가능하다.

```java
import java.util.ArrayList;
import java.util.Arrays;

/**
 * Final
 */
public class Final {

    public static void main(String[] args) {
        final ArrayList<String> a = new ArrayList<>(Arrays.asList("a", "b"));

        a.add("c");
        System.out.println(a);

        a.remove("a");
        System.out.println(a);
    }
}
```

- `final`은 주로 프로그램 수행 도중 값이 변경되면 안되는 상황에서 사용한다.

> 💡 Unmodifiable List
>
> - 앞서 살펴 본 것 처럼 `final`로 선언된 리스트는 값을 더하거나 빼는것은 가능하다. 다만, 재할당이 불가능할 뿐이다.
> - 만약 리스트에서 값을 더하거나 빼는것도 불가능하게 만드려면 `List.of`를 통해 수정이 불가능한 리스트 (`Unmodifiable List`)로 생성해야 한다.
>
> ```java
> import java.util.List;
>
> /**
>  * Final
>  */
> public class Final {
>
>     public static void main(String[] args) {
>         final List<String> a = List.of("a", "b");
>
>         a.add("c");  // UnsupportedOperationException
>         a.remove("a"); // UnsupportedOperationException
>     }
> }
> ```
