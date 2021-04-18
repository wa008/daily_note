# TODO

1. 再看看李宏毅的PDF
2. 看Bert原paper



# 笔记

1. attention是在seq2seq的基础上给隐藏层变量添加了新的信息，这些信息=输入层各隐藏层的加权和
2. NLP中利用样本增强，对原始句子进行反转，能取得一定的效果
3. 汇总
   1. RNN：循环网络结构，n个输入，n个输出
   2. LSTM：复杂的RNN
   3. Attention：注意力机制，可以应用到循环网络场景
   4. Self-attention：可以替代RNN，但怎么用一个输出描述n个输入？
      1. 链接上线文
      2. 支持并行
      3. 复杂度为n^2*d；n为序列长度，d为Embeding长度
   5. seq2seq：利用RNN，解决输入输出长度问题。可以用于不定长输入和输出
   6. Transformer：用self-attention替换了RNN的seq2seq结构。但整个结构还不太懂
   7. ELMo
      1. 利用LSTM预测next token
      2. 可以双向，但左右两段是没有交汇的
   8. Bert：pre-train训练出一个Embedding，再进行微调？
      1. pre-train：训练很吃资源。有各种pre-train的方法
         1. bert训练就是挡住一部分输入，用周围的输入来预测挡住的输入。spambert即挡住连续多个输入
            1. 挡住
            2. 替换
            3. 不替换
         2. UniLM看来很叼的样子，支持多输入多输出结构、语言模型结构、seq2seq结构
      2. 微调：各种微调方法
         1. 只微调pre-train后面的模型结构
         2. 微调整个模型，包括pre-train
         3. 针对pre-train部分，只微调一部分参数，微调部分称为adapter
   9. ernie
      1. n-grams
      2. 大量数据



# 疑问

1. Transformer解码层为什么多次循环Masked-multi-Head-Attention

# Transformer

1. self-attention中 1/sqrt(k)缩放

   Dot-product attention 和 additive attention 理论复杂度差不多，但是实际上dot-product attention 更快，因此选择了前者。当k较小时，两者的效果差不多，k较大时，dot-product 较差，猜测是k较大时，容易引起梯度出现极小值，因此缩放 1/sqrt(k)，抵消此部分影响。

2. Self-attention中怎么把batch_size 加进去





# Bert

1. Contextualized word Embedding: 输入整句话，考虑上下文，输出每个单词的Embedding
2. Bert and its family
   1. UniLM
      1. 全网络，Mask一部分，类似Bert的训练
      2. 只能看左边的信息，类似language model
      3. 包含encode 和 decode，encode可以看全部信息，decode只能看左边的信息。类似seq2seq模型
3. 其他预训练模型
   1. fast text： 利用英文的每个字母，生成Embedding vector
   2. ELMo - LSTM
   3. bert - self-attention
   4. Glove 不考虑上下文？



### NLP 问题

1. QA问题
2. 机器翻译



# pretrain model

1. fast text：利用英文的每个字母，生成Embedding vector

2. w2v：predicttive model

   1. cbow：用周围词预测当前

      将C个周围词的onehot向量求和，乘以 输入Embeding矩阵，变成C个周围词的隐藏层表示；再乘以输出Embeding的转置，变回长度为V的向量，对中间词向量的概率取对数，作为损失函数。 

      反向传播依次更新 输出Embeding矩阵和输出Embeding矩阵。最后还是求和使用吧。

   2. Skip-gram：当前词预测周围词

      与cbow同理，只是包含多个损失函数

   3. 其他补充

      1. 为什么需要两次Embeding：容易优化；最后将两者取平均

3. glove：count-base model

   **基本概念**：基于全局的token共现次数，构造任意两个token之间的共现矩阵，利用词向量去拟合共现矩阵。

   **公式推导**

   假如两个词之间的距离为d，那么他们的共现值为1/d，作者利用以下的公式表达 词向量和共现值之间的关系

   $$w_i^T\tilde{w_j} + b_i + \tilde{b_j} = log(X_{ij})$$

   其中$w_i, \tilde{w_j}$ 是我们要求解的词向量，$b$ 表示两个词向量的bias term，$b_i=log(X_i)$，利用上述公式，就可以构造loss function了，论文中利用MSE做为loss function，如下

   $$J=\sum\limits_{i,j=1}^{V}{f(X_{ij})(w_i^T\tilde{w_j} + b_i + \tilde{b_j} - log(X_{ij}))^2}$$

   其中$f(X_{ij})$为权重函数，共现次数越多的token，他们的权重越高，共现次数越小，权重越小，共现=0，就不参与loss计算。 同时希望权重函数不能过大。因此设计权重函数如下

   $$ F(x)=\left\{
   \begin{array}{cases}
   (x/x_{max})^{\alpha}       &      & {x>x_{max}}\\
   1     &      & {else}
   \end{array} \right. $$

   $\alpha$论文中设置0.75，$x_{max}$取100

   **训练过程如下**

   采用了AdaGrad的梯度下降算法，对矩阵中的所有非零元素进行**随机采样**，学习曲率（learning rate）设为0.05。最终学习得到的是两个vector是和 ${x}$ 和 ${\tilde{w}}$，因为${X}$是对称的（symmetric），所以从原理上讲 ${x}$ 和 ${\tilde{w}}$ 也是对称的，他们唯一的区别是初始化的值不一样，而导致最终的值不一样。所以这两者其实是等价的，都可以当成最终的结果来使用。但是为了提高鲁棒性，我们最终会选择两者之和作为最终的vector（两者的初始化不同相当于加了不同的随机噪声，所以能提高鲁棒性）。

   **优秀博客**：http://www.fanyeong.com/2018/02/19/glove-in-detail/

4. cbow vs glove from stanford cs224 lecture02

   1. glove：基于计数
      1. 训练速度快
      2. 高效使用到了统计数据
      3. 能捕捉到单词之间的相似性。比如swim 和 swiming
      4. 对共现次数大的pair过于重视；可以通过限制最大值解决
   2. cbow：基于预测
      1. 随词表库大小变动；glove也是这样？
      2. 在单词相似性之外能捕捉到更复杂pattern
      3. 可以在其他任务上继续训练，提高性能；类似pretrain, finetune的形势
      4. 没有完全利用统计数据

5. ELMo：两个单向LSTM训练

6. GPT：transformer in LM

7. Bert

   1. Unidirectional Transformer -> Biddirectional Transformer
   2. Standard LM -> Masked LM
   3. Only Token-level prediction -> Next Sentence prediction

8. ERNIE

   1. 多任务，大数据训练
   2. Ngram mask

9. XLNET

10. T5



