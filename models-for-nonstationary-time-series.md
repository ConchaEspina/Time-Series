---
description: 课本第5章
---

# 非平稳时间序列模型

## 通过差分平稳化

一阶差分： $$\nabla Y_t = Y_t - Y_{t-1}$$ 

### 使得模型的一阶差分是平稳过程的假设

1. $$Y_t = M_t + X_t$$ 

$$M_t$$是随时间缓慢变化的序列

通过$$\beta_{0,t}$$对$$M_t$$做最小二乘估计 

2. $$Y_t = M_t + e_t,\ M_t = M_{t-1} + \epsilon_t$$ 

$$M_t$$是由随机游动支配的随机的且随着时间缓慢变化的序列，$$\{e_t\}$$和 $$\{\epsilon_t\}$$是独立的白噪声序列 

### 使得模型的二阶差分是平稳过程的假设

1. $$Y_t = M_t + X_t$$ 

通过$$\beta_{0,t} + j \beta_{1,t}$$对$$M_t$$做最小二乘估计 

2.  $$Y_t = M_t + e_t,\ M_t = M_{t-1} + W_t,\ W_t = W_{t-1} + \epsilon_t$$ 

$$M_t$$的变化率$$\nabla M_t$$在时域上是慢变的，$$\{e_t\}$$和 $$\{\epsilon_t\}$$是独立的白噪声序列。

## ARIMA模型

* 自回归滑动平均求和模型

如果一个时间序列$$\{Y_t\}$$的$$d$$次差分 $$W_t = \nabla^d Y_t$$ 是一个平稳的ARMA过程，则称$$\{Y_t\}$$为自回归滑动平均求和模型。

* $$\text{ARIMA}(p,d,q)$$ 

如果一个时间序列$$\{Y_t\}$$的$$d$$次差分 $$W_t = \nabla^d Y_t$$ 服从$$\text{ARMA}(p,q)$$模型，则称$$\{Y_t\}$$是 $$\text{ARIMA}(p,d,q)$$过程。

$$d$$一般取1或2。

### $$\text{ARIMA}(p,1,q)$$过程

令 $$W_t = \nabla Y_t = Y_t - Y_{t-1}$$，有：

$$
W_t = \phi_1 W_{t-1} + \phi_2 W_{t-2} + \cdots + \phi_p W_{t-p} + e_t - \theta_1 e_{t-1} - \theta_2 e_{t-2} - \cdots - \theta_q e_{t-q}
$$

观测序列符号表示：

$$
Y_t - Y_{t-1} = \phi_1 (Y_{t-1} - Y_{t-2}) + \phi_2 (Y_{t-2} - Y_{t-3}) + \cdots + \phi_p (Y_{t-p} - Y_{t-p-1}) \\
+ e_t - \theta_1 e_{t-1} - \theta_2 e_{t-2} - \cdots - \theta_q e_{t-q} \\
\Rightarrow\ Y_t = (1+\phi_1) Y_{t-1} + (\phi_2-\phi_1) Y_{t-2} + (\phi_3-\phi_2) Y_{t-3} + \cdots + (\phi_p-\phi_{p-1}) Y_{t-p} \\
- \phi_p Y_{t-p-1} + e_t - \theta_1 e_{t-1} - \theta_2 e_{t-2} - \cdots - \theta_q e_{t-q}
$$

#### $$\text{ARIMA}(p,1,q)$$过程的平稳性

特征方程：

$$
1 - (1+\phi_1)x - (\phi_2-\phi_1)x^2 - (\phi_3-\phi_2)x^3 - \cdots - (\phi_p-\phi_{p-1})x^p + \phi_p x^{p+1} = 0 \\ = (1-x) (1 - \phi_1x - \phi_2x^2 - \cdots - \phi_px^p)
$$

特征方程的一个根为$$x = 1$$，说明这一模型不满足平稳性要求；其余p个根为相应$$\text{ARMA}(p,q)$$模型（即平稳过程$$\nabla Y_t$$）特征方程的根

#### $$\text{ARIMA}(p,1,q)$$过程的表达式

$$
Y_t = (Y_t - Y_{t-1}) + (Y_{t-1} - Y_{t-2}) + \cdots + (Y_{-m} - Y_{-m-1}) + Y_{-m-1}\\
= W_t + W_{t-1} + \cdots + W_{-m} + Y_{-m-1} = \sum_{j=-m}^t W_j, \qquad Y_t = 0, \forall t < -m
$$

