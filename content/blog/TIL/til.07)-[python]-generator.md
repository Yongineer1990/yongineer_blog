---
title: TIL.07) [Python] Generator
date: 2020-07-27 03:45:58
category: Python
thumbnail: './images/python.png'
draft: false
---

![](./images/python.png)

# Generator Function

- Generator 함수는 값을 Return 하기도 하면서 동시에 산출 (Yield)한다.
- Generator는 Iterator를 생성해주는 함수
- Generator 함수는 Generator object를 반환한다.

```python
def generator_squares():
    for i in range(3):
        yield i ** 2

def return_squares():
    for i in range(3):
        return i ** 2

print("generator_squares=", end=""), print(generator_squares())

print("return_squares=", end=""), print(return_squares())
```
```
generator_squares=<generator object generator_squares at 0x7f217aed3c80>
return_squares=0
```
> 값을 yield 하는 함수와 return하는 함수를 둘다 출력했을때 Generator 함수는 generator object를 반환하며 일반적인 함수는 해당 함수의 처리 결과를 Return 하는것을 알 수 있다.

- Iterator와 다르게 `__iter__`를 호출 한 후에 `__next__`함수를 호출하지 않아도 바로 `__next__`를 호출 할 수 있다.

## **Generator 함수의 `__iter__`  `__next__` & `Sand`**

### **`__iter__`**

```python
print("dir gen =", end=""), print(dir(generator_squares()))
```
```
dir gen =['__class__', '__del__', '__delattr__', '__dir__', '__doc__',
'__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__',
'__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__name__',
'__ne__', '__new__', '__next__', '__qualname__', '__reduce__', '__reduce_ex__',
'__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
'close', 'gi_code', 'gi_frame', 'gi_running', 'gi_yieldfrom', 'send', 'throw']
```

Generator 함수를 `dir`로 호출해 함수 종류를 확인해 보면 `__iter__`와 `__next__`함수 둘다 포함되어 있는것을 확인 할 수 있다.

> 💡따라서 Generator 함수는 Iterator 함수와 다르게 `__iter__`를 호출 한 뒤 곧바로 `__next__`함수를 호출하는 것이 가능한 것이다.

### `__next__`

Iterator와 마찬가지로 `__next__`함수로 값을 꺼내 올수 있고 더이상 꺼낼수 있는 값이 없으면 `StopIteration`이 발생한다.

```python
gen = generator_squares()
print(gen.__next__())
print(gen.__next__())
print(gen.__next__())
print(gen.__next__())

 #출력결과
0
1
4
Traceback (most recent call last):
  File "generator.py", line 14, in <module>
    print(gen.__next__())
StopIteration
```

### `Send`

Generator 함수는 실행 중에 `send` 함수를 통해 값을 전달 할 수 있다.

```python
def generator_send():
    received_value = 0

    while True:

        received_value = yield
        print("received_value = ",end=""), print(received_value)
        yield received_value * 2

gen = generator_send()
next(gen)
print(gen.send(2))

next(gen)
print(gen.send(3))

# 출력결과
received_value = 2
4
received_value = 3
6
```

Generator 함수는 `yield`를 이용하여 Generator 함수 실행 중 값을 전달 할 수 있고 이를 응용 main 실행 루프에서 연산 결과에 따라 호출도 제어 할 수 있다.

# Generator Expression

- Lazy Evaluation을 위해 사용될 수 있다.
    - 💡Lazy Evaluation : 실행을 지연 시키는 것을 의미

> **Generator Expression 문법**
>
> - List comprehension 문법과 비슷
> - `[]` 가 아닌 `()`를 사용하여 구현한다.
>
```python
L = [ 1,2,3]

def generate_square_from_list():
    result = ( x*x for x in L ) # Generator Expression
    print(result)
    return result

def print_iter(iter):
    for element in iter:
        print(element)

print_iter( generate_square_from_list() )
```

