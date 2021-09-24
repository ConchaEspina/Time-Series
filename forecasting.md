---
description: 课本第9章
---

# 预测

## 条件期望

$$Y$$的条件概率密度函数：

$$
f_{Y|X}(y|x) = \frac{f_{X,Y}(x,y)}{f_X(x)}
$$

$$Y$$的条件期望：

$$
E(Y|X=x) = \int_{\mathbb{R}} y\ f_{Y|X}(y|x)\ \ dy
$$

### 条件数学期望的性质

$$
E(aY+bZ+c | X=x) = aE(Y|X=x) + bE(Z|X=x) + c
$$

$$
E[h(Y)|X=x] = \int_\mathbb{R} h(y) f_{Y|X}(y|x)\ dy \\
\Rightarrow\ E[h(X)|X=x)]= h(x) \\
\Rightarrow\ E[h(X,Y)|X=x] = E[h(x,Y)|X=x]
$$

如果$$X$$和$$Y$$是独立的，则有：

$$
E(Y|X) = E(Y)
$$

#### 全期望公式

* 定理

$$
E(E(Y|X)g(X)) = E(Yg(X))
$$

证明如下：

$$
\text{LHS} = \int_\mathbb{R} \huge(\normalsize \int_\mathbb{R} y\ f_{Y|X}(y|x)\ \ dy\huge)\normalsize g(x) f_X(x)\ dx \\
= \int_\mathbb{R} \int_\mathbb{R} y\ g(x)\ f_{X,Y}(x,y)\ dy dx = \text{RHS}
$$

{% hint style="info" %}
若对$$\forall g(X),\ E(h(X)g(X)) = E(Yg(X))$$都成立，则必有$$h(X) = E(Y|X)$$。
{% endhint %}

* 全期望公式

$$
E[E(Y|X)] = E(Y)
$$

令上方定理$$g(X) = 1$$即可。

* 推论

$$
E(g(X)Y|X) = g(X) E(Y|X)
$$

证明如下：

$$
E[h(X)f(X)] = E[[g(X)E(Y|X)]f(X)] = E[E(Y|X) \tilde{f}(X)] \\
= E[Y \tilde{f}(X)] = E[[Yg(X)]f(X)]  = E[\tilde{Y}f(X)], \quad \forall\ f(X) \\ \quad \\
\Rightarrow\ g(X)E(Y|X) = h(X) = E(\tilde{Y}|X) = E(g(X)Y|X)
$$

## 最小均方误差预测

找到$$X$$的一个线性函数$$h(X)$$，使得$$E[Y-h(X)]^2$$达到最小。

$$
\begin{align}
&E[Y-h(X)]^2 = E[E\{[Y-h(X)]^2|X\}] \\ \\
\Rightarrow\ &E\{[Y-h(X)]^2|X\} = E\{[Y-h(x)]^2|X=x\} \\
&= E[Y^2|X=x] - 2h(x) E[Y|X=x] + [h(x)]^2 \\
&= \{h(x) - E[Y|X=x]\}^2 + E[Y^2|X=x] - \{E[Y|X=x]\}^2 \\ \\
\Rightarrow\ &\hat{h}(x) = E[Y|X=x]
\end{align}
$$

在任意$$X = x$$给定的条件下均可得到上述结果，故有

$$
h(X) = E[Y|X]
$$

是基于所有关于$$X$$的函数所得的$$Y$$的最优预测。

更一般地，如果用一个$$X_1, X_2, \cdots, X_n$$的函数来预测$$Y$$，则易证最小均方误差预测为

$$
E(Y|X_1, X_2, \cdots, X_n)
$$

### 时间序列的最小均方误差预测

基于时间序列可获得的历史数据$$Y_1, Y_2, \cdots, Y_t$$，预测未来$$\ell$$期的值$$Y_{t+l}$$，称时间$$t$$为预测起点，$$\ell$$为预测前置时间，得到最小均方误差预测值：

$$
\hat{Y}_t(\ell) = E(Y_{t+\ell}|Y_1, Y_2, \cdots, Y_t)
$$

## 确定性趋势

$$
Y_t = \mu_t + X_t
$$

其中$$X_t$$是均值为0、方差为$$\gamma_0$$的白噪声。

$$
\hat{Y}_t(\ell) = E(\mu_{t+\ell} + X_{t+\ell}\ |\ Y_1, \cdots, Y_t) \\
= E(\mu_{t+\ell}\ |\ Y_1, \cdots, Y_t) + E(X_{t+\ell}\ |\ Y_1, \cdots, Y_t) \\
= \mu_{t+\ell} + E(X_{t+\ell}) = \mu_{t+\ell}
$$

