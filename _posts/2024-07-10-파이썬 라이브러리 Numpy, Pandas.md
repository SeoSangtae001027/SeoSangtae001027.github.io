---
title: "파이썬 라이브러리 Numpy, Pandas"
date: 2024-07-10 19:00:00 +09:00 
# last_modified_at:
categories: [패스트캠퍼스, 데이터분석]
tags: [Python, Numpy, Pandas]
---

## Numpy 라이브러리

파이썬 프로그래밍에서 수치 계산을 효율적으로 처리하기 위해 사용되는 라이브러리이다.

## Numpy 코드

```python
import numpy as np 

arr = np.array([['1','2','3'], ['4','5','6']])

arr.ndim # 2차원 
arr.shape # 2행 3열
arr.dtype # int64

arr.astype(np.float64) # 타입변경
```

넘파이를 이용한 행렬 생성 및 차원, 구조, 타입 파악에 대한 코드이다.

```python
arr = np.arange(10)

arr[0:4]

# 6보다 큰 값만 표현하시오.
arr > 6 # Boolean Indexing: True, False

arr[arr > 6]
```

불리언 인덱싱을 통한 원하는 데이터 추출 방법이다.

```python
arr = np.arange(10)

arr.reshape(2,5)
arr.reshape(5,2)
# arr1.reshape(3,5)

arr = np.array([30, 100, 90, 50, 10])

new_arr = np.sort(arr)[::-1]

new_arr
```

배열의 구조 변경과 정렬에 대한 코드이다. 데이터 형식이 수치형이 아니더라도 정렬은 가능하다.

## Pandas 라이브러리

데이터 분석과 조작을 용이하게 해주는 파이썬 라이브러리이다.

## Pandas 코드

```python
data = ['A', 'B', 'C', 'D', 'E']

se = pd.Series(data)

se.index
se.values

se[0:3]

se.name = 'alphabet'
se.index.name = 'No.'
se
```
Series는 1개의 컬럼으로 구성된 1차원 데이터 셋이다.

```python
data = {
    'country' : ['kor', 'usa', 'china', 'japan'],
    'rank' : [1,2,3,4],
    'grade' : ['A', 'B', 'C', 'D']
}

df = pd.DataFrame(data)

type(df)
df

# df.loc(인덱스값, 컬럼명) => 좌표값 입력
df.loc[2, ['country', 'grade']]
df.loc[:, ['country', 'grade']] 


# boolean indexing
# | 또는, & 그리고
df[(df['rank'] >= 2) & (df['grade'] == 'B')]

# filter()
df.filter(like='a', axis=1) # 컬럼명을 기준으로 c가 포함된 컬럼을 출력해줘
df.filter(like='a', axis=0) # 인덱스값을 기준으로 c가 포함된 행데이터를 출력해줘
```
데이터프레임은 데이터를 다루기 가장 편리한 형태로 CRUD 작업을 진행할 수 있다.
