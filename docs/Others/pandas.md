---
layout: default
title: Pandas
parent: Others
nav_order: 1
has_children: true
has_toc: true
---

# Pandas
Table 데이터를 다루는 Python 라이브러리

## FIle read, load

```python
df = pd.read_csv(
	"data/movie.csv",
	index_col=0 #첫번째 컬럼을 인덱스로 사용
	index_col='이름' #'특정 컬럼을 인덱스로 사용
)

df = pd.to_csv(
	"data/movie.csv",
	sep="\t",
	skiprow=[1,3]	#특정 행을 제외 후 바로 다음 행이 header
	skiprow=3 #지정된 갯수만큼 제외
	nrows=4 #원하는 갯수만큼
)
```

- csv, xlsx, html, text 등등 여러가지 포멧으로 사용 가능

## Series

단일 행 데이터를 관리하는 클래스

- index - 각 데이터 값에 연결된 label, 직접 설정하지 않으면 0부터 양수의 숫자로 자동 설정된다. 리스트의 index같은 순번의 개념이 아니라 dictionary의 key와 비슷한 개념이다. 그러나 dictionary와는 다르게 key가 중복될 수 있다.
- numpy의 Ndarray로 구성되어있다 → 벡터화 연산 지원
- 단일 데이터 타입만을 가질 수 있다. 만약 [1, ‘ㄱ’, ‘ㄴ’, 2, ‘ㄹ’] 이런식의 숫자/문자로 구성되어있다면 object라는 타입으로 취급한다 → 이는 연산에서 성능이 저하 될 수 있음.

### 시리즈 생성

```python
test_series = pd.Series(
		[70, 100, 80, 95, 75],
		index=['국어', '국어', '수학', '과학', '국사'] #기본 인덱스 이름 (0~)대신 다른 인덱스 이름을 사용
)
```

### 데이터 조회

```python
test_series[0] # 순번으로 조회
test_series['국어'] #인덱스 label로 조회
```

### 데이터 변경

```python
test_series.index = pd.Index(list("가나다라마바사"))
test_series.index = list("가나다라마바사") #가능은 하지만 타입체크에서 error
```

```python
test_series.rename({'국어' : "Korean"}, inplace=True) # 국어 -> 'Korean'으로 변경
```

- inplace옵션: 원본데이터 자체를 수정, 기본값은 새로운 데이터 반환

## DataFrame(df)

```python
# 임시 데이터 (2차원 구조의 형태)
data = {
    '이름' : ['채치수', '정대만', '송태섭', '서태웅', '강백호', '변덕규', '황태산', '윤대협'],
    '학교' : ['북산고', '북산고', '북산고', '북산고', '북산고', '능남고', '능남고', '능남고'],
    '키' : [197, 184, 168, 187, 188, 202, 188, 190],
    '국어' : [90, 40, 80, 40, 15, 80, 55, 100],
    '영어' : [85, 35, 75, 60, 20, 100, 65, 85],
    '수학' : [100, 50, 70, 70, 10, 95, 45, 90],
    '과학' : [95, 55, 80, 75, 35, 85, 40, 95],
    '사회' : [85, 25, 75, 80, 10, 80, 35, 95],
    'SW특기' : ['Python', 'Java', 'Javascript', '', '', 'C', 'PYTHON', 'C#']
}
```

### DataFrame 생성

```python
df = pd.DataFrame(
	data, 
	index=['1번', '2번', '3번', '4번', '5번', '6번', '7번', '8번']
)

df = pd.DataFrame(
	data, 
	columns=['이름', '학교', '키'] #특정 컬럼만 가지고 DataFrame생성
)
```

### 데이터 조회

```python
df['이름']
df[['수학', '과학']] # 수학,과학 2가지 컬럼가져오기
df.이름
df.drop_duplicates(subset=['c1', 'c2']) #중복 제거 후 조회
df.select_dtypes(include=['object', 'bool']) # object, bool 타입의 컬럼만 조회
df.filter(like="math") #컬럼명에 math가 포함된 컬럼만 조회
df.head() # default 5: 0~N개의 row조회
df.tail() # default 5: -1~N개의 row 조회
df.columns # 컬럼 명 조회
df.index #index 명 조회
df.values #값만 조회
df['이름'].str.lower() #소문자로 변경 후 조회

#filtering
df[df['키' > 160]]
df[~df['키' > 160]] #~: not
df[df['이름'].str.startwith('박')]
df[~df['이름'].str.contains('박')] # 이름에 '박'이 포함되지 않은 사람
df[df['이름'].isin(['강백호', '송태섭'])] #list에 속한 데이터

```

### DataFrame index

```python
# index의 이름 변경
df.index.name = '지원순서'
# index 삭제
df.reset_index(drop=True)
# 인덱스 설정
df.set_index('이름')
# 인덱스를 기준으로 정렬
df.sort_index(ascding=False) #ascding
```

### 데이터 집계

```python
df.describe() # 계산가능한 데이터에 대한 여러 집계
df.info() #컬럼별 데이터 타입, 데이터 수량, 메모리 사용량 등 정보 조회
df.shape # 전체 rows, columns 수량 조회
df.min() #최소값
df.max() #최대값
df.mean(numeric_only=True) #평균 numeric_only: 계산이 가능한 값만
df.sum() #합계
df.isna() #null인지 아닌지
df.count() #유효 데이터 수
df.unique() #유니크한 데이터만 조회
df.median() #산술 데이터를 갖는 모든 열의 중앙값
df.std() # 표준편차
df.corr() #모든 열에 대하여 2개씩 짝을짓고 각 경우에 대한 상관계수 계산
```

### Location/ index Location

```python
df.loc[*row_selector, column_selector*]
df.lov[df['키'] > 180, '이름']
df.loc[(df['키'] > 180) & (df['수학'] > 80), '이름'] # &: and, |: or, ~: not
```

### 결측치

```python
df.fillna('결측') #na값을 특정값으로 채움
df.isna() # 결측치는 True로 표현
df.dropna(axis='index') # 결측치가 포함된 row 제거 axis: 'index'|'coloums'
df.dropna(axis='index', how='all') #모든 값이 na값인 경우 제거
```

### 정렬

```python
df.sort_values(['이름', '키'], ascending=[True, False]] #이름: 오름차순 -> 키: 내림차순
```

### 함수 사용

```python
df['키'].apply(lambda x: str(x) + 'cm') # row 하나씩 값을 가져와서 적용
```

### 그룹화

```python
df.groupby('학교')get_group('북산고')
df.groupby('학교').mean() # 계산 가능한 데이터의 평균값을 같은학교끼리 그룹
df.groupby('학교').size() # 그룹별 데이터 수
df.groupby(['학교', '학년'])
df.groupby('학년').mean().sort_values('키')
df.groupby('학교').value_counts(normaize=True) #normalize: 백분율
```

### query

```python
df.query('@변수 > 0') #외부 변수 사용
df.query('(a > 0) & (b > 0)') #컬럼 다중 조건
df.query('a in [가, 나]') #컬럼 값 조회
df.query('a not in [가, 나]')
df.query('`띄 어 쓰기` > 0') #컬럼명에 공백이 있을 경우
```