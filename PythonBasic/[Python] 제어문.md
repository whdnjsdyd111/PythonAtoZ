# if

다음은 `if` 와 `else` 를 사용한 조건문 기본 구조다.

```python
if 조건문:
  수행 문장
  수행 문장
  ...
else:
  수행 문장
  수행 문장
  ...
```

`if` 가 참이면 해당 조건문들을 시행하고, 거짓이면 `else` 문의 문장들을 수행한다.
파이썬은 들여쓰기를 통해 if 에 속하는 문장임을 표현한다.

```python
if 조건문:
    수행할 문장1
수행할 문장2
    수행할 문장3
```

위와 같이 작성하면 들여쓰기 오류가 발생할 것이다.

## 비교 연산자

조건문에 사용할 비교 연산자에는 다음과 같이 있다.


비교연산자|설명|
---|---|
x < y|	x가 y보다 작다|
x > y|	x가 y보다 크다|
x == y|	x와 y가 같다|
x != y|	x와 y가 같지 않다|
x >= y|	x가 y보다 크거나 같다|
x <= y	x가 y보다 작거나 같다

```python
>>> x = 3
>>> y = 2
>>> x > y
True
>>> x < y
False
```

## and, or, not

비교 연산자 외에는 and, or, not 도 존재한다.


연산자|	설명|
---|---|
x or y|	x와 y 둘중에 하나만 참이어도 참이다|
x and y|	x와 y 모두 참이어야 참이다|
not x|	x가 거짓이면 참이다|

```python
>>> money = 2000
>>> card = True
>>> if money >= 3000 or card:
...     print("택시를 타고 가라")
... else:
...     print("걸어가라")
...
택시를 타고 가라
```

## in, not in

어떠한 변수 안에 원하는 값이 있는지 찾을 때 사용한다.


in|	not in|
---|---|
x in 리스트|	x not in 리스트|
x in 튜플|	x not in 튜플|
x in 문자열|	x not in 문자열|

```python
>>> 1 in [1, 2, 3]
True
>>> 1 not in [1, 2, 3]
False
>>> 'a' in ('a', 'b', 'c')
True
>>> 'j' not in 'python'
True
```

## pass

조건문 안에 아무 행동도 하고싶지 않을 때가 있다.
그렇다고 문장을 작성하지 않으면 구문 오류가 발생하므로 `pass` 를 사용한다

```python
>>> pocket = ['paper', 'money', 'cellphone']
>>> if 'money' in pocket:
...     pass 
... else:
...     print("카드를 꺼내라")
```

# elif

다중 조건에 대한 판단을 위해 `elif` 를 사용한다.

```python
if <조건문>:
    <수행할 문장1> 
    <수행할 문장2>
    ...
elif <조건문>:
    <수행할 문장1>
    <수행할 문장2>
    ...
elif <조건문>:
    <수행할 문장1>
    <수행할 문장2>
    ...
else:
   <수행할 문장1>
   <수행할 문장2>
   ... 
```

# 조건부 표현식 (conditional expression)

한 변수에 대해 조건에 따라 다르게 할당할 때 조건부 표현식을 사용한다.

```python
if score >= 60:
    message = "success"
else:
    message = "failure"
```

위와 같은 코드를 다음과 같이 작성할 수 있다.

```python
message = "success" if score >= 60 else "failure"
```

조건부 표현식의 형식은 다음과 같다.

`조건문이 참인 경우` if `조건문` else `조건문이 거짓인 경우`

# while 문

다음은 `while` 문의 기본적인 구조이다.

```python
while <조건문>:
  <수행 문장1>
  <수행 문장1>
```

`while` 문은 조건문이 참인 동안 반복하여 수행한다.

### 무한 루프

그렇다면 조건문이 항상 참이면 어떻게 될까?

```python
while True:
  수행할 문장1
  ...
```

위와 같은 상황은 조건문이 True 이므로 항상 참이다. 그래서 무한하게 수행될 것이다.
그래서 의미없는 무한 루프를 조심하자.

# break & continue

`while` 문을 강제로 빠져나가고 싶은 경우가 있다면 `break` 문을 사용하면 된다.

예를 들어 한 변수를 1씩 증가시키다가 5가 되었을 때 `break` 문을 사용하여 `while` 문을 빠져나가보자.

```python
>>> a = 1
>>> while True:
...   a = a + 1
...   if a == 5:
...     break
>>> print(a)
5
```

`break` 문과 달리 그 다음 문장들을 생략만 하고 싶다면 `continue` 문을 사용하면 된다.

1부터 10까지 숫자 중 홀수만 출력하는 프로그램을 작성해보자.

```python
>>> a = 0
>>> while a < 10:
...   a = a + 1
...   if a % 2 == 0: continue
...   print(a)
...
1
3
5
7
9
```

# for 문

for 문의 기본 구조는 다음과 같다.

```python
for 변수 in 리스트(또는 튜플, 문자열):
    수행할 문장1
    수행할 문장2
    ...
```

리스트, 튜플, 문자열 등을 첫 번째 요소부터 마지막 요소까지 차례로 변수를 받아 수행한다.

for 문을 사용하는 예시이다.

```python
>>> test_list = ['one', 'two', 'three'] 
>>> for i in test_list: 
...     print(i)
... 
one 
two 
three
```

그리고 for 문도 마찬가지로 앞서 `while1` 문 때 본 `break` 와 `continue` 를 사용할 수 있다.

### range 함수

`range` 함수를 사용하여 for 문을 순차적으로 쉽게 사용할 수 있다.
예를 들어 `range(10)` 은 0 부터 10 미만의 숫자를 포함하는 range 객체를 만들어준다.

이를 이용하여 간단하게 1부터 10까지 더하는 프로그램을 만들어보자.

```python
>>> add = 0 
>>> for i in range(1, 11): 
...     add = add + i 
... 
>>> print(add)
55
```

### 리스트 내포

먼저, 기존 리스트의 숫자들을 3배 곱한 리스트를 만든다고 생각해보자.

```python
>>> a = [1,2,3,4]
>>> result = []
>>> for num in a:
...     result.append(num*3)
...
>>> print(result)
[3, 6, 9, 12]
```

위와 같은 예제는 리스트에 내포하여 간단하게 해결할 수 있다.

```python
>>> a = [1,2,3,4]
>>> result = [num * 3 for num in a]
>>> print(result)
[3, 6, 9, 12]
```

리스트 내포에는 if 문도 가능하다.

```python
>>> a = [1,2,3,4]
>>> result = [num * 3 for num in a if num % 2 == 0]
>>> print(result)
[6, 12]
```

