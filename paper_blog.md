# Emotion-Cause Pair Extraction

#### 作者的目标

降低【情感-原因】pair提取的难度，使得【情感】tag不需要提前标注，与工业界的应用场景更加贴合

#### 如果文中提出了一种新技术，那么提出这种新技术的关键因素是什么

把目标任务分为两步，使得【情感】tag的提取变为机器任务，不依赖人工标注

#### 论文中那些内容对你有用

任务划分为两步，解决痛点

对同一个clause中的每个word通过LSTM+Attention做represention，得到clause的表征向量，再对不同的clause做LSTM，提取clause的上下文相关的表征，表征提取方式对我来讲很新颖

> inter-EC任务中，为什么拿clause的上下文无关表征si去预测，而不是clause的上下文相关的表征ri去预测呢？

inter-EC and inter-CE任务，利用emotion的结果作为cause判定的输入，充分利用信息

> 既然信息互补有效，那双方同时互补会不会更有效，独立训练 emotion 和 cause，再利用emotion 和 cause的结果分别重训 cause 和 emotion，达到相互提升的效果？

#### 你还想关注哪些参考资料

暂无