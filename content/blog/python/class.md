---
title: TIL.08) [Python] Class
date: 2020-07-28 19:11:98
category: Python
thumbnail: ../TIL/images/python.png
draft: false
---

![](../TIL/images/python.png)

# Class

- 부류
- 동일한 범주에 속하는 대상들을 일정한 기준에 따라 나누어 놓은 것

![](https://yongineer.duckdns.org/python/01.png)

- Car : Class
- Lexus, BMW, Benz, Hyundai : Object

# Class 정의 하기

- class이름은 각 단어의 앞글자를 대문자로 사용
- 만약 한 단어 이상으로 이루어져 있다면 밑줄(_) 없이 모든 단어를 붙이되 각 단어의 앞 글자는 대문자를 사용
    - ScotchWishkey
    - VeryVeryReallySuperHyperAbsoluteLongClassName

```python
class Car:
    pass
```

- class가 정의 되면 class로 부터 실체화 (Instantiate)가 가능하다.
- 실체화는 함수를 호출하듯 클래스를 호출하면 된다.

```python
bmw = Car()
hyundai = Car()
```

# Attribute of Class

- 클래스에 정의되는 공통 요소들
- 예를 들어, 자동차는 다음과 같은 공통 요소를 갖을 수 있다.
    1. Maker (현대, BMW 등)
    2. 모델명 (BMW350i, Genesis GV80 등)
    3. 마력
        - 이 외에도 토크, 엔진, 중량, 트렁크 크기 등 많지만 예제의 간단함을 위해 위 3가지 요소만 고려
- class에 속성을 정의하기 위해서는 `__init__`함수를 통해 정의 해줘야 한다.

```python
class Car:
    def __init__(self, maker, model, horse_power):
        self.maker       = maker
        self.model       = model
        self.horse_power = horse_power
```

- class안에서 정의해주는 함수는 `function`이 아닌 `method` 라고 한다.

## \__init\__

- special methods라고 한다.
- class가 실체화 될때 사용되는 함수

```python
hyundai = Car("현대", "제네시스", 500)
```

- 위 코드는 이미 `__init__` method가 호출 되었는데 `__init__` 이라고 정확히 명시하지 않았지만 class가 실체화 될 때 자동으로 `__init__` method가 호출된다.

## Self

- Class에서 object를 가리킨다.
- `Self` Parameter는 class가 실체화된 object를 넘겨주어야 하며 Python이 자동으로 넘겨준다.
- `Self` Parameter는 항상 `__init__` method의 맨 처음 Parameter로 정의 되어야 한다.

    ```python
        def __init__(self, maker, model, horse_power):
    ```

> **💡Class object가 `__init__` method에게 인자를 전달하는 과정**
 >
 >![](https://yongineer.duckdns.org/python/02.png)
 >
 >```python
 >class Car:
 >    def __init__(self, maker, model, horse_power):
 >        self.maker       = maker
 >        self.model       = model
 >        self.horse_power = horse_power
 >```
 >
 >즉, 위 예시 코드는 매개변수로 넘겨진 `maker`, `model`, `horse_power`값을 동일한 이름으로 `self`에 저장하는 것을 볼 수 있다.

# Class Method

클래스에서 `__init__`메서드 말고 다른 메서드를 추가 할 수 있다. 메서드와 속성의 차이는 명사와 동사의 차이와 같다. 속성은 해당 객체의 이름 등의 정해진 성질인 반면 메서드는 move, eat과 같이 객체가 행할 수 있는 어떠한 액션과 같다. 따라서 honk메서드를 추가 한다면 다음과 같다.

```python
class Car:
    def __init__(self, maker, model, horse_power):
        self.maker       = maker
        self.model       = model
        self.horse_power = horse_power

    def honk(self):
        return "빠라바라빠라밤"
```

honk메서드를 보면 `self`를 상속받는 걸 볼 수 있다. 즉, 모든 메서드에는 `self`라는 매개변수가 첫번째 매개변수로 들어가야 한다.

honk함수는 다음 처럼 사용 가능 하다.

```python
hyundai = Car("현대", "제네시스", 500)
hyundai.honk()
> "빠라바라빠라밤"
```

이렇듯 객체에서 메서드를 사용할 때는 `.` 을 사용하여 객체를 호출한다. 즉, `<객체>.<메서드>`형식으로 호출하고 이를 dot notation이라고 한다.

그런데 이제 모든 차는 경적소리에서 제조사의 회사명이 들어간다고 가정해보자. 예를 들어 현대차의 경적소리는 '현대 빠라바라빠라밤'이라고 울려야 한다.

그렇다면 `honk`메서드에서 해당 객체의 회사 정보를 가져오려면 어떻게 해야할까? 다행히도 `__init__`메서드에서 `self`객체에 해당 정보를 저장해 놓았기 때문에 다음과 같이 가져올 수 있다.

```python
class Car:
    def __init__(self, maker, model, horse_power):
        self.maker       = maker
        self.model       = model
        self.horse_power = horse_power

    def honk(self):
        return f"{self.maker} 빠라바라빠라밤"
```

# Class 왜 쓸까?

- 클래스를 사용하면 코드의 구조를 더 효과적으로 구현할 수 있기 때문이다.
- 코드를 클래스 위주로 작성하는 것을 object oriented programming (객체 지향 프로그래밍) 이라고 한다.
