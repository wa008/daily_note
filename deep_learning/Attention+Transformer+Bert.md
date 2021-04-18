# TODO

1. 再看看李宏毅的PDF
2. 看Bert原paper



### 背景知识

5. seq2seq：利用RNN，解决输入输出长度问题。可以用于不定长输入和输出
6. Transformer：用self-attention替换了RNN的seq2seq结构

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

11. UniLM看来很叼的样子，支持多输入多输出结构、语言模型结构、seq2seq结构



# Transformer

1. attention是在seq2seq的基础上给隐藏层变量添加了新的信息，这些信息=输入层各隐藏层的加权和

2. self-attention中 1/sqrt(k)缩放

   Dot-product attention 和 additive attention 理论复杂度差不多，但是实际上dot-product attention 更快，因此选择了前者。当k较小时，两者的效果差不多，k较大时，dot-product 较差，猜测是k较大时，容易引起梯度出现极小值，因此缩放 1/sqrt(k)，抵消此部分影响。

3. Self-attention中怎么把batch_size 加进去：self-attention进行Q、K、V计算时，是三维数组进行的计算，(a, b, c) * (c, x, y)，只要相邻的两维长度相同即可

4. 正则

   1. Residual add 之前进行dropout
   2. FFN：relu激活之后进行dropout

5. multi-head attention：利用self-attention将多个head concat，再乘以一个转化矩阵

6. FFN有两个线性层，神经元个数：d_model -> dim_feedforward -> d_model；中间层需要activation，末层不需要

7. 影响序列长度依赖性的一个重要因素，就是信号在输入-输出的网络结构中传输的长度，该长度越短，依赖性学习的就越好。因为信息丢失？

8. 从计算复杂度上讲，句子长度n越小，self-attention的性能就优于RNN，因为self-attention的复杂度为 n^2 * d，RNN的复杂度为 n * d ^ 2。那么n很大的时候，self-attention的性能就会下降，要怎么优化呢？

9. attention对模型解释性的贡献

10. 疑问

    1. Transformer解码层为什么都要mask：6层中第一层需要，其他层要不要都行，为了方便都要了

```python
def __init__(self, d_model, nhead, dim_feedforward=2048, dropout=0.1, activation="relu"):
    super(TransformerEncoderLayer, self).__init__()
    self.self_attn = MultiheadAttention(d_model, nhead, dropout=dropout)
    # Implementation of Feedforward model
    self.linear1 = Linear(d_model, dim_feedforward)
    self.dropout = Dropout(dropout)
    self.linear2 = Linear(dim_feedforward, d_model)

    self.norm1 = LayerNorm(d_model)
    self.norm2 = LayerNorm(d_model)
    self.dropout1 = Dropout(dropout)
    self.dropout2 = Dropout(dropout)

    self.activation = _get_activation_fn(activation)

def __setstate__(self, state):
    if 'activation' not in state:
        state['activation'] = F.relu
    super(TransformerEncoderLayer, self).__setstate__(state)

def forward(self, src: Tensor, src_mask: Optional[Tensor] = None, src_key_padding_mask: Optional[Tensor] = None) -> Tensor:
    src2 = self.self_attn(src, src, src, attn_mask=src_mask,
                            key_padding_mask=src_key_padding_mask)[0]
    src = src + self.dropout1(src2)
    src = self.norm1(src)
    src2 = self.linear2(self.dropout(self.activation(self.linear1(src))))
    src = src + self.dropout2(src2)
    src = self.norm2(src)
    return src
```



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



