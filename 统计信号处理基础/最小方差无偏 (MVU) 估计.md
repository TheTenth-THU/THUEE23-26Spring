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


> [!note] MVU与MSE的关系
> 某种程度上，MVU是对无法实现的最小MSE准则的迂回实现：
> $$
> \begin{align} 
> \min \left\{ \mathrm{var}(\hat{\theta}) \right\} &= \min \left\{ \mathbb{E} \left[ (\hat{\theta} - \mathbb{E} [ \hat{\theta} ] )^2 \right] \right\} \\
> &\stackrel{\mathbb{E} [ \hat{\theta} ] = \theta}{=\!=\!=\!=\!=} \min \left\{ \mathbb{E} \left[ (\hat{\theta} - \theta)^2 \right] \right\} = \min \left\{ \mathrm{mse}(\hat{\theta}) \right\} 
> \end{align}
> $$
> 
> 由于MSE包含了方差和偏差两部分，而MVU直接要求无偏，因此在所有无偏估计中，MVU的MSE就等于它的方差，因此MVU是在所有无偏估计中具有最小MSE的估计。

## Cramer-Rao 下界

**{Cramer|克拉美}-{Rao|罗}下界 (Cramer-Rao lower bound, CRLB)** 是满足一定正则条件的任何无偏估计的方差的下界，其给定了无偏估计的方差的理论最小值。

### 标量参数 CRLB 定理

> [!theorem] 标量参数 CRLB 定理
> 
> 假定概率密度函数 $p\left( \v{x};\theta \right)$ 满足以下正则条件
> $$
> \mathbb{E} \left[ \frac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta }  \right] = 0
> $$
> 则对于任何无偏估计 $\hat{\theta}$，
> 1. 方差满足
> 		$$
> 		\mathrm{var}(\hat{\theta}) \geq \frac{1}{\mathbb{E} \left[ \left( \cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } \right)^{2} \right] }
> 		= \frac{1}{- \mathbb{E} \left[ \cfrac{ \partial^{2} \ln p\left( \v{x};\theta \right) }{ \partial \theta^{2} }  \right] }
> 		$$
> 		该下界称为 **Cramer-Rao 下界**，其中 $\mathbb{E} \left[ \left( \cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } \right)^{2} \right] = -\mathbb{E} \left[ \cfrac{ \partial^{2} \ln p\left( \v{x};\theta \right) }{ \partial \theta^{2} }  \right]$ 称为 **Fisher 信息**。
> 2. 当且仅当存在函数 $\mathcal{I}(\cdot)$、$g(\cdot)$ 使得
> 		$$
> 		\frac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } = \mathcal{I}(\theta) \left( g\left( \v{x} \right) - \theta \right)
> 		$$
> 		才可得达到上述下限的 MVU 估计量 $\hat{\theta} = g\left( \v{x} \right)$，称为**有效估计量** (efficient estimator)，此时 $\mathrm{var}(\hat{\theta}) = \cfrac{1}{\mathcal{I}(\theta)}$。

CRLB 给出了无偏估计的方差的理论最小值，因此如果存在一个无偏估计的方差等于 CRLB，那么它就是 MVU 估计。反之，如果一个无偏估计的方差大于 CRLB，那么它很可能不是 MVU 估计。

#### 使用 CRLB 识别 MVU

CRLB 可用于评价一个无偏估计是否为 MVU 估计：
+ 若 $\mathrm{var}(\hat{\theta}) = \mathrm{CRLB}(\hat{\theta})$，则 $\hat{\theta}$ 是最佳的 MVU 估计。
+ 若 $\mathrm{var}(\hat{\theta}) > \mathrm{CRLB}(\hat{\theta})$，则 $\hat{\theta}$ 可能不是 MVU 估计。

> [!example]- 计算估计的CRLB：示例
> 
> **电平估计。** 
> 
> 考虑一个测量系统
> $$
> x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 其中噪声 $w[n] \sim \mathcal{N}(0, \sigma^{2})$ 是一个零均值的 Gauss 白噪声，$A$ 是一个未知的常数。我们希望估计 $A$ 的值。
> 
> ---
> 
> 尝试计算 CRLB。测量值的分布为
> $$
> \begin{align} 
> p\left( \v{x};A \right) &= \prod_{n=0}^{N-1} \frac{1}{\sqrt{2\pi\sigma^{2}}} \exp \left( -\frac{(x[n] - A)^{2}}{2\sigma^{2}} \right) \\
> &= \frac{1}{(2\pi\sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right)
> \end{align}
> $$
> 因此对数似然函数为
> $$
> \ln p\left( \v{x};A \right) = -\frac{N}{2} \ln (2\pi\sigma^{2}) - \frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2}
> $$
> 对 $A$ 求导得到
> $$
> \begin{align}
> &\frac{ \partial \ln p\left( \v{x};A \right) }{ \partial A } = -\frac{1}{2\sigma^{2}} \cdot 2 \sum_{n=0}^{N-1} (x[n] - A) (-1) = \frac{1}{\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A) \\
> &\frac{ \partial^{2} \ln p\left( \v{x};A \right) }{ \partial A^{2} } = -\frac{N}{\sigma^{2}}
> \end{align}
> $$
> 因此 CRLB 为 $\mathrm{var}(\hat{A}) \geq \cfrac{1}{N/\sigma^{2}} = \cfrac{\sigma^{2}}{N}$。
^Example-2-1

