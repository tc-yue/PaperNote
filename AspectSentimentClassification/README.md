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
    - [Memory](###memory)
    - [Transfer](###transfer)
- [code](https://github.com/songyouwei/ABSA-PyTorch)

### RNN
- Effective LSTMs for Target-Dependent Sentiment Classification - COLING 2016
### CNN
- [Aspect Based Sentiment Analysis with Gated Convolutional Networks - ACL2018](AspectBasedSentimentAnalysiswithGatedConvolutionalNetworks.md)
- Parameterized Convolutional Neural Networks for Aspect Level Sentiment Classification EMNLP 2018
### Transfer
- [Exploiting Document Knowledge for Aspect-level Sentiment Classification -ACL2018](ExploitingDocumentKnowledgeforAspect-levelSentimentClassification.md)
- Exploiting Coarse-to-Fine Task Transfer for Aspect-level Sentiment Classification -AAAI 2019
### Memory
- Aspect Level Sentiment Classification with Deep Memory Network - ACL 2016
- Recurrent Attention Network on Memory for Aspect Sentiment Analysis -EMNLP 2017
- Target-Sensitive Memory Networks for Aspect Sentiment Classification - ACL 2018
- Convolution-based Memory Network for Aspect-based Sentiment Analysis - SIGIR 2018

### Attention
- Attention-based LSTM for Aspect-level Sentiment Classification - EMNLP 2016
- Interactive Attention Networks for Aspect-Level Sentiment Classification - IJCAI 2017
- Aspect-level Sentiment Classification with HEAT (HiErarchical ATtention) Network -CIKM 2017
- Multi-grained Attention Network for Aspect-Level Sentiment Classification EMNLP 2018
- Targeted Aspect-Based Sentiment Analysis via Embedding Commonsense Knowledge into an Attentive LSTM AAAI 2018
- Aspect Sentiment Classification with both Word-level and Clause-level Attention Networks IJCAI 2018

### Transformer
- Transformation Networks for Target Oriented Sentiment Classification  -ACL 2018

### Report
- SemEval-2014 Task 4: Aspect Based Sentiment Analysis
- SemEval-2015 Task 12: Aspect Based Sentiment Analysis
- SemEval-2016 Task 5: Aspect Based Sentiment Analysis