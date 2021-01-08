比赛链接：https://www.kaggle.com/c/riiid-test-answer-prediction

比赛题目：给出每个用户历史做过的题目，上过的课程，预测下次做某个题目时是否能作对

实验结果：用大佬开源的notebook拿到最高分，丢人。。。0.785

总结：

1. notebook类型的比赛可以自己本地train model，然后上传到notebook上做预测？
2. 提交的notebook需要被自动重新运行一遍，要在比赛结束前运行完才算有效，仅仅在比赛结束前submit是不行的
3. lgb基本是构造各种强特，自己加了一个特征，提升了0.001
4. 应该是可以当做序列问题处理的，但没有train过序列模型
5. kaggle的notebook是可以del删除变量内存的。
   1. `df = df[df['col'] != 'A']`同变量变小不能回收内存
   2. `df2 = df[df['col'] != 'A'] ;del df;`转移变量后del，能回收内存
   3. 查看变量占内存的大小时，也占内存
6. kaggle的新套路是不显示测试集数据。测试集进行多次小迭代。此次6w次迭代，每次迭代40个样本，需考虑测试集特征生成的时效问题。
7. 数据量太大时，得找机器。。。



### 大佬notebook特征

kaggle-riid 0.772

1. lecture
   1. 对type和partof做onehot处理，最后置为0/1
   2. userid参加lecture的次数
   3. userid参加讲座数/（lecture+question数）
2. user / content / task_container  下 count / sum / std / cerrectness
3. user 尝试 content 的次数
4. quetion中的part字段
5. userid解题的平均时间，userid解当前题目的时间,contentid解题的平均时间，时间差
6. timestamp变 hour？
7. Lagtime： 当前时间与userid下最大时间的差值
8. tags 变 onehot
9. bundle_id
10. Part_bundle_id
11. explanation_cum
12. prior_question_had_explanation
13. 树模型无法学习除法、减法，多构造除法特征
14. tag cum求和、计数
15. 数据泄露：groupby中除了cumcount、cumsum，都可能存在泄露问题；max_time_stamp



### 构造特征

+ userid：
  + performence：能力，回答对的概率 - 问题难度
+ question
  + 回答数量难度-回答对的概率，第一次回答对的概率；
  + userid+contentid： rank+avg
  + tag维度：置信度、加权和、取值
+ lecture
+ 其他



### 实验结果

6. train: 0.75966, test:0.72584
   1. 新修改样本
7. 添加特征user_id_prior_question_elapsed_time_mean
   1. train: 0.75986, test:0.72247
   2. auc降低
8. 添加user_lecture_count; 
   1. train: 0.7596598700, test:0.7258373359
   2. 提升0.001
9. 添加user_question_count;
   1. train: 0.7597523969, test:0.7268702075
   2. 提升0.002



### 数据

+ train
  + `row_id`: (int64) ID code for the row.
  + `timestamp`: (int64) the time in milliseconds between this user interaction and the first event completion from that user.
  + `user_id`: (int32) ID code for the user.
  + `content_id`: (int16) ID code for the user interaction
  + `content_type_id`: (int8) 0 if the event was a question being posed to the user, 1 if the event was the user watching a lecture.
  + `task_container_id`: (int16) Id code for the batch of questions or lectures. For example, a user might see three questions in a row before seeing the explanations for any of them. Those three would all share a `task_container_id`.
  + `user_answer`: (int8) the user's answer to the question, if any. Read -1 as null, for lectures.
  + `answered_correctly`: (int8) if the user responded correctly. Read -1 as null, for lectures.
  + `prior_question_elapsed_time`: (float32) The average time in milliseconds it took a user to answer each question in the previous question bundle, ignoring any lectures in between. Is null for a user's first question bundle or lecture. Note that the time is the average time a user took to solve each question in the previous bundle.
  + `prior_question_had_explanation`: (bool) Whether or not the user saw an explanation and the correct response(s) after answering the previous question bundle, ignoring any lectures in between. The value is shared across a single question bundle, and is null for a user's first question bundle or lecture. Typically the first several questions a user sees were part of an onboarding diagnostic test where they did not get any feedback.
+ question
  + `question_id`: foreign key for the train/test content_id column, when the content type is question (0).
  + `bundle_id`: code for which questions are served together.
  + `correct_answer`: the answer to the question. Can be compared with the train `user_answer` column to check if the user was right.
  + `part`: the relevant [section of the TOEIC test](https://www.iibc-global.org/english/toeic/test/lr/about/format.html).
  + `tags`: one or more detailed tag codes for the question. The meaning of the tags will not be provided, but these codes are sufficient for clustering the questions together.
+ lecture
  + `lecture_id`: foreign key for the train/test content_id column, when the content type is lecture (1).
  + `part`: top level category code for the lecture. 
  + `tag`: one tag codes for the lecture. The meaning of the tags will not be provided, but these codes are sufficient for clustering the lectures together.
  + `type_of`: brief description of the core purpose of the lecture

