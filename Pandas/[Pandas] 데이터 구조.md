[Python] 데이터 구조
====================

데이터 유형, 인덱싱 및 축 레이블지정/정렬 등에 대한 기본적인 동작은 모든 개체에 적용이 된다.
시작하기 위해서 먼저 판다스 패키지를 가져온다. 학습에 필요를 위해 Numpy 도 불러온다.

```python
import pandas as pd

import numpy as np
```

명심해야 할 것은 데이터 정렬은 고유하다는 것이다.
레이블과 데이터 간의 연결은 사용자가 명시적으로 끊지 않는 한 끊어지지 않는다.

# Series

**series** 는 모든 데이터 유형(정수, 문자열, 부동 소수점 숫자, 파이썬 객체 등)을 보유할 수 있는 1차원 레이블 배열이다.
축 레이블을 집합적으로 **인덱스**라 한다.

시리즈를 생성하는 기본적인 함수는 다음과 같다.

```python
s = pd.Series(data, index=index)
```

위에서 `data` 는 다음과 같이 다양하게 인수로 줄 수 있다.

- 딕셔너리
- ndarray (Numpy 의 n 차원 배열)
- 스칼라 값

`index` 는 축 레이블 목록이다. 따라서 인덱스는 데이터가 무엇인지에 따라 여러가지 경우로 나뉜다.

### ndarray 일 경우

`data` 가 `ndarray` 일 때, 인덱스는 데이터와 같은 길이어야 한다.
인수로 인덱스를 전달하지 않았다면, 값이 있는 기본적인 인덱스가 생성된다. (0, 1, ... len(data) - 1)

```python
>>> s = pd.Series(np.random.random(5), index=["a", "b", "c", "d", "e"])
>>> s
a    0.860941
b    0.062698
c    0.019142
d    0.619229
e    0.896744
dtype: float64
>>> s.index
Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
>>> pd.Series(np.random.random(5))
0    0.223832
1    0.744169
2    0.335417
3    0.329441
4    0.935291
dtype: float64
```

판다스는 고유하지 않은 인덱스 값을 지원하기 때문에 중복 인덱스 값을 허용하지 않는 작업에 대해 조심할 필요가 있다.
이유는 성능 기반으로 진행하기 위함이다.


### 딕셔너리일 경우

Series 는 ditcs 로 인스턴스화할 수 있다.

```python
>>> d = {"b": 1, "a": 0, "c": 2}
>>> pd.Series(d)
b    1
a    0
c    2
dtype: int64
```

인덱스가 전달되면 레이블에 매칭되는 것이 없을 경우 누락된 데이터(`NaN`)로 초기화된다.

```python
>>> d = {"a": 0.0, "b": 1.0, "c": 2.0}
>>> pd.Series(d)
a    0.0
b    1.0
c    2.0
dtype: float64
>>> pd.Series(d, index=["b", "c", "d", "a"])
b    1.0
c    2.0
d    NaN
a    0.0
dtype: float64
```

### 스칼라일 경우

`data` 가 스칼라 값이면 인덱스를 제공해야 한다.
값은 인덱스 길이와 일치하도록 반복된다.

```python
>>> pd.Series(5.0, index=["a", "b", "c", "d", "e"])
a    5.0
b    5.0
c    5.0
d    5.0
e    5.0
dtype: float64
```

## Series 는 ndarray 와 유사

`Series` 는 `ndarray` 와 매우 유사하게 작동하고 대부분의 Numpy 함수를 인수로 사용 가능하다.
하지만, 슬라이싱 같은 작업은 인덱스도 가능하다.

```python
>>> s[0]
0.4691122999071863
>>> s[:3]
a    0.469112
b   -0.282863
c   -1.509059
dtype: float64
>>> s[s > s.median()]
a    0.469112
e    1.212112
dtype: float64
>>> s[[4, 3, 1]] # 추후 인덱싱 섹션에서 다룸
e    1.212112
d   -1.135632
b   -0.282863
dtype: float64
>>> np.exp(s)
a    1.598575
b    0.753623
c    0.221118
d    0.321219
e    3.360575
dtype: float64
```

넘파이 배열처럼 판다스 시리즈도 `dtype` 을 가진다.

```python
>>> s.dtype
dtype('float64')
```

이는 일반적으론 Numpy 의 `dtype` 이다. 하지만 판다스나 타 라이브러리는 Numpy 의 유형의 시스템에서, `dtype` 은 `ExtensionDtype` 이 될 수도 있다. Pandas 를 예로들면 `Categorical` 범주형 데이터와 `Nullable integer` 널 입력 가능한 정수형 데이터 타입이 있다.

