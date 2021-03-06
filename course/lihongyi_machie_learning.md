## 计划

计划两周搞完，deadline=11.22

1. Word embedding
2. 作业五六七、Explainable AI、Adversarial attack 对抗学习、network compression
3. Seq2seq 等作业八，三节课
4. 作业九，五节课
5. GAN 作业十一，10个视频，5节课左右
6. 作业十三、十四、十五，5节课左右

update from 2020.11.11



## 进展

| 日期       | 计划                       | 进展                                                         |
| :--------- | -------------------------- | ------------------------------------------------------------ |
| 11.11 周三 | 制定计划                   | 制定计划                                                     |
| 11.12      | word embedding             | done                                                         |
| 11.14      | 四个小时学习，作业五六七八 | 作业五2/8,作业六2/8,作业七２/6,作业八done                    |
| 11.15      | 四个小时学习，作业九       | 除了BERT，done                                               |
| 11.16      | GAN 1/10                   | done                                                         |
| 11.17      | GAN 3/10                   | done                                                         |
| 11.18      | GAN 4/10                   | done                                                         |
| 11.19      | GAN 5/10                   | done                                                         |
| 11.21      | META learning and so on    | META learning done;Meta learning - LSTM todo;life-long learning done;RL done;Advanced Version todo |



## 笔记

1. word embedding：oneHot输入 -> 一层隐藏层 -> 输入，隐藏层即Embedding向量
   1. 分类
      1. count base：用两个词预测他们同时在文章中出现的次数，比如Glove
      2. predict base：用前面的n个词预测后面的词
      3. Cbow: 用周围的词预测中间的词
      4. Skip gram: 用中间的词预测周围多个词
   2. 特性
      1. 国王 - 王后 = 叔叔 - 阿姨
      2. 语言特征，用于语言翻译：分别训练中英的Embedding，预先匹配一部分中英对照，dog = 狗 + (cat - 猫)
   3. 其他
      1. Document Embedding多个论文
      2. bag of word，但无法考虑时序
      3. 考虑时序
2. 解释机器学习
   1. 与shap值类似，调整各个特征值，看对结果的影响。非树模型中可以用微分表示
3. 模型攻击与反攻击
   1. 通过对x进行微调，使得预测值与真实目标相差越大
4. 网络压缩
   1. 在有限制的环境里训练模型
      1. 训大模型，通过剪纸等各种操作得到小模型（不断迭代一个循环，计算权重较小的网络和权重->删除该部分->微调补足效果下降）
      2. 直接训小模型，原理与上相悖
5. Pointer Network
   1. 从输入中选择概率最大的输入，参与decode的过程
6. 递归神经网络
   1. Tree LSTM
7. 无监督学习
   1. PCA：理论还要再看看。可用于推荐系统，根据人物和目标之间的关系，生成表示人物和目标的vector。
8. [Neighbor Embedding](https://youtu.be/GBUEjkpoxXc) 
   1. t-SNE: 主要用于可视化。将高维中的点以相同的相互距离映射到低维空间，方便进行可视化或其他操作。
9. Auto-encoder
   1. 语义和音色分开
   2. 不同的训练目标
   3. 容易被人类理解
10. GAN
    1. 第一节课
       1. 生成->评估。
       2. 每轮分别迭代生成和评估的network
       3. Generator: 自下而上，从局部到整体
       4. Dsicriminator: 自上而下，从整体到局部
    2. 第四节
       1. KL divergence
       2. Jensen-Shannon divergence
    3. 第五节fGAN
       1. 不同的f函数，衡量部分会是不同的函数，比如KL 散度，Reverse KL
11. Meta learning 
    1. 让学习一个具有学习f函数的F函数，F产出f，f产出数据结果
    2. Meta learning-Gradient descent LSTM:todo
12. life-long learning
    1. 终身学习，学完一个再学习另一个；顺序很重要
13. Reinforcement learning
    1. 对抗学习，obs+actor+reward，找到使得reward最大的actor
    2. 如果reward无法微分(求导)，那就用policy gradient硬train一发



