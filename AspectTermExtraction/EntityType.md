# Entity-Type
- Fine-Grained Entity Type Classification based on local context,
    - two different mentions of same entity can have different labels
- 数据集
    - 目前的数据集都是远程监督构建，无论entity mention 的上下文是什么，都根据entity进行同样的标注。导致了noisy。
- 和兴
    - context 和 entity 表示
    - 损失函数，层次标签
    - 去噪

------------------------------------------
#### Neural Architectures for Fine-grained Entity Type Classification - EACL 2017
- 当成多标签分类，取logit最大的标签，判断logit>0.5
- 特征
- 模型：
    - mention 表示：词向量平均
    - attentive encoder：双向lstm对左右文本编码，然后计算注意力，进行weighted sum
    - 将人工特征，mention，attentive三者表示拼接。
- 层次化label encoding
    - 标签都是有层次的：person->artist->musician
    - 学习标签矩阵的层次关系：
    - 将层次标签编码成binary matrix S，
        - person : [1,0,0,...]
        - person/artist: [1,1,0,....]
        - person/artist/musician: [1,1,1,...]
    - 通过binary matrix 与 参数矩阵V 的相乘就可以得到标签矩阵，用来分类。
    - This enables us to share parameters between labels in the same hierarchy,
    potentially making learning easier for infrequent types that can now
    draw upon annotations of other types in the same hierarchy.

---------------
#### Fine-Grained Entity Type Classification by Jointly Learning Representations and Label Embeddings - EACL 2017
- Training set partition
    - 将远程监督构建的训练数据划分为clean和noisy 数据
    - 如果标注在一个分类路径上，则为clean，例如:person-artist-actor
    - 反之为noisy,例如:person-artist-politician
- Model
    - Mention Rep
        - char lstm 单向
    - Context Rep
        - include entity mention tokens within both left and right context to explicitly encode context relative to an entity mention
        - 左边->entity end
        - entity start -> 右边
        - 双向LSTM 编码
    - Feature and label embeddings
        - 将Context和mention rep 作为feature rep， 映射成label的空间，将二者的dot product作为相似度
    - optimization
        - clean data hinge loss，1标签和0标签
        - noisy data hinge loss，最大logit标签，和0标签
    - inference
        - 预测时从顶至下，计算该层label与feature的score，选择最大得分的节点。直到节点score<0
        - Why? 如果一个人既是singer 又是actor?

------------------------
#### Hierarchical losses and New Resources for Fine-Grained Entity Typing and Linking    - ACL 2018
- Introduction
    - 实际上实体和其类型应该具有层次结构，大多方法忽略了这种结构，或者引入层次信息提升很小
    - 现有的数据集，类型层次结构很浅只有两个
- Corpora
    - 医学语料，平均层次14.4
