
├── data
│   └── basketball
│       └── 남고부_통합데이터.xlsx
├── docs
│   ├── architecture.md
│   ├── overview.md
│   └── system_design.md
├── example.py
├── [[#KUSF_Dashboard.py]]
├── /[[#pages]]
│   ├──[[#01_경기력평가지표.py]]
│   ├── 02_농구.py
│   ├── 03_배구.py
│   └── 04_야구.py
├── requirements.txt
└── utils
    ├── __pycache__
    │   └── sample_data.cpython-311.pyc
    ├── charts.py
    ├── data_loader.py
    ├── metrics.py
    └── sample_data.py


ToDo
- [ ] [[#KUSF_Dashboard.py]]  col1~3 value 를 실제 데이터 행 range 로 변경
- [ ] [[#01_경기력평가지표.py]] 산식 수정



## KUSF_Dashboard.py

```python
import streamlit as st

st.set_page_config(
    page_title="KUSF 경기력평가지표 Dashboard",
    page_icon="🏆",
    layout="wide",
)

st.title("🏆 KUSF 체육특기자 경기력 평가지표 Dashboard")

col1, col2, col3 = st.columns(3)

with col1:
    st.metric(
        label="🏀 농구",
        value="2631명",
        delta="Prototype",
    )

with col2:
    st.metric(
        label="🏐 배구",
        value="5명",
        delta="Prototype",
    )

with col3:
    st.metric(
        label="⚾ 야구",
        value="5명",
        delta="Prototype",
    )

st.divider()

st.markdown(
    """
### 프로젝트 개요

- KUSF 체육특기자 경기력 평가지표 Dashboard
- 농구 / 배구 / 야구 경기기록 분석
- 연구진 내부 모니터링 시스템

좌측 메뉴에서 각 종목 페이지를 선택하세요.
"""
)
```

## pages

### 01_경기력평가지표.py

```python
import streamlit as st

st.title("📐 경기력 평가지표")

st.markdown(
    r"""
## 농구 경기력 지표

$$
PI = \frac{득점 + 리바운드 + 어시스트}{경기수}
$$

(예시)

---

## 배구 경기력 지표

$$
PI = 공격성공률 + 블로킹 + 서브
$$

(예시)

---

## 야구 경기력 지표

$$
PI = 안타 + 타점 + 득점
$$

(예시)
"""
)
```

### 02_농구.py
```python
import streamlit as st
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

from utils.data_loader import load_basketball_data

import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

plt.rcParams["font.family"] = "NanumGothic"
plt.rcParams["axes.unicode_minus"] = False

st.title("🏀 농구")

df = load_basketball_data()

min_min = int(df["MIN"].min())
max_min = int(df["MIN"].max())

selected_min = st.slider(
    "출전시간(MIN)",
    min_value=min_min,
    max_value=max_min,
    value=(min_min, max_min),
)

filtered_df = df[
    (df["MIN"] >= selected_min[0]) &
    (df["MIN"] <= selected_min[1])
]

num_df = filtered_df.select_dtypes(include="number")
num_df = num_df.drop(columns=["MIN", "참가대회 8.1", "시즌"], errors="ignore")

cols = st.columns(2)

for idx, col in enumerate(num_df.columns):

    fig, ax = plt.subplots(figsize=(8, 5))

    sns.histplot(
        data=num_df,
        x=col,
        bins=20,
        kde=True,
        ax=ax
    )

    ax.set_title(col)

    with cols[idx % 2]:
        st.pyplot(fig)
```

