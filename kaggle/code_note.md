# 代码笔记

监控大内存变量

```python
import psutil
import os
import sys

print ("{:.2f}".format(psutil.Process(os.getpid()).memory_percent()))
out = []
for var, obj in list(locals().items()):
    out.append([var, sys.getsizeof(obj) // 1024 // 1024])
for a, b in sorted(out, key = lambda x: -x[1])[: 5]:
    print("{:}\t{:}MB".format(a, b))
```

