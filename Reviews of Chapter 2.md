# 时间序列模型的基本概念

### 时间序列与随机过程

随机变量序列：$Y_t:t = 0, \pm1,\pm2,\ldots$称为一个**随机过程**。

自协方差函数：$\gamma_{t,s} = Cov(Y_t,Y_s)，t,s = 0,\pm1,\pm2,\ldots$

> t,s 表明了自协方差函数（时间序列上的前后相关），描述一个变量 X(t) 在不同时刻t1,t2的取值之间的二阶混合中心矩（协方差）。

### 随机游动

定义：

![](http://opmza2br0.bkt.clouddn.com/17-7-30/24230031.jpg)

![](http://opmza2br0.bkt.clouddn.com/17-7-30/49486098.jpg)

结论~

> 随着时间的推移，相邻时点上Y值的正相关程度越来越强？为啥啊

而另一方面，对时点相距遥远的Y值，其相关程度越来越弱。

思考：对于某时期突然离群的点该如何考虑？

### 滑动平均

定义：

![](http://opmza2br0.bkt.clouddn.com/17-7-30/81108359.jpg)

![](http://opmza2br0.bkt.clouddn.com/17-7-30/2745660.jpg)

注意$\rho_{2,1} = \rho_{3,2} = \rho_{4,3} = \rho_{9,8} = 0.5$，无论何时出现，间隔一个单位时间的$Y$值之间具有完全相同的相关系数，此外，对于更一般的情形，对所有的 $t$ ，都有$\rho_{t,t-k}$相等。

**平稳性**

弱平稳（二阶矩平稳）定义：

1. 均值函数在所有时间上恒为常数。
2. $\gamma_{t,t-k} = \gamma_{0,k}$，对所有的时间$t$和滞后$k$。

**随机余弦波**

对于如下定义的一个过程，$Y_t = \cos[2\pi(\frac{t}{12} + \Phi)]\ \ t = 0,\pm1,\pm2$。

![](http://opmza2br0.bkt.clouddn.com/17-7-30/33109792.jpg)

这个例子表示，对于给定的时间序列，从图像上去判定平稳性并不是一个好的决策。

> 但是我觉得对于重复图像，不是应该更倾向于判定它具有平稳性吗？



