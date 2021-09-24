---
description: 课本第11章
---

# 时间序列回归模型

## 干预分析

假设干预是通过改变时间序列的**均值函数或趋势**而对过程施加影响的。

$$
Y_t = m_t + N_t
$$

其中$$m_t$$代表均值函数的变化，$$N_t$$代表未受干预影响的基础时间序列，称作自然过程或无扰过程。

**预干预数据：**假设在$$T$$之前，$$m_t$$与0无异，称时间序列 $$\{Y_t,\ t<T\}$$ 为预干预数据。

### 阶梯函数与脉冲函数

阶梯函数：

$$
S_t^{(T)} = I(t \geq T) = 
\begin{cases}
1,\quad t \geq T \\
0,\quad \text{otherwise}
\end{cases}
$$

阶梯函数在预干预期间为0，干预之后为1。

脉冲函数：

$$
P_t^{(T)} = S_t^{(T)} - S_{t-1}^{(T)} =  I(t = T) = 
\begin{cases}
1,\quad t=T \\
0,\quad \text{otherwise}
\end{cases}
$$

$$P_t^{(T)}$$是干预发生时间的指示器或虚拟标志变量。

$$
P_t^{(T)} = (1-B) S_t^{(T)}  \\
S_t^{(T)} = \frac{1}{1-B} P_t^{(T)}
$$

### 偏移

即时偏移：

$$
m_t = \omega S_t^{(T)}
$$

延迟偏移：

$$
m_t = \omega S_{t-d}^{(T)}
$$

### 常见干预模型

#### 阶梯响应干预示例

$$
m_t = \delta m_{t-1} + \omega S_{t-1}^{(T)} = 
\begin{cases} 
\omega \Large\frac{1 - \delta^{t-T}}{1 - \delta}\normalsize,\quad t > T   \\
0,\quad \text{otherwise}
\end{cases}
$$

其中 $$m_0=0$$（后续示例同假设）。

* $$0<\delta<1$$：$$m_t \to \Large\frac{\omega}{1-\delta}$$ ​即均值函数的最终变化量
* $$\delta = 1$$：$$m_t = \omega(T-t), \quad t \geq T$$ 

#### 脉冲响应干预示例

只在某一时点产生影响的干预效应：

$$
m_t = \omega P_t^{(T)}
$$

逐渐消失且无延迟的干预效应：

$$
m_t = \delta m_{t-1} + \omega P_t^{(T)} = \omega \delta^{t-T} I(t \geq T)
$$

逐渐消失且有延迟的干预效应：

$$
m_t = \delta m_{t-1} + \omega P_{t-1}^{(T)} \\ \quad \\
\Rightarrow\ m_t = \delta B m_t + \omega B P_t^{(T)} = \frac{\omega B}{1 - \delta B} P_t^{(T)}
$$

#### 均值函数变化的ARMA类型表达式

$$
m_t = \frac{\omega(B)}{\delta(B)} P_t^{(T)}
$$

用阶梯虚拟变量来表示也可。

## 异常值

### 可加异常值与新息异常值

| 可加异常值（AO） | 新息异常值（IO） |
| :---: | :---: |
| 基础过程受到可叠加性的扰动 | 误差（新息）受到扰动 |
| $$Y_t^\prime = Y_t + \omega_A P_t^{(T)} \\ = \begin{cases}Y_T + \omega_A,\quad t=T \\ Y_t,\quad t \neq T \end{cases}$$  | $$e_t^\prime = e_t + \omega_I P_t^{(T)} \\ =  \begin{cases}e_T + \omega_I,\quad t=T \\ e_t,\quad t \neq T \end{cases}$$  |
| $$m_t = \omega_A P_t^{(T)}$$  | $$Y_t^\prime = Y_t + \psi_{t-T} \omega_I$$  |
| 只在时刻$$T$$的观测值受到影响 | 时刻$$T$$及其后的所有观测值都受到影响 |

### 异常值类型的识别

#### 新息异常值（IO）的识别

**残差：**

$$
a_t = Y_t^\prime - \pi_1 Y_{t-1}^\prime - \pi_2 Y_{t-2}^\prime - \cdots
$$

假设过程均值为0且所有参数已知（未知的参数用样本估计量替代）

如果序列只在时刻$$T$$有IO：

