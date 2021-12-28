## NumPy 배열 속성 지정

```python
import numpy as np
x1 = np.random.randint(10, size=7)
x2 = np.random.randint(10, size=(4,3))
x3 = np.random.randint(10, size=(2,4,3))
# x3.ndim: 3, x3.shape: (2,4,3), x3.size: 24
# x3 dtype: int32
# x3 itemsize: 4, x3 nbytes: 96
```

`ndim`: 차원의 개수

`shape`:각 차원의 크기

`size`: 전체 배열 크기

`dtype`: 배열의 데이터 타입

`itemsize`: 각 배열 요소 크기(바이트)

`nbytes`: 배열의 전체 크기(바이트)

## 배열 인덱싱: 단일 요소에 접근하기

```python
# 인덱싱 사용가능
x1[-2]
# 다차원배열 → 콤마로 구분된 인덱스 튜플 이용
x2[1,2]
```

인덱스 표기법을 이용해 값을 수정 가능

## 배열 슬라이싱: 하위 배열에 접근하기

슬라이스(slice)표기법응 이용해 하위 배열에 접근 가능 - `:` 이용

```python
x[start:stop:step]
# 기본: start=0, stop=차원 크기, step=1

x=np.arange(10)
x[:4] # Out: array([0, 1, 2, 3])
x[7:] # Out: array([7, 8, 9])
x[2:5] # Out: array([2, 3, 4])
x[2::3] # Out: array([2, 5, 8])
#step이 음수일 때
x[::-2] # Out: array([9, 7, 5, 3, 1])
x[7::-2] # Out: array([7, 5, 3, 1])
```

## **다차원 슬라이싱도 다중 슬라이스로 동작**(`,`로 구분)

```python
x2[:2, :3] # Out: array([[3, 5, 2], [7, 6, 8]])
x2[::-1, ::-1] # Out: array([[7, 7, 6, 1], [8, 8, 6, 7], [4, 2, 5, 3]])
```

**배열의 행이나 열에 접근**하기도 슬라이싱을 이용해서 할 수 있다.

```python
print(x2[:,0]) # x2의 첫번째 열
print(x2[0,:]) # x2의 첫번째 행
print(x2[0])   # x2의 첫번째 행 -> 행에 접근하는 경우 빈 슬라이스 생략 가능
```

### 사본이 아닌 뷰로서의 하위 배열

**파이썬 리스트와의 차이점** : copy가 아닌 view를 반환한다.

```python
# 2*2하위 배열 추출
x2_sub = x2[:2,:2]
print(x2_sub)

x2_sub[0,0] = 999
print(x2_sub)
print(x2) # -> x2[0,0]이 999로 바뀜
```

이는 큰 데이터셋을 다룰 때 유용하다.

### 배열의 사본 만들기

`copy()`메서드 이용

```python
x2_sub_copy = x2[:2,:2].copy()
print(x2_sub_copy)

x2_sub_copy[0,0]=213
print(x2_sub_copy)
print(x2) # x2[0,0]은 그대로다
```

## 배열 재구조화

### 배열의 형상 변경 - `reshape()` 메서드 이용

초기 배열 규모와 변경하려는 배열의 규모와 일치해야 가능하다.

```python
grid = np.arange(1,10).reshape((3,3)) # 3*3으로 변경
print(grid)
```

### 슬라이스 연산 내에 `newaxis` 키워드 이용

```python
x = np.array([1,2,3])
# 행 벡터
x[np.newaxis, :] # Out: array([[1, 2, 3]])
# 열 벡터
x[:, np.newaxis] # Out: array([[1], [2], [3]])
```

## 배열 연결 및 분할

### 배열 연결

`np.concatenate` 이용

```python
#1차원배열 연결
x = np.array([1,2,3])
y = np.array([3,2,1])
z = np.array([6,7,8])
np.concatenate([x,y,z]) #Out: array([1, 2, 3, 3, 2, 1, 6, 7, 8])

#2차원배열 연결
a = np.array([[1,2,3],
            [4,5,6]])
#첫번째 축을 따라 연결
np.concatenate([a,a]) 
# Out: array([[1, 2, 3],  [4, 5, 6],  [1, 2, 3],  [4, 5, 6]])
#두번째 축을 따라 연결
np.concatenate([a,a], axis=1)
# Out: array([[1, 2, 3, 1, 2, 3],  [4, 5, 6, 4, 5, 6]])

```

`np.vstack` : vertical stack, 수직 스택 / `np.hstack` : horizontal stack, 수평 스택

```python
x = np.array([1,2,3])
y = np.array([[9,8,7], [6,5,4]])
z = np.array([[98], [76]])

#수직으로 배열 쌓기
np.vstack([x,y])
# Out: array([[1, 2, 3],  [9, 8, 7],  [6, 5, 4]])

#수평으로 배열 쌓기
np.hstack([y,z])
# Out: array([[ 9,  8,  7, 98],  [ 6,  5,  4, 76]])
```

`np.dstack` : 세번째 축을 따라 배열 쌓기

### 배열 분할하기

`np.split`

```python
#분할 지점 알려주는 인덱스 목록 전달
x = [1,2,3,9,9,3,2,1]
x1,x2,x3 = np.split(x,[3,5])
print(x1,x2,x3) # Out: [1 2 3] [9 9] [3 2 1]
```

`np.vsplit`, `np.hsplit`

```python
upper, lower = np.vsplit(x,[2])
print(upper) # Out: [[0 1 2 3] [4 5 6 7]]
print(lower) # Out: [[ 8  9 10 11] [12 13 14 15]]

left, right = np.hsplit(x,[2])
print(left) # Out: [[ 0  1] [ 4  5] [ 8  9] [12 13]]
print(right) # Out: [[ 2  3] [ 6  7] [10 11] [14 15]]
```

`np.dsplit` : 세번째 축을 따라서 배열 분할
