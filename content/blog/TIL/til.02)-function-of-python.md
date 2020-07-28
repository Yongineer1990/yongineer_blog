---
title: TIL.02) Function of Python
date: 2020-07-27 02:07:58
category: Python
thumbnail: './images/python.png' 
draft: false
---

![](./images/python.png)

# 1. Creating and Calling
파이썬에서 함수를 생성하고 호출할때는 다음과 같은 형식을 취한다.
### Example
**Code**
```
def hello_world():
	print("hello world!")

hello_world()
```

**Result**
```
hello world!
```

이때 def는 예약어이며 함수의 이름은 사용자가 임의로 지정가능하다.

# 2. Arguments
## Arguments and Parameter
- 인자(Arguments)는 함수를 호출할때 전달하는 값을 의미
- 매개변수(Parameter)는 함수에 입력으로 전달된 값을 받는 변수를 의미

### Example

**Code**
```
def add (a,b): # Parameter
  return a+b

a = add (3,5) # Arguments
print(a)
```
**result**
```
8
```

## Number of Argumenst
- 함수는 지정한 매개변수의 개수 만큼 인자를 받아야 한다.
- 만약 2개의 매개변수를 생성한 후 1개의 인자를 받게되거나 3개의 인자를 받게된다면 에러를 호출한다.

### Example

**Code**
```
def minus (a,b): # 2개의 parameter 지정
  return a-b

a = minus(1,5) # 2개의 Arguments가 전달되어야 함
print(a)
```
**Result**
```
5
```

### Error case

**Code**
```
def greetings(name1, name2):
  print(f"hello {name1}\n안녕하세요 {name2}")

greetings("yongineer")
```

**Result**
```
TypeError: greetings() missing 1 required positional argument: 'name2'
```
*name2 매개변수에 인자를 전달하지 않아 에러 발생*

## Keyword Arguments
- 함수 생성시 정의한 매개변수의 순서대로 전달하지 않고 매개변수 키워드에 직접 인자를 전달하는 방법
- key = value 형태를 취한다.

### Example

**Code**
```
def profile (age, name, address):
  print (f"이름 : {name}")
  print (f"나이 : {age}")
  print (f"주소 : {address}")

profile(name = "yongineer", age = 31, address = "korea")
```

**Result**
```
이름 : yongineer
나이 : 31
주소 : korea
```

## *args
- 인자를 무한대로 전달해야 하거나 전달할 인자의 개수가 정해져 있지 않을때 사용한다.
- 키워드는 일반적으로 arguments의 약어 args를 사용하나 사용자 임의로 지정 가능하다.

### Example
전달된 인자 모두를 더하는 함수

**Code**
```
def all_add (*args):
  result = 0
  for i in args:
    result = result + i
  return result

a = all_add(1,2,3)
print(a)
```

**Result**
```
6
```

## **kwargs
- 함수의 필요 매개변수가 무한정이거나 정해져 있지 않을때 사용한다.
- 키워드는 일반적으로 Keyword Arguments의 약어 kwargs를 사용하나 사용자 임의로 지정 가능하다.

### Example

**Code**
```
def phone_number(**kwargs):
  print ("010-" + kwargs["middle_number"] + "-" 
  	+ kwargs["last_number"])

phone_number(middle_number = "1234", last_number = "5678")
```

**Result**
```
010-1234-5678
```

## Default Parameter
- 함수의 기본값을 지정 한다.
- 매개변수가 있는 함수를 인자를 전달하지 않아도 호출가능하며 이때 기본값이 인자로 전달된다.

### Example
**Code**
```
def my_country (country = "Korea"):
  print(f"I am from {country}")

my_country("Spian")
my_country() # 매개변수가 존재하는 함수에 인자를 전달하지 않음
```
**Example**
```
I am from Spian
I am from Korea
```

>✅ **Default Parameter의 정의 순서**

