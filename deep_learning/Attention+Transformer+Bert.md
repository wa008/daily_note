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
   5. seq2seq：利用RNN，解决输入输出长度问题。可以用于不定长输入和输出
   6. Transformer：用self-attention替换了RNN的seq2seq结构。但整个结构还不太懂
   7. Bert：pre-train训练出一个Embedding，再进行微调？
      1. pre-train：训练很吃资源。有各种pre-train的方法
         1. bert用到的微调就是挡住一部分输入，用周围的输入来预测挡住的输入。spambert即挡住连续多个输入
            1. 挡住
            2. 替换
            3. 不替换
         2. UniLM看来很叼的样子，支持多输入多输出结构、语言模型结构、seq2seq结构
      2. 微调：各种微调方法
         1. 只微调pre-train后面的模型结构
         2. 微调整个模型，包括pre-train
         3. 针对pre-train部分，只微调一部分参数，微调部分称为adapter





# 疑问

1. Transformer解码层为什么多次循环Masked-multi-Head-Attention
2. Batch Norm 和 Layer Norm



# Bert

1. Contextualized word Embedding: 输入整句话，考虑上下文，输出每个单词的Embedding
2. Bert and its family
   1. UniLM
      1. 全网络，Mask一部分，类似Bert的训练
      2. 只能看左边的信息，类似language model
      3. 包含encode 和 decode，encode可以看全部信息，decode只能看左边的信息。类似seq2seq模型



