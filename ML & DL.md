[toc]

### 可视化

1. 绘制多图

   ```Python
   import matplotlib.pyplot as plt
   plt.subplot(row, col, i) # row行，col列的大图上，绘制第i个图
   ```

2. 概率分布图

   ```
   plt.hist(x=x, bins=10) # bins控制柱子数量
   ```

3. 散点图

   ```
   plt.scatter(x_list,y_list)
   ```

4. 柱状图

   ```
   name_list = ['Monday','Tuesday','Friday','Sunday']  
   num_list = [1.5,0.6,7.8,6]  
   plt.bar(range(len(num_list)), num_list,color='rgb',tick_label=name_list)  
   ```

5. 像素、大小设置

   ```
   import matplotlib.pyplot as plt
   plt.rcParams['savefig.dpi'] = 300 #图片像素
   plt.rcParams['figure.dpi'] = 300 #分辨率
   ```

### ML

1. sklearn
2. xgboost
3. lgbm
4. 自动调参工具
   1.  hyperopt
   2. *GridSearchCV*
5. 集成学习
   1. Boosting：对当前训练样本的分布/权重进行调整，使得当前基学习器训错的训练集数据在后续的数据中更受关注（典型：AdaBoost
   2. Bagging：有放回地对训练集进行采样，采N份，训练N个基学习器，求和作为结果，随机森林即用的此种集成方式
   3. Stacking：M条数据，对训练集进行K折交叉验证，使用N个模型进行训练，形成M * N的矩阵，利用新模型重新进行训练，即模型叠加
   4. Blending：类似于Stacking，从训练集中准备一部分数据作为留出集，使用这部分不相交的数据作为Base Model，重新训练
6. PCA
   1. 用以下两个衡量标准来降低特征的维度，两个目标的结论一致
      1. 最近重构性：样本点到超平面的距离足够近
      2. 最大可分性：样本点在超平面的投影尽可能分开
   2. 假如特征维度从N降低到M，则通过矩阵做特征值分解，取最大的M个特征值对应的特征向量
   3. 与原始特征相比，舍弃了N - M个特征，舍弃的必要性
      1. 使得样本的采样密度更大
      2. 小特征值对应的特征向量往往与噪音数据有关

### DL

1. rnn
2. cnn