Default Parameter는 Non-Default Parameter보다 먼저 위치하지 않아야 하는데 그 이유는 다음과 같다
![](https://images.velog.io/images/yongineer1990/post/5959a363-7982-45a9-b697-d960a18aa0e8/image.png)
만약 위 소스 코드에서 default parameter의 위치를 바꿔 아래와 같이 작성하고 함수를 호출하면 다음과 같이 동작하게 된다.

```
def love_you(my_name = "정우성", your_name): 
    print(f"{my_name} loves {your_name}")

love_you("아이유") 
```
- ``` love_you("아이유")``` 는 ```love_you``` 함수의 `your_name` 매개변수에 전달할 목적으로 사용되었으나 실제로 어디에 전달되는지 모르는 상황이 발생한다.
- 따라서 python에서는 이런 상황을 미연에 방지하고자 default값이 없는 매개변수가 항상 가장 먼저 위치하도록 문법을 정의하고 있으며 만약 이를 어기면 다음과 같은 에러메시지를 반환한다.
```SyntaxError: non-default argument follows default argument```

## Parameter 우선 순위

>_우선순위는 위에서 부터 아래순_
1. Regular Positional Arguments
2. Default Arguments
3. Variable Length Positional Arguments
4. Non-Default Keyword-Only Arguments
5. Keyword-Only Arguments
6. Keyword-Only Arguments With Defaults
7. Variable Length Keyword Arguments

![](https://images.velog.io/images/yongineer1990/post/18cf1983-c229-4a84-a2ee-4051793b9ed9/image.png)

따라서 위 ✅Default Parameter의 정의 순서에서의 
```
def love_you(my_name = "정우성", your_name): 
    print(f"{my_name} loves {your_name}")

love_you("아이유") 
```
위 코드는 Default Arguments가 Regular Positional Arguments 보다 앞에 위치하여 에러를 발생하게 되었던 것이다. 그러면 위 코드를 수정하는 방법에는 무엇이 있을까? 
1️⃣ Parameter 우선 순위에 맞춰서 Default Arguments와 Regular Positional Arguments의 순서를 바꿔주는 방법이다.
```
def love_you(your_name, my_name = "정우성"): 
    print(f"{my_name} loves {your_name}")

love_you("아이유") 
```
2️⃣ 완벽하지 않지만 에러는 나지 않는 두번째 방법은 Regular Positional Arguments를 Variable Length Positional Arguments로 수정하는 방법이다.
```
def love_you(my_name = "정우성", *your_name): 
    print(f"{my_name} loves {your_name}")

love_you("정우성","아이유") 
```
다만 아직 Default Arguments의 기본 값을 다시 전달해야 하는 문제와
```
def love_you(my_name = "정우성", *your_name): 
    print(f"{my_name} loves {your_name}")

love_you("정우성","아이유","IU") 
```
위와 같이 작성시 `정우성 loves ('아이유', 'IU')`과 같이 출력되지만 이쁘지 않다는점이 문제다. 2️⃣ 방법에 대해서는 앞으로 공부를 더 하면서 해결 방법을 찾아봐야 할거 같다.

> ### Default Arguments & Keyword-Only Arguments With Default
>
- Default Arguments에서 `= value`의 의미는 전달된 인자가 없을때 `value`의 값을 기본값으로 설정한다는 의미
- Keyword-Only Arguments With Default 에서의 `= value`의 의미는 해당 `keyword`를 호출하지 않았을때 `value`를 기본값으로 사용한다는 의미
- 따라서 Keyword-Only Arguments With Default에서 키워드를 지정하지 않고 인자 전달시 해당 인자는 Regular Positional Arguments로 인식되어 에러가 발생한다.
>

**Example Code**
```
def args_practice1 (default_args = "default_args"):
  print(default_args) Default Arguments
def args_practice2 (*, keyword_only_args = "keyword_only_args"): 
  print(keyword_only_args)

args_practice1("change default_args")
args_practice2 ()
args_practice2 (keyword_only_args = "change keyword_only_args")
args_practice2("change keyword_only_args") # Error
```
**Example Result**

```
change default_args
keyword_only_args
change keyword_only_args
Traceback (most recent call last):
  File "main.py", line 9, in <module>
    args_practice2("change keyword_only_args")
TypeError: args_practice2() takes 0 positional arguments but 1 was given
```