#### 使用 CRLB 求解有效估计量

CRLB 还可用于求解 MVU 估计量，将对数似然函数的导数表达为 $\cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } = \mathcal{I}(\theta) \left( g\left( \v{x} \right) - \theta \right)$ 的形式，即得到有效估计量 $\hat{\theta} = g\left( \v{x} \right)$ 及其方差 $\mathrm{var}(\hat{\theta}) = \cfrac{1}{\mathcal{I}(\theta)}$。注意：
+ $\mathcal{I}(\theta)$ 是 $\theta$ 的函数，**不能与观测数据 $\v{x}$ 有关**。
+ $g(\v{x})$ 是 $\v{x}$ 的函数，**不能与待估计参数 $\theta$ 有关**。

> [!example]- 使用CRLB求解MVU：示例
> 
> **电平估计。** 
> 
> 考虑一个测量系统
> $$
> x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 其中噪声 $w[n] \sim \mathcal{N}(0, \sigma^{2})$ 是一个零均值的 Gauss 白噪声，$A$ 是一个未知的常数。我们希望估计 $A$ 的值。
> 
> ---
> 
> 同上例，对数似然函数的导数为 
> $$
> \frac{ \partial \ln p\left( \v{x};A \right) }{ \partial A } = \frac{1}{\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A) = \frac{N}{\sigma^{2}} \left( \frac{1}{N} \sum_{n=0}^{N-1} x[n] - A \right)
> $$
> 因此 $\mathcal{I}(A) = \cfrac{N}{\sigma^{2}}$，$g(\v{x}) = \cfrac{1}{N} \sum\limits_{n=0}^{N-1} x[n]$，有效估计量为 $\hat{A} = \cfrac{1}{N} \sum\limits_{n=0}^{N-1} x[n]$，方差为 $\mathrm{var}(\hat{A}) = \cfrac{\sigma^{2}}{N}$。

#### 参数变换 CRLB

当待估计参数是某个参数（基本参数）的函数时，往往能直接估计性能界的是直接测量的基本参数的 CRLB，此时需要通过**参数变换**来得到待估计参数的 CRLB。

> [!theorem] 参数变换 CRLB 定理
> 假设 $\theta$ 是一个基本参数，$\alpha = g(\theta)$ 是一个待估计参数，其中 $g(\cdot)$ 是一个可微函数，则 $\alpha$ 的 CRLB 为
> $$
> \mathrm{var}(\hat{\alpha}) \geq \frac{\left( \cfrac{ \partial g(\theta) }{ \partial \theta } \right)^{2}}{\mathbb{E} \left[ \left(  \cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta }  \right)^{2} \right] } = \frac{\left( \cfrac{ \partial g(\theta) }{ \partial \theta } \right)^{2}}{- \mathbb{E} \left[ \cfrac{ \partial^{2} \ln p\left( \v{x};\theta \right) }{ \partial \theta^{2} }  \right] } 
> $$
> 即二者的 CRLB 满足
> $$
> \mathrm{CRLB}(\hat{\alpha}) = \left( \frac{ \partial g(\theta) }{ \partial \theta }  \right)^{2} \cdot \mathrm{CRLB}(\hat{\theta})
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

