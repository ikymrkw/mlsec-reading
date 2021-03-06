# 機械学習セキュリティ 文献リスト

記事「機械学習セキュリティ研究のフロンティア」 https://doi.org/10.1587/essfr.15.1_37 の参考文献リスト（増強版）です。

## General
- Papernot+ 2018, SoK: Security and Privacy in Machine Learning``, EuroS&P 2018. [IEEE](https://ieeexplore.ieee.org/document/8406613)
- Biggio and Roli, Wild Patterns: Ten years after the rise of adversarial machine learning, Pattern Recognition, Vol. 84, 2018. [DOI](https://doi.org/10.1016/j.patcog.2018.07.023)
- [Adversarial Machine Learning Reading List](https://nicholas.carlini.com/writing/2018/adversarial-machine-learning-reading-list.html) by N. Carlini

## Adversarial examples (or Evasion)
- 日本語の入門的記事: [Elix Tech Blog](https://elix-tech.github.io/ja/2017/10/15/adversarial.html)
- あるポスドクの [Reading List](https://github.com/chawins/Adversarial-Examples-Reading-List)
- Ilyas 2019, Adversarial Examples Are Not Bugs; They Are Features

### Attack strategies
基本は勾配ベースの最適化。探索範囲は Lpノルムが主流（画像は L2 と L∞ が多い）。
- Szegedy+ 2013, [arXiv](https://arxiv.org/abs/1312.6199) - おそらく初めて "adversarial example" という名前を使った論文。
    - 距離制約はせずに、できるだけ近くでできるだけ損失関数が大きい（または目的のクラスに対して小さい）摂動を、L-BFGSで探索した、と書いてある。L-BFGSは2階微分を使った山登り法の一種らしい。
- FGSM: 損失関数の勾配の符号のε倍を1回加える手法。単純で計算が早いが、なかなか実用的。Goodfellow+ ICLR 2015: [arXiv](http://arxiv.org/abs/1412.6572)
- PGD: FGSMを複数ステップ繰り返す。Madry+ ICLR 2018: [arXiv](https://arxiv.org/abs/1706.06083)

実際には値を変えられる範囲が決まっているので、範囲を超えるような勾配成分は無視する（性能的には望ましい）か、範囲を超えて摂動させてから範囲内にクリップする（性能は劣るが実装が容易）必要がある。

特殊な探索範囲
- JSMA: Papernot+ 2016: saliency map（つまりJacobian = 勾配(偏微分)の配列）のうち絶対値の大きなものから k 個選んで摂動させる。つまり L0ノルム <= k で探索している。IEEE Euro S&P 2016, [arXiv](https://arxiv.org/pdf/1511.07528.pdf)

### Hard-label attacks
数値（確信度）がなくてもクエリーにより近似推定する。
- (TBA)

### Adversarial examples against NLP
- 記事 [私のブックマーク:言語処理分野におけるAdversarial Example](https://jsai.ixsq.nii.ac.jp/ej/index.php?action=pages_view_main&active_action=repository_action_common_download&item_id=10412&item_no=1&attribute_id=22&file_no=1&page_id=13&block_id=23)

### Adversarial examples against Preprocessing
- Image scaling [USENIX Sec 2020](https://www.usenix.org/conference/usenixsecurity20/presentation/quiring)
- 自然言語の前処理（埋め込み）に対する攻撃もある (TBA)

### Robust attacks
- Sharif+ 2016., Accessorize to a Crime, [ACM](https://dl.acm.org/doi/10.1145/2976749.2978392) CCS 2016 -- 眼鏡型
- Eykholt+ 2018, Robust Physical-World Attacks, CVPR 2018, [arXiv](https://arxiv.org/abs/1707.08945) -- 交通標識
- Athalye+ 2018, Synthesizing Robust Adversarial Examples, ICML 2018, [arXiv](https://arxiv.org/abs/1707.07397) -- 3Dプリンタで作ったカメ [YouTube](https://www.youtube.com/watch?v=YXy6oX1iNoA) [labsix](https://www.labsix.org/physical-objects-that-fool-neural-nets/)
- Adversarial patch: [arXiv](https://arxiv.org/pdf/1712.09665.pdf)

### Audio attacks
- (TBA)

### Defense strategies
検知とロバスト化

Obfuscated gradients

Non-obfuscated gradients --- Adversarial training

Certified robustness
- Maximum safe radius (最大安全距離) の形式検証
    - Katz 2017 (Reluplex), Tjeng 2019, Weng 2018 など
- Raghunathan, et al., Certified Defenses against Adversarial Examples, ICLR 2018 [arXiv](https://arxiv.org/abs/1801.09344)
    - MNISTで実験成功（※何をもって成功としている？）
- Cohen, et al., Certified Robustness via Randomized Smoothing, ICML 2019 [arXiv](https://arxiv.org/abs/1902.02918)
    - ガウス球内での期待値でロバストな結果を得ようとする。
    - モデル自体もロバストにするためにガウスノイズを加えた訓練データも加えて訓練する。それだけだとadversarialなノイズに弱いかもしれないので、期待値を取る。
    - 期待値は実際には計算できないので、モンテカルロする。
    - MNISTに加え ImageNet でも成功している（※何をもって成功としている？）

### Adaptive attacks for adversarial examples
- C&W 2017: 検知手法を破る。Adversarial Examples Are Not Easily Detected. [arXiv](https://arxiv.org/abs/1705.07263)
- ACW 2018: ロバスト化手法を破る。Obfuscated gradients Give a False Sense of Security. [arXiv](https://arxiv.org/abs/1802.00420)
- Tramer+ 2019: 新しい防御手法に対する適応的攻撃。方法論的な観点に踏み込んでいる。
- Carlini+ 2020: オンラインのliving document。適応的攻撃の方法論の整理。
- C&W, RobustML - https://www.robust-ml.org/

## Poisoning (+ poisoned models or backdoors)
- Biggio+ 2012, Poisoning Attacks Against SVM, ICML 2012, [ACM](\url{https://dl.acm.org/doi/10.5555/3042573.3042761}
)
- Munoz-Gonzalez+ 2017, Towards Poisoning of Deep Learning Algorithms with Back-gradient Optimization, AISec 2017, [ACM](https://dl.acm.org/doi/10.1145/3128572.3140451) -- against Logistic Regression and Deep Neural Network
- Munoz-Gonzalezの記事: https://internationaldataspaces.org/how-to-poison-data-based-on-ai/

### Defenses
- RONI (Reject on Negative Impact): [ACM](https://dl.acm.org/doi/10.5555/1387709.1387716)
- Bagging: Biggio+ 2011, Bagging Classifiers for Fighting Poisoning Attacks in Adversarial Classification Tasks, MCS 2011
- Outlier Detection
- Data Sanitization: Steinhardt+ 2017, Certified Defenses for Data Poisoning Attacks, NIPS 2017

## Model extraction (or Model reconstruction/stealing)
- Tramer+ 2016, Stealing Machine Learning Models via Prediction APIs, USENIX Security 2016 -- 木とDNN
- Orekondy+, Knockoff Nets: [GSch](https://scholar.google.com/scholar?cluster=18254316857573945122&hl=ja&as_sdt=0,5)
    - 攻撃対象モデルの訓練データをまったく知らなくても複製モデルを作れるのが特徴。Active learning風にクエリーを工夫する。
    - 中規模サイズの画像の認識に対し、(Budget Bがクエリー回数と読むと）5万～10万くらいで accuracy が saturate している。
- Juuti+, PRADA: [GSch](https://scholar.google.com/scholar?cluster=378782222120699560&hl=ja&as_sdt=0,5)
    - ごく一部の訓練データサンプルから敵対的サンプルを使ってデータ増強。従来より accuracy, transferability ともに向上。MNISTとGTSRB (32x32) で実験。10万強のクエリーを行う。
    - 攻撃検知手法 (PRADA) を提案。連続したクエリーの分布を見て複製のための振る舞いを検知する
### Model extraction against NLP models
- [Stealing BERT](http://www.cleverhans.io/2020/04/06/stealing-bert.html) by Krishna and Papernot

## Model inversion
- Fredrikson: ワルファリン。出力->入力かも。属性推定。
- Fredrikson+ 2015, Model inversion Attacks that Exploit Confidence Information ..., ACM CCS 2015
    - 顔認識。入出力->訓練データ。全属性の推定だが顔画像というフォーマットが固定的。再現が難しい（良い例だけ cherry-pick している）？
- Basu: S. Basu, R. Izmailov , C. Mesterharm, “Membership Model Inversion attacks for Deep Networks,” NeurIPS 2019, Workshop on Privacy in Machine Learning, 2019.
- Yang: Z. Yang, J. Zhang, E.-C. Chang and Z. Liang, "Neural Network Inversion in Adversarial Setting via Background Knowledge Alignment," the 2019 ACM SIGSAC Conference on Computer and Communications Security, Pages 225-240, 2019 [IEEE](https://dl.acm.org/doi/abs/10.1145/3319535.3354261)
- GPT-2 inversion: 自然言語、生成モデル。入出力->訓練データ。[arXiv](https://arxiv.org/abs/2012.07805)
- Song+ 2019: Shokriら。Advex対策するとMembership Inferenceに弱くなる。CCS 2019, [arXiv](https://arxiv.org/abs/1905.10291)

### Model inversion against NLP models
- Pan: "Privacy Risks of General Purpose Language Models" [IEEE S&P 2020](https://www.computer.org/csdl/proceedings-article/sp/2020/349700b471/1j2LgooZ4fS), [Semantic Scholar](https://www.semanticscholar.org/paper/Privacy-Risks-of-General-Purpose-Language-Models-Pan-Zhang/b3c73de96640ee858f83c3f0eda2a3d15d59b847)
- Song: "Information Leakage in Embedding Models" [ACM CCS 2020](https://dl.acm.org/doi/10.1145/3372297.3417270)

## Transferability
- Papernot Black-box ...

## Cross-category attacks

## Practical aspects
- Kumar+ 2020, Adversarial Machine Learning: Industry Perspectives [arXiv](https://arxiv.org/pdf/2002.05646.pdf)
    - 企業へのインタビューと SDL的な取り組みへの提言
- [Adversarial ML Threat Matrix](https://github.com/mitre/advmlthreatmatrix/blob/master/pages/case-studies-page.md) Oct 2020 初版
    - MITREとMicrosoft (Kumarら) が中心となってまとめたらしい
    - 次の2つが主なコンテンツ
        - Matrix。行列という名前だが、実際にはリストのリスト。ATT&CKを参考に、攻撃フェーズごとに攻撃テクニックのリストを提示。こうした形で概念と用語を整理し、攻撃や脆弱性の議論・情報交換をしやすくするのが目的。
        - Case Studies。製品レベルのAIシステムに限定し、実際に可能だった攻撃や脆弱性を、初版では13事例リストアップ。Matrixのどのテクニックが使われたか示しており、Matrixを使った議論・情報交換の実例となっている。
- NISTIR 8269 (2019)
    - 2021年2月現在、まだ[ドラフト](https://csrc.nist.gov/publications/detail/nistir/8269/draft)
    - Taxonomy and Terminology というタイトルのとおり、概念と用語の整理を行っている
