- https://github.com/cbaziotis/ntua-slp-semeval2018
-



====参数设置====
- 1:To reduce variance and boost accuracy, we ensemble 10 CNNs and 10 LSTMs together through soft voting. The models ensembled have different random weight initializations, different number of epochs (from 4 to 20 in total), different set of filter sizes (either [1,2,3], [3,4,5] or [5,6,7]) and different embedding pre-training algorithms
- 2：batch 128，Adam clip norm5，embedding 300,lstm 150,
    - l2 0.0001。Dropout embedding 0.3,lstm 0.5,lstm connection 0.25
    - 使用Bayes 优化调参。

- 3: 五折交叉验证。开发集最好模型，预测后取平均
    - glove 100维，dropout 0.2 embeddinglayer fine tuning；gru 64
    - adam batch 16

----------------------------

1. BB twtr at SemEval-2017 Task 4: Twitter Sentiment Analysis with CNNs and LSTMs
    -   采集推特数据正负例各五百万，利用表情符策略自动标注
    -   使用100million 无标注tweet预训练词向量。
    -   上面训练的词向量无情感，给词向量加情感微调。使用CNN对无监督的词向量训练微调
    -   The first epoch of the training is done with the embeddings frozen in order to minimize large changes in the embeddings. We then unfreeze the embeddings and train for 6 more epochs  After this training stage, words with very different sentiment polarity (ex. “good” vs. “bad”) are far apart in the embedding space.
    -    freeze them for the first ∼ 5 epochs. We then train for another ∼ 5 epochs with unfrozen embeddings and a learning rate reduced by a factor of 10. We pick the cross-entropy as the loss function, and we weight it by the inverse frequency of the true classes to counteract the imbalanced dataset
    -    不平衡 损失函数赋权

------------------------------

2. DataStories at SemEval-2017 Task 4: Deep LSTM with Attention for Message-level and Topic-based Sentiment Analysis
    - 双层LSTM for more abstract features ,后使用location based attention
    - 有源码
    - embedding层后面加入Gaossian noise，相当于给每一维度加一个高斯噪声， 随机数据增强，robust to over fitting
    - 在embedding后面，lstm内部，attention加权后每个dense前 加入dropout 比率不一样
    - dropout prevents co-adaption of neurons，like ensemble learning
    - 在分类dense层加入L2正则
    - 在 Adam中加入clipnorm
    - L2 正则加入到最后一层，
    - 早停 当验证集损失不再下降
    - 不微调embedding layers。在训练集中出现的词，分类器会对某些情感划分一定的向量空间区域。然而只出现在测试集没出现在训练集回保存在初始状态，这样就不会因为词向量微调，导致测试集未出现的词预测情感错误。---但即使微调，开发集的词也是一样的。
    - 不平衡 频率倒数
    -
    -


---------------------
3.Connecting Targets to Tweets: Semantic Attention-Based Model for Target-Specific Stance Detection
- BiGru-cnn :propagate all the semantic and syntactic information in a tweet into two fixed hidden state vectors, which could become a
bottleneck, when there exist some long-distance dependencies in the tweet