`Series` 를 지원하는 실제 배열이 필요하다면, `Series.array` 를 사용하자.

```python
>>> s.array
<PandasArray>
[ 0.4691122999071863, -0.2828633443286633, -1.5090585031735124,
 -1.1356323710171934,  1.2121120250208506]
Length: 5, dtype: float64
```

위의 경우는 인덱스 없이 일부 작업만 수행할 때 유용하다.

`Series.array` 는 항상 `ExtensionArray` 가 될 것이다. 쉽게말하면, `ExtensionArray` 는 하나 또는 그 이상의 `numpy.ndarray` 같은 구체적인 배열을 둘러싼 얇은 래퍼이다.
pandas 를 통해 `ExtensionArray` 를 사용하거나 `Dataframe` 의 한 열이나 `Series` 에 저장할 수 있다.

`Series` 는 `ndarray` 와 유사하긴 하지만 실제 `ndarray` 가 필요하면 `Series.to_numpy()` 를 사용해야 한다.

```python
>>> s.to_numpy()
array([ 0.4691, -0.2829, -1.5091, -1.1356,  1.2121])
```

`Series` 가 `ExtensionArray` 가 지원되더라도 `Series.to_numpy()` 는 Numpy 의 `ndarray` 를 반환한다는 것을 명심하자.

## Series 는 dict 와 유사

`Series` 는 인덱스 라벨로 값을 얻거나 설정한다는 것에 고정된 크기인 딕셔너리와 유사하다.

```python
>>> s["a"]
0.4691122999071863

>>> s["e"] = 12.0
>>> s 
a     0.469112
b    -0.282863
c    -1.509059
d    -1.135632
e    12.000000
dtype: float64

>>> "e" in s
True

>>> "f" in s
False
```

포함되어 있지 않은 라벨에 접근하면 에러가 발생한다.

```python
>>> s["notkey"]
KeyError: 'f'
```

이럴 땐, `get` 메소드를 사용하여 None 이나 명시된 기본 값을 얻을 수 있다.

```python
>>> s.get("f")

>>> s.get("f", np.nan)
nan
```

## Series 를 통한 벡터화된 작업 및 라벨 정렬

Numpy 배열을 작업할 때, 값으로 루핑하는 것은 필수가 아니다.
pandans 에서 `Series` 로 작업할 때도 마찬가지 이며, Series 는 `ndarray` 에 대한 대부분의 Numpy 함수도 사용가능하다.

```python
>>> s + s
a     0.938225
b    -0.565727
c    -3.018117
d    -2.271265
e    24.000000
dtype: float64

>>> s * 2
a     0.938225
b    -0.565727
c    -3.018117
d    -2.271265
e    24.000000
dtype: float64

>>> np.exp(s)
a         1.598575
b         0.753623
c         0.221118
d         0.321219
e    162754.791419
dtype: float64
```

`Series` 와 `ndarray` 간의 큰 차이점은, `Series` 는 레이블을 기반으로 하여 데이터를 자동으로 정렬한다는 것이다.
그래서 관련된 Series 에 동일한 레이블이 있는지 여부를 고려하지 않고 계산을 작성이 가능하다.

```python
>>> s[1:] + s[:-1]
a         NaN
b   -0.565727
c   -3.018117
d   -2.271265
e         NaN
dtype: float64
```

 정렬되지 않은 시리즈간의 작업 결과는 관련한 인덱스의 합집합일 것이다.
 한 시리즈나 다른 시리즈에서 레이블을 찾을 수 없다면 결과가 누락된 것(`NaN`)으로 표시된다.
 데이터 정렬을 수행하지 않고 코드를 작성할 수 있다는 점에서 데이터 분석 및 연구에서 큰 자유도와 유연성을 부여한다.
 
 pandas 는 일반적으로 정보 소실을 방지하기 위해 다른 인덱싱된 개체 간에는 기본적으로 합집합이 되도록 했다.
 
 ## 이름 속성
 
 Series 에는 다음과 같은 `name` 속성도 있다.
 
 ```python
>>> s = pd.Series(np.random.randn(5), name="something")
>>> s
0   -0.494929
1    1.071804
2    0.721555
3   -0.706771
4   -1.039575
Name: something, dtype: float64

>>> s.name
'something'
```

Series 의 `name` 은 `DataFrame` 의 한 조각을 가져올 때 자동적으로 할당된다.

