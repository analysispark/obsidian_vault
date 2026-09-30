#### 주제 : #Markdown #Obsidian 

#### 출처: https://velog.io/@d2h10s/LaTex-Markdown-%EC%88%98%EC%8B%9D-%EC%9E%91%EC%84%B1%EB%B2%95

# 🔣 수식 표현

## 수식의 정렬

### 왼쪽정렬(기본)

`$`로 수식의 앞 뒤를 감싸면 수식을 작성할 수 있습니다.  
별다른 조치를 취하지 않고 수식을 작성하면 기본적으로 왼쪽 정렬이 됩니다.  
일반 문장 사이에 수식을 넣는 것도 가능합니다.

```null
$x+y=1$
$x$는 $y$와의 합이 $1$이다.
```

$x+y=1$
$x$는 $y$와의 합이 $1$이다.

### 중앙정렬

`$$` 사이에 수식을 적으면 중앙 정렬이 됩니다.  
원래는 `$$`가 어떻게 붙든 상관없지만, velog에서는 반드시 여는`$$`와 닫는 `$$`는 다른 줄에 있어야 합니다.  
다음은 가능한 세 가지 형태입니다.

```null
$$
x+y=1$$

$$x+y=1
$$

$$
x+y=1
$$
```

$$
x+y=1$$

### 특정 문자를 기준으로 정렬

일반적으로 수식을 전개할 때 `=`기호를 기준으로 정렬합니다.  
하지만 그냥 중앙정렬을 하면 다음과 같이 보입니다.

```null
$$
\begin{aligned}
f(x)=ax^2+bx+c \\
g(x)=Ax^4
\end{aligned}
$$
```

$$
\begin{aligned}
f(x)=ax^2+bx+c \\
g(x)=Ax^4
\end{aligned}
$$

이때 `aligned` 심볼을 통하여 특정 문자를 기준으로 정렬할 수 있습니다.  
정렬 기준은 `&`를 기준으로 정렬됩니다.

```null
$$
\begin{aligned}
f(x)&=ax^2+bx+c\\
g(x)&=Ax^4
\end{aligned}$$
```

$$
\begin{aligned}
f(x)&=ax^2+bx+c\\
g(x)&=Ax^4
\end{aligned}$$

---

## 수식 내에서의 줄바꿈

수식에서 `Enter key`를 누른다고 해서 줄바꿈이 되지 않습니다. `\\`를 입력하면 줄바꿈을 할 수 있습니다.

```null
$$x+y=3\\-x+3y=2$$
```

$$x+y=3\\-x+3y=2$$

## 수식 내에서의 띄어쓰기

수식 안에서는 띄어쓰기를 해도 적용되지 않습니다. 다음과 같이 명시적으로 띄어쓰기를 입력하여야 합니다.

```null
$local minimum$(띄어쓰기 적용 X)
$local\,minimum$(띄어쓰기 한 번)
$local\;minimum$(띄어쓰기 두 번)
$local\quad minimum$(띄어쓰기 네 번)
```

$local minimum$(띄어쓰기 적용 X)
$local\,minimum$(띄어쓰기 한 번)
$local\;minimum$(띄어쓰기 두 번)
$local\quad minimum$(띄어쓰기 네 번)

## 곱셈 기호

의외로 많이 쓰나 잘 알지 못하는 기호인 것 같습니다.

```null
y = A \times x + B
```

$$y = A \times x + B$$

---

## 첨자

윗 첨자는 `^` 기호로, 아랫 첨자는 `_` 기호로 적습니다.  
오른쪽에 한 글자가 자동으로 첨자로 들어가게 되고 두 글자 이상을 적용하려면 `{ }`(중괄호)로 감싸면 됩니다.

```null
$a_1, a^2, a_1^2$
$y_i=x_i^3+x_{i-1}^2+x_{i-2}$
```

$a_1, a^2, a_1^2$
$y_i=x_i^3+x_{i-1}^2+x_{i-2}$

---

## 분수 표기법

분수 표기법에는 두 가지 방법이 있습니다.

1. `\over`를 사용하면 \over를 기준으로 왼쪽에 있는 수식은 모두 분자, 오른쪽에 있는 수식은 모두 분모로 들어가게 됩니다.
2. `\frac`을 사용하게 되면 첫 번째 문자는 분자, 두 번째 문자는 분모로 들어가게 됩니다. 두 문자 이상이라면 중괄호`{ }`를 통하여 묶어주면 됩니다.

```null
$s^2+2s+s\over s+\sqrt s+1$
$\frac{1+s}{s(s+2)}$
```

$s^2+2s+s\over s+\sqrt s+1$
$\frac{1+s}{s(s+2)}$

## 절대값 표기법

일반적으로 절대값을 표기할 때는 키보드 위의 `|` 문자를 사용하게 됩니다.  
하지만 이렇게 하면 분수와 같이 큰 객체에 맞게 resizable한 기호를 사용할 수 없습니다.  
그럴 땐 `\vert`와 `\left`, `\right`를 통하여 좌우 기호를 명시해주면 됩니다.

```null
$\vert x \vert$
$\left\lvert \frac{s^2+1}{s^3+2s^2+3s+1} \right\rvert$
```

$\vert x \vert$
$\left\lvert \frac{s^2+1}{s^3+2s^2+3s+1} \right\rvert$

---

## sin, log와 같은 기호를 세워서 표기

