[Python] 객체지향프로그래밍
===========================

프로그램이 어떤 작업을 수행하기 위해선 데이터와 이를 조작하는 행위가 필요하다.
그래서 서로 연관된 데이터와 해당 함수를 조작하기 위한 함수를 하나의 집합으로 모은 **객체**를 사용한다.
이러한 객체들을 사용하여 각각의 상호작용으로 접근하는 프로그래밍을 객체지향 프로그래밍이라 한다.

객체에 포함되는 변수나 함수는 **멤버**나 **속성**이라 불린다.
파이썬에서의 객체지향프로그래밍을 살펴보자.

# 클래스 Class

**클래스**는 객체에 필요한 구조와 행위를 정의한 것이다.
다음은 클래스를 가장 간단하게 표현한 것이다.

```python
class ClassExp:
  pass
```

위 클래스는 아무 기능을 갖고있지 않은 클래스다.
위 `ClassExp` 클래스를 만들어 사용하는 방법은 다음과 같다.

```python
>>> a = ClassExp()
>>> b = ClassExp()
```

a, b 변수에 객체를 저장하였다.
a 와 b 는 클래스로 만들어 각자의 성질을 가진 객체. 즉, `ClassExp` 의 인스턴스라고 한다.

먼저 더하기, 빼기 기능이 있는 계산기를 예로 들자.
처음에는 숫자가 0 이며, 계산 한 후에는 해당 결과를 저장하고 있어야한다.
이러한 기능들을 클래스로 표현해본다.

```python
class Calculator:
  def __init__(self):
    self.result = 0
    
  def add(self, num):
    self.result += num
    return self.result
    
  def sub(self, num):
    self.result -= num
    return self.result
```

위 클래스의 타입을 보면 다음과 같다.

```python
>>> a = Calculator()
>>> type(a)
<class '__main__.Calculator instance at 0x10171f518'>
```

## 클래스 메서드 Method

먼저 위에서 Calculator 클래스에 정의한 add 메소드를 보자.

```python
  def add(self, num):
    self.result += num
    return self.result
```

가장 먼저 눈에 띄는 것은 `self` 이다.

파이썬의 클래스 메소드는 항상 맨 앞에 한 개의 변수가 추가된다.
또한 메소드 호출 시 값이 없으면 파이썬이 대신 자동으로 값을 할당한다.
이 변수는 객체 자신을 참조하는 것으로, 일반적으로 `self` 로 이름을 짓는다.

`a.add(1)` 와 같이 메소드를 호출할 수 있으며 이는 다음과 같은 `Calculator.(a, 1)` 형태로 바뀐다.

## 생성자 Constructor

그 다음으로는 `__init__(self)` 메서드를 보자.

이는 클래스가 인스턴스화 될 때 호출되는 것으로 생성자로 사용된다.

# 상속 Inheritance

객체지향 프로그래밍의 핵심 중 하나인 **상속**은 코드를 재사용할 수 있다는 큰 장점이 있다.

위 예제인 덧셈 뺄셈의 계산기 클래스가 존재하는데 이 기능들을 그대로 사용하면서 곱셈 나눗셈 기능을 추가하여 새로운 클래스를 작성하고 싶을 때 상속 개념을 사용하면 된다.

```python
class MoreCalculator(Calculator): # 상속할 클래스를 괄호안에 작성
  def mul(self, a):
    self.result *= a
    return self.result
    
  def div(self, a):
    self.result /= a
    return self.result
```
  
## 메소드 오버라이딩

이번에는 계산기의 기능을 더 개선한다고 생각해보자.
상식적으로 나눗셈을 할 때 0을 나누는 것은 수학적으로 성립되지 않는 개념이다.
그래서 `MoreCalculator` 클래스의 기능을 그대로 사용하면서 더 확장해보자.

이때, `div` 메소드를 동일한 이름으로 다시 만드는 것이 **메서드 오버라이딩**이다.

```python
class UpdateCalculator(MoreCalculator):
  def div(self, a):
    if self.result == 0 or a == 0:
      return 0
    else:
      return self.result / a
```
