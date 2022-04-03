---
title: 제어문 - for each
date: 2022-04-04 00:34:47
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

```java
/**
 * ForEach
 */
public class ForEach {
    public static void main(String[] args) {
        String[] numbers = {"one", "two", "three"};

        // for
        for(int i=0; i<numbers.length; i++) {
            System.out.println(numbers[i]);
        }

        // for each
        for(String number: numbers) {
            System.out.println(number);
        }
    }

}
```

- `iterate` 객체에 루프를 실행할 때는 일반적인 `for loop`이 아닌 `for each loop`를 수행 할 수 있다.

> 💡 Iterator 객체 종류
>
> ---
> - Array
> - List
> - ArrayList
> - Set
