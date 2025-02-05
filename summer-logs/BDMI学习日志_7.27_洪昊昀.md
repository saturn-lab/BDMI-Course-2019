# BDMI学习日志

作者: 洪昊昀

日期：2019年7月27日

## Abstract

今天由课程以前的助教郑文勋学长来和我们讲解深度学习在业界的特点。业界和学术里有许多的相似，当然更有很多的不同，成本、效率、产品迭代速度等，都是需要考虑的，而且业界里的很多理论可能和专业学生的常识有些出入，比如阿里使用的大多数是CPU，而不是我们学生一直都想使用的GPU，因为在它的应用场景下，CPU会更加高效，这在后面会进行解释。

## 深度学习在业界的特点

### 应用场景特点：

1. **流式数据，吞吐量大**---------->分布式，实时
2. **数据采集复杂，有噪声 **---------->数据清洗，来源查证
3. **模型需要快速迭代**-------->往往就因为效益要求，不会把模型做到完美
4. **任务丰富** -------->迁移学习



### 模型结构特点：

YOUTUB论文的图：前面是特征，比如watch vector   search vector  (每个人评估数据来源)，后面加了全连接网络，但是在**丰富的embedding+简单的核心**。

**丰富的embedding+简单的核心**这样的架构是为了工程实现的便利，分布式的便利。



### Sparse embedding

学号->one hot->全连接

One-Hot编码，又称为一位有效编码，主要是采用N位状态寄存器来对N个状态进行编码，每个状态都由他独立的寄存器位，并且在任意时候只有一位有效。One-Hot编码是分类变量作为二进制向量的表示。这首先要求将分类值映射到整数值。然后，每个整数值被表示为二进制向量，除了整数的索引之外，它都是零值，它被标记为1。

总而言之，one-hot编码就相当于一条在某一点被涂黑的纸带。

等价于一个学号到向量的映射

用查表代替矩阵乘法

O（1）的复杂度可以达到



**优化方法论：**

以Sparse特征（ID类）为核心

连续变量甚至会分段后作为Sparse使用

简单核心一般不动

特征交叉很常见（年龄和性别之类的经常作为embedding表的特征）



**分布式：**

ID数量多、类型多

丰富的Embeding+简单核心

。简单核心网络上的方向梯度传播

根据embedding表的key的梯度分配

Model Parallel

  按照Embedding分机器（梯度分配）

Data Parallel

  多个worker同时处理果皮数据（核心网络的梯度计算） 重点：基本工程设施和架构搭建



**迁移学习：**

多任务的迁移学习：（在空间上）

  本质相同的特征共享embedding

  同样的学号，在预测迟到率和预测挂科率上共享Embedding（Embedding一样，但是后面的核心不同，一个用来预测迟到率，另一个预测挂科率）



单任务在时间上的迁移学习（时间是越短越好的）数据的分布可能随时间在改变

  实时数据进行模型训练（直接把目前所有的数据扔进去然后顶下模型是不对的）

  适应天与天之间的差别

在数据池里面更新，数据池是拿其中一小时的数据，然后模型在跑，数据是不断更新的





**Rules of Machine Learning: Best Practices for ML Engineering**

1. 业务刚开始，没有数据，写规则即可、
2. 开始使用ML，数据已经积累起来了，规则已经很多了，维护不了了，模型可以简单，但是基础设施一定要扎实（为迭代做准备）
3. 特征工程，往模型里添加特征就能或的收益
4. 模型结构优化，微调



业界：分布式存储和计算，sql会用的比较多，查询数据方面有在用

​            要做的事情一般是在别人的任务上添砖加瓦，

​			但是日常用的语言自己都要会

​			

阿里的理论是，如果怕不用图像，CPU的效率会更高，因为全连接网络没什么矩阵运算，而且CPU会更加便宜

写好代码，再放到它的分布式系统里，然后十几二十分钟才能看到debug结果

本机跑不会有什么问题，放到分布式里面，数据处理上会出现问题，因为有数据先后的问题，是比较复杂的。



## 大作业展示

看了同学们的很多展示，大家对开源项目复现了，不过对具体解决方案的描述不是很清晰，大多停留在表面框架上，论文研究方面就是有些较多新知识的论文没有深入浅出地讲解，底层的数学知识没有讲解得很清晰，展示效果不是很好。同学们的选题角度各种各样，也让我了解到了很多的深度学习方向，以后也会多在深度学习方面扎实理论基础，进行更有价值的开发。



