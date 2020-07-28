---
title: TIL.04) Python Parameter
date: 2020-07-27 02:07:90
category: Python
thumbnail: './images/python.png' 
draft: false
---

![](./images/python.png)

# Parameter 우선 순위

>_우선순위는 위에서 부터 아래순_
1. Regular Positional Arguments
2. Default Arguments
3. Variable Length Positional Arguments
4. Non-Default Keyword-Only Arguments
5. Keyword-Only Arguments
6. Keyword-Only Arguments With Defaults
7. Variable Length Keyword Arguments

![](https://images.velog.io/images/yongineer1990/post/18cf1983-c229-4a84-a2ee-4051793b9ed9/image.png)

따라서 [TIL.02) Function of Python ](https://optimistic-jepsen-f9d493.netlify.app/TIL/til.02\)-function-of-python/)에서 ✅ Default Parameter의 정의 순서의 아래 코드는
```
def love_you(my_name = "정우성", your_name): 
    print(f"{my_name} loves {your_name}")

love_you("아이유") 
```
Default Arguments가 Regular Positional Arguments 보다 앞에 위치하여 에러를 발생하게 되었던 것이다. 그러면 이 에러를 수정하는 방법에는 무엇이 있을까? 
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
> **Example Code**
>```
def args_practice1 (default_args = "default_args"):
  print(default_args) Default Arguments
def args_practice2 (*, keyword_only_args = "keyword_only_args"): 
  print(keyword_only_args)
>
args_practice1("change default_args")
args_practice2 ()
args_practice2 (keyword_only_args = "change keyword_only_args")
args_practice2("change keyword_only_args") # Error
>```
> **Example Result**
>```
>change default_args
keyword_only_args
change keyword_only_args
Traceback (most recent call last):
  File "main.py", line 9, in <module>
    args_practice2("change keyword_only_args")
TypeError: args_practice2() takes 0 positional arguments but 1 was given
>```

# Positional arguments & Variable length arguments
>**Code**
```
def func_param_with_var_args(name, *args, age):
    print("name=",end=""), print(name)
    print("args=",end=""), print(args)
    print("age=",end=""), print(age)

func_param_with_var_args("wecode", "01012341234", "seoul", 20)
```
**Result**
```
TypeError: func_param_with_var_args() missing 1 required keyword-only argument: 'age' 
```
  
위 예제는 `Keyword-only arguments` 매개변수에 에 값이 전달 되지 않아 발생하였는데 먼저 `func_param_with_var_args` 함수의 매개변수 구성 요소를 위에서 정리한대로 정리해보면 다음과 같이 구성되어 있다.
  >`def func_param_with_var_args (Positional Arguments, Variable Length Arguments, Non-Default Keyword-Only Arguments`
  
호출시 전달한 인자의 구성요소를 살펴 보면 다음과 같다.
  >`func_param_with_var_args (Positional Arguments, Variable Length Arguments, Variable Length Arguments, Variable Length Arguments)`
  
  즉 `Non-Default Keyword-Only Arguments`를 매개변수로 사용하겠다고 했으나 정작 인자를 전달하지 않아 에러가 발생되었다. 이는 `Non-Default Keyword-Only Arguments` 부분을 지우고 실행해보면 알 수 있는데

>**Code**
```
def func_param_with_var_args(name, *args):
    print("name=",end=""), print(name)
    print("args=",end=""), print(args)

func_param_with_var_args("wecode", "01012341234", "seoul", 20)
```

> **Result**
```
name=wecode
args=('01012341234', 'seoul', 20)
```

  같은 인자를 전달했지만 잘 실행되는것을 알 수 있다.
  즉, 전달한 모든 인자가 `Variable Length Arguments`로 전달되었음을 알 수 있다.
  이를 해결하기 위해 `Keyword Arguments`의 규칙을 따라야 하는데 `Keyword = Value`형태로 인자를 전달해야 한다. 따라서 예제 코드를 수정하면 다음과 같이 수정이 가능하다.
  >**Code**
```
def func_param_with_var_args(name, *args, age):
    print("name=",end=""), print(name)
    print("args=",end=""), print(args)
    print("age=",end=""), print(age)
func_param_with_var_args("wecode", "01012341234", "seoul", age = 20)
```
> **Result**
```
name=wecode
args=('01012341234', 'seoul')
age=20
```

# Variable length keyword arguments
>**Code**
```
def func_param_with_kwargs(name, age, **kwargs, address=0):
    print("name=",end=""), print(name)
    print("age=",end=""), print(age)
    print(“kwargs=“,end=“”), print(kwargs)
    print(“address=“,end=“”), print(address)
>
func_param_with_kwargs("wecode", "20", mobile="01012341234", address="seoul")
```
>**Result**
  `SyntaxError: invalid syntax`

위 예제는 매개변수 순서 규칙을 지키지 않아 에러가 발생하였는데 위에서 정리한 매개변수 우선 순위 규칙을 보면 `Variable Keyword Arguments`보다 `Default Arguments`가 먼저 위치해야 한다. 따라서 `address=0`는 `**kwargs`보다 앞에 위치하여야 한다. 따라서 매개변수의 순서를 바꿔 주어야 한다.

>**Code**
  ```
def func_param_with_kwargs(name, age, address=0, **kwargs):
    print("name=",end=""), print(name)
    print("age=",end=""), print(age)
    print("kwargs=",end=""), print(kwargs)
    print("address=",end=""), print(address)
func_param_with_kwargs("wecode", "20", mobile="01012341234",address="seoul")
  ```
> **Result**
```
name=wecode
age=20
kwargs={'mobile': '01012341234'}
address=seoul
```
  
  또는
  
  >**Code**
  ```
def func_param_with_kwargs(name, age, address=0, **kwargs):
    print("name=",end=""), print(name)
    print("age=",end=""), print(age)
    print("kwargs=",end=""), print(kwargs)
    print("address=",end=""), print(address)

func_param_with_kwargs("wecode", "20", "seoul" mobile="01012341234") #인자의 위치를 바꿔 키워드를 사용하지 않고 인자 전달
  ```
>  **Result**
 ```
name=wecode
age=20
kwargs={'mobile': '01012341234'}
address=seoul
  ```
  
>**💡 왜 `address=0`는 Default Arguments 인가?**
>
>이것도 매개변수 우선 순위와 관련 있는데 `Variable Length Positional Arguments`를 매개변수로 사용한 이후에 사용하는 `Keyword = value` 형태의 매개변수는 `Keyword-Only Arguments with Default` 라고 이해하면 쉽다. 따라서 위 예제의 `address=0`는 `Default Arguments`가 된다. 확인하는 방법은 간단하다 
>
>**Code**
```
def func_param_with_kwargs(name, age, address=0, **kwargs):
    print("name=",end=""), print(name)
    print("age=",end=""), print(age)
    print("kwargs=",end=""), print(kwargs)
    print("address=",end=""), print(address)

func_param_with_kwargs("wecode", "20", mobile="01012341234")
```
>  **Result**
```
name=wecode
age=20
kwargs={'mobile': '01012341234'}
address=0
```
>
  실행 결과를 보면 `Default Arguments`와 같이 인자를 전달하지 않는 경우 기본값이 사용되는것을 확인 가능하다.

  # Positional Arguments & Variable Length Keyword Arguments
  
  >**Code**
  ```
  def mixed_params(name="wecode", *args, age, **kwargs, address):
    print("name=",end=""), print(name)
    print("args=",end=""), print(args)
    print("age=",end=""), print(age)
    print("kwargs=",end=""), print(kwargs)
    print("address=",end=""), print(address)
>
mixed_params(20, "wecode", "01012341234", "male" ,mobile="01012341234", address="seoul")
  ```
  >**Result**
  ```
  SyntaxError: invalid syntax
  ```

 위 코드는 인자의 전달 순서에 맞게 매개변수의 순서를 변경해줘야 할 필요가 있는데 문제가 되는 매개변수와 이유는 다음과 같다.
> **1️⃣ : `address`**
  `Non-Default Keyword-Only Arguments`이나 `Variable Length Keyword Arguments`보다 뒤에 위치하고 있어 수정이 필요하다.
  >
  **2️⃣ : `age`**
  `Regular Positional Arguments`이나 `Default Arguments` 보다 뒤에 위치하고 있어 수정이 필요하다.
  
  따라서 매개변수의 우선 순위에 따라 코드를 수정하면 다음과 같다.
  >**Code**
  ```
  def mixed_params(age, name="wecode", *args, address, **kwargs):
    print("name=",end=""), print(name)
    print("args=",end=""), print(args)
    print("age=",end=""), print(age)
    print("kwargs=",end=""), print(kwargs)
    print("address=",end=""), print(address)
>
mixed_params(20, "wecode", "01012341234", "male" ,mobile="01012341234", address="seoul")
  ```
  >**Result**
  ```
  name=wecode
args=('01012341234', 'male')
age=20
kwargs={'mobile': '01012341234'}
address=seoul
  ```