{% hint style="info" %}
相比平稳的的情况，这里用$$W_t$$或 $$W_t$$中的白噪声项来明确表示观测序列更为困难。由于非平稳过程**不在统计平衡点上**，所以**不能假设模型可向过去方向无限延伸**，或是从$$t = -\infty$$开始。但是可以且应当假设模型自某时点（例如$$t = -m$$）开始，其中$$-m$$早于$$t = 1$$，为序列的首次观测时间。为了方便，当$$t < -m$$时取 $$Y_t = 0$$。把差分方程从$$t = -m$$到$$t = t$$进行求和，就可以得到上方的表达式。
{% endhint %}

* 拓展：$$\text{ARIMA}(p,2,q)$$的情况

$$
Y_t = \sum^t_{j=-m}\sum^j_{i=-m} W_i = \sum^{t+m}_{j=0} (j+1) W_{t-j}
$$

这些表示作用有限，但是可以用来研究ARIMA模型的协方差特征

### ARIMA模型的特例

* 不包含自回归项： $$\text{IMA}(d,q)$$ 
* 不包含滑动平均项： $$\text{ARI}(p,d)$$ 

### $$\text{IMA}(1,1)$$模型

$$
Y_t = Y_{t-1} +e_t - \theta e_{t-1}
$$

#### $$\text{IMA}(1,1)$$ 模型的白噪声表达

$$
Y_t = e_t - \theta e_{t-1} + Y_{t-1} = (e_t - \theta e_{t-1}) + (e_{t-1} - \theta e_{t-2}) + Y_{t-2} \\
= \cdots = (e_t - \theta e_{t-1}) + (e_{t-1} - \theta e_{t-2}) + \cdots + (e_{-m} - \theta e_{-m-1}) \\
= e_t + (1-\theta)e_{t-1} + (1-\theta)e_{t-2} + \cdots + (1-\theta)e_{-m} - \theta e_{-m-1}
$$

白噪声项的权重不会随着滞后逐渐消失

#### $$\text{IMA}(1,1)$$模型的方差、协方差与自相关函数

* 方差

$$
\gamma_0 = [1 + (t+m)(1-\theta)^2 + \theta^2] \sigma_e^2
$$

* 协方差

$$
\gamma_k = [(1-\theta) + (t+m-k)(1-\theta)^2 + \theta^2] \sigma_e^2
$$

* 协方差

对较大的$$m$$和中等大小的$$k$$： 

$$
\rho_k = \frac{1- \theta + \theta^2 + (t+m-k) (1-\theta)^2}{\sqrt{[1 + \theta^2 + (t+m) (1-\theta)^2] [1 + \theta^2 + (t+m-k) (1-\theta)^2]}} \\ \quad \\
\approx \sqrt{\frac{t+m-k}{t+m}} \approx 1
$$

当$$t$$增加时，$$\text{Var}(Y_t)$$会**随之增加并无限增大**。同时对多个滞后期数$$k=1,2,\dots$$，$$Y_t$$和 $$Y_{t-k}$$的相关系数呈现**高度正相关**。 

### $$\text{IMA}(2,2)$$模型 

$$
\nabla^2 Y_t = e_t - \theta_1 e_{t-1} - \theta_2 e_{t-2} \\
\Rightarrow\ Y_t = 2 Y_{t-1} - Y_{t-2} + e_t - \theta_1 e_{t-1} - \theta_2 e_{t-2}
$$

#### $$\text{IMA}(2,2)$$ 模型的白噪声表达

$$
\begin{align}
Y_t &= \sum_{j=0}^{t+m} (j+1) (e_{t-j} - \theta_1 e_{t-j-1} - \theta_2 e_{t-j-2}) \\
=& e_t + \sum_{j=1}^{t+m} [1+\theta_2+(1-\theta_1-\theta_2)j] e_{t-j} - [(t+m+1)\theta_1+(t+m)\theta_2] e_{-m-1} \\
&- (t+m+1)\theta_2 e_{-m-2} \\
=& e_t + \sum_{j=1}^{t+m} [(j+1)-j\theta_1-(j-1)\theta_2] e_{t-j} - [(t+m+1)\theta_1+(t+m)\theta_2] e_{-m-1} \\
&- (t+m+1)\theta_2 e_{-m-2} \\
=& e_t + \sum_{j=1}^{t+m} \psi_j e_{t-j}  - [(t+m+1)\theta_1+(t+m)\theta_2] e_{-m-1} - (t+m+1)\theta_2 e_{-m-2}, \\
&\psi_j = 1+\theta_2+(1-\theta_1-\theta_2)j
\end{align}
$$