Series 의 `name` 은 `rename()` 함수로 이름을 다시 작성할 수 있다.

```python
>>> s2 = s.rename("different")
>>> s2.name
'different'
```

위에서 `s` 와 `s1` 는 서로 다른 객체를 참조한다.

# DataFrame

`DataFrame` 은 다른 유형의 열이 있는 2차원 레이블 데이터 구조이다.
스프레드시트나 SQL 테이블, 그리고 Series 개체의 딕셔너리와 비슷하다고 할 수 있다.
가장 일반적으로 사용되는 객체이며, `Series` 처럼 다양한 종류의 입력을 허용한다.

- `ndarray`, list, dict 나 Series 의 딕셔너리
- 2차원 numpy.ndarray
- 구조체 ndarray 나 레코드 ndarray
- Series
- 다른 DataFrame

인덱스와 컬럼은 선택적으로 달 수 있으며, 인덱스/컬럼을 달았다면 DataFrame 결과에 대한 인덱스 컬럼을 받을 수 있다. 그래서 Series 딕셔너리 와 특정 인덱스는 입력받은 인덱스와 일치하지 않는 모든 데이터를 버린다.

## Series 딕셔너리 또는 딕셔너리 경우

인덱스 결과 값은 다양한 Series 의 인덱스들의 합집합일 수 있다.
중첩된 딕셔너리가 존재한다면, 먼저 Series 로 변환시킨다.

전달받은 컬럼이 없다면, dict 키 값을 리스트로 정렬하여 컬럼으로 지정한다.

```python
>>> d = {
...     "one": pd.Series([1.0, 2.0, 3.0], index=["a", "b", "c"]),
...     "two": pd.Series([1.0, 2.0, 3.0, 4.0], index=["a", "b", "c", "d"]),
... }

>>> df = pd.DataFrame(d)
   one  two
a  1.0  1.0
b  2.0  2.0
c  3.0  3.0
d  NaN  4.0

>>> pd.DataFrame(d, index=["d", "b", "a"])
   one  two
d  NaN  4.0
b  2.0  2.0
a  1.0  1.0

>>> pd.DataFrame(d, index=["d", "b", "a"], columns=["two", "three"])
   two three
d  4.0   NaN
b  2.0   NaN
a  1.0   NaN
```

로우와 컬럼 레이블은 인덱스와 컬럼 속성으로 각각을 접근할 수 있다.

특정 컬럼 집합이 딕셔너리 와 함께 전달되면 컬럼은 딕셔너리 키로 재정의한다.

```python
>>> df.index
Index(['a', 'b', 'c', 'd'], dtype='object')

>>> df.columns
Index(['one', 'two'], dtype='object')
```

## ndarray 딕셔너리 나 배열일 경우

인덱스를 인수로 전달하면 배열 길이와 동일해야 한다.
인덱스를 넣지 않으면 `range(배열길이)` 로 초기화 된다.

```python
>>> d = {"one": [1.0, 2.0, 3.0, 4.0], "two": [4.0, 3.0, 2.0, 1.0]}
>>> pd.DataFrame(d)
   one  two
0  1.0  4.0
1  2.0  3.0
2  3.0  2.0
3  4.0  1.0

>>> pd.DataFrame(d, index=["a", "b", "c", "d"])
   one  two
a  1.0  4.0
b  2.0  3.0
c  3.0  2.0
d  4.0  1.0
```

## 구조체 배열 또는 레코드 배열일 경우

이 경우에는 배열의 딕셔너리와 동일하게 처리된다.

```python
>>> data = np.zeros((2,), dtype=[("A", "i4"), ("B", "f4"), ("C", "a10")])
>>> data[:] = [(1, 2.0, "Hello"), (2, 3.0, "World")]
>>> pd.DataFrame(data)
   A    B         C
0  1  2.0  b'Hello'
1  2  3.0  b'World'

>>> pd.DataFrame(data, index=["first", "second"])
        A    B         C
first   1  2.0  b'Hello'
second  2  3.0  b'World'

>>> pd.DataFrame(data, columns=["C", "A", "B"])
          C  A    B
0  b'Hello'  1  2.0
1  b'World'  2  3.0
```

DataFrame 은 2차원 Numpy ndarray 처럼 정확하게 작동하지 않는 다는 것을 명심하자.

##  리스트일 경우

