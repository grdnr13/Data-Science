벡터화 연산 이용해서 빠르게 만든다 → 일반적으로 NumPy의 Universal functions(ufuncs) 이용해서 구현

# 루프는 느리다

파이썬 기본 구현에서 몇 가지 연산은 매우 느리다. 동적인 인터프리터 언어이기 때문. 

파이썬은 작은 연산이 반복되는 상황에서 느리다. 예를 들어 주어진 배열의 역수를 계산해야 할 때, 루프의 사이클마다 타입확인과 함수 디스패치에서 병목 현상이 발생해서 연산과 저장에 수 초의 시간이 걸리게 된다. 

# UFuncs 소개

**벡터화 연산**: 빠르다! 여러 종류의 연산에 대해 정적 타입 체계를 가진 컴파일된 루틴에 편리한 인터페이스 제공 ⇒ 배열에 연산을 수행해서 각 요소에 적용

두 배열간 연산도 가능하다

```python
import numpy as np

def compute_reciprocals(values):
    output = np.empty(len(values))
    for i in range(len(values)):
        output[i]=1.0/values[i]
    return output

values = np.random.randint(1,10,size=5)
%timeit (compute_reciprocals(values)) #8.58 µs ± 168 ns per loop
%timeit (1.0/values) #868 ns ± 11.5 ns per loop
```

# NumPy 유니버설 함수(UFuncs)

### 배열 산술 연산

| 연산자 | 대응하는 ufuncs | 설명 |
| --- | --- | --- |
| + | np.add | 덧셈 |
| - | np.subtract | 뺄셈 |
| - | np.negative | 단항 음수 |
| * | np.multiply | 곱셈 |
| / | np.divide | 나눗셈 |
| // | np.floor_divide | floor 나눗셈 |
| ** | np.power | 지수 연산 |
| % | np.mod | 나머지 연산 |

### 절댓값 함수

`np.absolute` aka `np.abs`

```python
x = np.array([-2,-1,0,1,2])
#파이썬 내장 절댓값 함수
abs(x)
#NumPy ufunc 이용
np.absolute(x)
np.abs(x)

#복소수 데이터도 처리가능 -> 절댓값은 크기를 반환
x = np.array([3-4j, 4-3j, 2+0j, 0+1j])
np.abs(x) # Out: array([5., 5., 2., 1.])
```

### 삼각함수

```python
#삼각함수
#각도 배열 정의
theta = np.linspace(0, np.pi, 3) #np.pi는 PI

print("sin(theta) = ", np.sin(theta))
print("cos(theta) = ", np.cos(theta))
print("tan(theta) = ", np.tan(theta))

#역삼각함수
x = [-1,0,1]
print("arcsin(theta) = ", np.arcsin(x))
print("arccos(theta) = ", np.arccos(x))
print("arctan(theta) = ", np.arctan(x))
```

### 지수, 로그

```python
#지수 연산
x = [1,2,3]
print("e^x = ", np.exp(x)) # Out: e^x =  [ 2.71828183  7.3890561  20.08553692]
print("2^x = ", np.exp2(x)) # Out: 2^x =  [2. 4. 8.]
print("3^x = ", np.power(3, x)) # Out: 3^x =  [ 3  9 27]

#로그 연산
x=[1,2,4,8]
print("ln(x) = ", np.log(x))
print("log2(x) = ", np.log2(x))
print("log10(x) = ", np.log10(x))
```

### 특화된 ufuncs

쌍곡선 삼각 함수, 비트연산, 비교연산자 등 다양한 ufuncs 있음. 

`scipy.special` 서브모듈 이용

```python
from scipy import special
#감마함수 관련 함수
x = [1,5,10]
print("gamma(x)     = ", special.gamma(x))
print("ln|gamma(x)| = ", special.gammaln(x))
print("beta(x)      = ", special.beta(x, 2))

#오차함수(가우스적분), 보수와 역수
x = np.array([0,0.3,0.7,1.0])
print("erf(x)    = ", special.erf(x))
print("erfc(x)   = ", special.erfc(x))
print("erfinv(x) = ", special.erfinv(x))
```

# 고급 Ufuncs 기능

### 출력 지정

대규모 연산을 할 때에는 임시 배열을 사용하지 않고, 연산 결과를 저장할 배열을 지정하는 것이 유용할 수 있다. → `out` 인수 사용

```python
x = np.arange(5)
y = np.empty(5)
np.multiply(x,10,out=y) # 출력을 y로 설정

y = np.zeros(10)
np.power(2,x,out=y[::2]) # 배열 뷰와 함께 사용
```

### 집계

배열을 특정 연산으로 축소시키기 → `reduce` 메서드 사용

```python
x = np.arange(1,6)
np.add.reduce(x) # Out: 15
np.multiply.reduce(x) # Out: 120
```

계산의 중간 결과 모두 저장 → `accumulate` 사용

```python
np.add.accumulate(x) # Out: array([ 1,  3,  6, 10, 15], dtype=int32)
np.multiply.accumulate(x) # Out: array([  1,   2,   6,  24, 120], dtype=int32)
```

### 외적 Outer products

서로 다른 두 입력값의 모든 쌍에 대한 출력값 계산 - `outer` 메서드 사용

```python
x = np.arange(1,6)
np.multiply.outer(x,x)
```