$$
a_t = \begin{cases}
a_T + \omega_I,\quad t=T \\
a_t,\quad t \neq T
\end{cases}
$$

$$\omega_I$$可用$$\tilde{\omega}_I = a_T$$来估计。

**时刻**$$T$$**上IO的检验统计量：**

$$
\lambda_{1,T} = \frac{a_T}{\sigma}\ \dot\sim\ N(0,1)
$$

**假设：**

* **零假设：**无异常值 → 此时检验统计量近似服从标准正态分布
* **备择假设：**在$$T$$时刻存在异常值

**检验方法：**Bonfferoni律

$$
\lambda_1 = \max_{1 \leq t \leq n} |\lambda_{1,t}|
$$

假设$$\lambda_{1,t}$$的最大值在$$t = T$$时取到，如果$$\lambda_1 > Z_{1-\frac{\alpha}{2n}}$$，那么认为第$$T$$个观测值必然是IO。（误判率不大于$$\alpha$$）

{% hint style="info" %}
异常值可能导致$$\sigma$$的极大似然估计值偏大，因此可以采取一种对于噪声标准差的稳健估计来代替极大似然估计。
{% endhint %}

#### 可加异常值（AO）的识别

假设过程只在时刻$$T$$上存在AO，在其他时点上无异常值。

$$
a_t = - \omega_A \pi_{t-T} + e_t = 
\begin{cases}
e_t,\quad t<T \\
\omega_A + e_T,\quad t=T \\
 - \omega_A \pi_{t-T} + e_t,\quad t>T
\end{cases}
$$

其中 $$\pi_j = \begin{cases}-1,\quad j=0 \\ 0,\quad j<0 \end{cases}$$。

\*\*\*\*$$\omega_A$$**的最小二乘估计：**

$$
\tilde{\omega}_{T,A} = - \rho^2 \sum_{t=1}^n \pi_{t-T} a_t
$$

其中$$\rho^2 = (1 + \pi_1^2 + \pi_2^2 + \cdots + \pi_{n-T}^2)^{-1},\ \text{Var}(\tilde{\omega}_{T,A}) = \rho^2\sigma^2$$ 

**时刻**$$T$$**上AO的检验统计量：**

$$
\lambda_{2,T} = \frac{\tilde{\omega}_{T,A}}{\rho\sigma} \to N(0,1)
$$

**假设：**

* **零假设：**无异常值 → 此时检验统计量渐近服从标准正态分布
* **备择假设：**在$$T$$时刻存在异常值

**检验方法：**通常$$T$$未知，需要重复对每个时间点进行检验，再次用Bonfferoni律来控制总体误差。**当在时间**$$T$$**上检验到异常值时，如果**$$|\lambda_{1,T}| > |\lambda_{2,T}|$$**，则可将观测值归类到IO，否则就是AO。**

{% hint style="info" %}
当找出一个异常之后，可将其纳入模型中，然后反复对修正的模型进行异常值检验，直到不再发现异常值为止。
{% endhint %}

## 伪相关

### 概念

互协方差函数：

$$
\gamma_{t,s}(X, Y) =  \text{Cov}(X_t, Y_s)
$$

$$X$$和$$Y$$联合弱平稳：

* 均值为常数
* 协方差$$\gamma_{t,s}(X,Y)$$是关于时差$$t-s$$的函数

对于联合平稳的过程，定义$$X$$和$$Y$$在滞后$$k$$的**互相关函数（CCF）：**

$$
\rho_k(X,Y) = \text{Corr}(X_t, Y_{t-k}) =  \text{Corr}(X_{t+k}, Y_t)
$$

互相关函数一般**不是**偶函数。

**样本互相关函数（样本CCF）：**

$$
r_k(X,Y) = \frac{\sum (X_t - \bar{X})(Y_{t-k} - \bar{Y})}{\sqrt{\sum (X_t - \bar{X})^2} \sqrt{\sum (Y_t - \bar{Y})^2}}
$$

### 回归模型1

$$
Y_t = \beta_0 + \beta_1 X_{t-d} + e_t
$$

其中所有的$$X$$是方差为$$\sigma_X^2$$的独立同分布的随机变量，所有的$$e$$是方差为$$\sigma_e^2$$的白噪声序列且独立于$$X$$。

