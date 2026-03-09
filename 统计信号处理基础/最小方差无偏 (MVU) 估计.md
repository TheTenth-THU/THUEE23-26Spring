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

**$\stackrel{\text{克拉美罗}}{\text{Cramer-Rao}}$ 下界 (Cramer-Rao lower bound, CRLB)** 是满足一定正则条件的任何无偏估计的方差的下界，其给定了无偏估计的方差的理论最小值。

### 标量参数 CRLB 定理

> [!theorem] 标量参数 CRLB 定理
> 
> 假定概率密度函数 $p\left( \v{x};\theta \right)$ 满足以下正则条件
> $$
> \mathbb{E} \left[ \dfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta }  \right] = 0
> $$
> 则对于任何无偏估计 $\hat{\theta}$，
> 1. 方差满足
> 	$$
> 	\mathrm{var}(\hat{\theta}) \geq \dfrac{1}{\mathbb{E} \left[ \left( \cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } \right)^{2} \right] }
> 	= \dfrac{1}{- \mathbb{E} \left[ \cfrac{ \partial^{2} \ln p\left( \v{x};\theta \right) }{ \partial \theta^{2} }  \right] }
> 	$$
> 	该下界称为 **Cramer-Rao 下界**，其中 $\mathbb{E} \left[ \left( \cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } \right)^{2} \right] = -\mathbb{E} \left[ \cfrac{ \partial^{2} \ln p\left( \v{x};\theta \right) }{ \partial \theta^{2} }  \right]$ 称为 **Fisher 信息**。
> 2. 当且仅当存在函数 $\mathcal{I}(\cdot)$、$g(\cdot)$ 使得
> 	$$
> 	\dfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } = \mathcal{I}(\theta) \left( g\left( \v{x} \right) - \theta \right)
> 	$$
> 	才可得达到上述下限的 MVU 估计量 $\hat{\theta} = g\left( \v{x} \right)$，称为**有效估计量** (efficient estimator)，此时 $\mathrm{var}(\hat{\theta}) = \cfrac{1}{\mathcal{I}(\theta)}$。

CRLB 给出了无偏估计的方差的理论最小值，因此如果存在一个无偏估计的方差等于 CRLB，那么它就是 MVU 估计。反之，如果一个无偏估计的方差大于 CRLB，那么它很可能不是 MVU 估计。

#### 使用 CRLB 识别 MVU

> [!example] 例
> 
> **电平估计。** 
> 
> 考虑一个测量系统
> $$
> x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 其中噪声 $w[n] \sim \mathcal{N}(0, \sigma^{2})$ 是一个零均值的 Gauss 白噪声，$A$ 是一个未知的常数。我们希望估计 $A$ 的值。

尝试计算 CRLB。测量值的分布为
$$
\begin{align} 
p\left( \v{x};A \right) &= \prod_{n=0}^{N-1} \dfrac{1}{\sqrt{2\pi\sigma^{2}}} \exp \left( -\dfrac{(x[n] - A)^{2}}{2\sigma^{2}} \right) \\
&= \dfrac{1}{(2\pi\sigma^{2})^{N/2}} \exp \left( -\dfrac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right)
\end{align}
$$
因此对数似然函数为
$$
\ln p\left( \v{x};A \right) = -\dfrac{N}{2} \ln (2\pi\sigma^{2}) - \dfrac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2}
$$
对 $A$ 求导得到
$$
\begin{align}
&\dfrac{ \partial \ln p\left( \v{x};A \right) }{ \partial A } = -\dfrac{1}{2\sigma^{2}} \cdot 2 \sum_{n=0}^{N-1} (x[n] - A) (-1) = \dfrac{1}{\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A) \\
&\dfrac{ \partial^{2} \ln p\left( \v{x};A \right) }{ \partial A^{2} } = -\dfrac{N}{\sigma^{2}}
\end{align}
$$
因此 CRLB 为 $\mathrm{var}(\hat{A}) \geq \dfrac{1}{N/\sigma^{2}} = \dfrac{\sigma^{2}}{N}$。


#### 参数变换 CRLB

