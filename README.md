# Machine Learning Security Reading List

## General
- Papernot SoK
- Biggio Wild Patterns

## Adversarial examples (or Evasion)

### Attack strategies
基本は勾配ベースの最適化。探索範囲は Lpノルムが主流（画像は L2 と L∞ が多い）。
- FGSM: 勾配の符号のε倍を1回加える手法。単純で計算が早いが、なかなか実用的。
- iFGSM: FGSMをiterativeに、小さなεで数回行う。
- PGD: iFGSMに似ているが符号のε倍ではなく勾配そのもののε倍(?)を iterative に加える。もっとも一般的な手法

実際には値を変えられる範囲が決まっているので、範囲を超えるような勾配成分は無視する（性能的には望ましい）か、範囲を超えて摂動させてから範囲内にクリップする（性能は劣るが実装が容易）必要がある。

特殊な探索範囲
- JSMA: saliency map（つまりJacobian = 勾配(偏微分)の配列）のうち絶対値の大きなものから k 個選んで摂動させる。つまり L0ノルム <= k で探索している。

### Hard-label attacks
数値（確信度）がなくてもクエリーにより近似推定する。

### Defense strategies
検知とロバスト化

### Adaptive attacks for adversarial examples
- C&W 2017: 検知手法を破る
- C&W 2018: ロバスト化手法を破る。Obfuscated gradients
- Tramer 2019: 新しい防御手法に対する適応的攻撃。方法論的な観点に踏み込んでいる。
- Carlini 2020: オンラインのliving document。適応的攻撃の方法論の整理。

## Poisoning (+ poisoned models or backdoors)
- Biggio, against SVM
- Munoz-Gonzalez, against Logistic Regression and Deep Neural Network

### Defenses
- RONI (Reject on Negative Impact)
- Outlier Detection

## Model extraction (or Model reconstruction/stealing)
- Tramer: 木とDNN
- Orekondy, et al., Knockoff Nets: [GSch](https://scholar.google.com/scholar?cluster=18254316857573945122&hl=ja&as_sdt=0,5)
- PRADA: 強力な新攻撃手法と対抗する防御手法 (PRADA)

## Model inversion
- Fredrikson: ワルファリン。出力->入力かも。属性推定。
- Fredrikson: 顔認識。入出力->訓練データ。全属性の推定だが顔画像というフォーマットが固定的。再現が難しい（良い例だけ cherry-pick している？）という話も。
- ...
- GPT-2 inversion: 自然言語、生成モデル。入出力->訓練データ。

## Transferability
- Papernot Black-box ...

## Cross-category attacks
