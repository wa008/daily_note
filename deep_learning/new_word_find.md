# 新词发现

### 词左右信息熵

取左邻和友邻的最小信息熵作为改词的信息熵。





### 内部凝聚度

凝聚度 = P(x) / (P(x1) * P(x2))

词的不同切分方法，会影响凝聚度，比如把「的影院」切分为「的影」和「园」是不合理的。所有要取所有切分方法的凝聚度最小值，作为该词的凝聚度



