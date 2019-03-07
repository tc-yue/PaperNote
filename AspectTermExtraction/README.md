#### 评价对象抽取
- 抽取评论文本中的评价对象，
    - NER: extracting all aspect terms (e.g., “beef”) from a review corpus
    例如对于 _Its speed is incredible_ 要抽取出 _speed_
    - Topic Extraction: clustering aspect terms with similar meaning into categories
    where each category represents a single aspect
    (e.g., cluster “beef”, “pork”, “pasta”, and “tomato” into one aspect food).
    - Aspect categorization：多标签分类任务，从预定的几个主题类别中判断


### NER
- [A Survey on Recent Advances in Named Entity Recognition from Deep Learning models COLING2018](SurveyonDeepLearningforNamedEntityRecognition.md)
- [Survey on Deep Learning for Named Entity Recognition](SurveyonDeepLearningforNamedEntityRecognition.md)
- Bidirectional LSTM-CRF Models for Sequence Tagging
- End-to-end Sequence Labeling via Bi-directional LSTM-CNNs-CRF
- Fast and Accurate Entity Recognition with Iterated Dilated Convolutions
- Named entity recognition with bidirectional LSTM-CNNS
- Chinese NER Using Lattice LSTM

### Aspect Term Extraction
- [Double Embeddings and CNN based Sequence Labeling for Aspect-Extraction -ACL2018](DoubleEmbeddingsandCNNbasedSequenceLabelingforAspectExtraction.md)
- Aspect Term Extraction with History Attention and Selective Transformation -IJCAI 2018
- A Unified Model for Opinion Target Extraction and Target Sentiment Prediction -AAAI 2019
- Coupled multi-layer attentions for co-extraction of aspect and opinion terms. AAAI
- Deep multi-task learning for aspect term extraction with memory interaction.
- Lifelong-rl: Lifelong relaxation labeling for separating entities and aspects in opinion targets. EMNLP
- Lifelong learning crf for supervised aspect extraction. ACL

### Aspect Topic Extraction
- [An unsupervised neural attention model for aspect extraction. -ACL2017](AnUnsupervisedNeuralAttentionModelforAspectExtraction.md)