白噪声项的权重不会随着滞后逐渐消失，表现为$$j$$的线性函数形式。

#### $$\text{IMA}(2,2)$$模型的方差、协方差与自相关函数

当$$t$$增加时，$$\text{Var}(Y_t)$$会迅速增长，且对于所有中等大小的$$k$$值，$$Y_t$$和 $$Y_{t-k}$$的相关系数近似为1。 

$$\text{IMA}(2,2)$$模型的形状非常平滑，且走势受到逐渐增大的方差以及邻近相关系数的强正相关性影响。

### $$\text{ARI}(1,1)$$模型

$$
Y_t - Y_{t-1} =\phi (Y_{t-1} - Y_{t-2}) + e_t \\
\Rightarrow\ Y_t = (1+\phi) Y_{t-1} - \phi Y_{t-2} + e_t
$$

其中$$|\phi| < 1$$。

#### $$\text{ARI}(1,1)$$ 模型的白噪声表达

* 通用模型

如何将任意ARIMA表示成白噪音的线性组合形式 $$Y_t = e_t + \psi_1 e_{t-1} + \psi_2 e_{t-2} + \cdots$$？

借助下方等式求出各个$$\psi$$权重：

$$
(1 - \phi_1x - \cdots - \phi_px^p) (1 - x)^d (1 + \psi_1x + \cdots + \psi_{t+m}x^{t+m}) = 1 - \theta_1x - \cdots - \theta_qx^q
$$

{% hint style="info" %}
$$\phi(B) \nabla^d Y_t = \theta(B) e_t \Rightarrow \phi(B) \nabla^d [\psi(B) e_t] = \theta(B) e_t$$ 

如果不设首次观测时间$$-m$$的话，$$\psi$$系数的表达式可能不收敛。
{% endhint %}

只要令等式两边相同次幂项的系数相等，即可求出所有$$\psi$$权重。

* 特例：$$\text{ARI}(1,1)$$模型

$$
(1-\phi x)(1-x)(1+\psi_1x+\psi_2x^2+\cdots+\psi_{t+m}x^{t+m}) = 1 \\
\Rightarrow (1-\phi x)(1-x)(\cdots+\psi_{k-2}x^{k-2}+\psi_{k-1}x^{k-1}+\psi_kx^k+\cdots) = 1 \\ \quad \\
\Rightarrow \psi_k = (1+\phi)\psi_{k-1} - \phi\psi_{k-2}, \quad k \geq 2
$$

其中 $$\psi_0=1,\ \psi_1 = 1+\phi$$。

$$
\psi_k = \frac{1-\phi^{k+1}}{1-\phi}, \quad k \geq 1
$$

## ARIMA模型中的常数项

对于$$\text{ARIMA}(p,d,q)$$模型， $$\nabla^d Y_t = W_t$$是平稳的$$\text{ARMA}(p,q)$$过程，假设其具有非零常数均值$$\mu$$：

$$
W_t = \theta_0 + \phi_1 W_{t-1} + \phi_2 W_{t-2} + \cdots + \phi_p W_{t-p} + e_t - \theta_1 e_{t-1} - \theta_2 e_{t-2} - \cdots - \theta_q e_{t-q} \\ \quad \\
\Rightarrow\ \mu = \theta_0 + (\phi_1 + \phi_2 + \cdots + \phi_p) \mu \\
\Rightarrow\ \mu = \frac{\theta_0}{1 - \phi_1 - \phi_2 \ \cdots - \phi_p}
$$

### 具有非零常数均值的$$\text{IMA}(1,1)$$模型

$$
Y_t = Y_{t-1} + \theta_0 + e_t - \theta_1 e_{t-1} \\
\Rightarrow\ W_t = \theta_0 + e_t - \theta_1 e_{t-1}
$$

白噪音表达形式：

$$
Y_t = \theta_0 + e_t - \theta_1 e_{t-1} + Y_{t-1} = \theta_0 + e_t - \theta_1 e_{t-1} + (\theta_0 + e_{t-1} - \theta_1 e_{t-2} + Y_{t-2}) \\
= 2\theta_0 + e_t + (1 - \theta_1) e_{t-1} - \theta_1 e_{t-2} + (\theta_0 + e_{t-2} - \theta_1 e_{t-3} + Y_{t-3}) = \cdots \\
= (t+m+1) \theta_0 + e_t + (1 - \theta_1) e_{t-1} + \cdots + (1 - \theta_1) e_{-m} - \theta_0 e_{-m-1}
$$

