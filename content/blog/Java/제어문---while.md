---
title: 제어문 - While
date: 2022-08-10 02:08:18
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)

# 제어문 - While

```java
while (조건문) {
    <수행할 문장1>;
    <수행할 문장2>;
    <수행할 문장3>;
    ...
}
```

- 조건문이 참인 동안 `while`문의 수행할 문장들을 반복하여 수행한다.
- 예를들어 `10번 찍어 안 넘어가는 나무 없다`라는 요구사항을 `while`문으로 구현하면 다음과 같다.

    ```java
    public class TreeHit {
        public static void main(String[] args) {
            int treeHit = 0;
            while (treeHit < 10) {
                treeHit++;
                System.out.println("나무를 " + treeHit + "번 찍었습니다.");

                if (treeHit == 10) {
                    System.out.println("나무 넘어갑니다.");
                }
            }
        }
    }
    ```

    ```java
    나무를  1번 찍었습니다.
    나무를  2번 찍었습니다.
    나무를  3번 찍었습니다.
    나무를  4번 찍었습니다.
    나무를  5번 찍었습니다.
    나무를  6번 찍었습니다.
    나무를  7번 찍었습니다.
    나무를  8번 찍었습니다.
    나무를  9번 찍었습니다.
    나무를  10번 찍었습니다.
    나무 넘어갑니다.
    ```

    - 위 예시에서 `while`문의 조건은 `treeHit < 10`이다. 따라서 `treeHit`이 10보다 작은 동안에는 `while`문을 수행한다.
    - `while`문 안에는 `treeHit`을 증가시키는 `treeHit++`가 존재하여 `treeHit`을 계속 1씩 증가 시킨다.
    - `treeHit`이 10이 되면 조건문 `treeHit < 10`은 거짓이 되고 `while`문을 종료하게 된다.

        > 💡`treeHit++`는 `treeHit += 1`과 같다.
        >


## 무한 루프

---

```java
while (true) {
    <수행할 문장1>;
    <수행할 문장2>;
    ...
}
```

- `while`문의 조건이 `true`이므로 이 조건은 항상 참이 된다. 따라서 조건이 참인 동안에는 항상 `while`문이 수행되므로 무한루프가 가능하다.

## Break

---

```java
public class WhileBreak {
    public static void main(String[] args) {
        int coffee = 10;
        int money = 300;

        while (money > 0) {
            System.out.println("돈을 받았으니 커피를 줍니다.");
            coffee--;
            System.out.println("남은 커피의 양: " + coffee);
            if (coffee == 0) {
                System.out.println("커피가 다 떨어졌습니다.");
                break;
            }
        }
    }
}
```

- `while`문을 강제로 빠져 나갈때 사용한다.
- 위 예시 에서는 `money`가 300으로 `while`문의 조건인 `while (money > 0)`조건에서 항상 참으로 무한 루프를 돌게 된다.
- 그리고 `while`문 수행시 `coffee-—`에 의해 `coffee`는 1개씩 줄어들게 되고 만약 `coffee`가 0이라면 `if (coffee == 0)`가 수행된다. 이때 `break`가 호출되며 `while`문을 빠져 나가게 된다.

## Countinue

---

```java
public class WhileContinue {

    public static void main(String[] args) {
        int a = 0;
        while (a < 10) {
            a++;
            if (a % 2 == 0) {
                continue;  // 짝수인 경우 조건문으로 돌아간다.
            }

            System.out.println(a);  // 홀수만 출력
        }
    }
}
```

- 만약 `while`문을 수행할때 어떤 조건을 검사하여 해당 조건에 맞지 않는 경우 `while`문을 빠져 나가는 대신 `while`문의 맨 처음 (조건 검사)로 돌아가고 싶을때 `continue`를 사용한다.
- 위 예시에서는 1 ~ 10의 수 중 홀수만 출력하는 예시이다.
- `a`가 10보다 작은 동안 `a`는 1씩 증가한다. 이때 `if (a & 2 == 0)`가 참인 경우 즉, `a`가 짝수인 경우에는 `continue`가 수행되고 `while`문의 처음으로 돌아가게 되어 `System.out.println(a)`는 수행되지 않기 때문에 홀수만 출력된다.
