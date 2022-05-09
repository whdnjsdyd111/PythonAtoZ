[Python] 내장 함수
==================

파이썬의 내장함수를 살펴보자.

## abs

절댓값을 되돌려 준다.

```python
>>> abs(-3)
3
```

## all

`all(x)` 는 자료형 x 를 인수로 받아 해당 요소들이 모두 참이면 True, 하나라도 거짓이면 False 를 반환한다.

```python
>>> all([1, 2, 3])
True
>>> all([0, 1, 2])
False
```

## any

`any(x)` 는 자료형 x 를 인수로 받아 해당 요소 중 하나라도 참이면 True, 모두 거짓이면 False 를 반환한다.

```python
>>> any([1, 3, 4, 0])
True
>>> any([0, ""])
False
```

## chr

유니코드 값을 받아 해당 문자를 출력한다.

```python
>>> chr(97)
'a'
```

## dir

객체가 자체적으로 가지고 있는 변수나 함수를 보여준다.

```python
>>> dir([1, 2, 3])
['append', 'count', 'extend', 'index', 'insert', 'pop',...]
>>> dir({'1':'a'})
['clear', 'copy', 'get', 'has_key', 'items', 'keys',...]
```

## divmod

인수로 숫자 2 개를 받아 몫과 나머지를 튜플 형태로 반환한다.

```python
>>> divmod(7, 3)
(2, 1)
```

## enumerate

순서가 있는 자료형(리스트, 튜플, 문자열 등)을 입력받아 인덱스 값을 포함하는 enumerate 객체를 돌려준다.

```python
>>> for i, name in enumerate(['body', 'foo', 'bar']):
...     print(i, name)
...
0 body
1 foo
2 bar
```

위와 같이 순서 index 와 그 값을 쉽게 알 수 있다.
반복문에 현재 위치(index)를 알고 싶을 때 enumerate 가 유용하다.

## eval

실행 가능한 문자열(`1 + 2` , `'hi' + 'a'` 등)의 입력을 받아 실행한 결괏값을 돌려준다.

```python
>>> eval('1 + 2')
3
>>> eval('divmod(4, 3)')
(1, 1)
```

## filter

`filter` 함수는 첫 번째 인수로 함수 이름을, 두 번째 인수로 그 함수에 들어갈 반복 가능한 자료형을 받는다.
그리고 자료형을 가지고 첫 번째 인수인 함수에 입력되었을 때 반환 값이 참인 것만 묶어(걸러) 반환한다.

```python
def positive(x):
  return x > 0
  
print(list(filter(positive, [1, -3, 2, 0, -5, 6])))
```

위 결과는 `[1, 2, 6]` 으로 필터링되었다.
람다식을 사용하여 더 쉽게 사용할 수 있다.

```python
>>> list(filter(lambda x : x > 0, [1, -3, 2, 0, -5, 6]))
[1, 2, 6]
```

## hex

16진수로 변환하여 반환한다.

```python
>>> hex(234)
0xea
```

## id

객체를 입력받아 주소 값을 돌려준다.

```python
>>> a = 3
>>> id(3)
135072304
>>> id(a)
135072304
>>> b = a
>>> id(b)
135072304
```

## input

사용자의 입력을 받는 함수이다.
매개변수로 문자열을 주면 그 문자열은 프롬프트가 된다.

```python
>>> a = input()
hi
>>> a
'hi'
>>> b = input("Enter: ")
Enter: hi
```

## int

`int(x)` 는 문자열 형태의 숫자나 소수점이 있는 숫자 등을 정수 형태로 돌려준다.

```python
>>> int('3')
3
>>> int(3.4)
3
```

## isinstance

`isinstance(object, class)` 는 첫 번째 인수로 인스턴스, 두 번째 인수로 클래스 이름을 받아, 입력받은 인스턴스가 그 클래스의 인스턴스인지 판단하여 반환한다.

```python
>>> class Person: pass
...
>>> a = Person()
>>> isinstance(a, Person)
True
>>> b = 3
>>> isinstance(b, Person)
False
```

## len

입력받은 인수의 전체 길이 또는 개수를 돌려준다.

```python
>>> len('python')
6
>>> len([1, 2, 3])
3
```

## list

반복 가능한 자료형을 인수로 받아 리스트로 만들어준다.

```python
>>> list("python")
['p', 'y', 't', 'h', 'o', 'n']
>>> list((1,2,3))
[1, 2, 3]
```

## map

`map(f, iterable)` 함수는 함수 f 와 반복 가능한 자료형을 입력받아 자료형의 각 요소를 `f` 함수가 수행한 결과를 묶어 반환해준다.

```python
>>> def two_times(x): 
...     return x*2
...
>>> list(map(two_times, [1, 2, 3, 4]))
[2, 4, 6, 8]
```

이 또한 필터와 같이 람다식을 사용하여 간편하게 이용 가능하다.

```python
>>> list(map(lambda a: a*2, [1, 2, 3, 4]))
[2, 4, 6, 8]
```

## max

인수로 받은 반복 가능한 자료형 중 최댓값을 돌려주는 함수다.

```python
>>> max([1, 2, 3, 4])
4
```

## min

max 와 반대로 최솟값을 반환해준다.

```python
>>> min([1, 2, 3])
1
```

## oct

정수 형태의 숫자를 8진수 문자열로 바꾸어 돌려준다.

```python
>>> oct(34)
0o42
```

## ord

`ord(c)` 인수를 유니코드 값으로 반환해준다.

```python
>>> ord('a')
97
```

## pow

`pow(x, y)` 는 `x` 의 `y` 제곱 결과를 돌려준다.

```python
>>> pow(2, 4)
16
```

## round

`round(number[,ndigits])` 함수는 숫자를 입력받아 반올림해준다.

```python
>>> round(4.6)
5
```

다음과 같이 소수점 반올림할 자릿수를 표시할 수 있다.

```python
>>> round(5.678, 2)
5.68
```

## sorted

`sorted(iterable)` 함수는 입력값을 정렬한 후 리스트로 돌려준다.

```python
>>> sorted([3, 1, 2])
[1, 2, 3]
>>> sorted(["zero"])
['e', 'o', 'r', 'z']
```

## str

인수를 문자열 형태로 반환해준다.

```python
>>> str(3)
'3'
```

## sum

`sum(iterable)` 은 입력받은 리스트나 튜플의 모든 요소 합을 돌려주는 함수다.

```python
>>> sum([1,2,3])
6
```

## tuple

`tuple(iterable)` 은 반복 가능한 자료형을 입력받아 튜플 형태로 바꾸어 반환한다.

```python
>>> tuple("abc")
('a', 'b', 'c')
>>> tuple([1, 2 ,3])
(1, 2, 3)
```

## type

`type(object)` 은 입력값의 자료형이 무엇인지 알려준다.

```python
>>> type("abc")
<class 'str'>
>>> type([])
<class 'list'>
```

## zip

`zip(*iterable)` 은 동일한 개수로 이루어진 자료형을 묶어 주는 역할을 해준다.

```python
>>> list(zip([1, 2, 3], [4, 5, 6]))
[(1, 4), (2, 5), (3, 6)]
>>> list(zip([1, 2, 3], [4, 5, 6], [7, 8, 9]))
[(1, 4, 7), (2, 5, 8), (3, 6, 9)]
>>> list(zip("abc", "def"))
[('a', 'd'), ('b', 'e'), ('c', 'f')]
```
