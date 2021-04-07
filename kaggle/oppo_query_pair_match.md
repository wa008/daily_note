### 知识待补

1. lookahead
2. RoBERTa
3. ELECTRA
4. albert



# oppo pair match

1. weight 验证
2. 预训练优化器使用lookahead
3. 统计词频，wwm
   1. Beg->end bug？统计词频，修改首词判定
   2. __ + # 生成词频统计
4. 三个模型融合





| 实验                                              | train set | valid set | online score |
| ------------------------------------------------- | --------- | --------- | ------------ |
| Train_epoch=1 finetune_epoch=1                    |           | 0.8?      | 0.78?        |
| Train_epoch=40 finetune_epoch=20                  | 0.99981   | 0.944     | 0.857        |
| 利用pretrain model 且 embeding [CLS]等对齐        | 0.99987   | 0.955     | 0.872        |
| 修改为CLS标签，分数降低，epoch修正中              | 0.9997    | 0.915     | 0.814        |
| 基于第三版 不完全增强语料 wa009-v8 预训练loss 1.0 | 0.9998    | 0.9506    | 0.867        |
| 训练集合增强 然后划分验证集                       | 0.9998    | 0.991     | 0.872        |
| 修改weight 1:1.1                                  | 0.9998    | 0.991     | 0.874        |
| Lookahead + 1:1.3 v6                              | 0.9999    | 0.992     | 0.8778       |
| 1:1.4 v7                                          | 0.9999    | 0.9929    | 0.8772       |

| -                             | -       |        |                      |
| ----------------------------- | ------- | ------ | -------------------- |
| 使用n-gram mask               | 0.99999 | 0.9953 | 0.884897;提升0.00001 |
| 1:0.7 weight v1               | 0.9999  | 0.993  | 0.877030             |
| ELECTRA+k-fold+checkpoint集成 |         |        | 0.885                |
|                               |         |        |                      |



# 已知结论

1. finetune 利用 cls 标签，而不是整合全量到一维，会导致结果下降
2. 预料增强
   1. pair位置互换
   2. 单句和pair一起用
   3. 利用并查集构造更多pair
3. 利用预训练好的模型，重新加载不预训练直接finetune会导致结果不一样。Trainer上应该还有其他坑
4. 负样本也构造样本增强
5. 预训练增强修改为仅label=1
6. 验证重新加载模型，能导致效果与训练后使用保持一致
   1. BertTokenizer保存机制暂时有问题。可以重新加载上次生成的vocab文件;
7. 调整交叉熵的类别权重，暂时比例1:1.3最好，参考原始的训练集黑白样本比例





rsync -e 'ssh -p 8057' -ah --progress ./oppo_data.tar.gz  root@106.12.19.48:/data/kaggle/

pip uninstall typing

pip install git+https://github.com/huggingface/transformers

2080TI 和 P100 差不多，80w数据，预训练都要1个小时左右（2080 50min，P100 55min）





```
35.5609
```