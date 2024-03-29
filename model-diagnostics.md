---
description: 课本第8章
---

# 模型诊断

> 如何检验一个拟合模型的适当性并且在需要的时候改进该模型

## 残差分析

**残差 = 实际值 - 预测值**

对于带有滑动平均项的一般ARMA模型，可以用其无穷阶自回归形式定义残差：

$$
Y_t = \pi_1 Y_{t-1} + \pi_2 Y_{t-2} + \pi_3 Y_{t-3} + \cdots + e_t \\
\Rightarrow\ \hat{e}_t = Y_t - \hat{\pi}_1 Y_{t-1}  - \hat{\pi}_2 Y_{t-2}  - \hat{\pi}_3 Y_{t-3} - \cdots
$$

如果模型被正确识别，并且参数估计充分接近真值，那么残差就应该近似具有白噪声的性质，即独立同分布的具有零均值和相等标准差的正态变量。

→ 与这些性质的偏离能够帮助发现更合适的模型

###  残差图

如果模型是适当的则期望残差图的图形是一个围绕零水平线的、无任何趋势的长方形散点图。

一些异常情况：

* 残差的变化幅度有波动
* 多个残差值超出Bonferroni边界

### 残差的正态性

正态性检验方法：

* 分位数-分位数图
* Shapiro-Wilk正态检验

### 残差的自相关

$$
\hat{r}_k\ \dot\sim\ N(0,\frac{1}{n})
$$

$$\hat{r}_k$$的95%置信区间： $$[-\Large\frac{1.96}{\sqrt{n}}\normalsize,\ \Large\frac{1.96}{\sqrt{n}}\normalsize ]$$ 

→ 在实际当中相当于检验残差的样本ACF图中是否有超过95%的竖线落在表示置信区间范围的虚线之内，有时也常用 $$\pm \Large\frac{2}{\sqrt{n}}$$来方便地表示区间范围。

### Ljung-Box检验

#### Box-Pierce

$$
Q = n(\hat{r}_1^2 + \hat{r}_2^2 + \cdots + \hat{r}_K^2)
$$

如果估计的是正确的$$\text{ARMA}(p,q)$$模型，那么对于大的$$n$$有

$$
Q\ \dot\sim\ \chi^2(K-p-q)
$$

拟合一个错误的模型将会增大$$Q$$值。

#### Ljung-Box

$$
Q_* = n(n+2)(\frac{\hat{r}_1^2}{n-1} + \frac{\hat{r}_2^2}{n-2} + \cdots + \frac{\hat{r}_K^2}{n-K})
$$

相较于Box-Pierce统计量，Ljung-Box统计量在模型拟合正确的条件下更接近卡方分布，且总有$$Q_* > Q$$，故Ljung-Box统计量更不容易忽略不恰当的模型。

**检验方法：当p值大于5%时，没有统计上显著的证据来拒绝（误差项是不相关的）原假设。**

## 过度拟合和参数冗余

### 过度拟合

**过度拟合：**拟合一个比原始模型更一般的模型，这个新模型与原始模型较为接近且原始模型是它的一个特例。

判断原始模型合适的准则：

1. 额外的参数的估计不显著地不为零
2. 共同的参数的估计与原始的估计相比没有显著的改变

一些注意事项：

1. 小心识别一个原始模型。如果一个简单模型看起来是合适的就优先对这个模型进行检验
2. 在过度拟合时不要同时增加AR和MA部分的阶数
3. 按残差分析建议的方向来扩展模型

### 参数冗余

对于一个ARMA模型，如果$$\phi(B) Y_t = \theta(B) e_t$$是正确的，那么对任意常数$$c$$，模型 $$(1-cB)\phi(B)Y_t = (1-cB)\theta(B)e_t$$也是正确的。为了使ARMA模型的参数具有唯一性，必须约掉AR和MA特征多项式的任何公因子。

