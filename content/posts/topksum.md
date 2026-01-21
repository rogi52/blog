+++
draft = true
title = 'Top-K Sum'
math = true
date = 2025-12-22T08:30:00+09:00
+++

## 概要
長さ $n$ の数列 $(a_1, a_2, \dots, a_n)$ について，
大きい方から $k$ 個の和 $\mathrm{TopSum} _ {k}(a)$ は
$$
\mathrm{TopSum} _ {k}(a) = \min_{\lambda \in \mathbb{R}} \left\lbrack k\lambda + \sum_{i=1}^{n} \max(0, a_i - \lambda)  \right\rbrack
$$
と表せることを確認します．


## 内容
$\mathrm{TopSum} _ {k}(a)$ は，次の整数計画問題の最適値と等しいです．
$$
\begin{array}{cl}
\text{max.} & \displaystyle \sum_{i=1}^{n} a_i x_i \\\\
\text{s.t.} & \displaystyle \sum_{i=1}^{n} x_i = k \\\\
            & x_i \in \lbrace 0, 1 \rbrace \quad (i = 1, \dots, n) 
\end{array}
$$

制約行列が完全単模なので，$x_i$ の範囲を $[0, 1]$ へ緩和しても最適値は変化しません．
$$
\begin{array}{cl}
\text{max.} & \displaystyle \sum_{i=1}^{n} a_i x_i \\\\
\text{s.t.} & \displaystyle \sum_{i=1}^{n} x_i = k \\\\
            & 0 \leq x_i \leq 1 \quad (i = 1, \dots, n) 
\end{array}
$$

強双対定理より，次の問題の最適値とも等しいです．
$$
\begin{array}{ccc}
\text{min.} & k \lambda + \displaystyle\sum_{i=1}^{n} y_i &   \\\\
\text{s.t.} & y_i + \lambda \geq a_i                      & (i = 1, \dots, n)  \\\\
            & y_i \geq 0                                  & (i = 1, \dots, n) \\\\
            & \lambda \in \mathbb{R} &
\end{array}
$$

$\lambda$ を固定したとき，
$y_i$ はできるだけ小さくしたいので，$y_i = \max(0, a_i - \lambda)$ になります．
よって次の式を得ます．
$$
\mathrm{TopSum} _ {k}(a) = \min_{\lambda \in \mathbb{R}} \left\lbrack k\lambda +  \sum_{i=1}^{n} \max(0, a_i - \lambda)  \right\rbrack
$$

## 応用例
[ABC227-F Treasure Hunting](https://atcoder.jp/contests/abc227/tasks/abc227_f) の Bonus 問題である， $O(H^2 W^2)$ 時間解を導出します．

$(1,1)$ から $(H,W)$ への経路全体を $\mathcal{P}$ とします．
また経路 $P \in \mathcal{P}$ に対して，現れる要素を並べた列を $A_P$ とします．

$$
\begin{aligned}
&\phantom{{}={}}\min_{P \in \mathcal{P}} ~  \mathrm{TopSum} _ {K} ( A_P ) 
\\\\
&{}={}\min_{P \in \mathcal{P}} ~ \min_{\lambda \in \mathbb{R}} \left\lbrack K\lambda + \sum_{(i, j) \in P} \max(0, A_{i, j} - \lambda)  \right\rbrack
\\\\
&{}={}\min_{\lambda \in \mathbb{R}} ~ \min_{P \in \mathcal{P}} \left\lbrack K\lambda + \sum_{(i, j) \in P} \max(0, A_{i, j} - \lambda) \right\rbrack 
\end{aligned}
$$

$\lambda$ を固定したとき，内側の $\min_P \bigl[ K\lambda + \sum \max(0, A_{i, j} - \lambda)\bigr]$ はDPで $O(HW)$ 時間で計算できます．
$\lambda$ は各マスの値 $O(HW)$ 通りを試せば十分なので，全体で $O(H^2 W^2)$ 時間になります．