> [!example]+ 计算非线性变换参数的CRLB：示例
> 
> **功率估计。**
> 
> 考虑一个测量系统
> $$
> x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 其中噪声 $w[n] \sim \mathcal{N}(0, \sigma^{2})$ 是一个零均值的 Gauss 白噪声，电平 $A$ 是一个未知的常数。
> 
> 已知 $A$ 的有效估计量 $\hat{A} = \cfrac{1}{N} \sum\limits_{n=0}^{N-1} x[n]$，我们希望估计功率 $A^{2}$ 的值。
> 
> ---
> 
> $A \mapsto A^{2}$ 是一个非线性变换，有
> $$
> \mathbb{E} \left[ \hat{A}^{2} \right] = \mathrm{var}(\hat{A}) + \left( \mathbb{E} [ \hat{A} ] \right)^{2} = \frac{\sigma^{2}}{N} + A^{2} \neq A^{2}
> $$
> 因此 $\hat{A}^{2}$ 是一个有偏估计；但当 $N \to \infty$ 时，$\mathbb{E} \left[ \hat{A}^{2} \right] \to A^{2}$，因此 $\hat{A}^{2}$ 是一个**渐近无偏估计 (asymptotically unbiased estimator)**。
> 
> 此外，其方差为
> $$
> \begin{align}
> &\begin{aligned} 
> \mathrm{var} (\hat{A}^{2}) &= \mathbb{E} \left[ \hat{A}^{4} \right] - \left( \mathbb{E} \left[ \hat{A}^{2} \right] \right)^{2}  \\
> &= A^{4} + \frac{6\sigma^{2}}{N} A^{2} + \frac{3\sigma^{4}}{N^{2}} - \left( A^{2} + \frac{\sigma^{2}}{N} \right)^{2} \\
> &= \frac{4\sigma^{2}}{N} A^{2} + \frac{2\sigma^{4}}{N^{2}} \xrightarrow{N \to \infty} \frac{4\sigma^{2}}{N} A^{2} 
> \end{aligned} \\
> &\mathrm{CRLB}(\hat{A}^{2}) = \left( \frac{ \partial A^{2} }{ \partial A }  \right)^{2} \cdot \mathrm{CRLB}(\hat{A}) = 4A^{2} \cdot \frac{\sigma^{2}}{N} = \frac{4A^{2}\sigma^{2}}{N}
> \end{align}
> $$
> 即 $\hat{A}^{2}$ 的方差**渐近达到 CRLB**，因此 $\hat{A}^{2}$ 是 $A^{2}$ 的一个**渐近 MVU 估计量 (asymptotically MVU estimator)**。

对任意非线性变换 $\alpha = g(\theta)$，若有 $\hat{\theta}$ 是 $\theta$ 的有效估计量，则由
$$
g(\hat{\theta}) = g(\theta) + g'(\theta) (\hat{\theta} - \theta) + o(\hat{\theta} - \theta)
$$
在 $N \to \infty$ 时，$\hat{\theta} \xrightarrow{P} \theta$，此时 $g(\theta)$ 可线性化处理，故 $g(\hat{\theta})$ 是 $g(\theta)$ 的一个**渐近 MVU 估计量**。

### 矢量参数 CRLB 定理

假定概率密度函数 $p\left( \v{x};\v{\theta} \right)$ 满足以下正则条件
$$
\mathbb{E} \left[ \frac{ \partial \ln p\left( \v{x};\v{\theta} \right) }{ \partial \theta_{i} }  \right] = 0, \qquad i = 1, 2, \cdots, k
$$
则对于任何无偏估计 $\hat{\v{\theta}}$，
1. 协方差矩阵满足
	$$
	\boldsymbol{C}_{\hat{\v{\theta}}} \succeq \boldsymbol{I}^{-1}(\v{\theta})
	$$
	其中 $\boldsymbol{I}(\v{\theta})$ 是 $\v{\theta}$ 的 **Fisher 信息矩阵**，定义为 $\Big( \boldsymbol{I}(\v{\theta}) \Big)_{i,j} = -\mathbb{E} \left[ \cfrac{ \partial^{2} \ln p\left( \v{x};\v{\theta} \right) }{ \partial \theta_{i} \partial \theta_{j} }  \right]$。
2. 当且仅当存在 $p$ 维函数 $\v{g}(\cdot)$ 和 $p \times p$ 矩阵 $\boldsymbol{I}(\cdot)$ 使得
	$$
	\frac{ \partial \ln p\left( \v{x};\v{\theta} \right) }{ \partial \v{\theta} } = \boldsymbol{I}(\v{\theta}) \left( \v{g}\left( \v{x} \right) - \v{\theta} \right)
	$$
	才可得达到上述下限的 MVU 估计量 $\hat{\v{\theta}} = \v{g}\left( \v{x} \right)$，此时 $\boldsymbol{C}_{\hat{\v{\theta}}} = \boldsymbol{I}^{-1}(\v{\theta})$。

若 $\v{\alpha} = \v{g}(\v{\theta})$，则 $\v{\alpha}$ 的 CRLB 为
$$
\boldsymbol{C}_{\hat{\v{\alpha}}} \succeq \boldsymbol{J}(\v{\theta}) \boldsymbol{I}^{-1}(\v{\theta}) \boldsymbol{J}^{\mathrm{T}}(\v{\theta})
$$
其中 $\boldsymbol{J}(\v{\theta})$ 是 $g(\cdot)$ 的 Jacobian 矩阵，即 $\Big( \boldsymbol{J}(\v{\theta}) \Big)_{i,j} = \cfrac{ \partial g_{i}(\v{\theta}) }{ \partial \theta_{j} }$。

