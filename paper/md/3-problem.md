# Problem 3

## 3.1
$X, Y$を次のよう定義する。
$$X=(\boldsymbol{x_1}, \boldsymbol{x_2}, ... \boldsymbol{x_n}).T$$
$$(Y)_{i, j}= \left\{
    \begin{array}{l}
       0  & (i \neq j)\\
       y_i & (i = j)
    \end{array}
  \right. $$

主問題は以下のようになる。
$$
\begin{aligned}
 \underset{\boldsymbol{w}, \boldsymbol{\xi}} {\text{minimize}} && \lambda \boldsymbol{w}^T\boldsymbol{w} + \boldsymbol{1} \boldsymbol{\xi}  \\
\text{subject to} && \boldsymbol{\xi} \geq \boldsymbol{0}\\
 && \boldsymbol{\xi} \geq \boldsymbol{1} - (\boldsymbol{w}^T X^T Y)^T
  \end{aligned}
$$
ラグランジェ関数:$L$は以下のようになる。
$$
L(\boldsymbol{w}, \boldsymbol{\xi}, \boldsymbol{\mu}, \boldsymbol{\nu}) = \lambda \boldsymbol{w}^T \boldsymbol{w}+\boldsymbol{1}^T\boldsymbol{\xi} - \boldsymbol{\mu}^T\boldsymbol{\xi} - \boldsymbol{\nu}^T(\boldsymbol{\xi} -(\boldsymbol{1} - (\boldsymbol{w}^T X^T Y)^T))
$$
ラグランジェ双対関数:$\tilde{L}$は以下のようになる。
$$
\begin{eqnarray*}
 \tilde{L}(\boldsymbol{\mu}, \boldsymbol{\nu}) && =  \underset{\boldsymbol{w}, \boldsymbol{\xi}\in D}{\text{inf}} L(\boldsymbol{w}, \boldsymbol{\xi}, \boldsymbol{\mu}, \boldsymbol{\nu})\\ 
&& = \underset{\boldsymbol{w}, \boldsymbol{\xi}\in D}{\text{inf}}   \lambda(\boldsymbol{w}-\frac{1}{2\lambda}X^TY\boldsymbol{\nu})^T(\boldsymbol{w}-\frac{1}{2\lambda}X^TY\boldsymbol{\nu}) + (\boldsymbol{1}-\boldsymbol{\mu}-\boldsymbol{\nu})^T\boldsymbol{\xi} - \frac{1}{4\lambda}(X^TY\boldsymbol{\nu})^T(X^TY\boldsymbol{\nu})+\boldsymbol{1}^T\boldsymbol{\nu}\\ 
&& =  \begin{array}{ll}
       - \frac{1}{4\lambda}(X^TY\boldsymbol{\nu})^T(X^TY\boldsymbol{\nu})+\boldsymbol{1}^T\boldsymbol{\nu}  & ((\boldsymbol{1}-\boldsymbol{\mu}-\boldsymbol{\nu})>\boldsymbol{0})\\
       -\infty & (else)
      \end{array}
\end{eqnarray*}
$$
双対問題は$\boldsymbol{\nu}\geq 0, \boldsymbol{\mu} \geq 0$条件下での$\tilde{L}$の最大化問題である。そのため、$((\boldsymbol{1}-\boldsymbol{\mu}-\boldsymbol{\nu})>\boldsymbol{0})$を満たす必要がある。また、$\boldsymbol{\mu}$については無視できる。
そのため、$\boldsymbol{K}=(X^TY)^T(X^TY), \boldsymbol{\alpha}=\boldsymbol{\nu}$とすると、双対問題は以下のようにかける。
$$
\begin{aligned}
 \underset{\boldsymbol{\alpha}} {\text{maximize}} && - \frac{1}{4\lambda}\boldsymbol{\alpha}^T\boldsymbol{K}\boldsymbol{\alpha}+\boldsymbol{\alpha}^T\boldsymbol{1}^T\\
\text{subject to} && \boldsymbol{1} \geq \boldsymbol{\alpha} \geq \boldsymbol{0}\\
  \end{aligned}
$$

## 3.2
$$\frac{\partial L}{\partial w}=2\lambda\boldsymbol{w}-X^TY\boldsymbol{\nu}$$
KKT条件から、$\frac{\partial L}{\partial w}=0$より、
$$\boldsymbol{w}=\frac{X^TY\boldsymbol{\nu}}{2\lambda}$$
双対問題の解を$\alpha$としたので、
$$
\begin{eqnarray*}
\hat{\boldsymbol{w}}=&&\frac{X^TY\boldsymbol{\alpha}}{2\lambda}\\
=&&\frac{1}{2\lambda}\sum_{i}{\alpha_i y_i\boldsymbol{x}_i}\\
\end{eqnarray*}
$$
となる。

## 3.3
$\alpha$の更新式に間違いがあると感じた。
$$
\boldsymbol{\alpha}^{(t)} = P_{[0,1]^n}(\boldsymbol{\alpha}^{(t-1)}-\eta_t(\frac{1}{2\lambda}\boldsymbol{K}\boldsymbol{\alpha}-\boldsymbol{1}))
$$
と表記されているが、正しくは
$$
\begin{equation}
\label{update}
\boldsymbol{\alpha}^{(t)} = P_{[0,1]^n}(\boldsymbol{\alpha}^{(t-1)}-\eta_t(\frac{1}{2\lambda}\boldsymbol{K}\boldsymbol{\alpha}^{(t-1)}-\boldsymbol{1}))
\end{equation}
$$
であると思う。

上の式による$\alpha$の更新を、双対問題の目的関数を$D(\boldsymbol{\alpha})$とし、以下の終了条件を満たすまで、繰り返す。

$$ 
|D(\boldsymbol{\alpha}^{(t)}) - D(\boldsymbol{\alpha}^{(t-1)})|< \epsilon
$$

$\boldsymbol{\alpha}$の更新のたび、3.2で求めた式で、$\boldsymbol{w}$の更新を行う。


更新のたび、主問題の目的関数、双対問題の目的関数を求め、それぞれの値と差分をプロットする。

デートセットに関して、Toy Datasets のDataset II を用いた。
各種変数に関して、重み$w, \alpha$の初期値として$\boldsymbol{w}=\boldsymbol{1}, \boldsymbol{\alpha}=\boldsymbol{1}$,正規化項に出現する定数について$\lambda = 1$, 終了判定に用いる定数について$\epsilon = 1e-6$ とした。

![fig1](md/fig/3_3_both.png)
![fig1](md/fig/3_3_diff.png)
初期の値の変化が大きすぎるので、反復200回以降のプロットをした。
![fig1](md/fig/3_3_both_200.png)
![fig1](md/fig/3_3_diff_200.png)

主問題の目的関数、双対問題の目的関数の差が十分小さくなり、最適化がなされた。

dataset II の分類の様子は以下のようになった。
![fig1](md/fig/3_3_classify.png)
