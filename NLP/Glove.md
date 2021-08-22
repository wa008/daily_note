# 基本概念

考虑 $w1$ 和 $w2$ 的共显频率，构建向量拟合共显频率，loss函数如下
$$
\sum_{w1,w2}{F(X_{ij})(w_i^T\hat{w_j} + b_i + \hat{b_j} - log(X_{ij}))}
$$

# 调参

$\alpha$ ：业务上通过调整该指，使得 loss 更多关注低频共线词，我们不用过于关注高频词，只要高于阈值即可

# 其他思考

苏神认为添加的常量 $b$ 有问题，会导致停用词等出现次数多的词 模长更大，但实验效果一般。https://kexue.fm/archives/4675

相似度：模长代表了重要程度，可归一化之后再求内积，作为相似度。https://kexue.fm/archives/4677

苏神的 simple_glove 相比原生的 Glove，拟合的是两个词的 点间互信息，而原生Glovo 拟合共现频次 https://kexue.fm/archives/4675

# 参考博客

csdn少见的整齐博客：https://blog.csdn.net/coderTC/article/details/73864097

之前还能看，后来就没了：http://www.fanyeong.com/2018/02/19/glove-in-detail/

