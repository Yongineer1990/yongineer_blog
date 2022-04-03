---
title: Class
date: 2022-04-04 01:04:69
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

```java
class Animal {
    String name; // 객체 변수, 인스턴스 변수, 멤버 변수, 속성

    public void setName(String name) {  // 메소드 생성
        this.name = name;
    }
}

/**
 * Sample
 */
public class Sample {
    public static void main(String[] args) {
        Animal cat = new Animal(); // 객체 생성, 초기화
        cat.setName("kitty");
        System.out.println(cat.name);

        Animal dog = new Animal();
        dog.setName("puppy");  // 객체 변수는 공유되지 않는다.
        System.out.println(dog.name);
    }
}
```
보통의 경우 `Class`는 파일 단위로 관리된다. 위의 경우 `Animal` 클래스는 당연히 `Animal.java`에서 단독으로 관리하는것이 일반적이다.

## 객체

### 객체 생성 (초기화)

```java
class Animal {
}

public class Sample {
    public static void main(String[] args) {
        Animal cat = new Animal();
    }
}
```

- `new`키워드를 통해 객체를 생성한다.
- 클래스(`Animal`)는 설계도이며
- 설계도에 의해 만들어진 제품(`cat`)은 객체가 된다.
- 따라서 다음과 같이 무수히 많은 객체를 생성 할 수도 있다.

    ```java
    Animal cat = new Animal();
    Animal dog = new Animal();
    Animal horse = new Animal();
    ...
    ```


> **💡 객체와 인스턴스**
>
> - `Class`에 의해 만들어진 객체를 `Instance`라고도 한다.
> - 즉, `Animal cat = new Animal()` 이렇게 만들어진 `cat`은 객체이다. 그리고 `cat`이라는 객체는 `Animal`의 인스턴스 이다.
> - 인스턴스는 객체(`cat`)이 어떤 클래스(`Animal`)의 객체인지를 관계 위주로 설명할 때 사용된다.
> - 따라서 `cat`은 인스턴스 보다는 `cat`은 객체가 더 올바른 표현이며
> - `cat`은 `Animal`의 객체 보다는 `cat`은 `Animal`의 인스턴스 라는 표현이 올바른 표현이다.

### 객체 변수

```java
class Animal {
    String name;
}

public class Sample {
    public static void main(String[] args) {
        Animal cat = new Animal();
    }
}
```

- 클래스에 선언된 변수를 객체변수라고 한다.
- 인스턴스 변수, 멤버 변수, 속성이라고도 한다.
- 객체 변수는 도트연산자(`.`)를 통해 접근 가능 하다.

    ```java
    cat.name   // 객체: cat, 객체변수: name
    ```

- 다만, 위 예시에서는 객체 변수에 값이 할당되어 있지 않은 상태로 `null`이 출력된다.

# 메소드

```java
class Animal {
    String name;

    public void setName(String name) {
        this.name = name;
    }
}

public class Sample {
    public static void main(String[] args) {
        Animal cat = new Animal();
        System.out.println(cat.name);
    }
}
```

- 클래스 내부에 구현된 함수를 의미한다.
- 위 예시, `Animal` 클래스에 생성된 `setName` 메소드는 다음과 같은 형태의 메소드이다.
    - Input : `String name`
    - Output : `void` (리턴값 없음)

## This

```java
class Animal {
    String name;

    public void setName(String name) {
        this.name = name;
    }
}

public class Sample {
    public static void main(String[] args) {
        Animal cat = new Animal();
        cat.setName("boby");  // 메소드 호출
        System.out.println(cat.name);
    }
}
```

- 메소드를 호출하기 위해서는 객체변수와 마찬가지로 도트연산자(`.`)를 사용한다.

    ```java
    cat.setName("boby");
    ```

- 이때 `this`는 클래스에 의해 생성된 객체를 지칭한다. 즉, `Animal cat = new Animal()`과 같이 `cat` 객체를 생성하고 `cat.setName(”boby”)`와 같이 `cat`객체에 의해 `setName` 메소드를 호출하면 `setName` 메소드 내부에 선언된 `this`는 `cat`을 가리킨다.

## 객체 변수는 공유 되지 않는다.

```java
class Animal {
    String name;

    public void setName(String name) {
        this.name = name;
    }
}

public class Sample {
    public static void main(String[] args) {
        Animal cat = new Animal();
        cat.setName("boby");

        Animal dog = new Animal();
        dog.setName("happy");

				System.out.println(cat.name);
        System.out.println(dog.name);
    }
}

/* Output
boby
happy
*/
```

- 새롭게 생성된 객체 끼리는 변수를 공유하지 않고 독립적으로 유지 된다.
- 단, `Static`을 이용하게 되면 객체 변수를 공유 하도록 만들 수 있다.