```python
<generator object generate_square_from_list.<locals>.<genexpr> at 0x7f2c1de20c80>
1
4
9
```

# Lazy Evaluation

## Lazy Evaluation은 무엇일까?

만약 내가 아이스크림 장사를 하는데 오늘 **손님이 100명 들어올 것을 예상하여 아이스크림을 100개 준비**했다. 그런데 오늘 하필 손님이 5명밖에 안오는 바람에 95개의 아이스크림이 악성 재고가 생겼다. 내가 만약 **손님이 오는 데로 아이스크림을 준비 했더라면** 95개의 재고가 발생하지 않을 수 있지 않을까?  Eager evaluation과 Lazy evaluation은 바로 이런 차이이다.

- **Eager evaluation** : 손님이 100명 들어올 것을 예상하고 아이스크림을 100개 준비 하는 것.
- **Lazy evaluation** : 손님이 오는 데로 아이스크림을 준비 하는 것.

Python에서 Eager evaluation는 List Comprehension으로 구현하며 Lazy evaluation은 Generator Expression으로 구현한다.

## Lazy Evaluation의 장점

### Memory를 효율적으로 관리 할 수 있다.

- **List Comprehension**
    - 모든 데이터를 `List`에 저장해야 하므로 `List`의 크기가 커질 수록 Memory에 적재되는 데이터양이 늘어 나게 된다.
- **Generator Expression**
    - `__next__`함수를 통해 필요 할때 마다 데이터를 Memory에 적재하여 필요한 만큼만 Memory자원을 사용 가능하게 한다.

> **💡List Comprehension VS Generator Expression**
>
```python
import sys
sys.getsizeof([i for i in range(100) if i % 2]) # List Comprehension
-> 4264
sys.getsizeof((i for i in range(100) if i % 2)) # Generator Expression
-> 112
```
>
List Comprehension과 Generator Expression으로 각각 생성 했을 때 메모리 점유 상태를 보면 알 수 있듯 Generator Expression이 List Comprehension 보다 적은량의 메모리를 점유하고 있음을 확인 할 수 있다.

### 불필요한 연산을 피할 수 있다.

> **Code**
>
```python
import time

L = [1,2,3]

def print_iter(iter):
    for element in iter:
        print(element)

def lazy_return(num):
    print("sleep 1s")
    time.sleep(1)
    return num

print("comprehension_list=")
comprehension_list = [ lazy_return(i) for i in L ]
print_iter(comprehension_list)

print("generator_exp=")
generator_exp = ( lazy_return(i) for i in L )
print_iter(generator_exp)
```
>
> **Result**
>
```python
comprehension_list=
sleep 1s
sleep 1s
sleep 1s
1
2
3
generator_exp=
sleep 1s
1
sleep 1s
2
sleep 1s
3
```

**List Comprehension**

- **모든 연산이 한번에 수행**됨을 확인 할 수 있다.
- 만약 수행해야 하는 연산의 시간이 매우 길거나 List의 크기가 매우 클 경우 수행하는 **연산이 과부하**가 걸릴수도 있을 것이다.

**Generator Expression**

- **필요할때마다 연산이 수행**됨을 확인 할 수 있다.
- 따라서 **연산이 필요할때만 실행**되며 (수행 시간이 긴 연산을 필요한 순간 까지 늦출 수 있다.)
- 불필요한 값들이 계산되는 시간을 기다릴 필요가 없다.

> **💡Eager evaluation (List comprehension)의 연산 방법**
![](https://images.velog.io/images/yongineer1990/post/86c9d19c-c927-410f-99ef-0b95336eac1f/9905FD465C4A993D20.gif)

> **💡Lazy evaluation (Generator expression)의 연산 방법**
![](https://images.velog.io/images/yongineer1990/post/173eee1a-07cf-455f-8854-d2c5b174a4a1/99CC3B505C4A994A1C.gif)
