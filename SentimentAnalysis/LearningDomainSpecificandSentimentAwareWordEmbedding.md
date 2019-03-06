- ACL 2018 LongPaper
- Introduction
    - 大部分的基于深度学习的情感分析方法都使用通用语料的词向量。
    - Tang 尝试把情感信息传入到词向量中去。Sentiment Aware Embedding 可以区分good和bad。
    实际上情感信息是容易获得的（比如用户的打分和评论）、
    - 然而有些情感词是domain-sensitive。例如lightweight在电子领域就是积极的，在电影评论
    领域就是消极的。
- Model
    -
- Experiment
    - 用户画像，情感分析，文本匹配
    - 实验分析1.0的惩罚项系数，效果较好，可视化也展示了不同hop的焦点不同
    - hops 数10-30 效果较好
-   ![pic](pics/ssase/1.png)
