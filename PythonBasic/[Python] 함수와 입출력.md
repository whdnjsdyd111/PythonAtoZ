[Python] 함수와 입출력
======================

# 함수

프로그래밍을 하며, 어떠한 입력값에 따라 다른 결과값을 얻고 싶을 때가 있다.
이 때 사용하는 것이 함수이다. 기본적인 구조는 다음과 같다.

```python
def 함수명(매개변수):
  <수행 문장1>
  <수행 문장2>
  ...
```

간단한 예로 두 매개변수를 받아 더한 값을 출력하는 함수를 만들어보자.

```python
def add(a, b):
  return a + b
```

이제 함수를 사용해보자.

```python
>>> a = 3
>>> b = 4
>>> c = add(a, b)
>>> print(c)
7
```

변수 a, b 를 `add` 함수에 매개변수로 전달하여 이에 대한 결과값이 c 에 대입되어 7 이 출력되었다.

## 함수 형태

### 입력값 O, 결괏값 O

앞서 살펴본 예제와 같은 형태로, 입력값과 결괏값이 존재하는 함수다.

```python
def 함수명(매개변수):
  <수행 문장>
  ...
  
  return 결과값
```

### 입력값 X, 결괏값 O

입력값이 존재하지 않고 결괏값만 존재하는 함수다.

```python
def 함수명():
  <수행 문장>
  ...
  
  return 결과값
```

예제는 다음과 같다.

```python
>>> def say():
...   return 'Hello'
...
>>> a = say()
>>> print(a)
Hello
```

위 예제의 `say()` 와 같이 호출할 때는 괄호에 아무 값도 넣지 않아야 한다.

### 입력값 O, 결괏값 X

결과값 없는 함수도 존재하며, 예제는 다음과 같다.

```python
>>> def add(a, b):
...   print("%d + %d = %d" % (a, b, a+b))
...
>>> add(1,2)
1 + 2 = 3
```

결괏값이 정말 없는지 함수 결과를 변수에 대입해보자.

```python
>>> a = add(3,4)
1 + 2 = 3
>>> print(a)
None
```

a 값이 `None` 으로 나타나는 것을 확인할 수 있다.

### 입력값 X, 결괏값 X

입력값 결괏값 모두 없는 함수도 존재한다. 예제는 다음과 같다.

```python
>>> def say(): 
...     print('Hi')
... 
>>> say()
Hi
```

### 매개변수 지정

다음과 같은 예가 있다.

```python
>>> def add(a, b):
...     return a+b
... 
```

위 함수를 사용할 때 매개변수를 지정하여 사용할 수 있다.

```python
>>> result = add(a=3, b=7) # b=7, a=3 과 같이 순서 상관 없음
>>> print(result)
10
```

### 매개변수 여러 값 받기

입력값을 몇 개가 될지 모를 때는 다음과 같은 방법을 사용할 수 있다.

```python
def 함수명(*매개변수):
  <수행 문장>
  ...
```

위와 같이 `*매개변수` 형태로 바뀐 것을 볼 수 있다.

먼저 예제로 입력값들을 받아 모두 더한 값을 돌려주는 함수를 만들자.

```python
>>> def add_many(*args):
...   result = 0
...   for i in args:
...     result += i
...   return result
```

위 함수는 몇 개의 입력값을 입력하든 상관없다.
`*args` 와 같은 매개변수가 입력값들을 모두 모아 **튜플**로 만들어주기 때문이다.

만약 `add_many(1, 2, 3)` 는 `(1, 2, 3)` 이 되는 형태다.
당연히 함수를 정의할 때 `def add_many(args1, *args2)` 와 같이 다른 매개변수도 사용할 수 있다.

### 키워드 파라미터

키워드 파라미터는 매개변수 앞에 `**` 를 붙여 사용한다.

```python
>>> def print_kwargs(**kwargs):
...   print(kwargs)
```

키워드 파라미터는 매개변수로 받은 값들을 딕셔너리로 받는다.

```python
>>> print_kwargs(name='foo', age=3)
{'age': 3, 'name': 'foo'}
```

### 튜플 결괏값

```python
>>> def add_mul(a, b):
...   return a+b, a*b
>>> print(add_mul(2, 3))
(5, 6)
```

위와 같이 결괏값을 여러 값을 명시하면 튜플 형태로 되돌려준다.
그렇다고 `return` 문을 여러 개를 작성하면 안된다.

### 매개변수 초깃값 설정

```python
def say(name, old, man=True):
  print("이름은 %s, 나이는 %d" % (name, old))
  if man:
    print("남자입니다.")
  else:
    print("여자입니다.")
```

위와 같은 함수가 있다고 생각하자. 먼저 살펴볼 것은 `man=True` 이다.
이는 매개변수를 설정하지 않았을 때의 기본 값을 설정할 수 있다. 초깃값을 설정하면 해당 매개변수는 생략할 수 있다.
다음과 같이 함수를 사용할 수 있다.

```python
say('파이썬', 23) # 이때 man 은 초깃값인 True
say('파이썬', 23, False)
```

### 범수 범위

