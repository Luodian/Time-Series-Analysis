# Autoregressive  integrated moving average model

黄皮书的第四章讲了AR模型，MA模型，以及两者相结合的ARMA模型，那么现在，我们进一步进化到更加广泛应用的著名的ARIMA模型。

由Box 和 jenkins 在70年代初提出的著名的时间序列预测方法，又称box-jenkins模型。

其中$ARIMA（p,d,q）$ 称为差分自回归移动平均模型，AR是自回归，p是自回归项，MA 为移动平均，q为移动平均项数，**d 为时间序列成为平稳时所做的差分次数。**

ARIMA模型：是指将非平稳时间序列转换为平稳时间序列，然后将因变量仅对它的滞后值以及随机误差项的现值和滞后值进行回归所建立的模型。

ARIMA模型的基本思想是：将预测对象随时间推移而形成的数据序列视为一个随机序列，用一定的数学模型来近似描述这个序列。这个模型一旦被识别后就可以从时间序列的过去值及现在值来预测未来值。现代统计方法、[计量经济模型](https://baike.baidu.com/item/%E8%AE%A1%E9%87%8F%E7%BB%8F%E6%B5%8E%E6%A8%A1%E5%9E%8B)在某种程度上已经能够帮助企业对未来进行预测。

**具有时变均值的任何时间序列都是非平稳的**，仅当有理由相信该确定下趋势『永远』恰当时，才认为该模型是合理 的，在商业和经济类模型中，我们认为很多具有随机趋势的序列均适用参数相对较少的模型。

```R
> quartz(width = 4.875, height = 3, pointsize = 8)
> data(oil.price)
> plot(oil.price, ylab = 'Price per Barrel', type = 'l')
```

![QQ20170803-191500@2x](../../QQ20170803-191500@2x.png)

稳定性条件：

- 恒定的平均数
- 恒定的方差
- 不随时间变化的自协方差

稳定性测试方法：

- **绘制滚动统计**：可以绘制移动平均数和移动方差，观察它是否随着时间变化。但是，这更多的是一种视觉技术。
- **DF检验**： 这是一种检查数据稳定性的统计测试。

![](http://upload-images.jianshu.io/upload_images/2080959-f073c0236f66c05f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### **消除趋势和季节性的方法：差分和分解**

**通过差分平稳化**

例如对石油价格进行对数差分：

```R
plot(diff(log(oil.price)), ylab = "Price per Barrel", type = 'l')
```

![QQ20170803-200733@2x](../../QQ20170803-200733@2x.png)

看起来差分之后的序列会更加平稳，一般来说，二次差分之后的非平稳过程基本会变得平稳。
注意如果从一个时期，到下一个时期，$Y_t​$ 趋向于存在相对稳定的百分比变化，那么我们使用对数过程会更好的拟合，使用对数差分，通常在金融中被称为收益率。

![QQ20170803-215047@2x](../../QQ20170803-215047@2x.png)

![QQ20170803-215058@2x](../../QQ20170803-215058@2x.png)

**我们可以通过使用滚动均值，滚动标准差来进行检验。**

![](http://upload-images.jianshu.io/upload_images/2080959-f073c0236f66c05f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

进行对数差分之后的检验结果。

![](http://upload-images.jianshu.io/upload_images/2080959-e985cdefb34b7716.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到平均数和标准差随着时间有小的变化。同时，DF 检验统计量小于 10% 的临界值，因此该时间序列在90% 的置信区间上是稳定的。

同样可以采取二阶或三阶差分在具体情况中获得更好的结果。

### 分解

![](http://upload-images.jianshu.io/upload_images/2080959-0567414743cbb7a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个图从上到下分别为

1. 分解之后的对数曲线
2. 趋势
3. 季节性
4. 残差

接下来我们检验残差的稳定性，得到如下的结果。

![](http://upload-images.jianshu.io/upload_images/2080959-7dc4cfe06876ab89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这基本说明，我们的数据经过分解后消除了季节性，变得逐渐稳定起来。

### ARIMA模型

如果一个时间序列 ${Y_t}$ 的 $d$ 次**差分** $W_t = \nabla^d Y_t $ 是一个**平稳**的 ARMA 过程，则称 ${Y_t}$ 为**自回归滑动平均求和**模型。

如果 $W_t$ 服从 ARMA(p,q) 模型，我们则称 $Y_t$ 是一个ARIMA(p,d,q) 过程。幸好，实际上，通常取 d = 1 或最多为 2.

下面考虑 ARIMA(p,1,q) 过程，即令 $W_t = Y_t - Y_{t - 1}$

**平稳性要求特征多项式的根绝对值  > 1**

**统计平衡**：系统最概然分布时所处的状态。

- IMA（1，1）模型代表了许多时间序列，尤其是那些在经济和商业中产生的序列，模型为 $Y_t = Y_{t-1} + e_t - \theta e_{t-1} $

  注意此模型的$Var(Y_t) $ 会随着 $t$ 的增加而不断地无限增加，同时其相关系数不断地逼近与 1.

### **幂变换**

![IMG_0603](../../../Downloads/IMG_0603.JPG)

![IMG_0604](../../../Downloads/IMG_0604.JPG)

wtf，感觉这个函数无敌了。。







