# program language

1. cpp
2. Python
    1. time模块
    
       ```python
       # time时间格式转换
       dt = "2020-07-29 12:00:00"
   timeArray = time.strptime(dt, "%Y-%m-%d %H:%M:%S") # 标准时间格式 转化为 时间数组
      timestamp = time.mktime(timeArray) # 时间数组 转化为 时间戳
      time_local = time.localtime(timestamp) # 时间戳 转换成 时间数组
      dt = time.strftime("%Y-%m-%d %H:%M:%S",time_local) # 时间数组 转换为 标准时间格式
      ```
3. Go
4. Java