与零均值的情况相比，模型多了具有斜率$$\theta_0$$的线性确定时间趋势项$$(t+m+1)\theta_0$$。

等价表达形式：

$$
Y_t = Y_t^\prime + \beta_0 + \beta_1 t
$$

其中$$Y_t^\prime$$是$$\text{IMA}(1,1)$$序列，$$E(\nabla Y_t) = \beta_1,\ E(\nabla Y_t^\prime) = 0$$。

## 其他变换

### 百分比变动和对数

适合进行对数变换的序列特征：随着序列值的增大，序列的散度即围绕某一值的波动也越大，反之亦然。

示例：

* 标准差与序列的水平值成正比（$$\sqrt{\text{Var}(Y_t)} = \mu_t \sigma$$）：对数变换后方差为常数$$\sigma^2$$ 
* 序列水平值呈指数变化：对数变换后序列呈现线性时间趋势

#### 具有相对稳定百分比变化的序列

$$
Y_t = (1+X_t) Y_{t-1} \\
\Rightarrow\ \log(Y_t) - \log(Y_{t-1}) = \log(1+X_t)
$$

若将$$X_t$$限制在一个比较小的值（如$$|X_t|<0.2$$）则可以得到一个良好的逼近：

$$
\nabla \log(Y_t) \approx X_t
$$

### 幂变换

$$
g(x) = \begin{cases}
\Large\frac{x^\lambda - 1}{\lambda}\normalsize,\quad \lambda \neq 0 \\
\log(x),\quad \lambda = 0
\end{cases}
$$

##  延迟算子

$$
B Y_t = Y_{t-1}
$$

### 延迟算子的性质

* 线性性：$$B (aY_t + bX_t + c) = aBY_t + bBX_t + c = aY_{t-1} + bX_{t-1} + c$$ 
* $$B^mY_t = Y_{t-m},\ \forall m \in N_+$$ 

### ARMA模型的延迟算子表示

#### MA模型

$$
Y_t = e_t - \theta_1 e_{t-1} - \theta_2 e_{t-2} - \cdots - \theta_q e_{t-q}  \\ = e_t - \theta_1 B e_t - \theta_2 B^2 e_t - \cdots - \theta_q B^q e_t \\ = (1 - \theta_1 B - \theta_2 B^2 - \cdots - \theta_q B^q) e_t   \\ \quad \\
\Rightarrow\ Y_t = \theta(B) e_t
$$

#### AR模型

$$
e_t = Y_t - \phi_1 Y_{t-1} - \phi_2 Y_{t-2} - \cdots - \phi_p Y_{t-p} \\ = Y_t - \phi_1 B Y_t - \phi_2 B^2 Y_t - \cdots - \phi_p B^p Y_t \\ = (1 - \phi_1 B - \phi_2 B^2 - \cdots - \phi_p B^p) Y_t \\ \quad \\
\Rightarrow\ \phi(B) Y_t = e_t
$$

#### ARMA模型

$$
Y_t = \phi_1 Y_{t-1} + \phi_2 Y_{t-2} + \cdots + \phi_p Y_{t-p} + e_t - \theta_1 e_{t-1} - \theta_2 e_{t-2} -\cdots - \theta_q e_{t-q} \\
\Rightarrow\ (1 - \phi_1 B - \phi_2 B^2 - \cdots - \phi_p B^p) Y_t = (1 - \theta_1B - \theta_2 B^2 - \cdots - \theta_q B^q) e_t \\ \quad \\
\Rightarrow\ \phi(B) Y_t = \theta(B) e_t
$$

### 差分的延迟算子表示

#### 差分方程

* 一次差分

$$
\nabla Y_t = Y_t - Y_{t-1} = (1 - B) Y_t
$$

* 二次差分

$$
\nabla^2 Y_t = \nabla Y_t - \nabla Y_{t-1} = (1-B)^2 Y_t
$$

* 差分算子的延迟算子表示

$$
\nabla^d = (1-B)^d
$$

#### ARIMA模型

$$
\begin{cases}
X_t = \nabla^d Y_t = (1-B)^d Y_t \\
\phi(B) X_t = \theta(B) e_t
\end{cases} \\ \quad \\
\Rightarrow\ \phi(B) (1-B)^d Y_t = \theta(B) e_t
$$

