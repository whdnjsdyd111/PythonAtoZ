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

[Python] 패키지
===============

패키지는 모듈을 디렉터리 구조로 관리하는 것이다.

먼저 game 패키지를 예로 들면 다음과 같다.

```
game/
  __init__.py
  play/
    __init__.py
    run.py
    test.py
  sound/
    __init__.py
    wav.py
```

game 디렉터리가 패키지의 루트 디렉터리고, play 와 sound 는 서브 디렉터리다.

## 패키지 생성

위와 같은 game 패키지를 예제로 만들어보자.
먼저 패키지의 기본 구성 요소를 준비한다.

루트 디렉터리 game 디렉터리에서 각각의 파일을 생성한다.

```
game/__init__.py
game/play/__init__.py
game/play/run.py
game/play/test.py
game/sound/__init__.py
game/sound/way.py
```

그 다음 `run` , `test` , `way` 파일에 각각의 메소드를 추가한다.

```python
# run.py
def run_test()
  print("run")
```

```python
# test.py
def test()
  print("test")
```

```python
# way.py
def run_test()
  print("wau")
```

## `__init__.py` 용도

위 각각 패키지에서 디렉터리마다 `__init__.py` 를 생성했는데 이 역할은 디렉터리 패키지의 일부임을 알리는 역할이다.
이 파일이 없다면 패키지로 인식하지 않는다.

예를 들어 패키지에서 `play` 의 하위 파일을 모두 사용한다고 해보자.

```python
from game.run import *
run.run_test()
NameError: name 'echo' is not defined
```

그럼 이름 오류(NameError)가 발생한다.
이렇게 디렉터리의 모듈을 `*` 사용하면 해당 디렉터리의 `__init__.py` 파일에 `__all__` 변수 설정하고 import 할 수 있도록 정의해야 한다.

```python
# run/__init__.py
__all__ = ['run']
```

여기에서 `__all__` 지정된 모듈들만 import 된다.

## relative 패키지

`play/run.py` 모듈에서 `sound/way.py` 모듈을 사용하고 싶다면 다음과 같이 사용하면 된다.

```python
# way.py
from game.run.run import run_test
```

위처럼 전체경로를 사용하여 import 할 수 있지만 다음과 같이 relative 하게 import 도 가능하다.

```python
# way.py
from ..run.run import run_test
```
