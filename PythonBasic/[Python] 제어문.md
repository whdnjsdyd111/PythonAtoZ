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
