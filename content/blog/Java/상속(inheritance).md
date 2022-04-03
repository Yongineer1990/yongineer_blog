---
title: 상속(Inheritance)
date: 2022-04-04 03:04:89
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

## 상속

```java
class Animal {
    String name;

    void setName(String name) {
        this.name = name;
    }
}

class Dog extends Animal {  // 상속

}

/**
 * Sample
 */
public class Sample {

    public static void main(String[] args) {
				// Dog 클래스에 name 객체 변수와 setName 메소드를 생성하지 않았지만 사용 가능
        Dog dog = new Dog();
        dog.setName("choco");
        System.out.println(dog.name);
    }
}
```

- `extends`키워드로 상속 가능
- 상속시 부모의 메소드를 그대로 사용 가능

## 부모 클래스의 기능 확장

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
    } // 부모 클래스보다 더 많은 메소드 정의
}

/**
 * Sample
 */
public class Sample {

    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.setName("choco");
        System.out.println(dog.name);
        dog.sleep();  // 확장된 자식 메소드 사용
    }
}
```

- 자식 클래스는 부모 클래스보다 더 많은 기능을 가질 수 있다.

## IS-A 관계

앞선 예시에서 `Dog` 클래스는 `Animal` 클래스를 상속 받았다. 즉, `Dog`은 `Animal`의 하위 개념이라고 할 수 있다. 이런 경우 자바에서는 Dog `is a` Animal 즉, `IS-A` 관계라고 표현한다.

이렇게 `IS-A` 관계에 있을때 자식 클래스의 객체는 부모 클래스의 자료형인 것 처럼 사용할 수 있다.

따라서 다음과 같이 작성이 가능하다.

```java
Animal dog = new Dog();  // Dog is a Animal
```

다만 한가지 주의 해야 할 점은 `Dog`객체를 `Animal` 자료형으로 사용할 경우에는 `Dog` 클래스에만 존재하는 확장 기능 (`Sleep` 메소드)을 사용 할 수 없다는 점이다. 위 경우에는 `Animal` 클래스에서 구현한 `setName` 메소드만 사용 가능하다.

또한, 부모클래스로 만들어진 객체를 자식 클래스의 자료형으로는 사용할 수 없다.

```java
Dog dog = new Animal();  // 컴파일 오류: 부모 클래스로 만든 객체는 자식 클래스의 자료형으로 사용할 수 없다.
```

> **💡 Object Class**
>
>
> 자바의 모든 클래스는 `Object` 클래스를 상속 받는다. 따라서 앞서 만든 `Animal` 클래스는 사실 다음과 같다.
>
> ```java
> class Animal extends Object {
>     String name;
>
>     void setName(String name) {
>         this.name = name;
>     }
> }
> ```
>
> 그러나 자바에서는 `Object` 클래스를 자동으로 상속 받게 구현되어 있다. 따라서 생략이 가능한 것이며 이 의미는 다음과 같이 작성하는 것도 가능하다는 말과 같다.
>
> ```java
> Object animal = new Animal();  // Animal is a Object
> Object dog = new Dog();  // Dog is a Object
> ```

## Method overriding

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
    void sleep() {
        System.out.println(this.name + " zzz in house");
    }  // Method overriding
}

public class Sample {
    public static void main(String[] args) {
        HouseDog houseDog = new HouseDog();
        houseDog.setName("happy");
        houseDog.sleep();  // happy zzz in house 출력
    }
}
```

- 부모클래스의 메소드를 자식클래스가 동일한 형태로 구현하는 행위를 `Method overriding` 이라고한다.
- 오버라이딩된 메소드는 부모의 메소드보다 높은 우선순위를 가지게 된다.

## Method overloading

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
    void sleep() {
        System.out.println(this.name + " zzz in house");
    }

    void sleep(int hour) {
        System.out.println(this.name + " zzz in house for " + hour + " hours");
    }  // Method overloading
}

public class Sample {
    public static void main(String[] args) {
        HouseDog houseDog = new HouseDog();
        houseDog.setName("happy");
        houseDog.sleep();  // happy zzz in house 출력
        houseDog.sleep(3);  // happy zzz in house for 3 hours 출력
    }
}
```

- 동일한 메소드를 매개변수만 다르게 생성할때 `Method overloading`이라 한다.

## 다중상속

- 자바는 다중 상속을 지원하지 않는다.