일반적으로 함수 안에서 선언한 변수는 함수에서만 사용가능 하다.
함수 밖의 변수를 사용하고 싶다면 `global` 를 사용하면 된다.

```python
>>> a = 1
... def change():
...   global a
...   a = 3
  
>>> change()
>>> print(a)
3
```

### 람다식 (lambda)

lambda 는 함수를 예약어로 간결하게 사용하는 것이다.

```python
def add(a, b)
  return a + b
```

위 함수를 람다식으로 작성하면 다음과 같다.

```python
add = lambda a, b: a + b
```

람다식은 예약어기 때문에 return 명령 없어도 결괏값을 돌려준다.

입출력
======

# 입력과 출력

## 사용자 입력 `input`

사용자가 입력한 값을 받고 싶을 때는 `input` 함수를 사용한다.

```python
>>> a = input()
python
>>> a
python
>>> b = input()
3
>>> b
3
>>> type(b)
<class 'str>
```

input 으로 입력되는 모든 것은 문자열로 취급된다.
그래서 숫자로 사용하고 싶다면 변환 후에 사용하자.

## 사용자 출력 `print`

지금까지 `print` 함수로 출력을 해왔다.
조금 더 자세히 보자.

```python
>>> print("Hi " "python") # 큰 따옴표로 둘러싼 문자열은 + 연산과 동일
Hi python
>>> print("Hi " + "python")
Hi python
>>> print("Hi", "python") # 띄어쓰기는 콤마로
>>> for i in range(10):
...   print(i, end=' ') # 개행되지 않도록 end 로 끝문자 지정
0 1 2 3 4 5 6 7 8 9
```

파일 입출력
===========

# 파일 생성

다음은 파일을 생성하는 짧은 코드이다.

```python
f = open('새파일.txt', 'w')
f.close()
```

파이썬 내장 함수인 `open` 함수는 파일 이름과 파일 열기 모드를 입력값으로 받아 결과값을 파일 객체로 돌려준다.

파일 열기 모드 종류는 다음과 같다.

파일열기모드|설명|
---|---|
r|읽기 모드 - 읽기만 가능|
w|쓰기 모드 - 파일에 내용을 쓸 때 사용|
a|추가 모드 - 파일 마지막에 내용을 추가할 때 사용|

쓰기모드 사용 시 해당 파일이 이미 존재할 경우, 기존 내용이 없어지고 새로운 파일이 생성된다.
특정 디렉터리에 생성하고 싶다면 `C:/download` 와 같이 작성하면 된다.

`f.close()` 는 열려 있는 파일 객체를 닫아주는 역할을 한다.
프로그램이 종료될 때 자동으로 객체를 닫아주지만 직접 닫아서 열려 있는 파일을 다시 사용하게 되는 오류를 범하지 말자.

# 파일 쓰기

```python
f = open("새파일.txt", 'w')
for i in range(1, 11):
  data = "%d번째 줄\n" % i
  f.write(data)
f.close()
```

`write` 함수를 통해 파일에 내용을 쓸 수 있다.
파일 내용을 확인해보자.

![image](https://user-images.githubusercontent.com/66655578/166890371-cf7271a3-7611-4822-8aa2-e3afba732735.png)

# 파일 읽기

이번엔 파일을 읽어보자.

1. `readline` 함수

```python
f = open("새파일.txt", 'r')
line = f.readline()
print(line)
f.close()
```

`readline` 함수는 한 줄씩 읽어들이는 함수다.
위 프로그램을 실행한다면 `1번째 줄` 이 출력될 것이다.
그렇다면 모든 줄을 읽고 싶다면 다음과 같이 작성하면 된다.

```python
f = open("새파일.txt", 'r')
while True:
  line = f.readline()
  if not line: break
  print(line)
f.close()
```

`readline` 는 한 줄씩 읽다가 더 이상 읽을 줄이 없다면 `빈 문자열''` 을 리턴한다.
이를 이용해 `not line: break` 로 빠져 나가도록 한다.

2. `readlines` 함수

`readlines` 함수는 모든 줄을 읽어 각각의 줄을 리스트로 돌려준다.

```python
f = open("새파일.txt", 'r')
lines = f.readlines()
for line in lines:
    print(line)
f.close()
```

3. `read` 함수

`read` 함수는 파일 내용 전체를 문자열로 돌려준다.

```python
f = open("새파일.txt", 'r')
data = f.read()
print(data)
f.close()
```

# 파일 내용 추가

파일에 내용을 추가할 때는 파일 열기 모드를 `a` 로 연다.

```python
f = open("C:/doit/새파일.txt",'a')
for i in range(11, 20):
    data = "%d번째 줄\n" % i
    f.write(data)
f.close()
```

해당 코드를 실행하면 기존 내용 뒤에 새로운 내용들이 추가되는 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/66655578/166896289-e4742c4d-a198-4aa1-ac59-969df5eaf73a.png)

### `with` 문 사용

이전 예제들은 모두 `open` 후 `close` 로 객체를 닫아주었다.
`with` 문을 사용하면 해당 블록을 벗어날 때 열린 객체를 자동으로 `close` 되어 닫아준다.

```python
with open("example.txt", 'w') as f:
  f.write("use with")
```
