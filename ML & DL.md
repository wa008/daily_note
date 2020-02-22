[toc]

### 可视化

1. 绘制多图

   ```Python
   import matplotlib.pyplot as plt
   plt.subplot(row, col, i) # row行，col列的大图上，绘制第i个图
   ```

2. 概率分布图

   ```
   plt.hist(x=x, bins=10) # bins控制柱子数量
   ```

3. 散点图

   ```
   plt.scatter(x_list,y_list)
   ```

4. 柱状图

   ```
   name_list = ['Monday','Tuesday','Friday','Sunday']  
   num_list = [1.5,0.6,7.8,6]  
   plt.bar(range(len(num_list)), num_list,color='rgb',tick_label=name_list)  
   ```

5. 

### ML

1. sklearn
2. xgboost
3. lgbm
4. 自动调参工具

### DL

1. rnn
2. cnn