### 趋势示例

#### 线性趋势

$$
\hat{Y}_t(\ell) = \mu_{t+\ell} = \beta_0 + \beta_1 (t+\ell)
$$

#### 季节模型

$$
\hat{Y}_t(\ell) = \mu_{t+\ell} = \mu_{t+s+\ell} = \hat{Y}_t(\ell+s)
$$

### 预测误差

$$
e_t(\ell) = Y_{t+\ell} - \hat{Y}_t(\ell) = \mu_{t+\ell} + X_{t+\ell} - \mu_{t+\ell} = X_{t+\ell}
$$

#### 预测误差的性质

* 期望：$$E(e_t(\ell)) = 0$$ → $$\hat{Y}_t(\ell)$$无偏
* 方差： $$\text{Var}(e_t(\ell)) = \gamma_0$$ 

## ARIMA预测

### $$\text{AR}(1)$$ 

#### $$\ell = 1$$ 

$$
Y_{t+1} - \mu = \phi(Y_t - \mu) + e_{t+1}
$$

对两边取条件期望，得：

$$
\hat{Y}_t(1) - \mu = \phi [E(Y_t | Y_1, \cdots, Y_t) - \mu] + E(e_{t+1} | Y_1, \cdots, Y_t) \\
= \phi (Y_t - \mu) + E(e_{t+1}) = \phi (Y_t - \mu) \\ \quad \\
\Rightarrow\ \hat{Y}_t(1) = \phi(Y_t - \mu) + \mu
$$

预测误差：

$$
e_t(1)  = Y_{t+1} - \hat{Y}_t(1) = Y_{t+1} - \mu - \phi(Y_t - \mu) = e_{t+1} \\ \quad \\
\Rightarrow\ E(e_t(1)) = 0,\ \text{Var}(e_t(1)) = \sigma_e^2
$$

#### 一般情况

$$
\hat{Y}_t(\ell) = \phi[E(Y_{t+\ell-1} | Y_1, \cdots, Y_t) - \mu] + E(e_{t+\ell-1} | Y_1, \cdots, Y_t) + \mu \\
= \phi [\hat{Y}_t(\ell-1) - \mu] + \mu = \phi^2 [\hat{Y}_t(\ell-2) - \mu] + \mu \\
= \cdots = \phi^{\ell-1} [\hat{Y}_t(1) - \mu] + \mu = \phi^\ell (Y_t - \mu) + \mu
$$

因为$$|\phi|<1$$，所以当$$\ell$$很大的时候，有$$\hat{Y}_t(\ell) \approx \mu$$。

预测误差：

$$
Y_t = \mu + e_t + \phi e_{t-1} + \phi^2 e_{t-2} + \cdots
$$

$$
\begin{align}
e_t(\ell) &= Y_{t+\ell} - \hat{Y}_t(\ell) = Y_{t+\ell} - \mu - \phi^\ell(Y_t - \mu) \\
&= (e_{t+\ell} + \phi e_{t+\ell-1} + \phi^2 e_{t+\ell-2} + \cdots) - \phi^\ell (e_t + \phi e_{t-1} + \phi^2 e_{t-2} + \cdots)  \\
&= e_{t+\ell} + \phi e_{t+\ell-1} + \phi^2 e_{t+\ell-2} + \cdots + \phi^{\ell-1} e_{t+1}\\
\Rightarrow\ &E(e_t(\ell)) = 0,\ \text{Var}(e_t(\ell)) = \frac{1-\phi^{2\ell}}{1-\phi^2} \sigma_e^2
\end{align}
$$

 由上面的结果可以推出如下结论：

* $$\hat{Y}_t(\ell)$$是无偏估计
* 当$$\ell$$很大时，有$$\text{Var}(e_t(\ell)) \approx \Large\frac{1}{1-\phi^2} \normalsize\sigma_e^2$$或 $$e_t(\ell) \approx Y_{t+\ell} \Rightarrow \text{Var}(e_t(\ell)) \approx \text{Var}(Y_{t+\ell}) = \gamma_0 = \Large\frac{1}{1-\phi^2} \normalsize\sigma_e^2$$ 

### $$\text{MA}(1)$$ 

#### $$\ell = 1$$ 

$$
Y_{t+1} = \mu + e_{t+1} - \theta e_t
$$

对两边取条件期望，得：

$$
\hat{Y}_t(1) = \mu + E(e_{t+1} | Y_1, \cdots, Y_t)- \theta E(e_t | Y_1, \cdots, Y_t) = \mu - \theta e_t
$$