## MVU 基本求解方法

上述[[#使用 CRLB 求解有效估计量]]的方法是通过 CRLB 来求解 MVU 估计量的一种方法。然而，其求出的一定是**有效估计量**，但很多情形下MVU不一定达到CRLB，因此不能由该方法求解出 MVU 估计量。

### 线性模型方法

#### 简单线性模型方法

> [!definition] 线性模型
> 
> **线性模型**是指这样的测量模型，其观测数据 $\v{x}$ 与待估计参数 $\v{\theta}$ 之间满足线性关系
> $$
> \v{x} = \boldsymbol{H} \v{\theta} + \v{w}
> $$
> 其中：
> + $\v{x}$ 是一个 $N$ 维的观测向量；
> + $\boldsymbol{H}$ 是一个 $N \times p$ ($N>p$) 的已知矩阵，称为**设计矩阵** (design matrix)；
> + $\v{\theta}$ 是一个 $p$ 维的未知参数向量；
> + $\v{w}$ 是一个 $N$ 维的独立噪声向量，服从Gauss分布 $\v{w} \sim \mathcal{N}(\v{0}, \sigma^{2} \boldsymbol{I})$。

针对线性模型，尝试按照[[#使用 CRLB 求解有效估计量]]的方法求解估计量 $\hat{\v{\theta}}$，有
$$
\begin{align}
&p\left( \v{x};\v{\theta} \right) = \frac{1}{(2\pi\sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} (\v{x} - \boldsymbol{H} \v{\theta})^{\mathrm{T}} (\v{x} - \boldsymbol{H} \v{\theta}) \right) \\
& \begin{aligned} 
\implies \ln p\left( \v{x};\v{\theta} \right) &= -\frac{N}{2} \ln (2\pi\sigma^{2}) - \frac{1}{2\sigma^{2}} (\v{x} - \boldsymbol{H} \v{\theta})^{\mathrm{T}} (\v{x} - \boldsymbol{H} \v{\theta}) \\
&= -\frac{N}{2} \ln (2\pi\sigma^{2}) - \frac{1}{2\sigma^{2}} \left( \v{x}^{\mathrm{T}} \v{x} - 2\v{\theta}^{\mathrm{T}} \boldsymbol{H}^{\mathrm{T}} \v{x} + \v{\theta}^{\mathrm{T}} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{H} \v{\theta} \right) 
\end{aligned} \\
& \implies \frac{ \partial \ln p\left( \v{x};\v{\theta} \right) }{ \partial \v{\theta} } = -\frac{1}{2\sigma^{2}} \cdot 2 \left( -\boldsymbol{H}^{\mathrm{T}} \v{x} + \boldsymbol{H}^{\mathrm{T}} \boldsymbol{H} \v{\theta} \right) = \frac{1}{\sigma^{2}} \boldsymbol{H}^{\mathrm{T}} (\v{x} - \boldsymbol{H} \v{\theta})
\end{align}
$$
根据CRLB定理，尝试将上式写成 $\boldsymbol{I}(\v{\theta}) \left( \v{g}\left( \v{x} \right) - \v{\theta} \right)$ 的形式，有
$$
\frac{ \partial \ln p\left( \v{x};\v{\theta} \right) }{ \partial \v{\theta} } = \frac{1}{\sigma^{2}} \boldsymbol{H}^{\mathrm{T}} (\v{x} - \boldsymbol{H} \v{\theta})
= \frac{\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H}}{\sigma^{2}} \left( (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x} - \v{\theta} \right)
$$
因此**线性模型对应的 MVU 估计量是有效估计量**
$$
\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x}
$$
其**协方差矩阵**为
$$
\boldsymbol{C}_{\hat{\v{\theta}}} = \boldsymbol{I}^{-1}(\v{\theta}) = \sigma^{2} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1}
$$
同时，由Gauss分布性质，$\hat{\v{\theta}}$ 服从 **Gauss 分布 $\hat{\v{\theta}} \sim \mathcal{N}(\v{\theta}, \sigma^{2} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1})$**。

