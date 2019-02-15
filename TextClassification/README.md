# Text Classification (文本分类)
- 预测一段文本的类别，
    - [广义文本分类](##TextClassification)
    - [多标签文本分类](###Multi-Label)
        - 预测文本所属的多个标签，类别不一定
    - [文档级文本分类](###DocomentLevel)
        - 长文本，文档级



### TextClassification
1. Recurrent Convolutional Neural Networks for Text Classification - AAAI 2015
    - 传统的RNN是一个bias model， 倾向于保存后面的词信息。CNN 是unbiased model，可以平等区别短语不同部分，
    而且卷积核窗口是很难确定的，小窗口会导致关键信息损失，大窗口会导致大的参数空间（很难训练）
    - max-pooling layer (判断哪些词在文本分类中是重要的)
    - 实际上是一个双向RNN层，后面接一个最大池化层。


### DocumentLevel

### Multi-Label
1. [Sequence Generation Model for Multi-Label Classification —— COLING2018](SequenceGenerationModelforMulti-LabelClassification.md)