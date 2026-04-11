+++
title = "テスト：ブログの全デザインコンポーネント確認"
date = 2026-04-11
draft = true
+++

{{< math >}}

## 基本的な定理枠（全6色テスト）

{{< box color="blue" title="クック・レビンの定理" id="thm-cook-levin" >}}
充足可能性問題 ($\text{SAT}$) は $\mathbf{NP}$ 完全である。
計算量理論における最も重要な定理の一つであり、以下のように記述される。
$$x^* = \argmin_{x \in X} f(x)$$
{{< /box >}}

{{< box color="purple" title="グラフの変形に関する補題" >}}
任意の無向グラフ $G$ において、奇数次の頂点は偶数個存在する（握手補題）。
これは後の完全性証明において、ガジェットを接続する際に重要な役割を果たす。
{{< /box >}}

{{< box color="yellow" title="クラス $\mathbf{NP}$ の定義" >}}
ある決定問題が $\mathbf{NP}$ に属するとは、その問題の「Yes」という答えに対する証拠（Certificate）が与えられたとき、それが正しいかどうかを多項式時間で検証できることをいう。
{{< /box >}}

{{< box color="green" title="3-SATの例" >}}
論理式が乗法標準形 (CNF) であり、かつ各節がちょうど3つのリテラルからなる $\text{SAT}$ を **3-SAT** と呼ぶ。
例: $(x_1 \lor \lnot x_2 \lor x_3) \land (\lnot x_1 \lor x_2 \lor x_4)$
{{< /box >}}

{{< box color="red" title="還元時の注意" >}}
多項式時間チューリング還元と多項式時間マッピング還元（カープ還元）は異なる概念である。通常、$\mathbf{NP}$ 完全性を示す際はカープ還元 $\le_p$ を用いる。
{{< /box >}}

{{< box color="teal" title="独立集合問題 (Independent Set)" >}}
与えられたグラフ $G = (V, E)$ と整数 $k$ について、サイズ $k$ 以上の独立集合が存在するか判定せよ。
{{< /box >}}

---

## 自動採番とプレフィックスの上書きテスト

タイトル（`title`）を省略したり、手動で種類（`prefix`）を変えたりするテストです。

{{< box color="blue" >}}
タイトルを指定しない場合、自動的に「定理 2」のようにカウントアップされて表示されます。
{{< /box >}}

{{< box color="blue" prefix="系" title="定理1から導かれる系" >}}
`prefix="系"` と指定すると、色はそのままで新しいカウンターが動き「系 1」となります。
{{< /box >}}

---

## 折り畳み枠のテスト（fold）

証明などを隠すときに使います。

{{< fold color="gray" title="【証明】定理 1 のスケッチ（クリックで展開）" >}}
先ほどの [定理 1](#thm-cook-levin) の証明の概要です。

1. 計算の履歴（Tableau）を構成する。
2. 各セルが正しく遷移しているかをチェックする論理式 $\phi$ を構築する。
3. 全体のサイズが入力の多項式に収まることを示す。

$$\phi = \phi_{\text{cell}} \land \phi_{\text{start}} \land \phi_{\text{move}} \land \phi_{\text{accept}}$$
$\blacksquare$
{{< /fold >}}

---

## インライン装飾のテスト

本文中のインラインコードやリンクが美しく表示されるかのテストです。

* **インラインコード:** C++の `std::sort(v.begin(), v.end())` や、計算量 $O(N \log N)$ がNotion風の背景色で表示されます。
* **標準Markdownリンク:** これは [Googleへのリンク](https://google.com) のテストです。ホバー時に青いアニメーションが出ます。
* **HTML直書きリンク:** こんにちは <u>下線付きテキスト</u> です。

{{< box color="blue" title="タイトル" id="thm-label" >}}
hogehogeho
{{< /box >}}


[参照](#thm-cook-levin) で参照できる．