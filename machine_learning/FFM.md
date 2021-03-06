# FM

https://tech.meituan.com/2016/03/03/deep-understanding-of-ffm-principles-and-practices.html

相比LR模型，引入了自动学习特征交叉的机制。每一维特征用一个低维稠密的隐向量来表示，特征交叉的权重即两个隐向量的点乘。

原始复杂度为 d * n ^ 2，利用二次项优化，可降低到 d * n。这一步优化利用了二项式分解，极其巧妙，可参考博客，优化之后可以在线性时间内训练和预测，非常高效。



# FFM

在FM的基础上，引入field的概念。使用field 将特征剥离开来，既区分了不同的field，也保证了同一个field内不同特征的相关性。

不同类型的特征被视为同一个field，比如onehot特征。相比FM，FFM对每一维特征构造f个隐向量，表示当前特征对其他field的隐向量，f表示总体field的个数。复杂度为d * n ^ 2 * f，无进行二项式优化。

1. 对每个样本加入特征值为1的特征，引入了因子化一次项，避免了因缺少一次项带来的模型误差。
2. 省略零值特征：零值特征对于模型没有贡献，省略后提升性能。
3. 样本归一化？奇怪的操作，但有时候很需要，类似于layer normalization



# DeepFM

http://fancyerii.github.io/2019/12/19/deepfm/

DeepFM = FM + DNN

把FM的高性能表征和DNN的深度表征融合在一起，共用Embeding层。同一个field使用一个低维稠密向量来表征