$$
\rho_k(X,Y) = 
\begin{cases}
\Large\frac{\beta_1 \sigma_X}{\sqrt{\beta_1^2\sigma_X^2 + \sigma_e^2}}\normalsize,\quad k = -d   \\
0,\quad k\neq -d
\end{cases}
$$

如果$$X$$与$$Y$$独立，即当且仅当$$\beta_1 = 0$$时，有$$r_k(X,Y) \to N(0,\Large\frac{1}{n}\normalsize)$$。

$$|r_k(X,Y)| > \Large\frac{1.96}{\sqrt{n}}$$ → 样本互相关系数显著不为零

### 回归模型2

$$
Y_t = \beta_0 + \beta_1 X_{t-d} + Z_t
$$

其中$$Z_t$$满足某一$$\text{ARIMA}(p,d,q)$$模型。

过程$$X$$和$$Y$$中的自相关性导致样本CCF不再渐近服从于 $$N(0,\Large\frac{1}{n}\normalsize)$$ 的分布。

如果$$X$$与$$Y$$均平稳且相互独立（$$\beta_1 = 0$$），则$$\sqrt{n} r_k(X,Y)$$的方差渐近等于 $$1+2\sum\limits_{k=1}^\infty \rho_k(X)\rho_k(Y) $$ ，其中$$\rho_k(X)$$和$$\rho_k(Y)$$分别表示滞后$$k$$上$$X$$和$$Y$$的自相关系数。

假设$$X$$和$$Y$$都是$$\text{AR}(1)$$过程，其$$\text{AR}(1)$$系数分别为$$\phi_X$$和$$\phi_Y$$，那么有

$$
r_k(X,Y) \to N(0, \frac{1+\phi_X\phi_Y}{n(1-\phi_X\phi_Y)})
$$

当两个$$\text{AR}(1)$$系数都接近于1时， $$r_k(X,Y)$$的样本方差与名义量值$$\Large\frac{1}{n}$$的比率趋于无穷大。

非平稳数据的样本互相关系数方差偏大的问题更加严重，且在大样本情况下样本互相关系数的分布也不再近似于正态分布。

## 预白化与随机回归

### 概念

**预白化的目的：**将$$X$$和$$Y$$之间的线性关系从其各自的自相关关系中剥离出来

$$
\text{Var}(r_k) \to \frac{1}{n} [1+2\sum\limits_{k=1}^\infty \rho_k(X)\rho_k(Y)]
$$

$$X$$和$$Y$$当中只要至少有一个是白噪声，那么$$\text{Var}(r_k)$$就渐近等于 $$\Large\frac{1}{n}$$。 

如果$$X_t$$满足某可逆$$\text{ARIMA}(p,d,q)$$模型，就有

$$
\tilde{X}_t = (1 - \pi_1B - \pi_2B^2 - \cdots) X_t = \pi(B) X_t
$$

* $$\tilde{X}_t$$：白噪声
* $$\pi(B)$$：滤波器

通过滤波器$$\pi(B)$$将$$X$$转化为$$\tilde{X}$$的过程，称为**白化**或**预白化**。

→ 预白化是一种_线性_运算

### 预白化的应用

**应用步骤：**

1. 基于$$X$$过程生成滤波器$$\pi(B)$$，并得到$$X$$预白化后的过程$$\tilde{X}$$
2. 使用_同一_滤波器将$$Y$$预白化，得到$$\tilde{Y}$$（$$\tilde{Y}$$不必是白噪声过程，但假定是平稳的）
3. 计算$$\tilde{Y}$$和$$\tilde{X}$$的CCF

**预白化方法的优点：**

1. 运用截断的$$\Large\frac{1.96}{\sqrt{n}}$$，可以评价预白化数据样本CCF的统计显著性
2. 按照这种方法估计出的CCF所对应的理论值与某些回归系数成比例

{% hint style="info" %}
$$Y_t = \sum\limits_{j=-\infty}^\infty \beta_j X_{t-j} + Z_t\ \Rightarrow\ \tilde{Y}_t = \sum\limits_{k=-\infty}^\infty \beta_k \tilde{X}_{t-k} + \tilde{Z}_t$$ 

$$\rho_k(\tilde{X},\tilde{Y}) = \text{Corr}(\tilde{X}_{t+k}, \tilde{Y}_t) = \Large\frac{\beta_{-k} \tilde\sigma_X}{\tilde\sigma_Y} $$ 
{% endhint %}



