---
title: Constructor
date: 2022-04-04 03:44:76
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)


## 생성자

```java
class Animal {
    String name;

    void setName(String name) {
        this.name = name;
    }
}

class Dog extends Animal {
    void sleep() {
        System.out.println(this.name+"zzz");
    }
}

class HouseDog extends Dog {
    HouseDog(String name) {
        this.setName(name);
    } // Constructor

    void sleep() {
        System.out.println(this.name+" zzz in house");
    }

    void sleep(int hour) {
        System.out.println(this.name+" zzz in house for " + hour + " hours");
    }
}

/**
 * Sample
 */
public class Sample {

    public static void main(String[] args) {
        HouseDog dog = new HouseDog("Happy"); // 생성자를 전달하지 않으면 컴파일 에러
        System.out.println(dog.name);
    }
}
```

- 반드시 객체변수에 값을 설정해야만 객체가 생성될 수 있도록 강제할때 생성자(`Constructor`)를 사용한다.
- 생성자가 만들어지면 생성자를 누락한 상태에서 객체 생성시 컴파일 에러가 발생한다.

    ```java
    HouseDog dog = new HouseDog("happy"); -> ⭕️
    HouseDog dog = new HouseDog(); -> ❌
    ```


### 생성자 규칙

1. 클래스명과 메소드명이 동일해야 한다.
2. `return` 타입을 명시 하지 않는다 (`void`도 허용하지 않는다.)

## Constructor overloading

```java
class Animal {
    String name;

    void setName(String name) {
        this.name = name;
    }
}

class Dog extends Animal {
    void sleep() {
        System.out.println(this.name + " zzz");
    }
}

class HouseDog extends Dog {
    HouseDog(String name) {
        this.setName(name);
    }

    HouseDog(int type) {
        if (type == 1) {
            this.setName("yorkshire");
        } else if (type == 2) {
            this.setName("bulldog");
        }
    } // Constructor overloading

    void sleep() {
        System.out.println(this.name + " zzz in house");
    }

    void sleep(int hour) {
        System.out.println(this.name + " zzz in house for " + hour + " hours");
    }
}

public class Sample {
    public static void main(String[] args) {
        HouseDog happy = new HouseDog("happy");
        HouseDog yorkshire = new HouseDog(1);  // 오버로딩된 생성자 사용
        System.out.println(happy.name);  // happy 출력
        System.out.println(yorkshire.name);  // yorkshire 출력
    }
}
```

- 메소드와 마찬가지로 매개변수가 다른 생성자를 추가 생성 할 수 있으며 이렇게 생성되는것을 `Constructor overloading`이라 한다.
