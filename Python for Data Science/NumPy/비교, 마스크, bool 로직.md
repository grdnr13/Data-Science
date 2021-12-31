# ufuncs으로서의 비교 연산자

```python
x = np.array([1,2,3,4,5])
x<3  # Out: array([ True,  True, False, False, False])
x!=3 # Out: array([ True,  True, False,  True,  True])

#두 배열을 항목별로 비교할 수도 있다
(3*x) == (x**2) # Out: array([False, False,  True, False, False])
```

| 연산자 | 대응하는 ufuncs |
| --- | --- |
| == | np.equal |
| ≠ | np.not_equal |
| < | np.less |
| ≤ | np.less_equal |
| > | np.greater |
| ≥ | np.greater_equal |

모든 크기와 형상의 배열에 적용된다. 결과물 = bool 배열

# bool 배열으로 작업하기

### 요소 개수 세기

True 개수 세기

```python
# x = [[5 0 3 3]
#      [7 9 3 5]
#      [2 4 7 6]]

# 1. np.count_nonzero 이용
np.count_nonzero(x<5) # Out: 6

# 2. np.sum 이용 -> True=1, False=0
np.sum(x<5) # Out: 6
#sum을 이용하면 다른 NumPy 집계 함수와 함께 행/열 따라 계산 가능
np.sum(x<5, axis=1) # Out: array([3, 1, 2])
```

파이썬 내장함수 사용하면 의도하지 않은 결과가 나올수도 있음. np이용

### bool 연산자

비트 단위 로직 연산자 `&`, `|`, `^`, `~` 사용가능

```python
np.sum((x>1) & (x<5)) # Out: 5
np.sum(~((x<=1)|(x>=5))) # Out: 5
```

| operator | ufunc |
| --- | --- |
| `&` | np.bitwise_and |
| `\|` | np.bitwise_or |
| `^` | np.bitwise_xor |
| `~` | np.bitwise_not |

# 마스크로서의 bool 배열

bool 배열을 마스크로 사용해서 데이터의 특정 부분집합을 선택할 수 있다.

bool 배열을 인덱스로 사용하면 마스킹 연산을 할 수 있다.

```python
x[x<5] # Out: array([0, 3, 3, 3, 2, 4])
```
