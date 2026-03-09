## MVU 的含义

**最小方差无偏 (minimum variance unbiased, MVU) 估计**是指在所有无偏估计中，方差最小的估计。换句话说，MVU 估计在所有无偏估计中具有最小的平均误差。

### 无偏

一个估计 $\hat{\theta}$ 是**无偏的**，如果它的期望值等于真值 $\theta$，即
$$
\mathbb{E}[\hat{\theta}] = \theta, \qquad a < \theta < b
$$
其中 $a$ 和 $b$ 是参数 $\theta$ 的定义域的下界和上界。

### 最小方差

在所有满足无偏条件的估计中，MVU 估计 $\hat{\theta}_{\mathrm{MVU}}$ 满足
$$
\mathrm{var}(\hat{\theta}_{\mathrm{MVU}}) = \min \left\{ \mathrm{var}(\hat{\theta}) : \mathbb{E}[\hat{\theta}] = \theta \right\}, \qquad a < \theta < b
$$
MVU 所要求的是**一致最小**，即对于参数 $\theta$ 的所有可能取值，MVU 估计的方差都不大于其他无偏估计的方差。


> [!note] MVU 与 MSE 的关系
> 某种程度上，MVU 是对无法实现的最小 MSE 准则的迂回实现：
> $$
> \begin{align} 
> \min \left\{ \mathrm{var}(\hat{\theta}) \right\} &= \min \left\{ \mathbb{E} \left[ (\hat{\theta} - \mathbb{E} [ \hat{\theta} ] )^2 \right] \right\} \\
> &\stackrel{\mathbb{E} [ \hat{\theta} ] = \theta}{=\!=\!=\!=\!=} \min \left\{ \mathbb{E} \left[ (\hat{\theta} - \theta)^2 \right] \right\} = \min \left\{ \mathrm{mse}(\hat{\theta}) \right\} 
> \end{align}
> $$
> 
> 由于 MSE 包含了方差和偏差两部分，而 MVU 直接要求无偏，因此在所有无偏估计中，MVU 的 MSE 就等于它的方差，因此 MVU 是在所有无偏估计中具有最小 MSE 的估计。

## Cramer-Rao 下界

**Cramer-Rao 下界 (Cramer-Rao lower bound, CRLB)** 是满足一定正则条件的任何无偏估计的方差的下界，其给定了无偏估计的方差的理论最小值。

### 标量参数 CRLB 定理

> [!theorem] 标量参数 CRLB 定理
> 
> 假定概率密度函数 $p\left( \v{x};\theta \right)$ 满足以下正则条件
> $$
> \mathbb{E} \left[ \dfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta }  \right] = 0
> $$
> 则对于任何无偏估计 $\hat{\theta}$，
> 1. 方差满足
> $$
> \mathrm{var}(\hat{\theta}) \geq \dfrac{1}{\mathbb{E} \left[ \left( \cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } \right)^{2} \right] }
> = \dfrac{1}{- \mathbb{E} \left[ \dfrac{ \partial^{2} \ln p\left( \v{x};\theta \right) }{ \partial \theta^{2} }  \right] }
> $$
> 2. 当且仅当存在函数 $\mathcal{I}(\cdot)$、$g(\cdot)$ 使得
> $$
> \dfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } = \mathcal{I}(\theta) \left( g\left( \v{x} \right) - \theta \right)
> $$
> 才可得达到上述下限的 MVU 估计量 $\hat{\theta} = g\left( \v{x} \right)$，称为**有效估计量** (efficient estimator)，此时 $\mathrm{var}(\hat{\theta}) = \cfrac{1}{\mathcal{I}(\theta)}$。