- Model
    - Token Representation
        - 词向量
        - 位置向量，
    - Sentence Representation
        - CNN+MAX-pooling
    - Entity Mention Representation
        - 平均
    - Mention-Level Typing
        - binary cross entropy
    - Entity-Level Typing
        - mention-level的标注都是用远程监督标注的，所有的mention都会标注成该entity所属的所有type
        - Multi-Instance Multi-Label
        - 构建同一个entity的bag，每次采样k个mentions进行训练
        - 最后的bag predictions 通过逐元素的LogSumExp
    - Entity Linking
        - 对于数据集中的每个mention，生成100个候选entities。
        - 计算sentence representation和 候选embedding的距离，
    - Hierarchical:
        - 两个类型之间的关系是isA，s(c1,c2）
        - 学习类型标签的embedding，类似于学习知识图谱中isA关系的节点embedding
        - 以多任务方式训练，最后的损失是二者之和。

-------------------------------------------------
#### Neural Fine-Grained Entity Type Classification with Hierarchy-Aware Loss - NAACL 2018
- 目前的fine-grained entity type 数据都是通过远程监督的方式得到。
    - 首先利用NER识别Entity mention
    - 将Entity mention 和知识图谱的Entity 关联
    - 找到知识图谱中每个entity的类型，作为目标entity mention的候选类型
    - out-of-context：例如齐达内既是教练又是运动员，但在某个上下文中只可能是一种类型
    - overly-specific:标注过细，在某个文章中齐达内只是person，但给他标注成了coach
- Model
    - embedding：位置向量是与mention的相对距离，随机初始化。与词向量拼接
    - Mention:
        - 词向量平均
        - mention左右各一个词，形成sequence，利用单向lstm进行学习
    - Context：
        - 将mention与context拼接，利用双向LSTM+attention进行encode
    - 损失函数类似于单标签分类的交叉熵
    - 针对ooc：只给与预测最大的logit损失
    - 针对os：父标签损失函数不同权重


### ultra-Fine Entity Typing -ACL 2018
- Distant Supervision
    - Entity Linking
        - 提高召回率，维基百科第一句话都是实体的介绍
    - Contextualized supervision
        -
- Model
    - context representation：
        - 词向量与位置向量（看当前词是否before，inside，或者after mention）拼接。双向LSTM加attention
    - Mention Representation
        - character based CNN
        - weighted sum of word embeddings
        - 最终的表示是二者的拼接
    - Label
        - 多标签分类，sigmoid(Wr)
    - Multi-task objective
        - 远程监督构建的数据集会使得标签分布不正常
            - KB会标注更general的
            - head words 会标注更细粒度的
            - 缺少不同层次的类型，不意味着错误。例如head word是inventor，但不应该不鼓励模型去预测person
        - 训练的时候只有在每个层次上有真实标签，才会更新该层次的标签矩阵


---
### improving neural fine-grained entity typing with knowledge attention    AAAI 2018
- Introduction
    - entity-context separation
        - 目前的方法都是对context和entity 分别建模，忽略了二者的相关性
        - 例如: _Gates and Allen co-founded Microsoft, which became the largest software company._
          当考虑 _Microsoft_ 的实体类型的时候 _company_ 应该是重要的，但考虑 _Gates_ 的实体类型的时候应该不是很重要
    - text-knowledge separation
        -  知识图谱中的关系信息可以有助于实体类型判断
        -  (USA, shares_border_with, Canada), 可以推断出Canada 的实体类型
- Model
    - entity mention representation
        - 词向量平均
        - 通常实体是由1,2个词组成，因此复杂模型（rnn,cnn）容易过拟合
    - context representation
        - 双向lstm 对左右文本编码，然后计算注意力，加权求和
    - type predictor
        - 多标签分类sigmoid binary cross entropy
    - Attention
        - semantic attention，location attention，注意力计算与实体无关
        - mention attention：将entity mention 作为query 计算双线性匹配度，
        - knowledge attention： 利用TransE 将关系信息编码到实体向量
            - 利用TransE的实体向量和双向lstm表示计算注意力
            - 测试阶段无法确定entity mention对应的是知识库中哪个实体，甚至会out-of-kb。==why==?
            - 利用context和mention 进行entity mention 重构，计算真实与重构向量的L2距离作为损失函数
        - Knowledge Attention Disambiguation
            -  匹配entity mention 和知识库中的实体候选列表
            - 计算候选列表中的表示与重构表示的L2距离
            - 如果该距离大于某个阈值则认为可以替代，否则用重构的向量作为表示


---
#### Fine-grained Entity Typing through Increased Discourse Context and Adaptive Classification Thresholds
- Model
    - entity representation
        - 词向量平均
    - sentence representation
        - 两层双向lstm句子编码
        - 利用实体表示与句子表示计算双线性匹配注意力
    - document representation
        - 利用doc2vec 的表示进行mlp encode
    - Adaptive Thresholds
        - 根据验证集选择阈值

-----
#### Imposing Label-Relational Inductive Bias for Extremely Fine-Grained Entity Typing ACL 2019

- 跟kbp 2019比赛相关的是这个实体类型细化的任务: [2012 AAAI] Fine-Grained Entity Recognition
Figer: 65.5
[2015 ACL] Embedding Methods for Fine Grained Entity Type Classification
[2016 EMNLP] AFET: Automatic Fine-Grained Entity Typing by Hierarchical Partial-Label Embedding
AFET算法, Figer: 66.4
[2016 SIGKDD] Label Noise Reduction in Entity Typing by Heterogeneous Partial-Label Embedding
LNR算法, Figer: 74.9
[2016 AKBC] An Attentive Neural Architecture for Fine-Grained Entity Type Classification
Figer: 74.94
[2017 EACL] Neural Architectures for Fine-grained Entity Type Classification
Attentive算法, Figer: 75.36