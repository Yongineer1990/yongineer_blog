---
title: TIL.26) Coding Test - K번째수
date: 2020-08-01 03:08:36
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# Question

### **문제 설명**

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
3. 2에서 나온 배열의 3번째 숫자는 5입니다.

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- array의 길이는 1 이상 100 이하입니다.
- array의 각 원소는 1 이상 100 이하입니다.
- commands의 길이는 1 이상 50 이하입니다.
- commands의 각 원소는 길이가 3입니다.

# 나의 해결방식

```python
def solution(array, commands):
    answer = []
    for i in commands:
        result = array[i[0]-1 : i[1]]
        result.sort()
        answer.append(result[i[2]-1])
    return answer
```

첫번째 시도에 사용한 코드고 바로 통과되었다. 내 코드는 다음과 같은 과정을 거친다.

1. 2차원 배열로 인자가 전달된 `commands`를 일단 하나씩 불러온다 (`i`)
2. `i`의 0번째 값과 1번째 값으로 `array`를 자르고 그 결과를 `result`변수에 저장한다.
3. `result`를 `sort()`함수로 정렬하고
4. `answer`에  `i`의 2번째 값을 저장한다.

그러나 좀더 간결하게 코드를 만들고 싶어 `list comprehension`을 사용하여 리팩토링 진행하였다.

```python
def solution(array, commands):
    answer = [sorted(array[i[0]-1 : i[1]])[i[2]-1] for i in commands]
    return answer
```

그리고 위와 같이 제출하고 혼자 뿌듯해 했을 때 괴물과 같은 코드를 보고 경악을 금치 못했다...

## 진짜 너무하네....

```python
def solution(array, commands):
    return list(map(lambda x:sorted(array[x[0]-1:x[1]])[x[2]-1], commands))
```

이건 뭔가요...?

하나씩 뜯어봅시다.

일단 이분은 `map()`함수와 `lambda`식을 적용하여 해결하셨는데 `map`은 입력받은 자료형의 각 요소가 합수에 의해 수행된 결과를 묶어서 `iterator`로 반환한다. 따라서 내 해결법과 같이 `for loop`을 사용할 필요가 없어진다.

`lambda x`는 `commands`의 각 요소가 되고 (`List`)이 `x`의 index 0과 1 로 `array`를 자르고 `sorted`를 통해 정렬한다 그리고 index 2의 값으로 원하는 값을 찾는다.

 자 그럼 `map`과 `lambda` 공부하자.

# lambda

```python
lambda 인수1, 인수2 ... : 표현식
```

- 익명함수이기 때문에 한번 쓰이고 다음줄로 넘어가면 `heap`메모리 영역에서 증발하게 된다.
- 일반적인 함수는 객체를 만들고, 재사용하기 위해 메모리에 공간을 차지한다.

## 1. 람다 표현식으로 함수 만들기

먼저 인자로 받은 수에 10을 더해서 반환하는 함수를 만든다고 가정했을 때 다음과 같이 만들수 있다.

```python
def plus_ten(x):
  return x + 10

print(plus_ten(1))
```

위 예제함수를 `lambda`표현식으로 바꾸면 아래와 같다.

```python
lambda x: x+10
```

`lambda` 표현식은 위와 같이 `lambda`에 매개변수를 지정하고 `:`뒤에 반환값으로 사용할 식을 지정한다.

```python
<function <lambda> at 0x7f3df15803a0> 
```

그런데 실행해보면 위와 같이 함수 객체가 반환되는데 이 상태로는 함수를 호출 할 수 없다. 왜냐하면 위에서도 설명했듯 람다표현식은 이름이 없는 함수 즉, 익명 함수를 만들기 때문이다.

그렇기 때문에 람다로 만든 익명함수를 호출하려면 변수에 할당 해야 한다.

```python
plus_ten = lambda x: x+10
print(plus_ten(1))

# Result
11
```

