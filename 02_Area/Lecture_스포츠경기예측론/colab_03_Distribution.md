---
aliases:
  - 단국대학교 스포츠사이언스융합학과
tags:
  - 강의
  - 수업자료
  - 스포츠경기예측
  - colab
  - 분포
  - 베르누이분포
  - 카이제곱분포
  - t분포
  - 정규분포
  - F분포
title: 스포츠경기예측론
author: Jihoon, Park
---
~~~ipynb
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

np.random.seed(1234)

plt.rcParams['figure.figsize'] = (8,5)
plt.rcParams['font.size'] = 12
~~~

## 베르누이 분포

### 개념

동전 1번 던지기
  - 앞면 = 1
  - 뒷면 = 0

~~~ipynb
# 베르누이 분포

p = 0.7

x = stats.bernoulli.rvs(p, size=100) # 베르누이 분포 시뮬레이션

print("평균 =", np.mean(x))

plt.hist(x,
         bins=[-0.5,0.5,1.5],
         density=True,
         edgecolor='black')

plt.xticks([0,1])
plt.title("Bernoulli Distribution")
plt.show()
~~~

## 이항분포

### 개념

- 동전을 10번 던짐
- 성공 횟수를 센다


~~~ipynb
n = 10
p = 0.7

x = stats.binom.rvs(n=n,
                    p=p,
                    size=10000)

print("평균 =", np.mean(x))

plt.hist(x,
         bins=np.arange(-0.5,11.5,1),
         density=True,
         edgecolor='black')

plt.title("Binomial Distribution")
plt.xlabel("Number of Successes")
plt.show()
~~~

## 정규분포

### 이항분포에서 정규분포로

~~~ipynb
# 시행횟수 n을 증가시키기

fig, axes = plt.subplots(1,5, figsize=(15,4))

for ax, n in zip(axes, [10, 50, 100, 1000, 100000]):

    x = stats.binom.rvs(n=n,
                        p=0.5,
                        size=10000)

    ax.hist(x,
            bins=30,
            density=True,
            edgecolor='black')

    ax.set_title(f"n={n}")

plt.tight_layout()
plt.show()
~~~

~~~ipynb
x = np.random.normal(
    loc=100,
    scale=15,
    size=10000
)

print("평균 =", np.mean(x))
print("표준편차 =", np.std(x))

plt.hist(x,
         bins=40,
         density=True,
         edgecolor='black')

plt.title("Normal Distribution")
plt.show()
~~~

~~~ipynb
x = np.random.normal(100,15,10000)

plt.hist(x,
         bins=35,
         density=True,
         alpha=0.7)

xx = np.linspace(40,160,500)

yy = stats.norm.pdf(
    xx,
    loc=100,
    scale=15
)

plt.plot(xx, yy,
         color='red',
         linewidth=3)

plt.title("Normal Distribution")
plt.show()
~~~

## 카이제곱 분포

### 정규분포 값을 제곱한다

- 정규분포의 분산을 검정하고 싶다
- 편차는 음수가 있기 때문에 합이 0
- 제곱해서 나온 값(분산) 분포

~~~ipynb
z = np.random.normal(0,1,10000)

chi = z**2

plt.hist(chi,
         bins=40,
         density=True,
         edgecolor='black')

plt.title("Chi-square Distribution (df=1)")
plt.show()
~~~

~~~ipynb
# 자유도 5
chi = np.random.chisquare(
    df=5,
    size=10000
)

plt.hist(chi,
         bins=40,
         density=True,
         edgecolor='black')

plt.title("Chi-square Distribution (df=5)")
plt.show()
~~~

~~~ipynb
# 자유도에 따른 카이제곱 분포 변화

fig, axes = plt.subplots(1,3, figsize=(15,4))

for ax, df in zip(axes,[1,5,20]):

    x = np.random.chisquare(df=df,
                            size=10000)

    ax.hist(x,
            bins=40,
            density=True,
            edgecolor='black')

    ax.set_title(f"df={df}")

plt.tight_layout()
plt.show()
~~~

## t분포

- 소표본에서 정규분포와 다른 분포가 생기는 것을 발견
- 카이제곱 분포와 결합하여 새로운 분포를 생성

~~~ipynb
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import norm, t

# x축 범위
x = np.linspace(-5, 5, 1000)

