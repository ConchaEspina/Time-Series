---
description: 课本第7章
---

# 参数估计

> 如何估计一个识别的$$\text{ARIMA}(p,d,q)$$模型的参数

## 矩估计

令样本矩等于相应的原点矩

### 自回归模型

#### $$\text{AR}(1)$$模型

$$
\hat\phi = r_1
$$

#### $$\text{AR}(2)$$模型

$$
\begin{cases}
\rho_1 = \phi_1 + \rho_1\phi_2 \\
\rho_2 = \rho_1\phi_1 + \phi_2
\end{cases}
 \\ \quad \\
\Rightarrow\ \begin{cases}
\phi_1 = \Large\frac{\rho_1(1-\rho_2)}{1-\rho_1^2} \\
\phi_2 = \Large\frac{\rho_2-\rho_1^2}{1-\rho_1^2}
\end{cases} \\ \quad \\
\Rightarrow\ \begin{cases}
\hat{\phi}_1 = \Large\frac{r_1(1-r_2)}{1-r_1^2} \\
\hat{\phi}_2 = \Large\frac{r_2-r_1^2}{1-r_1^2}
\end{cases}
$$

#### $$\text{AR}(p)$$模型

Yule-Walker估计：

$$
\begin{cases}
\phi_1 + r_1 \phi_2 + r_2 \phi_3 + \cdots + r_{p-1} \phi_p = r_1 \\
r_1 \phi_1 + \phi_2 + r_1 \phi_3 + \cdots + r_{p-2} \phi_p = r_2 \\
\qquad \qquad \qquad \qquad \vdots \\
r_{p-1} \phi_1 + r_{p-2} \phi_2 + r_{p-3} \phi_3 + \cdots + \phi_p = r_p
\end{cases} \\ \quad \\
\Rightarrow\ \hat\phi_1, \hat\phi_2, \dots, \hat\phi_p
$$

