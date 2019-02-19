### Definition
- opinion targets: target/aspect term(出现在评价文本中的评价对象词)，aspect(评价方面)
- sentiments:positive,neutral,negative
- opinion holders:persons who hold opinions
- time:when opinions are given
--------------------
### Representation Learning
- 表示学习是情感分析的关键，推断文本的情感需要深度理解文本的语义
![文本表示](https://note.youdao.com/yws/public/resource/a7c99c18ac7067b5073d58b85f33e854/xmlnote/DFA073C8FBE646E8843F4A09A57AD494/2352)

- One-hot
    - 无法关联语义相似的词。microsoft & msft = 0
- Embedding
    - 将word映射到另一个空间，其中这个映射基于单射和结构保存性质
    - contininuous representation
    ![词向量](https://note.youdao.com/yws/public/resource/a7c99c18ac7067b5073d58b85f33e854/xmlnote/2E9AB38DD01542BD8AD837B55DA4CCF3/2366)
    - lookup table不是简单的查表，id对应的向量是可以训练的，训练参数的个数应该是word nums*embedding dims，也就是说lookup是一种全连接层
- Sentiment-specific word embedding
    - 普通的词向量只包含语义信息，情感信息较少。导致"good" 和"bad"的相似度较高。通常需要对词向量进行微调。
    - Learning Sentiment-Specific Word Embeddingfor Twitter Sentiment Classification - ACL 2014
        - 在C&W（目标将原始ngram得分高于随机替换中心词的得分）词向量的表示学习基础上，加入情感类别的目标函数，进行联合训练，期望得到与情感相关的词向量表示
        - C&W模型，目标对ngram进行打分，得分高低代表句子正常与否，正样本训练语料中所有的ngram，负样本是将ngram中的词随机替换得到的短语。
        - SSWEh 将C&W 模型顶层改成情感类别，目标情感分类的交叉熵。不用corrupted ngram。
        - SSWEr 让正向情感的得分尽量大于负向情感的得分，最后一层不使用softmax函数来进行概率预测。不用corrupted ngram。
        - SSWEu 损失函数加入C&W的损失，使其包含语义信息和情感信息。目标函数使得原始ngram语言模型得分比替换的好，原始ngram情感得分比替换的更接近于真实标注
        - 分别学习unigram，bigram，trigram
        - 效果泛化能力不一定好，比如在作者另一篇TDlstm 论文中就不如glove训练的词向量

    - BB twtr at Sem Eval-2017 Task 4: Twitter Sentiment Analysis with CNNs and LSTMs
        - Unsupervised training：利用Word2Vec，FastText，GLove在 一亿无标注推特语料训练词向量
        - Distant training：使用CNN 对distant 数据集进行情感二分类。对词向量进行微调，使得不同情感的词在向量空间中距离较远。Distant dataset：正负类各五百万，利用tweet中的表情符自动标注的noisy dataset。
    - Refining Word Embeddings for Sentiment Analysis - Emnlp 2017
        -  利用预训练的词向量和情感词典（具有情感得分[1-9]）对情感词向量进行refine。
        -  Nearest Neighbor ranking：首先利用预训练词向量计算和目标情感词的余弦距离最短的top k语义相似词作为其近邻，接着根据情感词典的情感score的差异进行re-rank。与目标词情感越近的词就会排名更高。
        -  Refinement model：对于某一个目标情感词，让其与其情感相似词更近，远离不相似的词，但也不要与初始的词向量更远。使用欧式距离
    - Building Large-Scale Twitter-Specific Sentiment Lexicon : A Representation Learning Approach
        - pass
- Cross lingual word embedding
    - WORD TRANSLATION WITHOUT PARALLEL DATA - ICLR 2018
- ----------
### Sentence-level
- 向量化 -> 句子表示 -> 情感分类
- cnn
    - K-max pooling
        - A convolutional neural network for
modelling sentences
    - 多个CNN神经网络
        -  Convolutional neural networks for sentence classification
    - 多种输入词向量
        -  A simple approach to exploiting multiple word embeddings for sentence classification
    - 字/词向量组合
        - Deep Convolutional Neural Networks for Sentiment Analysis of Short Texts
    - 非线性不连续的
        - Molding CNNs for text: non-linear, non-consecutive convolutions
- rnn
    - rnn
        - Context-Sensitive Lexicon Features for Neural Sentiment Analysis
    - rnn+cnn
        - Dependency Sensitive Convolutional Neural Networks for Modeling Sentences and Documents -NAACL 2016
            - CNN对于序列敏感，传统的卷积操作利用不同大小的卷积核抽取局部特征，类似于ngram处理。只有使用足够长的卷积核才能捕捉长距离依赖或者使用更深的网络。
            - 树结构或者循环神经网络的模型可以捕捉长距离依赖。
            - 在LSTM的输出后加入lstm
            - SST,IMDB,MR
        - Recurrent Convolutional Neural Networks for Text Classification - AAAI 2015
            - 先使用双向循环神经网络对序列编码，然后对每一个时间步的表示进行线性转换得到每个时间步的隐含语义向量。可以把这一步的循环结构看成卷积层 。然后利用最大池化的方式得到文本表示。
            - SST 数据集 47.21
- recursive nn
    - Sentence modeling with gated recursive neural network
- 利用额外数据
    -  Learning Distributed Representations of Sentences from Unlabelled Data
- 情感资源
    - 情感词典
        - Context-Sensitive Lexicon Features for Neural Sentiment Analysis.
    - 语言学约束
        - Linguistically Regularized LSTMs for Sentiment Classification
            -   在句子级情感分类中加入语言学知识包括情感词，负向词，程度副词。因为大部分负向词和程度副词都修饰其后面的词，所以使用反向lstm进行序列编码。对于LSTM序列中每个位置的语义表示进行情感分析。例如对于"an interesting movie",当序列编码到"movie"和"interesting movie"时的情感分布应该是不同的。
            - 使用三种正则。1，非情感正则，如果两个相连接的位置都是非情感词，那么两个位置的情感分布应该相似。2，情感正则，某一位置发现情感词，则与前面的位置的情感分布不同。3，否定词正则。4，程度副词正则
---------------------
### Aspect-level
- 显示找到文本中对aspect重要的词
- 实体级别(句子中出现的实体)
    - feature based svm
    - TDlstm
        -  Effective LSTMs for Target-Dependent Sentiment Classification
    - Deep memory，location enhanced
        -  Aspect Level Sentiment Classification with Deep Memory
Network
- 属性级别（实体对应的类别）
    -  attention
        - Attention-based LSTM for Aspect-level Sentiment Classification
        - Aspect-level Sentiment Classification with HEAT
        - Interactive Attention Networks for Aspect-Level Sentiment Classification
- 立场检测
    -   ：立场分析目标是识别出讨论或辩论双方的所持立场，这一任务与情感分
析密切相关，但相比情感分析，立场分析更具有挑战性。目前关于立场分析的研究
主要针对辩论网站内容，而对微博等社交媒体文本的立场分析研究刚开始起步。由
于微博文本长度短、语言表达自由，且回复关系非常稀疏，这些因素共同导致了针
对微博的立场分析效果较差。特别地，面向中文的立场分析几乎处于空白，因此业
界亟需为中文立场分析任务构建数据资源、举办评测活动，推动立场分析基础理论
与方法的研究与应用。
    - Stance Classification with Target-specic Neural Attention-Proceedings_results

-------------------
### Document-Level
- Rule lexicon based
    - 情感词 + 计数
    - 情感词 + 语法规则 + inference method
    - 利用情感资源中的词条，结合否定、转折、递进、等句法规则，对文本的情感极性进行判别。
- Feature based svm
    - 像对待topic-based categorization 一样的二分类，positive and negative bi-clf。svm + bag of words(n-grams) + 词性，情感词，句法特征等
    - bag of words 忽略词序和词的语义信息
- Latent n-gram
    -  Project n-gram to low-dimensional latent semantic space
        - Sentiment Classification Based on Supervised Latent n-gram
Analysis
- Paragraph vector
    -  doc2vec：represents each document by a dense vector which is trained to predict words in the document
- CNN
    - word -> sentence -> document
    - character cnn
- Hierachical NN
    - 语义组合性，层次化建模，词->句子->篇章
        - Document Modeling with Gated Recurrent Neural Network for Sentiment Classification.
    - 层次化注意力机制
        - Hierarchical Attention Networks for Document Classification
- Sentence Modeling
    - cnn with multiple filters.利用了unigram，bigram，trigram信息
    - rnn lstm
        - 使用最后一个hidden vectors
        - use all the hidden vectors(average them to get the docvec)
- Fast text
    - 平均词表示到文本表示，接着用于线性分类，可以不用预先训练的词向量，速度大幅提升。
- Text regions，one hot表示文本。
![image](https://note.youdao.com/yws/public/resource/a7c99c18ac7067b5073d58b85f33e854/xmlnote/921BAAD8C66E4CDC87A624730A08B8C2/2407)
- User and product
    - 将用户和产品信息用低维向量和矩阵表示，融入到词表示中。
        -  Learning semantic representations of users and products for document level sentiment classification
        - 上面模型输入层结合user vector和product vector计算量较大，其次只有word-level的用户和产品信息
    - 用户和产品attention机制
        - Neural Sentiment Classification with User and Product Attention
        - 用户向量和产品向量
-------------
### Emotion cause extraction 情感原因发现
- Clause level classification
- Phrase level extraction
    - Event-Driven Emotion Cause Extraction with Corpus Construction
    - A Question Answering Approach to Emotion Cause Extraction
