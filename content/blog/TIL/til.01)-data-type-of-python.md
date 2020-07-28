---
title: TIL.01) Data type of Python
date: 2020-07-27 01:07:38
category: Python 
thumbnail: './images/python.png'
draft: false
---

![](./images/python.png)

# Numeric Types
## 1. int
- 정수
- 메모리가 허용 가능한 선에서 무한대의 정수를 사용 가능
### Example
> **Code**
```
x = 1
y = 35656222554887711
z = -3255522
>
print(f"x : {x} \ndata type : {type(x)}")
print(f"y : {y} \ndata type : {type(y)}")
print(f"z : {z} \ndata type : {type(z)}")
```
> **Result**
```
x : 1 
data type : <class 'int'>
y : 35656222554887711 
data type : <class 'int'>
z : -3255522 
data type : <class 'int'>
```
_type(arg) : 인자의 Data type을 반환_

## 2. float
- 실수
- 부동 소수형
- 소수점 15자리까지 표현 가능
- 지수표기법 사용 가능
### Example
> **Code**
```
pi = 3.141592653589793238462643383279502884197169399375105820974944592307816406286
x = 35e3
y = 12E4
z = -87.7e100
>
print(f"pi : {pi} \nData type : {type(pi)}")
print(f"x : {x} \nData type : {type(x)}")
print(f"y : {y} \nData type : {ype(y)}")
print(f"z : {z} \nData type : {type(z)}")
```
> **Result**
```
pi : 3.141592653589793 
Data type : <class 'float'>
x : 35000.0 
Data type : <class 'float'>
y : 120000.0 
Data type : <class 'float'>
z : -8.77e+101 
Data type : <class 'float'>
```
_데이터의 크기가 소수점 15번째 자리 이상일때 반올림하고 나머지는 표현하지 않음_

## 3. complex
- 복소수 : 실수 + 허수
- 허수는 i가 아닌 j로 표현
### Example
> **Code**
```
x = 3+5j
y = 5j
z = -5j
>
print(f"x : {x} \nData type : {type(x)}")
print(f"y : {y} \nData type : {type(y)}")
print(f"z : {z} \nData type : {type(z)}")
```
> **Result**
```
x : (3+5j) 
Data type : <class 'complex'>
y : 5j 
Data type : <class 'complex'>
z : (-0-5j) 
Data type : <class 'complex'>
```
~~솔직히 어따 쓰는지 모르겠다~~

# Text Type
## 1. String
- 문자열을 가지런히 나열한 집합
- quote (',")로 감싸서 표현
- 문장을 나타낼때는 3개의 quote를 사용
- 리스트의 형태를 취함
### Example
> **Code**
```
a = "Hello, World!"
b = a[0:5]
>
print (f"a : {a} \nData type : {type(a)}")
print (f"b : {b} \nData type : {type(b)}")
```
> **Result**
```
a : Hello, World! 
Data type : <class 'str'>
b : Hello 
Data type : <class 'str'>
```

# Sequence Types
## 1. List
- 값의 수정 가능 (가변형)
- 값의 중복 가능
- 길이가 가변적
- 요소의 중복 가능
### Example
> **Code**
```
list1 = [10, 3.14, "string", [20, 30], (40, 50), {"now_play" : "Lonely Night"}]
>
print (f"list1 : {list1} \nData type : {type(list1)}")
print (len(list1)) #리스트의 길이
print (list1[0]) #리스트의 첫번째 데이터는 0
print (list1[3]) # 리스트 안에 리스트
print (list1[4]) # 리스트 안에 튜플
print (list1[5]) # 리스트 안에 딕셔너리
```
> **Result**
```
list1 : [10, 3.14, 'string', [20, 30], (40, 50), {'now_play': 'Lonery Night'}] 
Data type : <class 'list'>
6
10
[20, 30]
(40, 50)
{'now_play': 'Lonely Night'}
```

## 2. Tuple
- 값의 수정 불가 (불변형)
- 값의 중복 가능
- 길이가 가변적
- 요소의 중복 가능
### Example 01
> **Code**
```
tuple1 = (3, 3.14, "String", [10, 20], (30, 40), {"now_play" : "Stuck with U"})
>
print (f"tuple1 : {tuple1} \nData type : {type(tuple1)}")
print (len(tuple1)) #튜플의 길이
print (tuple1[0]) #튜플의 첫번째 데이터는 0
print (tuple1[3]) # 튜플 안에 리스트
print (tuple1[4]) # 튜플 안에 튜플
print (tuple1[5]) # 튜플 안에 딕셔너리
```
> **Result**
```
tuple1 : (3, 3.14, 'String', [10, 20], (30, 40), {'now_play': 'Stuck with U'}) 
Data type : <class 'tuple'>
6
3
[10, 20]
(30, 40)
{'now_play': 'Stuck with U'}
```

### Example 02
> **Code**
```
tuple1 = (3, 3.14, "String", [10, 20], (30, 40), {"now_play" : "Stuck with U"})
>
tuple1[0] = 10
print(tuple1)
```
> **Result**
```
tuple1[0] = 10
TypeError: 'tuple' object does not support item assignment
```
*튜플 내 값은 변경할 수 없음*

# Mapping Type
## 1. dict
- {key : value} 형태로 구성
- 가변형으로 데이터를 변경 가능
- key는 숫자형, 문자열, 튜플 사용 가능

### Example 01
> **Code**
```
dict1 = {"name" : "Yong-Jin", "age" : 31}
dict1["job"] = "Developer" # 데이터 세트 추가
dict1["age"] = 21 # value 변경
>
print (f"dict1 : {dict1} \nData type : {type(dict1)}")
```
> **Result**
```
dict1 : {'name': 'Yong-Jin', 'age': 23, 'job': 'Developer'} 
Data type : <class 'dict'>
```
