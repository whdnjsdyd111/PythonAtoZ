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


### dict 일 경우

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
