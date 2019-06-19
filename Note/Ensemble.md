### Reference
- [李宏毅机器学习](https://www.bilibili.com/video/av10590361/#page=23)
- 周志华机器学习第8章
- [使用sklearn进行集成学习](http://www.cnblogs.com/jasonfreak/p/5657196.html)
- [stacking心得](https://zhuanlan.zhihu.com/p/26890738)
- [xgboost实战](http://blog.csdn.net/sb19931201/article/details/52557382)
- blending  https://zhuanlan.zhihu.com/p/42229791

### bagging：
- 当原模型已经很复杂的时候，bias已经很小但variance很大时候。比较容易overfit的模型：Decision  Tree（在train上100%）
- random forest：bagging of decision tree

### boosting

- 将bias较大的弱学习器组合。f1较弱，->f2 来辅助（和f1差异较大），->f3 ...  学习器是顺序学习的
- 获取不同的学习器：a 训练数据重采样，训练数据重赋权重（改变cost function）
- adaboost
- 先训练f1，然后找到新的训练数据在f1上效果不好（50%。 reweighting 训练数据），用其训练f2。
- gbdt
    - ![](pics/ensemble/1.png)
    - ![](pics/ensemble/2.png)

### stacking

- voting final classification 是lr就可以，不需要太复杂。
- stacking oof 要固定kfold，否则容易过拟合。
    - 对于每一条训练数据相当于有机会利用其余所有数据进行训练后的模型预测
- 将 training data 分为两部分，一部分训练元模型，一部分训练final
    - ![](pics/ensemble/3.png)

### Kaggle_Guide
- [kaggle ensemble guide](https://mlwave.com/kaggle-ensembling-guide/)
- 直接从结果csv文件ensemble
- hard majority vote
    - 计算person 相关系数，选择表现好的相关性较低的模型
- 加权多数投票
    - 计算最好的模型三次，其余的模型一次
    - 只有当很多模型都一致否决最优模型的结果时，才发生改变
    - 提升有限，只对最好的模型进行微小的修改
- 相关性
    - 模型之间的低相关性会增强修错能力
- rank averaging
    - 多分类，将类别预测概率转为排序值，然后将排序值平均
    - Ranking averages do well on ranking and threshold-based metrics (like AUC) and search-engine quality metrics (like average precision at k).