> [!example]- 使用线性模型求解MVU：示例一
> 
> **直线拟合。**
> 
> 考虑一个测量系统
> $$
> x[n] = A + Bn + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 其中噪声 $w[n] \sim \mathcal{N}(0, \sigma^{2})$ 是一个零均值的 Gauss 白噪声，待估计参数为 $\v{\theta} = \begin{pmatrix}A & B\end{pmatrix}^{\mathrm{T}}$。
> 
> ---
> 
> 若直接利用[[#矢量参数 CRLB 定理]]，须求
> $$
> \begin{align}
> & p(\v{x}; \v{\theta}) = \prod\limits_{n=0}^{N-1} p(x[n]; \v{\theta}) = \frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( - \frac{1}{2\sigma^{2}} \sum\limits_{n=0}^{N-1} (x[n] - A - Bn)^{2} \right)  \\
> & \implies \ln p(\v{x}; \v{\theta}) = -\frac{N}{2} \ln 2\pi\sigma^{2} - \frac{1}{2\sigma^{2}} \sum\limits_{n=0}^{N-1} (x[n] - A - Bn)^{2} \\
> & \implies \begin{cases}
> \cfrac{ \partial \ln p(\v{x}; \v{\theta}) }{ \partial A } = - \cfrac{1}{\sigma^{2}} \sum\limits_{n=0}^{N-1} (x[n] - A - Bn), \\
> \cfrac{ \partial \ln p(\v{x}; \v{\theta}) }{ \partial B } = - \cfrac{1}{\sigma^{2}} \sum\limits_{n=0}^{N-1} (x[n] - A - Bn)n
> \end{cases}  \\
> & \implies \begin{cases}
> \cfrac{ \partial^{2} \ln p(\v{x}; \v{\theta}) }{ \partial A^{2} } = \cfrac{N}{\sigma^{2}},  
> & \cfrac{ \partial^{2} \ln p(\v{x}; \v{\theta}) }{ \partial B \partial A } = -\cfrac{1}{\sigma^{2}} \sum\limits_{n=0}^{N-1} n, \\
> \cfrac{ \partial^{2} \ln p(\v{x}; \v{\theta}) }{ \partial B^{2} } = -\cfrac{1}{\sigma^{2}} \sum\limits_{n=0}^{N-1} n^{2} ,  
> & \cfrac{ \partial^{2} \ln p(\v{x}; \v{\theta}) }{ \partial A \partial B } = -\cfrac{1}{\sigma^{2}} \sum\limits_{n=0}^{N-1} n 
> \end{cases}
> \end{align}
> $$
> 即得到Fisher信息矩阵
> $$
> \boldsymbol{I}(\v{\theta}) = \frac{1}{\sigma^{2}} \begin{pmatrix}
> N & \cfrac{N(N-1)}{2} \\
> \cfrac{N(N-1)}{2} & \cfrac{N(N-1)(2N-1)}{6}
> \end{pmatrix}
> $$
> 并尝试求解
> $$
> \frac{ \partial \ln p(\v{x}; \v{\theta}) }{ \partial \v{\theta} } = \begin{pmatrix}
> \cfrac{ \partial \ln p(\v{x}; \v{\theta}) }{ \partial A } \\
> \cfrac{ \partial \ln p(\v{x}; \v{\theta}) }{ \partial B }
> \end{pmatrix} = \boldsymbol{I}(\v{\theta}) \left( \v{g}(\v{x}) - \v{\theta} \right)
> $$
> 过程极为复杂。
> 
> 注意到该测量系统是一个**线性模型**，建模为
> $$
> \v{x} = \boldsymbol{H} \v{\theta} + \v{w}, \qquad
> \boldsymbol{H} = \begin{pmatrix}
> 1 & 0 \\
> 1 & 1 \\
> 1 & 2 \\
> \vdots & \vdots \\
> 1 & N-1
> \end{pmatrix}
> $$
> 因而直接代入得其 **MVU 估计量**为 $\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x}$，**协方差矩阵**为 $\boldsymbol{C}_{\hat{\v{\theta}}} = \sigma^{2} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1}$。计算得到
> $$
> \begin{align}
> & \boldsymbol{H}^{\mathrm{T}} \boldsymbol{H} = \begin{pmatrix}
> N & \sum\limits_{n=0}^{N-1} n \\
> \sum\limits_{n=0}^{N-1} n & \sum\limits_{n=0}^{N-1} n^{2}
> \end{pmatrix} = \begin{pmatrix}
> N & \cfrac{N(N-1)}{2} \\
> \cfrac{N(N-1)}{2} & \cfrac{N(N-1)(2N-1)}{6}
> \end{pmatrix} \\
> & (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} = \frac{12}{N^{2}(N-1)(N+1)} \begin{pmatrix}
> \cfrac{N(N-1)(2N-1)}{6} & -\cfrac{N(N-1)}{2} \\
> -\cfrac{N(N-1)}{2} & N
> \end{pmatrix} 
> \end{align}
> $$
> 故得到估计量
> $$
> \hat{\v{\theta}} = \begin{pmatrix}
> \hat{A} \\ \hat{B}
> \end{pmatrix} = \begin{pmatrix}
> \cfrac{2(2N-1)}{N(N+1)} \sum\limits_{n=0}^{N-1} x[n] - \cfrac{6}{N(N+1)} \sum\limits_{n=0}^{N-1} n x[n] \\
> -\cfrac{6}{N(N-1)} \sum\limits_{n=0}^{N-1} x[n] + \cfrac{12}{N(N-1)(N+1)} \sum\limits_{n=0}^{N-1} n x[n]
> \end{pmatrix}
> $$
> 其协方差矩阵为
> $$
> \begin{align} 
> \boldsymbol{C}_{\hat{\v{\theta}}} &= \sigma^{2} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1}  \\
> &= \frac{12\sigma^{2}}{N^{2}(N-1)(N+1)} \begin{pmatrix}
> \cfrac{N(N-1)(2N-1)}{6} & -\cfrac{N(N-1)}{2} \\
> -\cfrac{N(N-1)}{2} & N
> \end{pmatrix} 
> \end{align}
> $$
> 

