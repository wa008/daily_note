# oppo pair match

1. 样本增强，修改dataloader
   1. 负样本也构造样本增强
      1. q1-q2 = 1, q2-q3 = 0 ---> q1-q3 = 0
   2. 调整交叉熵的类别权重
   3. n-gram暂时看只能自己实现
      1. 问下各个群；科学空间
      2. 知乎、so已经提问
2. checkpoint 集成？
3. Lookahead
4. K-flod；最后再搞



| 实验                                              | train set | valid set | online score |
| ------------------------------------------------- | --------- | --------- | ------------ |
| Train_epoch=1 finetune_epoch=1                    |           | 0.8?      | 0.78?        |
| Train_epoch=40 finetune_epoch=20                  | 0.99981   | 0.944     | 0.857        |
| 利用pretrain model 且 embeding [CLS]等对齐        | 0.99987   | 0.955     | 0.872        |
| 修改为CLS标签，分数降低，epoch修正中              | 0.9997    | 0.915     | 0.814        |
| 基于第三版 不完全增强语料 wa009-v8 预训练loss 1.0 | 0.9998    | 0.9506    | 0.867        |
|                                                   |           |           |              |



# 已知结论

1. finetune 利用 cls 标签，而不是整合全量到一维，会导致结果下降
2. 预料增强
   1. pair位置互换
   2. 单句和pair一起用
   3. 利用并查集构造更多pair
3. 利用预训练好的模型，重新加载不预训练直接finetune会导致结果不一样。Trainer上应该还有其他坑





rsync -e 'ssh -p 8057' -ah --progress ./oppo_data.tar.gz  root@106.12.19.48:/data/kaggle/

pip uninstall typing

pip install git+https://github.com/huggingface/transformers

2080TI 和 P100 差不多，80w数据，预训练都要1个小时左右（2080 50min，P100 55min）