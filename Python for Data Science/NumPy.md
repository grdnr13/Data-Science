# NumPy

```python
import numpy as np
```



## NumPy Array

- 일반 리스트보다 빠르고 compact하다

- 리스트를 인자로 받아서 만들 수 있다.

  ```python
  x = np.array([1,2,3,4,5])
  ```

- 0 부터 인덱싱

- aka **ndarrays** or **N-dimensional array** (다차원 배열 가능)

### Properties

`array.ndim` : 몇차원 배열인가

`array.size` : 요소의 개수

`array.shape` : 각 차원에 저장된 요소의 수를 나타내는 정수들이 담긴 tuple 반환

### 요소 이용

```python
x = np.array([8,4,7])
#요소 추가
x = np.append(x,5)
#인덱스 제거
x = np.delete(x,4)
#정렬
x = np.sort(x)
```

#### 배열 만들기

```python
x = np.arange(startNum, endNum, interval)
```

### 배열 크기 재설정 : `reshape()`

```python
x = np.arange(1,7)
z = x.reshape(2,3)
y = z.reshape(6)
```

### 인덱싱, Slicing

- 일반 파이썬 배열과 동일

- 조건문으로도 가능

  ```python
  x = np.arange(1,13)
  print(x[x<4])
  printf(x[(x>3) and (x%2==0)])
  ```

  

### 연산

- 배열에서 기본 수학적 연산 가능하다. (`sum`,` min`,` max` 등)

  ```python
  x = np.arange(1,15)
  print(x.sum())
  ```

- 일반 숫자와 배열 간에 연산도 가능하다. 

  ```python
  x = np.arange(1,5)
  y = x*3
  #y = [3,6,9,12]
  ```

- 통계적 연산도 가능하다 

  - mean: `np.mean(x)`
  - median: `np.median(x)`
  - variance: `np.var(x)`
  - standard deviation: `np.std(x)`
