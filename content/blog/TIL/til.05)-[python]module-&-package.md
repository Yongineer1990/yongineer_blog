---
title: TIL.05) [Python]Module & Package
date: 2020-07-27 03:07:44
category: Python
thumbnail: './images/python.png'
draft: false
---

![](./images/python.png)

# Module

- 함수, 변수, 클래스를 모아놓은 파일

> 💡 왜 Module을 사용할까?
> - 다른 파일에서 **재사용**이 가능하게 하기 위해
> - 전체 코드가 한 파일에 넣기에는 너무 클 때 **여러 파일로 나누어서 정리 및 관리하기 위해**

# Module 만들기

- Python 파일을 만든 후
- 재사용 하고 싶은 함수, 클래스, 변수등을 작성하면 끝.

```python
# my_module.py
my_module_var = 7

def my_module_func():
    return "Hello!"

class MyModuleClass:
    pass
```

# Module 사용하기

- `module`을 사용할 파일에서
- `import` 키워드를 사용하여 불러온 후
- `<module 파일 이름>.<module 안에 작성한 variable, function, class의 이름>` 으로 사용

> **Exemple**
```python
my_module.my_module_func()
```
> **Result**
```python
Hello!
```

- 💡만약 해당 모듈이름과 연결하여 호출하지 않으면 Python은 해당 파일에서 사용할 변수/함수/클래스를 찾을것이고 작성하지 않았다면 에러가 날것이다.
![](https://images.velog.io/images/yongineer1990/post/47bd45c8-57ef-4417-afd1-c604f9a8533c/image.png)

# From ~ Import

- from import 키워드로 모듈을 불러올 수 있다.
- `from <Module 이름> import <변수/함수/클래스 1>, <변수/함수/클래스 2>...<변수/함수/클래스 n>`
- from import 키워드로 모듈 호출 시 모듈의 이름없이 바로 원하는 변수, 함수, 클래스를 사용할 수 있다.

```python
from my_module import my_module_func, my_module_var

print(my_module_var)
my_module_func()
```

- 모듈의 모든 요소를 import 할 때는 \* 을 사용한다 : `from <Module 이름> import *`
- 하지만 local scope를 갖고 있는 다른 변수, 함수, 클래스와 충돌이 발생할 수 있고 충돌이 발생했을 때 알기 어렵기 때문에 권장하지 않는 방법

# Import As

- import as 키워드로 모듈에서 사용할 요소들에 이름을 부여 할 수 있다.
- `from <Module 이름> import <변수/함수/클래스> as <사용자 지정 이름>`
- Module에도 이름을 부여할 수 있다.
- `import <Module 이름> as <사용자 지정 이름>`

> **💡왜 module에 이름을 붙일까?**
> 1. 서로 다른 모듈에서 동일한 이름의 변수, 함수, 클래스가 있다면 충돌할 수 있고
> 2. 원하는 요소의 이름이 너무 길수도 있기 때문에

# Package

- 모듈의 집합
- Python 파일들로 이루어져 있는 Directory가 하나의 Package가 된다.
![](https://images.velog.io/images/yongineer1990/post/bb729e22-a86d-41e6-a1f1-529e17b2710c/image.png)

# Import Package

- Module을 Import 할 때와 같은 방법
- 다만, 점(.)으로 원하는 모듈을 지정하여 Import 해야 한다.

```python
import pkg.mod1
from pkg.mod2 import func2

pkg.mod1.func2()
func2()
```

# Package Initialization

- `__init__.py` 파일로 Package의 초기 설정을 해줄 수 있다.
- Package가 Import 될 때 `__init__.py`의 코드들이 자동으로 실행 된다.

## **왜 `__init__.py` 파일을 사용할까?**

- **Import 할때 경로의 총 길이를 줄여줄 수 있다.**

```python
# __init__.py
from .mod1 import func2
```

```python
# main.py
from pkg import func2
# 이미 pkg.mod1에 있는 함수지만 __init__.py에서 이미 경로를 지정했기때문에 
# 모듈 파일을 지정하지 않고 사용가능

func2()
```

- **Package에서 Import할 수 있는 변수, 함수, 클래스 제한이 가능하다.**
    1. `__init__.py` 파일 내 `__all__` 변수를 통해 사용할 요소를 지정하여 제한 시킬 수 있다.
    2. `__all__` 변수의 Default 값은 모든 변수,함수,클래스 (때문에 따로 정의를 하여 요소를 제한 시킴)
    3. `__all__` 변수는 `String` 값의 요소를 갖고 있는 `List` (List of String)
    4. 따라서, Import해야 할 요소들을 `String` 으로 `List` 에 선언

```python
# __init__.py
from .mod1 import func2
from .mod2 import func3

__all__ = ['func2', 'func3'] #__all__ 변수에 사용할 함수 정의
```

```python
# main.py
from pkg import *

func2()
func3()
func4() ## <== Error. func4 함수는 __all__ 에 정의되지 않았으므로 import 될 수 없음.
```

> **💡왜 import 할 수 있는 요소를 제한할까?**
- 내부적으로만 사용되어야 하는 요소가 존재 한다.
- 이러한 요소가 Package 외부에서 Import 되는것을 막기 위해 제한 시킨다.

- **Package가 Import될 때 반드시 먼저 실행되어야 하는 코드들이 있을 때**

# 다른 사람이 만든 Package 사용하기

- Python의 Package Manager 사용 (PIP)
- `pip install Django`

# Import Search 순서

- sys.modules ➡️ built-in modules ➡️ sys.path

# sys.modules

- Python에서 Module, Package를 찾기 위해 가장 먼저 확인하는 곳
- `sys.modules`은 단순한 `dictionary`
- 이미 Import된 Module과 Package들을 저장하고 있다.
- 즉, 한번 Import된 Module, Package들을 Python이 또 다시 찾지 않도록 하는 기능을 갖고 있다.
- 새로 import하는 모듈은 `sys.modules` 에서 찾을 수 없다.

> **💡sys.modules에 import된 Module, Package를 확인 하는 방법**
```python
import sys
print(sys.modules)
```
```python
{
'sys': <module 'sys' (built-in)>, 
'builtins': <module 'builtins' (built-in)>, 
'_frozen_importlib': <module 'importlib._bootstrap' (frozen)>,...
'enum': <module 'enum' from '/usr/lib/python3.8/enum.py'>, 
'signal': <module 'signal' from '/usr/lib/python3.8/signal.py'>
}
```

`dictionary` 타입임을 확인 할 수 있다.

# built-in modules

- Python에서 제공하는 공식 라이브러리들
- 이미 Python에 포함되어 있기 때문에 Python이 쉽게 찾을 수 있다.
- `print()`, `set()` 과 같은 함수들이 해당된다.

# sys.path

- Python에서 마지막으로 찾는 곳
- 기본적으로 `List` 이며 `String` 요소들을 갖고 있다.
- Python package들의 경로를 갖고 있다.
- Python은 `List` 의 각 경로를 하나씩 확인해 가면서 해당 경로에 import 한 Package가 위치해 있는지 확인한다.
- 만약 `sys.path`에서도 못 찾는다면 Python은 `ModuleNotFoundError` 에러를 리턴한다.

> **💡sys.path 경로 확인 방법**
```python
import sys
print(sys.path)
```

```python
['/home/runner/Python-Training', '/opt/virtualenvs/python3/lib/pyth
on3.8/site-packages', '/usr/lib/python38.zip', '/usr/lib/python3.8'
, '/usr/lib/python3.8/lib-dynload']
```

`List` 타입임을 확인 할 수 있다.

# Absolute Path & Relative Path

- built-in modules나 pip를 통해 설치한 module 및 package는 일반적으로 import하는데 문제가 잘 없다.
- pip를 통해 패키지 설치시 site-packages라는 directory에 설치가 되며 이는 `sys.path`에 이미 포함되어 있다.
- 하지만 직접 개발한 local package를 import 할때는 해당 package의 위치에 맞게 import 경로를 잘 선언 해야 한다.

## Absolute Path

- 절대 경로
- import하는 파일이나 경로에 상관없이 항상 경로가 동일하기 때문
- 경로가 길어질 수 있다는 단점이 있다.

> **Absolute Path 사용방법**
>
```
└── my_app
    ├── main.py
    ├── package1
    │   ├── module1.py
    │   └── module2.py
    └── package2
        ├── __init__.py
        ├── module3.py
        ├── module4.py
        └── subpackage1
            └── module5.py
```
위와 같이 `package1`, `package2` 이라는 두개의 패키지와 `package2`와 같이 `subpackage1` 이라는 중첩 package를 갖고 있는 프로젝트에서  `package1`,`package2`를 import 한다고 가정할 때 다음과 같이 import 하면 된다.
>
```python
from package1 import module1
from package1.module2 import function1
from package2 import class1
from package2.subpackage1.module5 import function2
```
my_app이라는 최상위 directory에서 시작하여 경로를 지정한다. 따라서 `subpackage1`의 `module5` module의 `function2` 함수를 import 하기 위해 다음과 같은 경로를 거치게 된다.
- my_app ➡️ package2 ➡️ subpackage1 ➡️ module5
>
만약 Python이 리눅스라면 백슬래시(/) 윈도우라면 슬래시(\) 형태로 이 경로가 나타날 것이고 Python은 이점(.)을 이용하여 이 경로를 표현한다. 다만 최상위 directory인 my_app은 current directory라고 하는 현재의 프로젝트 디렉토리는 Default로 `sys.path`에 포함되어 생략되고 다음과 같이 표현 가능하다.
- `package2.subpackage1.module5`

## Relative Path

- import하는 위치를 기준으로 경로를 정의 한다.
- local package 안에서 다른 local package를 import 할때 사용 된다.
- 경로의 길이가 줄어든다
- 헷갈리기 쉽다.
- 파일 위치가 변경되면 경로 위치도 변경되어야 하는 단점이 있다.
- 왠만하면 Absolute Path를 사용하는걸 권장한다.

> **Relative Path 사용 방법**
>
```
└── my_app
    ├── main.py
    ├── package1
    │   ├── module1.py
    │   └── module2.py
    └── package2
        ├── __init__.py
        ├── module3.py
        ├── module4.py
        └── subpackage1
            └── module5.py
```
위와 같이 `package1`, `package2` 이라는 두개의 패키지와 `package2`와 같이 `subpackage1` 이라는 중첩 package를 갖고 있는 프로젝트에서  `package2`의 `module3`에서 `package2`의 `class1` 과 `package2`의 하위 package인 `subpackage1`의 `module5`의 `function2`를 import 한다고 가정할 때 다음과 같이 import 하면 된다.
>
```python
# package2/module3.py

from .import class1
from .subpackage1.module5 import function2
```
여기서 점(.)은 import 하는 파일의 현재 위치를 말한다. 즉, 위 예제에서 `module3`가 위치해 있는 경로인 package2가 현재 경로, 점(.)인 것이다.
그럼 만약 `subpackage1`에서 `package2`의 `module4`를 import 하려고 한다면 어떻게 해야 할까.
>
```python
# subpackage1/module5.py
from ..module4 import class4
```
>
그럴 때는 상위 디렉토리로 이동해야 하므로 점(.)을 2개 사용하여(..) 상위 디렉토리로 이동해야 한다.
![](https://images.velog.io/images/yongineer1990/post/336d6e0a-de52-44d1-afc1-24ed474fe8e9/image.png)
>(터미널에서 폴더 이동하는 것과 같다)

# Assignment

## 1. sys.modules 와  sys.path의 차이점

**sys.modules**

- Python에서 Module, Package를 찾기 위해 **가장 먼저 확인하는 곳**
- `sys.modules`은 단순한 `dictionary`
- 이미 Import된 **Module과 Package들을 저장**하고 있다.
- 즉, 한번 Import된 Module, Package들을 Python이 또 다시 찾지 않도록 하는 기능을 갖고 있다.
- 새로 import하는 모듈은 `sys.modules` 에서 찾을 수 없다.

**sys.path**

- Python에서 **마지막으로 찾는 곳**
- 기본적으로 `List` 이며 `String` 요소들을 갖고 있다.
- Python **package들의 경로**를 갖고 있다.
- Python은 `List` 의 각 경로를 하나씩 확인해 가면서 해당 경로에 import 한 Package가 위치해 있는지 확인한다.
- 만약 `sys.path`에서도 못 찾는다면 Python은 `ModuleNotFoundError` 에러를 리턴한다.

## 2. Python은 sys 모듈의 위치를 어떻게 찾을 수 있을까?

위에 잠깐 정리가 되었지만, `sys.modules`를 출력해보면 다음과 같은 결과를 얻을 수 있다.

```python
import sys
print(sys.modules)
```

```python
{
'sys': <module 'sys' (built-in)>, 
'builtins': <module 'builtins' (built-in)>, 
'_frozen_importlib': <module 'importlib._bootstrap' (frozen)>,...
'enum': <module 'enum' from '/usr/lib/python3.8/enum.py'>, 
'signal': <module 'signal' from '/usr/lib/python3.8/signal.py'>
}
```

여기서 제일 첫 줄을 보면 `'sys': <module 'sys' (built-in)>` 임을 확인 할 수 있다. 즉, Python은 sys module을 `built-in modules`에서 찾는다는 것을 확인 할 수 있다.

## 3. Absolute Path & Relative Path

**Absolute Path**

- 절대 경로
- import하는 파일이나 경로에 상관없이 **항상 경로가 동일하다.**
- 경로의 시작은 항상 **프로젝트 최 상단 디렉토리에서부터 시작**된다.
- **경로가 길어질 수 있다는 단점**이 있다.

**Relative Path**

- **import하는 위치를 기준으로 경로를 정의** 한다.
- local package 안에서 다른 local package를 import 할때 사용 된다.
- **경로의 길이가 줄어든다**
- 헷갈리기 쉽다.
- **파일 위치가 변경되면 경로 위치도 변경**되어야 하는 단점이 있다.

## 4. Calculator Package 만들어 보기

![](https://images.velog.io/images/yongineer1990/post/c40fa20d-2799-48d0-a07d-cd37798ca8fd/image.png)

## 5. main module에서 package의 module을 어떻게 import 해야 할까

만약 main 모듈에서 Relative Path로 module를 import하면 다음과 같은 오류가 발생된다.

- `Attempted relative import beyond top-level package`

원인은 [Python documents](https://docs.python.org/3/tutorial/modules.html#intra-package-references)에서 찾을 수 있는데 다음과 같이 안내 하고 있다.

> Since the name of the main module is always "**main**", modules intended for use as the main module of a Python application must always use absolute imports.

Python에서 Relative Path는 항상 Module의 이름을 기준으로 경로를 찾는데 main module의 이름은 항상 main이다. 이를 알 수 있는 방법은 `__name__` 이란 변수를 호출해 보면 알 수 있다.

> 💡예제 코드의 각 모듈에 `__name__` 변수를 출력하도록 수정 한 후 실행 해보자.
>
>**Code**
>
```python
# main.py
# absoulte path
#from calculator.add_and_multiply import add_and_multiply

# relative path
from calculator.add_and_multiply import add_and_multiply

if __name__ == '__main__':
    print(add_and_multiply(1, 2))
    print(__name__)
```

```python
# add_and_multiply.py
from .multiplication import multiply
# from calculator.multiplication import multiply

def add_and_multiply(a, b):
    print(__name__)

    return multiply(a, b) + (a+b)
```

```python
# multiplication.py
def multiply(a, b):
    print(__name__)
    return(a*b)
```

>**Result**
>
```python
calculator.add_and_multiply #add_and_multiply.py의 __name__
calculator.multiplication #multiplication.py의 __name__
5 #add_and_multiply module에서 리턴된 값
__main__ #main.py의 __name__
```

>따라서 main module에서 Relative Path를 통해 모듈의 경로를 찾을수 없으며 main module에서는 항상 Absolute Path를 사용해야 한다. 따라서 예제 코드를 수정 하면 다음과 같다.

```python

# absoulte path
from calculator.add_and_multiply import add_and_multiply 

# relative path
#from .calculator.add_and_multiply import add_and_multiply

if __name__ == '__main__':
    print(add_and_multiply(1,2))

```

# 6. Absolute Path & Relative Path

## Absolute Path
>
```python
from calculator.multiplication import multiply

def add_and_multiply(a, b):
    return multiply(a, b) + (a+b)
```

Python이 Module을 찾는 순서는 다음과 같다.
![](https://images.velog.io/images/yongineer1990/post/cfcbf0ea-1fbc-407d-9de1-f7fefcb411f2/image.png)

- 1️⃣ 전체 프로젝트 영역에서
- 2️⃣Calculator 패키지를 찾는다.
- 3️⃣Calculator 패키지를 찾았다면 Multiplication 모듈을 선택한다.

## Relative Path

```python
from .multiplication import multiply

def add_and_multiply(a, b):
    return multiply(a, b) + (a+b)
```

Python이 Module을 찾는 순서는 다음과 같다.
![](https://images.velog.io/images/yongineer1990/post/14c37371-3ea4-458a-8a2f-8219e6a891a3/image.png)

- 1️⃣import를 실행한 같은 디렉토리에서
- 2️⃣Multiplication 모듈을 찾은 다음 선택한다.

## 7. `__**init**__.py` 의 역할

- **Import 할때 경로의 총 길이를 줄여줄 수 있다.**


```python
# __init__.py
from .mod1 import func2
```

```python
# main.py
from pkg import func2
# 이미 pkg.mod1에 있는 함수지만 __init__.py에서 이미 경로를 지정했기때문에 
# 모듈 파일을 지정하지 않고 사용가능

func2()
```

- **Package에서 Import할 수 있는 변수, 함수, 클래스 제한이 가능하다.**
    1. `__init__.py` 파일 내 `__all__` 변수를 통해 사용할 요소를 지정하여 제한 시킬 수 있다.
    2. `__all__` 변수의 Default 값은 모든 변수,함수,클래스 (때문에 따로 정의를 하여 요소를 제한 시킴)
    3. `__all__` 변수는 `String` 값의 요소를 갖고 있는 `List` (List of String)
    4. 따라서, Import해야 할 요소들을 `String` 으로 `List` 에 선언


```python
# __init__.py
from .mod1 import func2
from .mod2 import func3

__all__ = ['func2', 'func3'] #__all__ 변수에 사용할 함수 정의
```

```python
# main.py
from pkg import *

func2()
func3()
func4() ## <== Error. func4 함수는 __all__ 에 정의되지 않았으므로 import 될 수 없음.
```

- **Package가 Import될 때 반드시 먼저 실행되어야 하는 코드들이 있을 때**
