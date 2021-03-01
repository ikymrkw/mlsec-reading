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

Obfuscated gradients

Non-obfuscated gradients --- Adversarial training

Certified robustness
- Raghunathan, et al., Certified Defenses against Adversarial Examples, ICLR 2018 https://arxiv.org/abs/1801.09344
    - MNISTで実験成功
- Cohen, et al., Certified Robustness via Randomized Smoothing, ICML 2019 https://arxiv.org/abs/1902.02918
    - ガウス球内での期待値でロバストな結果を得ようとする。
    - モデル自体もロバストにするためにガウスノイズを加えた訓練データも加えて訓練する。それだけだとadversarialなノイズに弱いかもしれないので、期待値を取る。
    - 期待値は実際には計算できないので、モンテカルロする。
    - MNISTに加え ImageNet でも成功している

### Adaptive attacks for adversarial examples
- C&W 2017: 検知手法を破る
- C&W 2018: ロバスト化手法を破る。Obfuscated gradients
- Tramer 2019: 新しい防御手法に対する適応的攻撃。方法論的な観点に踏み込んでいる。
- Carlini 2020: オンラインのliving document。適応的攻撃の方法論の整理。
- C&W, RobustML - https://www.robust-ml.org/

## Poisoning (+ poisoned models or backdoors)
- Biggio, against SVM
- Munoz-Gonzalez, against Logistic Regression and Deep Neural Network

### Defenses
- RONI (Reject on Negative Impact)
- Outlier Detection

## Model extraction (or Model reconstruction/stealing)
- Tramer: 木とDNN
- Orekondy, et al., Knockoff Nets: [GSch](https://scholar.google.com/scholar?cluster=18254316857573945122&hl=ja&as_sdt=0,5)
    - 攻撃対象モデルの訓練データをまったく知らなくても複製モデルを作れるのが特徴。Active learning風にクエリーを工夫する。
- Juuti, et al., PRADA: [GSch](https://scholar.google.com/scholar?cluster=378782222120699560&hl=ja&as_sdt=0,5)
    - 強力な新攻撃手法を提案。従来より accuracy, transferability ともに向上。
    - 攻撃検知手法 (PRADA) を提案。連続したクエリーの分布を見て複製のための振る舞いを検知する

## Model inversion
- Fredrikson: ワルファリン。出力->入力かも。属性推定。
- Fredrikson: 顔認識。入出力->訓練データ。全属性の推定だが顔画像というフォーマットが固定的。再現が難しい（良い例だけ cherry-pick している？）という話も。
- ...
- GPT-2 inversion: 自然言語、生成モデル。入出力->訓練データ。

## Transferability
- Papernot Black-box ...

## Cross-category attacks

## Practical aspects
- KumarらのIndustry Perspectives論文 2020
- [Adversarial ML Threat Matrix](https://github.com/mitre/advmlthreatmatrix/blob/master/pages/case-studies-page.md) Oct 2020 初版
    - MITREとMicrosoft (Kumarら) が中心となってまとめたらしい
    - 次の2つが主なコンテンツ
        - Matrix。行列という名前だが、実際にはリストのリスト。ATT&CKを参考に、攻撃フェーズごとに攻撃テクニックのリストを提示。こうした形で概念と用語を整理し、攻撃や脆弱性の議論・情報交換をしやすくするのが目的。
        - Case Studies。製品レベルのAIシステムに限定し、実際に可能だった攻撃や脆弱性を、初版では13事例リストアップ。Matrixのどのテクニックが使われたか示しており、Matrixを使った議論・情報交換の実例となっている。
