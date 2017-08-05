# 趋势

一般时间序列上的均值函数是完全任意的时间函数，平稳时间序列的均值函数一定是时域上的常数，我们如果无法获得常数的均值函数，也应当站在中间立场上去考虑相对简单的时域均值函数。

**随机游动**过程在任何时间上都有零均值。

### 时间的线性趋势和二次趋势

考虑如下的确定时间趋势：$\mu_t  = \beta_0 + \beta_1t$

~假设这中间的过程都可以由统计软件完成。

**实例**

![](http://opmza2br0.bkt.clouddn.com/17-7-30/12944562.jpg)

```R
libodeMBP:~ luodian$ R

R version 3.4.1 (2017-06-30) -- "Single Candle"
Copyright (C) 2017 The R Foundation for Statistical Computing
Platform: x86_64-apple-darwin15.6.0 (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> data(rwalk)
Warning message:
In data(rwalk) : data set ‘rwalk’ not found
> library(TSA)
Loading required package: leaps
Loading required package: locfit
locfit 1.5-9.1 	 2013-03-22
Loading required package: mgcv
Loading required package: nlme
This is mgcv 1.8-17. For overview type 'help("mgcv-package")'.
Loading required package: tseries

    ‘tseries’ version: 0.10-42

    ‘tseries’ is a package for time series analysis and computational
    finance.

    See ‘library(help="tseries")’ for details.


Attaching package: ‘TSA’

The following objects are masked from ‘package:stats’:

    acf, arima

The following object is masked from ‘package:utils’:

    tar

> data(rwalk)
> modelll = lm(rwalk~time(rwalk))
> summary(modell)
Error in summary(modell) : object 'modell' not found
> summary(modelll)

Call:
lm(formula = rwalk ~ time(rwalk))

Residuals:
     Min       1Q   Median       3Q      Max
-2.70045 -0.79782  0.06391  0.63064  2.22128

Coefficients:
             Estimate Std. Error t value Pr(>|t|)
(Intercept) -1.007888   0.297245  -3.391  0.00126 **
time(rwalk)  0.134087   0.008475  15.822  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1.137 on 58 degrees of freedom
Multiple R-squared:  0.8119,	Adjusted R-squared:  0.8086
F-statistic: 250.3 on 1 and 58 DF,  p-value: < 2.2e-16

> quartz.graph(width = 4.875, height = 2.5, pointsize = 8)
Error in quartz.graph(width = 4.875, height = 2.5, pointsize = 8) :
  could not find function "quartz.graph"
> quartz(width = 4.875, height = 2.5, pointsize = 8)
> plot(rwalk,type = 'o', ylab = 'y')
> abline(modelll)
```

![](http://opmza2br0.bkt.clouddn.com/17-7-30/69328752.jpg)



书中之后的篇幅会证明线性回归是具有较大的失误的。

### 余弦趋势

```R
R version 3.4.1 (2017-06-30) -- "Single Candle"
Copyright (C) 2017 The R Foundation for Statistical Computing
Platform: x86_64-apple-darwin15.6.0 (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> library(TSA)
Loading required package: leaps
Loading required package: locfit
locfit 1.5-9.1 	 2013-03-22
Loading required package: mgcv
Loading required package: nlme
This is mgcv 1.8-17. For overview type 'help("mgcv-package")'.
Loading required package: tseries

    ‘tseries’ version: 0.10-42

    ‘tseries’ is a package for time series analysis and computational
    finance.

    See ‘library(help="tseries")’ for details.


Attaching package: ‘TSA’

The following objects are masked from ‘package:stats’:

    acf, arima

The following object is masked from ‘package:utils’:

    tar

> har .= harmonic(tempdub,1)
Error: unexpected symbol in "har ."
> har = harmonic(tempdub,1)
Error in is.ts(x) : object 'tempdub' not found
> data(tempdub)
> month = season(tempdub)
> model2 = lm(tempdub~month,-1)
> summary(model2)

Call:
lm(formula = tempdub ~ month, data = -1)

Residuals:
    Min      1Q  Median      3Q     Max
-8.2750 -2.2479  0.1125  1.8896  9.8250

Coefficients:
               Estimate Std. Error t value Pr(>|t|)
(Intercept)      16.608      0.987  16.828  < 2e-16 ***
monthFebruary     4.042      1.396   2.896  0.00443 **
monthMarch       15.867      1.396  11.368  < 2e-16 ***
monthApril       29.917      1.396  21.434  < 2e-16 ***
monthMay         41.483      1.396  29.721  < 2e-16 ***
monthJune        50.892      1.396  36.461  < 2e-16 ***
monthJuly        55.108      1.396  39.482  < 2e-16 ***
monthAugust      52.725      1.396  37.775  < 2e-16 ***
monthSeptember   44.417      1.396  31.822  < 2e-16 ***
monthOctober     34.367      1.396  24.622  < 2e-16 ***
monthNovember    20.042      1.396  14.359  < 2e-16 ***
monthDecember     7.033      1.396   5.039 1.51e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.419 on 132 degrees of freedom
Multiple R-squared:  0.9712,	Adjusted R-squared:  0.9688
F-statistic: 405.1 on 11 and 132 DF,  p-value: < 2.2e-16

> model3 = lm(tempdub~month)
> summary(model3)

Call:
lm(formula = tempdub ~ month)

Residuals:
    Min      1Q  Median      3Q     Max
-8.2750 -2.2479  0.1125  1.8896  9.8250

Coefficients:
               Estimate Std. Error t value Pr(>|t|)
(Intercept)      16.608      0.987  16.828  < 2e-16 ***
monthFebruary     4.042      1.396   2.896  0.00443 **
monthMarch       15.867      1.396  11.368  < 2e-16 ***
monthApril       29.917      1.396  21.434  < 2e-16 ***
monthMay         41.483      1.396  29.721  < 2e-16 ***
monthJune        50.892      1.396  36.461  < 2e-16 ***
monthJuly        55.108      1.396  39.482  < 2e-16 ***
monthAugust      52.725      1.396  37.775  < 2e-16 ***
monthSeptember   44.417      1.396  31.822  < 2e-16 ***
monthOctober     34.367      1.396  24.622  < 2e-16 ***
monthNovember    20.042      1.396  14.359  < 2e-16 ***
monthDecember     7.033      1.396   5.039 1.51e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.419 on 132 degrees of freedom
Multiple R-squared:  0.9712,	Adjusted R-squared:  0.9688
F-statistic: 405.1 on 11 and 132 DF,  p-value: < 2.2e-16

> har = harmonic(tempdub,1)
> model4 = lm(tempdub~har)
> summary(model4)

Call:
lm(formula = tempdub ~ har)

Residuals:
     Min       1Q   Median       3Q      Max
-11.1580  -2.2756  -0.1457   2.3754  11.2671

Coefficients:
               Estimate Std. Error t value Pr(>|t|)
(Intercept)     46.2660     0.3088 149.816  < 2e-16 ***
harcos(2*pi*t) -26.7079     0.4367 -61.154  < 2e-16 ***
harsin(2*pi*t)  -2.1697     0.4367  -4.968 1.93e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.706 on 141 degrees of freedom
Multiple R-squared:  0.9639,	Adjusted R-squared:  0.9634
F-statistic:  1882 on 2 and 141 DF,  p-value: < 2.2e-16

> quartz(width = 4.875,height = 2.5, pointsize = 8)
> plot(ts(fitted(model4), freq = 12, start = c(1964,1)), ylab = 'Temperature', type = 'l', ylim = range(c(fitted(model4), tempdub)))
```



![](http://opmza2br0.bkt.clouddn.com/17-7-30/55707507.jpg)

### 残差分析

残差频率直方图：接近于正态分布那样，呈现某种对称性并且在高值和低值两端逐渐减小。

正态性可以通过QQ图来检验，正态分布的数据，QQ图应该近似于一条直线。

![](http://opmza2br0.bkt.clouddn.com/17-7-30/94252951.jpg)



### 回归的可靠性和有效性

> 不管我们拟合的是线性时间趋势，季节性趋势，余弦趋势还是其他趋势，普通回归都是使用最小二乘法的原则在线性模型中估计参数。

#### 样本自相关函数

定义：![](http://opmza2br0.bkt.clouddn.com/17-7-30/38854633.jpg)

