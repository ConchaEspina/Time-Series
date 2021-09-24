---
description: 课本第10章
---

# 季节模型

## 季节ARIMA模型

### 季节$$\text{MA}(Q)$$模型

季节周期为$$s$$的$$Q$$阶季节$$\text{MA}(Q)$$模型：

$$
Y_t = e_t - \Theta_1 e_{t-s} - \Theta_2 e_{t-2s} - \cdots - \Theta_Q e_{t-Qs}
$$

季节MA特征多项式：

$$
\Theta(x) = 1 - \Theta_1 x^s - \Theta_2 x^{2s} - \cdots - \Theta_Q x^{Qs}
$$

{% hint style="info" %}
模型可逆的条件：$$\Theta(x) = 0$$所有根的绝对值都必须大于1
{% endhint %}

该序列总是平稳的，且其自相关系数只在$$0, s, 2s, \dots, Qs$$等季节滞后上非零。

季节滞后上的自相关系数：

$$
\rho_{ks} = \frac{-\Theta_k + \Theta_1\Theta_{k+1} + \Theta_2\Theta_{k+2} + \cdots + \Theta_{Q-k}\Theta_Q}{1 + \Theta_1^2 + \Theta_2^2 + \cdots + \Theta_Q^2}
$$

季节$$\text{MA}(Q)$$模型可以看作阶数$$q = Qs$$的非季节性MA模型的特例，但除了季节滞后$$s,2s,\dots,Qs$$处，所有$$\theta$$值都取零。

### 季节$$\text{AR}(P)$$模型

季节周期为$$s$$的$$P$$阶季节$$\text{AR}(P)$$模型：

$$
Y_t = \Phi_1 Y_{t-s} + \Phi_2 Y_{t-2s} + \cdots + \Phi_P Y_{t-Ps} + e_t
$$

季节AR特征多项式：

$$
\Phi(x) = 1 - \Phi_1 x^s - \Phi_2 x^{2s} - \cdots - \Phi_P x^{Ps}
$$

{% hint style="info" %}
模型平稳的条件：$$\Phi(x) = 0$$所有根的绝对值都必须大于1
{% endhint %}

该序列的自相关系数只在$$0, s, 2s, \dots, Ps$$等季节滞后上非零。

季节滞后上的自相关系数：

$$
\rho_{ks} = \Phi^k,\quad k=1,2,\cdots
$$

季节$$\text{AR}(P)$$模型可以看作阶数$$p = Ps$$的非季节性AR模型的特例，但除了季节滞后$$s,2s,\dots,Ps$$处，所有$$\phi$$值都取零。

## 乘法季节ARMA模型

季节周期为$$s$$的乘法季节$$\text{ARMA}(p,q)\times(P,Q)_s$$模型是AR特征多项式为$$\phi(x)\Phi(x)$$、MA特征多项式为$$\theta(x)\Theta(x)$$的模型，其中：

$$
\begin{cases}
\phi(x) = 1 - \phi_1 x - \phi_2 x^2 - \cdots - \phi_p x^p \\
\Phi(x) = 1 - \Phi_1 x^s - \Phi_2 x^{2s} - \cdots - \Phi_P x^{Ps}
\end{cases} \\ \quad \\
\begin{cases}
\theta(x) = 1 - \theta_1 x - \theta_2 x^2 - \cdots - \theta_q x^q \\
\Theta(x) = 1 - \Theta_1 x^s - \Theta_2 x^{2s} - \cdots - \Theta_Q x^{Qs}
\end{cases}
$$

模型中可能也会包含常数项$$\theta_0$$ 

#### 例子：$$q = P = 1,\ p = Q = 0,\ s = 12$$ 

$$
Y_t = \Phi Y_{t-12} + e_t - \theta e_{t-1}
$$

$$
\gamma_0 = \frac{1+\theta^2}{1-\Phi^2} \sigma_e^2 \\
\rho_{12k} = \Phi^k,\quad k \geq 1 \\
\rho_{12k-1} = \rho_{12k+1} = -\frac{\theta}{1+\theta^2}\Phi^k,\quad k = 0,1,\cdots
$$

## 非平稳季节ARIMA模型

时间序列周期为$$s$$的季节差分：

$$
\nabla_s Y_t = Y_t - Y_{t-s}
$$

季节周期为$$s$$，非季节（规则的）阶数为$$p,d,q$$，季节阶数为$$P,D,Q$$的乘法季节$$\text{ARIMA}(p,d,q)\times(P,D,Q)_s$$模型要求差分序列

$$
W_t = \nabla^d \nabla_s^D Y_t
$$

满足某季节周期为$$s$$的乘法季节$$\text{ARMA}(p,q)\times(P,Q)_s$$模型。

## 模型识别、拟合和检验

### 模型识别

观察样本ACF图在季节滞后处是否存在强自相关关系

### 模型拟合

直接应第7章的参数估计方法即可

### 诊断性检验

#### 残差的相关性

* 绘制标准残差图，观察残差的模式
* 绘制残差的ACF图，观察残差的自相关性
* Ljung-Box检验

#### 残差的正态性

* 通过残差的直方图或QQ图检验残差的正态性
* 通过Shapiro-Wilk检验判断残差是否服从正态分布

## 季节模型预测

对模型递归地应用差分方程形式

### 季节$$\text{AR}(1)_{12}$$模型

$$
Y_t = \Phi Y_{t-12} + e_t
$$

