---
title: Interface
date: 2022-04-04 04:04:69
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

만약 사육사 (ZooKeeper)가 육식동물이 들어오면 먹이(feed)를 주는 기능을 하는 코드를 작성한다고 가정해보자 이때, 호랑이가 오면 사과를 주고, 사자가 오면 바나나를 주는 기능도 포함되었다고 가정하고 코드를 작성한다면 다음과 같을 것 이다.

```java
class Animal {
    String name;

    void setName(String name) {
        this.name = name;
    }
}

class Tiger extends Animal {
}

class Lion extends Animal {
}

class ZooKeeper {
    void feed(Tiger tiger) {  // 호랑이가 오면 사과를 던져 준다.
        System.out.println("feed apple");
    }

    void feed(Lion lion) {  // 사자가 오면 바나나를 던져준다.
        System.out.println("feed banana");
    }
}

public class Sample {
    public static void main(String[] args) {
        ZooKeeper zooKeeper = new ZooKeeper();
        Tiger tiger = new Tiger();
        Lion lion = new Lion();
        zooKeeper.feed(tiger);  // feed apple 출력
        zooKeeper.feed(lion);  // feed banana 출력
    }
}
```

하지만 이 상황에서 계속해서 육식동물이 추가된다면 어떻게 해야할까? 아마 매번 feed 메서드를 추가해야 할 것이다. 이런 상황에서 인터페이스가 도움이 된다.

## Interface

```java
/**
 * Predator
 */
interface Predator {
    String getFood();

    default void printFood() {
        System.out.printf("my food is %s\n", getFood());
    }

    int LEG_COUNT = 4;

    static int speed() {
        return LEG_COUNT * 30;
    }
}

class Animal {
    String name;

    void setName(String name) {
        this.name = name;
    }
}

class Tiger extends Animal implements Predator {
    public String getFood() {
        return "apple";
    }
}

class Lion extends Animal implements Predator {
    public String getFood() {
        return "banana";
    }
}

class Crocodile extends Animal implements Predator {
    public String getFood() {
        return "apple";
    }
}

class ZooKeeper {
    void feed(Predator predator) {
        System.out.println("feed "+predator.getFood());
    }

    void myFood(Predator predator) {
        predator.printFood();
    }

    void mySpeed(Predator predator) {
        System.out.println(Predator.speed());
    }
}

/**
 * Sample
 */
public class Sample {
    public static void main(String[] args) {
        ZooKeeper zooKeeper = new ZooKeeper();
        Tiger tiger = new Tiger();
        Lion lion = new Lion();
        Crocodile crocodile = new Crocodile();

        zooKeeper.feed(tiger);
        zooKeeper.feed(lion);
        zooKeeper.feed(crocodile);

        zooKeeper.myFood(tiger);
        zooKeeper.myFood(lion);
        zooKeeper.myFood(crocodile);

        zooKeeper.mySpeed(tiger);
    }
}
```

인터페이스는 클래스와 마찬가지로 별도의 파일로 관리하는게 일반적인 방법이나 편의를 위해 동일한 경로에 작성되었다.

- `interface`라는 키워드를 통해 인터페이스를 생성 할 수 있다.
- 인터페이스를 구현할때는 `implements` 키워드를 사용한다.
- 위 예제에서 이제 tiger와 lion은 다음과 같은 속성을 갖는다.
    - tiger : `Tiger 클래스의 객체`, `Predator 인터페이스의 객체`
    - lion : `Lion 클래스의 객체`, `Predator 인터페이스의 객체`
- feed 메소드의 입력으로 Tiger, Lion의 객체가 필요했지만 Predator라는 인터페이스로 대체 할 수 있게 되었다.
- 즉, 구현체가 늘어나더라도 인터페이스는 하나이기 때문에 feed 메서드는 오버로딩 되지 않아도 된다.

## 인터페이스의 메소드

```java
interface Predator {
    String getFood();
}

... (생략) ...
```

- 인터페이스의 메소드는 항상 `public`으로 구현해야 한다.
- 인터페이스의 메소드는 내용(body)이 없다. 그 이유는 `implements`한 클래스들이 구현해야 하기 때문이다.

    ```java
    ... (생략) ...

    class Tiger extends Animal implements Predator {
        public String getFood() {
            return "apple";
        }
    }

    class Lion extends Animal implements Predator {
        public String getFood() {
            return "banana";
        }
    }

    ... (생략) ...
    ```

- 또한 구현체를 사용하려는 클래스 (`ZooKeeper`)클래스도 인터페이스의 메소드를 사용해야 한다.

    ```java
    ... (생략) ...

    class ZooKeeper {
        void feed(Predator predator) {
            System.out.println("feed "+predator.getFood());
        }
    }

    ... (생략) ...
    ```


### Default method

```java
interface Predator {
    String getFood();

    default void printFood() {
        System.out.printf("my food is %s\n", getFood());
    }
}
```

- `default` 키워드로 구현한다.
- 인터페이스의 메서드는 구현체를 가질수 없지만 디폴트를 사용하면 실제 구현된 형태의 메서드를 가질수 있다.
- `default method`는 `overriding`이 가능하다.

### Static method

```java
interface Predator {
    String getFood();

    default void printFood() {
        System.out.printf("my food is %s\n", getFood());
    }

    int LEG_COUNT = 4;  // 인터페이스 상수

    static int speed() {
        return LEG_COUNT * 30;
    }
}
```

- `Java 8` 이후 부터 인터페이스에서 `Static` 메서드를 사용 할 수 있다.
- 일반 클래스의 `Static method`를 사용하는 것 처럼 도트연산자(`.`)로 사용 가능하다.

    ```java
    Predator.speed();
    ```


> **💡 Interface consts**
>
> - 인터페이스에서 정의한 상수는 `public static final`을 생략해도 자동 적용 된다.
