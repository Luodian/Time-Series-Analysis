# 平稳时间序列模型

### 一般线性过程

定义 ${Y_t}$ 表示我们观测到的时间序列，而且此后利用 ${e_t}$ 表示未观测到的白噪声序列，即一列均值为零的独立同分布的随机变量。

一般线性过程 ${Y_t}$ 可以表示成现在和过去白噪声变量的加权线性组合：$Y_t = e_t + \Phi_1 \cdot e_{t-1} + \ldots$

>  如果表达式右边事实是个无穷级数，那么我们需要给权数 $\Phi_i < \infty $ 还应该注意到，因为${e_t}$ 是无法观测到的，所以我们假设 $\Phi_0 = 1.$

文中一个很重要的例子为 $\phi $ 是指数递减的形式：$\Phi_j = \phi^j$

对于这个例子来说：

$E(Y_t) = E(e_t + \phi e_{t-1} + \phi^2 e_{t-2} \ldots) = 0$

$Var(Y_t) = Var(e_t + \phi e_{t-1} + \phi e_{t-2} + \ldots) = Var(e_t) + \phi^2 Var(e_{t-1}) + \phi^4 Var(e_{t-2}) + \ldots =  \delta_e (1 + \phi^2 + \phi^4 + \phi^6 + \ldots) = \frac{\delta_e^2}{1 - \phi^2}$

$Cov(Y_t, Y_{t-1}) = \frac{\phi \delta_e^2}{1- \phi^2}$

$Corr(Y_t,Y_{t-1})  = \phi$

$Corr(Y_t,Y_{t-k}) = \phi^k$

如此定义的过程是**平稳过程**——自协方差结构只与时间间隔相关，而与绝对时间无关。





