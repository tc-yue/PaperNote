## MIX: Multi-Channel Information Crossing for Text Matching
- KDD 2018
- model

    - __local matching__: 短语级匹配
        - MatchingPyramid等基于词向量的交互匹配模型会导致信息缺失。例如无法区分（all in& in all）
        （hard work & work hard）这种短语的相似度。
        - 本文利用多粒度的卷积核（1,2,3）去抽取短语的信息，
    - __global matching__ : 句子级匹配
        - 实际上，所有的短语对于两个句子的匹配应该有不同的权重。也就是说核心短语的匹配
        要比无关短语的匹配更重要。
        - 利用一个逐元素的注意力层去衡量交互之间的重要性。
        - 词的idf权重,例如两个实体短语，比介词短语重要。可训练。
        - 词性，例如两个人名，比两个形容词重要
        - 词的位置，QA中，前面的部分要比后面的部分更重要