```python
>>> data2 = [{"a": 1, "b": 2}, {"a": 5, "b": 10, "c": 20}]
>>> pd.DataFrame(data2) 
   a   b     c
0  1   2   NaN
1  5  10  20.0

>>> pd.DataFrame(data2, index=["first", "second"])
        a   b     c
first   1   2   NaN
second  5  10  20.0

>>> pd.DataFrame(data2, columns=["a", "b"])
   a   b
0  1   2
1  5  10
```

## 튜플 딕셔너리일 경우

튜플 딕셔너리을 인수로 넣는다면 자동적으로 멀티 인덱스를 생성할 수 있다.

```python
>>> pd.DataFrame(
...     {
...         ("a", "b"): {("A", "B"): 1, ("A", "C"): 2},
...         ("a", "a"): {("A", "C"): 3, ("A", "B"): 4},
...         ("a", "c"): {("A", "B"): 5, ("A", "C"): 6},
...         ("b", "a"): {("A", "C"): 7, ("A", "B"): 8},
...         ("b", "b"): {("A", "D"): 9, ("A", "B"): 10},
...     }
... )
       a              b      
       b    a    c    a     b
A B  1.0  4.0  5.0  8.0  10.0
  C  2.0  3.0  6.0  7.0   NaN
  D  NaN  NaN  NaN  NaN   9.0
```

## Series 일 경우

`Series` 를 인수로 넣었을 경우, 시리즈와 인덱스가 동일하고, 이름이 시리즈의 기존 이름을 가지는 컬럼 하나가 있는 DataFrame 이 된다. (단, 다른 컬럼 이름이 제공되지 않았을 때만)

## namedtuple 배열일 경우

`namedtuple` 배열에서 첫 번째 요소의 필드 이름으로 DataFrame 의 컬럼을 결정한다.
필드 이름으로 지정된 `namedtuple` (첫 번째)보다 짧은 `namedtuple` 에 대해서는 누락된 데이터로 표시되지만, 더 길다면 `ValueError` 에러를 발생시킨다.

```python
>>> from collections import namedtuple

>>> Point = namedtuple("Point", "x y")
>>> pd.DataFrame([Point(0, 0), Point(0, 3), (2, 3)]) 
   x  y
0  0  0
1  0  3
2  2  3

>>> Point3D = namedtuple("Point3D", "x y z")
>>> pd.DataFrame([Point3D(0, 0, 0), Point3D(0, 3, 5), Point(2, 3)])
   x  y    z
0  0  0  0.0
1  0  3  5.0
2  2  3  NaN
```construct 

## dataclass 배열일 경우

`dataclass` 는 DataFrame 생성자의 인수로 전달할 수 있다.
전달한 `dataclass` 배열은 딕셔너리 배열을 전달한 것과 같다.

배열의 모든 값은 `dataclass` 여야 하며, 배열 요소의 타입이 혼합되어 있으면 `TypeError` 에러가 발생한다.

```python
>>> from dataclasses import make_dataclass

>>> Point = make_dataclass("Point", [("x", int), ("y", int)])
>>> pd.DataFrame([Point(0, 0), Point(0, 3), Point(2, 3)])
   x  y
0  0  0
1  0  3
2  2  3
```

### 데이터 누락

누락된 데이터는 추후에 자세히 설명할 것이며, DataFrame 을 구성하기 위해서 누락된 데이터는 보통 `np.nan` 을 사용한다. 이에 대한 대안으로, `numpy.MaskedArray` 를 사용할 수도 있다.

## 대체 생성자

### DataFrame.from_dict

`DataFrame.from_dict` 는 배열과 비슷한 시퀀스 딕셔너리나 딕셔너리-딕셔너리를 사용하여 DataFrame 을 반환한다. 이는 디폴트 매개변수인 `orient` 를 제외하고는 DataFrame 생성자와 동일하게 작동한다. 그러나 `index` 가 입력되면 딕셔너리 키를 행 레이블로 설정하게 된다.

```python
>>> pd.DataFrame.from_dict(dict([("A", [1, 2, 3]), ("B", [4, 5, 6])])) 
   A  B
0  1  4
1  2  5
2  3  6
```

`orient='index'` 를 전달하면, 키는 행 레이블이 될 것이다.
이 경우에, 원하는 열 이름을 전달할 수도 있다.

```python
>>> pd.DataFrame.from_dict(
...     dict([("A", [1, 2, 3]), ("B", [4, 5, 6])]),
...     orient="index",
...     columns=["one", "two", "three"],
... )
   one  two  three
A    1    2      3
B    4    5      6
```