> [!example]+ 使用线性模型求解MVU：示例二
> 
> **频率分量幅度分析。**
> 
> 考虑测量系统
> $$
> x[n] = \sum\limits_{k=1}^{M} a_{k} \cos\left( \frac{2\pi kn}{N} \right) + \sum\limits_{k=1}^{M} b_{k} \sin\left( \frac{2\pi kn}{N} \right) + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 假定以基频 $\cfrac{1}{N}$ 的谐波分析，待估计参数为 $\v{\theta} = \begin{pmatrix}a_{1} & \cdots & a_{M} & b_{1} & \cdots & b_{M}\end{pmatrix}^{\mathrm{T}}$，噪声 $w[n] \sim \mathcal{N}(0, \sigma^{2})$ 是一个零均值的 Gauss 白噪声。
> 
> ---
> 
> 该测量系统也是一个**线性模型**，建模为
> $$
> \begin{align}
> & \v{x} = \boldsymbol{H} \v{\theta} + \v{w}, \qquad \\
> &\text{其中}\hspace{1em} \boldsymbol{H} = \begin{pmatrix}
> \v{h}_{1} & \v{h}_{2} & \cdots & \v{h}_{M} & \v{h}_{M+1} & \v{h}_{M+2} & \cdots & \v{h}_{2M}
> \end{pmatrix} \\
> &\hspace{3em} \v{h}_{k} = \begin{cases}
> \bigg( \cos\left( \cfrac{2\pi kn}{N} \right) \bigg)_{n=0}^{N-1}, & k = 1, 2, \cdots, M \\
> \bigg( \sin\left( \cfrac{2\pi (k-M)n}{N} \right) \bigg)_{n=0}^{N-1}, & k = M+1, M+2, \cdots, 2M
> \end{cases}
> \end{align}
> $$
> 得到 **MVU 估计量**为 
> $$
> \begin{align} 
> \hat{\v{\theta}} &= (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x} = \frac{2}{N} \boldsymbol{H}^{\mathrm{T}} \v{x} \\
> &= \frac{2}{N} \begin{pmatrix}
> \v{h}_{1}^{\mathrm{T}} \v{x} & \v{h}_{2}^{\mathrm{T}} \v{x} & \cdots & \v{h}_{M}^{\mathrm{T}} \v{x} & \v{h}_{M+1}^{\mathrm{T}} \v{x} & \v{h}_{M+2}^{\mathrm{T}} \v{x} & \cdots & \v{h}_{2M}^{\mathrm{T}} \v{x}
> \end{pmatrix}^{\mathrm{T}}
> \end{align}
> $$
> 即
> $$
> \begin{align} 
> \hat{a}_{k} = \frac{2}{N} \sum\limits_{n=0}^{N-1} x[n] \cos\left( \frac{2\pi kn}{N} \right), \quad
> \hat{b}_{k} = \frac{2}{N} \sum\limits_{n=0}^{N-1} x[n] \sin\left( \frac{2\pi kn}{N} \right), \\
> k = 1, 2, \cdots, M 
> \end{align}
> $$
> 可以看出，这就是**离散傅里叶变换 (discrete Fourier transform, DFT)** 的计算公式；此估计量的协方差矩阵为 $\boldsymbol{C}_{\hat{\v{\theta}}} = \sigma^{2} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} = \cfrac{2\sigma^{2}}{N} \boldsymbol{I}$。
> 

#### 线性模型的拓展