其中 $$E(e_{t+1} | Y_1, \cdots, Y_t) = E(e_{t+1}) = 0,\ E(e_t | Y_1, \cdots, Y_t) = e_t$$（由可逆性得$$e_t$$为 $$Y_1, \cdots, Y_t$$ 的函数）

预测误差：

$$
e_t(1) = Y_{t+1} - \hat{Y}_t(1) = e_{t+1}
$$

#### 一般情况

$$
\hat{Y}_t(\ell) = \mu + E(e_{t+\ell} | Y_1, \cdots, Y_t) - \theta E(e_{t+\ell-1} | Y_1, \cdots, Y_t) \\
= \mu + E(e_{t+\ell}) - \theta E(e_{t+\ell-1}) = \mu, \qquad \ell>1
$$

### $$\text{ARMA}(p,q)$$

$$
\begin{align}
\hat{Y}_t(\ell) &= E(Y_{t+\ell} | Y_1, \cdots, Y_t) \\
&= \phi_1 \hat{Y}_t(\ell-1) + \phi_2 \hat{Y}_t(\ell-2) + \cdots + \phi_p\hat{Y}_t(\ell-p) + \theta_0 - \theta_1 E(e_{t+\ell-1} | Y_1, \cdots, Y_t) \\
&- \theta_2 E(e_{t+\ell-2} | Y_1, \cdots, Y_t) - \cdots - \theta_q E(e_{t+\ell-q} | Y_1, \cdots, Y_t) \\
&= \sum_{i=1}^p \phi_i \hat{Y}_t(\ell-i) + \theta_0 - \sum_{j=1}^q \theta_j E(e_{t+\ell-j} | Y_1, \cdots, Y_t)
\end{align}
$$

其中 $$E(e_{t+j} | Y_1, \cdots, Y_t) = \begin{cases} 0, \quad j >0 \\ e_{t+j}, \quad j \leq 0 \end{cases},\quad \hat{Y}_t(j) = \begin{cases} \hat{Y}_t(j), \quad j>0 \\ Y_{t+j},\quad j \leq 0   \end{cases}$$ 

#### 特例：$$\text{ARMA}(1,1)$$

$$
\hat{Y}_t(1) = \phi Y_t + \theta_0 - \theta e_t \\
\hat{Y}_t(2) = \phi \hat{Y}_t(1) + \theta_0 \\ \quad \\
\Rightarrow\ \hat{Y}_t(\ell) = \phi \hat{Y}_t(\ell-1) + \theta_0, \quad l \geq 2
$$

$$
\begin{align}
&\hat{Y}_t(\ell) - \frac{\theta_0}{1-\phi} = \phi [\hat{Y}_t(\ell-1) - \frac{\theta_0}{1-\phi}] \\
\Rightarrow\ &\hat{Y}_t(\ell) - \mu = \phi [\hat{Y}_t(\ell-1) - \mu] = \phi^{\ell-1} [\hat{Y}_t(1) - \mu] \\
&= \phi^\ell (Y_t - \mu) - \phi^{\ell-1} e_t,\quad \ell \geq1 
\end{align}
$$

### 截断线性过程

$$
Y_{t+\ell} = C_t(\ell) + I_t(\ell),\quad \ell > 1
$$

其中$$C_t(\ell)$$包括常数项以及 $$Y_1, \dots, Y_t, e_1, \dots, e_t$$，$$I_t(\ell)$$包括 $$e_{t+1}, \dots, e_{t+\ell}$$

$$
\begin{cases}
E[C_t(\ell) | Y_1, \cdots, Y_t] = C_t(\ell) \\
E[I_t(\ell) | Y_1, \cdots, Y_t] = 0
\end{cases} \\ \quad \\
\Rightarrow\ \hat{Y}_t(\ell) = E[Y_{t+\ell} | Y_1, \cdots, Y_t] = C_t(\ell), \\
e_t(\ell) = Y_{t+\ell} - \hat{Y}_t(\ell) = I_t(\ell)
$$

#### $$C_t(\ell), I_t(\ell)$$的求解方法

