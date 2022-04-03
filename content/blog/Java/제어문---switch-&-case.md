---
title: 제어문 - Switch & Case
date: 2022-04-04 00:04:92
category: Java
thumbnail: { thumbnailSrc }
draft: false
---

![](https://www.hanumoka.net/images/20180412-java-equals-hashCode_1.png)


### Sample

```java
/**
 * SwitchCase
 */
public class SwitchCase {

    public static void main(String[] args) {
        int month = 8;
        String monthString = "";
        switch (month) {
            case 1:  monthString = "January";
                     break;
            case 2:  monthString = "February";
                     break;
            case 3:  monthString = "March";
                     break;
            case 4:  monthString = "April";
                     break;
            case 5:  monthString = "May";
                     break;
            case 6:  monthString = "June";
                     break;
            case 7:  monthString = "July";
                     break;
            case 8:  monthString = "August";
                     break;
            case 9:  monthString = "September";
                     break;
            case 10: monthString = "October";
                     break;
            case 11: monthString = "November";
                     break;
            case 12: monthString = "December";
                     break;
            default: monthString = "Invalid month";
                     break;
        }
        System.out.println(monthString);
    }
}
```

- `switch`에 입력변수와 값이 일치하는 `case`의 입력값이 있다면 해당 `case`문에 속한 명령이 실행된다.
- 각 `case`문 마다 `break`라는 명령이 있는데 이는 `case`문을 실행 한 뒤 `switch`문을 빠져나가기 위함이다.
- 따라서 만약 `break`가 없다면 다음 `case`문이 실행된다.
- `default`는 `case`의 입력값에 해당하는 조건이 없다면 실행되는 명령이다.
- `switch/case`문은 `if else`로 구조 변경이 가능하지만 `if else` 구조로 작성된 모든 코드를 `switch`로 변경할 수 는 없다.
