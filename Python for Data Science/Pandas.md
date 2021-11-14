# Pandas

```python
import pandas as pd
```

Series: 열 하나

DataFrame: 시리즈 여러개로 만들어진 다차원 테이블

## DataFrame

### DataFrame 만들기

- dictionary 이용

  ```python
  data = {
      'ages': [19, 22, 21, 25]
      'heights': [173, 156, 162, 167]
  }
  df = pd.DataFrame(data)
  ```

  - 각 key가 열에 해당

- DataFrame은 자동으로 숫자 인덱스를 만든다. 인덱스를 마음대로 만들 수도 있음.

  ```python
  df = pd.DataFrame(data, index=['JW', 'MK', 'MS', 'SY'])
  print(df.loc["MK"])
  ```

  - `loc[]`함수 이용해서 인덱스에 접근 가능

### Indexing & Slicing

- `[]`을 이용해서 열 하나 또는 여러개를 선택할 수 있다. (결과물- 하나일때: Series 객체 / 여러개일때: DataFrame)

  ```python
  print(df["heights", "ages"])
  ```

- `iloc`을 이용해서 숫자 인덱스로 데이터를 선택할 수 있다. 파이썬의 리스트 인덱싱과 동일하게 동작

  ```python
  print(df.iloc[1]) #second row
  print(df.iloc[:2]) #0~1 row
  ```

- 조건 이용해서 데이터를 선택하는 것도 가능하다.

  ```python
  df[(df['ages']>20) & df['heights']>160]
  ```

  

### 데이터 읽기

- Pandas에서는 CSV(comma-separated values)에서 데이터를 읽어올 수 있도록 지원해준다. 

  ```python
  df = pd.read_csv("ewha_students.csv")
  ```

- 앞에서부터 데이터 읽기: `head()`

  ```python
  print(df.head()) #첫줄읽기
  print(df.head(3)) #앞에서부터 3행 읽기
  ```

- 뒤에서부터 데이터 읽기: `tail()`

  ```python
  print(df.tail(20))
  ```

- 정보 읽어오기: `info()` - 행과 열의 개수, 데이터타입 등

  ```python
  df.info()
  ```

- Pandas에서 자동으로 인덱스 만들어준다. 데이터값에서 인덱스를 사용하고싶다면 `set_index()`를 이용하자

  ```python
  df.set_index("number", implace=True)
  ```

- 데이터값 삭제하기

  ```python
  df.drop('school', axis=1, implace=True)
  #axis=0 이면 row 삭제 / axis=1이면 column 삭제
  ```



### 데이터 이용해서 작업하기

- 열을 추가할 수도 있다.

  ```python
  #'month'라는 열을 'date'열(DD.MM.YY)을 기준으로 만든다
  df['month'] = pd.to_datetime(df['date'], format="%d.%m.%y").dt.month_name()
  ```

- 모든 numeric columns에 대한 통계: `describe()`함수 이용

  ```python
  df.describe()
  ```

  std, mean, min, max와 같은 통계를 알려준다.



### Grouping

- 열에서 각 카테고리의 빈도수를 알 수 있다 : `value_counts()`

  ```python
  #각 월마다 몇개의 값을 가지는지
  df['month'].value_counts()
  ```

- 열을 이용해서 dataset을 그룹화할 수 있다: `groupby()`

  ```python
  #월별 케이스 수를 계산
  df.groupby('month')['cases'].sum()
  ```

  