当待估计参数是某个参数（基本参数）的函数时，往往能直接估计性能界的是直接测量的基本参数的 CRLB，此时需要通过**参数变换**来得到待估计参数的 CRLB。

> [!theorem] 参数变换 CRLB 定理
> 假设 $\theta$ 是一个基本参数，$\alpha = g(\theta)$ 是一个待估计参数，其中 $g(\cdot)$ 是一个可微函数，则 $\alpha$ 的 CRLB 为
> $$
> \mathrm{var}(\hat{\alpha}) \geq \dfrac{\left( \cfrac{ \partial g(\theta) }{ \partial \theta } \right)^{2}}{\mathbb{E} \left[ \left(  \cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta }  \right)^{2} \right] } = \dfrac{\left( \cfrac{ \partial g(\theta) }{ \partial \theta } \right)^{2}}{- \mathbb{E} \left[ \cfrac{ \partial^{2} \ln p\left( \v{x};\theta \right) }{ \partial \theta^{2} }  \right] } 
> $$
> 即二者的 CRLB 满足
> $$
> \mathrm{CRLB}(\hat{\alpha}) = \left( \dfrac{ \partial g(\theta) }{ \partial \theta }  \right)^{2} \cdot \mathrm{CRLB}(\hat{\theta})
> $$

据此，可以考察某些参数变换是否保持有效估计量的有效性：
+ 对**线性变换** $\alpha = a\theta + b$，若有 $\hat{\theta}$ 是 $\theta$ 的有效估计量，则
$$
\begin{align}
&\mathbb{E} \left[ \hat{\alpha} \right] = \mathbb{E} \left[ a\hat{\theta} + b \right] = a\mathbb{E} \left[ \hat{\theta} \right] + b = a\theta + b = \alpha \\
&\mathrm{var}(\hat{\alpha}) = a^{2} \mathrm{var}(\hat{\theta}) = a^{2} \cdot \mathrm{CRLB}(\hat{\theta}) = \mathrm{CRLB}(\hat{\alpha})
\end{align}
$$
因此线性变换**保持有效估计量的有效性**。
+ 对**非线性变换** $\alpha = g(\theta)$，若有 $\hat{\theta}$ 是 $\theta$ 的有效估计量，则
$$
\mathbb{E} \left[ \hat{\alpha} \right] = \mathbb{E} \left[ g(\hat{\theta}) \right] \neq g\left(\mathbb{E} [ \hat{\theta} ]\right) = g(\theta) = \alpha
$$
因此此估计是**有偏的**，因此非线性变换**不保持有效估计量的有效性**。

---

> [!example] 例
> **功率估计。**
> 
> 考虑一个测量系统
> $$
> x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 其中噪声 $w[n] \sim \mathcal{N}(0, \sigma^{2})$ 是一个零均值的 Gauss 白噪声，电平 $A$ 是一个未知的常数。
> 
> 已知 $A$ 的有效估计量 $\hat{A} = \dfrac{1}{N} \sum\limits_{n=0}^{N-1} x[n]$，我们希望估计功率 $A^{2}$ 的值。

$A \mapsto A^{2}$ 是一个非线性变换，有
$$
\mathbb{E} \left[ \hat{A}^{2} \right] = \mathrm{var}(\hat{A}) + \left( \mathbb{E} [ \hat{A} ] \right)^{2} = \dfrac{\sigma^{2}}{N} + A^{2} \neq A^{2}
$$
因此 $\hat{A}^{2}$ 是一个有偏估计；但当 $N \to \infty$ 时，$\mathbb{E} \left[ \hat{A}^{2} \right] \to A^{2}$，因此 $\hat{A}^{2}$ 是一个**渐近无偏估计 (asymptotically unbiased estimator)**。

