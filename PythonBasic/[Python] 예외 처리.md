[Python] 예외 처리
==================

프로그램을 개발하다 보면 오류가 발생하기 나름이다.
이러한 예외를 무시하거나 처리리해야 할 때 **예외 처리**를 해야한다.

## 예외 발생하는 경우

예외가 발생하는 하는 것은 오타를 입력하여 구문 오류가 발생하는 것과 같은 **오류** 가 아닌 프로그램 실행 중 파일을 open 했을 때 해당 파일이 없을 때 발생하는 것이 **예외** 이다.

```python
>>> f = open("없는파일", 'r')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: '없는파일'
```

다른 예로는 0으로는 나눌 수 없는 데 이를 시도할 경우에 발생한다.

```python
>>> 1 / 0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
```

마지막으로 한 예는 배열에 접근할 때 범위를 벗어나는 것이다.

```python
>>> a = [1, 2, 3, 4, 5]
>>> a[6]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

이러한 예외가 발생하면 프로그램은 중단되고 오류 메시지를 보여준다.

## 예외 처리 기법

이제 예외를 처리는 기법에 대해 알아보자.

### try, except 문

다음은 기본적인 try except 문이다.

```python
try:
  ...
except [예외 [as 오류 메시지 변수]]:
  ...
```

try 블록을 수행 중 예외가 발생하면 except 블록을 수행한다.
try except 문은 세 가지 경우로 작성할 수 있다.

1. try, except 만 작성

```python
try:
  ...
except:
  ...
```

예외 발생 시 except 블록을 수행한다.

2. 발생 오류만 포함한 except

```python
try:
  ...
except 발생 오류:
  ...
```

위의 경우에는 except 문에 작성한 오류와 일치하는 것만 처리하여 except 블록을 수행한다.

3. 발생 오류와 오류 메시지 변수 포함한 except 문

```python
try:
  ...
except 발생 오류 as 오류 메시지 변수:
  ...
```

위 경우에는 오류 메시지를 알고 싶을 때 사용하는 방법이다.
예는 다음과 같다.

```python
try:
  4/0
except ZeroDivisionError as e:
  print(e)
```

위와 같이 오류 메시지를 포함한 변수를 출력하여 오류 메시지를 확인할 수 있다.

### try ... finally

try 문 마지막에 finally 절을 추가로 사용할 수 있다.
예외 발생 여부와 상관없이 항상 수행된다. 일반적으로 `close` 문으로 리소스를 닫아야 할 때 많이 사용한다.

### 다중 예외 처리

하나의 요류만 아닌 여러 오류를 처리해야 할 때 다음과 같은 구문을 작성하면 된다.

```python
try:
  ...
except 발생 오류1:
  ...
except 발생 오류2:
  ...
```

위의 경우 각각의 오류를 따로 처리해야할 때 사용하고, 공통적으로 함께 처리할 경우엔 다음과 같이 작성해도 좋다.

```python
try:
  ...
except (발생오류1, 발생오류2):
  ...
```

### try ... else 절

try 문에는 다음과 같은 else 문도 사용할 수 있다.

```python
try:
  ...
except 발생오류:
  ...
else: # 오류 없을 경우 시행
  ...
```

### 오류 회피

프로그래밍을 하다 보면 특정 오류를 통과시켜야 하는 경우가 있다. 이때는 `pass` 키워드를 사용한다.

```python
try:
  ...
except 발생오류:
  pass
```



