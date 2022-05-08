[Python] 모듈
=============

모듈은 함수, 변수, 클래스 등을 모아놓은 파일로 다른 프로그램에서 만든 것을 사용할 수 있다.

## 모듈 생성

```python
# mod1.py
def add(a, b):
  return a + b
  
def sub(a, b):
  return a - b
```

위와 같이 덧셈과 뺄셈이 있는 `mod1.py` 모듈을 만들었다.
확장자가 `.py` 라면 모두 모듈이라고 할 수 있다.

## 모듈 호출

`mod1.py` 모듈을 호출해보자.

```python
>>> import mod1
>>> print(mod1.add(3, 2))
5
>>> print(mod1.sub(6, 3))
3
```

`import` 키워드를 사용하여 모듈을 불러와 도트 연산자(`.`) 로 함수를 호출하면 된다.
이와 다르게 `mod1` 모듈에 있는 `add` 함수만 필요하다고 할 경우에는 `from 모듈이름 import 모듈함수` 와 같은 형태로 `from mod1 import add` 로 호출할 수도 있다.
`from mod1 import add, sub` 로 여러 함수를 불러올 수도 있다.

## `__name__`

`__name__` 은 파이썬 내부적으로 특별한 변수로, 모듈 이름 값이 저장되어 있다.
`mod1` 모듈에서 `__name__` 을 출력하면 `mod1` 이 출력된다.

```python
# mod1.py
def add(a, b):
  return a + b
  
def sub(a, b):
  return a - b
  
print(add(1, 4))
print(sub(3, 2))
```

모듈이 위와 같이 작성되어 있다면, 다른 모듈에서 `mod1` 모듈을 호출하면 자동적으로 출력함수 때문에 `5` 와 `1` 이 호출된다.
이를 방지하기 위해서 메인에서만 출력되도록 하기 위해서

```python
if __name__ == "__main__":
    print(add(1, 4))
    print(sub(4, 2))
```

와 같이 작성하면 된다.
