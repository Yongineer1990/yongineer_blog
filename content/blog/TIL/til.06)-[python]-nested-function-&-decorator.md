---
title: TIL.06) [Python] Nested Function & Decorator
date: 2020-07-27 03:07:61
category: Python
thumbnail: './images/python.png'
draft: false
---

![](./images/python.png)

# Nested Function

- 중첩 함수
- 함수 안에 함수를 선언함으로써 사용 가능

```python
def parent_function():
    def child_function():
        print("this is a child function")

    child_function()

parent_function()
```
```
"this is a child function"
```

# Why use nested function?

1. 가독성 향상을 위해
2. Closure

## 가독성

- 함수 안의 코드 중 반복되는 코드가 있다면 중첩 함수로 선언하면 부모 함수의 코드를 효과적으로 관리하고 가독성을 높일 수 있다.

```python
def print_all_elements(list_of_things):
    # 중첩함수 선언
    def print_each_element(things):
        for thing in things:
            print(thing)

    if len(list_of_things) > 0:
        print_each_element(list_of_things)
    else:
        print("There is nothing!")
```

## Closure

- 중첩 함수가 부모 함수의 변수나 정보를 가두어 사용하는 것
- 부모 함수는 리턴값으로 중첩 함수를 리턴한다.
- 부모 함수에서 리턴 했으므로 부모 함수의 변수는 직접적인 접근이 불가능 하지만 부모 함수가 리턴한 중첩 함수를 통해서 사용될 수 있다.

> **💡Closure는 언제 사용 할까?**
- 어떠한 정보를 기반으로 연산을 실행하고 싶지만 기반이 되는 정보는 접근을 제한하여 노출이 되거나 수정이 되지 못하게 하고 싶을 때 사용
- 주로 Factory 패턴을 구현할 때 사용
    > _💡Factory?_
    - 뭔가를 생성해내는 패턴으로 주로 함수나 오브젝트를 생성해내는데 사용
- 설정값이 노출되지 않아서 수정이 불가능 하게 하면서 해당 설정값을 기반으로하는 연산을 할 수 있는 함수를 만들 때

### Closure의 사용 방법

2의 승을 구하는 함수를 구현한다면 다음과 같은 코드로 작성이 가능할 것이다.

```python
def calculate_power_of_two(power):
    return 2 ** power

calculate_power_of_two(7)
-> 128
calculate_power_of_two(10)
-> 1024
```

하지만 위 함수는 2의 승밖에 구할 수 없다. 따라서 만일, 특정 숫자의 승을 구하는 함수가 필요 하지만 2가 아니라 설정되는 수의 승을 구하는 함수는 Closure를 사용하여 다음과 같이 구현할 수 있다.

```python
def generate_power(base_number):
    def nth_power(power):
        return base_number ** power

    return nth_power

calculate_power_of_two = generate_power(2)
calculate_power_of_two(7)
-> 128
calculate_power_of_two(10)
-> 1024

calculate_power_of_seven = generate_power(7)
calculate_power_of_seven(3)
-> 343
calculate_power_of_seven(5)
-> 16907
```

![](https://images.velog.io/images/yongineer1990/post/f87ae3e9-5e66-4a34-8384-41557dc65a90/image.png)

# Decorator

- 함수를 장식하는 함수
- Decorator가 있는 함수의 실행 순서
    1. Decorator
    2. Decorator가 존재하는 함수

        💡Decorator가 존재하는 함수는 무조건 Decorator가 먼저 선 호출 된다.

- Decorator는 Chain of Function, 즉 여러개의 함수가 연속적으로 호출되어야 하므로 반드시 중첩 함수만 Decorator로 사용이 가능하다.
    - 💡만약 중첩함수가 아니라면 함수를 리턴하지 않고 함수를 종료하므로 Decorator로 사용이 불가능한것이다.

> **Decorator 사용 예시**
>만약 유료회원에 한해서 주식정보를 리턴하는 함수가 있다고 가정하자. 일단 먼저 회원의 유료회원 여부를 판단하는   기능과 주식정보를 알려주는 기능으로 나눠서 구현이 가능할 것이다.
```python
def jackpot_stock_information():
    return "계시가 내려졌습니다. 삼성전자를 사세요!"
```

```python
def is_paid_user():
    return True #편의를 위해 모든 회원의 유료회원 여부를 True로 Return
```

>그리고 난 다음 회원이 유료 회원이라면 주식정보를 담은 함수를 호출시키면 구현이 가능 할 것이다.

```python
if is_paid_user():
    jackpot_stock_information()
```
>
하지만 주식 정보를 담은 `jackpot_stock_information()` 함수가 이곳뿐만 아니라 여러 곳에서 자주 사용한다면 항상 `is_paid_user()` 함수를 호출하여 유료회원 여부를 확인해 줘야 한다. 때문에 이 연결고리를 잊어먹고 `is_paid_user()` 를 통해 확인하지 않고 고급 주식 정보인 `jackpot_stock_information()` 함수를 호출할 확률이 있다. 따라서 `jackpot_stock_information()`함수를 구현 할때 Decorator로 `is_paid_user()`를 사용하여 매번 `jackpot_stock_information()` 호출 시 강제로 `is_paid_user()`가 호출되게 함수를 만들면 해결 할 수 있다.
>
하지만 현재 작성된 `is_paid_user()` 함수는 중첩 함수가 아니므로 해당 함수를 중첩 함수로 만들어 줘야 한다.
>
```python
def is_paid_user(func):
    user_paid = True # 간단화 하기 위해 무조건 True

    def wrapper(*args, **kwargs):
        if user_paid:
            func() #func() = jackpot_stock_information()
        else:
            return

    return wrapper
```
>
이렇게 만든 Decorator를 `jackpot_stock_information()` 함수에 달아준다.
>
```python
@is_paid_user #Decorator
def jackpot_stock_information():
    return "계시가 내려졌습니다. 삼성전자를 사세요!"
```

![](https://images.velog.io/images/yongineer1990/post/5cc53d22-55cb-4302-afc6-63df9d6a6536/image.png)

## 인자를 받는 Decorator

만약 Decorator가 인자를 받는다면 Decorator도 결국 함수이므로 해당 인자를 처리해줄 필요가 있다. 

따라서 만약 다음과 같이 Decorator에서 인자를 전달한다면 
>
```python
@name_decorator("정우성")
def greeting():
  return "Hello, "

greeting()
```

1. 먼저 Decorator에서 전달한 인자를 받아 처리해줄 함수
2. Decorator를 사용한 함수의 실제 Decorator
3. Decorator의 내장 함수

로 구현해야 한다. 따라서 Decorator는 다음과 같이 구현이 가능하다.
>
```python
def name_decorator(name): #@name_decorator에서 전달받은 인자를 처리해줄 함수
  def decorator(func): #실제 Decorator
    def wrapper(*args, **kwargs): #Decorator 내장 함수
      result = func() + name # = greeting() + name_decorator(name)
      return result
    return wrapper
  return decorator
```
