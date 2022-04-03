---
title: Method
date: 2022-04-04 02:04:14
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)


```java
/**
 * Method
 */
public class Method {
    // 일반적인 메소드
    int sum (int a, int b) {
        return a+b;
    }

    // 입력값이 없는 메소드
    String say() {
        return "Hi";
    }

    // 리턴값이 없는 메소드
    void noReturnSum(int a, int b) {
        System.out.println(a+"과 "+b+"의 합은 "+(a+b)+"입니다.");
    }

    // 입력값과 리턴값 둘다 없는 메소드
    void noParamsNoReturnSay() {
        System.out.println("Hi");
    }

    // return은 메소드 탈출용으로 사용 가능
    void sayNick(String nick) {
        if ("fool".equals(nick)) {
            return;
        }
        System.out.println(nick);
    }

    public static void main(String[] args) {
        int a = 3;
        int b = 4;

        Method method = new Method();

        int c = method.sum(a, b);
        System.out.println(c);

        System.out.println(method.say());

        method.noReturnSum(a, b);

        method.noParamsNoReturnSay();

        method.sayNick("Aiden");
        method.sayNick("fool");  // 출력되지 않는다.
    }
}

/* Output
7
Hi
3과 4의 합은 7입니다.
Hi
Aiden
*/
```

- 자바에서는 함수가 따로 존재 하지 않고 모든것을 메소드라고 한다.
- 또한 모든 메소드는 클래스 내에 존재한다.

## 매개변수와 인수

```java
public class Sample {
    int sum(int a, int b) {  // a, b 는 매개변수
        return a+b;
    }

    public static void main(String[] args) {
        Sample sample = new Sample();
        int c = sample.sum(3, 4);  // 3, 4는 인수

        System.out.println(c);  // 7 출력
    }
}
```

## 입력값과 리턴값

### 메소드의 구조

```java
리턴자료형 메소드명(입력자료형1 매개변수1, 입력자료형2 매개변수2, ...) {
    ...
    return 리턴값;  // 리턴자료형이 void 인 경우에는 return 문이 필요없다.
}
```

- 리턴 자료형 : 메소드 수행 후 돌려줄 값의 자료형
- 리턴값은 `return`명령을 사용한다.

### 일반적인 메소드

```java
int sum(int a, int b) {
    return a+b;
}
```

- 입력값 : `int a, int b`
- 리턴값 : `int`
- 사용법

    ```java
    Sample sample = new Sample();
    int result = sample.sum(3, 4);
    ```


### 입력값이 없는 메소드

```java
String say() {
    return "Hi";
}
```

- 입력값 : 없음
- 리턴값 : `String`
- 사용법

    ```java
    public class Sample {
        String say() {
            return "Hi";
        }

        public static void main(String[] args) {
            Sample sample = new Sample();
            String a = sample.say();
            System.out.println(a);  // "Hi" 출력
        }
    }
    ```


### 리턴값이 없는 메소드

```java
void sum(int a, int b) {
    System.out.println(a+"과 "+b+"의 합은 "+(a+b)+"입니다.");
}
```

- 입력값 : `int a, int b`
- 리턴값 : `void` (없음)
- 리턴값이 없는 메소드는 명시적으로 리턴 타입을 `void`로 선언해준다.
- 사용법

    ```java
    public class Sample {
        void sum(int a, int b) {
            System.out.println(a+"과 "+b+"의 합은 "+(a+b)+"입니다.");
        }

        public static void main(String[] args) {
            Sample sample = new Sample();
            sample.sum(3, 4);
        }
    }
    ```


### 입력값도 리턴값도 없는 메소드

```java
void say() {
    System.out.println("Hi");
}
```

- 입력값 : 없음
- 리턴값 : `void` (없음)
- 사용법

    ```java
    public class Sample {
        void say() {
            System.out.println("Hi");
        }

        public static void main(String[] args) {
            Sample sample = new Sample();
            sample.say();
        }
    }
    ```


### Return은 메소드를 즉시 탈출 시킨다.

```java
public class Sample {
    void sayNick(String nick) {
        if ("fool".equals(nick)) {
            return;
        }
        System.out.println("나의 별명은 "+nick+" 입니다.");
    }

    public static void main(String[] args) {
        Sample sample = new Sample();
        sample.sayNick("angel");
        sample.sayNick("fool");  // 출력되지 않는다.
    }
}
```

- `return`은 메서드를 즉시 탈출 시킨다.
- 특정 조건에 따라 메소드를 즉시 탈출 하고 싶을때 사용한다.
- 단, `return`을 통한 탍출은 리턴 자료형이 `void`인 메소드만 가능하다.

## Scope

```java
public class Sample {
    void varTest(int a) {
        a++;
    }

    public static void main(String[] args) {
        int a = 1;
        Sample sample = new Sample();
        sample.varTest(a); // 2
        System.out.println(a); // 1
    }
}
```

- 메소드 내에서 선언된 변수는 메소드 내에서만 쓰이며 이를 로컬 변수 (`local variable`)이라고 한다.
- 다만, 메소드에서 넘어온 값 재할당 한다면 증가 시킬 수 있다.

    ```java
    public class Sample {
        int varTest(int a) {
            a++;
            return a;
        }

        public static void main(String[] args) {
            int a = 1;
            Sample sample = new Sample();
            a = sample.varTest(a);
            System.out.println(a);
        }
    }
    ```

- 또한, 객체를 넘긴다면 메소드에서 처리한 결과를 그대로 이어 받을 수 있다.

    ```java
    public class Sample {

        int a;  // 객체변수 a

        void varTest(Sample sample) {
            sample.a++;
        }

        public static void main(String[] args) {
            Sample sample = new Sample();
            sample.a = 1;
            sample.varTest(sample);
            System.out.println(sample.a);
        }
    }
    ```

- 메소드가 객체를 전달받으면 메소드 내에 객체는 전달받은 객체 그 자체로 수행된다. 따라서 입력으로 전달 받은 객체의 객체변수의 값이 증가하게 되는 것이다.
- 이때 메소드의 입력항목이 값인지 객체인지 구별하는 기준은 입력항목의 자료형이 `primitive` 자료형인지 아닌지에 따라 나뉜다. int 자료형과 같은 `primitive` 자료형인 경우 값이 전달되고 그 이외의 경우 (`reference` 자료형)는 객체가 전달된다.
- 따라서 `this`를 활용할 수도 있다.

    ```java
    public class Sample {

        int a;  // 객체변수 a

        void varTest() {
            this.a++;
        }

        public static void main(String[] args) {
            Sample sample = new Sample();
            sample.a = 1;
            sample.varTest();
            System.out.println(sample.a);  // 2 출력
        }
    }
    ```