可用[Durbin-Levinson方程](model-specification.md#yang-ben-pian-zi-xiang-guan-han-shu)求解，但如果解接近平稳域的边界，则会产生较大的舍入误差。

### 滑动平均模型

#### $$\text{MA}(1)$$模型

$$
\rho_1 = -\frac{\theta}{1+\theta^2} \\ \quad \\
\Rightarrow\ r_1 = -\frac{\hat\theta}{1+\hat\theta^2} \\ \quad \\
\Rightarrow\ \hat\theta = -\frac{1}{2r_1} \pm \sqrt{\frac{1}{4r_1^2}-1}
$$

其中可逆解为 $$\hat\theta = \Large\frac{-1+\sqrt{1-4r_1^2}}{2r_1}$$ 

{% hint style="warning" %}
当$$|r_1| > 0.5$$时无法通过矩估计法得出$$\theta$$的估计量。
{% endhint %}

#### $$\text{MA}(q)$$模型

高度非线性，通过矩估计法很难求解。

采用矩估计法估计MA模型一般效果较差。

### 混合模型

$$\text{ARMA}(1,1)$$ 模型：

$$
\rho_k = \frac{(1-\theta\phi)(\phi-\theta)}{1-2\theta\phi+\theta^2} \phi^{k-1},\quad k \geq 1 \\ \quad \\
\Rightarrow\ \phi = \frac{\rho_2}{\rho_1} \\ \quad \\
\Rightarrow\ \hat\phi = \frac{r_2}{r_1}
$$

$$
r_1 = \frac{(1-\theta\hat{\phi})(\hat{\phi}-\theta)}{1-2\theta\hat{\phi}+\theta^2} \\ \quad \\
\Rightarrow \hat{\theta}
$$

若解存在，仅保留其中的可逆解。

### 噪声方差估计

估计$$\gamma_0 = \text{Var}(Y_t)$$：

$$
s^2 = \frac{1}{n-1} \sum_{t=1}^n (Y_t - \bar{Y})^2
$$

#### $$\text{AR}(p)$$模型

$$
\gamma_0 = \text{Var}(Y_t) = \text{Cov}(Y_t,\ \phi_1 Y_{t-1} + \cdots + \phi_p Y_{t-p} + e_t) \\
= \phi_1 \gamma_1 + \cdots + \phi_p \gamma_p + \sigma_e^2 = (\phi_1 \rho_1 + \cdots + \phi_p \rho_p) \gamma_0 + \sigma_e^2 \\ \quad \\
\Rightarrow\ \sigma_e^2 = (1 - \phi_1 \rho_1 - \cdots - \phi_p \rho_p) \gamma_0 \\ \quad \\
\Rightarrow\ \hat{\sigma}_e^2 = (1 - \hat{\phi}_1 r_1 - \cdots - \hat{\phi}_p r_p) s^2
$$

对$$\text{AR}(1)$$过程： $$\hat{\sigma}_e^2 = (1 - r_1^2)s^2$$ 

#### $$\text{MA}(q)$$模型

$$
\gamma_0 = \text{Var}(Y_t) = \text{Var}(e_t - \theta_1 e_{t-1} - \cdots - \theta_q e_{t-q}) = (1 + \theta_1^2 + \cdots + \theta_q^2) \sigma_e^2 \\ \quad \\
\Rightarrow\ \hat{\sigma}_e^2 = \frac{s^2}{1 + \hat{\theta_1}^2 + \cdots + \hat{\theta}_q^2}
$$

#### $$\text{ARMA}(1,1)$$模型

$$
\gamma_0 = \text{Var}(Y_t) = \text{Var}(\phi Y_{t-1} + e_t -\theta e_{t-1}) = \phi^2 \gamma_0 + (1  - 2\phi\theta + \theta^2) \sigma_e^2 \\ \quad \\
\Rightarrow\ \sigma_e^2 = \frac{1 - \phi^2}{1  - 2\phi\theta + \theta^2} \gamma_0 \\ \quad \\
\Rightarrow\ \hat{\sigma}_e^2 = \frac{1 - 
\hat{\phi}^2}{1  - 2\hat{\phi} \hat{\theta} + \hat{\theta}^2} s^2
$$

## 最小二乘估计

### 自回归模型

#### $$\text{AR}(1)$$模型

$$
Y_t - \mu = \phi(Y_{t-1} - \mu) + e_t \\
\Rightarrow\ S_c(\phi,\mu) = \sum_{t=2}^n [(Y_t - \mu) - \phi(Y_{t-1} - \mu)]^2
$$

$$
\frac{\partial S_c}{\partial \mu} = \sum_{t=2}^n 2[(Y_t - \mu) - \phi(Y_{t-1} - \mu)] (-1+\phi) = 0 \\
\Rightarrow\ \hat\mu = \frac{1}{(n-1)(1-\phi)} [\sum_{t=2}^n Y_t - \phi \sum_{t=2}^n Y_{t-1}] \\
= \frac{1}{1-\phi} [\frac{1}{1-n} \sum_{t=2}^n Y_t - \frac{\phi}{1-n} \sum_{t=2}^n Y_{t-1}] \\
\approx \frac{1}{1-\phi} (\bar{Y} - \phi \bar{Y}) = \bar{Y} \\ \quad \\
\Rightarrow\ \hat\mu \approx \bar{Y}
$$

$$
\frac{\partial  S_c}{\partial \phi} = - \sum_{t=2}^n 2[(Y_t - \mu) - \phi(Y_{t-1} - \mu)] (Y_{t-1} - \mu) = 0 \\
\Rightarrow\ \hat\phi = \frac{\sum\limits_{t=2}^n (Y_t - \hat\mu)(Y_{t-1} - \hat\mu)}{\sum\limits_{t=2}^n (Y_{t-1} - \hat\mu)^2} \approx \frac{\sum\limits_{t=2}^n (Y_t - \bar{Y})(Y_{t-1} - \bar{Y})}{\sum\limits_{t=2}^n (Y_{t-1} - \bar{Y})^2} \approx r_1
$$

#### $$\text{AR}(p)$$模型

$$
\hat\mu = \bar{Y}
$$

$$
\begin{cases}
\phi_1 + r_1 \phi_2 + r_2 \phi_3 + \cdots + r_{p-1} \phi_p = r_1 \\
r_1 \phi_1 + \phi_2 + r_1 \phi_3 + \cdots + r_{p-2} \phi_p = r_2 \\
\qquad \qquad \qquad \qquad \vdots \\
r_{p-1} \phi_1 + r_{p-2} \phi_2 + r_{p-3} \phi_3 + \cdots + \phi_p = r_p
\end{cases} \\ \quad \\
\Rightarrow\ \hat\phi_1, \hat\phi_2, \dots, \hat\phi_p
$$

### 滑动平均模型

#### $$\text{MA}(1)$$模型

$$
Y_t = e_t - \theta e_{t-1} \\
\Rightarrow\ Y_t + \theta Y_{t-1} + \theta^2 Y_{t-2} + \cdots = e_t \\
\Rightarrow\ S_c(\theta) = \sum (e_t)^2 = \sum [Y_t + \theta Y_{t-1} + \theta^2 Y_{t-2} + \cdots]^2
$$

→ 需要使用数值优化方法求解

#### $$\text{MA}(q)$$模型

思路与$$\text{MA}(1)$$模型类似，需要用到数值优化方法。

### 混合模型

#### $$\text{ARMA}(1,1)$$模型

$$
Y_t = \phi Y_{t-1} + e_t -\theta e_{t-1} \\
\Rightarrow\ e_t = Y_t - \phi Y_{t-1} + \theta e_{t-1} \\
\Rightarrow\ S_c(\phi,\theta) = \sum_{t=2}^n e_t^2
$$

$$
\arg\limits_{\phi,\theta} \min S_c(\phi,\theta)
$$

#### $$\text{ARMA}(p,q)$$模型

$$
\ e_t = Y_t - \phi_1 Y_{t-1} - \cdots - \phi_p Y_{t-p} + \theta_1 e_{t-1} + \cdots + \theta_q e_{t-q} \\
\Rightarrow\ S_c(\phi_1, \dots, \phi_p, \theta_1, \dots, \theta_q) = \sum_{t=2}^n e_t^2
$$

$$
\arg\limits_{\phi_1, \dots, \phi_p, \theta_1, \dots, \theta_q} \min S_c(\phi_1, \dots, \phi_p, \theta_1, \dots, \theta_q)
$$

→ 利用数值算法求解

## 极大似然与无条件最小二乘

* 优点：
  * 利用了数据包含的所有信息
  * 可在一般条件下得出许多大样本结论
* 缺点：需要运用具体的联合密度函数

### 极大似然估计

#### $$\text{AR}(1)$$模型

假设：$$e_t\ \text{i.i.d} \sim N(0, \sigma_e^2)$$ 

$$
f(e_t) = (2\pi \sigma_e^2)^{-\frac{1}{2}} \exp(-\frac{e_t^2}{2\sigma_e^2}) \\
f(e_2, e_3, \cdots, e_n) = (2\pi \sigma_e^2)^{-\frac{n-1}{2}} \exp(-\frac{1}{2\sigma_e^2} \sum_{t=2}^n e_t^2)
$$

令$$Y_1 = y_1,\ Y_1 \sim N(\mu, \Large\frac{\sigma_e^2}{1-\phi^2}\normalsize)$$ 

$$
Y_t - \mu - \phi(Y_{t-1} - \mu) = e_t \\
\Rightarrow\ f(y_2, y_3, \cdots, y_n | y_1) = (2\pi \sigma_e^2)^{-\frac{n-1}{2}} \exp\huge\{\normalsize-\frac{1}{2\sigma_e^2} \sum_{t=2}^n [y_t - \mu - \phi(y_{t-1} - \mu)]^2\huge\}\normalsize \\
\Rightarrow\ f(y_1, y_2,\cdots, y_n) = (2\pi \sigma_e^2)^{-\frac{n}{2}} (1 - \phi^2)^{\frac{1}{2}} \exp\huge\{\normalsize-\frac{1}{2\sigma_e^2} \sum_{t=2}^n [y_t - \mu - \phi(y_{t-1} - \mu)]^2 \\- \frac{1-\phi^2}{2\sigma_e^2}(y_1-\mu)^2 \huge\}\normalsize = (2\pi \sigma_e^2)^{-\frac{n}{2}} (1 - \phi^2)^{\frac{1}{2}} \exp\huge[\normalsize-\frac{1}{2\sigma_e^2} S(\phi, \mu) \huge]\normalsize
$$

$$
L(\phi, \mu, \sigma_e^2) = (2\pi \sigma_e^2)^{-\frac{n}{2}} (1 - \phi^2)^{\frac{1}{2}} \exp\huge[\normalsize-\frac{1}{2\sigma_e^2} S(\phi, \mu) \huge]\normalsize \\
\ell(\phi, \mu, \sigma_e^2) = -\frac{n}{2} \log(2\pi) -\frac{n}{2} \log(\sigma_e^2) + \frac{1}{2} \log(1-\phi^2) - \frac{1}{2\sigma_e^2} S(\phi, \mu)
$$

$$
\frac{\partial\ \ell(\phi, \mu, \sigma_e^2)}{\partial \sigma_e^2} = -\frac{n}{2\sigma_e^2} + \frac{S(\phi,\mu)}{2(\sigma_e^2)^2} = 0 \\ \quad \\
\Rightarrow\ \hat{\sigma}_e^2 = \frac{S(\hat\phi, \hat\mu)}{n}
$$

#### $$S(\phi,\mu)$$与$$S_c(\phi,\mu)$$的关系

$$
S(\phi,\mu) = \sum_{t=2}^n [Y_t - \mu - \phi(Y_{t-1} - \mu)]^2 + (1-\phi^2)(Y_1-\mu)^2 \\
= S_c(\phi,\mu) + (1-\phi^2)(Y_1-\mu)^2
$$

在大样本条件下，$$S(\phi,\mu)$$和$$S_c(\phi,\mu)$$的值非常接近，使得$$S(\phi,\mu)$$或$$S_c(\phi,\mu)$$最小化的$$\phi$$和$$\mu$$值也非常接近。

### 无条件最小二乘

$$
\arg\limits_{\phi,\mu} \min S(\phi,\mu)
$$

→ 需要通过数值方法求解

## 估计的性质

极大似然和最小二乘（条件或无条件）估计量的大样本性质相同。

当$$n$$较大时，估计量**近似无偏且服从正态分布**。

下列方差和相关系数由修改后的极大似然估计理论得到。

### 自回归模型

#### $$\text{AR}(1)$$模型

$$
\hat\phi\ \dot\sim\ N(\phi, \frac{1-\phi^2}{n})
$$

当$$\phi$$趋于$$\pm 1$$时，其估计量的方差会随之变小。

#### $$\text{AR}(2)$$模型

$$
\text{Var}(\hat{\phi}_1) \approx \text{Var}(\hat{\phi}_2) \approx \frac{1-\phi_2^2}{n} \\
\text{Corr}(\hat{\phi}_1, \hat{\phi}_2) \approx -\frac{\phi_1}{1-\phi_2} = -\rho_1
$$

### 滑动平均模型

#### $$\text{MA}(1)$$模型

$$
\hat{\theta}\ \dot\sim\ N(\theta, \frac{1-\theta^2}{n})
$$

{% hint style="info" %}
通过矩估计得到的$$\hat\theta$$的方差显著大于通过极大似然估计得到的估计量的方差，因此不建议对任何带有滑动平均部分的模型采用矩估计作为参数估计方法。
{% endhint %}

#### $$\text{MA}(2)$$模型

$$
\text{Var}(\hat{\theta}_1) \approx \text{Var}(\hat{\theta}_2) \approx \frac{1-\theta_2^2}{n} \\
\text{Corr}(\hat{\theta}_1, \hat{\theta}_2) \approx -\frac{\theta_1}{1-\theta_2}
$$

### 混合模型

#### $$\text{ARMA}(1,1)$$模型

$$
\text{Var}(\hat{\phi}) \approx [\frac{1-\phi^2}{n}][\frac{1-\phi\theta}{\phi-\theta}]^2 \\
\text{Var}(\hat{\theta}) \approx [\frac{1-\theta^2}{n}][\frac{1-\phi\theta}{\phi-\theta}]^2 \\
\text{Corr}(\hat{\phi}, \hat{\theta}) \approx \frac{\sqrt{(1-\phi^2)(1-\theta^2)}}{1-\phi\theta} \\
$$

当$$\phi$$和$$\theta$$的值十分相近时，它们的估计量的可变性会非常大。

{% hint style="warning" %}
过度拟合会损伤对低阶项参数的估计，因为高阶项（冗余项）的系数实际上是0。
{% endhint %}