上述线性模型的求解方法也适用于以下两种情况：
1. **允许Gauss噪声有色**，即 $\v{w} \sim \mathcal{N}(\v{0}, \boldsymbol{C})$，其中 $\boldsymbol{C}$ 是一个 $N \times N$ 的协方差矩阵，此时 MVU 估计量为 $\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}$，协方差矩阵为 $\boldsymbol{C}_{\hat{\v{\theta}}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1}$。
2. **允许观测数据 $\v{x}$ 含有已知信号 $\v{s}$**，即 $\v{x} = \boldsymbol{H} \v{\theta} + \v{s} + \v{w}$，其中 $\v{s}$ 是一个已知的 $N$ 维信号，此时 MVU 估计量为 $\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} (\v{x} - \v{s})$，协方差矩阵为 $\boldsymbol{C}_{\hat{\v{\theta}}} = \sigma^{2} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1}$。

> [!theorem] 线性模型的MVU估计量
> 
> 对于线性模型 $\v{x} = \boldsymbol{H} \v{\theta} + \v{s} + \v{w}$，其中 $\v{w} \sim \mathcal{N}(\v{0}, \boldsymbol{C})$，待估计参数 **$\v{\theta}$ 的MVU估计量**为
> $$
> \hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} (\v{x} - \v{s})
> $$
> 且为**有效估计量**；其**协方差矩阵**为
> $$
> \boldsymbol{C}_{\hat{\v{\theta}}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1}
> $$

### 充分统计量方法

#### 充分统计量及其完备性

