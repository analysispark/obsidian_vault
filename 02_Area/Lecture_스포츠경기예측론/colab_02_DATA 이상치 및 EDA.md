---
aliases:
  - 단국대학교 스포츠사이언스융합학과
tags:
  - 강의
  - 수업자료
  - 스포츠경기예측
  - colab
title: 스포츠경기예측론
author: Jihoon, Park
---
## 스포츠경기예측론

---
```ipynb
# 한글 폰트 설치

!apt-get -qq install fonts-nanum

  

# matplotlib 폰트 캐시 삭제

!rm -rf ~/.cache/matplotlib

  

import matplotlib.pyplot as plt

import matplotlib.font_manager as fm

  

plt.rcParams["font.family"] = "NanumGothic"

plt.rcParams["axes.unicode_minus"] = False

  

# 세션 재시작 필수!
```

---

```ipynb
from google.colab import auth

auth.authenticate_user()

  

import gspread

from google.auth import default

creds, _= default()

  

gc = gspread.authorize(creds)

  

url = "https://docs.google.com/spreadsheets/d/1f6eStS3fUYgFP-cYBdVGyTJHGbmkRbEu9m9kDXeYaXc/edit?usp=sharing"

sh = gc.open_by_url(url)

  

worksheet = sh.get_worksheet(0)

  

# Get DATA

import pandas as pd

  

rows = worksheet.get_all_records()

df = pd.DataFrame(rows)

  

df.head()
```

```ipynb
print(df.describe())
```

---

## 자료의 시각화

```ipynb
import seaborn as sns

import matplotlib.pyplot as plt

  

num_df = df.select_dtypes(include="number")

  

# Long format으로 변환

long_df = num_df.melt(

var_name="변인",

value_name="값"

)

  

# FacetGrid

g = sns.FacetGrid(

long_df,

col="변인",

col_wrap=4, # 한 행에 4개

sharex=False,

sharey=False,

height=3

)

  

g.map_dataframe(

sns.histplot,

x="값",

bins=20

)

  

g.set_titles("{col_name}")

  

plt.tight_layout()

plt.show()
```

---

## 변수 파악 및 일괄계산

```ipynb
df.columns
```

```ipynb
cols = ["1P%", "2P%", "3P%", "FG%"]

  

result = df[df[cols].eq(1.00).any(axis=1)]

  

print(result)
```

```ipynb
result
```

---

## 생년월일 변수 조작


```ipynb
df["생년월일"].unique()
```

```ipynb
df["생년월일"] = pd.to_datetime(

df["생년월일"],

errors="coerce"

).dt.date
```

```ipynb
print(df["생년월일"].dropna().min())

print(df["생년월일"].dropna().max())
```

---

## Groupby 를 통한 집계

```ipynb
df.groupby(["시즌", "대회명"])["GAME"].value_counts()
```

---

## 데이터 검산

```ipynb
import numpy as np

import pandas as pd

  

# 허용 오차 (소수점 계산용)

TOL = 1e-6

  

checks = {

"1. FG합계 = 2PM + 3PM":

df["FG합계"] == (df["2PM"] * 2 + df["3PM"] * 3),

  

"2. FT합계 = 1PM":

df["FT합계"] == df["1PM"],

  

"3. PTS = 1PM + 2*2PM + 3*3PM":

df["PTS"] == (df["1PM"] + 2*df["2PM"] + 3*df["3PM"]),

  

"4. 1PM <= 1PA":

df["1PM"] <= df["1PA"],

  

"5. 2PM <= 2PA":

df["2PM"] <= df["2PA"],

  

"6. 3PM <= 3PA":

df["3PM"] <= df["3PA"],

  

"7. 1P% = 1PM / 1PA":

(

np.where(df["1PA"] == 0, 0, df["1PM"] / df["1PA"])

- df["1P%"]

).__abs__() < TOL,

  

"8. 2P% = 2PM / 2PA":

(

np.where(df["2PA"] == 0, 0, df["2PM"] / df["2PA"])

- df["2P%"]

).__abs__() < TOL,

  

"9. 3P% = 3PM / 3PA":

(

np.where(df["3PA"] == 0, 0, df["3PM"] / df["3PA"])

- df["3P%"]

).__abs__() < TOL,

}

  

print("="*60)

print("데이터 검산 결과")

print("="*60)

  

for rule, mask in checks.items():

  

n_error = (~mask).sum()

  

print(f"{rule:<35} : {n_error:,}건")

  

if n_error > 0:

print(

df.loc[

~mask,

["팀명", "선수명", "시즌", "대회명"]

].head(5)

)

print("-"*60)
```



---

```ipynb
tmp = pd.DataFrame({

"2PM": df["2PM"],

"2PA": df["2PA"],

"기록값": df["2P%"],

"계산값": df["2PM"] / df["2PA"]

})

  

tmp["차이"] = (tmp["기록값"] - tmp["계산값"]).abs()

  

tmp.sort_values("차이", ascending=False).head(20)
```

```ipynb
df["선수명"].value_counts()
```

```ipynb
df["팀명_이름"] = df["팀명"] + df["선수명"]

dup_names = df["팀명_이름"].value_counts()

  

dup_names[dup_names > 1]
```

```ipynb
(

df.groupby(["시즌", "대회명"])["팀명_이름"]

.value_counts()

.loc[lambda x: x > 1]

)
```