$$
\begin{align}
&Y_{t+\ell} = \phi_1 Y_{t+\ell-1} + \cdots + \phi_p Y_{t+\ell-p} + e_{t+\ell} - \theta_1 e_{t+\ell-1} - \cdots - \theta_q e_{t+\ell-q} + \theta_0 \\ \quad \\
\Rightarrow\ &\phi(B)Y_{t+\ell} = \theta(B)e_{t+\ell} + \theta_0  \\ \quad \\
\Rightarrow\ &C_t(\ell) + I_t(\ell) = \phi_1 [C_t(\ell-1) + I_t(\ell-1)] + \cdots + \phi_p [C_t(\ell-p) + I_t(\ell-p)] \\ 
&+ e_{t+\ell} - \theta_1 e_{t+\ell-1} - \cdots - \theta_q e_{t+\ell-q} + \theta_0 \\ \quad \\
\Rightarrow\ &C_t(\ell) = \phi_1 C_t(\ell-1) + \cdots + \phi_p C_t(\ell-p)  - \theta_\ell e_t - \cdots - \theta_q e_{t+\ell-q} + \theta_0 \\
&I_t(\ell) = \phi_1 I_t(\ell-1) + \cdots + \phi_p I_t(\ell-p) + e_{t+\ell} - \theta_1 e_{t+\ell-1} - \cdots - \theta_{\ell-1} e_{t+1}
\end{align}
$$

其中当$$\tilde\ell \leq 0$$时，$$C_t(\tilde\ell) = Y_{t+\tilde\ell}, I_t(\tilde\ell) = 0$$。

* $$I_t(\ell)$$的通用形式

令 $$e_s^\prime = \begin{cases}e_s,\quad s>t \\ 0,\quad  s \leq t \end{cases}$$ 

$$
I_t(\ell) = \phi_1 I_t(\ell-1) + \cdots + \phi_p I_t(\ell-p) + e_{t+\ell}^\prime - \theta_1 e_{t+\ell-1}^\prime - \cdots - \theta_q e_{t+\ell-q}^\prime \\
\Rightarrow\ \phi(B) I_t(\ell) = \theta(B) e_{t+\ell}^\prime \\
\Rightarrow\ I_t(\ell) = \frac{\theta(B)}{\phi(B)} e_{t+\ell}^\prime = \psi(B) e_{t+\ell}^\prime = e_{t+\ell} + \psi_1 e_{t+\ell-1} + \cdots + \psi_{\ell-1} e_{t+1}
$$

$$
E[e_t(\ell)] = E[I_t(\ell)] = 0 \\
\text{Var}[e_t(\ell)] = \sigma_e^2 \sum_{j=0}^{\ell-1} \psi_j^2 \Rightarrow \lim_{\ell \to \infty} \text{Var}[e_t(\ell)] = \sigma_e^2 \sum_{j=0}^{\infty} \psi_j^2 \approx \gamma_0
$$

* $$\ell > q$$条件下$$C_t(\ell)$$的形式

$$
C_t(\ell) = \phi_1 C_t(\ell-1) + \cdots + \phi_p C_t(\ell-p) + \theta_0 \\
\Rightarrow\ \phi(B) C_t(\ell) = \theta_0
$$

当$$\phi_1 + \cdots + \phi_p \neq 1$$时：

$$
\phi(B) C = (1-\phi_1-\cdots-\phi_p)C = \theta_0 \\
\Rightarrow\ C = \frac{\theta_0}{1-\phi_1-\cdots-\phi_p}
$$

此时$$C_t(\ell)$$为常数

当$$\phi_1 + \cdots + \phi_p = 1$$时：

$$
\begin{align}
\phi(B) \ell &= (1-\phi_1 B- \cdots - \phi_p B^p) \ell \\
&= \ell - \phi_1 (\ell-1) - \cdots - \phi_p (\ell-p) \\
&= \phi_1 + 2\phi_2 + \cdots + p\phi_p
\end{align} \\
\Rightarrow\ \phi(B) \frac{\theta_0 \ell}{\phi_1 + 2\phi_2 + \cdots + p\phi_p} = \theta_0 = \phi(B) C_t(\ell)
$$

此时$$C_t(\ell)$$为确定性趋势

差分情况下：

$$
\begin{align}
(1-B)^d \ell^d &= (1-B)^{d-1} [\ell^d - (\ell-1)^d] \\
&= (1-B)^{d-1} (d \ell^{d-1} + \cdots) = d (1-B)^{d-1} \ell^{d-1} \\
&= d(d-1) \cdots 1 = d!
\end{align} \\ \quad \\
\Rightarrow\ (1-B)^d \frac{\theta_0 \ell^d}{d!} = \theta_0 = (1-B)^d C_t(\ell)
$$

### 非平稳模型

$$\text{ARIMA}(p,1,q)$$模型可以写成一个$$\text{ARMA}(p+1,q)$$模型的形式

