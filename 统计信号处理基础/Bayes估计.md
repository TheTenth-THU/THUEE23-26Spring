## 最小均方误差 (MMSE) 估计

### Bayes均方误差

为了体现**真值 $\theta$ 范围对估计量选取的影响**，使用视为随机变量的 $\theta$ 的分布加权此处的均方误差，得到
$$
\begin{align}
\dint \mathbb{E} \left[ (\hat{\theta} - \theta)^{2} \right] p(\theta) \dif \theta
&= \dint \left( \dint (\hat{\theta} - \theta)^{2} p(\v{x}; \theta) \dif \v{x} \right)_{\theta} p(\theta) \dif \theta \\
&= \iint (\hat{\theta} - \theta)^{2} p(\v{x} \mid \theta) p(\theta) \dif \v{x} \dif \theta 
= \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta
\end{align}
$$

> [!definition] Bayes均方误差
> 将待估计参数 $\theta$ 视为随机变量，并使用其分布加权经典均方误差，得到的加权均方误差称为 **Bayes均方误差 (Bayes mean square error, Bayes MSE)**，定义为
> $$
> \mathrm{Bmse} (\hat{\theta}) = \mathbb{E} \left[ (\theta - \hat{\theta})^{2} \right]  = \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta
> $$
> 注意 $\mathbb{E} \left[ (\theta - \hat{\theta})^{2} \right]$ 是一种带有约定俗成性质的简单记号，若交换 $\theta$ 和 $\hat{\theta}$ 的位置则一般表示经典MSE。

### MMSE估计量的求解

> [!definition] 最小均方误差 (MMSE) 估计
> **最小均方误差 (minimum mean square error, MMSE) 估计**是指在所有估计量中，具有最小Bayes均方误差的估计量，即
> $$
> \hat{\theta} = \arg\min_{\hat{\theta}} \mathrm{Bmse}(\hat{\theta}) = \arg\min_{\hat{\theta}} \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta
> $$

尝试分析Bayes MSE，注意到
$$
\begin{align}
\mathrm{Bmse}(\hat{\theta}) &= \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta 
= \iint (\hat{\theta} - \theta)^{2} p(\theta \mid \v{x}) p(\v{x}) \dif \theta \dif \v{x} \\
&= \int \left( \int (\hat{\theta} - \theta)^{2} p(\theta \mid \v{x}) \dif \theta \right) p(\v{x}) \dif \v{x}
\end{align}
$$
要最小化 $\mathrm{Bmse}(\hat{\theta})$，等价于最小化 $\dint (\hat{\theta} - \theta)^{2} p(\theta \mid \v{x}) \dif \theta$，因此MMSE估计量满足
$$
\begin{align}
0 &= \frac{\partial}{\partial \hat{\theta}} \int (\hat{\theta} - \theta)^{2} p(\theta \mid \v{x}) \dif \theta = \int \frac{\partial (\hat{\theta} - \theta)^{2}}{\partial \hat{\theta}} p(\theta \mid \v{x}) \dif \theta = \int 2(\hat{\theta} - \theta) p(\theta \mid \v{x}) \dif \theta  \\
&= 2 \left( \hat{\theta} - \int \theta p(\theta \mid \v{x}) \dif \theta \right)
\end{align}
$$
故
$$
\hat{\theta} = \int \theta p(\theta \mid \v{x}) \dif \theta = \mathbb{E}[\theta \mid \v{x}]
$$
即，**MMSE估计量是参数 $\theta$ 的后验分布 $p(\theta \mid \v{x})$ 的均值**。