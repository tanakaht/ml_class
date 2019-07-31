# Problem 5

## 5.1
$X, Y$を次のよう定義する。
$$X=(\boldsymbol{x_1}, \boldsymbol{x_2}, ... ,\boldsymbol{x_n}).T$$
$$(Y)_{i, j}= \left\{
    \begin{array}{l}
       0  & (i \neq j)\\
       y_i & (i = j)
    \end{array}
  \right. $$

Xから$\tilde{d}$個のピボット$(X_{z_1}, X_{z_1}, ...X_{z_{\tilde{d}}})$をランダムサンプリングにより取得し、ガウシアンカーネル$\phi$用いて、$\tilde{X}$を次のように定義する。
$$
\begin{eqnarray*}
\tilde{X} =&& (\phi(\boldsymbol{x_1}), \phi(\boldsymbol{x_2}), ...,\phi(\boldsymbol{x_n}))^T \\
 =&& (\tilde{\boldsymbol{x_1}}, \tilde{\boldsymbol{x_2}}, ...,\tilde{\boldsymbol{x_n}})^T
\end{eqnarray*}
$$

損失関数は$\sum_{i=0}^{n}{max(0, 1-y_i\boldsymbol{w}\boldsymbol{\tilde{x}_i}})$, 正則化項は$\lambda \|\boldsymbol{w}\|_2^2$ とする。

主問題は以下のようになる。
$$
\begin{aligned}
 \underset{\boldsymbol{w}, \boldsymbol{\xi}} {\text{minimize}} && \lambda \boldsymbol{w}^T\boldsymbol{w} + \boldsymbol{1} \boldsymbol{\xi}  \\
\text{subject to} && \boldsymbol{\xi} \geq \boldsymbol{0}\\
 && \boldsymbol{\xi} \geq \boldsymbol{1} - (\boldsymbol{w}^T \tilde{X}^T Y)^T
  \end{aligned}
$$

ちょうどProblem 3 において、$X$を$\tilde{X}$に置き換えた形になっているので、同様にして最適化できる。

## 5.2
Problem 3 と同様に実装した。

デートセットに関して、Toy Datasets のDataset I を用いた。
各種変数に関して、重み$w, \alpha$の初期値として$\boldsymbol{w}=\boldsymbol{1}, \boldsymbol{\alpha}=\boldsymbol{1}$,正規化項に出現する定数について$\lambda = 1$, 終了判定に用いる定数について$\epsilon = 1e-6$ とした。

ガウシアンカーネルに与えるパラメータ$\alpha$, ピボット数$\tilde{d}$,データセットの要素数$n$を次の範囲で全通り変更しながら、分類の様子、最適化の様子を観察した。
$$
\begin{aligned}
\alpha \in [0.1, 0.2, 0.4, 0.8, 1]\\
\tilde{d} \in [2, 10, 100, 200, 400]\\
n \in [100, 200, 400, 800]
\end{aligned}
$$
以下、それぞれのパラメータについて考察する。

### $\alpha$に関して
$\alpha$が分類結果に与える影響について、以下は$\tilde{d}=100, n=200$で固定し、$\alpha$を変更させていった時の分類の様子である。
![fig1](md/fig/plot/200_100_0.1.png)
![fig1](md/fig/plot/200_100_0.2.png)
![fig1](md/fig/plot/200_100_0.4.png)
![fig1](md/fig/plot/200_100_0.8.png)
![fig1](md/fig/plot/200_100_1.png)
$\alpha$が大きくなるにつれ分類の結果が向上し,0.8と1ではほとんど同じ結果が得られていることが見て取れる。
また、最適化の様子を主問題の目的関数と双対問題の目的関数の差分についてプロットしたものを見る。
![fig1](md/fig/diff/200_100_0.1.png)
![fig1](md/fig/diff/200_100_0.2.png)
![fig1](md/fig/diff/200_100_0.4.png)
![fig1](md/fig/diff/200_100_0.8.png)
![fig1](md/fig/diff/200_100_1.png)
$\alpha$が大きくなるにつれ収束が早くなっていることが見て取れる。

他のパラメータを動かした時の様子について、$\tilde{d}, n$を変更した場合もおおよそ同じ傾向が観れた。

### ピボット数$\tilde{d}$に関して
$\tilde{d}$が分類結果に与える影響について、以下は$\alpha=1, n=400$で固定し、$\tilde{d}$を変更させていった時の分類の様子である。
![fig1](md/fig/plot/400_2_1.png)
![fig1](md/fig/plot/400_10_1.png)
![fig1](md/fig/plot/400_100_1.png)
![fig1](md/fig/plot/400_200_1.png)
![fig1](md/fig/plot/400_400_1.png)
$\tilde{d}$が大きくなるにつれ分類の結果が向上していることが見て取れる。

また、最適化の様子を主問題の目的関数と双対問題の目的関数の差分についてプロットしたものを見る。
![fig1](md/fig/diff/400_2_1.png)
![fig1](md/fig/diff/400_10_1.png)
![fig1](md/fig/diff/400_100_1.png)
![fig1](md/fig/diff/400_200_1.png)
![fig1](md/fig/diff/400_400_1.png)
特徴量の次元が増えるため、$\tilde{d}$が大きくなるにつれ収束が遅くなっていることが見て取れる。

他のパラメータを動かした時の様子について、$n>\tilde{d}$となる場合を除いて、$n$を変更した場合もおおよそ同じ傾向が観れた。

$\alpha$を動かした時の様子に関して、以下は、$\alpha=0.1, n=400$で固定し、$\tilde{d}$を変更させていった時の分類の様子である。
![fig1](md/fig/plot/400_2_0.1.png)
![fig1](md/fig/plot/400_10_0.1.png)
![fig1](md/fig/plot/400_100_0.1.png)
![fig1](md/fig/plot/400_200_0.1.png)
![fig1](md/fig/plot/400_400_0.1.png)

このように、$\alpha$が小さすぎる場合、$\tilde{d}$を大きくしても学習がうまくいかない様子が観れた。

### 要素数$n$に関して
$n$が分類結果に与える影響について、以下は$\alpha=1, \tilde{d}=200$で固定し、$\tilde{d}$を変更させていった時の分類の様子である。
![fig1](md/fig/plot/100_100_1.png)
![fig1](md/fig/plot/200_100_1.png)
![fig1](md/fig/plot/400_100_1.png)
![fig1](md/fig/plot/800_100_1.png)
$n$が大きくなるにつれ元のドーナツ型のような分布を学習している様子が見て取れる。

また、最適化の様子を主問題の目的関数と双対問題の目的関数の差分についてプロットしたものを見る。
![fig1](md/fig/diff/100_2_1.png)
![fig1](md/fig/diff/200_10_1.png)
![fig1](md/fig/diff/400_100_1.png)
![fig1](md/fig/diff/800_200_1.png)
$n$が大きくなるにつれ収束が遅くなっていることが見て取れる。

十分大きな$\alpha, \tilde{d}$については、おおよそ同じ傾向が観れた。