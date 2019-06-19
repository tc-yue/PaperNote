
## 深度学习超参
- Reference
    - [我搭的神经网络不work该怎么办](https://mp.weixin.qq.com/s/ZpodcrPMGSgBzz-JpNeaUQ)
    - [如何诊断长短期记忆网络模型的过拟合和欠拟合？](https://mp.weixin.qq.com/s/5jQzI3siqY2Kc4oTPUjiSA)
    - [克服过拟合和提高泛化能力的20条技巧和诀窍](http://blog.csdn.net/starzhou/article/details/52754436)
    - [深度学习与XGBoost在小数据集上的测评](https://mp.weixin.qq.com/s/PF6-txnq3SV258EfNVufSw)
    - 吴恩达深度学习
    - [李宏毅深度学习技巧](https://www.bilibili.com/video/av10590361)
    - A sensitivit analysis of cnn for sentence clf
    - [dropout](https://www.jianshu.com/p/ba9ca3b07922)
    - [深度学习调参技巧](https://www.zhihu.com/question/25097993/answer/585714651)


### 1.防止过拟合

 - dropout 0.2-0.5，每一层都可以加
     -  以一定概率删除节点，添加噪声
     -  在网络中的每个线性层（如输入层卷积层或稠密层）前加入Dropout层。
     -  原始dropout论文推荐在隐层加入constraint，确保权重的最大范数不会超过3.
         ```
         Dense(60, kernel_initializer='normal', activation='relu', kernel_constraint=maxnorm(3))
         ```
     - 使用较大学习率并使用衰减和大的动量组合，constraint效果好，因为其可以防止大的学习率导致的参数blow up
     - 数据量小的时候，效果不好


 - 正则项:
    -  bias项正则可以不加,因为W几乎涵盖所有参数,b影响不大
    -  L1 正则W 会很稀疏,包含很多0.倾向使用L2。lambda越大模型越简单low variance high bias。


### 2.batch_size
- 尝试设一个很大，很小的batchsize，在其中用网格搜索选。epoch尽量打，LSTM和CNN对batch 比较敏感。
- 过大会降低梯度下降的随机性，8，16逐渐增大。减少随机梯度更新的方差，带来更大的步长意味着优化算法进展的更快。
较大的batch允许大的步长，但会有一个极限，一旦超过一定步长增加batch大小不再允许增加步长
- 较小的能带来更多起伏、跳出局部最小，训练结束平滑。
-  minibatch loss 或许会上升，SGD 噪声低，loss变化震荡，训练慢


### 3.学习率
- adaptive learning rates.
    - reduce the learning rate by some factor every few epochs.
    - 可以快速学到较好参数，然后fine tuning
### 4.激活函数
- sigmoid function:
    - deeper不代表好，
- 梯度消失:
    - Input 层附近梯度小，learn slow；
    - output 层附近梯度大，learn fast
- relu function:
    - fast to compute
    - biological reason
    - infinite sigmoid with different biases
    - vanishing gradient problems
### 5.层数
- 浅层开始，有一定效果，再逐步更深
### 6.隐藏unit数量
- 隐藏单元数量256到1024
- 如果你正在做分类，可以使用类别数目的5到10倍，作为隐藏单元的数量；
- 如果做回归，可以使用输入或输出变量数目的2到3倍。
- 隐藏单元的数量通常对神经网络性能影响很小
### 7.参数初始化
- 优先级较低 fromNG
### 8.超参选择
- Do not use grid。，从最要的开始选择
### 9.激活函数
- relu：计算快速，生物学理由，处理梯度消失

### 10.正则化的网络的激活函数
-  Normalize inputs can speed up learning
-  在使用激活函数之前batch Normalize





---------------------------------------------------
- 李宏毅深度学习tips
    - 首先在训练集检查performance（如果不好，模型本身没学到），在验证集检查（如果不好可能过拟合），
    - 模型好不一定是过拟合，可能在训练数据也不好。需要检查在训练集的结果
    - ![](http://img.blog.csdn.net/20171022214242354?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXVlMTE1MTE4MDcwMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