此外，其方差为
$$
\begin{align}
&\begin{aligned} 
\mathrm{var} (\hat{A}^{2}) &= \mathbb{E} \left[ \hat{A}^{4} \right] - \left( \mathbb{E} \left[ \hat{A}^{2} \right] \right)^{2}  \\
&= A^{4} + \dfrac{6\sigma^{2}}{N} A^{2} + \dfrac{3\sigma^{4}}{N^{2}} - \left( A^{2} + \dfrac{\sigma^{2}}{N} \right)^{2} \\
&= \dfrac{4\sigma^{2}}{N} A^{2} + \dfrac{2\sigma^{4}}{N^{2}} \xrightarrow{N \to \infty} \dfrac{4\sigma^{2}}{N} A^{2} 
\end{aligned} \\
&\mathrm{CRLB}(\hat{A}^{2}) = \left( \dfrac{ \partial A^{2} }{ \partial A }  \right)^{2} \cdot \mathrm{CRLB}(\hat{A}) = 4A^{2} \cdot \dfrac{\sigma^{2}}{N} = \dfrac{4A^{2}\sigma^{2}}{N}
\end{align}
$$
即 $\hat{A}^{2}$ 的方差**渐近达到 CRLB**，因此 $\hat{A}^{2}$ 是 $A^{2}$ 的一个**渐近 MVU 估计量 (asymptotically MVU estimator)**。

---

对任意非线性变换 $\alpha = g(\theta)$，若有 $\hat{\theta}$ 是 $\theta$ 的有效估计量，则由
$$
g(\hat{\theta}) = g(\theta) + g'(\theta) (\hat{\theta} - \theta) + o(\hat{\theta} - \theta)
$$
在 $N \to \infty$ 时，$\hat{\theta} \xrightarrow{P} \theta$，此时 $g(\theta)$ 可线性化处理，故 $g(\hat{\theta})$ 是 $g(\theta)$ 的一个**渐近 MVU 估计量**。

### 矢量参数 CRLB 定理

假定概率密度函数 $p\left( \v{x};\v{\theta} \right)$ 满足以下正则条件
$$
\mathbb{E} \left[ \dfrac{ \partial \ln p\left( \v{x};\v{\theta} \right) }{ \partial \theta_{i} }  \right] = 0, \qquad i = 1, 2, \cdots, k
$$
则对于任何无偏估计 $\hat{\v{\theta}}$，
1. 协方差矩阵满足
	$$
	\boldsymbol{C}_{\hat{\v{\theta}}} \succeq \boldsymbol{I}^{-1}(\v{\theta})
	$$
	其中 $\boldsymbol{I}(\v{\theta})$ 是 $\v{\theta}$ 的 **Fisher 信息矩阵**，定义为 $\Big( \boldsymbol{I}(\v{\theta}) \Big)_{i,j} = -\mathbb{E} \left[ \dfrac{ \partial^{2} \ln p\left( \v{x};\v{\theta} \right) }{ \partial \theta_{i} \partial \theta_{j} }  \right]$。
2. 当且仅当存在 $p$ 维函数 $\v{g}(\cdot)$ 和 $p \times p$ 矩阵 $\boldsymbol{I}(\cdot)$ 使得
	$$
	\dfrac{ \partial \ln p\left( \v{x};\v{\theta} \right) }{ \partial \v{\theta} } = \boldsymbol{I}(\v{\theta}) \left( \v{g}\left( \v{x} \right) - \v{\theta} \right)
	$$
	才可得达到上述下限的 MVU 估计量 $\hat{\v{\theta}} = \v{g}\left( \v{x} \right)$，此时 $\boldsymbol{C}_{\hat{\v{\theta}}} = \boldsymbol{I}^{-1}(\v{\theta})$。

若 $\v{\alpha} = \v{g}(\v{\theta})$，则 $\v{\alpha}$ 的 CRLB 为
$$
\boldsymbol{C}_{\hat{\v{\alpha}}} \succeq \boldsymbol{J}(\v{\theta}) \boldsymbol{I}^{-1}(\v{\theta}) \boldsymbol{J}^{\mathrm{T}}(\v{\theta})
$$
其中 $\boldsymbol{J}(\v{\theta})$ 是 $g(\cdot)$ 的 Jacobian 矩阵，即 $\Big( \boldsymbol{J}(\v{\theta}) \Big)_{i,j} = \dfrac{ \partial g_{i}(\v{\theta}) }{ \partial \theta_{j} }$。