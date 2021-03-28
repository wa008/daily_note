# oppo pair match

1. finetune 利用 cls标签，而不是所有
2. 样本增强，修改dataloader
   1. 样本增强通过修改文本语料修改
      1. 语料 a、b位置互换
      2. a=b&b=c ;a = c
   2. datacollect 修改为 mask+replace
   3. n-gram暂时看只能自己实现
      1. 问下各个群；科学空间
      2. 知乎、so已经提问
3. K-flod；3.25日搞



| 实验                                              | train set | valid set | online score |
| ------------------------------------------------- | --------- | --------- | ------------ |
| Train_epoch=1 finetune_epoch=1                    |           | 0.8?      | 0.78?        |
| Train_epoch=40 finetune_epoch=20                  | 0.99981   | 0.944     | 0.857        |
| 利用pretrain model 且 embeding [CLS]等对齐        | 0.99987   | 0.955     | 0.872        |
| 修改为CLS标签，分数降低，epoch修正中              | 0.9997    | 0.915     | 0.814        |
| 基于第三版 不完全增强语料 wa009-v8 预训练loss 1.0 | 0.9998    | 0.9506    | 0.867        |
|                                                   |           |           |              |

