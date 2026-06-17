# Python 데이터 시각화 완벽 가이드

> **대상 독자**: Matplotlib과 Seaborn을 처음 접하는 완전 초보자  
> **학습 목표**: 데이터 시각화의 기본 개념부터 고급 기법까지 체계적으로 이해하고, 실제 데이터에 적용할 수 있는 능력 배양

---

## 목차

1. [데이터 시각화란?](#1-데이터-시각화란)
2. [Matplotlib 기초](#2-matplotlib-기초)
3. [Matplotlib 실전 차트](#3-matplotlib-실전-차트)
4. [Seaborn 입문](#4-seaborn-입문)
5. [Seaborn 분포 시각화](#5-seaborn-분포-시각화)
6. [Seaborn 관계형 시각화](#6-seaborn-관계형-시각화)
7. [Seaborn 범주형 시각화](#7-seaborn-범주형-시각화)
8. [Seaborn 다변량 시각화](#8-seaborn-다변량-시각화)
9. [시각화 선택 가이드](#9-시각화-선택-가이드)

---

## 1. 데이터 시각화란?

### 1.1 왜 데이터 시각화가 필요한가?

데이터 시각화(Data Visualization)는 **숫자와 텍스트로 된 데이터를 그래픽 요소(점, 선, 막대, 색상 등)로 변환**하여 인간이 쉽게 인식하고 이해할 수 있게 만드는 과정입니다.

**인간의 뇌는 이미지를 텍스트보다 훨씬 빠르게 처리합니다.**
- 텍스트로 된 수십 개의 숫자를 보는 것보다, 하나의 그래프를 보는 것이 데이터의 패턴을 파악하기 훨씬 쉽습니다.
- 예를 들어, 월별 매출 데이터를 표로 보면 추세를 파악하기 어렵지만, 선 그래프로 보면 상승/하락 추세를 즉시 알 수 있습니다.

### 1.2 Python 데이터 시각화 라이브러리 생태계

Python에서 데이터 시각화를 위해 주로 사용하는 라이브러리는 다음과 같습니다:

| 라이브러리 | 특징 | 난이도 | 주요 용도 |
|---|---|---|---|
| **Matplotlib** | Python 시각화의 기초, 세밀한 커스터마이징 가능 | 중급 | 모든 종류의 2D/3D 플롯 |
| **Seaborn** | Matplotlib 기반, 통계 그래프에 특화 | 초급~중급 | 통계 분석, 다변량 시각화 |
| **Plotly** | 인터랙티브(상호작용) 그래프 | 중급 | 웹 대시보드, 프레젠테이션 |
| **Pandas** | DataFrame 내장 시각화 기능 | 초급 | 빠른 탐색적 데이터 분석(EDA) |

> **이 가이드의 핵심**: Matplotlib의 구조적 이해 + Seaborn의 통계적 시각화 능력을 결합하여 효과적인 데이터 시각화를 수행합니다.

---

## 2. Matplotlib 기초

### 2.1 Matplotlib이란?

**Matplotlib**은 Python에서 가장 널리 사용되는 저수준(low-level) 데이터 시각화 라이브러리입니다. MATLAB의 그래프 기능을 Python에서 구현하고자 해서 탄생했으며, 거의 모든 종류의 2D 그래프를 생성할 수 있습니다.

**설치 방법**:
```bash
pip install matplotlib
# 또는
python -m pip install matplotlib
```

### 2.2 Matplotlib의 핵심 구성 요소

Matplotlib을 이해하려면 세 가지 핵심 개념을 반드시 알아야 합니다:

#### Figure (종이/캔버스)
- 그래프가 그려지는 **전체 캔버스**입니다.
- 하나의 Figure 안에 여러 개의 그래프를 배치할 수 있습니다.
- 크기(`figsize`)와 해상도(`dpi`)를 조절할 수 있습니다.

#### Axes (그래프)
- Figure 내부의 **개별 그래프 영역**입니다.
- 하나의 Figure에 여러 개의 Axes를 배치할 수 있습니다. (이를 서브플롯이라고 합니다)
- 실제 데이터가 그려지는 공간입니다.

#### Axis (축)
- x축, y축 등 **좌표축**을 의미합니다.
- 눈금(tick), 레이블(label), 제목(title) 등이 Axis에 속합니다.

**비유**: Figure는 **스케치북 전체**, Axes는 **스케치북 한 페이지**, Axis는 **페이지의 눈금자**입니다.

### 2.3 Figure 생성하기

```python
import matplotlib.pyplot as plt

# 기본 Figure 생성 (빈 캔버스)
fig = plt.figure()
print(type(fig))  # <class 'matplotlib.figure.Figure'>

# 크기와 해상도 설정
fig = plt.figure(figsize=(10, 4), dpi=150)
# figsize=(가로, 세로) 단위: 인치(inches)
# dpi=150: 인치당 150픽셀
# 실제 픽셀 크기 = 10×150 = 1500픽셀(가로), 4×150 = 600픽셀(세로)
```

**figsize와 dpi 관계**:
- `figsize`는 Figure의 **물리적 크기**를 인치 단위로 결정합니다.
- `dpi`(Dots Per Inch)는 **해상도**를 결정합니다.
- **실제 픽셀 크기 = figsize × dpi**
- 예: `figsize=(10, 4)`, `dpi=100` → 1000×400 픽셀

**용도별 dpi 권장값**:
| 용도 | 권장 dpi | 이유 |
|---|---|---|
| 웹/화면 표시 | 100~150 | 빠른 렌더링, 적절한 화질 |
| 프레젠테이션 | 150~200 | 프로젝터에서 선명하게 보임 |
| 인쇄/출판 | 300 이상 | 고해상도 인쇄물 |

### 2.4 기본 선 그래프 (Line Plot)

```python
import numpy as np

x = np.arange(0, 10, 0.1)
y = np.sin(x)

plt.plot(x, y)
plt.show()
```

**코드 설명**:
- `np.arange(0, 10, 0.1)`: 0부터 10까지 0.1 간격으로 숫자 생성 → 총 100개 데이터
- `np.sin(x)`: x값에 대한 사인(sine) 함수 값 계산
- `plt.plot(x, y)`: x와 y 데이터를 선으로 연결하여 그래프 그리기
- `plt.show()`: 그래프를 화면에 표시

### 2.5 그래프 꾸미기 (Styling)

#### 색상(Color)
| 방식 | 예시 | 설명 |
|---|---|---|
| 문자 코드 | `'r'`, `'g'`, `'b'` | 빨강, 초록, 파랑 |
| 문자 코드 | `'k'`, `'w'`, `'y'` | 검정, 흰색, 노랑 |
| 16진수 | `'#FF5733'` | 정확한 색상 지정 |
| RGB 튜플 | `(1.0, 0.5, 0.2)` | 0~1 사이 실수 튜플 |
| CSS 색상명 | `'orange'`, `'skyblue'` | CSS에서 사용하는 이름 |

#### 선 스타일(Linestyle)
| 문자 | 설명 |
|---|---|
| `'-'` | 실선 (solid) |
| `'--'` | 파선 (dashed) |
| `'-.'` | 점선-파선 (dash-dot) |
| `':'` | 점선 (dotted) |

#### 마커(Marker)
| 문자 | 모양 |
|---|---|
| `'o'` | 원형 |
| `'s'` | 사각형 |
| `'^'` | 위쪽 삼각형 |
| `'D'` | 다이아몬드 |
| `'*'` | 별표 |
| `'x'` | X 표시 |

**꾸미기 예시**:
```python
plt.plot(x, y, color='red', linestyle='--', marker='o', markersize=8, label='sine wave')
plt.title('사인 파동 그래프')      # 그래프 제목
plt.xlabel('X축 (시간)')           # x축 레이블
plt.ylabel('Y축 (진폭)')           # y축 레이블
plt.legend()                       # 범례 표시
plt.grid(True)                     # 격자선 표시
plt.show()
```

### 2.6 여러 그래프 그리기

```python
x = np.linspace(0, 10, 100)

plt.plot(x, np.sin(x), label='sin(x)', color='blue')
plt.plot(x, np.cos(x), label='cos(x)', color='red', linestyle='--')

plt.title('삼각함수 비교')
plt.xlabel('x (라디안)')
plt.ylabel('y 값')
plt.legend(loc='upper right')  # 범례 위치 지정
plt.grid(True, alpha=0.3)      # 투명한 격자선
plt.show()
```

### 2.7 서브플롯 (Subplot)

하나의 Figure에 여러 개의 그래프를 배치하는 방법입니다.

```python
# 방법 1: plt.subplot(행, 열, 인덱스)
plt.figure(figsize=(10, 4))

plt.subplot(1, 2, 1)  # 1행 2열 중 1번째
plt.plot(x, np.sin(x))
plt.title('sin(x)')

plt.subplot(1, 2, 2)  # 1행 2열 중 2번째
plt.plot(x, np.cos(x))
plt.title('cos(x)')

plt.tight_layout()  # 서브플롯 간격 자동 조절
plt.show()
```

**`plt.tight_layout()`의 역할**:
- 여러 서브플롯이 겹치지 않도록 자동으로 간격을 조절합니다.
- 특히 제목이나 축 레이블이 길 때 필수적으로 사용합니다.

---

## 3. Matplotlib 실전 차트

이제 실제 데이터셋을 사용하여 다양한 유형의 차트를 만들어 봅시다.

### 3.1 사용할 데이터셋

| 데이터셋 | 설명 | 주요 변수 |
|---|---|---|
| **Iris (붓꽃)** | 머신러닝의 클래식한 분류 데이터셋 | sepal_length, sepal_width, petal_length, petal_width, species |
| **Titanic** | 1912년 타이타닉호 침몰 당시 승객 정보 | survived, pclass, sex, age, fare, embarked |
| **World Happiness Report** | 세계 각국의 행복 지수 데이터 | Overall rank, Country, Score, GDP per capita, Healthy life expectancy |

### 3.2 라이브러리 임포트 및 한글 폰트 설정

```python
import matplotlib.pyplot as plt
import pandas as pd
import urllib.request

# Windows 한글 폰트 설정
plt.rcParams['font.family'] = 'Malgun Gothic'
plt.rcParams['axes.unicode_minus'] = False
```

> **중요**: 한글 폰트 설정을 하지 않으면 그래프의 한글 제목과 레이블이 깨져서 네모 박스(□)로 표시됩니다.

### 3.3 선 그래프 (Line Plot)

**용도**: 시간/순서에 따른 데이터의 변화 추세를 보여줄 때 사용합니다.

**예시 - 월별 항공 승객 수 변화**:
```python
# flights 데이터: 1949년~1960년 월별 항공 승객 수
plt.figure(figsize=(12, 5))
plt.plot(df_flights['year'], df_flights['passengers'], marker='o')
plt.title('연도별 항공 승객 수 추세')
plt.xlabel('연도')
plt.ylabel('승객 수')
plt.grid(True, alpha=0.3)
plt.show()
```

**선 그래프 해석법**:
- **전반적인 추세**: 데이터가 상승하는가, 하락하는가, 아니면 일정한가?
- **계절성/주기성**: 특정 패턴이 반복되는가? (예: 여름철 승객 증가)
- **급격한 변화**: 어떤 시점에 급격한 변화가 있었는가? 이유는 무엇인가?

### 3.4 막대 그래프 (Bar Chart)

**용도**: 범주형 데이터의 값을 비교할 때 사용합니다.

**예시 - 붓꽃 품종별 꽃잎 길이 평균**:
```python
# 품종별 평균 꽃잎 길이 계산
mean_petal = df_iris.groupby('species')['petal_length'].mean()

plt.figure(figsize=(8, 5))
colors = ['coral', 'skyblue', 'lightgreen']
plt.bar(mean_petal.index, mean_petal.values, color=colors)
plt.title('붓꽃 품종별 평균 꽃잎 길이')
plt.xlabel('품종')
plt.ylabel('평균 꽃잎 길이 (cm)')
plt.show()
```

**막대 그래프 vs 선 그래프**:
| 막대 그래프 | 선 그래프 |
|---|---|
| 범주 간 **비교**에 적합 | 시간/순서에 따른 **추세**에 적합 |
| 각 범주가 독립적 | 데이터 간 연속성이 있음 |
| 범주 순서를 자유롭게 변경 가능 | x축 순서가 의미를 가짐 |

### 3.5 산점도 (Scatter Plot)

**용도**: 두 수치형 변수 간의 **관계(상관관계)**를 시각화할 때 사용합니다.

**예시 - 꽃받침 길이 vs 꽃잎 길이**:
```python
plt.figure(figsize=(8, 6))

# 품종별로 색상 구분
species_colors = {'setosa': 'red', 'versicolor': 'green', 'virginica': 'blue'}
for species, color in species_colors.items():
    subset = df_iris[df_iris['species'] == species]
    plt.scatter(subset['sepal_length'], subset['petal_length'],
                c=color, label=species, alpha=0.6)

plt.title('꽃받침 길이 vs 꽃잎 길이')
plt.xlabel('꽃받침 길이 (cm)')
plt.ylabel('꽃잎 길이 (cm)')
plt.legend()
plt.show()
```

**산점도 해석법**:
- **우상향(↗)**: 양의 상관관계 — x가 증가하면 y도 증가
- **우하향(↘)**: 음의 상관관계 — x가 증가하면 y는 감소
- **무작위 분포**: 상관관계 없음
- **군집(Cluster)**: 데이터가 특정 영역에 모여 있음 → 하위 그룹 존재 가능성

### 3.6 히스토그램 (Histogram)

**용도**: 하나의 수치형 변수의 **분포 형태**를 파악할 때 사용합니다.

**예시 - 타이타닉 승객 나이 분포**:
```python
plt.figure(figsize=(8, 5))
plt.hist(df_titanic['age'].dropna(), bins=30, edgecolor='black', alpha=0.7)
plt.title('타이타닉 승객 나이 분포')
plt.xlabel('나이')
plt.ylabel('인원 수')
plt.show()
```

**히스토그램 해석법**:
- **모양**: 정규분포(종형), 왜도(치우침), 쌍봉(두 개의 그룹) 등
- **중심**: 데이터가 어디에 집중되어 있는가? (평균/중앙값 근처)
- **퍼짐**: 데이터가 넓게 퍼져 있는가, 좁게 모여 있는가? (표준편차)
- **이상치**: 극단적으로 떨어진 데이터가 있는가?

**bins(구간 수)의 영향**:
- `bins`이 너무 작으면(예: 5): 세부 패턴을 놓칩니다.
- `bins`이 너무 많으면(예: 100): 노이즈가 많아져 패턴 파악이 어렵습니다.
- 일반적으로 20~50 사이가 적절합니다.

### 3.7 박스플롯 (Box Plot)

**용도**: 데이터의 **통계적 분포와 이상치**를 한눈에 보여줍니다.

**예시 - 붓꽃 품종별 꽃잎 너비 분포**:
```python
plt.figure(figsize=(8, 5))
plt.boxplot([setosa['petal_width'], versicolor['petal_width'], virginica['petal_width']],
            labels=['setosa', 'versicolor', 'virginica'])
plt.title('붓꽃 품종별 꽃잎 너비 분포')
plt.ylabel('꽃잎 너비 (cm)')
plt.show()
```

**박스플롯 구성 요소 상세**:

```
     ┌─ 이상치 (Outlier): 수염 범위를 벗어난 점
  ───┼─── Q3 + 1.5×IQR (상위 수염 끝)
     │
  ┌──┴──┐  ← Q3 (제3사분위수, 75% 지점)
  │     │
  │  ━━━│━━  ← Q2 (중앙값, Median, 50% 지점)
  │     │
  └──┬──┘  ← Q1 (제1사분위수, 25% 지점)
     │
  ───┼─── Q1 - 1.5×IQR (하위 수염 끝)
     │
```

- **IQR (InterQuartile Range)**: Q3 - Q1, 데이터의 중간 50%가 분포하는 범위
- **수염(Whisker)**: Q1 - 1.5×IQR부터 Q3 + 1.5×IQR까지의 범위
- **이상치(Outlier)**: 수염을 벗어난 데이터 포인트

### 3.8 파이 차트 (Pie Chart)

**용도**: 전체에 대한 **각 부분의 비율(구성비)**을 표현할 때 사용합니다.

**예시 - 타이타닉 생존/사망 비율**:
```python
survived_counts = df_titanic['survived'].value_counts()
labels = ['사망', '생존']
colors = ['lightcoral', 'lightgreen']

plt.figure(figsize=(7, 7))
plt.pie(survived_counts, labels=labels, colors=colors, autopct='%1.1f%%', startangle=90)
plt.title('타이타닉 생존/사망 비율')
plt.show()
```

**파이 차트 사용 시 주의사항**:
- **범주가 3~7개**일 때 가장 효과적입니다. 범주가 너무 많으면 조각이 작아져 가독성이 떨어집니다.
- **비슷한 크기의 조각**이 많으면 차이를 구분하기 어렵습니다. 이때는 막대 그래프가 더 적합합니다.
- **정확한 수치 비교**가 필요하면 막대 그래프를, **대략적인 비율 감각**이 필요하면 파이 차트를 사용합니다.

---

## 4. Seaborn 입문

### 4.1 Seaborn이란?

**Seaborn**은 Matplotlib을 기반으로 한 **고수준(high-level) 데이터 시각화 라이브러리**입니다. 통계적 데이터 시각화에 특화되어 있으며, Matplotlib보다 더 적은 코드로 더 아름답고 정보가 풍부한 그래프를 생성할 수 있습니다.

**설치 방법**:
```bash
conda install seaborn
# 또는
pip install seaborn
```

### 4.2 Matplotlib vs Seaborn

| 특징 | Matplotlib | Seaborn |
|---|---|---|
| **복잡도** | 세밀한 커스터마이징 가능 | 간결한 코드로 통계 그래프 생성 |
| **스타일** | 기본 스타일, 직접 설정 필요 | 미리 정의된 미려한 테마 제공 |
| **데이터 타입** | NumPy 배열 중심 | Pandas DataFrame과 완벽 통합 |
| **통계 기능** | 기본적 | 내장 통계 추정 및 신뢰구간 제공 |
| **학습 난이도** | 구조 이해 필요 | 상대적으로 쉬움 |

**관계**: Seaborn은 Matplotlib의 **래퍼(Wrapper)** 역할을 합니다. Seaborn으로 그래프를 그린 후, Matplotlib 함수를 추가로 호출하여 세부 스타일을 조정할 수 있습니다.

### 4.3 Seaborn의 주요 특징

1. **Pandas DataFrame과 원활하게 작동**: DataFrame의 컬럼명을 직접 사용할 수 있습니다.
2. **통계적 데이터 시각화**: 분포, 회귀, 범주형 데이터 등 통계 분석에 필요한 그래프를 내장하고 있습니다.
3. **자동 스타일링**: 색상 팔레트, 테마를 자동으로 적용하여 미려한 그래프를 생성합니다.
4. **다중 변수 시각화**: `hue`, `col`, `row` 파라미터로 쉽게 다차원 그래프(패싯 그래프)를 생성할 수 있습니다.

### 4.4 라이브러리 임포트

```python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
```

**별칭(Alias)의 의미**:
- `sns`: seaborn의 공식 별칭으로, 통계학자 **S**am **N**orman **S**eaborn의 이름에서 유래했습니다.
- `plt`: matplotlib.pyplot의 표준 별칭
- `pd`: pandas의 표준 별칭

### 4.5 Seaborn 내장 데이터셋

Seaborn은 학습과 실습을 위해 다양한 내장 데이터셋을 제공합니다.

```python
# 사용 가능한 데이터셋 목록 확인
dataset_names = sns.get_dataset_names()
print(dataset_names)
```

**주요 데이터셋**:

| 데이터셋 | 설명 | 주요 변수 | 활용 예시 |
|---|---|---|---|
| **tips** | 레스토랑 팁 데이터 | total_bill, tip, sex, smoker, day, time, size | 회귀분석, 범주형 시각화 |
| **iris** | 붓꽃 데이터 | sepal_length, sepal_width, petal_length, petal_width, species | 분류, 산점도 |
| **penguins** | 펭귄 데이터 | bill_length, flipper_length, body_mass, species, island | 다변량 분석 |
| **flights** | 항공 승객 데이터 | year, month, passengers | 시계열, 히트맵 |
| **titanic** | 타이타닉 생존 데이터 | survived, pclass, sex, age, fare | 분류, 생존 분석 |

```python
# 데이터셋 로드
df_tips = sns.load_dataset("tips")
df_iris = sns.load_dataset("iris")
df_penguins = sns.load_dataset("penguins")
df_flights = sns.load_dataset("flights")
df_titanic = sns.load_dataset("titanic")
```

### 4.6 Seaborn 테마 설정

Seaborn은 미리 정의된 **스타일 테마**를 제공하여 그래프의 전체적인 느낌을 쉽게 변경할 수 있습니다.

```python
# 사용 가능한 테마
sns.set_style("whitegrid")   # 흰색 배경 + 격자선
sns.set_style("darkgrid")    # 어두운 배경 + 격자선
sns.set_style("white")       # 흰색 배경 (기본)
sns.set_style("dark")        # 어두운 배경
sns.set_style("ticks")       # 눈금이 있는 스타일
```

**컨텍스트(context) 설정**:
```python
# 그래프의 크기와 선 굵기를 용도에 맞게 조정
sns.set_context("paper")     # 학술 논문용 (작은 크기)
sns.set_context("notebook")  # 노트북용 (기본)
sns.set_context("talk")      # 프레젠테이션용 (큰 크기)
sns.set_context("poster")    # 포스터용 (매우 큰 크기)
```

---

## 5. Seaborn 분포 시각화

데이터의 **분포(Distribution)**는 데이터가 어떤 값들로 구성되어 있고, 그 값들이 어떤 패턴으로 나타나는지를 보여줍니다.

### 5.1 히스토그램 (histplot)

**용도**: 수치형 데이터의 구간별 빈도(개수)를 막대로 표현합니다.

```python
sns.histplot(data=df_tips, x="total_bill", bins=20, kde=True)
plt.show()
```

**핵심 파라미터**:
- **`data`**: DataFrame
- **`x`**: 분포를 볼 수치형 변수
- **`bins`**: 구간 개수 (기본값: 자동)
- **`kde=True`**: KDE 곡선을 함께 표시
- **`hue`**: 범주별로 분포를 겹쳐 표시

**KDE (Kernel Density Estimate)**:
- 히스토그램의 계단 형태를 **부드러운 곡선**으로 근사합니다.
- 데이터의 **연속적인 밀도 분포**를 보여줍니다.
- `kde=True`로 활성화할 수 있습니다.

### 5.2 커널 밀도 추정 (kdeplot)

**용도**: KDE 곡선만을 단독으로 그려서 데이터의 밀도 분포를 부드럽게 시각화합니다.

```python
sns.kdeplot(data=df_tips, x="total_bill", hue="time", fill=True, alpha=0.3)
plt.show()
```

**핵심 파라미터**:
- **`fill=True`**: 곡선 아래 영역을 채웁니다.
- **`alpha`**: 채움 영역의 투명도
- **`bw_adjust`**: 대역폭 조절 (0.5=뾰족, 1.0=기본, 2.0=부드러움)
- **`hue`**: 범주별로 다른 색상의 KDE 곡선 표시

**bw_adjust의 의미**:
- 대역폭(bandwidth)이 작을수록(0.5): 데이터의 세부 변동을 더 민감하게 반영합니다. 곡선이 뾰족해집니다.
- 대역폭이 클수록(2.0): 전체적인 추세를 부드럽게 보여줍니다. 노이즈가 줄어듭니다.

### 5.3 누적분포함수 (ecdfplot)

**용도**: 데이터의 **누적 비율**을 보여줍니다. '전체 데이터 중 X 값 이하인 데이터가 몇 %인가?'를 파악할 수 있습니다.

```python
sns.ecdfplot(data=df_tips, x="total_bill", hue="time")
plt.show()
```

**해석법**:
- y축 0.5(50%) 지점에서 x축 값을 읽으면 → **중앙값(Median)**
- y축 0.25(25%) 지점 → **Q1 (제1사분위수)**
- y축 0.75(75%) 지점 → **Q3 (제3사분위수)**
- 두 범주의 곡선이 얼마나 떨어져 있는가 → 분포의 차이

### 5.4 러그 플롯 (rugplot)

**용도**: 데이터의 **개별 포인트 위치**를 x축에 짧은 선(깔개, rug)으로 표시합니다.

```python
sns.kdeplot(data=df_tips, x="total_bill")
sns.rugplot(data=df_tips, x="total_bill", color="red", alpha=0.5)
plt.show()
```

**특징**:
- 데이터가 **실제로 어디에 몰려 있는지**를 정확히 보여줍니다.
- 다른 분포 그래프(histplot, kdeplot)와 함께 사용하면 풍부한 정보를 전달합니다.
- 데이터 개수가 많으면 선들이 겹쳐서 보기 어려워질 수 있습니다.

### 5.5 통합 분포 그래프 (displot)

**용도**: `histplot`, `kdeplot`, `ecdfplot`, `rugplot`을 **하나의 함수**로 통합하여 사용합니다. `kind` 파라미터로 원하는 그래프 유형을 선택합니다.

```python
# 히스토그램 + KDE
sns.displot(data=df_tips, x="total_bill", kind="hist", kde=True)

# KDE만
sns.displot(data=df_tips, x="total_bill", kind="kde", hue="sex")

# ECDF
sns.displot(data=df_tips, x="total_bill", kind="ecdf", hue="sex")
```

**displot vs histplot/kdeplot**:
- `displot`은 **FacetGrid**를 반환하므로 `col`, `row` 파라미터를 사용하여 다중 패널 그래프를 쉽게 만들 수 있습니다.
- `histplot`/`kdeplot`은 단일 Axes 객체를 반환하므로 세부 조정이 더 용이합니다.

---

## 6. Seaborn 관계형 시각화

두 변수 간의 **관계(Relationship)**를 시각화합니다.

### 6.1 산점도 (scatterplot)

**용도**: 두 수치형 변수 간의 관계를 점으로 표현합니다.

```python
sns.scatterplot(data=df_iris, x="sepal_length", y="petal_length",
                hue="species", size="petal_width", style="species")
plt.show()
```

**핵심 파라미터**:
- **`x`, `y`**: 각 축에 배치할 수치형 변수
- **`hue`**: 범주형 변수 — 점의 색상으로 구분
- **`size`**: 수치형/범주형 변수 — 점의 크기로 구분
- **`style`**: 범주형 변수 — 점의 마커 모양으로 구분
- **`alpha`**: 점의 투명도 (0~1)

**3차원 정보 시각화**:
`hue`, `size`, `style`을 조합하면 2D 평면에 **4차원 정보**(x, y, 색상, 크기, 모양)를 표현할 수 있습니다.

### 6.2 선 그래프 (lineplot)

**용도**: x축이 순서를 가진 변수(시간, 순서 등)일 때, y값의 추세를 선으로 연결하여 표현합니다.

```python
sns.lineplot(data=df_flights, x="year", y="passengers", hue="month")
plt.show()
```

**선 그래프의 특징**:
- `scatterplot`과 달리, x축 값이 같은 데이터들은 **자동으로 평균**을 내고 신뢰구간을 표시합니다.
- 시간/순서에 따른 **추세와 변동**을 부드럽게 보여줍니다.
- `hue`로 여러 범주의 추세를 동시에 비교할 수 있습니다.

### 6.3 관계형 통합 그래프 (relplot)

**용도**: `scatterplot`과 `lineplot`을 하나의 함수(`kind` 파라미터)로 통합하고, **`col`과 `row`를 사용하여 다중 패널 그래프**를 쉽게 생성합니다.

```python
# 산점도 + 다중 패널
sns.relplot(data=df_iris, x="sepal_length", y="petal_length",
            hue="species", col="species")

# 선 그래프 + 다중 패널
sns.relplot(data=df_flights, x="year", y="passengers",
            kind="line", hue="month", col="month", col_wrap=4)
```

**col_wrap**:
- `col_wrap=4`는 한 행에 4개의 패널을 배치하고 다음 행으로 넘어갑니다.
- 패널 개수가 많을 때 가독성을 높여줍니다.

---

## 7. Seaborn 범주형 시각화

범주형(Categorical) 변수에 따른 수치형 변수의 분포나 값을 시각화합니다.

### 7.1 막대 그래프 (barplot)

**용도**: 범주형 변수(x)에 따른 수치형 변수(y)의 **평균(또는 다른 집계값)**을 막대로 표현합니다.

```python
sns.barplot(data=df_tips, x="day", y="tip")
plt.show()
```

**작동 원리**:
1. x축의 각 범주(요일)별로 데이터를 그룹화합니다.
2. 각 그룹의 y값(`tip`)에 대한 **평균(mean)**을 계산합니다.
3. 계산된 평균값이 막대의 높이가 됩니다.
4. **에러바(errorbar)**가 자동으로 표시되어 각 그룹 평균의 **신뢰구간(95% CI)**을 나타냅니다.

**핵심 파라미터**:
- **`estimator`**: 집계 함수 — `"mean"`(기본), `"sum"`, `"max"`, `"min"`, `"median"`
- **`errorbar`**: 에러바 유형 — `("ci", 95)`(기본), `("se", 1)`, `None`
- **`hue`**: 추가 범주로 막대 분할
- **`order`**, **`hue_order`**: 범주 표시 순서 지정
- **`orient`**: 방향 — `"v"`(세로, 기본), `"h"`(가로)

**에러바의 의미**:
- **신뢰구간(CI)**: 평균값이 95%의 확률로 이 범위 안에 존재함을 의미합니다. 값이 커질수록(99%) 에러바는 더 길어집니다.
- **표준오차(SE)**: 평균 추정치의 정확도를 나타냅니다. SE = SD / √n으로, 데이터 개수가 많아지면 작아집니다.
- 에러바가 겹치는 두 범주는 통계적으로 유의미한 차이가 없을 가능성이 있습니다.

### 7.2 빈도 막대 그래프 (countplot)

**용도**: 각 범주에 속한 **데이터의 개수(빈도)**를 막대로 표현합니다.

```python
sns.countplot(data=df_titanic, x="class", hue="survived", palette="Set2")
plt.show()
```

**barplot과의 차이**:
| 구분 | barplot | countplot |
|---|---|---|
| y축 의미 | 수치형 변수의 집계값(평균 등) | 각 범주의 데이터 개수 |
| y 파라미터 | 필요함 | 필요없음 (자동 계산) |
| 용도 | 범주별 수치 비교 | 범주별 빈도/비율 비교 |

### 7.3 상자 그림 (boxplot)

**용도**: 데이터의 **분포와 이상치**를 한눈에 보여주는 통계적 그래프입니다.

```python
sns.boxplot(data=df_penguins, x="species", y="flipper_length_mm", hue="sex")
plt.show()
```

**구성 요소 상세**:

```
     ┌─ 이상치 (Outlier)
  ───┼─── Q3 + 1.5×IQR (상위 수염 끝)
     │
  ┌──┴──┐  ← Q3 (제3사분위수, 75% 지점)
  │     │
  │  ━━━│━━  ← Q2 (중앙값, Median, 50% 지점)
  │     │
  └──┬──┘  ← Q1 (제1사분위수, 25% 지점)
     │
  ───┼─── Q1 - 1.5×IQR (하위 수염 끝)
     │
```

- **IQR (InterQuartile Range)**: Q3 - Q1, 데이터의 중간 50%가 분포하는 범위
- **이상치**: Q1 - 1.5×IQR보다 작거나, Q3 + 1.5×IQR보다 큰 값
- `flierprops` 파라미터로 이상치 점의 스타일(모양, 색상, 크기)을 조정할 수 있습니다.

### 7.4 바이올린 플롯 (violinplot)

**용도**: 박스플롯과 KDE를 결합하여 데이터의 **전체 분포 형태**를 보여줍니다.

```python
sns.violinplot(data=df_penguins, x="species", y="flipper_length_mm",
               hue="sex", split=True, inner="quart")
plt.show()
```

**구성 요소**:
- **외부 곡선**: KDE(커널 밀도 추정) 곡선 — 데이터가 많이 몰린 곳이 넓어집니다.
- **내부 표시**: `inner` 파라미터로 조절
  - `"box"`: 박스플롯 (기본)
  - `"quart"`: 사분위수 가로선만
  - `"point"`: 개별 데이터 점
  - `"stick"`: 개별 데이터 세로선
  - `None`: 내부 표시 없음

**`split=True`**: hue의 두 범주를 하나의 바이올린을 좌우로 분할하여 표시합니다. 이를 통해 같은 x축 위치에서 두 범주의 분포를 직접 비교할 수 있습니다.

### 7.5 스트립 플롯 (stripplot)

**용도**: 범주형 변수에 따른 **원본 데이터 포인트**를 직접 점으로 표시합니다.

```python
sns.stripplot(data=df_tips, x="day", y="tip", hue="sex", jitter=True)
plt.show()
```

**특징**:
- **`jitter=True`**: x축 범주 내에서 점들을 무작위로 좌우로 흔들어 겹침을 방지합니다.
- 데이터가 많으면 점들이 겹쳐서 개별 포인트를 구분하기 어려워집니다.
- `boxplot`이나 `violinplot`과 함께 사용하면 요약 통계량과 원본 데이터를 동시에 볼 수 있습니다.

### 7.6 스웜 플롯 (swarmplot)

**용도**: `stripplot`과 유사하지만, 점들이 **겹치지 않도록 알고리즘적으로 배치**합니다.

```python
sns.swarmplot(data=df_tips, x="day", y="tip", hue="sex", palette="Set2")
plt.show()
```

**stripplot vs swarmplot**:
| 구분 | stripplot | swarmplot |
|---|---|---|
| 겹침 | 점들이 겹칠 수 있음 | 점들이 겹치지 않음 |
| 알고리즘 | 무작위(random) 좌우 흔들기 | 알고리즘적 배치 |
| 속도 | 빠름 | 데이터가 많을 때 느림 |
| 용도 | 대용량 데이터 | 중소용량 데이터의 정밀 분포 확인 |

### 7.7 포인트 플롯 (pointplot)

**용도**: 범주형 변수에 따른 수치형 변수의 **평균(또는 추정치)을 점으로 표시하고 선으로 연결**합니다.

```python
sns.pointplot(data=df_flights, x="year", y="passengers", hue="month")
plt.show()
```

**특징**:
- 점들을 선으로 연결하여 **추세(trend)**를 보여줍니다.
- 에러바(신뢰구간)가 자동으로 표시됩니다.
- 순서가 있는 범주(시간, 등급 등)에서 **변화 방향**을 파악하기에 적합합니다.

### 7.8 범주형 통합 그래프 (catplot)

**용도**: 위의 모든 범주형 플롯을 `kind` 파라미터 하나로 통합하며, **`col`과 `row`를 사용하여 다중 패널 그래프**를 쉽게 생성합니다.

```python
# 기본 (kind="bar")
sns.catplot(data=df_tips, x="day", y="tip", kind="bar")

# kind 변경
sns.catplot(data=df_tips, x="day", y="tip", kind="box")
sns.catplot(data=df_tips, x="day", y="tip", kind="violin")
sns.catplot(data=df_tips, x="day", y="tip", kind="strip")

# 다중 패널
sns.catplot(data=df_tips, x="day", y="tip", kind="bar",
            col="sex", row="smoker", hue="day", palette="Set2")
```

**지원하는 kind 값**:
| kind | 대응 함수 | 설명 |
|---|---|---|
| `"bar"` | `barplot` | 막대 그래프 (기본값) |
| `"box"` | `boxplot` | 상자 그림 |
| `"violin"` | `violinplot` | 바이올린 플롯 |
| `"strip"` | `stripplot` | 스트립 플롯 |
| `"swarm"` | `swarmplot` | 스웜 플롯 |
| `"point"` | `pointplot` | 포인트 플롯 |

---

## 8. Seaborn 다변량 시각화

여러 변수 간의 관계를 **한 번에** 시각화합니다.

### 8.1 페어 플롯 (pairplot)

**용도**: 데이터셋의 **수치형 컬럼들 간의 모든 조합**을 한 번에 시각화합니다. 산점도 행렬(Scatter Plot Matrix)이라고도 불립니다.

```python
sns.pairplot(data=df_penguins, hue="species", palette="Set2")
plt.show()
```

**격자 구조**:

컬럼이 3개(A, B, C)인 경우, 3×3 격자가 생성됩니다:

```
      A         B         C
A [대각선]   [A vs B]  [A vs C]
B [B vs A]  [대각선]   [B vs C]
C [C vs A]  [C vs B]  [대각선]
```

- **대각선(diag_kind)**: 각 컬럼의 **단변량 분포**를 표시
  - `"hist"`: 히스토그램 (hue가 없을 때 기본)
  - `"kde"`: KDE 곡선 (hue가 있을 때 기본)
- **비대각선(kind)**: 두 컬럼 간의 **이변량 관계**를 표시
  - `"scatter"`: 산점도 (기본)
  - `"kde"`: 2D KDE 등고선
  - `"hist"`: 2D 히스토그램
  - `"reg"`: 회귀선이 추가된 산점도

**핵심 파라미터**:
- **`vars`**: 특정 컬럼만 선택하여 분석 (변수가 많을 때 유용)
- **`hue`**: 범주형 변수로 점들을 색상 구분 — 대각선이 자동으로 KDE로 변경됩니다.
- **`diag_kws`**: 대각선 그래프에 추가 옵션 전달
- **`plot_kws`**: 비대각선 그래프에 추가 옵션 전달

**인사이트**:
- `hue`를 사용하면 **숨겨진 패턴**을 발견할 수 있습니다.
- 예: 전체 데이터에서는 두 변수가 선형 관계에 보이지만, 범주별로 나누어 보면 각 범주 내에서는 관계가 약하거나 반대 방향일 수 있습니다. 이는 **심슨의 역설(Simpson's Paradox)**을 발견하는 데 도움이 됩니다.

### 8.2 페어 그리드 (PairGrid)

**용도**: `pairplot`보다 더 **세밀한 커스터마이징**이 가능한 저수준 인터페이스입니다.

```python
g = sns.PairGrid(df_penguins, vars=["bill_length_mm", "flipper_length_mm", "body_mass_g"],
                 hue="species")
g.map_diag(sns.histplot)    # 대각선: 히스토그램
g.map_offdiag(sns.scatterplot)  # 비대각선: 산점도
g.add_legend()
plt.show()
```

**PairGrid vs pairplot**:
- `pairplot`: 고수준 인터페이스, 간편함
- `PairGrid`: 저수준 인터페이스, 대각선과 비대각선에 **서로 다른 함수**를 매핑할 수 있음

---

## 9. 시각화 선택 가이드

### 9.1 상황별 추천 그래프

**"데이터가 어떤 모습인지 전체적으로 파악하고 싶다"** →
- 수치형 변수 1개: `histplot`, `kdeplot`, `boxplot`
- 수치형 변수 2개: `scatterplot`
- 수치형 변수 여러 개: `pairplot`

**"범주별로 비교하고 싶다"** →
- 범주별 평균 비교: `barplot`
- 범주별 빈도 비교: `countplot`
- 범주별 분포 비교: `boxplot`, `violinplot`
- 범주별 원본 데이터: `stripplot`, `swarmplot`

**"시간/순서에 따른 변화를 보고 싶다"** →
- `lineplot`, `pointplot`

**"전체 중 특정 부분의 비율을 보고 싶다"** →
- `pie chart` (Matplotlib)

### 9.2 Matplotlib vs Seaborn 선택 기준

| 상황 | 추천 라이브러리 | 이유 |
|---|---|---|
| 세부 스타일 커스터마이징 | Matplotlib | 모든 속성을 직접 제어 가능 |
| 빠른 통계 그래프 생성 | Seaborn | 적은 코드로 풍부한 정보 |
| Pandas DataFrame 사용 | Seaborn | 컬럼명 직접 사용, hue/col/row 지원 |
| 논문/출판용 고품질 그래프 | Matplotlib | dpi, 크기, 폰트 등 완벽 제어 |
| 데이터 탐색(EDA) | Seaborn | 다양한 통계 기능 내장 |
| 인터랙티브 웹 그래프 | Plotly | 마우스 호버, 줌 등 상호작용 가능 |

### 9.3 자주 하는 실수와 해결책

| 실수 | 원인 | 해결책 |
|---|---|---|
| 한글이 네모 박스(□)로 표시됨 | 한글 폰트 미설정 | `plt.rcParams['font.family'] = 'Malgun Gothic'` |
| 마이너스 기호가 깨짐 | 유니코드 마이너스 폰트 없음 | `plt.rcParams['axes.unicode_minus'] = False` |
| 그래프가 겹쳐 보임 | 서브플롯 간격 조절 안 함 | `plt.tight_layout()` |
| 범례(legend)가 그래프를 가림 | 기본 위치가 적절하지 않음 | `plt.legend(loc='best')` 또는 위치 직접 지정 |
| 색상이 너무 많아서 구분 어려움 | hue 범주가 너무 많음 | 색상 팔레트 변경 또는 데이터 그룹화 |

---

## 마무리

이 가이드에서는 Python 데이터 시각화의 두 기둥인 **Matplotlib**과 **Seaborn**을 체계적으로 다루었습니다.

### 핵심 정리

1. **Matplotlib**: 시각화의 기초가 되는 저수준 라이브러리. Figure, Axes, Axis의 구조를 이해하면 어떤 그래프든 자유롭게 커스터마이징할 수 있습니다.

2. **Seaborn**: 통계적 데이터 시각화에 특화된 고수준 라이브러리. Pandas DataFrame과 완벽하게 통합되어 있으며, `hue`, `col`, `row`를 활용한 다변량 시각화가 강점입니다.

3. **적절한 그래프 선택**: 데이터의 특성(수치형/범주형, 개수, 분포 등)과 분석 목적(비교, 추세, 분포, 관계 등)에 따라 가장 적합한 그래프를 선택하는 능력이 중요합니다.

### 다음 단계

- **실습**: 자신이 관심 있는 데이터셋을 찾아 다양한 그래프를 직접 그려보세요.
- **심화**: `FacetGrid`, `JointGrid`, `PairGrid` 등 Seaborn의 저수준 인터페이스를 학습하여 더 복잡한 시각화를 시도해보세요.
- **응용**: Plotly, Bokeh 등 인터랙티브 시각화 라이브러리를 학습하여 웹 대시보드를 만들어보세요.

---

> **참고**: 이 가이드는 `matplotlib1.ipynb`, `matplotlib2.ipynb`, `seaborn1.ipynb`, `seaborn2.ipynb`의 내용을 종합하여 작성되었습니다.
