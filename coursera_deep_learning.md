# Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization

## week 1

1. 指标
   1. trian, dev, test set： 数据量较小时（1000），按照60% 20% 20%划分。数据量较大时，dev and test可按照数据量进行划分，能表达模型表现即可
   2. 方差和偏差
2. 正则
   1. 参数惩罚项
   2. Droupout
   3. Early stopping: 根据不同迭代次数中验证集的loss变化趋势来选择stop位置
3. 目标优化
   1. 数据标准化： 二维数据距离，可以让数据分布更加分散
   2. 梯度 消失/爆炸
   3. 参数权重初始化
   4. 梯度check： 判断反向传播计算的梯度 与 微分计算的梯度 是否相等
      1. not use in training, only debug
      2. 同时计算参数惩罚项
      3. 不能和droupout一起用

## week 2

1. Mini-batch-gradient descent
   1. Batch_size = 2^x, 一般速度更快，性价比更高。
   2. 

![梯度下降时进行梯度平滑处理](/Users/huzhipeng03/Documents/github/daily_note/image-20200828093131542.png)

3. rmsprop
4. 