- [sklearn tutorial](http://scikit-learn.org/stable/tutorial/text_analytics/working_with_text_data.html)
- [blog](http://www.cnblogs.com/jasonfreak/tag/sklearn/)
- [网格搜索](http://blog.csdn.net/jasonding1354/article/details/50562522)
- [调参](https://www.zhihu.com/question/34470160)
- [sklearn建议](https://zhuanlan.zhihu.com/p/23339926)

### Introduction
```
# model persistence
from sklearn.externals import joblib
joblib.dump(clf, 'filename.pkl')
clf = joblib.load('filename.pkl')
```
### conventions
- type casting
	- 除非特定，输入会被cast为float64.
	- 回归target会被cast为float64,分类targets会保持原状。
	- 预测target保持原来的（integer或string）
- Multiclass vs Multilable fitting
```
from sklearn.svm import SVC
from sklearn.multiclass import OneVsRestClassifier
from sklearn.preprocessing import LabelBinarizer
X = [[1, 2], [2, 4], [4, 5], [3, 2], [3, 1]]
y = [0, 0, 1, 1, 2]
#分类器在一维多类标注上拟合，输出array([0, 0, 1, 1, 2])
classif = OneVsRestClassifier(estimator=SVC(random_state=0))
classif.fit(X, y).predict(X)
#分类器在二维二元标签上拟合，输出array([[1, 0, 0],
       [1, 0, 0],
       [0, 1, 0],
       [0, 0, 0],
       [0, 0, 0]])第五个和第六个实例返回全0，表明他们没有匹配上面任意一个标签。对于多标签输出，一个实例可以指定多个标签。
y = LabelBinarizer().fit_transform(y)
from sklearn.preprocessing import MultiLabelBinarizer
y = [[0, 1], [0, 2], [1, 3], [0, 2, 3], [2, 4]]
y = MultiLabelBinarizer().fit_transform(y)
```

- Model selection:choosing estimators and their parameters

```
#为cv策略生成train/test索引的列表
from sklearn.model_selection import KFold, cross_val_score
X = ["a", "a", "b", "c", "c", "c"]
k_fold = KFold(n_splits=3)
for train_indices, test_indices in k_fold.split(X):
     print('Train: %s | test: %s' % (train_indices,test_indices))
# 交叉验证
[svc.fit(X_digits[train], y_digits[train]).score(X_digits[test], y_digits[test])
         for train, test in k_fold.split(X_digits)]
#可以使用cross_val_score 直接计算交叉验证score。给定模型，交叉验证对象和输入数据，cvs重复分离数据到训练和测试集合。对于每个交叉验证的迭代，使用训练集合训练模型，根据测试集合计算得分
cross_val_score(svc, X_digits, y_digits, cv=k_fold, n_jobs=-1)
#给定数据，在拟合一个网格参数的模型期间计算得分，选择参数去最大化交叉验证得分。
from sklearn.model_selection import GridSearchCV, cross_val_score
Cs = np.logspace(-6, -1, 10)
clf = GridSearchCV(estimator=svc, param_grid=dict(C=Cs),
                   n_jobs=-1)
clf.fit(X_digits[:1000], y_digits[:1000])
#默认情况下 grid使用三折交叉验证。然而当检测到分类器时使用stratified 3-fold.
#nested cv
cross_val_score(clf, X_digits, y_digits)
两个交叉验证循环平行执行，一个是grid search去设置gamma，另一个是交叉验证去度量模型的准确。
```

