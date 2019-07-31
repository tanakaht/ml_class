# Problem 1
## 1.1
batch steepest gradient methodをにおける重み $w$ の更新式は，学習率 $\mu$ を用いて以下のように定義される．

$$ w^{(t+1)} = w^{(t)} - \mu \frac{\partial J(w^{(t)})}{\partial w}$$

以下の終了条件を満たすまで、更新を続ける。

$$ |J(w^t) - J(w^{(t+1)})|<\epsilon$$

## 1.2

Newton method における重み$w$ の更新式は，ヘッセ行列 $\nabla J(w)$ を用いて以下のように定義される．

$$ w^{(t+1)} = w^{(t)} -  {\nabla J(w^{(t)})}^{-1} \cdot J(w^{(t)})$$

以下の終了条件を満たすまで、更新を続ける。

$$ |J(w^t) - J(w^{(t+1)})|<\epsilon$$


## 1.3

それぞれの手法について，訓練終了時の重みを$\hat{w}$とし、各反復ごとの$J(w^{(t)})$と、$J(w^{(t)})-J(\hat{w})$の値の変化をプロットする．  
横軸は反復回数，縦軸は$J(w^{(t)})-J(\hat{w})$の値を対数スケールで表す．

各種、設定について述べる。
デートセットに関して、Toy Datasets のDataset IV を用いた。

両手法とも,重み$w$の初期値として$w=(1, 1, 1, 1, 1)$,正規化項に出現する定数について$\Lambda = 1$, 終了判定に用いる定数について$\epsilon = 1e-6$ とした。

batch steepest gradient method の学習率は0.01と設定した。

結果は以下のようになった。batch steepest gradient method が110回ほどの反復で終了条件を満たしたのに対し、Newton method では5回以内の反復でほぼ値が収束し,10回程度の反復で終了条件を満たしており、収束の速さがわかる.また、最終的にほぼ同じ損失に収束したことがわかる.

![fig1](md/fig/1_3_0.png)
![fig1](md/fig/1_3_3.png)
また、以下のように、データセットとそれぞれの回帰直線を第２成分と第３成分を用いて可視化した。
![fig1](md/fig/1_3_1.png)![fig1](md/fig/1_3_2.png)
両手法が重みについても、ほぼ同じ値に収束し、ほどほどの分類ができていることがわかる。

## 1.4
batch steepest gradient method の実装、評価のみ行った。
表記の簡潔化のため、正解ラベル$y$からone-hot化した行列$Y$を考える。
$$(Y)_{i,j} = [[y_i==j]]$$
多クラスロジスティック回帰についての損失関数に関して、以下のようにかける。
$$J(W)= -\sum_{i}{\log{(softmax(X_iW)_{y_i})}} + \lambda\|W\|_2^2$$
$$\frac{\partial J(W)}{\partial W}= X^T(Y-softmax(XW))+2\lambda W$$

batch steepest gradient methodをにおける重み $w$ の更新式は，学習率 $\mu$ を用いて以下のように定義される．

$$ W^{(t+1)} = W^{(t)} - \mu \frac{\partial J(W^{(t)})}{\partial W} = W^{(t)} - \mu (X^T(Y-softmax(XW))+2\lambda W)$$

以下の終了条件を満たすまで、更新を続ける。

$$ |J(W^t) - J(W^{(t+1)})|<\epsilon$$


batch steepest gradient methodについて，1.3と同様、訓練終了時の重みを$\hat{w}$とし、各反復ごとの$J(w^{(t)})$と、$J(w^{(t)})-J(\hat{w})$の値の変化をプロットする．  
横軸は反復回数，縦軸は$J(w^{(t)})-J(\hat{w})$の値を対数スケールで表す．

各種、設定について述べる。
デートセットに関して、Toy Datasets のDataset V を用いた。

両手法とも,重み$w$の初期値として$w=\left(
    \begin{array}{ccc}
      1 & 1 & 1 \\
      1 & 1 & 1 \\
      1 & 1 & 1 \\
      1 & 1 & 1 \\
      1 & 1 & 1 \\
    \end{array}
  \right)$,正規化項に出現する定数について$\Lambda = 1$ とした。

学習率は0.01と設定した。

結果は以下のようになった。２クラスの時と比べて収束までの反復回数が増えていることがわかる。

![fig1](md/fig/1_4_0.png)
![fig1](md/fig/1_4_1.png)
