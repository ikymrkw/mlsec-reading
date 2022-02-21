# 機械学習セキュリティ 文献リスト

記事「機械学習セキュリティ研究のフロンティア」 https://doi.org/10.1587/essfr.15.1_37 の参考文献リスト（増強版）です。

## General
- Papernot+ 2018, SoK: Security and Privacy in Machine Learning, EuroS&P 2018. [IEEE](https://ieeexplore.ieee.org/document/8406613)
- Biggio and Roli, Wild Patterns: Ten years after the rise of adversarial machine learning, Pattern Recognition, Vol. 84, 2018. [DOI](https://doi.org/10.1016/j.patcog.2018.07.023)
- [Adversarial Machine Learning Reading List](https://nicholas.carlini.com/writing/2018/adversarial-machine-learning-reading-list.html) by N. Carlini

## Adversarial examples (or Evasion)
- 日本語の入門的記事: [Elix Tech Blog](https://elix-tech.github.io/ja/2017/10/15/adversarial.html)
- あるポスドクの [Reading List](https://github.com/chawins/Adversarial-Examples-Reading-List)
- Ilyas 2019, Adversarial Examples Are Not Bugs; They Are Features, NeurIPS 2019. [arXiv](https://arxiv.org/abs/1905.02175) -- 分類に有効 (useful) だが正しい（人間の）判断には寄与しない (non-robust) 特徴量の存在が原因だ、と主張する論文

### Attack strategies
基本は勾配ベースの最適化。探索範囲は Lpノルムが主流（画像は L2 と L∞ が多い）。
- Szegedy+ 2013, Intriguing properties of neural networks. [arXiv](https://arxiv.org/abs/1312.6199)
    - おそらく初めて "adversarial example" という名前を使った論文。
    - 距離制約はせずに、できるだけ近くでできるだけ損失関数が大きい（または目的のクラスに対して小さい）摂動を、L-BFGSで探索した、と書いてある。L-BFGSは2階微分を使った山登り法の一種らしい。
- FGSM: 損失関数の勾配の符号のε倍を1回加える手法。単純で計算が早いが、なかなか実用的。Goodfellow+, Explaining and Harnessing Adversarial Examples, ICLR 2015: [arXiv](http://arxiv.org/abs/1412.6572)
- PGD: 摂動範囲内に戻しながら FGSMを複数ステップ繰り返す。この攻撃方法が adversarial training にとって（理想的には）最適であることを示した。Madry+, Towards Deep Learning Models Resistant to Adversarial Attacks, ICLR 2018: [arXiv](https://arxiv.org/abs/1706.06083)

特殊な探索範囲
- JSMA: Papernot+ 2016: saliency map（つまりJacobian = 勾配(偏微分)の配列）のうち絶対値の大きなものから k 個選んで摂動させる。つまり L0ノルム <= k で探索している。IEEE Euro S&P 2016, [arXiv](https://arxiv.org/pdf/1511.07528.pdf)

### Black-box attacks
基本的には white-box attack で評価すべきと言われているが（下記の adaptive attacks を参照）、 black-box attack の検討もされている。

- Transferability (転移性)
    - Papernot+ 2017, Practical Black-Box Attacks against Machine Learning, ASIA CCS 2017 -- 同じタスクであれば異なるモデルでも同じ adversarial example が通用しがち。つまり、訓練データが入手可能なら自分で訓練したり、 model extraction (後述) で攻撃対象モデルを複製したりすれば、手元の代理モデル (surrogate model, substitute model) に対して white-box attack をすればよい。
- Hard-label attacks
    - 数値（確信度）がなくてもクエリーにより近似推定する。(TBA)

### Adversarial examples against NLP
- 記事 [私のブックマーク:言語処理分野におけるAdversarial Example](https://jsai.ixsq.nii.ac.jp/ej/index.php?action=pages_view_main&active_action=repository_action_common_download&item_id=10412&item_no=1&attribute_id=22&file_no=1&page_id=13&block_id=23)
- Text: Ebrahimi+ 2018, HotFlip: White-Box Adversarial Examples for Text Classification, ACL 2018. [ACL](https://www.aclweb.org/anthology/P18-2006/)

### Adversarial examples against Preprocessing
- Image scaling [USENIX Sec 2020](https://www.usenix.org/conference/usenixsecurity20/presentation/quiring)
- 自然言語の前処理（埋め込み）に対する攻撃もある (TBA)

### Robust attacks
- Sharif+ 2016., Accessorize to a Crime, [ACM](https://dl.acm.org/doi/10.1145/2976749.2978392) CCS 2016 -- 眼鏡型
- Eykholt+ 2018, Robust Physical-World Attacks, CVPR 2018, [arXiv](https://arxiv.org/abs/1707.08945) -- 交通標識
- Athalye+ 2018, Synthesizing Robust Adversarial Examples, ICML 2018, [arXiv](https://arxiv.org/abs/1707.07397) -- 3Dプリンタで作ったカメ [YouTube](https://www.youtube.com/watch?v=YXy6oX1iNoA) [labsix](https://www.labsix.org/physical-objects-that-fool-neural-nets/)
- Brown+ 2017, Adversarial Patch, NIPS 2017. [arXiv](https://arxiv.org/pdf/1712.09665.pdf)

### Other data types
- Semantic Segmentation: Xiao+ 2018, Characterizing Adversarial Examples Based on Spatial Consistency Information for Semantic Segmentation, ECCV 2018. [arXiv](https://arxiv.org/abs/1810.05162) - 画像からの物体領域の切り出しに対する adversarial examples とその対策・性質。
- Audio: Carlini+ 2018, Audio Adversarial Examples: Targeted Attacks on Speech-to-Text, DLS 2018. [from author](https://nicholas.carlini.com/papers/2018_dls_audioadvex.pdf)
- Tabular: Cartella+ 2021, Adversarial Attacks for Tabular Data: Application to Fraud Detection and Imbalanced Data, SafeAI 2021. [arXiv](https://arxiv.org/abs/2101.08030) [SafeAI](https://safeai.webs.upv.es/wp-content/uploads/2021/02/01_SafeAI2021-Cartella-et-al-Adversarial-Attacks-for-Tabular-Data.pdf)


### Defense strategies
検知とロバスト化

Obfuscated gradients

Non-obfuscated gradients --- Adversarial training

Certified robustness
- Maximum safe radius (最大安全距離) の形式検証
    - Katz+ 2017 (Reluplex), Tjeng 2019, Weng 2018 など
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
- Tramer+ 2019, On Adaptive Attacks to Adversarial Example Defenses: [Web](https://nicholas.carlini.com/papers/2020_neurips_adaptiveattacks.pdf). 新しい防御手法に対する適応的攻撃。方法論的な観点に踏み込んでいる。
- Carlini+ 2020, On Evaluating Adversarial Robustness: [GitHub](https://github.com/evaluating-adversarial-robustness/adv-eval-paper). オンラインのliving document。適応的攻撃の方法論の整理。
- C&W, RobustML - https://www.robust-ml.org/

## Poisoning (+ poisoned models or backdoors)
- Microsoft Tay
    - O. Schwartz, "In 2016, Microsoft's Racist Chatbot Revealed the Dangers of Online Conversation", IEEE Spectrum, Nov. 2019. [link](https://spectrum.ieee.org/tech-talk/artificial-intelligence/machine-learning/in-2016-microsofts-racist-chatbot-revealed-the-dangers-of-online-conversation)
- Biggio+ 2012, Poisoning Attacks Against SVM, ICML 2012, [ACM](\url{https://dl.acm.org/doi/10.5555/3042573.3042761}
)
- Munoz-Gonzalez+ 2017, Towards Poisoning of Deep Learning Algorithms with Back-gradient Optimization, AISec 2017, [ACM](https://dl.acm.org/doi/10.1145/3128572.3140451) -- against Logistic Regression and Deep Neural Network
- Munoz-Gonzalezの記事: https://internationaldataspaces.org/how-to-poison-data-based-on-ai/

### Defenses
- RONI (Reject on Negative Impact): [ACM](https://dl.acm.org/doi/10.5555/1387709.1387716)
- Bagging: Biggio+ 2011, Bagging Classifiers for Fighting Poisoning Attacks in Adversarial Classification Tasks, MCS 2011
- Outlier Detection
- Data Sanitization: Steinhardt+ 2017, Certified Defenses for Data Poisoning Attacks, NIPS 2017
- 清水, 森川, 機械学習モデルに対するポイズニング攻撃意図の検知について, 暗号と情報セキュリティシンポジウム SCIS 2020.


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


## Training data inference (model inversion, attribute inference, membership inference, ...)
- Fredrikson: ワルファリン。出力->入力かも。属性推定。
- Fredrikson+ 2015, Model inversion Attacks that Exploit Confidence Information ..., ACM CCS 2015
    - 顔認識。入出力->訓練データ。全属性の推定だが顔画像というフォーマットが固定的。再現が難しい（良い例だけ cherry-pick している）？
- Basu: S. Basu, R. Izmailov , C. Mesterharm, “Membership Model Inversion attacks for Deep Networks,” NeurIPS 2019, Workshop on Privacy in Machine Learning, 2019.
- Yang: Z. Yang, J. Zhang, E.-C. Chang and Z. Liang, "Neural Network Inversion in Adversarial Setting via Background Knowledge Alignment," the 2019 ACM SIGSAC Conference on Computer and Communications Security, Pages 225-240, 2019 [IEEE](https://dl.acm.org/doi/abs/10.1145/3319535.3354261)
- GPT-2 inversion: 自然言語、生成モデル。入出力->訓練データ。[arXiv](https://arxiv.org/abs/2012.07805)
- Song+ 2019: Shokriら。Advex対策するとMembership Inferenceに弱くなる。CCS 2019, [arXiv](https://arxiv.org/abs/1905.10291)
- 樋口, 森川, 清水, 分類モデルに対するVAEを用いた教師データ推定攻撃, 暗号と情報セキュリティシンポジウム SCIS 2020.

- [TODO] membership inference attacks
    - Shokri, et al.
    - Salem, et al., ML-Leaks, 2018. [arXiv](https://arxiv.org/pdf/1806.01246.pdf)

### Model inversion against NLP models
- Pan: "Privacy Risks of General Purpose Language Models" [IEEE S&P 2020](https://www.computer.org/csdl/proceedings-article/sp/2020/349700b471/1j2LgooZ4fS), [Semantic Scholar](https://www.semanticscholar.org/paper/Privacy-Risks-of-General-Purpose-Language-Models-Pan-Zhang/b3c73de96640ee858f83c3f0eda2a3d15d59b847)
- Song: "Information Leakage in Embedding Models" [ACM CCS 2020](https://dl.acm.org/doi/10.1145/3372297.3417270)


## Cross-category attacks and trade-offs
- Hidano+ 2017: Model inversion attacks for prediction systems: Without knowledge of non-sensitive attributes, IEEE PST 2017. [IEEE](https://ieeexplore.ieee.org/document/8476925)
- Song+ 2019: Privacy Risks of Securing Machine Learning Models against Adversarial Examples, ACM CCS 2019. [arXiv](https://arxiv.org/abs/1905.10291)
- Shokri+ 2021: On the Privacy Risks of Model Explanations, arXiv preprint, 2021. [arXiv](https://arxiv.org/abs/1907.00164)
- Ghorbani+ 2019: Interpretation of Neural Networks Is Fragile, AAAI 2019. [AAAI](https://ojs.aaai.org/index.php/AAAI/article/view/4252)

## Practical aspects
- Kumar+ 2020, Adversarial Machine Learning: Industry Perspectives [arXiv](https://arxiv.org/pdf/2002.05646.pdf)
    - 企業へのインタビューと SDL的な取り組みへの提言
- [Adversarial ML Threat Matrix](https://github.com/mitre/advmlthreatmatrix/blob/master/pages/case-studies-page.md) Oct 2020 初版 -> 2021年 ATLAS に進化
    - MITREとMicrosoft (Kumarら) が中心となってまとめたらしい
    - 次の2つが主なコンテンツ
        - Matrix。行列という名前だが、実際にはリストのリスト。ATT&CKを参考に、攻撃フェーズごとに攻撃テクニックのリストを提示。こうした形で概念と用語を整理し、攻撃や脆弱性の議論・情報交換をしやすくするのが目的。
        - Case Studies。製品レベルのAIシステムに限定し、実際に可能だった攻撃や脆弱性を、初版では13事例リストアップ。Matrixのどのテクニックが使われたか示しており、Matrixを使った議論・情報交換の実例となっている。
- QA4AIコンソーシアム, AIプロダクト品質保証ガイドライン 2020.08版. [link](http://www.qa4ai.jp/QA4AI.Guideline.202008.pdf)
- 産業技術総合研究所, 機械学習品質マネジメントガイドライン 第1版, 2020. [link](https://www.cpsec.aist.go.jp/achievements/aiqm/) -- 2021年に第2版
- 森川, 清水, 樋口, 前田, 矢嶋, 敵対的機械学習のための脅威分析手法の提案, コンピュータセキュリティシンポジウム CSS 2019.
- 矢嶋, 清水, 森川, 大久保, 機械学習システムに潜むAIセキュリティ脆弱性の分析手法に関する一考察, 暗号と情報セキュリティシンポジウム SCIS 2021.

## Governments and International Activities
- [Security] NISTIR 8269 Draft (2019)
    - 2021年2月現在、まだ[ドラフト](https://csrc.nist.gov/publications/detail/nistir/8269/draft)
    - Taxonomy and Terminology というタイトルのとおり、概念と用語の整理を行っている
- [General] EU: Ethics Guidelines for Trustworthy AI (2019/11) [link](https://op.europa.eu/en/publication-detail/-/publication/d3988569-0434-11ea-8c1f-01aa75ed71a1)
- [General] EU: Whitepaper "On Artificial Intelligence - A European approach to excellence and trust" (2020/02/19) [link](https://ec.europa.eu/info/sites/info/files/commission-white-paper-artificial-intelligence-feb2020_en.pdf)
- [General] EU: AI Act (proposal) (2021/04) [link](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:52021PC0206&from=EN)
    - 主な内容
        - イノベーションと社会の安全・安定の両立を図る
        - リスクを理解し、高リスクなAIシステムを区別し、コントロールする
        - 頑健性（セキュリティ含む）やプライバシーを実現する技術は重要だ
- [Security] ENISA: Report "Artificial Intelligence Cybersecurity Challenges" (2020/12/15) [link](https://www.enisa.europa.eu/publications/artificial-intelligence-cybersecurity-challenges) [press release](https://www.enisa.europa.eu/news/enisa-news/enisa-ai-threat-landscape-report-unveils-major-cybersecurity-challenges)
    - 通称 AI Threat Landscape Report
    - ちなみに ENISA は EU のサイバーセキュリティの agency. この報告書は ENISA の ad-hoc AI WG が作成.
- [Security] ENISA: Report "Securing Machine Learning Algorithms" (2021/12/14) [link](https://www.enisa.europa.eu/publications/securing-machine-learning-algorithms)
- [General] 英ICO: Guidance on AI and Data Protection (2020/07) [link](https://ico.org.uk/for-organisations/guide-to-data-protection/key-dp-themes/guidance-on-artificial-intelligence-anddata-protection/)
    - データ保護観点からのガイダンス。リスク評価ツール (Excel) が付属。


