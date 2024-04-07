# THP算法文档

***

### 1. 算法简介

​	THP算法是一种基于拓扑Hawkes过程的因果推理算法，主要应用于有拓扑关联的告警数据，将不同的告警定义为不同类型的event（事件），推理不同事件之间的因果关联，找出故障根因。

​	THP算法处理的数据是非独立同分布型的，它能通过告警事件序列和主机拓扑关联模拟因果传播，推断因果关系。可以有节点间的拓扑数据，也可以没有。

### 2. 算法输入输出

（1）算法输入要求：

​		  -- 告警数据：pandas.Dataframe格式，需要event、timestamp(float)、node散列，列名按约定定义。

​		  -- 拓扑数据：np.npy格式，可以有，也可以无；若没有该数据，则输入None。

（2）算法输出格式：

​		  -- pandas.DataFrame格式，有两列，第一列尾ROOT，第二列尾RESULT，每一行代表着一对因果关系。

### 3. 算法参数

|  参数  | 数据类型 | 默认值 |                           参数说明                           |
| :----: | :------: | :----: | :----------------------------------------------------------: |
| delta  |  Float   |  0.1   | 用来限制因果最大时间间隔，该值越小越能推断出间隔大的因果关系 |
| window |  Float   |   12   |      预处理时间窗口跨度，通过调节该值能得出有关联的事件      |
| Width  |  Float   |   6    |                       可视化图表的长度                       |
| Height |  Float   |   4    |                       可视化图表的宽度                       |



### 4. 库函数结构与用法详解

```python
"""
THP算法包结构
"""
- TTPM_Package
	- alg_model
  	- __init__.py
    - data_simulation.py
    - prior_effect.py
    - ttpm_model.py
    - ttpm.py    # main()
    	- class SimulationData
      - class Read_data
      - class CausalInference
    - utils.py
  - test
  	- generate_data
    - test.py
```

### ttpm.py 分解

- SimulationData（模拟数据生成）   
- CausalInference（因果关系学习）
- Read_data  (读取数据)

***

1. **SimulationData（模拟数据生成） ** 

```python
# 实例化类
simul = SimulationData(dag_node, dag_edge, topo_node, topo_edge)
```

其中：dag_node: 所生成因果图事件的数量；dag_edge：因果事件连接边数；topo_node：拓扑节点数量；topo_edge：拓扑连接边数量。   

```python
# 调用函数simulation生成模拟数据
data, true_dag, topo_matrix = Simu.Simulation()
```

其中：data：模拟生成的数据，包含三个字段event、timestamp、node；true_dag: 模拟生成的真实因果图；topo_matrix: 模拟生成的拓扑关系矩阵。

2. **CausalInference（因果关系推理)**

```python
# 实例化类
causal = CausalInference(df, topo_matrix, delta)
```

其中：df：告警数据，包括字段event、timestamp、node； topo_matrix：设备拓扑图；delta：重要参数，通过调节该参数，确定存在因果关系的时间阈范围，delta越小，允许因果关系时间差越大。



**训练数据，学习因果关系**

```python
pre_causal_matrix = causal.train()
```

其中：pre_causal_matrix为学习到的因果关系矩阵。

**可视化**

```python
# 1.拓扑图（无向图）
causal.output_topo(width, height)
# 2.影响力图（有向图）
causal.output_influence(pre_causal_matrix, width, height)
```

其中：width和height分别为可视化图像的宽和高，pre_causal_matrix为训练得到的影响力图。

**输出**

```python
causal.output_result(pre_causal_matrix, dict_to_digital, save_path)
```

其中，pre_causal_matrix为训练学习到的因果关系矩阵； dict_to_digital为设备拓扑图到数字的映射关系字典； save_path为输出因果关系的存储路径。

##### 3. **Read_data (数据读取)**

**读取数据**

```python
# 方式1：MySQL读取
R = Read_data()
data = R.read_sql(conn_List, sql_language)
```

其中：conn_List是一个列表，包含四个元素host（MySQL服务器地址）, user（用户名）, password（密码）, db（数据库名称）, charset（连接编码）；sql_language是读取的MySQL语句。

```python
# 方式2：读取csv文件
data = R.read_csv(path)
```

****

### 用法示例：

```python
from TTPM_Package.alg_model.ttpm import *

"""模拟生成数据"""
# Simu = SimulationData(dag_node=4, dag_edge=7, topo_node=16, topo_edge=15)
# data, true_dag, topo_matrix = Simu.Simulation()
# data['node'] = 0

"""读取真实数据"""
data = pd.read_csv("shyh_data.csv")

"""running......"""
"""-------------------------"""
causal = CausalInference(data, topo_matrix=None, delta=0.01)
pre_causal_matrix = causal.train(wind=300)
dict_to_digital = causal.node_map()

causal.output_topo(6, 4)
causal.output_influence(pre_causal_matrix, 6, 4)

save_path = "result.csv"  # 保存路径
causal.output_result(pre_causal_matrix, dict_to_digital, save_path)
```



