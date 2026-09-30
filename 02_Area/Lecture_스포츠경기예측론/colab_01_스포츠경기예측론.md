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
print("안녕하세요. 이번 시간에는 Google Colab을 살펴보겠습니다.")
```

```ipynb
name = "박지훈"

print(name)
```

```ipynb
print("저의 이름은 " + name + "입니다")
```

```ipynb
print(f"저의 이름은 {name} 입니다.")
```

```ipynb
age = 20

  

print(f'저의 이름은 {name} 이고, 제 나이는 {age + 20}살 입니다.')
```

---

## Google Docs 자료 불러오기


```ipynb
from google.colab import auth

auth.authenticate_user()

  

import gspread

from google.auth import default

creds, _= default()

  

gc = gspread.authorize(creds)

  

url = "https://docs.google.com/spreadsheets/d/1TebxQF1zfaibXKJ1-M1TVSPDVlLP3jIQA3Td3vlDb88/edit?usp=sharing"

sh = gc.open_by_url(url)

  

worksheet = sh.get_worksheet(0)

  

# Get DATA

import pandas as pd

  

rows = worksheet.get_all_records()

df = pd.DataFrame(rows)

https://docs.google.com/spreadsheets/d/1UBaWtduC8gdCe5i2CYf7WH99HE3l3GGA/edit?usp=sharing&ouid=115917995092265390354&rtpof=true&sd=true

df.head()
```

---

### 산술평균

$$\bar{X} = \frac{1}{n} \sum_{i=1}^{n} X_i$$

  

### 표준편차

$$ s=\left( \frac{\sum_{i=1}^{n}(X_i-\bar{X})^2} {n-1} \right)^{\frac{1}{2}} $$

  
  

### 피어슨 상관계수

$$r = \frac{\sum_{i=1}^{n} (X_i - \bar{X})(Y_i - \bar{Y})}{\sqrt{\sum_{i=1}^{n} (X_i - \bar{X})^2 \sum_{i=1}^{n} (Y_i - \bar{Y})^2}}$$

---

```ipynb
x_mean = df["x"].mean()

y_mean = df["y"].mean()

  

print(f" x={x_mean} \n y={y_mean}") # \n 은 줄바꿈 코드
```

```ipynb
import numpy as np

  

cor_r = np.corrcoef(df["x"], df["y"])

print(cor_r)
```

```ipynb
import matplotlib.pyplot as plt

import seaborn as sns

  

plt.figure(figsize=(8, 6))

sns.scatterplot(x='x', y='y', data=df)

plt.title('Scatter Plot of X vs Y')

plt.xlabel('X')

plt.ylabel('Y')

plt.grid(True)

plt.show()
```