단어 앞에 `\`를 붙이게 되면 똑바로 글자를 쓸 수 있습니다.  
Markdown에서 명시 되어 있지 않은 수학 단어라면 오류가 발생합니다.

```null
$\log_{10}{(x+1)}$
$A\sin(bx+c)$
```

$\log_{10}{(x+1)}$
$A\sin(bx+c)$


---

## 극한/시그마 표기법

그냥 `\sum`과 `\lim` 심볼을 사용하게 되면 다음과 같이 linear하게 표기됩니다.

> lims→∞​s2
> 
> ∑i=0∞​(yi​−ti​)2

이럴 땐 `\displaystyle`을 앞에 명시하면 정상적으로 표시됩니다. 기본형인 linear 형태는 `\textstyle` 명시하면 됩니다.

```null
$\displaystyle\lim_{s\rarr\infin}{s^2}$
$\displaystyle\sum_{i=0}^{\infin}{(y_i-t_i)^2}$
```

$\displaystyle\lim_{s\rarr\infin}{s^2}$
$\displaystyle\sum_{i=0}^{\infin}{(y_i-t_i)^2}$

---

## 벡터 표기법

현재 `\vec` 심볼을 벨로그에서 사용할 수 없는 것으로 보입니다.  
`\vec` 대신 `\overrightarrow` 심볼을 사용하시면 화살표가 조금 더 크지만 올바로 출력됩니다.

```null
$\vec{a}$
$\overrightarrow{a}$
```

$\vec{a}$
$\overrightarrow{a}$

---

## 행렬 표기법

`matrix` 심볼을 통하여  
`&`로 열을 구분하고, `\\`로 행을 구분합니다.

```null
$\begin{matrix}1&2\\3&4\\ \end{matrix}$
$\begin{pmatrix}1&2\\3&4\\ \end{pmatrix}$
$\begin{bmatrix}1&2\\3&4\\ \end{bmatrix}$
$\begin{Bmatrix}1&2\\3&4\\ \end{Bmatrix}$
$\begin{vmatrix}1&2\\3&4\\ \end{vmatrix}$
$\begin{Vmatrix}1&2\\3&4\\ \end{Vmatrix}$
```

$\begin{matrix}1&2\\3&4\\ \end{matrix}$

$\begin{pmatrix}1&2\\3&4\\ \end{pmatrix}$

$\begin{bmatrix}1&2\\3&4\\ \end{bmatrix}$

$\begin{Bmatrix}1&2\\3&4\\ \end{Bmatrix}$

$\begin{vmatrix}1&2\\3&4\\ \end{vmatrix}$

$\begin{Vmatrix}1&2\\3&4\\ \end{Vmatrix}$


## 조각함수와 같은 case 표기법

`cases` 심볼을 통하여 작성할 수 있습니다.

```null
$\vert x\vert=
\begin{cases}
-x,\;if\;x<0\\
+x,\;if\;x\geq0
\end{cases}$
```

$$\vert x\vert=
\begin{cases}
-x,\;if\;x<0\\
+x,\;if\;x\geq0
\end{cases}$$

# ♾️ 기호

---

## 화살표

왼쪽 화살표와 오른쪽 화살표는 다음과 같이 적습니다.

```null
$\larr$
$\rarr$ or $\to$
```

> ←→

---

## 부등호 및 유사 등호

좌우 부등호, 근사, 유사, 플러스 마이너스 기호입니다.  
이 문자 외에도 매우 많지만 벨로그에서 표현이 안 되는 기호를 제외하고 자주 사용할만한 것들만 적었습니다.  
`\coloneq` 기호는 원래 줄이 하나가 아니고 두 개로 표시됩니다.

| 심볼                 | 기호  | 심볼                | 기호  |
| ------------------ | --- | ----------------- | --- |
| `$\gt$`            | >   | `$\lt$`           | <   |
| `$\geq$`           | ≥   | `$\leq$`          | ≤   |
| `$\approx$`        | ≈   | `$\sim$`          | ∼   |
| `$\cong$`          | ≅   | `$\approxeq$`     | ≊   |
| `$\simeq$`         | ≃   | `$\eqsim$`        | ≂   |
| `$\doteq$`         | ≐   |                   |     |
| `$\coloneq$`       | :−  | `$\eqcolon$`      | −:  |
| `$\fallingdotseq$` | ≒   | `$\risingdotseq$` | ≓   |
| `$\pm$`            | ±   | `$\mp$`           | ∓   |

## 점

여러 점 기호는 다음과 같은 심볼로 표기할 수 있습니다.  
상 하단 위치 구분을 위해 임의로 abc 문자를 오른쪽에 붙였습니다.  
이 문자 외에도 매우 많지만 벨로그에서 표현이 안 되는 기호를 제외하고 자주 사용할만한 것들만 적었습니다.

| 이름      | 마크다운           | 기호   |
| ------- | -------------- | ---- |
| 점 문자    | `$\dot{a}$`    | a˙   |
| 중간 점    | `$\cdot$`      | ⋅abc |
| 콜론      | `$\colon$`     | :abc |
| 하단 점    | `$\ldotp$`     | .abc |
| 말줄임표    | `$\cdots$`     | ⋯abc |
| 대각 말줄임표 | `$\ddots$`     | ⋱abc |
| 수직 말줄임표 | `$\vdots$`     | ⋮abc |
| 하단 말줄임표 | `$ldots$`      | …abc |
| 왜냐하면    | `$\because$`   | ∵abc |
| 그러므로    | `$\therefore$` | ∴abc |

## 단위 기호

섭씨(\celsius), 옴(\ohm), 위상각(\phase{}) 기호는 존재하나 벨로그에서 표시되지 않습니다.

|이름|마크다운|기호|
|---|---|---|
|도|`$\degree$`|°|
|옴|`$\Omega$`|Ω|