# 표준정규분포
normal_pdf = norm.pdf(x)

plt.figure(figsize=(10,6))

# 정규분포
plt.plot(
    x,
    normal_pdf,
    color='black',
    linewidth=3,
    label='Normal Distribution'
)

# t분포들
dfs = [1, 3, 5, 10, 30]

for df in dfs:
    plt.plot(
        x,
        t.pdf(x, df),
        linewidth=2,
        label=f't(df={df})'
    )

plt.title('t Distribution Approaches Normal Distribution')
plt.xlabel('Value')
plt.ylabel('Density')
plt.legend()
plt.grid(alpha=0.3)

plt.show()
~~~

~~~ipynb
from scipy.stats import norm, t
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(-5, 5, 1000)

fig, axes = plt.subplots(
    2,
    2,
    figsize=(12,8)
)

dfs = [1, 3, 10, 30]

for ax, df in zip(axes.ravel(), dfs):

    ax.plot(
        x,
        norm.pdf(x),
        color='black',
        linewidth=3,
        label='Normal'
    )

    ax.plot(
        x,
        t.pdf(x, df),
        color='red',
        linewidth=3,
        label=f't(df={df})'
    )

    ax.set_title(f'df = {df}')
    ax.legend()

plt.tight_layout()
plt.show()
~~~

## F분포

### 통계의 꽃

- 카이제곱 + 카이제곱
- 회귀분석의 가설검정
- 분산분석 (ANOVA - Analysis of Variance)
- 즉, 카이제곱 비율을 분석

~~~ipynb
import seaborn as sns

chi1 = np.random.chisquare(5, 50000)
chi2 = np.random.chisquare(20, 50000)

F = (chi1/5)/(chi2/20)

plt.figure(figsize=(10,6))

sns.kdeplot(chi1,
            label="Chi-square(df=5)")

sns.kdeplot(chi2,
            label="Chi-square(df=20)")

sns.kdeplot(F,
            label="F(5,20)",
            linewidth=3)

plt.xlim(0,10)

plt.legend()
plt.show()
~~~

~~~ipynb
x = np.random.f(
    dfnum=5,
    dfden=30,
    size=10000
)

plt.hist(x,
         bins=50,
         density=True,
         edgecolor='black')

plt.title("F Distribution")
plt.show()
~~~

~~~ipynb
fig, axes = plt.subplots(2,2,
                         figsize=(10,8))

settings = [
    (1,5),
    (5,10),
    (10,20),
    (30,30)
]

for ax, (df1, df2) in zip(axes.ravel(), settings):

    x = np.random.f(df1,
                    df2,
                    10000)

    ax.hist(x,
            bins=40,
            density=True,
            edgecolor='black')

    ax.set_title(f"F({df1},{df2})")

plt.tight_layout()
plt.show()
~~~

## 종합

### 각 분포의 전개
- 베르누이분포
- 이항분포
- 정규분포
- 카이제곱분포
- F분포

~~~ipynb
from scipy.stats import norm, t

fig, axes = plt.subplots(2,3, figsize=(15,8))

# Bernoulli
x = stats.bernoulli.rvs(0.5, size=10000)
axes[0,0].hist(x, bins=[-0.5,0.5,1.5])
axes[0,0].set_title("Bernoulli")

# Binomial
x = np.random.binomial(10,0.5,10000)
axes[0,1].hist(x, bins=11)
axes[0,1].set_title("Binomial")

# Normal
x = np.random.normal(0,1,10000)
axes[0,2].hist(x, bins=40)
axes[0,2].set_title("Normal")

# Chi-square
x = np.random.chisquare(5,10000)
axes[1,0].hist(x, bins=40)
axes[1,0].set_title("Chi-square")

# t vs Normal
xx = np.linspace(-5, 5, 1000)

axes[1,2].plot(
    xx,
    norm.pdf(xx),
    linewidth=3,
    label="Normal"
)

axes[1,2].plot(
    xx,
    t.pdf(xx, df=5),
    linewidth=3,
    label="t(df=5)"
)

axes[1,2].set_title("t vs Normal")
axes[1,2].legend()

# F
x = np.random.f(5,20,10000)
axes[1,1].hist(x, bins=40)
axes[1,1].set_title("F")



plt.tight_layout()
plt.show()
~~~
