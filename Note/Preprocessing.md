[比较不同缩放方法的效果](http://scikit-learn.org/stable/auto_examples/preprocessing/plot_all_scaling.html#sphx-glr-auto-examples-preprocessing-plot-all-scaling-py)
- 如果两个特征非常不同的scale 和包含许多非差大的离值点。这两个特点会导致数据可视化的困难，更重要的是它们会降低许多学习算法的预测性能。没有scaled的数据会很慢甚至使得很多基于梯度的学习器很难收敛。
- 实际上许多学习器假设每个特征值接近于0且所有的特征在可比较的scales上。特别地，基于metric和基于gradient的学习器经常假设近似的标准化数据（单位方差的中心数据）。==显著的例外是基于决策树的学习器对于任意scaling的数据都很鲁棒==
---------------------------
## 4.3 Preprocssing data
- 无量钢化：使不同规格的数据转换到同一规格，标准化（前提是特征值服从正态分布）和区间缩放法。
- 标准化是依照列处理，归一化依照行处理，目的在于样本向量在点乘运算或其他核函数计算相似性时有统一标准，也就是都转化为单位向量。Normalizer
- 普遍来说学习算法会从数据集标准化中受益。如果数据集合中出现离群值，鲁棒的scalers或transformer更合适。
### 4.3.1 Standardization,mean removal,variance scaling
- ==StandardScaler==
    - 列处理。如果独立特征不太像标准正态分布的数据（均值0，方差1），一些学习算法的表现会差。例如学习算法的目标函数使用的许多元素（如svm的RBF核或者线性模型的l1,l2正则）假设所有特征中心化到0且有近似的方差，如果一个特征方差的数量级远大于其他，它可能主导目标函数，使得学习器不能从其他特征很好的学习。
  - ```$x = \frac{x-\overline{x}}{s}$```
  - 这个类适合在Pipeline的早期步骤
 #### 4.3.1.1 Scaling features to a range
- 将特征缩放到某个最大最小区间内。通常是[0,1]。使用缩放的动机包括非常小标准差的特征带来的鲁棒性，保留稀疏数据的0。
- ==MinmaxScaler MaxAbsScaler==
- ```$x = \frac{x-min}{max-min}$```
### 4.3.3 Normalization
- 行处理。fit无用，单独处理每个样本。正则是让单个样本具有单位标准的过程。对于想要使用二次方例如点积或其它核去衡量样本对相似度时很有效。这个假设是使用在文本分类和聚类的向量空间模型的基本。==Normalizer==
- L2 ```$x = \frac{x}{\sqrt{\sum x^2}}$```

### 4.3.4 Binarization
#### 4.3.4.1 Feature binarization
- 特征二值化是将数值特征以阈值分为布尔值。对于后续的概率估计是有效的因为它假设输入数据是多变量伯努利分布的。在文本处理中也是常见（大概是简化概率推理）的即使对于正则化的计数（词频）或TDIDF特征通常效果略好
### 4.3.5 Encoding categorical features
- 有很多特征不是连续的而是类别离散的。这些特征可以用数字表达。但是这些数字表达不能直接用在学习器中，因为它把类别解释的有序了，通常不希望这样。使用==OneHotEncoder==转换类别特征。
### 4.3.6 Imputation of missing values
- 许多数据包含缺失值。基本的方法是丢去整个包含确实值的行或者列。然而这会带来丢失有价值数据（即使不完整）的代价，一个更好的策略从已知的部分数据中推断。==Imputer==
### 4.3.7 生成多项式特征
-  考虑输入数据的非线性而加入复杂特征是有效的。==PolynomialFeatures==
### 4.3.8