> ⚠️ 일반함수 vs 람다표현식
>
![](https://yongnas.iptime.org/python/til26_1.png)

## 2. 람다 표현식 자체를 호출하기

```python
(lambda x : x+10)(2)

# Result
12
```

람다표현식을 `()`로 묶고 다시 `()`에 인수를 넣어 호출하면 된다.

주의할 점은 람다 표현식 안에서 새 변수를 만들수 없다. 따라서 반환값 부분은 변수 없이 식 한 줄로 표현할 수 있어야 한다. 변수가 필요할경우에는 `def`로 함수를 만드는것이 좋다.

```python
(lambda x : y=10, x+y)(2)

# Result
SyntaxError: invalid syntax
```

다만 람다 표현식에서는 바깥에 있는 변수는 사용할 수 있다.

```python
y=10
(lambda x : x+y)(2)

# Result
12
```

## 3. 람다 표현식을 인수로 사용하기

람다 표현식을 사용하는 이유는 함수의 인수 부분에서 간단하게 함수를 만들기 위해서 주로 사용한다. 대표적인 예가 `map`이다.

먼저 `def`로 함수를 생성 후 `map`에 `plus_ten`함수와 리스트를 전달 한 후 `map`객체를 확인하기 위해 `list`로 변환하는 코드를 작성할 경우 다음과 같이 작성 할 수 있다.

```python
def plus_ten(x):
  return x + 10

result = list(map(plus_ten, [1,2,3]))
print(result)

# Result
[11, 12, 13]
```

이제 이 코드를 람다 표현식으로 변환하는 다음과 같이 변환 할 수 있다.

```python
result = list(map(lambda x: x + 10, [1,2,3]))
print(result)

# Result
[11, 12, 13]
```

결과를 출력하는 부분을 제외하면 코드가 많이 줄어든걸 볼 수 있다.

이처럼 람다 표현식은 함수를 다른 함수의 인수로 넣을 때 매우 편리하다.

> ⚠️ 람다 표현식으로 매개변수가 없는 함수 만들기
>
>    ```python
>    (lambda : 1)()
>    # Result
>    1
>
>    x = 10
>    (lambda : x)()
>    # Result
>    10
>    ```
>
`lambda`뒤에 아무것도 지정하지 않고 `:`을 붙인다. 단, `:`뒤에는 반드시 반환할 값이 있어야 한다.

## 4. 람다 표현식에 조건부 표현식 사용하기

```python
lambda 매개변수1, 매개변수2 ... : 식1 if 조건식 else 식2
```

람다 표현식안에서 `if`, `else`를 사용할 때는 일반적인 조건식 문법과 다르게 `:`을 붙이지 않는다. 또한 람다 표현식에서 `if`를 사용한다면 반드시 `else`를 사용해야 한다. 만약 `if`만 사용한다면 문법 에러가 발생한다.

```python
list(map(lambda x: x+1 if x == 2, [1,2,3]))

# Result
SyntaxError: invalid syntax
```

또한 람다 표현식 안에서는 `elif`를 사용할 수 없다. 

```python
list(map(lambda x: x+1 if x == 2 elif x == 3, [1,2,3]))

# Result
SyntaxError: invalid syntax
```

따라서 `elif`를 써야하는 상황이면 `식1 if 조건식1 else 식2 if 조건식2 else 식3`과 같이 `if`를 연속으로 사용해야 한다.

```python
# 리스트에서 2와 4를 1더함
list(map(lambda x: x+1 if x == 2 else x+1 if x == 4 else x, [1,2,3,4,5]))

# Result
[1, 3, 3, 5, 5]
```

그러나 위와 같은 예제는 복잡도는 낮으나 가독성이 떨어진다 이럴경우는 그냥 `def`함수를 만들어 `if`, `elif`, `else`를 사용하는게 나을거 같다.

다음은 `map`을 이용하여 리스트 `mylist`에서 2의 배수를 문자열로 변환하는 코드이다.

```python
mylist = [1,2,3,4,5,6,7,8,9,10]
result = list(map(lambda x: str(x) if x % 2 == 0 else x, mylist))

print(result)
```

`map`은 리스트의 요소를 각각 처리하므로 `lambda`의 반환값도 요소여야 한다.

위 예제는 요소가 2의 배수일 때는 문자열로 만들어 변환했고, 2의 배수가 아닐 때는 x값 그대로 반환했다.

![](https://yongnas.iptime.org/python/til26_2.png)

# map( )

```python
map(f, 반복 가능한 자료형)
```

- 내장함수
- 입력받은 자료형의 각 요소가 합수에 의해 수행된 결과를 묶어서 `map iterator`객체로 반환
- `Lazy evaluation`을 수행하기 때문에 메모리를 효율적으로 사용가능

> ⚠️ `Lazy evalution`의 자세한 내용은 [여기](https://yongineer.netlify.app/TIL/til.07\)-[python]-generator/#lazy-evaluation) 참조

## 1. map과 lambda 사용하기

```python
mylist = [1,2,3,4,5,6,7,8,9]
result = list(map(lambda x: x*2, mylist))
print(result)

# Result
[2, 4, 6, 8, 10, 12, 14, 16, 18]
```

## 2. map에 객체를 여러개 넣기

```python
list1 = [2,4,6,8,10]
list2 = [1,3,5,7,9]
result = list(map(lambda x, y: x-y, list1, list2))
print(result)

# Result
[1, 1, 1, 1, 1]
```

리스트 두 개를 처리할 때는 람다 표현식에서 `lambda x, y: x - y`와 같이 매개별수를 두개 지정하면 된다. 그리고 `map`에 람다표현식을 넣고 두 리스트를 `,`로 구분지어서 넣어준다.

즉, 람다 표현식의 매개변수 개수 만큼 반복 가능한 객체도 그 만큼 전달해줘야 한다.

# filter( )

```python
filter(func, 반복 가능한 객체)
```

- 반복 가능한 객체에서 득정 조건에 맞는 요소만 가져온다.
- `filter`에 지정한 함수의 반환값이 `True`일때 해당 요소만 가져온다.

```python
a = [8, 3, 2, 10, 15, 7, 1, 9, 0, 11]

result = list(filter(lambda x: x > 5 and x < 10, a))
print(result)

# Result
[8, 7, 9]
```

위 예제는 주어진 리스트에서 50보다 크고 150보다 작은 요소를 가져오는 예제이다.

이때 람다표현식을 사용하여 `filter`안에 `lambda x: x > 5 and x < 10`를 넣어주면 `def`함수를 작성하지 않고 간단하게 해당 요소를 가져올 수 있다.


![](https://yongnas.iptime.org/python/til26_3.png)

# reduce( )

```python
from functools import reduce
reduce(함수, 반복가능한객체)
```

- 반복 가능한 객체의 각 요소를 지정된 함수로 처리한 뒤 이절 결과와 누적해서 반환한다.
- 내장함수가 아니어서 `functools`모듈에서 `reduce`함수를 가져와야 한다.

```python
from functools import reduce

a = [1, 2, 3, 4, 5]

result = reduce(lambda x, y: x + y, a)
print(result)

# Result
15
```

`lambda x, y: x + y`와 같이 매개변수 `x`, `y`를 지정한 뒤 `x+y`의 결과를 반환하도록 만들었다.


![](https://yongnas.iptime.org/python/til26_4.png)

# 그런데 map lambda는 최선일까?

`map`과 `lambda`에대해 공부하면서 자료를 찾아보니 많은 사람들이 나와 같은 해결법인 `List comprehension`과 비교하는 [글](https://stackoverflow.com/questions/1247486/list-comprehension-vs-map)이 여럿 보였다. 과연 `map lambda`는 최선의 방법일까?

만약 `map lambda`가 `Lazy evaluation`을 수행하기 때문에 효율성이 좋다면 이미 [일전에 배운적 있던](https://yongineer.netlify.app/TIL/til.07\)-[python]-generator/#generator-expression) `Generator Expression`또한 `Lazy evaluation`을 수행하지 않는가? 그럼 이 친구도 `map lambda`만큼 효율성이 있지 않을까?

이를 알아보기 위해 간단한 테스트 코드를 작성했다.

`map lambda` VS `List comprehension` VS `Generator Expression`

```python
import time
xs=range(100000)

def map_lambda(xs):
  start = time.time()
  map(lambda x: x+2, xs)
  print(f"map_lambda time : {(time.time() - start)*1000}")

def lc(xs):
  start = time.time()
  [x+2 for x in xs]
  print(f"list_comp time : {(time.time() - start)*1000}")

def ge(xs):
  start = time.time()
  (x+2 for x in xs)
  print(f"ge time : {(time.time() - start)*1000}")

map_lambda(xs)
ge(xs)
lc(xs)
```

결과는 다음과 같았다.

```python
# map lambda
map_lambda time : 0.0021457672119140625

# Generator Expression
ge time : 0.0019073486328125

# List Comprehension
list_comp time : 15.371084213256836
```

**두둥탁**

확실히 `Lazy evaluation`을 수행하는 `map lambda`쪽의 속도가 더 빨랐다. 그런데 `Generator Expression`의 속도도 만만치 않았다. 그리고 `List Comprehension`의 속도는 역시나 느렸다.

그런데 효율성이 좋은 코드라도 가독성이 떨어진다면 문제이지 않을까? `map lambda`의 효율성은 좋으나 그 가독성은 떨어진다 반면 `Generator Expression`의 가독성은 그래도 `map lambda`보다는 좋다고 생각한다. 

또한 많은 개발자들 또한 이에 대해 고민하는거 같다. 어떤 [개발자 블로그](https://treyhunner.com/2018/09/stop-writing-lambda-expressions/#Should_you_ever_use_lambda_expressions?)를 보니 `lambda` 사용에 관해 다음과 같은 글이 있어서 그 내용을 조금 발췌하면 다음과 같다.

`lambda`의 사용은 다음과 같은 문제가 있다.

1. `lambda`표현식은 다수의 파이썬 프로그래머에게 아직 생소한 구문이다.
2. `lambda`로 생성한 함수는 익명함수 이기때문에 코드를 읽어보는것만이 유일하게 해당 함수가 수행하는 작업을 파악하는 방법이다.
3. `lambda`표현식에는 문장이 하나만 포함될 수 있으므로 튜플 언패킹과 같이 가독성을 향상시키는 기능을 사용할 수 없다.
4. 몇몇`lambda`함수는 표준 라이브러리 또는 파이썬 내장함수로 대체할 수 있다.

반면 해당 블로거는 `lambda`표현식을 다음과 같은 조건을 모두 충족하는 경우에는 사용해도 된다고 한다.

1. 람다 표현식으로 처리해야할 작업이 규모가 작으며 함수의 이름을 갖을 필요가 없을때
2. 함수를 정의하는 이름 보다 람다 표현식을 사용했을때 코드를 이해하기 쉽다면
3. 해당 함수의 기능이 아직 확실하지 않을때
4. **팀원 모두가 람다표현식을 이해하고 사용하기로 동의했을때**

사실 마지막이 가장 큰 이유일거 같다.

# 그래서 결론

```python
def solution(array, commands):
    return list(sorted(array[i[0]-1 : i[1]])[i[2]-1] for i in commands)
```

 이번 문제의 가장 적절한 답은 이것이 아닐까 조심스럽게 생각해본다.

*배운것도 못써먹는 나는 레전드다*