$$
\hat{Y}_t(\ell) = \Phi \hat{Y}_t(\ell-12) = \Phi^{k+1} Y_{t+r-11}
$$

其中 $$\ell = 12k + r + 1, 0 \leq r < 12, k = 0, 1, \dots$$ 

{% hint style="info" %}
注意$$k$$是$$\Large\frac{\ell-1}{12}$$的整数部分，$$\Large\frac{r}{12}$$是$$\Large\frac{\ell-1}{12}$$的小数部分。
{% endhint %}

$$
\begin{align}
e_t(\ell) &= Y_{t+\ell} - \hat{Y}_t(\ell) \\
&= [e_{t+\ell} + \Phi e_{t+\ell-12} + \cdots + \Phi^k 
e_{t+\ell-12k} + \Phi^{k+1} Y_{t+\ell-12(k+1)}] - \hat{Y}_t(\ell) \\
&= e_{t+\ell} + \Phi e_{t+\ell-12} + \cdots + \Phi^k e_{t+\ell-12k} \\
\Rightarrow\ &\text{Var}(e_t(\ell)) = (1+ \Phi^2 + \cdots + \Phi^{2k}) \sigma_e^2 = \frac{1-\Phi^{2k+2}}{1-\Phi^2} \sigma_e^2
\end{align}
$$

### 季节$$\text{MA}(1)_{12}$$模型

$$
Y_t = e_t - \Theta e_{t-12} + \theta_0
$$

$$
\begin{cases}
\hat{Y}_t(1) = -\Theta e_{t-11} + \theta_0 \\
\hat{Y}_t(2) = -\Theta e_{t-10} + \theta_0 \\
\qquad \qquad \vdots \\
\hat{Y}_t(12) = -\Theta e_t + \theta_0
\end{cases}
$$

$$
\hat{Y}_t(\ell) = \theta_0,\quad \ell >12
$$

$$
e_t(\ell) = Y_t(\ell) - \hat{Y}_t(\ell) = \begin{cases}
e_{t+\ell},\quad \ell =1, \cdots, 12 \\
e_{t+\ell} - \Theta e_{t+\ell-12},\quad \ell > 12
\end{cases} \\ \quad \\
\Rightarrow\ \text{Var}(e_t(\ell)) = \begin{cases}
\sigma_e^2,\quad \ell =1, \cdots, 12 \\
(1+\Theta^2) \sigma_e^2,\quad \ell > 12
\end{cases}
$$

### $$\text{ARIMA}(0,0,0)\times(0,1,1)_{12}$$模型

$$
\nabla_{12} Y_t = Y_t - Y_{t-12} = e_t - \Theta e_{t-12} \\
\Rightarrow\ Y_{t+\ell} = Y_{t+\ell-12} + e_{t+\ell} - \Theta e_{t+\ell-12}
$$

$$
\begin{cases}
\hat{Y}_t(1) = Y_{t-11} - \Theta e_{t-11} \\
\hat{Y}_t(2) = Y_{t-10} - \Theta e_{t-10} \\
\qquad \qquad \vdots \\
\hat{Y}_t(12) = Y_t - \Theta e_t
\end{cases}
$$

$$
\hat{Y}_t(\ell) = \hat{Y}_t(\ell-12),\quad \ell > 12
$$

$$
Y_t = (1-\Theta)(Y_{t-12} + \Theta Y_{t-24} + \Theta^2 Y_{t-36} + \cdots) + e_t \\
\Rightarrow\ \hat{Y}_t(\ell) = (1-\Theta)[\hat{Y}_t(\ell-12) + \Theta \hat{Y}_t(\ell-24) + \Theta^2 \hat{Y}_t(\ell-36) + \cdots]
$$

令$$k$$为$$\Large\frac{\ell-1}{12}$$的整数部分，则有

$$
\hat{Y}_t(\ell) = (1-\Theta)[\hat{Y}_t(\ell-12) + \Theta \hat{Y}_t(\ell-24) + \cdots + \Theta^k \hat{Y}_t(\ell-12k)] \\
+ (1-\Theta)[\Theta^{k+1}Y_{t+\ell-12(k+1)}+\Theta^{k+2}Y_{t+\ell-12(k+2)}+\cdots] \\ \quad \\
\Rightarrow\ e_t(\ell) = (1-\Theta)[e_t(\ell-12) + \Theta e_t(\ell-24) + \cdots + \Theta^k e_t(\ell-12k)] \\
= e_{t+\ell} + (1-\Theta) e_{t+\ell-12} + \cdots + (1-\Theta) e_{t+\ell-12k}
$$

$$
\text{Var}(e_t(\ell)) = [1+k(1-\Theta)^2] \sigma_e^2
$$

### $$\text{ARIMA}(0,1,1)\times(0,1,1)_{12}$$模型

$$
\nabla \nabla_{12} Y_t = (e_t - \theta e_{t-1}) - \Theta(e_{t-12} - \theta e_{t-13}) \\
\Rightarrow\ Y_t = Y_{t-1} + Y_{t-12} - Y_{t-13} + e_t - \theta e_{t-1} - \Theta e_{t-12} + \theta\Theta e_{t-13}
$$

$$
\hat{Y}_t(\ell) = A_1 + A_2 \ell + \sum_{j=0}^6 [B_{1j} \cos(\frac{2\pi j\ell}{12}) + B_{2j} \sin(\frac{2\pi j\ell}{12})]
$$

### 预测极限

同

与普通ARIMA模型的预测极限求解方法类似。

