比赛链接：https://www.kaggle.com/c/riiid-test-answer-prediction

### 训练数据

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

### 大佬notebook

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



0.773 todo



### EDA 

+ userid+contentid groupby，90%的size=1

### 构造特征

+ userid：
  + 能力：回答对的概率 - 问题难度
+ question
  + 回答数量难度-回答对的概率，第一次回答对的概率；
  + userid+contentid： rank+avg
  + tag维度：置信度、加权和、取值
+ lecture
+ 其他



### 实验结果