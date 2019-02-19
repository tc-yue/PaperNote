- Character-level Convolutional Networks for Text Classification.  NIPS 2015
    - 2.2 Character quantization
        - 其余的UNK
        ```
        abcdefghijklmnopqrstuvwxyz0123456789
        -,;.!?:’’’/\|_@#$%ˆ&*˜‘+-=<>()[]{}
        ```
    - 2.4 Data Augmentation using Thesaurus
        - 文本数据增强不能类似图像做旋转，容易使语义冲突。
        - 利用语义复述进行数据增强，简单的方法对词或短语进行同义词替换。
        - 每个句子选择r个词替换，每个词随机选择替换的同义词。 都是几何分布
    - 3.3 字母表选择
        - 英文字符不区分大小写，因为大小写不改变词的语义

    - 中文语料参考
        - 基于卷积神经网络的微博情感倾向性分析 -- 中文信息学报