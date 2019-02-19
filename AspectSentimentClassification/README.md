# AspectSentimentClassification (面向评价对象情感分类)
- 面向评价对象的情感分类主要包括两种
    - Aspect Category  评价对象类，判断文本中对某一主题类（预先给定）的情感倾向
    - Aspect term  评价词， 判断文本中出现的某一个实体（可以是一个词或短语）的情感倾向
    - "Average to good Thai food, but terrible delivery"
        - 评价词是Thai food
        - 评价对象类是service（没有出现在文本中）

- 分类
    - [RNN](###rnn)
    - [CNN](###cnn)
    - [ATTNETION](###attention)
    - [Transfer](###transfer)
- [code](https://github.com/songyouwei/ABSA-PyTorch)


### CNN
- [Aspect Based Sentiment Analysis with Gated Convolutional Networks - ACL2018](AspectBasedSentimentAnalysiswithGatedConvolutionalNetworks.md)

### Transfer
- [Exploiting Document Knowledge for Aspect-level Sentiment Classification -ACL2018](ExploitingDocumentKnowledgeforAspect-levelSentimentClassification.md)

### Memory
- Target-Sensitive Memory Networks for Aspect Sentiment Classification ACL 2018

### Attention
- Multi-grained Attention Network for Aspect-Level Sentiment Classification EMNLP 2018
- Targeted Aspect-Based Sentiment Analysis via Embedding Commonsense Knowledge into an Attentive LSTM AAAI 2018
- Aspect Sentiment Classification with both Word-level and Clause-level Attention
Networks IJCAI 2018