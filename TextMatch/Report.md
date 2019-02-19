---
#### Beihang-MSRA at SemEval-2017 Task 3: A Ranking System with Neural Matching Features for Community Question Answering
- 两个自然语言句子表达相同意思但使用不同却语义相关的词。
- subtask A:   question-comment similarity
     - 给定1个question和10个comments，根据相似度排序。
            设计一些特征判别comment是好还是坏：comment长度，comment是否包含url或email。
- subtask B:   question-question similarity
     给定1个question和10个questions，根据相似度排序。
- ranking scores：
    - text preprocessing.
    - 用空格替换特殊字符和标点符号，小写化，去除停用词，词干化，句法分析。nltk， stanford parser。
    - feature extraction
	 - 传统特征:
		 - 将文本转为tfidf的one hot 表示，计算两个文本的cosine
            最长公共子序列，ngram词重叠，tree kernels?translation probability?
     - 神经匹配特征：
	     - w2v cosine
	     -  bilstm+cnn 计算文本对的匹配得分作为特征，M1是u的词向量矩阵dot r词向量矩阵，M2是u隐层输出矩阵dot A dot  r隐层词向量矩阵(A是一个参数）。
	     对于taskB，question对标注perfect match 和relevant 为1，irrelevant为0。
	      ![这里写图片描述](http://img.blog.csdn.net/20171106210117329?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXVlMTE1MTE4MDcwMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
	- feature combination:
    xgboost 结合所有特征。

-----------
####  [哈工大基于关键词及主题的CNN模型](https://mp.weixin.qq.com/s/TYEWKdX4IEDLd8rnsASnnw)
> - 融合主题模型的方法及基于神经网络模型的方法。同时考虑了全局信息及细节信息。充分利用主题信息，减少无关信息对判断问题相似的影响。减少问题长度造成的偏差。
1. 关键词抽取：基于词引力及依存关系的抽取。对问题的每个子句抽取关键词后将其排序。
1. 基于关键词相似及相异特征的CNN模型：检查一个关键词序列中的关键词在语义上能否被另一个关键词序列中的某些关键词替代。根据皮尔森相关系数计算相似矩阵Am*n
1. 计算主题相似度：
2. 问句相似度计算：
![这里写图片描述](http://img.blog.csdn.net/20171106215245194?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXVlMTE1MTE4MDcwMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
- 王煦祥. 面向问答的问句关键词提取技术研究[D].哈尔滨工业大学,2016.
- Wang Z, Mi H, Ittycheriah A. Sentence Similarity Learning by Lexical Decomposition and Compos-ition[J]. 2016.