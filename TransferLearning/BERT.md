- [code](https://allennlp.org/elmo)
- Introduction
    - 对于特定的任务只需要在预训练的BERT基础上加输出层。
- Model
    - 输入
        - TokenEmbedding: WordPiece分词及向量
        - PositionEmbedding：最长512，可学习位置向量
        - 第一个Token是[CLS]，对于分类任务这个位置的最后一层隐层输出作为文本表示。
        对于其他任务，忽略这个位置的向量。
        - 句子对合并输入，用[SEP]隔开。句子A和句子B分别有词向量
        - 单句输入只使用句子A的向量
    - Masked LM
        - 标准的语言模型只能单向训练。为了训练深度双向的语言模型，采用随机mask输入token的方式，
        然后将该位置对应的最后隐层向量进行softmax分类，预测masked token。
        - 由于微调阶段没有[MASK]，这会导致预训练阶段和微调的错配。
        - 随机选择15%的token
            - 80%的情况替换成[MASK]
            - 10%的情况随机替换成别的词
            - 10%的情况下不替换
        - Transformer encoder不知道哪个词会被预测或者哪个词被随机替换，保持每个输入token的分布式表示。
        另外只有1.5%的词随机替换，所以不会影响模型的语言理解。
        - 虽然MLM只预测15%的token，需要更多的训练才会收敛。但实验表明只会比预测所有词的担心语言模型慢，
        但效果的提升很明显。
    - 预测下一个句子
        - 由于NLI，QA等任务都需要理解句子之间的关系，然而语言模型无法捕捉。训练了一个二分类。
        选择句子A和B作为训练，随机50%替换句子B构成假样本。
    - 预训练
        - 文档级的预料，抽取较长连续序列
        - 从文档中采样两个“句子”（实际上比单个句子长很多）。50%句子B被其他句子随机替换。
        总长度<=512 tokens. 语言模型会在tokenization后mask掉15%的token。
    - 微调
        - 对于分类任务，选择第一个token[CLS]的向量作为表示。然后后面接一个分类层
        和softmax。 分类层的参数和BERT的参数同时进行微调。大部分超参和预训练阶段相同
        只有batch size，学习率和训练轮数 不同
- 实验
    - 基于特征的方法
        - 不是所有的任务都适合Transformer，从预训练的模型获取固定的特征会减少计算时间
        - 在NER上的实验，把最后四层的表示拼接与finetune的效果相差0.3


- https://www.zhihu.com/collection/120087973
https://zhuanlan.zhihu.com/p/65470719
- BERT Post-Training for Review Reading Comprehension and Aspect-based Sentiment Analysis
- Utilizing BERT for Aspect-Based Sentiment Analysis via Constructing Auxiliary Sentence.
- https://mp.weixin.qq.com/s/1Cz6js4kYdvc8g4oKjVPeA