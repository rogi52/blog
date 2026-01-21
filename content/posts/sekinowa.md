+++
date = '2026-01-14T14:39:49+09:00'
draft = true
title = '積の和典型'
math = true
toc = true
+++

- [1問目: ABC399 F - Range Power Sum](#1問目-abc399-f---range-power-sum)
- [2問目: ABC330 G - Inversion Squared](#2問目-abc330-g---inversion-squared)
- [3問目: ABC277 G - Random Walk to Millionaire](#3問目-abc277-g---random-walk-to-millionaire)
- [4問目: ABC231 G - Balls in Boxes](#4問目-abc231-g---balls-in-boxes)
- [5問目: ARC182 C - Sum of Number of Divisors of Product](#5問目-arc182-c---sum-of-number-of-divisors-of-product)
- [6問目: ARC157 C - YY Square](#6問目-arc157-c---yy-square)
- [7問目: ARC124 E - Pass to Next](#7問目-arc124-e---pass-to-next)
- [8問目: ARC110 D - Binomial Coefficient is Fun](#8問目-arc110-d---binomial-coefficient-is-fun)
- [9問目: 第6回 ドワンゴからの挑戦状 予選 C - Cookie Distribution](#9問目-第6回-ドワンゴからの挑戦状-予選-c---cookie-distribution)
- [10問目: TTPC2023 P - Bridge Elimination](#10問目-ttpc2023-p---bridge-elimination)



## 1問目: [ABC399 F - Range Power Sum](https://atcoder.jp/contests/abc399/tasks/abc399_f)

$$
\begin{align*}
&\phantom{=}\sum_{r = 1}^{N} \sum_{l=0}^{r} \left( \sum_{l \leq i < r} A_i \right)^K \\\\
&= \sum_{r = 1}^{N} \sum_{l=0}^{r} \left( S_r - S_l \right)^K \\\\
&= \sum_{r = 1}^{N} \sum_{l=0}^{r} \left[\frac{x^K}{K!}\right] \exp\left({(S_r - S_l)x}\right) \\\\
&= \sum_{r = 1}^{N} \sum_{l=0}^{r} \left[\frac{x^K}{K!}\right] \exp(S_r x) \exp(-S_l x) \\\\
&= \left[\frac{x^K}{K!}\right] \sum_{r = 1}^{N} \exp(S_r x) \left(\sum_{l=0}^{r} \exp(-S_l x)\right)
\end{align*}
$$

[drogskolさんの解説](https://atcoder.jp/contests/abc399/editorial/12577) では積に $\Theta(K^2)$ 時間かけていますが，欲しいのは $x^K$ の係数だけなので，$\Theta(K)$ 時間で計算できます．
よって全体で $O(NK)$ 時間です．


## 2問目: [ABC330 G - Inversion Squared](https://atcoder.jp/contests/abc330/tasks/abc330_g)

$$
\begin{align*}
&\phantom{=} \sum_{P} \left(\sum_{i < j} 1_{P_i > P_j}\right)^2 \\\\
&= \sum_{P} \left[\frac{x^2}{2!}\right] \exp\left(\left(\sum_{i < j} 1_{P_i > P_j}\right) x\right) \\\\
&= \sum_{P} \left[\frac{x^2}{2!}\right] \prod_{i < j} \exp(1_{P_i > P_j} x) \\\\
&= \sum_{P} \left[\frac{x^2}{2!}\right] \prod_{i < j} (1 + 1_{P_i > P_j} x + (1_{P_i > P_j})^2 x^2 / 2!) \\\\
&= \sum_{P} \left( \sum_{i<j} 1_{P_i > P_j} + \sum_{(i, j) \neq (k, l)} 1_{P_i > P_j} 1_{P_k > P_l} \right)
\end{align*}
$$


## 3問目: [ABC277 G - Random Walk to Millionaire](https://atcoder.jp/contests/abc277/tasks/abc277_g)

$$
\begin{align*}
&\phantom{=} \sum_{W} \left( \sum_{v \in W} 1_{C_v = 0} \right)^2 \\\\
&= \sum_{W} \left[\frac{x^2}{2!}\right] \exp\left(\left( \sum_{v \in W} 1_{C_v = 0} \right)x\right) \\\\
&= \sum_{W} \left[\frac{x^2}{2!}\right] \prod_{v \in W} \exp((1_{C_v = 0} )x) \\\\
&=  \left[\frac{x^2}{2!}\right] \sum_{W}  \prod_{v \in W} \exp((1_{C_v = 0} )x)
\end{align*} \\\\
$$


## 4問目: [ABC231 G - Balls in Boxes](https://atcoder.jp/contests/abc231/tasks/abc231_g)

$B_i = A_i + X_i$ とします．

$$
\begin{align*}
&\phantom{=} \frac{1}{N^K} \sum_{X_1 + X_2 + \dots X_N = K} \frac{K!}{X_1! X_2! \dots X_N!} \prod_{i=1}^{N} (A_i + X_i) \\\\
&= \frac{K!}{N^K} \sum_{X_1 + X_2 + \dots X_N = K} \prod_{i=1}^{N} \frac{A_i + X_i}{X_i!} \\\\
&= \frac{K!}{N^K} [x^K] \prod_{i=1}^{N} \left( \sum_{m=0}^{\infty} (A_i + m) \frac{x^m}{m!} \right) \\\\
&= \frac{K!}{N^K} [x^K] \prod_{i=1}^{N} (A_i + x) e^x \\\\
&= \frac{K!}{N^K} [x^K] e^{Nx} \prod_{i=1}^{N} (A_i + x) \\\\
\end{align*}
$$

$\prod_{i=1}^{N} (A_i + x) = \sum_{j=0}^{N} c_j x^j$ とおくと，

$$
\begin{align*}
\frac{K!}{N^K} \sum_{j=0}^{N} \frac{N^{K-j}}{(K-j)!} \cdot c_j 
 = \sum_{j=0}^{N} \frac{c_j}{N^j} \cdot \frac{K!}{(K-j)!}
\end{align*}
$$

$\prod_{i=1}^{N} (A_i + x)$ はマージテクで $O(N (\log N)^2)$ 時間で計算できます．
最後の集計部分は $O(N)$ 時間で計算できるので，全体で $O(N (\log N)^2)$ 時間です．


## 5問目: [ARC182 C - Sum of Number of Divisors of Product](https://atcoder.jp/contests/arc182/tasks/arc182_c)

## 6問目: [ARC157 C - YY Square](https://atcoder.jp/contests/arc157/tasks/arc157_c)

$\texttt{XXYY}$ であれば，$P = \lbrace \texttt{XX}, \texttt{XY}, \texttt{YY} \rbrace$ です．

$$
\begin{align*}
&\phantom{=}\sum_{P} \left(\sum_{e \in P} 1_{e = \texttt{YY}}\right)^2 \\\\
&= \sum_{P} \left[\frac{x^2}{2!}\right] \exp\left(\left(\sum_{e \in P} 1_{e = \texttt{YY}}\right)x\right) \\\\
&= \sum_{P} \left[\frac{x^2}{2!}\right] \prod_{e \in P} \exp((1_{e=\texttt{YY}})x) \\\\
&= \left[\frac{x^2}{2!}\right] \sum_{P} \prod_{e \in P} \exp((1_{e=\texttt{YY}})x)
\end{align*}
$$

$\mathrm{dp} _ {i, j} \coloneqq {}$ $(i, j)$ までのパス $P$ について，
$\prod _ {e \in P} \exp((1 _ {e=\texttt{YY}})x)$ の和とします．
$\mathrm{dp} _ {i, j} = \mathrm{dp} _ {i-1, j} \cdot \exp((1_{s_{i-1,j}s_{i,j}=\texttt{YY}})x) + \mathrm{dp_{i, j-1}} \cdot \exp((1_{s_{i,j-1}s_{i,j}=\texttt{YY}})x)$ から計算できます．
高々次数 $2$ まで管理すれば良いので，$O(HW)$ 時間です．


## 7問目: [ARC124 E - Pass to Next](https://atcoder.jp/contests/arc124/tasks/arc124_e)

$$
\begin{align*}
&\phantom{=} \sum \prod_{i=1}^{N} (a_i - x_i + x_{i-1}) \\\\
&= \sum \prod_{i=1}^{N} [z_i] \exp((a_i - x_i + x_{i-1})z_i) \\\\
&= \sum [z_1 z_2 \dots z_N] \exp\left( \sum_{i=1}^{N} (a_i - x_i + x_{i-1}) z_i \right) \\\\
&= \sum [z_1 z_2 \dots z_N] \exp\left( \sum_{i=1}^{N} a_i z_i + \sum_{i=1}^{N} x_i (z_{i+1} - z_i) \right) \\\\
&= [z_1 z_2 \dots z_N] \exp\left(\sum_{i=1}^{N} a_i z_i\right) \prod_{i=1}^{N} \sum_{x_i=L}^{a_i} \exp(x_i(z_{i+1} - z_i))
\end{align*}
$$

## 8問目: [ARC110 D - Binomial Coefficient is Fun](https://atcoder.jp/contests/arc110/tasks/arc110_d)

$$
\begin{align*}
&\phantom{=} [x^M] \frac{1}{1 - x} \prod_{i=1}^{N} \left( \sum_{n=0}^{\infty} \binom{n}{A_i} x^n \right) \\\\
&= [x^M] \frac{1}{1 - x} \prod_{i=1}^{N} \frac{x^{A_i}}{(1 - x)^{A_i+1}} \\\\
&= [x^M] \frac{x^{S}}{(1 - x)^{S + N + 1}} \\\\
&= [x^{M-S}] \frac{1}{(1 - x)^{S + N + 1}} \\\\
&= \binom{M + N}{S + N}
\end{align*}
$$

$S \coloneqq \sum_{i=1}^{N} A_i$ としました．


## 9問目: [第6回 ドワンゴからの挑戦状 予選 C - Cookie Distribution](https://atcoder.jp/contests/dwacon6th-prelims/tasks/dwacon6th_prelims_c)

$$
\begin{align*}
&\phantom{=} \sum \prod_{i=1}^{N} c_i \\\\
&= \sum \prod_{i=1}^{N} [y_i] \exp(c_i y_i) \\\\
&= [y_1 y_2 \dots y_N] \sum \exp\left(\sum_{i=1}^{N} c_i y_i\right) \\\\
&= [y_1 y_2 \dots y_N] \sum \exp\left(\sum_{i=1}^{N} \left(\sum_{j=1}^{K} x_{i,j}\right) y_i\right) \\\\
&= [y_1 y_2 \dots y_N] \sum \exp\left(\sum_{j=1}^{K} \left(\sum_{i=1}^{N} x_{i,j} y_i\right)\right) \\\\
&= [y_1^1 y_2^1 \dots y_N^1] \sum \prod_{j=1}^{K} \exp \left( \sum_{i=1}^{N} x_{i, j} y_i \right) \\\\
&= [y_1^1 y_2^1 \dots y_N^1] \prod_{j=1}^{K} \left( \sum_{\sum_{i} x_{i, j} = a_j} \exp\left( \sum_{i=1}^{N} x_{i,j} y_i \right) \right) \\\\
&= [y_1^1 y_2^1 \dots y_N^1] \prod_{j=1}^{K} \left( [T^{a_j}] \prod_{i=1}^{N} (1 + e^{y_i} T) \right) \\\\
&= [y_1^1 y_2^1 \dots y_N^1] \prod_{j=1}^{K} \left( [T^{a_j}] \prod_{i=1}^{N} (1 + (1 + y_i) T) \right) \\\\
&= \sum_{m_1 + m_2 + \dots m_K = N} \frac{N!}{m_1! m_2! \dots m_K!} \prod_{j=1}^{K} [T^{a_j}] (1 + T)^{N-m} T^{m} \frac{x^m}{m!} \\\\
&= \sum_{m_1 + m_2 + \dots m_K = N} \frac{N!}{m_1! m_2! \dots m_K!} \prod_{j=1}^{K} [T^{a_j - m}] (1 + T)^{N-m} \frac{x^m}{m!} \\\\
&= \sum_{m_1 + m_2 + \dots m_K = N} \frac{N!}{m_1! m_2! \dots m_K!} \prod_{j=1}^{K} \binom{N - m}{a_j - m} \frac{x^m}{m!} \\\\
&= \left[ \frac{x^N}{N!} \right] \prod_{j=1}^{K} \left( \sum_{m=0}^{a_j} \binom{N - m}{a_j - m} \frac{x^m}{m!} \right)
\end{align*}
$$

高々 $N$ 次まで管理すれば良いので，
各 $j$ で，$N$ 次同士の多項式の乗算，
つまり $O(K N \log N)$ 時間で計算できます．


## 10問目: [TTPC2023 P - Bridge Elimination](https://atcoder.jp/contests/ttpc2023/tasks/ttpc2023_p)