直观地，包含原始测量数据有关待估计参数所有信息的统计量称为**充分统计量 (sufficient statistic)**。例如，对[[#^Example-2-1|电平估计问题]]，$T(\v{x}) = \sum\limits_{n=0}^{N-1} x[n]$ 就是一个充分统计量，可以直接由 $T(\v{x})$ 求出 $A$ 的 MVU 估计量 $\hat{A} = \cfrac{T(\v{x})}{N}$。

考虑**充分统计量对似然函数的作用**，研究
$$
p\left( \v{x} \mid T(\v{x}) = T_{0} ;\, A \right) = \frac{p\left( \v{x}, T(\v{x}) - T_{0} ;\, A \right)}{p\left( T(\v{x}) = T_{0} ;\, A \right)}
= \frac{p\left( \v{x} ; A \right) \delta(T(\v{x}) = T_{0})}{p\left( T(\v{x}) = T_{0} ;\, A \right)}
$$
在[[#^Example-2-1|上例]]中，分子为
$$
\begin{align}
& \begin{aligned}
p(\v{x}; A) &= \prod_{n=0}^{N-1} \frac{1}{\sqrt{2\pi\sigma^{2}}} \exp \left( -\frac{(x[n] - A)^{2}}{2\sigma^{2}} \right) \\
&= \frac{1}{(2\pi\sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right) \\
&= \frac{1}{(2\pi\sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \left( \sum_{n=0}^{N-1} x[n]^{2} - 2A \sum_{n=0}^{N-1} x[n] + N A^{2} \right) \right)
\end{aligned} \\
& \begin{aligned} 
\implies p(\v{x}; A) \delta(T(\v{x}) = T_{0}) &= \frac{1}{(2\pi\sigma^{2})^{N/2}} \delta(T(\v{x}) = T_{0}) \\
&\hspace{1em} \cdot\exp \left( -\frac{1}{2\sigma^{2}} \left( \sum_{n=0}^{N-1} x[n]^{2} - 2A T_{0} + N A^{2} \right) \right)  
\end{aligned}
\end{align}
$$
分母上，由 $T(\v{x}) = \sum\limits_{n=0}^{N-1} x[n]$ 服从 Gauss 分布 $\mathcal{N}(N A, N \sigma^{2})$，有
$$
\begin{align}
p\left( T(\v{x}) = T_{0} ;\, A \right) &= \frac{1}{\sqrt{2\pi N \sigma^{2}}} \exp \left( -\frac{(T_{0} - N A)^{2}}{2 N \sigma^{2}} \right) \\
&= \frac{1}{\sqrt{2\pi N \sigma^{2}}} \exp \left( -\frac{1}{2\sigma^{2}} \left( \frac{T_{0}^{2}}{N} - 2A T_{0} + N A^{2} \right) \right)
\end{align}
$$
因此
$$
\begin{align}
&p\left( \v{x} \mid T(\v{x}) = T_{0} ;\, A \right) = \frac{p\left( \v{x}; A \right) \delta(T(\v{x}) = T_{0})}{p\left( T(\v{x}) = T_{0} ;\, A \right)} \\
&= \frac{\cfrac{1}{(2\pi\sigma^{2})^{N/2}} \delta(T(\v{x}) = T_{0}) \exp \left( -\cfrac{1}{2\sigma^{2}} \left( \sum\limits_{n=0}^{N-1} x[n]^{2} - 2A T_{0} + N A^{2} \right) \right)}{\cfrac{1}{\sqrt{2\pi N \sigma^{2}}} \exp \left( -\cfrac{1}{2\sigma^{2}} \left( \cfrac{T_{0}^{2}}{N} - 2A T_{0} + N A^{2} \right) \right)} \\
&= \frac{\sqrt{ N }}{(2\pi \sigma^{2})^{(N-1)/2}} \exp \left( -\frac{1}{2\sigma^{2}} \left( \sum_{n=0}^{N-1} x[n]^{2} - \frac{T_{0}^{2}}{N} \right) \right) \delta(T(\v{x}) = T_{0}) \\
&= p\left( \v{x} \mid T(\v{x}) = T_{0} \right)
\end{align}
$$
此时**似然函数与待估计参数无关**，这是「观测数据有关待估计参数所有信息已包含在充分统计量中」的数学表述。

> [!definition] 充分统计量
> 若对于任意观测数据 $\v{x}$ 和参数 $\theta$，统计量 $T(\v{x})$ 满足
> $$
> p\left( \v{x} \mid T(\v{x}) ;\, \theta \right) = p\left( \v{x} \mid T(\v{x}) \right)
> $$
> 则称 $T(\v{x})$ 是 $\theta$ 的一个**充分统计量**。

充分统计量有以下性质：
+ 一旦充分统计量确定，**似然函数就与待估计参数无关**；反过来，**充分统计量特定于待估计参数**，待估计参数变化时相应的充分统计量一般也会变化；
+ 所谓「充分」是相对于原始观测数据而言的，**原始观测量总是充分统计量**，但通常不是最小集，充分统计量并不唯一。

> [!definition] 充分统计量的完备性
> 若对 $\theta$ 的充分统计量 $T$，方程
> $$
> \dint_{-\infty}^{\infty} \nu(T) p(T; \theta) \dif T = 0 
> $$
> 在任意 $\theta$ 下都只对 $\nu(T) = 0$ 成立，则称 $T$ 是 $\theta$ 的一个**完备 (complete)** 的充分统计量。

所谓「完备」，是指充分统计量的分布族中不存在非零函数 $\nu(\cdot)$ 与该分布族中的每个分布都正交，即不存在非零函数 $\nu(\cdot)$ 满足 $\mathbb{E} \left[ \nu(T) \right] = 0$ 对任意 $\theta$ 都成立。这样，**满足无偏性约束 $\mathbb{E} \left[ \hat{\theta} \right] = \theta$ 的估计量 $\hat{\theta} = g(T)$ 只有至多唯一的解**，若存在，则该解就是 MVU 估计量。

#### 利用RBLS定理求MVU估计量

> [!theorem] {Neyman|内曼}-{Fisher|费舍尔} 因子分解定理
> 若观测数据 $\v{x}$ 的概率密度函数 $p(\v{x}; \theta)$ 可以分解为
> $$
> p(\v{x}; \theta) = g\left( T(\v{x}), \theta \right) h(\v{x})
> $$
> 其中 $g(\cdot, \cdot)$ 是只通过 $T(\v{x})$ 才与 $\v{x}$ 有关的函数，$h(\cdot)$ 只是与 $\v{x}$ 有关的函数，则 **$T(\v{x})$ 是 $\theta$ 的一个充分统计量**。
> 
> 反之，若 $T(\v{x})$ 是 $\theta$ 的一个充分统计量，则 $p(\v{x}; \theta)$ 可以分解为上述形式。

> [!theorem] {Rao|拉奥}-{Black|布莱克}-{Lehmann|莱曼}-{Scheffé|谢费} (RBLS) 定理
> 若有 $\check{\theta}$ 是 $\theta$ 的一个无偏估计量，$T(\v{x})$ 是 $\theta$ 的一个充分统计量，则估计量
> $$
> \hat{\theta} = \mathbb{E} \left[ \check{\theta} \mid T(\v{x}) \right]
> $$
> 1. 是 $\theta$ 的一个**{适用|与真值无关}**的**无偏**估计量；
> 2. 对所有 $\theta$，都有 $\mathrm{var}(\hat{\theta}) \le \mathrm{var}(\check{\theta})$；
> 3. 若 $T(\v{x})$ 是 $\theta$ 的一个**完备**的充分统计量，则 $\hat{\theta}$ 是 $\theta$ 的一个**MVU 估计量**。

利用RBLS定理求解MVU估计量的步骤如下：
1. 利用Neyman-Fisher因子分解定理求出充分统计量 $T(\v{x})$；
2. 检查 $T(\v{x})$ 是否**完备**；
3. 若完备，则**任选一个无偏估计量 $\check{\theta}$ 计算 $\hat{\theta} = \mathbb{E} \left[ \check{\theta} \mathop{\Big|} T(\v{x}) \right]$**，或**直接{找到|凑出}无偏函数 $\hat{\theta} = f(T(\v{x}))$**，即得到MVU 估计量 $\hat{\theta}$。