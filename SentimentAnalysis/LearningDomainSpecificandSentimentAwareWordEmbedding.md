- ACL 2014
-  原始的词向量模型无法捕获情感信息，
- Model
    - C&W model
        - 随机替换ngram中的一个词，构建corrupted ngram，训练目标就是让原始的ngram的得分高于
        corrupted ngram得分。
        - hinge loss， loss = max(0,1-f(t)+f(t^r)), f()是一维标量
    - sentiment-specific word embedding
        - __SSWEh__:使用一个句子的ngram作为输入，预测这个ngram的情感倾向。交叉熵损失函数
        - __SSWEr__：上面的softmax，交叉熵对于0,1的二分类限制过强。移除softmax，使用hinge loss
        的变体，为了拉大0,1得分的差距
        - __SSWEu__：同时加入语义信息和情感信息。将corrupted ngram的loss 和 SSWEr 的loss加起来。
    - Twitter数据利用情感符号来自动标注