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

| 日期       | 计划                       | 进展     |
| :--------- | -------------------------- | -------- |
| 11.11 周三 | 制定计划                   | 制定计划 |
| 11.12      | word embedding             | done     |
| 11.14      | 四个小时学习，作业五六七八 |          |
| 11.15      | 四个小时学习，作业九       |          |



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

