---
description: 课本第2章
---

# 基本概念

## 均值、方差和协方差

对随机过程$$\{Y_t\}$$ ，有：

* 均值函数： $$\mu_t = E(Y_t)$$ 
* 自协方差函数： $$\gamma_{t,s} = \text{Cov}(Y_t, Y_s)$$ 
* 自相关函数： $$\rho_{t,s} = \text{Corr}(Y_t, Y_s) = \Large\frac{\gamma_{t,s}}{\sqrt{\gamma_{t,t} \gamma_{s,s}}}$$ 

_均值、自协方差、自相关的性质_

### 随机游动

$$e_1, e_2, \dots$$ 为均值是0，方差是 $$\sigma^2_e$$ 的独立同分布的随机变量序列

随机游动过程：

$$
\begin{cases}
\begin{align}
Y_1 &= e_1 \\
Y_2 &= e_1 +e_2 \\
&\vdots \\
Y_t &= e_1 + e_2 + \cdots + e_t
\end{align}
\end{cases}
$$

或写作

$$
Y_t = Y_{t-1} + e_t
$$

性质：

* 均值： $$\mu_t = E(Y_t) = 0$$ 
* 方差： $$\text{Var}(Y_t) = t\sigma^2_e$$ 
* 协方差： $$\gamma_{t,s} = t\sigma^2_e \quad (1\leq t \leq s)$$ 
* 自相关函数： $$\rho_{t,s} = \Large\sqrt{\frac{t}{s}} \normalsize\quad (1\leq t \leq s)$$ 

### 滑动平均

$$
Y_t = \frac{e_t + e_{t-1}}{2}
$$

性质：

* 均值： $$\mu_t = E(Y_t) = 0$$ 
* 方差与协方差： $$\gamma_{t,s} =  \begin{cases}  0.5\sigma^2_e,\quad |t-s|=0 \\ 0.25\sigma_e^2, \quad |t-s| = 1 \\ 0, \quad |t-s| >1 \end{cases}$$ 
* 自相关函数： $$\rho_{t,s} =  \begin{cases}  1,\quad |t-s|=0 \\ 0.5, \quad |t-s| = 1 \\ 0, \quad |t-s| >1 \end{cases}$$ 

## 平稳性

### 严平稳性

“分布平稳”

#### 定义

对一切时滞 $$k$$ 和时点 $$t_1, t_2, \dots, t_n$$ ，都有 $$Y_{t_1}, Y_{t_2}, \dots, Y_{t_n}$$ 与 $$Y_{t_1-k}, Y_{t_2-k}, \dots, Y_{t_n-k}$$ 的联合分布相同

#### 性质

1. 均值： $$E(Y_t) = E(Y_{t-k})$$ 
2. 方差： $$\text{Var}(Y_t) = \text{Var}(Y_{t-k}) \quad (E(X_t^2) < + \infty) $$ 
3. 协方差： $$\gamma_{t,s} = \gamma_{0, |t-s|}$$ 

记号： $$\begin{cases}  \gamma_k = \text{Cov}(Y_t, Y_{t-k}),\  \gamma_0 = \text{Var}(Y_t) \\ \rho_k = \text{Corr}(Y_t, Y_{t-k}) = \Large\frac{\gamma_k}{\gamma_0} \normalsize ,\  \rho_0 = 1 \end{cases}$$ 

### 宽平稳性

“二阶矩平稳”

#### 条件

1. 均值函数在所有时间上恒为常数
2. $$\gamma_{t,t-k} = \gamma_{0,k}$$，对所有的时间 $$t$$ 和滞后 $$k$$ 

如果过程的联合分布是多元正分布，那么严平稳与宽平稳是一致的。

### 白噪声

$$\{e_t\}$$ 为独立同分布的随机变量序列

严平稳，0均值，方差 $$\text{Var}(e_t) = \sigma^2_e$$ 

