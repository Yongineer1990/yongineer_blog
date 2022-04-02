---
title: 자료형 - Enum
date: 2022-04-03 03:54:56
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

Enum은 서로 관련이 있는 여러개의 상수 집합을 정의할 때 사용하며 아래와 같이 생성 할 수 있다.

```java
enum CoffeeType {
    AMERICANO,
    ICE_AMERICANO,
    CAFE_LATTE
};
```

### Sample

```java
/**
 * Enum
 */
public class Enum {

    enum CoffeeType {
        AMERICANO,
        ICE_AMERICANO,
        CAFE_LATTE
    };

    public static void main(String[] args) {
        System.out.println(CoffeeType.AMERICANO);  // AMERICANO
        System.out.println(CoffeeType.ICE_AMERICANO);  // ICE_AMERICANO
        System.out.println(CoffeeType.CAFE_LATTE);  // CAFE_LATTE
    }
}
```

또한 반복문에서도 사용 가능 하다.

```java
/**
 * Enum
 */
public class Enum {

    enum CoffeeType {
        AMERICANO,
        ICE_AMERICANO,
        CAFE_LATTE
    };

    public static void main(String[] args) {
        for(CoffeeType type: CoffeeType.values()) {
            System.out.println(type);
        }
    }
}

/* Output
AMERICANO
ICE_AMERICANO
CAFE_LATTE
*/
```

### 사용하는 이유

- 매직넘버를 사용할 때 보다 코드가 명확해진다.
- 잘못된 값을 사용함으로 인해 발생할 수 있는 위험성이 사라진다.

> 💡 매직넘버?
>
> - 프로그래밍에서 상수로 선언하지 않은 숫자를 말한다.
