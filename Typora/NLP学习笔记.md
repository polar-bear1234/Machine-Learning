# WordVec

***

###### 函数是将某些输入变换为某些输出的变换器，神经网络也将输入变换为输出。对于三分类问题，神经网络将输入的每笔数据都变成了三维数据，每个维度对应各个类的得分。

###### 1个epoch相当于模型“看过”一遍所有的学习数据。

###### 在NLP中，计算机理解语言有基于同义词词典的方法，还有基于计数（统计）的方法，目前，使用语料库对单词进行向量化是主流方法。分布式假设：单词的含义由其周围的单词构成，含义上相近的单词距离上理应相近。

###### 用向量表示单词的方法可分为两种：一种是基于统计的方法；一种是基于推理的方法。都依赖于分布式假设。

###### word2vec中输入侧权重的每一行对应各个单词的分布式表示，这样的分布式表示能够很好的捕获单词的含义。CBOW模型学习的是语料库中单词的出现模式，学习过程就是调整权重，权重学习到蕴含单词出现模式的向量。

###### NLP中，单词的密集向量表示称为词嵌入(Embedding)，或者单词的**分布式表示**。用一个低维的向量表示一个物体，可以是一个词、物体等。其作用是使距离相近的向量对应的物体有相近的含义。用低维向量对物体进行编码，还能保留其含义。

###### 从一个权重参数矩阵中抽取“单词ID对应行（向量）”的层，即将矩阵的某个特定的行取出来，这里我们称之为Embedding层。在这个Embedding层存放词嵌入（分布式表示）。Embedding层，在某种程度上，就是用来降维的，降维的原理就是矩阵乘法 。

###### 基于word2vec获得的单词的分布式表示内嵌了单词含义，在相似的上下文中使用的单词在单词向量空间上处于相近的位置。

###### word2vec 的目的：获得单词的分布式表示，是用来生成词向量的工具。

###### word2vec 通过CBOW预测中间词或通过skip-gram预测周围词这个过程，在不断减小损失函数的过程中，不断优化了单词的向量表示，从而生成词向量。

##### embedding把稀疏矩阵变成了一个密集矩阵，这个密集矩阵用N个特征来表征所有的文字。在这个密集矩阵中，表象上代表着密集矩阵跟单个字的一一对应关系，实际上还蕴含了大量的字与字之间，词与词之间甚至句子与句子之间的内在关系，他们之间的关系，用的是嵌入层学习来的参数进行表征。embedding把独立的向量关联了起来。

![image-20230205114119496](/Users/zyl/Library/Application Support/typora-user-images/过度表.png)

###### 中间这个矩阵就是权重表，在embedding过程中也称为过度矩阵，最终的词向量就是通过这个过度矩阵按特定的行和顺序取出来的。
