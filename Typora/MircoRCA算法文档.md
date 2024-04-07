# MicroRCA算法

***

### 1. 算法简介

​	MicroRCA是一种通过利用系统指标延时数据和性能指标数据进行异常检测，并根据链路结构关系划分异常传播路径，进行根因定位的算法。该算法适用于有服务间调用关系、以及系统监控指标的微服务架构场景，输入需要两份数据，分别是服务响应时间指标和系统资源利用率指标，输出服务间根因排序，定位微服务之间异常传播的根本原因。

​	该算法异常检测阶段基于三个假设，假设1: 服务是否异常与服务响应时间强相关；假设2: 主机的资源利用率与服务的响应时间强相关 ；假设3: 容器资源利用率与服务性能相关。因此，不满足以上假设时，算法的准确性将会受到影响。

### 2. 算法输入输出

（1）算法输入要求：

​		  -- 应用程序级别指标：pandas.Dataframe格式，需要时间戳timestamp，以及若干微服务间的响应时间（根据需要而定）。

![image-20230217094047664](/Users/zyl/Desktop/Typora/graph/image-20230217094047664.png)

​		  -- 系统级别指标数据：pandas.Dataframe格式，

![image-20230217094243809](/Users/zyl/Desktop/Typora/graph/image-20230217094243809.png)

（2）算法输出格式：

​		  -- pandas.DataFrame格式，有两列，一列是微服务名Micro_name，第二列是根因分数。

<img src="/Users/zyl/Desktop/Typora/graph/image-20230217101016210.png" alt="image-20230217101016210" style="zoom: 33%;" />

### 3. 算法参数

|   参数    | 数据类型 | 默认值 |                           参数说明                           |
| :-------: | :------: | :----: | :----------------------------------------------------------: |
| threshold |  Float   |  0.1   | 用来限制因果最大时间间隔，该值越小越能推断出间隔大的因果关系 |
|  window   |  Float   |   12   |      预处理时间窗口跨度，通过调节该值能得出有关联的事件      |
|   Width   |  Float   |   6    |                       可视化图表的长度                       |
|  Height   |  Float   |   4    |                       可视化图表的宽度                       |

### 4. 库函数与用法详解

```python
"""MicroRCA算法包结构"""
- RCA_Package
	- alg_main
    - anomaly_find.py       # 异常检测
    	- (class): AnomalyLearning  
    - root_causal_find.py   # 根因定位
    	- (class): RootCausalFind
  - test
    - test.py
```

