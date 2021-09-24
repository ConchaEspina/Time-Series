---
description: 课本第3章
---

# 趋势

## 常数均值的估计

$$
Y_t = \mu + X_t
$$

其中 $$E(X_t) = 0$$ 

$$\mu$$的无偏估计：

$$
\bar{Y} = \frac{1}{n} \sum_{t=1}^n Y_t
$$

 性质：（假设 $$\{Y_t\} $$ 或等价地 $$\{X_t\} $$ 是平稳时间序列）

1. $$E(\bar{Y}) = \mu$$ 
2. $$\text{Var}(\bar{Y}) =  \Large\frac{\gamma_0}{n} [\normalsize \sum\limits^{n-1}_{k=-n+1} (1 - \Large\frac{|k|}{n} \normalsize)\rho_k \Large] \normalsize =  \Large\frac{\gamma_0}{n} [\normalsize 1 + 2 \sum\limits^{n-1}_{k=1} (1 - \Large\frac{k}{n} \normalsize)\rho_k \Large]$$ 

## 回归方法

### 时间的线性趋势和二次趋势

线性趋势：

$$
\mu_t = \beta_0 + \beta_1 t
$$

估计方法：古典最小二乘

$$
\begin{align}
&Q(\beta_0, \beta_1) = \sum_{t=1}^n [Y_t - (\beta_0 + \beta_1 t)]^2 \\

\Rightarrow &\hat{\beta}_0 =  \bar{Y} - \hat{\beta}_1 \bar{t} \\
&\hat{\beta}_1 = \frac{\sum\limits_{t=1}^n (Y_t - \bar{Y})(t-\bar{t})}{\sum\limits_{t=1}^n (t-\bar{t})^2}
\end{align}
$$

{% hint style="info" %}
当且仅当确定性趋势“永远”恰当时，才能认为具有这一确定性趋势的模型是合理的，否则可能需要考虑非平稳时间序列模型等情况。
{% endhint %}

### 周期性或季节性趋势

$$
Y_t = \mu_t + X_t
$$

其对所有 $$t$$，$$E(X_t) = 0,\ \mu_t = \mu_{t-k}$$，$$|k|$$ 为周期

## 残差分析

无法观测的随机项 $$\{X_t\}$$ 可以通过残差来估计或预测：

$$
\hat{X_t} = Y_t - \hat{\mu}_t
$$

### 残差的特性

* 如果趋势模型恰当，残差应当基本上像真实随机项那样变化
* 如果随机项是白噪声，那么残差在行为上应该大致类似于均值为0、标准差为一有限正数的独立（正态）随机变量

### 残差分析步骤

一般先对残差做标准化

1. 观察残差的时间序列图 → 是否为没有明显模式的矩形散点图？
2. 观察残差与估计的相应趋势 →残差的大小与拟合的趋势值的大小有没有相关关系？
3. 观察残差的直方图/QQ图 → 残差是否具有显著正态性？
4. 游程检验 → 残差是否具有明显独立性？

{% hint style="info" %}
游程检验：残差大多位于中位数附近且倾向于随着时间“一起变动”→相邻残差正相关；残差围绕中位数上下震荡→相邻残差负相关
{% endhint %}

### 样本自相关函数

$$
r_k = \hat{\rho}_k = \frac{\Large\frac{1}{n-k}\normalsize \sum\limits_{t=k+1}^n (Y_t - \bar{Y})(Y_{t-k} - \bar{Y})}{\Large\frac{1}{n-1}\normalsize \sum\limits_{t=1}^n (Y_t - \bar{Y})^2} \approx  \frac{\sum\limits_{t=k+1}^n (Y_t - \bar{Y})(Y_{t-k} - \bar{Y})}{\sum\limits_{t=1}^n (Y_t - \bar{Y})^2}, \qquad k=1,2,\cdots
$$

#### 样本自相关函数的性质和应用

$$r_k$$ 就是 $$\rho_k$$ 的估计值

 $$r_k$$ 的近似标准误差为 $$\frac{1}{\sqrt{n}}$$ 

接受假设 “$$\rho_k = 0$$” 的判断标准： $$r_k$$ 落在 $$\pm \frac{2}{\sqrt{n}}$$ 区间之内

