## Hadoop学习：

1. 框架：
   1. HDFS    海量数据存储系统
   2. MapReduce  分布式运算模型
   3. YARN  资源管理

Yarn设计的初衷就是为了在同一套集群上面跑不同的应用框架。至于为什么要在同一套集群上面跑不同的应用则是出于提高集群资源利用率的角度考虑的。

http://www.jianshu.com/p/ac1840abc63f?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io

https://www.quora.com/How-do-I-learn-deep-learning-in-2-months

学习路线：

1. Ng ML课程学习完

2. DP课程以及一些tutorial

3. 找到侧重点，go deeper

4. 做一些东西

5. Doing is key to becoming an expert. Try to build something which interests you and matches your skill level. Here are a few suggestions to get you thinking:

   - As is tradition, start with classifying the [MNIST dataset](http://yann.lecun.com/exdb/mnist/)
   - Try face detection and classification on [ImageNet](http://image-net.org/index). If you are up to it, do the [ImageNet Challenge 2016](http://image-net.org/challenges/LSVRC/2016/).
   - Do a Twitter sentiment analysis using [RNNs](https://cs224d.stanford.edu/reports/YuanYe.pdf) or [CNNs](http://casa.disi.unitn.it/~moschitt/since2013/2015_SIGIR_Severyn_TwitterSentimentAnalysis.pdf)
   - Teach neural networks to reproduce the artistic style of famous painters ([A Neural Algorithm of Artistic Style](http://arxiv.org/abs/1508.06576v1))
   - [Compose Music With Recurrent Neural Networks](http://www.hexahedria.com/2015/08/03/composing-music-with-recurrent-neural-networks/)
   - [Play ping-pong using Deep Reinforcement Learning](http://karpathy.github.io/2016/05/31/rl/)
   - Use [Neural Networks to Rate a selfie](http://karpathy.github.io/2015/10/25/selfie/)
   - Automatically [color Black & White pictures using Deep Learning](https://twitter.com/ColorizeBot)

   For more inspiration, take a look at CS231n [Winter 2016](http://cs231n.stanford.edu/reports2016.html) & [Winter 2015](http://cs231n.stanford.edu/reports.html)projects. Also keep an eye on the Kaggle and HackerRank competitions for fun stuff and the opportunities to compete and learn.

## 排序算法

1. 快速排序，tips：找出基准值，采用递归方法，比基准值大的在右边，比基准值小的在左边，时间复杂度nlogn，最查的n^2

2. 归并排序，思想是合并两个有序数列

3. 堆排序？

4. 外排序：针对内存限制，排序数据不能一次全部导入到内存中，可以分批导入，将每批排序后，在小批量的读入内存中，归并排序。

   ##### 提高速度思路

   1. 增加读写接口
   2. 使用ssd替代传统硬盘
   3. 程序中采用多线程读取数据
   4. 增加cpu数量 核心等
   5. 采用分布式处理平台×

   ​


## 一致性hash算法



##概率分布模型

1. 二项分布
   1. 试验次数固定
   2. 每次概率一致
   3. 结果只有两种可能
   4. 寻找成功n次的概率
2. 几何分布
   1. 试验次数固定
   2. 每次概率一致
   3. 结果只有两种可能
   4. 寻找事件第一次成功的概率
3. 超几何分布（有放回试验）
4. 正态分布（连续概率），涉及到均值和标准差的一个轴对称模型，应用很广，如噪声等，66% 95% 99%分别对应1  2  3个标准差的概率























































