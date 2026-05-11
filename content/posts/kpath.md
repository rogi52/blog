+++
date = '2026-05-07T20:06:30+09:00'
draft = false
title = '$k$-$\mathsf{PATH}$ を $2^k \cdot n^{O(1)}$ 時間で解くアルゴリズムを学ぶ'
+++


{{< box color="teal" title="$k$-$\mathsf{PATH}$" id="prob-label" numbering="false" >}}
$n$ 頂点の単純無向グラフが与えられる．
$k$ 頂点のパスが存在するか判定せよ．
{{< /box >}}

$k$-$\mathsf{PATH}$ 問題を解くアルゴリズムとして Color-Coding[^1] が有名ですが，
ここでは代数的な手法を用いて高速な $O^*(2^k)$ 時間を達成するアルゴリズム[^2] を解説します．

## アルゴリズム

各頂点 $v$ に値 $x_v$ を割り当てます．
$$ x_{v} = \sum_{i=1}^{k} r_{v, i} z_i \in \operatorname{GF}(2^l)[z_1, \dots, z_k]/\langle z_1^2, \dots, z_k^2 \rangle$$
ここで $r_{v, i}$ は $\operatorname{GF}(2^l)$ から一様ランダムに選んだ値です．
また，無向辺 $\lbrace u, v\rbrace$ を 2 本の有向辺 $(u, v), (v, u)$ として，
全ての有向辺 $(u, v)$ に重み $w_{u, v} \in \operatorname{GF}(2^l)$ を割り当てます．
ただし $l = \lceil \log_2(4k) \rceil$ とします．

全 $k$-walk の重みの積の総和を考えます．
$$ S \coloneqq \sum_{(v_1, \dots, v_k) \colon k\text{-walk}} \prod_{i=1}^{k} x_{v_i} \prod_{i=1}^{k-1} w_{v_i, v_{i+1}} $$
これは DP で高速に計算できます．
$\operatorname{dp}(v, i) \coloneqq {}$ 頂点 $v$ を端点とする，頂点数 $i$ の walk の重みの積の総和，と定義します．
$\operatorname{dp}(v, 1) \xleftarrow{} x_{v}$ と初期化した後，
各辺 $(u, v)$ に対して
$$ \operatorname{dp}(v, i) \xleftarrow{+} \operatorname{dp}(u, i-1) \cdot x_{v} \cdot w_{u, v} $$ の遷移式に従って計算すれば良いです．
管理すべき項数は $2^k$ 個なので， 遷移あたり $O^*(2^k)$ 時間です．

{{< box color="orange" title="" id="claim-label" numbering="false" >}}
$k$-path が存在する $\iff$ $[z_1 \cdots z_k]S \neq 0$ 
{{< /box >}}


walk が同じ頂点を通る場合を考えます．
重み積の中に $x_v^2$ が出てきますが，
$$
x_v^2 = \left( \sum_{i=1}^{k} r_{v, i} z_i \right)^2 
= \sum_{i=1}^{k} (r_{v, i} z_i)^2 + \sum_{i < j} 2\\,r_{v, i} r_{v, j} z_i z_j = 0
$$
最後の等号は $z_i^2 = 0$ と $2\\, r_{v, i} r_{v, j} = 0$ よりわかります．
したがって，自己交差する walk の重みは消えます．

一方 walk が同じ頂点を通らない，つまり path の場合，
使う有向辺の集合が一意に定まるため，他の path と相殺されません．
さらに，頂点重み部分 $x_{v_1} x_{v_2} \cdots x_{v_k}$ から $z_1 z_2 \cdots z_k$ の項が残ります．

{{< fold color="gray" title="辺重みの必要性" >}}
辺重みをなくし，頂点重みだけで考えてみます．
パス $(v_1, v_2, \dots, v_k)$ に対して，
そのパスを反転したパス $(v_k, v_{k-1}, \dots, v_1)$ も全く同じ重みを持つので，
項がちょうど打ち消し合ってしまい，path を検出できません．
{{< /fold >}}

$[z_1 \cdots z_k]S$ の非 $0$ 判定は， $r$ と $w$ にランダムな値を代入して行います．
$r$ について $k$ 次，$w$ について $(k-1)$ 次の計 $(2k-1)$ 次多項式なので，
Schwartz-Zippel の補題より
$$ \frac{2k - 1}{2^l} < \frac{2k}{4k} = \frac{1}{2} $$
$l = \lceil \log_2(4k) \rceil$ と設定していました．
以上で片側誤り確率 $\frac{1}{2}$ 未満のアルゴリズムが完成です．




## 空間計算量の改善

先ほどの解法では， DP テーブルの各状態で $2^k$ 項を管理する必要があり，指数サイズの空間を必要とします．
ここでは，包除原理を用いて空間計算量を多項式に落とします．

計算に $z_i^2 = 0$ を課さない場合を考えます．
つまり $S \in \operatorname{GF}(2^l)[z_1, \dots, z_k]$ をそのまま展開したとします．
自己交差する walk は，高々 $(k-1)$ 種類の頂点しか通らないので，生成される項には必ず欠けた変数 $z_i$ が存在します．
一方で path は全ての変数を 1 回ずつ使った $z_1 z_2 \cdots z_k$ の項を生成します．

したがって，$S$ から $z_1 z_2 \cdots z_k$ の項だけ計算できれば，パスの存在判定ができます．
$T \subseteq [k]$ に対して，
$S(T)$ を「$S$ の変数に $i \in T$ なら $z_i = 1$，$i \notin T$ なら $z_i = 0$ を代入した評価値」とすると，求める係数は次のように抽出できます．
$$ [z_1 \dots z_k] S = \sum_{T \subseteq [k]} (-1)^{k - |T|} S(T) = \sum_{T \subseteq [k]} S(T) $$
よって $S(T)$ の和を管理する変数を持っておき，
$0$ と $1$ の割り当て $2^k$ 通りに対して，
スカラー値（$\operatorname{GF}(2^l)$ の元）で 多項式時間の DP をします．
時間計算量を $O^*(2^k)$ 時間に保ちつつ，
空間計算量は， スカラー値を保持する DP テーブルの多項式サイズで済みます．



## 終わりに

自己交差を打ち消すところが上手いですね．

さらに指数の底を改善するアルゴリズムもあるようです[^3] ．





[^1]: Color-Coding の解説は次の記事が参考になります．  
[AtCoder Algorithm Lectures. Color-Coding.](https://info.atcoder.jp/entry/algorithm_lectures/color_coding)  
[Le Algorithm. color-codingと脱乱択化.](https://lealgorithm.blogspot.com/2018/11/color-coding.html)

[^2]: Williams, Ryan. "Finding paths of length $k$ in $O^*(2^k)$ time." Information Processing Letters 109.6 (2009): 315-318.

[^3]: Björklund, Andreas, et al. "Narrow sieves for parameterized paths and packings." Journal of Computer and System Sciences 87 (2017): 119-139.