[toc]

### 本代码库主要包含的内容

1. 需要被copy的代码
2. 需要多看几遍的重要知识

### 可视化

1. 绘制多图

   ```Python
   import matplotlib.pyplot as plt
   plt.figure(figsize = (20, 5))
   plt.subplot(row, col, i) # row行, col列的大图上，绘制第i个图
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

### 工具

1. 结果集成也是有方法的？？

   ```url
   https://github.com/abhishekkrthakur/pysembler
   ```

2. 监控内存

   ```python
   print ("memory persent : {:}".format(psutil.virtual_memory().percent))
   ```

3. 自动调参工具

   1.  hyperopt
   2.  GridSearchCV

4. numpy

   1. 矩阵相乘，星乘代表相同位置做乘法，点乘代表矩阵相乘

      ```python
      c = a * b # 星乘
      c = a.dot(b) # 点乘
      c = np.dot(a, b) # 点乘
      ```

### NLP

1. 分词库

   ```
   from sklearn.feature_extraction.text import CountVectorizer
   vec = CountVectorizer(ngram_range=(2, 2)).fit(corpus) # 两个相邻单词作为一组
   ```

   

### ML

1. sklearn
2. xgboost
3. lgbm
4. 异常检测算法
   1. PCA：主成分分析
      1. 利用协方差矩阵提取关键的M维特征，达到降维的目的
   2. MCD: Minimum Covariance Determinant，最小协方差行列式
   3. SVM：
      1. S3VM：一种半监督学习方法，找到一个低密度可分割的平面
      2. TSVM：
      3. one-class SVM：假设训练集只有一种label
   4. LOF，Local Outlier Factor：局部异常因子，一个样本点周围的样本点所处位置的平均密度比上该样本点所在位置的密度
   5. kNN：k Nearest Neighbors
   6. HBOS：基于各个特征的频数直方图的无监督异常点检测算法
   7. ABOD：计算每个样本与所有其他样本对所形成的夹角的方差，异常点因为远离正常点，因此方差变化小。
   8. Isolation Forest：孤立森林
   9. Feature Bagging：笼统的算法
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
7. 评价指标
   1. auc: roc与x轴围成的面积
   2. roc: x轴为伪阳性率, 即TP / (TP + FN)，y轴为真阳性率，即(FP / (FP + TN)), 将二分类的阈值从1逐步降低为0时，那么roc曲线上的点将从(0, 0)开始，要么沿x轴移动一段，要么沿y轴移动一段。

### DL

1. pytorch
   1. 写完了猫狗demo，可以跑下测试机数据去kaggle上交一发
   2. backword使用的变量比如是torch，不能转为np后再转torch，这样会无法获取历史值？
2. mlp
   1. 最常见的塔状设计：1024ReLU->512ReLU->256ReLU（来自Coogle推荐系统文章
3. rnn
   1. demo写完了，还有个bug
   2. Embedding
   3. word2vec
4. cnn

### 不常用

1. 头文件

   ```python
   #encoding:utf-8
   import json
   import time
   import random
   import numpy as np
   import pandas as pd
   from collections import Counter
   import warnings
   warnings.filterwarnings('ignore')
   import sys
   from sklearn.externals import joblib
   from PIL import Image
   import numpy as np
   import matplotlib.pyplot as plt
   import torch
   import time
   from torch.autograd import Variable
   from glob import glob
   import os
   import torch.nn as nn
   from torch.optim import lr_scheduler
   from torch import optim
   from torchvision.datasets import ImageFolder
   from torchvision.utils import make_grid
   import shutil
   from torchvision import transforms
   from torchvision import models
   from torchtext import data, datasets
   from nltk import ngrams
   from torchtext.vocab import GloVe, Vectors
   from collections import defaultdict
   data_path = r'D:\kaggle\data\spooky-author-identification\a'[: -1]
   ```