$$
\nabla Y_t = \phi_1 \nabla Y_{t-1} + \cdots + \phi_p \nabla Y_{t-p} + e_t - \theta_1 e_{t-1} - \cdots - \theta_qe_{t-q} \\ \quad \\
\Rightarrow\ Y_t = \varphi_1 Y_{t-1} + \varphi_2 Y_{t-2} + \cdots + \varphi_p Y_{t-p} \\
+ \varphi_{p+1} Y_{t-p-1} + e_t - \theta_1 e_{t-1} - \cdots - \theta_qe_{t-q}
$$

其中 $$\varphi_1 = 1 + \phi_1,\ \varphi_j = \phi_j - \phi_{j-1}\ (j=1,\dots,p), \varphi_{p+1} = -\phi_p$$ 

类似地，$$\text{ARIMA}(p,d,q)$$模型可以写成一个$$\text{ARMA}(p+d,q)$$模型的形式，有$$p+d$$个$$\varphi$$系数，之后采用ARMA模型的预测方法即可。

#### 特例：$$\text{ARIMA}(1,1,1)$$

$$
Y_t = (1+\phi) Y_{t-1} - \phi Y_{t-2} + \theta_0 + e_t -\theta e_{t-1} \\ \quad \\
\Rightarrow \begin{cases}
\hat{Y}_t(1) = (1+\phi) Y_t - \phi Y_{t-1} + \theta_0  -\theta e_t \\
\hat{Y}_t(2) = (1+\phi) \hat{Y}_t(1) - \phi Y_t + \theta_0 \\
\hat{Y}_t(\ell) =  (1+\phi) \hat{Y}_t(\ell-1) - \phi \hat{Y}_t(\ell-2) + \theta_0 
\end{cases}
$$

## 预测极限

### 确定性趋势

$$
Y_t = \mu_t + X_t
$$

假设白噪声随机项$$X_t\ \text{i.i.d.}\sim N(0, \gamma_0) $$ 

$$
\hat{Y}_t(\ell) = \mu_{t+\ell} \\
e_t(\ell) = Y_{t+\ell} - \hat{Y}_t(\ell) = X_{t+\ell} \\ \quad \\
\Rightarrow\ [Y_{t+\ell} - \hat{Y}_t(\ell)]\ \text{i.i.d.}\sim N(0,\gamma_0)
$$

对给定的置信水平$$1-\alpha$$，有：

$$
P[-z_{1-\frac{\alpha}{2}} < \frac{Y_{t+\ell} - \hat{Y}_t(\ell)}{\sqrt{\text{Var}(e_t(\ell))}} < z_{1-\frac{\alpha}{2}}] = 1-\alpha
$$

故有可信度为$$(1-\alpha)100\%$$的预测极限：

$$
\hat{Y}_t(\ell) \pm z_{1-\frac{\alpha}{2}} \sqrt{\text{Var}(e_t(\ell))}
$$

### ARIMA模型

如果白噪声独立地来自某正态分布，那么预测误差也服从正态分布。

$$
E[e_t(\ell)] = E[I_t(\ell)] = 0 \\
\text{Var}[e_t(\ell)] = \sigma_e^2 \sum_{j=0}^{\ell-1} \psi_j^2 \\ \quad \\
\Rightarrow Y_{t+\ell} - \hat{Y}_t(\ell) \sim N(0, 
\text{Var}(e_t(\ell)))
$$

可信度为$$(1-\alpha)100\%$$的预测极限同样为：

$$
\hat{Y}_t(\ell) \pm z_{1-\frac{\alpha}{2}} \sqrt{\text{Var}(e_t(\ell))}
$$

## ARIMA预测的更新

$$
Y_{t+\ell+1} = C_t(\ell+1) + I_t(\ell+1) \\
= C_t(\ell+1) + e_{t+\ell+1} + \psi_1 e_{t+\ell} + \cdots + \psi_\ell e_{t+1} \\
\Rightarrow\ \hat{Y}_{t+1}(\ell) = C_t(\ell+1) + \psi_\ell e_{t+1}
$$

$$
\begin{cases}
\hat{Y}_{t+1}(\ell) = C_t(\ell+1) + \psi_\ell e_{t+1}\qquad \text{(new)} \\
\hat{Y}_t(\ell+1) = C_t(\ell+1)\qquad \text{(old)}
\end{cases} \\
\Rightarrow\ \hat{Y}_{t+1}(\ell) = \hat{Y}_t(\ell+1) + \psi_\ell e_{t+1} = \hat{Y}_t(\ell+1) + \psi_\ell [Y_{t+1} - \hat{Y}_t(1)]
$$

