# Attention

1. Attention有哪些参数可以学习？
2. 训练之后怎么适配测试集中的不同长度？
3. 源码过于复杂



# Transformer

1. self_attention：三个输入为q, k, v
2. feedforward有两个线性层，神经元个数：d_model -> dim_feedforward -> d_model；中间层需要activation，末层不需要
3. 每层的每一attention, linear layer都有dropout
4. encoder 6层
   1. 每个输入层一输入，一输出
   2. 当前层的输出为下一层的输入
5. decoder 6层
   1. 每层两个输入，memory （encoder层的输出）和 target，一个输出 output
   2. 每层的memory输入保持不变，都是encoder层的输入；output作为下一层的输入
6. decoder的每一层中的第二个 multi-head attention
   1. query = 上一attention的输出
   2. key = value = encoder 输出的 memory
7. transformer类似的seq2seq结构的模型，预测时第一个output的target可以用开始标识符来代替
8. 实战demo
   1. 仅仅构建encode, https://pytorch.org/tutorials/beginner/transformer_tutorial.html
   2. 稍微改造了下transformer源码, https://towardsdatascience.com/how-to-code-the-transformer-in-pytorch-24db27c8f9ec
9. mask, src, taget, memory 都分别有mask, xxx_key_padding_mask？
   1. 区别
   2. target第一个attention 为mask attention, why
10. 怎么处理输出长度不确定的问题，比如机器翻译

encodeLayer

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

