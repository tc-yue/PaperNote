- http://www.mooc.ai/course/574/learn?lessonid=2904#lesson/2904
- http://ruder.io/multi-task/index.html#introduction
- https://xpqiu.github.io/
- 多任务学习是一种inductive transfer (归纳迁移)。
    - 通过引入inductive bias 来提高模型性能。
    - 例如L1正则是一种归纳偏置，会导致稀疏参数。多任务学习通过辅助的任务，导致模型泛化性更强。
    - 多任务学习可以看作是一种归纳迁移学习（Inductive Transfer Learning），
    即通过利用包含在相关任务中的信息作为归纳偏置（Inductive Bias）来提高泛化能力
- 联合学习joint loss，同一个数据集有多种标注，例如同一个句子既有词性标注，又有实体识别。
- 如果两个任务是分别的数据集，就是multi-task。训练方式，词性标注和实体识别是不同的标注数据集
    - joint training
        - 随机选择一个任务
        - 从该任务中随机选择一个训练样本
        - 更新该任务的参数
    - FineTune
        - 利用Finetune策略去优化每个任务的性能
- 优点
    - 隐式的数据增强
    - 更好的表示学习
        - 一个表示去满足多多个任务
    - 正则化
        - 防止过拟合
-  分类
    - Multi-Domain， Text Classification
    - Multi-level， POS and NER

- 模型
    - Hard Share
        - 每个任务的表示一样
    - soft share
        - 每个任务的表示不一样
    - Auxiliary tasks，专注主任务的性能
### 硬共享
- Recurrent Neural Network for Text Classification with Multi-Task Learning - IJCAI2016
    - share-layer：多个任务用同一个BiLSTM 建模。
    - specific layer：将共享层的输出，和词向量拼接作为任务特定rnn的输入。
- Same Representation, Different Attentions:Shareable Sentence Representation Learning from Multiple Tasks - IJCAI 2018
    - share-layer:同样的RNN进行表示。对于某个特定任务，句子中不是所有的信息都是有用的。
    - specific layer：每个任务有个query，对share-layer 的表示进行attention weighted sum。
        - static attentive
            - query vector 随机初始化
        - Dynamic attentive
            - 引入辅助任务，预测句子的领域。因此共享的句子表示中就会包含领域信息。利用一个static
            domain query 得到句子的domain weighted representation，利用这个表示作为query vector 去生成分类的注意力。
        - Transferability
            - 将source 任务学到的共享表示，迁移到特定任务
        - 好的句子表示需要包括语言学信息，引入序列标注作为辅助任务
- Multi-Task Deep Neural Networks (MT-DNN) for Natural Language Understanding

- 共享私有
    - 所有任务共享一个RNN，每个任务有个私有的RNN，
    - 将私有的RNN和共享的RNN拼接作为表示

### 软共享
- cross-stitch networks for multi-task learning - CVPR 2016
- Gated Multi-Task Network for Text Classification - NAACL 2018
    - 任务相对分离，可以从别的任务获取特征表示。
    - 对于任务j，利用gated 选择其他任务中可以利用的信息，加入到j的表示中
- Latent Multi-task Architecture Learning - AAAI 2019
- Learning What to Share：Leaky Multi-Task Network for Text Classification - COlING 2018

### 辅助任务
- Learning Sentence Embeddings with Auxiliary Tasks for Cross-Domain Sentiment Classification -EMNLP 2016
    - 辅助任务，将domain-independent 的情感词扣掉，预测该句子是否包含情感词
- Extractive Summarization Using Multi-task Learning with Document Classification -EMNLP 2017
- Multi-Task Attention-based Neural Networks for Implicit Discourse Relationship Representation and Identification -EMNLP 2018
- Exploiting Document Knowledge for Aspect-level Sentiment Classification   - ACL 2018
    - 主任务aspect-Sentiment
    - 辅任务 文本分类，共享双向LSTM表示学习。
- Open-Domain Name Error Detection using a Multi-Task RNN - EMNLP 2015
- Multi-task Learning of Pairwise Sequence Classification Tasks
Over Disparate Label Spaces - NAACL2018

### Trick
- 不同任务loss的权重
    - 主辅任务，可以选择一个超参数lambda，根据验证集调节
- 修改任务sample的方式
    - 训练时对于主任务sample的更频繁
    - sample 概率p1 = 2p2


