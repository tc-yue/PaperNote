# sklearn 学习
### 1.13 特征选择
- [sklearn](http://scikit-learn.org/stable/modules/feature_selection.html#feature-selection)
- [结合Scikit-learn介绍几种常用的特征选择方法](http://www.cnblogs.com/hhh5460/p/5186226.html)
- [特征工程](http://www.cnblogs.com/jasonfreak/p/5448385.html)
1. VarainceThreshold
   - 去除方差没有达到某个阈值的特征。

2. 单变量特征选择
    - 单变量特征选择基于单变量统计检验选择最佳特征。
        - ==selectKbest== 保留k最高特征
        - ==selectPercentile== 保留最高比例特征
        - ==GenericUnivariateSelect== 可以通过超参搜索选择最优单变量特征
        - For regression: f_regression, mutual_info_regression
        - For classification: chi2, f_classif, mutual_info_classif
    - 这些方法通过f-test评估两个随机变量的线性依赖程度。另一方面互信息方法可以捕获任何的统计依赖，但是由于参数，需要很多准确预测例子
3. 递归特征淘汰
    - 给定一个额外的学习器其可以给特征权重（如线性模型的系数），递归特征淘汰是通过递归的考虑越来越小的特征集合从而选择特征。首先在初始特征集合上训练，每个特征的重要性通过==coef_ /feature_importance== 得到。然后从当前特征集合淘汰最不重要特征。这个程序递归的重复直到选择到目标数目的特征。
4. 使用SelectFromModel 选择
    - ==SelectFromModel== 是一个元转换器，可以被用于任何拟合后带有coef或feature_importances方法的学习器。被认为不重要的特征可以移除，如果相关coef或feature_importances值低于threshold。除了特定threshold数值，有内建启发式方法去找到threshold。
    - L1-based：具有L1正则的线性模型有稀疏的解决方法，预测的许多系数是0.当目标是减少数据的维度而使用其他分类器。选择非0系数
    - Tree-based:基于树结构学习器可以被用于计算特征重要性，反过来可以被用于抛弃无关特征。
5. 特征选择作为pipeline的一部分.
    - 特征选择通常被用于预处理步骤。推荐使用Pipeline
    ```
    clf = Pipeline([
      ('feature_selection', SelectFromModel(LinearSVC(penalty="l1"))),
      ('classification', RandomForestClassifier())
    ])
    clf.fit(X, y)
    ```
    - 在这个小片段，我们利用了SVC加上selectfrommodel 去评估特征重要性和选择最相关特征。然后，在转换后的输出上也就是仅仅使用相关特征训练随机森林。

