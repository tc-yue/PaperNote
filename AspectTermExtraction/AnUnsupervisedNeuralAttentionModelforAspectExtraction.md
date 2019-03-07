-  An Unsupervised Neural Attention Model for Aspect Extraction - ACL 2017
- Introduction
    - 经常共现的词会在向量空间相近的位置。利用注意力机制去选择句子中与Aspect相关的词去构建Aspect Embedding。
    训练过程类似于Auto Encoder
- Model
    - 模型的目的是学习Aspect Embedding，每个aspect通过查找向量空间内最近的词（代表性的）
    - 注意力机制的句子表示
        - 首先对句子的词向量平均，得到句子的全局表示。利用该全局表示与每个词计算注意力权重，可以衡量句子中每个词与aspect的相关性。
        - 进而对词向量加权得到句子表示。
        -  First, we filter the word through the transformation M which is able to capture the relevance of the
        word to the K aspects. Then we capture the relevance of the filtered word to the sentence
        by taking the inner product of the filtered word to the global context .
    - Sentence Reconstruction
        - 首先对句子表示进行降维和线性变换，得到句子属于每个Aspect的概率
        - 将该概率与Aspect embedding 相乘得到重构的向量表示
    - Objective
        - Max-Margin
        - negative sample，让构建的句子表示与target 相似不同于其他的negative samples、
    - 正则项
        - Aspect Embedding Matrix T 可能会冗余，加一个正则，让任意的aspect vector尽可能不相似。

- Experiment
    - 利用余弦相似度来找向量空间内能代表aspect的词
    - 根据这些top-ranked代表性的词映射为标注的aspect