### DataFrame.from_records

`DataFrame.from_records` 는 튜플 리스트나 구조체 자료형을 사용하는 `ndarray` 로 생성할 수 있다.
이는 DataFrame 인덱스가 구조체 자료형의 명시된 필드일 수도 있다는 것을 제외하면 기본적인 DataFrame 처럼 작동된다.

```python
>>> data
array([(1, 2., b'Hello'), (2, 3., b'World')],
      dtype=[('A', '<i4'), ('B', '<f4'), ('C', 'S10')])

>>> pd.DataFrame.from_records(data, index="C")
          A    B
C               
b'Hello'  1  2.0
b'World'  2  3.0
```

## 컬럼 선택, 추가, 삭제

DataFrame 은 인덱스가 유사한 Series 객체의 딕셔너리처럼 의미적으로 취급이 가능하다.
컬럼 가져오기, 세팅, 삭제는 딕셔너리 작업과 유사하게 동일한 구문으로 작동한다.

```python
>>> df["one"]
a    1.0
b    2.0
c    3.0
d    NaN
Name: one, dtype: float64

>>> df["three"] = df["one"] * df["two"]

>>> df["flag"] = df["one"] > 2

>>> df
   one  two  three   flag
a  1.0  1.0    1.0  False
b  2.0  2.0    4.0  False
c  3.0  3.0    9.0   True
d  NaN  4.0    NaN  False
```

딕셔너리처럼 열을 삭제하거나 편집도 가능하다.

```python
>>> del df["two"]

>>> three = df.pop("three")

>>> df
   one   flag
a  1.0  False
b  2.0  False
c  3.0   True
d  NaN  False
```

스칼라 값을 삽입하면 해당 값으로 열을 채우게 된다.

```python
>>> df["foo"] = "bar"

>>> df
   one   flag  foo
a  1.0  False  bar
b  2.0  False  bar
c  3.0   True  bar
d  NaN  False  bar
```

DataFrame 과 동일한 인덱스가 없는 Series 를 삽입할 때, DataFrame 의 인덱스를 따르게 된다.

```python
>>> df["one_trunc"] = df["one"][:2]

>>> df
   one   flag  foo  one_trunc
a  1.0  False  bar        1.0
b  2.0  False  bar        2.0
c  3.0   True  bar        NaN
d  NaN  False  bar        NaN
```

원시 `ndarray` 를 삽입할 수 있지만 길이는 DataFrame 인덱스와 일치해야 한다.

기본적으로 열은 끝에 추가로 삽입되며, `insert` 함수로 특정 위치에 삽입할 수 있다.

```python
>>> df.insert(1, "bar", df["one"])

>>> df
   one  bar   flag  foo  one_trunc
a  1.0  1.0  False  bar        1.0
b  2.0  2.0  False  bar        2.0
c  3.0  3.0   True  bar        NaN
d  NaN  NaN  False  bar        NaN
```

## 메서드 체인에 새 컬럼 할당

DataFrame 은 파생된 새로운 열을 쉽게 만들 수 있는 `assign()` 함수가 있다.

```python
>>> iris = pd.read_csv("data/iris.data")

>>> iris.head()
   SepalLength  SepalWidth  PetalLength  PetalWidth         Name
0          5.1         3.5          1.4         0.2  Iris-setosa
1          4.9         3.0          1.4         0.2  Iris-setosa
2          4.7         3.2          1.3         0.2  Iris-setosa
3          4.6         3.1          1.5         0.2  Iris-setosa
4          5.0         3.6          1.4         0.2  Iris-setosa

>>> iris.assign(sepal_ratio=iris["SepalWidth"] / iris["SepalLength"]).head()
   SepalLength  SepalWidth  PetalLength  PetalWidth         Name  sepal_ratio
0          5.1         3.5          1.4         0.2  Iris-setosa     0.686275
1          4.9         3.0          1.4         0.2  Iris-setosa     0.612245
2          4.7         3.2          1.3         0.2  Iris-setosa     0.680851
3          4.6         3.1          1.5         0.2  Iris-setosa     0.673913
4          5.0         3.6          1.4         0.2  Iris-setosa     0.720000
```

위 예제에서 계산된 값을 삽입했다. 람다식을 전달하여 쉽게 할당할 수도 있다.
`iris.assign(sepal_ratio=lambda x: (x["SepalWidth"] / x["SepalLength"])).head()

`assign` 함수는 항상 데이터 복사본을 반환하고 원본은 그대로 둔다.
