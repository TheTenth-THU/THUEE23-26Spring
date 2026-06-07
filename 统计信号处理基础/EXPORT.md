# 经典参数估计

## 估计理论分类

### 经典估计

**经典估计**假设**真值 $A$ 是一个固定的常数**，测量值 $x[n]$ 是 $A$ 加上一个随机误差 $w[n]$ 的结果，即 $x[n] = A + w[n]$，其中 $w[n]$ 是一个零均值的随机变量，从而测量值服从**分布 $p(x;A)$**。

经典估计的目标是从测量值 $x[n]$ 中估计出真值 $A$。

### Bayes估计

**Bayes估计**假设**真值 $A$ 是一个随机变量**，测量值 $x[n]$ 是 $A$ 加上另一个随机误差 $w[n]$ 的结果，即 $x[n] = A + w[n]$，其中 $w[n]$ 是一个与 $A$ 独立的零均值随机变量，从而测量值服从**二元分布 $p(x,A)$**。

Bayes估计的目标是从测量值 $x[n]$ 中估计出真值 $A$ 的**后验分布 $p(A|x)$**，从而得到对 $A$ 的估计。

## 均方误差 (MSE) 准则

**均方误差** (mean square error, MSE) 定义为估计值 $\hat{\theta}$ 与真值 $\theta$ 之间的平方误差的期望值，即
$$
\mathrm{mse}(\hat{\theta}) = \mathbb{E}\left[(\hat{\theta} - \theta)^2\right]
$$

MSE 可以分解为**方差** (variance) 和**偏差** (bias) 的平方，即
$$
\begin{align}
\mathrm{mse}(\hat{\theta}) &= \mathbb{E} \left[(\hat{\theta} - \theta)^2\right] \\
&= \mathbb{E} \left[(\hat{\theta} - \mathbb{E}[\hat{\theta}] + \mathbb{E}[\hat{\theta}] - \theta)^2\right] \\
&= \mathbb{E} \left[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2 + (\mathbb{E}[\hat{\theta}] - \theta)^2 + 2(\hat{\theta} - \mathbb{E}[\hat{\theta}])(\mathbb{E}[\hat{\theta}] - \theta)\right]  \\
&= \underbrace{ \mathbb{E} \left[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2\right] }_{ \mathrm{var(\hat{\theta})} } + \underbrace{ (\mathbb{E}[\hat{\theta}] - \theta)^2 }_{ b^{2}(\theta) }
\end{align}
$$
其中 $\mathrm{var}(\hat{\theta})$ 是估计值的方差，$b(\theta)$ 是估计值的偏差。

由于 $\mathrm{mse}(\hat{\theta})$ 包含了方差和偏差两部分，**与真值直接相关**，因此 $\min\left\{ \mathrm{mse(\hat{\theta})} \right\}$ 一般**不可实现**。

## 经典参数估计方法概述

MSE 一般不可实现，因此需要引入一定的**假设**来简化问题，从而得到可实现的估计方法。

假定要求**估计量无偏**，即 $\mathbb{E}[\hat{\theta}] = \theta$，则 $\mathrm{mse}(\hat{\theta})$ 退化为方差 $\mathrm{var}(\hat{\theta})$，由此MSE得到[[#最小方差无偏 (MVU) 估计]]。特别地，若进一步假设**估计量对观测数据呈线性关系**，则得到[[#最佳线性无偏估计 (BLUE)]]。

假定要求**估计量使似然函数最大**，依赖于 $p(\v{x};\v{\theta})$ 已知，则得到[[#最大似然估计 (MLE)]]。当达到CRLB的MVU估计量存在时，MLE可以求得该有效统计量；即使不存在达到CRLB的MVU估计量，MLE也具有**渐近有效**的性质。

假定要求**观测数据满足一定的信号模型**，则得到[[#最小二乘估计 (LSE)]]。



# 最小方差无偏 (MVU) 估计

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
> 由于MSE包含了方差和偏差两部分，而MVU直接要求无偏，因此在所有无偏估计中，MVU的MSE就等于它的方差，因此MVU是**在所有无偏估计中具有最小MSE** 的估计。

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

> [!example]- 计算估计的CRLB：示例 ^Example-2-1
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


#### 使用 CRLB 求解有效估计量

CRLB 还可用于求解 MVU 估计量，将对数似然函数的导数表达为 $\cfrac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } = \mathcal{I}(\theta) \left( g\left( \v{x} \right) - \theta \right)$ 的形式，即得到有效估计量 $\hat{\theta} = g\left( \v{x} \right)$ 及其方差 $\mathrm{var}(\hat{\theta}) = \cfrac{1}{\mathcal{I}(\theta)}$。注意：
+ $\mathcal{I}(\theta)$ 是 $\theta$ 的函数，**不能与观测数据 $\v{x}$ 有关**。
+ $g(\v{x})$ 是 $\v{x}$ 的函数，**不能与待估计参数 $\theta$ 有关**。

> [!example]- 使用CRLB求解MVU：示例 ^Example-MVU-CRLB
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
	&\mathbb{E} \left[ \hat{\alpha} \right] = a\mathbb{E} \left[ \hat{\theta} \right] + b = a\theta + b = \alpha \\
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

上述[[#使用 CRLB 求解有效估计量]]的方法是通过 CRLB 来求解 MVU 估计量的一种方法。然而，其求出的一定是**有效估计量**，但很多情形下 **MVU不一定达到CRLB**，因此不能由该方法求解出 MVU 估计量。

```tx
| 方法 | 适用情况 ||
| ^^ | 有效估计量 | 非有效估计量 |
| :--- | :--- | :--- |
| CRLB方法 | 若存在，一定可求解，须构造 $\frac{ \partial \ln p\left( \v{x};\theta \right) }{ \partial \theta } = \mathcal{I}(\theta) \left( g\left( \v{x} \right) - \theta \right)$ | 不适用 |
| **[[#特例方法：线性模型方法\|线性模型方法]]** | 对于线性测量模型，若存在，一定可求解，且可直接套用公式 | 不适用 |
| **[[#一般方法：充分统计量方法\|充分统计量方法]]** | 若存在，可能可求解 | 可能可求解 |
```

### 特例方法：线性模型方法

#### 简单线性模型方法

> [!definition] 线性模型 ^Linear-Model
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
\mark{ \hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x} }
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
1. **允许Gauss噪声有色**，即 $\v{w} \sim \mathcal{N}(\v{0}, \boldsymbol{C})$，其中 $\boldsymbol{C}$ 是一个 $N \times N$ 的协方差矩阵。此时 MVU 估计量为 $\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}$，协方差矩阵为 $\boldsymbol{C}_{\hat{\v{\theta}}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1}$。
2. **允许观测数据 $\v{x}$ 含有已知信号 $\v{s}$**，即 $\v{x} = \boldsymbol{H} \v{\theta} + \v{s} + \v{w}$，其中 $\v{s}$ 是一个已知的 $N$ 维信号。此时 MVU 估计量为 $\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} (\v{x} - \v{s})$，协方差矩阵为 $\boldsymbol{C}_{\hat{\v{\theta}}} = \sigma^{2} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1}$。

> [!theorem] 线性模型的MVU估计量 ^Linear-Model-MVU-Estimator
> 
> 对于线性模型 $\v{x} = \boldsymbol{H} \v{\theta} + \v{s} + \v{w}$，其中 $\v{w} \sim \mathcal{N}(\v{0}, \boldsymbol{C})$，待估计参数 **$\v{\theta}$ 的MVU估计量**为
> $$
> \hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} (\v{x} - \v{s})
> $$
> 且为**有效估计量**；其**协方差矩阵**为
> $$
> \boldsymbol{C}_{\hat{\v{\theta}}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1}
> $$

### 一般方法：充分统计量方法

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
> \mathbb{E} \left[ \nu(T) \right] = \dint_{-\infty}^{\infty} \nu(T) p(T; \theta) \dif T = 0 
> $$
> 在任意 $\theta$ 下都只对 $\nu(T) = 0$ 成立，则称 $T$ 是 $\theta$ 的一个**完备 (complete)** 的充分统计量。

所谓「完备」，是指充分统计量的分布族中不存在非零函数 $\nu(\cdot)$ 与该分布族中的每个分布都正交，即不存在非零函数 $\nu(\cdot)$ 满足 $\mathbb{E} \left[ \nu(T) \right] = 0$ 对任意 $\theta$ 都成立。这样，**满足无偏性约束 $\mathbb{E} \left[ \hat{\theta} \right] = \theta$ 的估计量 $\hat{\theta} = g(T)$ 只有至多唯一的解**，若存在，则该解就是 MVU 估计量。

对于多个充分统计量（对应于多个待估计参数）的情况，完备性要求
$$
\mathbb{E} \left[ \nu(\v{T}) \right] = \idotsint_{-\infty}^{\infty} \nu(\v{T}) p_{T_{n} \mid T_{(1:n-1)}}(T_{n}; \v{\theta}) \dif T_{n} \cdots p_{T_{1}}(T_{1}; \v{\theta}) \dif T_{1} = 0 \iff \nu(\v{T}) \equiv 0, \qquad \forall \v{\theta}
$$

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
> \hat{\theta} = \mathbb{E} \left[ \check{\theta} \mid[\Big] T(\v{x}) \right]
> $$
> 1. 是 $\theta$ 的一个**{适用|与真值无关}**的**无偏**估计量；
> 2. 对所有 $\theta$，都有 $\mathrm{var}(\hat{\theta}) \le \mathrm{var}(\check{\theta})$；
> 3. 若 $T(\v{x})$ 是 $\theta$ 的一个**完备**的充分统计量，则 $\hat{\theta}$ 是 $\theta$ 的一个 **MVU 估计量**。

利用RBLS定理求解MVU估计量的步骤如下：
1. 利用Neyman-Fisher因子分解定理求出充分统计量 $T(\v{x})$；
2. 检查 $T(\v{x})$ 是否**完备**；
3. 若完备，则**任选一个无偏估计量 $\check{\theta}$ 计算 $\hat{\theta} = \mathbb{E} \left[ \check{\theta} \mathop{\Big|} T(\v{x}) \right]$**，或**直接{找到|凑出}无偏函数 $\hat{\theta} = f(T(\v{x}))$**，即得到MVU 估计量 $\hat{\theta}$。

> [!example] 使用充分统计量方法求解MVU：示例
> 
> **提取已知频率的正弦分量。**
> 假设一个已知频率 $f_{0}$ 的正弦分量被埋在 Gauss 白噪声中，即测量系统为
> $$
> x[n] = A \cos(2\pi f_{0} n) + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 其中 $w[n]$ 是一个均值为0、方差为 $\sigma^{2}$ 的 Gauss 白噪声，求振幅 $A$ 和噪声方差 $\sigma^{2}$ 的 MVU 估计量。
> 
> ---
> 
> 若 $\sigma^2$ 已知，则这一估计问题能比较容易地通过[[#特例方法：线性模型方法|线性模型]]求解，但很遗憾 $\sigma^2$ 是未知的，只好尝试充分统计量方法。
> 
> **1. 利用Neyman-Fisher因子分解定理求出充分统计量。**
> 
> $\v{x}$ 的 PDF 由 $A$ 和 $\sigma^2$ 两个参数决定
> $$
> \begin{align} 
> p\left( \v{x}; A, \sigma^{2} \right) &= \prod_{n=0}^{N-1} \frac{1}{\sqrt{2\pi\sigma^{2}}} \exp \left( -\frac{(x[n] - A \cos(2\pi f_{0} n))^{2}}{2\sigma^{2}} \right) \\
> &= \frac{1}{(2\pi\sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A \cos(2\pi f_{0} n))^{2} \right) \\
> &= \frac{1}{(2\pi\sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum\limits_{n=0}^{N-1} x^{2}[n] + \frac{A}{\sigma^{2}} \sum\limits_{n=0}^{N-1} x[n] \cos(2\pi f_{0} n) - \frac{A^{2}}{2\sigma^{2}} \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) \right)
> \end{align}
> $$
> 利用Neyman-Fisher因子分解定理，可以得到**充分统计量**为
> $$
> T_{1}(\v{x}) = \sum\limits_{n=0}^{N-1} x[n] \cos(2\pi f_{0} n), \qquad
> T_{2}(\v{x}) = \sum\limits_{n=0}^{N-1} x^{2}[n]
> $$
> 
> **2. 检查充分统计量的完备性。**
> 
> 根据**完备性**的定义，考察方程
> $$
> \dint_{-\infty}^{\infty} \left( \dint_{-\infty}^{\infty} \nu(T_{1}, T_{2}) p_{T_{2}  \mid T_{1}} (T_{2} ; \theta) \dif T_{2} \right) p_{T_{1}}(T_{1}; \theta) \dif T_{1} = 0, \qquad \forall \theta
> $$
> 注意到 **$T_{1}(\v{x})$ 仍为Gauss分布的随机变量**，$p_{T_{1}}(T_{1}; \theta) > 0$，因此须内层积分
> $$
> \dint_{-\infty}^{\infty} \nu(T_{1}, T_{2}) p_{T_{2}  \mid T_{1}} (T_{2} ; \theta) \dif T_{2} = 0, \qquad \forall T_{1}, \theta
> $$
> 而 $T_{2} \mid T_{1}$ 服从一个**非中心 $\chi_{v}^{2}(\lambda)$ 分布**，同样有 $p_{T_{2}  \mid T_{1}} (T_{2} ; \theta) > 0$，因此须 $\nu(T_{1}, T_{2}) = 0$，即 $T_{1}(\v{x})$ 和 $T_{2}(\v{x})$ 是**完备的充分统计量**。
> 
> **3. 构造无偏函数作为估计量。**
> 
> 进而，根据RBLS定理，只需凑出一个无偏估计量 $\hat{\theta} = f(T_{1}, T_{2})$，即得到MVU估计量。为此，先考察两估计量的一阶矩
> $$
> \begin{align}
> & \mathbb{E} \left[ T_{1}(\v{x}) \right] = \sum\limits_{n=0}^{N-1} \mathbb{E} \left[ x[n] \right] \cos(2\pi f_{0} n) = A \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) \\
> & \mathbb{E} \left[ T_{2}(\v{x}) \right] = \sum\limits_{n=0}^{N-1} \mathbb{E} \left[ x^{2}[n] \right] = \sum\limits_{n=0}^{N-1} \left( \mathrm{var}(x[n]) + (\mathbb{E} [x[n]])^{2} \right) 
> = N \sigma^{2} + A^{2} \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n)
> \end{align}
> $$
> 注意到对于 $A$ 可以直接凑出无偏函数，即其MVU估计量为
> $$
> \hat{A} = \frac{T_{1}(\v{x})}{\sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n)} = \frac{\sum\limits_{n=0}^{N-1} x[n] \cos(2\pi f_{0} n)}{\sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n)}
> $$
> 但对于 $\sigma^{2}$ 则**无法无偏地消去 $A^{2}$** 直接凑出无偏函数，因此考虑再引入含有 $A^{2}$ 的二阶矩
> $$
> \mathbb{E} \left[ T_{1}^{2}(\v{x}) \right] = \mathrm{var}(T_{1}(\v{x})) + (\mathbb{E} [T_{1}(\v{x})])^{2} = \sigma^{2} \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) + A^{2} \left( \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) \right)^{2}
> $$
> 从而可以消去 $A^{2}$，凑出
> $$
> \begin{align}
> & \mathbb{E} \left[ T_{1}^{2}(\v{x}) \right] \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) - \mathbb{E} \left[ T_{2}(\v{x}) \right] \left( \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) \right)^{2} \\
> &= \sigma^{2} \left( \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) \right)^{2} - N \sigma^{2} \left( \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) \right)^{2} \\
> &= - (N-1) \left( \sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n) \right)^{2} \cdot \sigma^{2}
> \end{align}
> $$
> 即 $\sigma^{2}$ 的 MVU 估计量为
> $$
> \hat{\sigma^{2}} = \frac{1}{N-1} \left( T_{2}(\v{x}) - \frac{T_{1}^{2}(\v{x})}{\sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n)} \right) = \frac{1}{N-1} \left( \sum\limits_{n=0}^{N-1} x^{2}[n] - \frac{\left( \sum\limits_{n=0}^{N-1} x[n] \cos(2\pi f_{0} n) \right)^{2}}{\sum\limits_{n=0}^{N-1} \cos^{2}(2\pi f_{0} n)} \right)
> $$
> 






# 最佳线性无偏估计 (BLUE)

## BLUE的含义

> [!definition] 最佳线性无偏估计 (BLUE)
> **最佳线性无偏估计 (best linear unbiased estimation, BLUE)** 是指在所有**线性无偏估计量**中，具有最小方差的估计量。

具体地，BLUE要求
1. 估计量 $\hat{\theta}$ 对观察数据 $\v{x}$ 是**线性**的，即 $\hat{\theta} = \v{a}^{\mathrm{T}} \v{x}$，其中 $\v{a}$ 是一个 $N$ 维的权重向量；
2. 估计量 $\hat{\theta}$ 是**无偏**的，即 $\mathbb{E} \left[ \hat{\theta} \right] = \sum\limits_{n=0}^{N-1} a_{n} \mathbb{E} \left[ x[n] \right] = \theta$，若设 $\mathbb{E} \left[ x[n] \right] = s[n] \theta$，则无偏性约束为 $\sum\limits_{n=0}^{N-1} a_{n} s[n] = \v{a}^{\mathrm{T}} \v{s} = 1$；
3. 估计量 $\hat{\theta}$ 的**方差最小**，有
    $$
    \begin{align}
    \mathrm{var} (\hat{\theta}) &= \mathbb{E} \left[ (\v{a}^{\mathrm{T}} \v{x} - \v{a}^{\mathrm{T}} \mathbb{E} \left[ \v{x} \right] )^{2} \right]  \\
    &= \mathbb{E} \left[ \v{a}^{\mathrm{T}} (\v{x} - \mathbb{E} \left[ \v{x} \right]) (\v{x} - \mathbb{E} \left[ \v{x} \right])^{\mathrm{T}} \v{a} \right] = \v{a}^{\mathrm{T}} \boldsymbol{C} \v{a}
    \end{align}
    $$
    其中 $\boldsymbol{C}$ 是观测数据 $\v{x}$ 的协方差矩阵，即要求 $\v{a}$ 使 **$\v{a}^{\mathrm{T}} \boldsymbol{C} \v{a}$ 最小**。

## BLUE的求解

综上，BLUE的求解问题可以转化为**约束优化问题**
$$
\hat{\theta} = \left( \arg\min_{\v{a}} \v{a}^{\mathrm{T}} \boldsymbol{C} \v{a} \right)^{\mathrm{T}} \v{x} \quad \text{s.t.} \quad \v{a}^{\mathrm{T}} \v{s} = 1
$$
采用 **Lagrange 乘子法**，构造Lagrange函数
$$
\mathcal{L}(\v{a}, \lambda) = \v{a}^{\mathrm{T}} \boldsymbol{C} \v{a} + \lambda (\v{a}^{\mathrm{T}} \v{s} - 1)
$$
对 $\v{a}$ 求偏导数并令其为零，得到
$$
\left.\begin{array}{r}
\cfrac{ \partial \mathcal{L} }{ \partial \v{a} } = 2 \boldsymbol{C} \v{a} + \lambda \v{s} = \v{0} \implies \v{a} = -\cfrac{\lambda}{2} \boldsymbol{C}^{-1} \v{s} \\
\v{a}^{\mathrm{T}} \v{s} = 1 
\end{array}\right\} \implies \begin{cases}
\lambda = -\cfrac{2}{\v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s}} \\
\v{a}_{\mathrm{opt}} = \cfrac{\boldsymbol{C}^{-1} \v{s}}{\v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s}}
\end{cases}
$$
进而得到BLUE估计量为
$$
\mark{ \hat{\theta} = \v{a}_{\mathrm{opt}}^{\mathrm{T}} \v{x} = \frac{\v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}}{\v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s}} }
$$
其方差为
$$
\mathrm{var} (\hat{\theta}) = \v{a}_{\mathrm{opt}}^{\mathrm{T}} \boldsymbol{C} \v{a}_{\mathrm{opt}} = \frac{1}{\v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s}}
$$

## 矢量参数BLUE

将上述结果推广到矢量参数 $\v{\theta}$ 的情况，限定每个估计量与观测数据呈线性关系
$$
\hat{\theta}_{i} = \sum\limits_{n=0}^{N-1} a_{i,n} x[n] = \v{a}_{i}^{\mathrm{T}} \v{x}, \qquad i = 1, 2, \cdots, M
$$
且给定观测数据的期望为 $\mathbb{E} \left[ \v{x} \right] = \boldsymbol{H} \v{\theta}$，$\boldsymbol{H} = \Big( \v{h}_{j} \Big)_{j}$，协方差矩阵为 $\boldsymbol{C}$，则BLUE的求解即
$$
\hat{\v{\theta}} = \left( \Big( \arg\min_{\v{a}_{i}} \v{a}_{i}^{\mathrm{T}} \boldsymbol{C} \v{a}_{i} \Big)_{i} \right)^{\mathrm{T}} \v{x} \quad \text{s.t.} \quad \v{a}_{i}^{\mathrm{T}} \v{h}_{j} = \delta_{ij}
$$
解出
$$
\hat{\v{\theta}} = \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}
$$
其**各个分量的方差**（注意未给出协方差）为
$$
\mathrm{var} (\hat{\theta}_{i}) = \left( \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1} \right) _{i,i}
$$

注意到，**线性模型的MVU估计量**与**矢量参数BLUE**的表达式基本相同。事实上，有以下定理：

> [!theorem] {Gauss|高斯}-{Markov|马尔可夫} 定理 ^Gauss-Markov-Theorem
> 如果数据具有**一般线性模型**的形式 
> $$
> \v{x} = \boldsymbol{H} \v{\theta} + \v{w}
> $$
> 其中 $\boldsymbol{H}$ 是一个 $N \times p$ 的已知矩阵，$\v{\theta}$ 是一个 $p$ 维的待估计参数，$\v{w}$ 是一个零均值、协方差矩阵为 $\boldsymbol{C}$ 的{随机|不一定为 Gauss}噪声，则 $\v{\theta}$ 的**BLUE估计量**是
> $$
> \hat{\v{\theta}} = \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}
> $$
> 其**协方差矩阵**为 $\boldsymbol{C}_{\hat{\v{\theta}}} = \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1}$，即每个分量的方差为 $\mathrm{var} (\hat{\theta}_{i}) = \left( \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1} \right) _{i,i}$。
> 

特别地，若 $\v{w}$ 为Gauss噪声，则上述 **BLUE估计量也是 $\v{\theta}$ 的MVU估计量**。



# 最大似然估计 (MLE)

## MLE的含义

对观测数据 $\v{x}$，将使得 $p(\v{x}; \theta)$ 达到最大值的参数 $\theta$ 作为估计量，称为 $\theta$ 的**最大似然估计 (maximum likelihood estimation, MLE)**。

最大似然估计直接给出了估计量
$$
\mark{ \hat{\theta} = \arg\max\limits_{\theta} p(\v{x}; \theta) }
$$

> [!example] 求解MLE估计量：示例 ^Example-1
> 
> **白噪声中电平估计问题。**
> 考虑测量系统
> $$
> x[n] = A + w[n], \qquad n = 0,1,\dots,N-1
> $$
> 其中待估计参数为信号幅度 $A$，且同时为 Gauss 白噪声 $w[n]$ 的方差，即 $w[n] \sim \mathcal{N}(0, A)$。
> 
> ---
> 
> 使用MLE方法求解 $A$ 的估计量 $\hat{A}$，首先写出似然函数
> $$
> \begin{align}
> & \begin{aligned} 
> p(\v{x}; A) &= \prod_{n=0}^{N-1} \frac{1}{\sqrt{2\pi A}} \exp \left( -\frac{(x[n] - A)^{2}}{2A} \right)  \\
> &= \frac{1}{(2\pi A)^{N/2}} \exp \left( -\frac{1}{2A} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right) 
> \end{aligned} \\
> & \implies \ln p(\v{x}; A) = -\frac{N}{2} \ln (2\pi A) - \frac{1}{2A} \sum_{n=0}^{N-1} (x[n] - A)^{2} \\
> & \implies \frac{\partial \ln p(\v{x}; A)}{\partial A} = -\frac{N}{2A} + \frac{1}{2A^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} + \frac{1}{A} \sum_{n=0}^{N-1} (x[n] - A)
> \end{align}
> $$
> 令 $\cfrac{\partial \ln p(\v{x}; A)}{\partial A} \Bigg|_{\hat{A}} = 0$，得到 $\hat{A}^{2} + \hat{A} - \cfrac{1}{N} \sum\limits_{n=0}^{N-1} x^{2}[n] = 0$，解得
> $$
> \hat{A} = \frac{-1 + \sqrt{1 + 4 \cdot \cfrac{1}{N} \sum\limits_{n=0}^{N-1} x^{2}[n]}}{2} = - \frac{1}{2} + \sqrt{\frac{1}{4} + \frac{1}{N} \sum_{n=0}^{N-1} x^{2}[n]}
> $$

### MLE与MVU的关系

假定似然函数可导，则MLE估计量 $\hat{\theta}$ 满足
$$
\left. \frac{\partial \ln p(\v{x}; \theta)}{\partial \theta} \right|_{\theta = \hat{\theta}} = 0
$$
如果存在一个有效统计量 $g(\v{x})$，则由 [[#最小方差无偏 (MVU) 估计#标量参数 CRLB 定理|CRLB定理]]知 $g(\v{x})$ 满足
$$
\frac{\partial \ln p(\v{x}; \theta)}{\partial \theta} = \mathcal{I}(\theta) (g(\v{x}) - \theta)
$$
因此 $g(\v{x})$ 也是MLE估计量，即**当存在达到CRLB的MVU估计量时，MLE可以求得该有效统计量**。

### MLE的参数变换不变性

> [!theorem] MLE的参数变换不变性
> 若参数 $\alpha = g(\theta)$，则 $\alpha$ 的MLE **$\hat{\alpha} = g(\hat{\theta})$**，其中 $\hat{\theta}$ 是 $\theta$ 的MLE。
> 
> 若 $g$ 非单射，那么 $\hat{\alpha}$ 是使综合修正的似然函数最大的参数，即
> $$
> \hat{\alpha} = \arg\max_{\alpha} p_{\mathrm{T}}(\v{x}; \alpha) = \arg\max_{\alpha} \max_{\theta \in g^{-1}(\alpha)} p(\v{x}; \theta)
> $$

不同于CRLB参数变换中只有线性变换保持无偏性和有效性，MLE的参数变换不变性保证了**任何单射变换**后仍是MLE估计量。

> [!example] MLE的参数变换：示例
> **信噪比估计。**
> 假设有 $N$ 个独立同分布的观测样本，服从均值为 $A$、方差为 $\sigma^{2}$ 的正态分布，二者均为未知参数，求信噪比 $\mathrm{SNR} = \cfrac{A^{2}}{\sigma^{2}}$ 的MLE估计量 $\widehat{\mathrm{SNR}}$。
> 
> ---
> 
> 首先写出似然函数
> $$
> \begin{align}
> & p(\v{x}; A, \sigma^{2}) = \prod_{n=0}^{N-1} \frac{1}{\sqrt{2\pi \sigma^{2}}} \exp \left( -\frac{(x[n] - A)^{2}}{2\sigma^{2}} \right) 
> = \frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right) \\
> & \ln p(\v{x}; A, \sigma^{2}) = -\frac{N}{2} \ln (2\pi \sigma^{2}) - \frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2}
> \end{align}
> $$
> $A$ 的估计量 $\hat{A}$ 满足 
> $$
> \frac{ \partial \ln p(\v{x}; A, \sigma^{2}) }{ \partial A } = \frac{1}{\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A) = 0 \implies \hat{A} = \frac{1}{N} \sum_{n=0}^{N-1} x[n] = \bar{x}
> $$
> $\sigma^{2}$ 的估计量 $\hat{\sigma}^{2}$ 满足
> $$
> \frac{ \partial \ln p(\v{x}; A, \sigma^{2}) }{ \partial \sigma^{2} } = -\frac{N}{2\sigma^{2}} + \frac{1}{2\sigma^{4}} \sum_{n=0}^{N-1} (x[n] - A)^{2} = 0 \implies \hat{\sigma}^{2} = \frac{1}{N} \sum_{n=0}^{N-1} (x[n] - \bar{x})^{2}
> $$
> 因此 $\mathrm{SNR}$ 的MLE估计量为
> $$
> \widehat{\mathrm{SNR}} = \frac{\hat{A}^{2}}{\hat{\sigma}^{2}} = \frac{\left( \frac{1}{N} \sum_{n=0}^{N-1} x[n] \right)^{2}}{\frac{1}{N} \sum_{n=0}^{N-1} (x[n] - \bar{x})^{2}}
> = \frac{N \bar{x}^{2}}{\sum_{n=0}^{N-1} (x[n] - \bar{x})^{2}}
> $$
> 

### 矢量参数MLE

当待估计参数是一个矢量 $\v{\theta}$ 时，MLE的定义同样适用，即
$$
\hat{\v{\theta}} = \arg\max_{\v{\theta}} p(\v{x}; \v{\theta})
$$
如果似然函数可导，则MLE估计量 $\hat{\v{\theta}}$ 满足
$$
\frac{\partial \ln p(\v{x}; \v{\theta})}{\partial \v{\theta}} \Bigg|_{\hat{\v{\theta}}}
= \begin{pmatrix}
\frac{\partial \ln p(\v{x}; \v{\theta})}{\partial \theta_{1}} \Bigg|_{\hat{\v{\theta}}} & 
\frac{\partial \ln p(\v{x}; \v{\theta})}{\partial \theta_{2}} \Bigg|_{\hat{\v{\theta}}} & \cdots & 
\frac{\partial \ln p(\v{x}; \v{\theta})}{\partial \theta_{p}} \Bigg|_{\hat{\v{\theta}}} 
\end{pmatrix}^{\mathrm{T}} = \v{0}
$$

特别地，对**一般线性模型** $\v{x} = \boldsymbol{H} \v{\theta} + \v{w}$，其中 $\v{w}$ 是零均值、协方差矩阵为 $\boldsymbol{C}$ 的Gauss噪声，有
$$
\frac{\partial \ln p(\v{x}; \v{\theta})}{\partial \v{\theta}} = \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} (\v{x} - \boldsymbol{H} \v{\theta})
= (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H}) \left( (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x} - \v{\theta} \right) 
$$
即得到MLE估计量为
$$
\mark{ \hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x} }
$$
其协方差为
$$
\boldsymbol{C}_{\hat{\v{\theta}}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1}
$$
这一结果与[[#最小方差无偏 (MVU) 估计#^Linear-Model-MVU-Estimator|线性模型的MVU估计量]]完全相同，因此**一般线性模型的MLE估计量也是达到CRLB的MVU估计量**。

## MLE的性能

检查上面[[#^Example-1|白噪声中电平估计问题]]一例中的MLE估计量 $\hat{A}$ 的性能，首先计算其期望
$$
\begin{align}
\mathbb{E} \left[ \hat{A} \right] &= -\frac{1}{2} + \mathbb{E} \left[ \sqrt{\frac{1}{4} + \frac{1}{N} \sum_{n=0}^{N-1} x^{2}[n]} \right] \\
&\; \begin{aligned}
\neq - \frac{1}{2} + \sqrt{\frac{1}{4} + \frac{1}{N} \sum_{n=0}^{N-1} \mathbb{E} \left[ x^{2}[n] \right]} &= -\frac{1}{2} + \sqrt{\frac{1}{4} + \mathbb{E} \left[ x^{2}[n] \right] } \\
&= -\frac{1}{2} + \sqrt{\frac{1}{4} + A^{2} + A} = A
\end{aligned}
\end{align}
$$
这是一个**有偏**估计；但在 $N \to \infty$ 时，可对 $\hat{A}$ 以 $u = \cfrac{1}{N} \sum\limits_{n=0}^{N-1} x^{2}[n]$ 为变量**线性化**，在 $\mathbb{E} \left[ u \right] = A + A^{2}$ 附近做 Taylor 展开
$$
\begin{align}
\hat{A} &= -\frac{1}{2} + \sqrt{u + \frac{1}{4}} \\
&\approx -\frac{1}{2} + \sqrt{A + A^{2} + \frac{1}{4}} + \left. \frac{\partial \hat{A}}{\partial u} \right|_{u = A + A^{2}} (u - A - A^{2}) \\
&= A + \frac{1}{2\left( A + \cfrac{1}{2} \right)} \left( \frac{1}{N} \sum_{n=0}^{N-1} x^{2}[n] - A - A^{2} \right) \xrightarrow{\mathbb{E}} A
\end{align}
$$
因此 $\hat{A}$ 是**渐近无偏**的。

进一步计算 $\hat{A}$ 的方差，可类似线性化后得到
$$
\begin{align}
\mathrm{var} (\hat{A}) &\approx \frac{1}{4\left( A + \cfrac{1}{2} \right)^{2}} \mathrm{var} \left(  \frac{1}{N} \sum_{n=0}^{N-1} x^{2}[n]  \right) = \frac{1}{4\left( A + \cfrac{1}{2} \right)^{2}} \cdot \frac{1}{N} \mathrm{var} (x^{2}[n]) \\
&= \frac{1}{4\left( A + \cfrac{1}{2} \right)^{2}} \cdot \frac{1}{N} (2A^{2} + 4A^{3}) = \frac{A^{2}}{N\left( A+\cfrac{1}{2} \right)}
\end{align}
$$
而CRLB为 $\mathrm{var}(\hat{A}) \ge \cfrac{1}{-\mathbb{E} \left[ \cfrac{ \partial ^{2} \ln p(\v{x};A) }{ \partial A^{2} } \right]} = \cfrac{A^{2}}{N\left( A+\cfrac{1}{2} \right)}$，因此 $\hat{A}$ 是**渐近有效**的。

> [!theorem] MLE 的渐近有效性
> 如果 $p(\v{x}; \theta)$ 满足一定正则条件：
> + $\mathbb{E} \left[ \frac{\partial \ln p}{\partial \theta} \right] = 0$，
> + $\mathbb{E} \left[ \frac{\partial^{2} \ln p}{\partial \theta^{2}} \right] = -\mathcal{I}(\theta)$，Fisher 信息量 $\mathcal{I}(\theta) > 0$ 且连续，
> + 存在函数 $M(\v{x})$ 满足 $\mathbb{E}[M(\v{x})] < \infty$，使得 $\left| \frac{\partial^{3} \ln p(\v{x}; \theta)}{\partial \theta^{3}} \right| \leq M(\v{x})$，
> 
> 则对足够多的数据记录，MLE估计量 $\hat{\theta}$ **渐近服从正态分布**
> $$
> \hat{\theta} \stackrel{\mathrm{a}}{\sim} \mathcal{N} \left( \theta, \frac{1}{\mathcal{I}(\theta)} \right)
> $$
> 其中 $\mathcal{I}(\theta)$ 是 Fisher 信息量。

这一性质给出了MLE估计量的渐近性能：
1. MLE是**渐近无偏**的，即 $\mathbb{E} \left[ \hat{\theta} \right] \xrightarrow{N \to \infty} \theta$；
2. MLE是**渐近有效**的，其方差**渐近达到CRLB**，即 $\mathrm{var}(\hat{\theta}) \xrightarrow{N \to \infty} \cfrac{1}{\mathcal{I}(\theta)}$，但有限样本下可大于、等于或小于CRLB。



# 最小二乘估计 (LSE)

## 最小二乘估计的含义

> [!definition] 最小二乘估计
> 已知信号模型 $s[n;\theta]$，假定观测数据 $x[n]$ 满足 $x[n] = s[n;\theta] + w[n]$，其中 $w[n]$ 是噪声，则 $\theta$ 的**最小二乘估计 (least squares estimation)** 为
> $$
> \hat{\theta} = \arg\min_{\theta} \left\{ \sum_{n=0}^{N-1} (x[n] - s[n;\theta])^{2} \right\} 
> $$
> 其中 $J(\theta) = \sum\limits_{n=0}^{N-1} (x[n] - s[n;\theta])^{2}$ 称为**最小二乘误差**。

与 [[#最小方差无偏 (MVU) 估计|MVU]] 和 [[#最大似然估计 (MLE)|MLE]] 相比，最小二乘估计没有对**观测数据的概率分布**做出任何假设，而是假定了**观测数据满足的信号模型**。

## 线性最小二乘估计

更经常地，最小二乘估计被用来求解**线性模型**中的参数，即假设信号模型 $\v{s}$ 与未知参数 $\v{\theta}$ 满足线性关系
$$
\v{s} = \boldsymbol{H} \v{\theta}
$$
其中 $\boldsymbol{H}$ 是列满秩的观测矩阵，相应的观测数据为 $\v{x} = \boldsymbol{H} \v{\theta} + \v{w}$，$\v{w}$ 是噪声。

### 线性最小二乘估计的简单求解

最小二乘误差为
$$
\begin{align}
J(\v{\theta}) &= \sum_{n=0}^{N-1} (x[n] - s[n;\v{\theta}])^{2} = \left\lVert \v{x} - \v{s} \right\rVert_{2}^{2} \\
&= (\v{x} - \boldsymbol{H}\v{\theta})^{\mathrm{T}} (\v{x} - \boldsymbol{H}\v{\theta}) = \v{x}^{\mathrm{T}} \v{x} - 2 \v{\theta}^{\mathrm{T}} \boldsymbol{H}^{\mathrm{T}} \v{x} + \v{\theta}^{\mathrm{T}} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{H} \v{\theta}
\end{align}
$$
因此，最小二乘估计量 $\hat{\v{\theta}}$ 满足
$$
\frac{ \partial J(\v{\theta}) }{ \partial \v{\theta} } \Bigg|_{\hat{\v{\theta}}} = - 2 \boldsymbol{H}^{\mathrm{T}} \v{x} + 2 \boldsymbol{H}^{\mathrm{T}} \boldsymbol{H} \hat{\v{\theta}} = \v{0} 
\implies 
\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x}
$$
相应的最小二乘误差为
$$
J(\hat{\v{\theta}}) = \v{x}^{\mathrm{T}} (\v{x} - \boldsymbol{H} \hat{\v{\theta}})
$$

另一方面，$\v{s} = \boldsymbol{H} \v{\theta} = \sum\limits_{i=1}^{p} \theta_{i} \v{h}_{i}$ 是 **$\boldsymbol{H}$ 的列空间**中的一个向量，要 $\left\lVert \v{x} - \v{s} \right\rVert_{2}^{2}$ 最小，$\v{s}$ 应当是 $\v{x}$ 在 $\boldsymbol{H}$ 的列空间中的**正交投影**，因此
$$
\begin{align} 
& \v{\varepsilon} = \v{x} - \v{s} \perp \v{h}_{i}, \quad \forall i = 1, 2, \cdots, p  \\
& \implies \boldsymbol{H}^{\mathrm{T}} (\v{x} - \boldsymbol{H} \hat{\v{\theta}}) = \v{0} 
\implies \hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x}
\end{align}
$$

### 加权最小二乘估计

当观测数据 $\v{x}$ 的不同分量具有不同的可靠性时，可以**对最小二乘误差加权**，即
$$
\begin{align} 
J(\v{\theta}) &= (\v{x} - \boldsymbol{H}\v{\theta})^{\mathrm{T}} \boldsymbol{W} (\v{x} - \boldsymbol{H}\v{\theta})  \\
&= \v{x}^{\mathrm{T}} \boldsymbol{W} \v{x} - \v{x}^{\mathrm{T}} \boldsymbol{W} \boldsymbol{H} \v{\theta} - \v{\theta}^{\mathrm{T}} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{W} \v{x} + \v{\theta}^{\mathrm{T}} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{W} \boldsymbol{H} \v{\theta}
\end{align}
$$
权系数 $\boldsymbol{W}$ 一般为对称矩阵。

最小二乘估计量 $\hat{\v{\theta}}$ 满足
$$
\begin{align} 
\frac{ \partial J(\v{\theta}) }{ \partial \v{\theta} } \Bigg|_{\hat{\v{\theta}}} = -\v{H}^{\mathrm{T}} \boldsymbol{W}^{\mathrm{T}} \v{x} - \boldsymbol{H}^{\mathrm{T}} \boldsymbol{W} \v{x} + 2 \boldsymbol{H}^{\mathrm{T}} \boldsymbol{W} \boldsymbol{H} \hat{\v{\theta}} = \v{0}  \\
\implies \hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{W} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{W} \v{x}
\end{align}
$$
相应的最小二乘误差为
$$
J(\hat{\v{\theta}}) = \v{x}^{\mathrm{T}} \boldsymbol{W} (\v{x} - \boldsymbol{H} \hat{\v{\theta}})
= \v{x}^{\mathrm{T}} \left( \boldsymbol{W} - \boldsymbol{W} \boldsymbol{H} (\boldsymbol{H}^{\mathrm{T}}\boldsymbol{W}\boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{W} \right)  \v{x}
$$

一种常见的加权最小二乘估计是**广义最小二乘估计 (generalized least squares estimation)**，即当观测数据 $\v{x}$ 的协方差矩阵为 $\boldsymbol{C}$ 时，取 $\boldsymbol{W} = \boldsymbol{C}^{-1}$，得到
$$
\mark{ \hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x} }
$$
这个结果与 [[#最小方差无偏 (MVU) 估计|MVU]]、[[#最佳线性无偏估计 (BLUE)|BLUE]]、[[#最大似然估计 (MLE)|MLE]] 对有色线性模型的估计结果及 [[#最佳线性无偏估计 (BLUE)#^Gauss-Markov-Theorem|Gauss-Markov 定理]]的结论均相同。

### 约束最小二乘估计

部分情形下，未知参数 $\v{\theta}$ 满足某些约束条件，如
$$
\boldsymbol{A} \v{\theta} = \v{b}
$$
此时的最小二乘误差应进一步构成 Lagrange 函数
$$
J_{\mathrm{c}}(\v{\theta}, \v{\lambda}) = (\v{x} - \boldsymbol{H}\v{\theta})^{\mathrm{T}} (\v{x} - \boldsymbol{H}\v{\theta}) + \v{\lambda}^{\mathrm{T}} (\boldsymbol{A} \v{\theta} - \v{b})
$$
约束最小二乘估计量 $\hat{\v{\theta}}_{\mathrm{c}}$ 满足
$$
\frac{ \partial J_{\mathrm{c}} }{ \partial \v{\theta} } \Bigg|_{\hat{\v{\theta}}_{\mathrm{c}}} = \v{0}
\implies 
\hat{\v{\theta}}_{\mathrm{c}} = \hat{\v{\theta}} - (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \underbrace{ \left( \boldsymbol{A} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \right)^{-1} (\boldsymbol{A} \hat{\v{\theta}} - \v{b}) }_{ \v{\lambda}/2 }
$$
其中 $\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x}$ 是无约束条件下的最小二乘估计量，约束估计量 $\hat{\v{\theta}}_{\mathrm{c}}$ 相当于在无约束估计量 $\hat{\v{\theta}}$ 的基础上，沿着 $\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H}$ 的逆矩阵作用下的 $\boldsymbol{A}^{\mathrm{T}}$ 方向进行修正，使得最终的估计量满足约束条件 $\boldsymbol{A} \hat{\v{\theta}}_{\mathrm{c}} = \v{b}$。

# Bayes估计

## Bayes风险

### 均方误差定义的Bayes风险

为了体现**真值 $\theta$ 范围对估计量选取的影响**，将待估计参数 $\theta$ 视为随机变量，并使用其分布加权经典均方误差，得到的加权均方误差称为 **Bayes均方误差 (Bayes mean square error, Bayes MSE)**，即
$$
\begin{align}
\mathrm{Bmse} (\hat{\theta}) &= \dint \mathbb{E} \left[ (\hat{\theta} - \theta)^{2} \right] p(\theta) \dif \theta
= \dint \left( \dint (\hat{\theta} - \theta)^{2} p(\v{x}; \theta) \dif \v{x} \right)_{\theta} p(\theta) \dif \theta \\
&= \iint (\hat{\theta} - \theta)^{2} p(\v{x} \mid \theta) p(\theta) \dif \v{x} \dif \theta 
= \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta
\end{align}
$$
最小化Bayes MSE得到 $\theta$ 的估计量，称为[[#最小均方误差 (MMSE) 估计]]。

Bayes MSE相当于**以 $C(\varepsilon) = \varepsilon^{2}$ 为代价函数**，对所有可能的 $\theta$ 和 $\hat{\theta}$ 的组合进行加权平均得到期望代价
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
称为 **Bayes风险 (Bayes risk)**。

### 绝对误差定义的Bayes风险

同样地，若以 $C(\varepsilon) = |\varepsilon|$ 为代价函数，则得到
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint |\hat{\theta} - \theta| p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
最小化这一Bayes风险得到的估计量称为[[#最小绝对误差 (MAE) 估计]]。

### 成功失败型误差定义的Bayes风险

**成功失败型误差 (0-1 loss)** 的代价函数为
$$
C(\varepsilon) = \begin{cases}
0, & \text{if } |\varepsilon| < \delta \\
1, & \text{otherwise}
\end{cases}
$$
因此得到的Bayes风险为
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
最小化这一Bayes风险得到的估计量称为[[#最大后验概率 (MAP) 估计]]。

> [!note] 三种Bayes估计量的比较
> MMSE、MAE、MAP三种Bayes估计量分别是**后验分布的均值、中值、最值**，一般并不相等。的别地，对于对称的单峰分布，如 **Gauss分布**，三者相等，**三种估计方法等价**。

## Bayes估计的性能改进

对于白噪声中电平估计的问题，既可以[[#最小方差无偏 (MVU) 估计#^Example-MVU-CRLB|使用CRLB求解MVU估计量]]，得到
$$
\hat{A}_{\mathrm{MVU}} = \frac{1}{N} \sum_{n=0}^{N-1} x[n] = \bar{x}, \qquad \mathrm{mse}(\hat{A}_{\mathrm{MVU}}) = \frac{\sigma^{2}}{N}
$$
也可以[[#最小均方误差 (MMSE) 估计#^Example-MMSE|基于先验求解MMSE估计量]]，得到
$$
\hat{A}_{\mathrm{MMSE}} = \frac{N\sigma_{A}^{2} \bar{x} + \sigma^{2} \mu_{A}}{N\sigma_{A}^{2} + \sigma^{2}}, \qquad \mathrm{Bmse}(\hat{A}_{\mathrm{MMSE}}) = \frac{\sigma^{2} \sigma_{A}^{2}}{N\sigma_{A}^{2} + \sigma^{2}}
$$
可以看出，MMSE估计量 $\hat{A}_{\mathrm{MMSE}}$ 是 **MVU估计量 $\hat{A}_{\mathrm{MVU}}$ 和先验均值 $\mu_{A}$ 的加权平均**
$$
\begin{align} 
& \hat{A}_{\mathrm{MMSE}} = \cfrac{\frac{N}{\sigma^{2}}}{\frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}}} \hat{A}_{\mathrm{MVU}} + \cfrac{\frac{1}{\sigma_{A}^{2}}}{\frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}}} \mu_{A} = \alpha \hat{A}_{\mathrm{MVU}} + (1-\alpha) \mu_{A}  \\
& \mathrm{Bmse}(\hat{A}_{\mathrm{MMSE}}) = \cfrac{1}{\frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}}}  = \alpha \frac{\sigma^{2}}{N} = \alpha \cdot \mathrm{mse}(\hat{A}_{\mathrm{MVU}})
\end{align}
$$
其权重 $\alpha = \cfrac{\frac{N}{\sigma^{2}}}{\frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}}}$ 反映了数据和先验信息的相对可靠性。

对某一次估计，MMSE估计量的MSE为
$$
\begin{align}
\mathrm{mse}(\hat{A}_{\mathrm{MMSE}}) &= \mathbb{E} \left[ (\hat{A}_{\mathrm{MMSE}} - A)^{2} \right] = \mathbb{E} \left[ (\hat{A}_{\mathrm{MMSE}} - \mathbb{E}[\hat{A}_{\mathrm{MMSE}}] + \mathbb{E}[\hat{A}_{\mathrm{MMSE}}] - A)^{2} \right] \\
&= \mathrm{var}(\hat{A}_{\mathrm{MMSE}}) + (\mathbb{E}[\hat{A}_{\mathrm{MMSE}}] - A)^{2}  \\
&= \alpha^{2} \mathrm{var}(\hat{A}_{\mathrm{MVU}}) + (\alpha A + (1-\alpha) \mu_{A} - A)^{2} \\
&= \alpha^{2} \frac{\sigma^{2}}{N} + (1-\alpha)^{2} (A - \mu_{A})^{2}
\end{align}
$$
由于 $\alpha < 1$，显然有
$$
\mathrm{mse}(\hat{A}_{\mathrm{MMSE}}) \Big|_{|A - \mu_{A}| \ll \frac{\sigma}{\sqrt{ N }}} < \mathrm{Bmse}(\hat{A}_{\mathrm{MMSE}}) < \mathrm{mse}(\hat{A}_{\mathrm{MVU}}) \ll \mathrm{mse}(\hat{A}_{\mathrm{MMSE}}) \Big|_{|A - \mu_{A}| \gg \frac{\sigma}{\sqrt{ N }}}
$$

```tikz
\begin{document}
\large
\begin{tikzpicture}

\draw[-latex] (-3, 0) -- (3, 0) node[below] {$A$}; 
\draw[-latex] (-2.5, -0.5) -- (-2.5, 4) node[left] {MSE};

\draw[thick, blue!50] plot[domain=-3:3] (\x, {0.3*pow(\x, 2) + 0.72}) node[right] {$\mathrm{mse}(\hat{A}_{\mathrm{MMSE}})$};
\draw[thick, red!50] plot[domain=-3:3] (\x, {1.2}) node[right] {$\mathrm{Bmse}(\hat{A}_{\mathrm{MMSE}})$};
\draw[thick, orange!50] plot[domain=-3:3] (\x, {2}) node[right] {$\mathrm{mse}(\hat{A}_{\mathrm{MVU}})$};

\draw[dashed] (0, 0.72) -- (0, 0) node[below] {$\mu_{A}$};

\end{tikzpicture}
\end{document}
```

可见，Bayes估计量对估计性能的改进是**平均意义上的改进**。
+ 就某一次估计而言，视真值与先验的匹配程度，MMSE估计量的性能可能优于或劣于MVU估计量；
+ **Bayes MSE是对经典MSE依先验分布的加权平均**，就这一平均意义而言，MMSE估计量的性能优于MVU估计量；
+ 当先验信息不准确时，会起到负面作用，MMSE估计量的性能将远劣于MVU估计量。


# 最小均方误差 (MMSE) 估计

## Bayes均方误差

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
> 

需要注意，$\mathbb{E} \left[ (\theta - \hat{\theta})^{2} \right]$ 是一种带有约定俗成性质的简单记号，若交换 $\theta$ 和 $\hat{\theta}$ 的位置则一般表示[[#经典参数估计#均方误差 (MSE) 准则|经典MSE]]
$$
\mathrm{mse}(\hat{\theta}) = \mathbb{E} \left[ (\hat{\theta} - \theta)^{2} \right] = \int (\hat{\theta} - \theta)^{2} p(\v{x}; \theta) \dif \v{x}
$$

## MMSE估计

以Bayes MSE为准则，在所有估计量中选取具有最小Bayes MSE的估计量，即得到**最小均方误差估计**，又称 **Bayes估计**。

> [!definition] 最小均方误差 (MMSE) 估计
> **最小均方误差 (minimum mean square error, MMSE) 估计**是指在所有估计量中，具有最小Bayes均方误差的估计量，即
> $$
> \hat{\theta} = \arg\min_{\hat{\theta}} \mathrm{Bmse}(\hat{\theta}) = \arg\min_{\hat{\theta}} \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta
> $$

### MMSE估计量的求解

尝试分析Bayes MSE，注意到
$$
\begin{align}
\mathrm{Bmse}(\hat{\theta}) &= \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta 
= \iint (\hat{\theta} - \theta)^{2} p(\theta \mid \v{x}) p(\v{x}) \dif \theta \dif \v{x} \\
&= \int \left( \int (\hat{\theta} - \theta)^{2} p(\theta \mid \v{x}) \dif \theta \right) p(\v{x}) \dif \v{x}
\end{align}
$$
要最小化 $\mathrm{Bmse}(\hat{\theta})$，等价于**在每一个 $\v{x}$ 处通过 $\hat{\theta}(\v{x})$ 最小化 $\dint (\hat{\theta} - \theta)^{2} p(\theta \mid \v{x}) \dif \theta$**，因此MMSE估计量满足
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
即，**MMSE估计量是参数 $\theta$ 基于后验分布 $p(\theta \mid \v{x})$ 的均值**。

> [!example]- 求解 MMSE 估计量：示例 ^Example-MMSE
>
> **白噪声中电平估计问题。**
> 考虑
> $$
> x[n] = A + w[n], \qquad n = 0, 1, \dots, N-1
> $$
> 其中待估计参数为信号幅度 $A$，噪声 $w[n]$ 是均值为0、方差为 $\sigma^{2}$ 的Gauss白噪声。**假设 $A$ 的先验分布为Gauss分布 $\mathcal{N}(\mu_{A}, \sigma^{2}_{A})$**，求 $A$ 的MMSE估计量。
> 
> ---
> 
> 欲求解 $A$ 的MMSE估计量 $\hat{A}$，首先**写出 $A$ 的后验分布** $p(A \mid \v{x}) = \frac{p(\v{x} \mid A) p(A)}{\dint_{-\infty}^{+\infty} p(\v{x} \mid A) p(A) \dif A}$，其中
> $$
> p(\v{x} \mid A) = \prod_{n=0}^{N-1} \frac{1}{\sqrt{2\pi \sigma^{2}}} \exp \left( -\frac{(x[n] - A)^{2}}{2\sigma^{2}} \right) = \frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right)
> $$
> 而 $p(A) = \frac{1}{\sqrt{2\pi \sigma^{2}_{A}}} \exp \left( -\frac{(A - \mu_{A})^{2}}{2\sigma^{2}_{A}} \right)$，因此
> $$
> \begin{align}
> p(\v{x}\mid A) p(A) &= \frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right) \frac{1}{\sqrt{2\pi \sigma^{2}_{A}}} \exp \left( -\frac{(A - \mu_{A})^{2}}{2\sigma^{2}_{A}} \right) \\
> &= \frac{1}{(2\pi \sigma^{2})^{N/2} \sqrt{2\pi \sigma^{2}_{A}}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} - \frac{(A - \mu_{A})^{2}}{2\sigma^{2}_{A}} \right) \\
> &= \frac{1}{(2\pi \sigma^{2})^{N/2} \sqrt{2\pi \sigma^{2}_{A}}} \exp \left( -\frac{1}{2\sigma^{2}} \left( \sum\limits_{n=0}^{N-1} (x[n])^{2} -2A \sum\limits_{n=0}^{N-1} x[n] + NA^{2} \right) - \frac{(A - \mu_{A})^{2}}{2\sigma^{2}_{A}} \right)
> \end{align}
> $$
> 只需关注与 $A$ 有关的因子，考察
> $$
> \begin{align}
> Q(A) &= \frac{1}{2\sigma^{2}} \left( 2A \sum\limits_{n=0}^{N-1} x[n] - NA^{2} \right) - \frac{(A - \mu_{A})^{2}}{2\sigma^{2}_{A}} \\
> &= - \underbrace{ \left( \frac{N}{2\sigma^{2}} + \dfrac{1}{2\sigma_{A}^{2}} \right) }_{ 1/2\sigma_{A\mid \v{x}}^{2} } A^{2} + \underbrace{ \left( \frac{1}{\sigma^{2}} \sum\limits_{n=0}^{N-1} x[n] + \frac{\mu_{A}}{\sigma_{A}^{2}} \right) }_{ \mu_{A\mid \v{x}}/\sigma_{A\mid \v{x}}^{2} } A - \frac{\mu_{A}^{2}}{2\sigma_{A}^{2}} \\
> &= - \frac{1}{2\sigma_{A\mid \v{x}}^{2}} \left( A^{2} - 2 \mu_{A\mid \v{x}} A + \mu_{A\mid \v{x}}^{2} \right) + \frac{\mu_{A\mid \v{x}}^{2}}{2\sigma_{A\mid \v{x}}^{2}} - \frac{\mu_{A}^{2}}{2\sigma_{A}^{2}} 
> \end{align}
> $$
> 其中
> $$
> \mu_{A\mid \v{x}} = \sigma_{A\mid \v{x}}^{2} \left( \frac{1}{\sigma^{2}} \sum\limits_{n=0}^{N-1} x[n] + \frac{\mu_{A}}{\sigma_{A}^{2}} \right), \qquad 
> \sigma_{A\mid \v{x}}^{2} = \left( \frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}} \right)^{-1}
> $$
> 与 $A$ 无关的因子均可约去，即得
> $$
> p(A \mid \v{x}) = \frac{p(\v{x} \mid A) p(A)}{\dint_{-\infty}^{+\infty} p(\v{x} \mid A) p(A) \dif A}
> = \frac{1}{\sqrt{2\pi \sigma_{A\mid \v{x}}^{2}}} \exp \left( -\frac{(A - \mu_{A\mid \v{x}})^{2}}{2\sigma_{A\mid \v{x}}^{2}} \right)
> $$
> 服从**均值为 $\mu_{A\mid \v{x}}$、方差为 $\sigma_{A\mid \v{x}}^{2}$ 的Gauss分布**。
> 
> 于是MMSE估计量为
> $$
> \hat{A} = \int A p(A \mid \v{x}) \dif A = \mathbb{E} \left[ A \mid \v{x} \right] = \mu_{A\mid \v{x}} = \sigma_{A\mid \v{x}}^{2} \left( \frac{1}{\sigma^{2}} \sum_{n=0}^{N-1} x[n] + \frac{\mu_{A}}{\sigma_{A}^{2}} \right) = \frac{\sigma_{A}^{2} \sum\limits_{n=0}^{N-1} x[n] + \sigma^{2} \mu_{A}}{N\sigma_{A}^{2} + \sigma^{2}}
> $$
> 其性能由Bayes MSE衡量
> $$
> \begin{align} 
> \mathrm{Bmse} (\hat{A}) &= \iint (\hat{A} - A)^{2} p(\v{x}, A) \dif \v{x} \dif A = \int \left( \int (\hat{A} - A)^{2} p(A \mid \v{x}) \dif A \right) p(\v{x}) \dif \v{x}  \\
> &= \int \sigma_{A\mid \v{x}}^{2} p(\v{x}) \dif \v{x} = \sigma_{A\mid \v{x}}^{2} 
> = \left( \frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}} \right)^{-1} = \frac{\sigma^{2} \sigma_{A}^{2}}{N\sigma_{A}^{2} + \sigma^{2}}
> \end{align}
> $$

### 矢量参数MMSE估计

当存在未知而不感兴趣的参数时，Bayes框架下可**通过积分消除这些参数**的影响，即
$$
p(\v{\theta} \mid \v{x}) = \dint p(\v{\theta}, \v{\alpha} \mid \v{x}) \dif \v{\phi}, \qquad
p(\v{x} \mid \v{\theta}) = \dint p(\v{x} \mid \v{\theta}, \v{\alpha}) p(\v{\alpha} \mid \v{\theta}) \dif \v{\alpha}
$$
进而求得MMSE估计量。

进一步地，为了估计矢量参数 $\v{\theta}$，可依次估计每个参数 $\theta_{i}$ 而将剩余参数视为暂不感兴趣的参数，即得到
$$
\hat{\theta}_{i} = \int \theta_{i} p(\theta_{i} \mid \v{x}) \dif \theta_{i} 
= \int \theta_{i} \left( \idotsint p(\v{\theta} \mid \v{x}) \prod_{j \neq i} \dif \theta_{j} \right) \dif \theta_{i} 
= \int \theta_{i} p(\v{\theta} \mid \v{x}) \dif \v{\theta}
$$
因此矢量参数 $\v{\theta}$ 的MMSE估计量为
$$
\hat{\v{\theta}} = \begin{pmatrix}
\hat{\theta}_{1} \\ \hat{\theta}_{2} \\ \vdots \\ \hat{\theta}_{M}
\end{pmatrix} = \int \begin{pmatrix}
\theta_{1} \\ \theta_{2} \\ \vdots \\ \theta_{M}
\end{pmatrix} p(\v{\theta} \mid \v{x}) \dif \v{\theta}
= \int \v{\theta} p(\v{\theta} \mid \v{x}) \dif \v{\theta} = \mathbb{E}[\v{\theta} \mid \v{x}]
$$

### MMSE估计量的性质

#### 线性变换不变性

若 $\v{\alpha} = \boldsymbol{A} \v{\theta} + \v{b}$ 是 $\v{\theta}$ 的线性变换，其中 $\boldsymbol{A}$ 是可逆矩阵，$\v{b}$ 是常数向量，则 $\v{\alpha}$ 的MMSE估计量为 $\hat{\v{\alpha}} = \boldsymbol{A} \hat{\v{\theta}} + \v{b}$。

#### 对待估计参数可加性

设 $\v{\theta} = \v{\theta}_{1} + \v{\theta}_{2}$，其中 $\v{\theta}_{1}$ 和 $\v{\theta}_{2}$ 是两个待估计随机向量，则相应MMSE估计量是可加的，即
$$
\hat{\v{\theta}} = \mathbb{E} \left[ \v{\theta} \mid[\Big] \v{x} \right] = \mathbb{E} \left[ \v{\theta}_{1} + \v{\theta}_{2} \mid[\Big] \v{x} \right] = \mathbb{E} \left[ \v{\theta}_{1} \mid[\Big] \v{x} \right] + \mathbb{E} \left[ \v{\theta}_{2} \mid[\Big] \v{x} \right] = \hat{\v{\theta}}_{1} + \hat{\v{\theta}}_{2}
$$

#### 对独立Guass数据矢量可加性

对于Gauss先验、Gauss数据分布，MMSE估计量亦为[[#线性最小均方误差 (LMMSE) 估计]]估计量
$$
\hat{\v{\theta}} = \mathbb{E} \left[ \v{\theta} \mid[\Big] \v{x} \right] = \mathbb{E} \left[ \v{\theta} \right] + \boldsymbol{C}_{\theta x} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}])
$$
若 $\v{\theta}, \v{x}_{1}, \v{x}_{2}$ 是联合Gauss的，数据矢量 $\v{x} = \v{x}_{1} + \v{x}_{2}$，且 $\v{x}_{1}$ 和 $\v{x}_{2}$ 相互独立，则MMSE估计量为
$$
\begin{align}
\hat{\v{\theta}} = \mathbb{E} \left[ \v{\theta} \mid[\Big] \v{x} \right] &= \mathbb{E} \left[ \v{\theta} \right] + \boldsymbol{C}_{\theta x_{1}} \boldsymbol{C}_{x_{1} x_{1}}^{-1} (\v{x}_{1} - \mathbb{E}[\v{x}_{1}]) + \boldsymbol{C}_{\theta x_{2}} \boldsymbol{C}_{x_{2} x_{2}}^{-1} (\v{x}_{2} - \mathbb{E}[\v{x}_{2}]) \\
&= \mathbb{E} \left[ \v{\theta} \mid[\Big] \v{x}_{1} \right] + \mathbb{E} \left[ \v{\theta} \mid[\Big] \v{x}_{2} \right] - \mathbb{E} \left[ \v{\theta} \right]
\end{align}
$$


# 最小绝对误差 (MAE) 估计

## MAE估计

### 绝对误差定义的Bayes风险

以**绝对误差** $C(\varepsilon) = |\varepsilon|$ 为代价函数，则得到
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint |\hat{\theta} - \theta| p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
最小化这一Bayes风险得到的估计量称为**最小绝对误差 (MAE) 估计**。

> [!definition] 最小绝对误差 (MAE) 估计
> **最小绝对误差 (minimum absolute error, MAE) 估计**是指在所有估计量中，具有最小绝对误差定义的Bayes风险的估计量，即
> $$
> \hat{\theta} = \arg\min_{\hat{\theta}} \iint |\hat{\theta} - \theta| p(\v{x}, \theta) \dif \v{x} \dif \theta
> $$

### MAE估计量

尝试分析MAE定义的Bayes风险，注意到
$$
\mathfrak{R} = \iint |\hat{\theta} - \theta| p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint |\hat{\theta} - \theta| p(\theta \mid \v{x}) p(\v{x}) \dif \theta \dif \v{x} = \int \left( \int |\hat{\theta} - \theta| p(\theta \mid \v{x}) \dif \theta \right) p(\v{x}) \dif \v{x}
$$
要最小化 $\mathfrak{R}$，等价于**在每一个 $\v{x}$ 处通过 $\hat{\theta}(\v{x})$ 最小化 $\dint |\hat{\theta} - \theta| p(\theta \mid \v{x}) \dif \theta$**。为便于求导，将绝对值函数分段表示，由Leibniz准则得到
$$
\begin{align}
0 &= \frac{ \partial }{ \partial \theta }  \dint |\hat{\theta} - \theta| p(\theta \mid \v{x}) \dif \theta = \frac{ \partial }{ \partial \theta } \int_{-\infty}^{\hat{\theta}} (\hat{\theta} - \theta) p(\theta \mid \v{x}) \dif \theta + \frac{ \partial }{ \partial \theta } \int_{\hat{\theta}}^{\infty} (\theta - \hat{\theta}) p(\theta \mid \v{x}) \dif \theta  \\
&= \int_{-\infty}^{\hat{\theta}} p(\theta \mid \v{x}) \dif \theta - \int_{\hat{\theta}}^{\infty} p(\theta \mid \v{x}) \dif \theta 
\end{align}
$$
因此要求
$$
\int_{-\infty}^{\hat{\theta}} p(\theta \mid \v{x}) \dif \theta = \int_{\hat{\theta}}^{\infty} p(\theta \mid \v{x}) \dif \theta
$$
此时称 $\hat{\theta}$ 是后验分布 $p(\theta \mid \v{x})$ 的**中值**，即MAE估计量是参数 $\theta$ 基于后验分布 $p(\theta \mid \v{x})$ 的中值。



# 最大后验概率 (MAP) 估计

## MAP估计

### 成功失败型误差定义的Bayes风险

**成功失败型误差 (0-1 loss)** 的代价函数为
$$
C(\varepsilon) = \begin{cases}
0, & \text{if } |\varepsilon| < \delta \\
1, & \text{otherwise}
\end{cases} = \mathbb{1}(|\varepsilon| \geq \delta)
$$
因此得到的Bayes风险为
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
最小化这一Bayes风险得到的估计量称为**最大后验概率 (maximum a posteriori, MAP) 估计**。

> [!definition] 最大后验概率 (MAP) 估计
> **最大后验概率 (maximum a posteriori, MAP) 估计**是指在所有估计量中，具有最小成功失败型误差定义的Bayes风险的估计量，即
> $$
> \hat{\theta} = \arg\min_{\hat{\theta}} \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\v{x}, \theta) \dif \v{x} \dif \theta
> $$

后面将看到，当 $\delta \to 0$ 时，MAP估计量是参数 $\theta$ 使后验分布 $p(\theta \mid \v{x})$ 取得最大值的点，即 **MAP估计量是参数 $\theta$ 基于后验分布 $p(\theta \mid \v{x})$ 的众数**。因此得名「最大后验概率估计」。

### MAP估计量

尝试分析MAP定义的Bayes风险，类似可有
$$
\begin{align} 
\mathfrak{R} &= \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\theta \mid \v{x}) p(\v{x}) \dif \theta \dif \v{x}  \\
&= \int \left( \int \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\theta \mid \v{x}) \dif \theta  \right) p(\v{x}) \dif \v{x} 
\end{align}
$$
要最小化 $\mathfrak{R}$，等价于**在每一个 $\v{x}$ 处通过 $\hat{\theta}(\v{x})$ 最小化 $\dint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\theta \mid \v{x}) \dif \theta$**。将指示函数分段表示，即
$$
\dint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\theta \mid \v{x}) \dif \theta = \int_{-\infty}^{\hat{\theta} - \delta} p(\theta \mid \v{x}) \dif \theta + \int_{\hat{\theta} + \delta}^{\infty} p(\theta \mid \v{x}) \dif \theta
= 1 - \int_{\hat{\theta} - \delta}^{\hat{\theta} + \delta} p(\theta \mid \v{x}) \dif \theta
$$
因此要求最大化 $\dint_{\hat{\theta} - \delta}^{\hat{\theta} + \delta} p(\theta \mid \v{x}) \dif \theta$，当 $\delta \to 0$ 时，MAP估计量是参数 $\theta$ **使后验分布 $p(\theta \mid \v{x})$ 取得最大值**的点，即
$$
\hat{\theta} = \arg\max_{\hat{\theta}} p(\hat{\theta} \mid \v{x})
= \arg\max_{\hat{\theta}} \frac{p(\v{x} \mid \hat{\theta}) p(\hat{\theta})}{p(\v{x})}
= \arg\max_{\hat{\theta}} p(\v{x} \mid \hat{\theta}) p(\hat{\theta})
= \arg\max_{\hat{\theta}} \left( \ln p(\v{x} \mid \hat{\theta}) + \ln p(\hat{\theta}) \right)
$$

### Bayes MLE估计

当数据量极大时，先验信息的作用会被观测数据的作用所淹没，此时MAP估计量趋近于
$$
\hat{\theta} = \arg\max_{\hat{\theta}} p(\v{x} \mid \hat{\theta}) = \arg\max_{\hat{\theta}} \ln p(\v{x} \mid \hat{\theta})
$$
与经典[[#最大似然估计 (MLE)]] 类似，而似然函数是条件概率密度函数 $p(\v{x} \mid \theta)$，因此称这一估计量为 **Bayes MLE估计**，又称**极大似然后验 (maximum a posteriori likelihood, MAPLE) 估计**。



# 线性最小均方误差 (LMMSE) 估计

## LMMSE估计

类似于[[#最佳线性无偏估计 (BLUE)]] 与[[最小方差无偏 (MVU) 估计]]的关系，**线性最小均方误差估计**是**线性**的**最小均方误差**估计，即在满足线性约束的前提下使均方误差最小的估计方法。

> [!definition] LMMSE估计
> **线性最小均方误差 (linear minimum mean square error, LMMSE) 估计**是指在所有**线性估计量**中，具有最小均方误差的估计量，也称**线性Bayes估计**。

具体地，LMMSE 要求
1. 估计量 $\hat{\theta}$ 对观察数据 $\v{x}$ 是**线性**的，即 **$\hat{\theta} = \sum\limits_{n=0}^{N-1} a_{n} x[n] + a_{N}$**，其中 $\v{a} = (a_{0}, a_{1}, \cdots, a_{N-1})^{\mathrm{T}}$ 是一个 $N$ 维的权重向量，$a_{N}$ 是一个常数项；
2. 估计量 $\hat{\theta}$ 的 **Bayes均方误差** $\mathrm{Bmse} (\hat{\theta}) = \mathbb{E} \left[ (\theta - \hat{\theta})^{2} \right]$ 最小。

与BLUE不同的是，LMMSE估计量**不要求满足无偏性约束**，因此在某些情况下可能具有更小的均方误差。

### LMMSE估计量的求解

将 $\hat{\theta}$ 代入 Bayes MSE 的定义，得到
$$
\mathrm{Bmse}(\hat{\theta}) = \mathbb{E} \left[ (\theta - \hat{\theta})^{2} \right] = \mathbb{E} \left[ \left( \theta - \sum_{n=0}^{N-1} a_{n} x[n] - a_{N} \right)^{2} \right] 
$$
变动每个 $a_{i}$ 都改变 $\mathrm{Bmse}(\hat{\theta})$，在最小值处应有
$$
\frac{ \partial }{ \partial a_{i} } \mathrm{Bmse}(\hat{\theta}) = 0, \qquad i = 0, 1, \cdots, N-1, N
$$

首先**考虑 $a_{N}$ 的方程**，有
$$
\frac{ \partial }{ \partial a_{N} } \mathrm{Bmse}(\hat{\theta}) = \mathbb{E} \left[ 2 \left( \sum_{n=0}^{N-1} a_{n} x[n] + a_{N} - \theta \right) \right] = 0
\implies
a_{N} = \mathbb{E}[\theta] - \sum_{n=0}^{N-1} a_{n} \mathbb{E}[x[n]]
$$
于是
$$
\begin{align}
\mathrm{Bmse}(\hat{\theta}) &= \mathbb{E} \left[ \left( \theta - \sum_{n=0}^{N-1} a_{n} x[n] - a_{N} \right)^{2} \right] 
= \mathbb{E} \left[ \left( (\theta - \mathbb{E}[\theta]) - \sum_{n=0}^{N-1} a_{n} (x[n] - \mathbb{E}[x[n]]) \right)^{2} \right] \\
&= \mathbb{E} \left[ \left( \v{a}^{\mathrm{T}} (\v{x} - \mathbb{E}[\v{x}]) - (\theta - \mathbb{E}[\theta]) \right)^{2} \right] \\
&= \mathbb{E} \left[ \left( \v{a}^{\mathrm{T}} (\v{x} - \mathbb{E}[\v{x}]) - (\theta - \mathbb{E}[\theta]) \right) \left( \v{a}^{\mathrm{T}} (\v{x} - \mathbb{E}[\v{x}]) - (\theta - \mathbb{E}[\theta]) \right)^{\mathrm{T}} \right] \\
&= \mathbb{E} \left[ \v{a}^{\mathrm{T}} (\v{x} - \mathbb{E}[\v{x}]) (\v{x} - \mathbb{E}[\v{x}])^{\mathrm{T}} \v{a} \right] 
- \mathbb{E} \left[ \v{a}^{\mathrm{T}} (\v{x} - \mathbb{E}[\v{x}]) (\theta - \mathbb{E}[\theta])^{\mathrm{T}} \right] \\
&\hspace{1em}- \mathbb{E} \left[ (\theta - \mathbb{E}[\theta]) (\v{x} - \mathbb{E}[\v{x}])^{\mathrm{T}} \v{a} \right] + \mathbb{E} \left[ (\theta - \mathbb{E}[\theta])^{2} \right] \\
&= \v{a}^{\mathrm{T}} \boldsymbol{C}_{xx} \v{a} - \v{a}^{\mathrm{T}} \boldsymbol{C}_{x\theta} - \boldsymbol{C}_{\theta x} \v{a} + \boldsymbol{C}_{\theta \theta}
\end{align}
$$
其中，引入协方差矩阵：
+ 观测数据 $\v{x}$ 的协方差矩阵 $\boldsymbol{C}_{xx} = \mathbb{E} \left[ (\v{x} - \mathbb{E}[\v{x}]) (\v{x} - \mathbb{E}[\v{x}])^{\mathrm{T}} \right]$ 为 $N\times N$ 的矩阵，
+ 观测数据 $\v{x}$ 与参数 $\theta$ 的互协方差矩阵 $\boldsymbol{C}_{x\theta} = \mathbb{E} \left[ (\v{x} - \mathbb{E}[\v{x}]) (\theta - \mathbb{E}[\theta])^{\mathrm{T}} \right]$ 为 $N \times 1$ 向量，且对称地有 $\boldsymbol{C}_{\theta x} = \boldsymbol{C}_{x\theta}^{\mathrm{T}}$，
+ 参数 $\theta$ 的协方差矩阵 $\boldsymbol{C}_{\theta \theta} = \mathbb{E} \left[ (\theta - \mathbb{E}[\theta])^{2} \right]$ 为标量，即参数 $\theta$ 的方差。

这样，**考虑 $a_{1}$、$a_{2}$、$\cdots$、$a_{N-1}$ 的方程**，即
$$
\frac{ \partial }{ \partial \v{a} } \mathrm{Bmse}(\hat{\theta}) = 2 \boldsymbol{C}_{xx} \v{a} - 2 \boldsymbol{C}_{x\theta} = \v{0} \implies \v{a} = \boldsymbol{C}_{xx}^{-1} \boldsymbol{C}_{x\theta}
$$
因此，**LMMSE估计量**为
$$
\hat{\theta} = \v{a}^{\mathrm{T}} \v{x} + a_{N} = \mark{ \mathbb{E}[\theta] + \boldsymbol{C}_{\theta x} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}]) }
$$
其 **Bayes均方误差**为
$$
\mathrm{Bmse}(\hat{\theta}) = \mathbb{E} \left[ (\theta - \hat{\theta})^{2} \right] = \mark{ \boldsymbol{C}_{\theta \theta} - \boldsymbol{C}_{\theta x} \boldsymbol{C}_{xx}^{-1} \boldsymbol{C}_{x\theta} }
$$

类似于[[#最佳线性无偏估计 (BLUE)]]，LMMSE估计量也**只与待估计参数的一阶矩、二阶矩有关**，因此在实际应用中不需要知道参数 $\theta$ 的完整分布信息。

> [!example] 求解 LMMSE 估计量：示例 ^ExampleLMMSE
> 
> **白噪声中电平估计。** 
> 
> 考虑一个测量系统
> $$
> x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> $$
> 其中噪声 $w[n] \sim \mathcal{N}(0, \sigma^{2})$ 是一个零均值的 Gauss 白噪声，$A$ 服从Gauss分布 $\mathcal{N}(0, \sigma_{A}^{2})$。给出 $A$ 的LMMSE估计量 $\hat{A}$ 和其Bayes均方误差 $\mathrm{Bmse}(\hat{A})$。
> 
> ---
> 
> 根据公式，只需计算相关的协方差矩阵，有
> $$
> \begin{align}
> & \begin{aligned}
> \boldsymbol{C}_{xx} &= \mathbb{E} \left[ (\v{x} - \mathbb{E}[\v{x}]) (\v{x} - \mathbb{E}[\v{x}])^{\mathrm{T}} \right] = \mathbb{E} \left[ (A \v{1} + \v{w}) (A \v{1} + \v{w})^{\mathrm{T}} \right]  \\
> &= \mathbb{E} \left[ A^{2} \v{1} \v{1}^{\mathrm{T}} + A \v{1} \v{w}^{\mathrm{T}} + A \v{w} \v{1}^{\mathrm{T}} + \v{w} \v{w}^{\mathrm{T}} \right] = \sigma_{A}^{2} \v{1} \v{1}^{\mathrm{T}} + \sigma^{2} \boldsymbol{I}
> \end{aligned} \\
> & \boldsymbol{C}_{xA} = \mathbb{E} \left[ (\v{x} - \mathbb{E}[\v{x}]) (A - \mathbb{E}[A])^{\mathrm{T}} \right] = \mathbb{E} \left[ (A \v{1} + \v{w}) A \right] = \sigma_{A}^{2} \v{1} \\
> & \boldsymbol{C}_{AA} = \mathbb{E} \left[ (A - \mathbb{E}[A])^{2} \right] = \sigma_{A}^{2}
> \end{align}
> $$
> 由**Woodbury恒等式 $\left( \boldsymbol{B} + \v{u} \v{u}^{\mathrm{T}} \right)^{-1} = \boldsymbol{B}^{-1} - \dfrac{\boldsymbol{B}^{-1} \v{u} \v{u}^{\mathrm{T}} \boldsymbol{B}^{-1}}{1 + \v{u}^{\mathrm{T}}\boldsymbol{B}^{-1}\v{u}}$**，对 $\boldsymbol{C}_{xx}$ 求逆得到
> $$
> \boldsymbol{C}_{xx}^{-1} = \frac{1}{\sigma^{2}} \left( \boldsymbol{I} - \frac{\sigma_{A}^{2}}{N\sigma_{A}^{2} + \sigma^{2}} \v{1} \v{1}^{\mathrm{T}} \right)
> $$
> 因此，LMMSE估计量为
> $$
> \hat{A} = \mathbb{E}[A] + \boldsymbol{C}_{A x} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}]) = \sigma_{A}^{2} \v{1}^{\mathrm{T}} (\sigma_{A}^{2} \v{1} \v{1}^{\mathrm{T}} + \sigma^{2} \boldsymbol{I})^{-1} \v{x}
> = \frac{\sigma_{A}^{2}}{N\sigma_{A}^{2} + \sigma^{2}} \sum\limits_{n=0}^{N-1} x[n]
> $$
> 其 Bayes MSE 为
> $$
> \mathrm{Bmse}(\hat{A}) = \boldsymbol{C}_{AA} - \boldsymbol{C}_{A x} \boldsymbol{C}_{xx}^{-1} \boldsymbol{C}_{x A} = \sigma_{A}^{2} - \sigma_{A}^{4} \v{1}^{\mathrm{T}} (\sigma_{A}^{2} \v{1} \v{1}^{\mathrm{T}} + \sigma^{2} \boldsymbol{I})^{-1} \v{1}
> = \frac{\sigma_{A}^{2} \sigma^{2}}{N\sigma_{A}^{2} + \sigma^{2}}
> $$

### 矢量参数LMMSE

LMMSE仍是MMSE，因此类似于[[#最小均方误差 (MMSE) 估计#矢量参数MMSE估计|矢量参数MMSE估计]]通过积分消除无关参数的结论，有
$$
\hat{\v{\theta}} = \begin{pmatrix}
\hat{\theta}_{1} \\ \hat{\theta}_{2} \\ \vdots \\ \hat{\theta}_{p}
\end{pmatrix} = \begin{pmatrix}
\mathbb{E}[\theta_{1}] + \boldsymbol{C}_{\theta_{1} x} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}]) \\
\mathbb{E}[\theta_{2}] + \boldsymbol{C}_{\theta_{2} x} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}]) \\
\vdots \\
\mathbb{E}[\theta_{p}] + \boldsymbol{C}_{\theta_{p} x} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}])
\end{pmatrix} = \mathbb{E}[\v{\theta}] + \boldsymbol{C}_{\theta x} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}])
$$
此时 $\boldsymbol{C}_{\theta x}$ 为 $p \times N$ 的互协方差矩阵，每一行对应是参数 $\theta_{i}$ 与观测数据 $\v{x}$ 的互协方差矩阵 $\boldsymbol{C}_{\theta_{i} x}$。

> [!note] Bayes一般线性模型
> 《概率论与随机过程（2）》中提到，**多维Gauss分布的条件分布**也是Gauss分布，且其条件均值和方差为
> $$
> \begin{align}
> & \mathbb{E} [ \v{y} \mid \v{x} ] = \mathbb{E}[\v{y}] + \boldsymbol{C}_{yx} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}]), \qquad
> \boldsymbol{C}_{y\mid \v{x}} = \boldsymbol{C}_{yy} - \boldsymbol{C}_{yx} \boldsymbol{C}_{xx}^{-1} \boldsymbol{C}_{xy} \\
> & \text{其中}\quad \begin{pmatrix}
> \v{x} \\ \v{y}
> \end{pmatrix} \sim \mathcal{N} \left( \begin{pmatrix}\mathbb{E}[\v{x}] \\ \mathbb{E}[\v{y}] \end{pmatrix}, \begin{pmatrix}
> \boldsymbol{C}_{xx} & \boldsymbol{C}_{xy} \\
> \boldsymbol{C}_{yx} & \boldsymbol{C}_{yy}
> \end{pmatrix} \right)
> \end{align}
> $$
> 注意到，LMMSE估计量及其Bayes MSE的形式与上述多维Gauss条件分布的均值和方差的形式相似，即**在 $\v{x}$、$\v{\theta}$ 服从多维Gauss分布的条件下，LMMSE估计量即为 $\theta$ 的后验均值，Bayes MSE即为 $\theta$ 的后验方差**。这一条件要求观测数据满足 **Bayes一般线性模型**，即
> $$
> \v{x} = \boldsymbol{H} \v{\theta} + \v{w}
> $$
> 其中，$\boldsymbol{H}$ 是 $N \times p$ 的观测矩阵，$\v{\theta}$ 是 $p$ 维的待估计参数，$\v{w}$ 是一个零均值、协方差矩阵为 $\boldsymbol{C}_{w}$、与 $\v{\theta}$ 无关的随机噪声。Bayes一般线性模型的MMSE估计量正是
> $$
> \begin{align} 
> \hat{\v{\theta}} = \mathbb{E} [ \v{\theta} \mid \v{x} ] &= \mathbb{E}[\v{\theta}] + \boldsymbol{C}_{\theta x} \boldsymbol{C}_{xx}^{-1} (\v{x} - \mathbb{E}[\v{x}])  \\
> &= \mathbb{E}[\v{\theta}] + \boldsymbol{C}_{\theta \theta} \boldsymbol{H}^{\mathrm{T}} (\boldsymbol{H} \boldsymbol{C}_{\theta \theta} \boldsymbol{H}^{\mathrm{T}} + \boldsymbol{C}_{w})^{-1} (\v{x} - \boldsymbol{H} \mathbb{E}[\v{\theta}]) 
> \end{align}
> $$

## 序贯LMMSE

实际应用中，观测数据 $\v{x}$ 可能是**逐渐获得**的，因此需要在每次获得新数据时更新估计量。显然重复计算LMMSE估计量的公式效率较低，因此需要一种**序贯**的更新方法，即**序贯LMMSE (sequential LMMSE)**。

> [!example] 序贯计算 LMMSE 估计量：示例
> 
> **白噪声中电平的序贯估计。** 
> 
> 考虑[[#^ExampleLMMSE|前面示例估计问题]]，已知根据前 $N$ 个观测数据 $x[0], x[1], \cdots, x[N-1]$ 得到 $A$ 的LMMSE估计量 $\hat{A}[N-1]$ 及 $\mathrm{Bmse}(\hat{A}[N-1])$ 为
> $$
> \hat{A}[N-1] = \frac{\sigma_{A}^{2}}{N\sigma_{A}^{2} + \sigma^{2}} \sum\limits_{n=0}^{N-1} x[n], \qquad \mathrm{Bmse}(\hat{A}[N-1]) = \frac{\sigma_{A}^{2} \sigma^{2}}{N\sigma_{A}^{2} + \sigma^{2}}
> $$
> 当获得第 $N$ 个观测数据 $x[N]$ 后，如何更新获得 $A$ 的LMMSE估计量 $\hat{A}[N]$ 及 $\mathrm{Bmse}(\hat{A}[N])$？
> 
> ---
> 
> 当获得第 $N$ 个观测数据 $x[N]$ 后，更新的 **LMMSE估计量**为
> $$
> \begin{align}
> \hat{A}[N] &= \frac{\sigma_{A}^{2}}{(N+1)\sigma_{A}^{2} + \sigma^{2}} \sum\limits_{n=0}^{N} x[n] = \frac{\sigma_{A}^{2}}{(N+1)\sigma_{A}^{2} + \sigma^{2}} \left( \sum\limits_{n=0}^{N-1} x[n] + x[N] \right) \\
> &= \frac{\sigma_{A}^{2}}{(N+1)\sigma_{A}^{2} + \sigma^{2}} \left( \frac{N \sigma_{A}^{2} + \sigma^{2}}{\sigma_{A}^{2}} \hat{A}[N-1] + x[N] \right) \\
> &= \hat{A}[N-1] + \underbrace{ \frac{\sigma_{A}^{2}}{(N+1)\sigma_{A}^{2} + \sigma^{2}} }_{ K[N] } \left( x[N] - \hat{A}[N-1] \right)
> \end{align}
> $$
> 可见，更新的LMMSE估计量 $\hat{A}[N]$ 相对前一个估计量 $\hat{A}[N-1]$ 的增量是**新息 $x[N] - \hat{A}[N-1]$ 的 $K[N]$ 增益缩放**，其中增益因子 $K[N]$ 的形式为
> $$
> K[N] = \frac{\sigma_{A}^{2}}{\sigma_{A}^{2} + N\sigma_{A}^{2} + \sigma^{2}} 
> = \frac{\frac{\sigma_{A}^{2} \sigma^{2}}{N\sigma_{A}^{2} + \sigma^{2}}}{\frac{\sigma_{A}^{2} \sigma^{2}}{N\sigma_{A}^{2} + \sigma^{2}} + \sigma^{2}}
> = \frac{\mathrm{Bmse}(\hat{A}[N-1])}{\mathrm{Bmse}(\hat{A}[N-1]) + \sigma^{2}}
> $$
> 而更新的 **Bayes均方误差**为
> $$
> \begin{align}
> \mathrm{Bmse}(\hat{A}[N]) &= \frac{\sigma_{A}^{2} \sigma^{2}}{(N+1)\sigma_{A}^{2} + \sigma^{2}} = \frac{N\sigma_{A}^{2} + \sigma^{2}}{(N+1)\sigma_{A}^{2} + \sigma^{2}} \cdot \frac{\sigma_{A}^{2} \sigma^{2}}{N\sigma_{A}^{2} + \sigma^{2}} \\
> &= \left( 1 - \frac{\sigma_{A}^{2}}{(N+1)\sigma_{A}^{2} + \sigma^{2}} \right) \cdot \frac{\sigma_{A}^{2} \sigma^{2}}{N\sigma_{A}^{2} + \sigma^{2}} = (1 - K[N]) \cdot \mathrm{Bmse}(\hat{A}[N-1])
> \end{align}
> $$
> 

一般地，**序贯LMMSE估计**的更新公式为
$$
\mark{ \begin{cases}
\hat{\theta}[N] = \hat{\theta}[N-1] + \underbrace{ \dfrac{\mathrm{Bmse}(\hat{\theta}[N-1])}{\mathrm{Bmse}(\hat{\theta}[N-1]) + \sigma_{w}^{2}} }_{ K[N] } \left( x[N] - \hat{\theta}[N-1] \right), \\
\mathrm{Bmse}(\hat{\theta}[N]) = \underbrace{ \dfrac{\sigma_{w}^{2}}{\mathrm{Bmse}(\hat{\theta}[N-1]) + \sigma_{w}^{2}} }_{ 1 - K[N] } \mathrm{Bmse}(\hat{\theta}[N-1]),
\end{cases} } \qquad
N = 1, 2, \cdots
$$
其中，增益因子 $K[N] = \dfrac{\mathrm{Bmse}(\hat{\theta}[N-1])}{\mathrm{Bmse}(\hat{\theta}[N-1]) + \sigma_{w}^{2}}$，$\sigma_{w}^{2}$ 是新观测数据 $x[N]$ 的噪声方差。

特别地，在[[#^ExampleLMMSE|前一示例]]中取 $N = 1$ 时，仅有一个观测数据 $x[0]$，由原始公式得LMMSE估计量及其Bayes MSE为
$$
\begin{cases}
\hat{A}[0] = \mathbb{E}[A] + \frac{\sigma_{A}^{2}}{\sigma_{A}^{2} + \sigma^{2}} (x[0] - \mathbb{E}[x[0]]), \\
\mathrm{Bmse}(\hat{A}[0]) = \sigma_{A}^{2} - \dfrac{\sigma_{A}^{4}}{\sigma_{A}^{2} + \sigma^{2}} = \sigma_{A}^{2} \left( 1 - \frac{\sigma_{A}^{2}}{\sigma_{A}^{2} + \sigma^{2}} \right) 
\end{cases}
$$
若取 $\begin{cases} \hat{A}[-1] = \mathbb{E} \left[ A \right], \\ \mathrm{Bmse}(\hat{A}[-1]) = \sigma_{A}^{2} \end{cases}$ 作为初始值，则上述更新公式将同样适用。推广到一般的序贯LMMSE估计中，**初始值**即取为**先验值**
$$
\mark{ \hat{\theta}[-1] = \mathbb{E}[\theta], \qquad \mathrm{Bmse}(\hat{\theta}[-1]) = \boldsymbol{C}_{\theta \theta} }
$$


# Wiener滤波

## Wiener滤波的任务

给定被噪声污染的观测信号 $x[n] = s[n] + w[n]$，并假定信号 $s[n]$ 和噪声 $w[n]$ 是零均值、**宽平稳**、相互独立的随机过程，这样 $x[n]$ 也是一个零均值、宽平稳的随机过程。Wiener滤波的任务是**从 $x[n]$ 中恢复出原始信号 $s[n]$**，即完成
1. **滤波**：用对应的已知观测值 $x[0], x[1], x[2], \cdots, x[n]$ 来估计对应时刻的信号值 $\theta = s[n]$；
2. **平滑**：用观测值 $\cdots, x[-1], x[0], x[1], x[2], \cdots$ 来估计范围内任一时刻的信号值 $\theta = s[n]$；
3. **预测**：用固定的观测值 $x[0], x[1], x[2], \cdots, x[N-1]$ 来估计未来某一时刻的信号值 $\theta = x[N-1+l]$，其中 $l > 0$。

Wiener滤波基于[[#线性最小均方误差 (LMMSE) 估计]]计算上述三类问题的估计值。

## Wiener滤波的求解

### 滤波

已知观测值 $x[0], x[1], x[2], \cdots, x[n]$，使用LMMSE估计 $\theta = s[n]$。由于信号 $s[n]$ 和噪声 $w[n]$ 是零均值、宽平稳、相互独立的随机过程，LMMSE估计量为
$$
\hat{s}[n] = \hat{\theta} = \mathbb{E} \left[ s[n] \right] + \boldsymbol{C}_{s[n], \v{x}} \boldsymbol{C}_{\v{x}, \v{x}}^{-1} \left( \v{x} - \mathbb{E} \left[ \v{x} \right] \right) = \boldsymbol{C}_{s[n], \v{x}} \boldsymbol{C}_{\v{x}, \v{x}}^{-1} \v{x}
$$
计算涉及的协方差矩阵，有
$$
\begin{align}
& \begin{aligned}
\boldsymbol{C}_{s[n], \v{x}} &= \mathbb{E} \big[ s[n] \big(x[0], x[1], x[2], \cdots, x[n]\big) \big] \\
&= \mathbb{E} \big[ s[n] \big(s[0], s[1], s[2], \cdots, s[n]\big) \big] + \mathbb{E} \big[ s[n] \big(w[0], w[1], w[2], \cdots, w[n]\big) \big] \\
&= \big(\mathbb{E} \left[ s[n] s[0] \right], \mathbb{E} \left[ s[n] s[1] \right], \mathbb{E} \left[ s[n] s[2] \right], \cdots, \mathbb{E} \left[ s[n] s[n] \right]\big)  \\
&= \big(r_{ss}[n], r_{ss}[n-1], r_{ss}[n-2], \cdots, r_{ss}[0]\big) =: \v{r}_{ss}^{\mathrm{T}}
\end{aligned} \\
& \begin{aligned}
\boldsymbol{C}_{\v{x}, \v{x}} &= \mathbb{E} \left[ \v{x} \v{x}^{\mathrm{T}} \right] = \mathbb{E} \left[ (\v{s} + \v{w}) (\v{s} + \v{w})^{\mathrm{T}} \right] = \mathbb{E} \left[ \v{s} \v{s}^{\mathrm{T}} \right] + \mathbb{E} \left[ \v{w} \v{w}^{\mathrm{T}} \right] \\
&= \boldsymbol{C}_{\v{s}, \v{s}} + \boldsymbol{C}_{\v{w}, \v{w}} = \boldsymbol{R}_{\v{s}, \v{s}} + \boldsymbol{R}_{\v{w}, \v{w}} = \boldsymbol{R}_{\v{x}, \v{x}}
\end{aligned}
\end{align}
$$
故
$$
\hat{s}[n] = \v{r}_{ss}^{\mathrm{T}} (\boldsymbol{R}_{\v{s}, \v{s}} + \boldsymbol{R}_{\v{w}, \v{w}})^{-1} \v{x} = \v{r}_{ss}^{\mathrm{T}} \boldsymbol{R}_{\v{x}, \v{x}}^{-1} \v{x}
$$
这一估计量可以看作 **FIR滤波器**的输出，不妨设 $\hat{s}[n] = \sum\limits_{k=0}^{n} h^{(n)}[n-k] x[k] = \v{h}^{\mathrm{T}} \v{x}$，立得滤波器的系数 $\v{h} = \boldsymbol{R}_{\v{x}, \v{x}}^{-1} \v{r}_{ss}$，即满足方程组
$$
\underbrace{ \begin{pmatrix}
r_{xx}[0] & r_{xx}[1] & \cdots & r_{xx}[n] \\
r_{xx}[1] & r_{xx}[0] & \cdots & r_{xx}[n-1] \\
\vdots & \vdots & \ddots & \vdots \\
r_{xx}[n] & r_{xx}[n-1] & \cdots & r_{xx}[0]
\end{pmatrix} }_{ \boldsymbol{R}_{\v{x}, \v{x}} }
\underbrace{ \begin{pmatrix}
h^{(n)}[n] \\ h^{(n)}[n-1] \\ \vdots \\ h^{(n)}[0]
\end{pmatrix} }_{ \v{h} }
= \underbrace{ \begin{pmatrix}
r_{ss}[n] \\ r_{ss}[n-1] \\ \vdots \\ r_{ss}[0]
\end{pmatrix} }_{ \v{r}_{ss} }
$$
$$
\mark{ \boldsymbol{R}_{\v{x}, \v{x}} \v{h} = \v{r}_{ss} }
$$
由 $\boldsymbol{R}_{\v{x}, \v{x}}$ 的对称性，上述方程组又可以写成
$$
\mark{ \begin{pmatrix}
r_{xx}[0] & r_{xx}[1] & \cdots & r_{xx}[n] \\
r_{xx}[1] & r_{xx}[0] & \cdots & r_{xx}[n-1] \\
\vdots & \vdots & \ddots & \vdots \\
r_{xx}[n] & r_{xx}[n-1] & \cdots & r_{xx}[0]
\end{pmatrix}
\begin{pmatrix}
h^{(n)}[0] \\ h^{(n)}[1] \\ \vdots \\ h^{(n)}[n]
\end{pmatrix}
= \begin{pmatrix}
r_{ss}[0] \\ r_{ss}[1] \\ \vdots \\ r_{ss}[n]
\end{pmatrix} }
$$
这称为 **Wiener-{Hopf|霍夫} 滤波方程**，可以通过 Levinson-Durbin算法高效求解。

> [!example] 设计Wiener滤波器：示例
> 
> **提取 $\mathrm{AR(1)}$ 过程信号。**
> 已知 $x[n] = s[n] + w[n]$。其中 $s[n]$ 是一个 $\mathrm{AR}(1)$ 过程，满足递推关系 $s[n] = 0.95s[n-1] + u[n]$，$u[n]$ 是服从正态分布 $\mathcal{N}(0, 1)$ 的Gauss白噪声，$w[n]$ 是服从正态分布 $\mathcal{N}(0, 0.5)$ 的Gauss过程。设计一个长度为 2 的Wiener滤波器来估计 $s[n]$。
> 
> ---
> 
> 求长度为2的Wiener滤波器，需求解Wiener-Hopf滤波方程
> $$
> \begin{pmatrix}
> r_{xx}[0] & r_{xx}[1] \\ r_{xx}[1] & r_{xx}[0]
> \end{pmatrix} \begin{pmatrix}
> h[0] \\ h[1]
> \end{pmatrix} = \begin{pmatrix}
> r_{ss}[0] \\ r_{ss}[1]
> \end{pmatrix}
> $$
> 因此只需求出 $r_{ss}[0]$、$r_{ss}[1]$、$r_{xx}[0]$、$r_{xx}[1]$ 四个自相关函数值。
> 
> **求 $r_{ss}[0]$ 和 $r_{ss}[1]$。**
> 由于 $s[n]$ 是 $\mathrm{AR}(1)$ 过程，满足 $s[n] = a s[n-1] + u[n]$，其中 $a = 0.95$，则
> $$
> \begin{align} 
> & r_{ss}[0] = \mathbb{E} \left[ s^{2}[n] \right] = a^2 \mathbb{E} \left[ s^{2}[n-1] \right] + \mathbb{E} \left[ u^{2}[n] \right] = a^2 r_{ss}[0] + \sigma_u^2
> \implies r_{ss}[0] = \frac{\sigma_u^2}{1 - a^2}  \\
> & r_{ss}[1] = \mathbb{E} \left[ s[n] s[n-1] \right] = a \mathbb{E} \left[ s^{2}[n-1] \right] + \mathbb{E} \left[ u[n] s[n-1] \right] = a r_{ss}[0]
> \end{align}
> $$
> 代入数值计算得
> $$
> r_{ss}[0] = \frac{400}{39}, \qquad
> r_{ss}[1] = \frac{380}{39}
> $$
> 
> **求 $r_{xx}[0]$ 和 $r_{xx}[1]$。**
> 由于 $x[n] = s[n] + w[n]$，且 $s[n] \perp w[n]$，故
> $$
> r_{xx}[k] = r_{ss}[k] + r_{ww}[k]
> $$
> 其中 $w[n] \sim \mathcal{N}(0, 0.5)$ 是白噪声，$r_{ww}[0] = \sigma_w^2 = \frac{1}{2}$，$r_{ww}[1] = 0$，故
> $$
> r_{xx}[0] = r_{ss}[0] + r_{ww}[0] = \frac{839}{78}, \qquad
> r_{xx}[1] = r_{ss}[1] + r_{ww}[1] = \frac{380}{39}
> $$
> 
> 将上述结果代入方程，解得
> $$
> h[0] = \frac{2400}{3239} \approx 0.7410, \qquad
> h[1] = \frac{760}{3239} \approx 0.2346
> $$
> 即该Wiener滤波器为
> $$
> \hat{s}[n] = 0.7410 x[n] + 0.2346 x[n-1]
> $$
> 

### 平滑

已知全部观测值 $\cdots, x[-1], x[0], x[1], x[2], \cdots$，使用LMMSE估计 $\theta = s[n]$。类似地，由零均值，LMMSE估计量可写为 **IIR滤波器**的输出，即
$$
\hat{s}[n] = \sum\limits_{k=-\infty}^{\infty} h[k] x[n-k]
$$
根据**正交原理**，误差与每一个观测数据正交，即 $\mathbb{E} \left[ (s[n] - \hat{s}[n]) x[m] \right] = 0$，从而得到
$$
\begin{align}
r_{ss}[n - m] &= \mathbb{E} \left[ s[n] s[m] \right] = \mathbb{E} \left[ s[n] (s[m] + w[m]) \right] = \mathbb{E} \left[ s[n] x[m] \right] \\
&= \mathbb{E} \left[ \hat{s}[n] x[m] \right] = \mathbb{E} \left[ \sum\limits_{k=-\infty}^{\infty} h[k] x[n-k] \cdot x[m] \right] \\
&= \sum\limits_{k=-\infty}^{\infty} h[k] \cdot \mathbb{E} \left[ x[n-k] x[m] \right] = \sum\limits_{k=-\infty}^{\infty} h[k] r_{xx}[n - m -k]
\end{align}
$$
即
$$
r_{ss}[n] = \sum\limits_{k=-\infty}^{\infty} h[k] r_{xx}[n-k], \qquad \text{i.e.} \qquad r_{ss}[n] = h[n] * r_{xx}[n]
$$
对上述方程两端进行Fourier变换，得到
$$
H(f) = \frac{P_{ss}(f)}{P_{xx}(f)} = \frac{P_{ss}(f)}{P_{ss}(f) + P_{ww}(f)} = \frac{\eta(f)}{\eta(f) + 1}
$$
此即**无限Wiener平滑器**的频率响应，其中 $P_{ss}(f)$、$P_{ww}(f)$ 分别是信号、噪声的功率谱密度，$\eta(f) = \frac{P_{ss}(f)}{P_{ww}(f)}$ 是信噪比。

### 预测

已知固定的观测值 $x[0], x[1], x[2], \cdots, x[N-1]$，使用LMMSE估计 $\theta = x[N-1+l]$，其中 $l > 0$。同样地，由零均值，
$$
\hat{x}[N-1+l] = \boldsymbol{C}_{x[N-1+l], \v{x}} \boldsymbol{C}_{\v{x}, \v{x}}^{-1} \v{x}
$$
其中 $\boldsymbol{C}_{\v{x},\v{x}} = \boldsymbol{R}_{\v{x}, \v{x}}$ 与滤波问题中的相同，而 $\boldsymbol{C}_{x[N-1+l], \v{x}}$ 为
$$
\begin{align} 
\boldsymbol{C}_{x[N-1+l], \v{x}} &= \mathbb{E} \big[ x[N-1+l] \big(x[0], x[1], x[2], \cdots, x[N-1]\big) \big] \\
&= \big(r_{xx}[N-1+l], r_{xx}[N-2+l], r_{xx}[N-3+l], \cdots, r_{xx}[l]\big) =: \v{r}_{xx}^{\mathrm{T}}
\end{align}
$$
因此，预测问题的LMMSE估计量为
$$
\hat{x}[N-1+l] = \v{r}_{xx}^{\mathrm{T}} \boldsymbol{R}_{\v{x}, \v{x}}^{-1} \v{x}
$$
同样将上述估计量看作FIR滤波器的输出，记为 $\hat{x}[N-1+l] = \sum\limits_{k=0}^{N-1} h[N-k] x[k] = \v{h}^{\mathrm{T}} \v{x}$，立得滤波器的系数 $\v{h} = \boldsymbol{R}_{\v{x}, \v{x}}^{-1} \v{r}_{xx}$，即满足方程组
$$
\underbrace{ \begin{pmatrix}
r_{xx}[0] & r_{xx}[1] & \cdots & r_{xx}[N-1] \\
r_{xx}[1] & r_{xx}[0] & \cdots & r_{xx}[N-2] \\
\vdots & \vdots & \ddots & \vdots \\
r_{xx}[N-1] & r_{xx}[N-2] & \cdots & r_{xx}[0]
\end{pmatrix} }_{ \boldsymbol{R}_{\v{x}, \v{x}} }
\underbrace{ \begin{pmatrix}
h[N] \\ h[N-1] \\ \vdots \\ h[1]
\end{pmatrix} }_{ \v{h} }
= \underbrace{ \begin{pmatrix}
r_{xx}[N-1+l] \\ r_{xx}[N-2+l] \\ \vdots \\ r_{xx}[l]
\end{pmatrix} }_{ \v{r}_{xx} }
$$
同样地，上述方程组也可以写成
$$
\begin{pmatrix}
r_{xx}[0] & r_{xx}[1] & \cdots & r_{xx}[N-1] \\
r_{xx}[1] & r_{xx}[0] & \cdots & r_{xx}[N-2] \\
\vdots & \vdots & \ddots & \vdots \\
r_{xx}[N-1] & r_{xx}[N-2] & \cdots & r_{xx}[0]
\end{pmatrix}
\begin{pmatrix}
h[1] \\ h[2] \\ \vdots \\ h[N]
\end{pmatrix}
= \begin{pmatrix}
r_{xx}[l] \\ r_{xx}[l+1] \\ \vdots \\ r_{xx}[l+N-1]
\end{pmatrix}
$$
这称为**线性预测 Wiener-{Hopf|霍夫} 方程**。



# Kalman滤波

## Kalman滤波的任务

### 动态信号模型

考虑一个具有Markov性质的动态系统，其状态 $s[n]$ 满足如下递推关系
$$
s[n] = a s[n-1] + u[n], \qquad n \ge 0
$$
其中，$u[n]$ 是均值为0、方差为 $\sigma_{u}^{2}$ 的Gauss白噪声，称为**驱动噪声**，系统的初始状态 $s[-1] \sim \mathcal{N}(\mu_{s}, \sigma_{s}^{2})$ 与驱动噪声 $u[n]$ 互相独立。这个信号模型称为**一阶Gauss-Markov信号模型**，是一个具有记忆性的随机过程。

### Kalman滤波

Kalman滤波假定信号符合一阶Gauss-Markov信号模型，即有
$$
\begin{cases}
\text{状态方程} & s[n] = a s[n-1] + u[n],  \\
\text{观测方程} & x[n] = s[n] + w[n], 
\end{cases} \qquad n \ge 0
$$
其中，
+ **驱动噪声 $u[n]$** 相互独立，且 $u[n] \sim \mathcal{N}(0, \sigma_{u}^{2})$；
+ **观测噪声 $w[n]$** 相互独立，且 $w[n] \sim \mathcal{N}(0, \sigma_{\mathrm{n}}^{2})$；
+ **初始状态 $s[-1]$** 服从Gauss分布 $\mathcal{N}(0, \sigma_{s}^{2})$；
+ 驱动噪声 $u[n]$、观测噪声 $w[n]$ 与初始状态 $s[-1]$ 互相独立。

Kalman滤波的任务是**从观测信号 $x[0], x[1], x[2], \cdots, x[n]$ 中恢复出原始信号 $s[n]$**。使用[[#最小均方误差 (MMSE) 估计]]，Kalman滤波的估计量为
$$
\hat{\v{s}} = \mathbb{E} [ \v{s} \mid \v{x} ] = \mathbb{E} [\v{s}] + \boldsymbol{C}_{\v{s}, \v{x}} \boldsymbol{C}_{\v{x}, \v{x}}^{-1} \left( \v{x} - \mathbb{E} [\v{x}] \right) = \boldsymbol{C}_{\v{s}, \v{x}} \boldsymbol{C}_{\v{x}, \v{x}}^{-1} \v{x}
$$
由于各个随机变量均为Gauss分布，MMSE估计量等价于LMMSE估计量，加上状态方程的递推关系和Markov性，通过旧估计量可以**更新**得到新估计量，即可以**序贯**计算。

为了区分不同数据条件下所得估计量的不同，记 $\hat{s}[n \mid m]$ 表示在观测数据 $x[0], x[1], \cdots, x[m]$ 的条件下对 $s[n]$ 的估计量。这样，Kalman滤波的任务转换为：**已知上一估计量 $\hat{s}[n-1 \mid n-1]$ 及其最小MSE $M[n-1 \mid n-1]$，获得新观测数据 $x[n]$ 后，计算新的估计量 $\hat{s}[n \mid n]$ 及其最小MSE $M[n \mid n]$**。

## Kalman滤波的序贯实现

### 估计量的分解计算

已知 [[#最小均方误差 (MMSE) 估计#对独立Guass数据矢量可加性|MMSE估计量具有对独立Guass数据矢量的可加性]]，我们希望将 $\hat{s}[n \mid n]$ 分解为**分别依赖于 $x[n]$ 和 $x[0], x[1], \cdots, x[n-1]$ 的两部分**，便于使用之前的估计量 $\hat{s}[n-1 \mid n-1]$。

然而，$x[n] = as[n-1] + u[n] + w[n]$ 与 $x[0], x[1], \cdots, x[n-1]$ 相关，因此无法直接分解。为此，引入**新息 (innovation)** $\t{x}[n]$，定义为
$$
\t{x}[n] = x[n] - \mathbb{E} \left[ x[n] \mid x[0], x[1], \cdots, x[n-1] \right] \triangleq x[n] - \hat{x}[n \mid n-1]
$$
新息 $\t{x}[n]$ 是 $x[n]$ 中不可被之前观测数据 $x[0], x[1], \cdots, x[n-1]$ 预测的部分，因此由LMMSE正交原理知 $\t{x}[n]$ 与 $x[0], x[1], \cdots, x[n-1]$ 相互独立。这样，$\hat{s}[n \mid n]$ 就可以分解为两部分，即
$$
\begin{align}
\hat{s}[n \mid n] &= \mathbb{E} \left[ s[n] \mid x[0], x[1], \cdots, x[n-1], x[n] \right] \\
&= \mathbb{E} \left[ s[n] \mid x[0], x[1], \cdots, x[n-1], \t{x}[n] \right] \\
&= \underbrace{ \mathbb{E} \left[ s[n] \mid x[0], x[1], \cdots, x[n-1] \right] }_{ 先前数据估计 } + \underbrace{ \mathbb{E} \left[ s[n] \mid \t{x}[n] \right] }_{ 新息估计 } 
\end{align}
$$

#### 先前数据估计

由信号模型，$s[n] = a s[n-1] + u[n]$，因此
$$
\begin{align}
\hat{s}[n \mid n-1] &\triangleq \mathbb{E} \left[ s[n] \mid x[0], x[1], \cdots, x[n-1] \right] = \mathbb{E} \left[ a s[n-1] + u[n] \mid x[0], x[1], \cdots, x[n-1] \right] \\
&= a \cdot \mathbb{E} \left[ s[n-1] \mid x[0], x[1], \cdots, x[n-1] \right] + \mathbb{E} \left[ u[n] \mid x[0], x[1], \cdots, x[n-1] \right] \\
&= a \hat{s}[n-1 \mid n-1] + 0 = a \hat{s}[n-1 \mid n-1]
\end{align}
$$
这部分估计量依赖于之前的估计量 $\hat{s}[n-1 \mid n-1]$，因此称为**预测**。

#### 新息估计

直接使用LMMSE公式，有
$$
\mathbb{E} \left[ s[n] \mid \t{x}[n] \right] = \mathbb{E} \left[ s[n] \right] + \frac{\mathrm{Cov} \left( s[n], \t{x}[n] \right)}{\mathrm{Var} \left( \t{x}[n] \right)} \left( \t{x}[n] - \mathbb{E} \left[ \t{x}[n] \right] \right) 
$$
由于 $s[n] = a^{n+1} s[-1] + \sum\limits_{k=0}^{n} a^{n-k} u[k]$ 和 $\t{x}[n] = x[n] - \sum\limits_{k=0}^{n-1} h^{(n)}[n-k] x[k]$ 均为零均值，即 $\mathbb{E} \left[ s[n] \right] = \mathbb{E} \left[ \t{x}[n] \right] = 0$，因此
$$
\mathbb{E} \left[ s[n] \mid \t{x}[n] \right] = \frac{\mathrm{Cov} \left( s[n], \t{x}[n] \right)}{\mathrm{Var} \left( \t{x}[n] \right)} \t{x}[n] =: K[n] \t{x}[n]
$$
这部分估计量依赖于新观测数据 $x[n]$，因此称为**修正**，其中 $K[n]$ 称为 **Kalman增益**。

具体地，$\boldsymbol{C}_{s\t{x}} = \mathrm{Cov}(s[n], \t{x}[n])$ 为
$$
\begin{align}
\mathrm{Cov}(s[n], \t{x}[n]) &= \mathbb{E} \left[ (s[n] - \mathbb{E} \left[ s[n] \right] ) (\t{x}[n] - \mathbb{E} \left[ \t{x}[n] \right]) \right] = \mathbb{E} \left[ s[n] \t{x}[n] \right] \\
&= \mathbb{E} \left[ s[n] (x[n] - \hat{x}[n \mid n-1]) \right] = \mathbb{E} \left[ s[n] (s[n] + w[n] - \hat{s}[n \mid n-1]) \right] \\
&= \mathbb{E} \left[ s[n] (s[n] - \hat{s}[n \mid n-1]) \right] + \cancelto{0}{ \mathbb{E} \left[ s[n] w[n] \right] } \\
&= \mathbb{E} \left[ (s[n] - \hat{s}[n \mid n-1]) (s[n] - \hat{s}[n \mid n-1]) \right] = M[n \mid n-1]
\end{align}
$$
另一边，$\boldsymbol{C}_{\t{x}\t{x}} = \mathrm{Var}(\t{x}[n])$ 为
$$
\begin{align}
\mathrm{Var}(\t{x}[n]) &= \mathbb{E} \left[ (\t{x}[n] - \mathbb{E} \left[ \t{x}[n] \right])^{2} \right] = \mathbb{E} \left[ \t{x}[n]^{2} \right] = \mathbb{E} \left[ (x[n] - \hat{x}[n \mid n-1])^{2} \right] \\
&= \mathbb{E} \left[ (s[n] + w[n] - \hat{s}[n \mid n-1])^{2} \right] \\
&= \mathbb{E} \left[ (s[n] - \hat{s}[n \mid n-1])^{2} \right] + \mathbb{E} \left[ w[n]^{2} \right] + 2 \cdot \cancelto{0}{ \mathbb{E} \left[ (s[n] - \hat{s}[n \mid n-1]) w[n] \right] } \\
&= M[n \mid n-1] + \sigma_{\mathrm{n}}^{2}
\end{align}
$$
于是 **Kalman增益** $K[n]$ 为
$$
K[n] = \frac{\mathrm{Cov} \left( s[n], \t{x}[n] \right)}{\mathrm{Var} \left( \t{x}[n] \right)} = \frac{M[n \mid n-1]}{M[n \mid n-1] + \sigma_{\mathrm{n}}^{2}}
$$
其中 $M[n \mid n-1]$ 是**预测**的最小MSE，即称为**最小预测MSE**，具体为
$$
\begin{align}
M[n \mid n-1] &= \mathbb{E} \left[ (s[n] - \hat{s}[n \mid n-1])^{2} \right] = \mathbb{E} \left[ (a s[n-1] + u[n] - a \hat{s}[n-1 \mid n-1])^{2} \right] \\
&= \mathbb{E} \left[ a^{2} (s[n-1] - \hat{s}[n-1 \mid n-1])^{2} \right] + \mathbb{E} \left[ u[n]^{2} \right] + 2a \cdot \cancelto{0}{ \mathbb{E} \left[ (s[n-1] - \hat{s}[n-1 \mid n-1]) u[n] \right] } \\
&= a^{2} M[n-1 \mid n-1] + \sigma_{u}^{2}
\end{align}
$$

### 估计性能分析

Kalman滤波的估计量 $\hat{s}[n \mid n]$ 的最小MSE $M[n \mid n]$ 为
$$
\begin{align}
M[n \mid n] &= \mathbb{E} \left[ (s[n] - \hat{s}[n \mid n])^{2} \right] = \mathbb{E} \left[ (s[n] - \hat{s}[n \mid n-1] - K[n] \t{x}[n])^{2} \right] \\
&= \mathbb{E} \left[ (s[n] - \hat{s}[n \mid n-1])^{2} \right] + K^{2}[n] \cdot \mathbb{E} \left[ \t{x}[n]^{2} \right] - 2K[n] \cdot \mathbb{E} \left[ (s[n] - \hat{s}[n \mid n-1]) \t{x}[n] \right] \\
&= M[n \mid n-1] + K^{2}[n] \cdot \mathrm{Var}(\t{x}[n]) - 2K[n] \cdot \mathrm{Cov}(s[n], \t{x}[n]) \\
&= M[n \mid n-1] + K[n] \cdot \mathrm{Cov}(s[n], \t{x}[n]) - 2K[n] \cdot \mathrm{Cov}(s[n], \t{x}[n]) \\
&= M[n \mid n-1] - K[n] M[n \mid n-1] = (1 - K[n]) M[n \mid n-1] 
\end{align}
$$
可见，更新修正后的最小MSE $M[n \mid n]$ 比预测的最小MSE $M[n \mid n-1]$ 更小，且Kalman增益 $K[n]$ 越大，修正部分的权重越大，估计量 $\hat{s}[n \mid n]$ 越依赖于新观测数据 $x[n]$，最小MSE $M[n \mid n]$ 越小。

> [!theorem] 标量状态标量观测Kalman滤波
> 对于上述一阶Gauss-Markov信号模型，Kalman滤波的**估计量 $\hat{s}[n \mid n]$** 的更新公式为
> $$
> \hat{s}[n \mid n] = \underbrace{ a \hat{s}[n-1 \mid n-1] }_{ \text{预测} } + \underbrace{ K[n] \cdot (x[n] - a \hat{s}[n-1 \mid n-1]) }_{ \text{修正} }
> $$
> 这一估计的**最小MSE $M[n \mid n]$** 的更新公式为
> $$
> M[n \mid n] = \underbrace{ (1 - K[n]) \cdot \underbrace{ M[n \mid n-1] }_{ \text{预测} } }_{ \text{修正} } = (1 - K[n]) \cdot (a^{2} M[n-1 \mid n-1] + \sigma_{u}^{2})
> $$
> 其中，**Kalman增益 $K[n]$** 为
> $$
> K[n] = \frac{M[n \mid n-1]}{M[n \mid n-1] + \sigma_{\mathrm{n}}^{2}} = \frac{a^{2} M[n-1 \mid n-1] + \sigma_{u}^{2}}{a^{2} M[n-1 \mid n-1] + \sigma_{u}^{2} + \sigma_{\mathrm{n}}^{2}}
> $$
> 上述更新公式的初始条件为 $\hat{s}[-1 \mid -1] = 0$、$M[-1 \mid -1] = \sigma_{s}^{2}$。

对于非零均值信号模型，即起始条件 $s[-1] \sim \mathcal{N}(\mu_{s}, \sigma_{s}^{2})$，上面的Kalman滤波估计量更新公式依然适用，只是初始化条件变为 $\hat{s}[-1 \mid -1] = \mu_{s}$、$M[-1 \mid -1] = \sigma_{s}^{2}$。

## 矢量及扩展Kalman滤波

### 矢量状态标量观测Kalman滤波

假定信号模型为
$$
\begin{cases}
\text{状态方程} & \v{s}[n] = \boldsymbol{A} \v{s}[n-1] + \boldsymbol{B} \v{u}[n],  \\
\text{观测方程} & x[n] = \v{h}^{\mathrm{T}}[n] \v{s}[n] + w[n],
\end{cases} \quad n \ge 0
$$
其中，
+ 状态 $\v{s}[n]$ 为 $p \times 1$ 维矢量，$\boldsymbol{A}$、$\boldsymbol{B}$ 分别为 $p \times p$ 和 $p \times r$ 维已知矩阵，$\v{h}[n]$ 为 $p \times 1$ 维已知矢量；
+ **驱动噪声 $\v{u}[n]$** 为 $r \times 1$ 维矢量，样本之间相互独立，且 $\v{u}[n] \sim \mathcal{N}(\v{0}, \boldsymbol{Q})$；
+ **观测噪声 $w[n]$** 相互独立，且 $w[n] \sim \mathcal{N}(0, \sigma_{\mathrm{n}}^{2})$；
+ **初始状态 $\v{s}[-1]$** 服从Gauss分布 $\mathcal{N}(\v{\mu}_{s}, \boldsymbol{C}_{s})$；
+ 驱动噪声 $\v{u}[n]$、观测噪声 $w[n]$ 与初始状态 $\v{s}[-1]$ 互相独立。

对这一信号模型，Kalman滤波的**估计量 $\hat{s}[n \mid n]$** 的更新公式为
$$
\hat{\v{s}}[n \mid n] = \boldsymbol{A} \hat{\v{s}}[n-1 \mid n-1] + \v{K}[n]  (x[n] - \v{h}^{\mathrm{T}}[n] \boldsymbol{A} \hat{\v{s}}[n-1 \mid n-1])
$$
这一估计的**最小MSE $\boldsymbol{M}[n \mid n]$** 的更新公式为
$$
\boldsymbol{M}[n \mid n] = (\boldsymbol{I} - \v{K}[n] \v{h}^{\mathrm{T}}[n]) \boldsymbol{M}[n \mid n-1] = (\boldsymbol{I} - \v{K}[n] \v{h}^{\mathrm{T}}[n]) (\boldsymbol{A} \boldsymbol{M}[n-1 \mid n-1] \boldsymbol{A}^{\mathrm{T}} + \boldsymbol{B} \boldsymbol{Q} \boldsymbol{B}^{\mathrm{T}})
$$
其中，**Kalman增益 $\v{K}[n]$** 为 $p \times 1$ 维矢量，具体为
$$
\v{K}[n] = \frac{\boldsymbol{M}[n \mid n-1] \v{h}[n]}{\sigma_{\mathrm{n}}^{2} + \v{h}^{\mathrm{T}}[n] \boldsymbol{M}[n \mid n-1] \v{h}[n]} 
= \frac{(\boldsymbol{A} \boldsymbol{M}[n-1 \mid n-1] \boldsymbol{A}^{\mathrm{T}} + \boldsymbol{B} \boldsymbol{Q} \boldsymbol{B}^{\mathrm{T}}) \v{h}[n]}{\sigma_{\mathrm{n}}^{2} + \v{h}^{\mathrm{T}}[n] (\boldsymbol{A} \boldsymbol{M}[n-1 \mid n-1] \boldsymbol{A}^{\mathrm{T}} + \boldsymbol{B} \boldsymbol{Q} \boldsymbol{B}^{\mathrm{T}}) \v{h}[n]}
$$
上述更新公式的初始条件为 $\hat{\v{s}}[-1 \mid -1] = \v{\mu}_{s}$、$\boldsymbol{M}[-1 \mid -1] = \boldsymbol{C}_{s}$。

### 矢量状态矢量观测Kalman滤波

假定信号模型为
$$
\begin{cases}
\text{状态方程} & \v{s}[n] = \boldsymbol{A} \v{s}[n-1] + \boldsymbol{B} \v{u}[n],  \\
\text{观测方程} & \v{x}[n] = \boldsymbol{H}[n] \v{s}[n] + \v{w}[n],
\end{cases} \quad n \ge 0
$$
其中，
+ 状态 $\v{s}[n]$ 为 $p \times 1$ 维矢量，$\boldsymbol{A}$、$\boldsymbol{B}$ 分别为 $p \times p$ 和 $p \times r$ 维已知矩阵，$\boldsymbol{H}[n]$ 为 $M \times p$ 维已知矩阵；
+ **驱动噪声 $\v{u}[n]$** 为 $r \times 1$ 维矢量，样本之间相互独立，且 $\v{u}[n] \sim \mathcal{N}(\v{0}, \boldsymbol{Q})$；
+ **观测噪声 $\v{w}[n]$** 为 $M \times 1$ 维矢量，样本之间相互独立，且 $\v{w}[n] \sim \mathcal{N}(\v{0}, \boldsymbol{C}[n])$；
+ **初始状态 $\v{s}[-1]$** 服从Gauss分布 $\mathcal{N}(\v{\mu}_{s}, \boldsymbol{C}_{s})$；
+ 驱动噪声 $\v{u}[n]$、观测噪声 $\v{w}[n]$ 与初始状态 $\v{s}[-1]$ 互相独立。

对这一信号模型，Kalman滤波的**估计量 $\hat{s}[n \mid n]$** 的更新公式为
$$
\hat{\v{s}}[n \mid n] = \boldsymbol{A} \hat{\v{s}}[n-1 \mid n-1] + \boldsymbol{K}[n]  (\v{x}[n] - \boldsymbol{H}[n] \boldsymbol{A} \hat{\v{s}}[n-1 \mid n-1])
$$
这一估计的**最小MSE $\boldsymbol{M}[n \mid n]$** 的更新公式为
$$
\boldsymbol{M}[n \mid n] = (\boldsymbol{I} - \boldsymbol{K}[n] \boldsymbol{H}[n]) \boldsymbol{M}[n \mid n-1] = (\boldsymbol{I} - \boldsymbol{K}[n] \boldsymbol{H}[n]) (\boldsymbol{A} \boldsymbol{M}[n-1 \mid n-1] \boldsymbol{A}^{\mathrm{T}} + \boldsymbol{B} \boldsymbol{Q} \boldsymbol{B}^{\mathrm{T}})
$$
其中，**Kalman增益 $\boldsymbol{K}[n]$** 为 $p \times M$ 维矩阵，具体为
$$
\boldsymbol{K}[n] = \boldsymbol{M}[n \mid n-1] \boldsymbol{H}^{\mathrm{T}}[n] (\boldsymbol{C}[n] + \boldsymbol{H}[n] \boldsymbol{M}[n \mid n-1] \boldsymbol{H}^{\mathrm{T}}[n])^{-1}
$$
上述更新公式的初始条件为 $\hat{\v{s}}[-1 \mid -1] = \v{\mu}_{s}$、$\boldsymbol{M}[-1 \mid -1] = \boldsymbol{C}_{s}$。

### 扩展Kalman滤波

假定信号模型为非线性的动态系统，即
$$
\begin{cases}
\text{状态方程} & \v{s}[n] = \v{a}(\v{s}[n-1]) + \boldsymbol{B} \v{u}[n],  \\
\text{观测方程} & \v{x}[n] = \v{h}(\v{s}[n]) + \v{w}[n],
\end{cases} \quad n \ge 0
$$
其中，
+ 状态 $\v{s}[n]$ 为 $p \times 1$ 维矢量，$\v{a}(\cdot)$ 和 $\v{h}(\cdot)$ 分别为 $p$ 维、$M$ 维非线性函数，$\boldsymbol{B}$ 为 $p \times r$ 维已知矩阵；
+ **驱动噪声 $\v{u}[n]$** 为 $r \times 1$ 维矢量，样本之间相互独立，且 $\v{u}[n] \sim \mathcal{N}(\v{0}, \boldsymbol{Q})$；
+ **观测噪声 $\v{w}[n]$** 为 $M \times 1$ 维矢量，样本之间相互独立，且 $\v{w}[n] \sim \mathcal{N}(\v{0}, \boldsymbol{C}[n])$；
+ **初始状态 $\v{s}[-1]$** 服从Gauss分布 $\mathcal{N}(\v{\mu}_{s}, \boldsymbol{C}_{s})$；
+ 驱动噪声 $\v{u}[n]$、观测噪声 $\v{w}[n]$ 与初始状态 $\v{s}[-1]$ 互相独立。

对这一信号模型，考虑使用**一阶Taylor级数展开**线性化，得到
$$
\begin{align}
& \v{a}(\v{s}[n-1]) \approx \v{a}(\hat{\v{s}}[n-1 \mid n-1]) + \boldsymbol{A}[n-1] (\v{s}[n-1] - \hat{\v{s}}[n-1 \mid n-1]) \\
& \v{h}(\v{s}[n]) \approx \v{h}(\hat{\v{s}}[n \mid n-1]) + \boldsymbol{H}[n] (\v{s}[n] - \hat{\v{s}}[n \mid n-1])
\end{align}
$$
其中，$\boldsymbol{A}[n-1]$ 和 $\boldsymbol{H}[n]$ 分别为 $\v{a}(\cdot)$ 和 $\v{h}(\cdot)$ 在 $\hat{\v{s}}[n-1 \mid n-1]$ 和 $\hat{\v{s}}[n \mid n-1]$ 处的Jacobian矩阵，即
$$
\boldsymbol{A}[n-1] = \frac{ \partial \v{a} }{ \partial \v{s}[n-1] } \Bigg|_{\v{s}[n-1] = \hat{\v{s}}[n-1 \mid n-1]}, \qquad \boldsymbol{H}[n] = \frac{ \partial \v{h} }{ \partial \v{s}[n] } \Bigg|_{\v{s}[n] = \hat{\v{s}}[n \mid n-1]}
$$
整理得到扩展Kalman滤波的**估计量 $\hat{\v{s}}[n \mid n]$** 的更新公式为
$$
\hat{\v{s}}[n \mid n] = \v{a}(\hat{\v{s}}[n-1 \mid n-1]) + \boldsymbol{K}[n]  (\v{x}[n] - \v{h}(\hat{\v{s}}[n \mid n-1]))
$$
这一估计的**最小MSE $\boldsymbol{M}[n \mid n]$** 的更新公式为
$$
\begin{align} 
\boldsymbol{M}[n \mid n] &= (\boldsymbol{I} - \boldsymbol{K}[n] \boldsymbol{H}[n]) \boldsymbol{M}[n \mid n-1]  \\
&= (\boldsymbol{I} - \boldsymbol{K}[n] \boldsymbol{H}[n]) (\boldsymbol{A}[n-1] \boldsymbol{M}[n-1 \mid n-1] \boldsymbol{A}^{\mathrm{T}}[n-1] + \boldsymbol{B} \boldsymbol{Q} \boldsymbol{B}^{\mathrm{T}}) 
\end{align}
$$
其中，**Kalman增益 $\boldsymbol{K}[n]$** 为 $p \times M$ 维矩阵，具体为
$$
\boldsymbol{K}[n] = \boldsymbol{M}[n \mid n-1] \boldsymbol{H}^{\mathrm{T}}[n] (\boldsymbol{C}[n] + \boldsymbol{H}[n] \boldsymbol{M}[n \mid n-1] \boldsymbol{H}^{\mathrm{T}}[n])^{-1}
$$
预测量 $\hat{\v{s}}[n \mid n-1]$ 为
$$
\hat{\v{s}}[n \mid n-1] = \v{a}(\hat{\v{s}}[n-1 \mid n-1])
$$
最小预测MSE $\boldsymbol{M}[n \mid n-1]$ 为
$$
\boldsymbol{M}[n \mid n-1] = \boldsymbol{A}[n-1] \boldsymbol{M}[n-1 \mid n-1] \boldsymbol{A}^{\mathrm{T}}[n-1] + \boldsymbol{B} \boldsymbol{Q} \boldsymbol{B}^{\mathrm{T}}
$$
上述更新公式的初始条件为 $\hat{\v{s}}[-1 \mid -1] = \v{\mu}_{s}$、$\boldsymbol{M}[-1 \mid -1] = \boldsymbol{C}_{s}$。


# 信号检测

## 信号检测常用概率密度函数及其性质

### Gauss分布

Gauss分布的概率密度函数为
$$
p(x) = \frac{1}{\sqrt{2 \pi \sigma^{2}}} \exp \left( -\frac{(x - \mu)^{2}}{2 \sigma^{2}} \right), \qquad -\infty < x < +\infty
$$
其中 $\mu$ 是均值，$\sigma^{2}$ 是方差，这样的Gauss分布记作 $x \sim \mathcal{N}(\mu, \sigma^{2})$。

进一步地，关于Gauss分布的高阶矩，有以下结论：
+ $\mathbb{E} \left[ x^{n} \right] = \sum\limits_{k=0}^{n} \binom{n}{k} \mathbb{E} \left[ (x - \mu)^{k} \right] \mu^{n-k}$，$\mathbb{E} \left[ (x + \mu)^{n} \right] = \sum\limits_{k=0}^{n} \binom{n}{k} \mathbb{E} \left[ x^{k} \right] \mu^{n-k}$，
+ $\mathbb{E} \left[ (x - \mu)^{k} \right] = \begin{cases} (k - 1)!! \sigma^{k}, & k\text{ 为偶数}, \\ 0, & k\text{ 为奇数} \end{cases}$，
+ $\mu=0$ 时，$\mathbb{E} \left[ x^{n} \right] = \begin{cases} (n - 1)!! \sigma^{n}, & n\text{ 为偶数}, \\ 0, & n\text{ 为奇数} \end{cases}$。 

对于**标准Gauss分布** $x \sim \mathcal{N}(0, 1)$，其累积分布函数记为
$$
\varPhi(x) = \int_{-\infty}^{x} \frac{1}{\sqrt{2 \pi}} \exp \left( -\frac{1}{2} t^{2} \right) \dif t
$$
而其互补累积分布函数记为
$$
Q(x) = \int_{x}^{+\infty} \frac{1}{\sqrt{2 \pi}} \exp \left( -\frac{1}{2} t^{2} \right) \dif t = 1 - \varPhi(x)
$$
立得 $Q$ 函数满足
$$
1 - Q(x) = \varPhi(x) = Q(-x), \qquad Q^{-1}(x) = -Q^{-1}(1-x)
$$

### $\chi^{2}$ 分布

若 $x_{i} \sim \mathcal{N}(0, 1)$，则 $x = \sum\limits_{i=1}^{v} x_{i}^{2}$ 服从**中心 $\chi_{v}^{2}$ 分布**
$$
p(x) = \begin{cases}
\frac{1}{2^{\frac{1}{2} v} \varGamma\left( \frac{v}{2} \right)} x^{\frac{1}{2} v - 1} \e^{-\frac{1}{2}x}, & x > 0, \\
0, & \text{其他}
\end{cases}
$$
其中 $v$ 是自由度，$\varGamma(\cdot)$ 是Gamma函数。中心 $\chi^{2}$ 分布的均值为 $v$，方差为 $2v$。

若 $x_{i} \sim \mathcal{N}(\mu_{i}, 1)$，则 $x = \sum\limits_{i=1}^{v} x_{i}^{2}$ 服从**非中心 $\chi_{v}^{2}(\lambda)$ 分布**
$$
p(x) = \begin{cases}
\frac{1}{2} \left( \frac{x}{\lambda} \right)^{\frac{v-2}{4}} \exp \left( -\frac{x + \lambda}{2} \right) I_{\frac{v}{2} - 1} \left( \sqrt{\lambda x} \right), & x > 0, \\
0, & \text{其他}
\end{cases}
$$
其中 $\lambda = \sum\limits_{i=1}^{v} \mu_{i}^{2}$ 是非中心参数，$I_{\nu}(\cdot)$ 是第一类修正Bessel函数
$$
I_{\nu}(u) = \frac{\left( \frac{1}{2} u \right)^{\nu}}{\sqrt{ \pi } \varGamma\left( \nu + \frac{1}{2} \right)} \dint_{0}^{\pi} \exp \left( u \cos \theta \right) \sin^{2\nu} \theta \dif \theta
= \sum\limits_{k=0}^{\infty} \frac{\left( \frac{1}{2} u \right)^{2k + \nu}}{k! \varGamma(k + \nu + 1)}
$$
非中心 $\chi^{2}$ 分布的均值为 $v + \lambda$，方差为 $2(v + 2\lambda)$。

#### Gauss变量的二次型的分布

设 $\v{x} = (x_{1}, x_{2}, \cdots, x_{n})^{\mathrm{T}} \sim \mathcal{N}(\v{\mu}, \boldsymbol{C})$ 是 $n$ 维Gauss随机向量，$\boldsymbol{A}$ 为 $n \times n$ 的实对称矩阵，则二次型 $y = \v{x}^{\mathrm{T}} \boldsymbol{A} \v{x}$ 的分布特性如下：
1. 如果 $\boldsymbol{A} = \boldsymbol{C}^{-1}$，$\v{\mu} = \v{0}$，则 $y = \v{x}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}$ 服从**中心 $\chi_{n}^{2}$ 分布**；
2. 如果 $\boldsymbol{A} = \boldsymbol{C}^{-1}$，$\v{\mu} \neq \v{0}$，则 $y = \v{x}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}$ 服从**非中心 $\chi_{n}^{2}(\lambda)$ 分布**，其中 $\lambda = \v{\mu}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{\mu}$；
3. 如果 $\boldsymbol{A}$ 是等幂矩阵，即 $\boldsymbol{A}^{2} = \boldsymbol{A}$，且 $\boldsymbol{C} = \boldsymbol{I}$，$\v{\mu} = \v{0}$，则 $y = \v{x}^{\mathrm{T}} \boldsymbol{A} \v{x}$ 服从**中心 $\chi_{r}^{2}$ 分布**，其中 $r$ 是 $\boldsymbol{A}$ 的秩。

### Rayleigh分布、Rice分布

设 $x_{1}, x_{2} \sim \mathcal{N}(0, \sigma^{2})$，则 $x = \sqrt{x_{1}^{2} + x_{2}^{2}}$ 服从 **Rayleigh分布**
$$
p(x) = \begin{cases}
\frac{x}{\sigma^{2}} \exp \left( -\frac{x^{2}}{2 \sigma^{2}} \right), & x > 0, \\
0, & \text{其他}
\end{cases}
$$
Rayleigh分布的均值为 $\sigma \sqrt{\pi / 2}$，方差为 $\frac{4 - \pi}{2} \sigma^{2}$。

设 $x_{1} \sim \mathcal{N}(\mu_{1}, \sigma^{2})$，$x_{2} \sim \mathcal{N}(\mu_{2}, \sigma^{2})$，则 $x = \sqrt{x_{1}^{2} + x_{2}^{2}}$ 服从 **Rice分布**
$$
p(x) = \begin{cases}
\frac{x}{\sigma^{2}} \exp \left( -\frac{x^{2} + \mu_{1}^{2} + \mu_{2}^{2}}{2 \sigma^{2}} \right) I_{0} \left( \frac{x \sqrt{\mu_{1}^{2} + \mu_{2}^{2}}}{\sigma^{2}} \right), & x > 0, \\
0, & \text{其他}
\end{cases}
$$
其中 $I_{0}(\cdot)$ 是第一类修正Bessel函数。Rice分布的均值为 $\sigma \sqrt{\pi / 2} L_{1/2} \left( -\frac{\mu_{1}^{2} + \mu_{2}^{2}}{2 \sigma^{2}} \right)$，方差为 $2 \sigma^{2} + \mu_{1}^{2} + \mu_{2}^{2} - \frac{\pi}{2} \sigma^{2} L_{1/2}^{2} \left( -\frac{\mu_{1}^{2} + \mu_{2}^{2}}{2 \sigma^{2}} \right)$，其中 $L_{\nu}(\cdot)$ 是 $\nu$ 阶Laguerre多项式。

## 检测问题的性能指标

对一个一般的二分类检测问题，定义「无信号」的情况为**零假设**
$$
\mathcal{H}_{0}:\quad x[n] = w[n], \qquad n = 0, 1, 2, \cdots, N-1
$$
而「有信号」的情况为**备择假设**
$$
\mathcal{H}_{1}:\quad x[n] = s[n] + w[n], \qquad n = 0, 1, 2, \cdots, N-1
$$
其中 $s[n]$ 是信号，$w[n]$ 是噪声。这样，可以定义以下概率描述检测的性能：
+ **虚警概率** $P_{\mathrm{FA}} = P(\mathcal{H}_1 ; \mathcal{H}_0)$，即在零假设 $\mathcal{H}_0$ 成立的前提下，检测器错误地判定为备择假设 $\mathcal{H}_1$ 的概率；
+ **漏检概率** $P_{\mathrm{M}} = P(\mathcal{H}_0 ; \mathcal{H}_1)$，即在备择假设 $\mathcal{H}_1$ 成立的前提下，检测器错误地判定为零假设 $\mathcal{H}_0$ 的概率；
+ **检出概率** $P_{\mathrm{D}} = P(\mathcal{H}_1 ; \mathcal{H}_1) = 1 - P_{\mathrm{M}}$，即在备择假设 $\mathcal{H}_1$ 成立的前提下，检测器正确地判定为备择假设 $\mathcal{H}_1$ 的概率。

### Neyman-Pearson准则

一些检测问题中，没有明确的先验知识，判错的代价、判对的收益既难以绝对量化，亦难以相对量化，因此无法使用Bayes准则来设计检测器。此时，可以使用Neyman-Pearson准则来设计检测器。

Neyman-Pearson准则是**在给定虚警概率 $P_{\mathrm{FA}} = \alpha$ 的前提下，最大化检出概率 $P_{\mathrm{D}}$**，或者等价地**在给定虚警概率 $P_{\mathrm{FA}} = \alpha$ 的前提下，最小化漏检概率 $P_{\mathrm{M}}$**。这是一个Lagrange乘子优化问题，引入
$$
J = P_{\mathrm{D}} + \lambda (P_{\mathrm{FA}} - \alpha) = \dint_{R_{1}} p(\v{x} ; \mathcal{H}_1) \dif \v{x} + \lambda \left( \dint_{R_{1}} p(\v{x} ; \mathcal{H}_0) \dif \v{x} - \alpha \right)
$$
其中 $R_{1}$ 是判定为备择假设 $\mathcal{H}_1$ 的区域。显然只要 $p(\v{x} ; \mathcal{H}_1) + \lambda p(\v{x} ; \mathcal{H}_0) > 0$，将此点纳入 $R_{1}$ 就可使 $J$ 增大，因此最优的检测器为**似然比检验 (likelihood ratio test, LRT)**
$$
\mark{ L(x) := \frac{p(\v{x} ; \mathcal{H}_1)}{p(\v{x} ; \mathcal{H}_0)} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} -\lambda =: \gamma }
$$
其中门限 $\gamma$ 可以通过虚警概率 $P_{\mathrm{FA}} = \alpha$ 来确定，即
$$
P_{\mathrm{FA}} = \int_{L(\v{x}) > \gamma} p(\v{x} ; \mathcal{H}_0) \dif \v{x} = \alpha
$$

> [!example] 使用Neyman-Pearson准则检测电平信号：示例
> 考虑检测系统
> $$
> \begin{align}
> & \mathcal{H}_{0}:\quad x[n] = w[n], \qquad n = 0, 1, 2, \cdots, N-1 \\
> & \mathcal{H}_{1}:\quad x[n] = A + w[n], \qquad n = 0, 1, 2, \cdots, N-1
> \end{align}
> $$
> 其中信号 $A>0$，噪声 $w[n]$ 是均值为0、方差为 $\sigma^{2}$ 的Gauss白噪声。使用Neyman-Pearson准则设计检测器，并分析其性能。
> 
> ---
> 
> 
> 欲使用Neyman-Pearson准则设计检测器，首先写出似然比
> $$
> \frac{p(\v{x} ; \mathcal{H}_1)}{p(\v{x} ; \mathcal{H}_0)} = \frac{\prod\limits_{n=0}^{N-1} \frac{1}{\sqrt{2\pi \sigma^{2}}} \exp \left( -\frac{(x[n] - A)^{2}}{2\sigma^{2}} \right)}{\prod\limits_{n=0}^{N-1} \frac{1}{\sqrt{2\pi \sigma^{2}}} \exp \left( -\frac{x^{2}[n]}{2\sigma^{2}} \right)} = \exp \left( \frac{A}{\sigma^{2}} \sum_{n=0}^{N-1} x[n] - \frac{NA^{2}}{2\sigma^{2}} \right) \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma
> $$
> 因此最优的检测器为
> $$
> \frac{1}{N} \sum_{n=0}^{N-1} x[n] \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \frac{A}{2} + \frac{\sigma^{2}}{NA} \ln \gamma =: \gamma'
> $$
> 左侧的统计量称为**检测统计量**，其门限 $\gamma'$ 可整体直接由虚警概率 $P_{\mathrm{FA}}$ 来确定，即
> $$
> P_{\mathrm{FA}} = P \left( \frac{1}{N} \sum_{n=0}^{N-1} x[n] > \gamma' ; \mathcal{H}_0 \right) = Q \left( \frac{\gamma'}{\sigma / \sqrt{N}} \right)
> \implies
> \gamma' = \frac{\sigma}{\sqrt{N}} Q^{-1}(P_{\mathrm{FA}})
> $$
> 相应地，检出概率为
> $$
> P_{\mathrm{D}} = P \left( \frac{1}{N} \sum_{n=0}^{N-1} x[n] > \gamma' ; \mathcal{H}_1 \right) = Q \left( \frac{\gamma' - A}{\sigma / \sqrt{N}} \right) = Q \left( Q^{-1}(P_{\mathrm{FA}}) - \sqrt{ \frac{NA^{2}}{\sigma^{2}} } \right)
> $$
> 这一 $P_{\mathrm{D}}-P_{\mathrm{FA}}$ 关系称为**接收机工作特性 (receiver operating characteristic, ROC)**，是检测器性能的一个重要指标。ROC曲线越接近左上角，说明检测器性能越好。

### 最小错误概率准则

定义**错误概率**
$$
\begin{align} 
P_{\mathrm{e}} &= P(\mathcal{H}_1) P(\mathcal{H}_0 \mid \mathcal{H}_1) + P(\mathcal{H}_0) P(\mathcal{H}_1 \mid \mathcal{H}_0) = P(\mathcal{H}_1) P_{\mathrm{M}} + P(\mathcal{H}_0) P_{\mathrm{FA}}  \\
&= P(\mathcal{H}_1) \int_{R_{0}} p(\v{x} \mid \mathcal{H}_1) \dif \v{x} + P(\mathcal{H}_0) \int_{R_{1}} p(\v{x} \mid \mathcal{H}_0) \dif \v{x} \\
&= P(\mathcal{H}_1) \left( 1 - \dint_{R_{1}} p(\v{x} \mid \mathcal{H}_1) \dif \v{x} \right) + P(\mathcal{H}_0) \dint_{R_{1}} p(\v{x} \mid \mathcal{H}_0) \dif \v{x} \\
&= P(\mathcal{H}_1) + \dint_{R_{1}} \left(P(\mathcal{H}_0) p(\v{x} \mid \mathcal{H}_0) - P(\mathcal{H}_1) p(\v{x} \mid \mathcal{H}_1) \right) \dif \v{x}
\end{align}
$$
对于**判错的代价可量化且相同**的二分类检测问题，可以使用最小错误概率准则，即**最小化错误概率 $P_{\mathrm{e}}$**。显然当 $P(\mathcal{H}_0) p(\v{x} \mid \mathcal{H}_0) - P(\mathcal{H}_1) p(\v{x} \mid \mathcal{H}_1) < 0$ 时，将此点纳入 $R_{1}$ 就可使 $P_{\mathrm{e}}$ 减小，因此最优的检测器同样为**似然比检验**
$$
\mark{ \frac{p(\v{x} \mid \mathcal{H}_1)}{p(\v{x} \mid \mathcal{H}_0)} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \frac{P(\mathcal{H}_0)}{P(\mathcal{H}_1)} }
$$
这实际上是**最大后验概率 (MAP) 检测器**，即
$$
\frac{p(\v{x} \mid \mathcal{H}_1) P(\mathcal{H}_1)}{p(\v{x})} = p(\mathcal{H}_{1} \mid \v{x}) \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} p(\mathcal{H}_{0} \mid \v{x}) = \frac{p(\v{x} \mid \mathcal{H}_0) P(\mathcal{H}_0)}{p(\v{x})}
$$
若先验概率 $P(\mathcal{H}_0)$ 和 $P(\mathcal{H}_1)$ 相等，则MAP退化为**最大似然 (maximum likelihood, ML) 检测器**。

### 最小Bayes风险准则

若漏检、虚警的代价可分别量化为 $C_{01}$、$C_{10}$，判对的收益可分别量化为 $C_{00}$、$C_{11}$，则可以定义 **Bayes风险**
$$
\begin{align}
R &= P(\mathcal{H}_1) \left( C_{11} P(\mathcal{H}_1 \mid \mathcal{H}_1) + C_{01} P(\mathcal{H}_0 \mid \mathcal{H}_1) \right) + P(\mathcal{H}_0) \left( C_{10} P(\mathcal{H}_1 \mid \mathcal{H}_0) + C_{00} P(\mathcal{H}_0 \mid \mathcal{H}_0) \right) \\
&= C_{00} P(\mathcal{H}_0) \left( 1 - \dint_{R_{1}} p(\v{x} \mid \mathcal{H}_0) \dif \v{x} \right) + C_{10} P(\mathcal{H}_0) \dint_{R_{1}} p(\v{x} \mid \mathcal{H}_0) \dif \v{x}  \\
&\hspace{1em}+ C_{01} P(\mathcal{H}_1) \left( 1 - \dint_{R_{1}} p(\v{x} \mid \mathcal{H}_1) \dif \v{x} \right) + C_{11} P(\mathcal{H}_1) \dint_{R_{1}} p(\v{x} \mid \mathcal{H}_1) \dif \v{x} \\
&= C_{00} P(\mathcal{H}_0) + C_{01} P(\mathcal{H}_1) + \dint_{R_{1}} \left( (C_{10} - C_{00}) P(\mathcal{H}_0) p(\v{x} \mid \mathcal{H}_0) - (C_{01} - C_{11}) P(\mathcal{H}_1) p(\v{x} \mid \mathcal{H}_1) \right) \dif \v{x}
\end{align}
$$
最小Bayes风险准则是**最小化Bayes风险 $R$**，而当 $(C_{10} - C_{00}) P(\mathcal{H}_0) p(\v{x} \mid \mathcal{H}_0) - (C_{01} - C_{11}) P(\mathcal{H}_1) p(\v{x} \mid \mathcal{H}_1) < 0$ 时，将此点纳入 $R_{1}$ 就可使 $R$ 减小，又一般有 $C_{10} > C_{00}$、$C_{01} > C_{11}$，因此最优的检测器为
$$
\mark{ \frac{p(\v{x} \mid \mathcal{H}_1)}{p(\v{x} \mid \mathcal{H}_0)} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \frac{(C_{10} - C_{00}) P(\mathcal{H}_0)}{(C_{01} - C_{11}) P(\mathcal{H}_1)} }
$$
特别地，当风险一致，即 $C_{10} - C_{00} = C_{01} - C_{11}$ 时，最优的检测器退化为**最小错误概率检测器**，即 **MAP检测器**；当风险一致且先验概率相等时，最优的检测器退化为 **ML检测器**。

## 假设检验方法

综合以上三种准则，设计检测器的核心都是**似然比检验**。当已知数据的概率密度函数时，似然比比较容易计算，特别地可分为：
+ 概率密度函数中无未知参数时，即「已知」信号检测问题，使用**[[#简单假设检验]]**；
+ 概率密度函数中有未知参数时，即「未知」信号检测问题，使用**[[#复合假设检验]]**。

对于某一假设下数据的概率密度函数未知的情况，可以使用**非参数检验**，如秩检验、符号检验、Wilcoxon秩和检验等。

# 简单假设检验

## 雷达信号检测

雷达信号检测问题可以描述为如下两个假设：
+ $\mathcal{H}_0$ —— 雷达信号中没有目标存在，即只有噪声。
+ $\mathcal{H}_1$ —— 雷达信号中有目标存在，即有反射回波信号，其上叠加噪声。

由于有无目标的先验概率通常未知，且判对判错的代价无法量化，因此应当采用 **Neyman-Pearson准则**来设计检测器。

### Gauss白噪声情况

简化数学模型为
$$
\begin{align} 
\mathcal{H}_0 &:\quad x[n] = w[n], \qquad n = 0, 1, 2, \cdots, N-1  \\
\mathcal{H}_1 &:\quad x[n] = s[n] + w[n], \qquad n = 0, 1, 2, \cdots, N-1
\end{align}
$$
其中 $s[n]$ 已知，$w[n]$ 是均值为0、方差为 $\sigma^{2}$ 的Gauss白噪声。Neyman-Pearson准则检测器为
$$
\begin{align} 
L(\v{x}) := \frac{p(\v{x} ; \mathcal{H}_1)}{p(\v{x} ; \mathcal{H}_0)} 
&= \frac{\prod\limits_{n=0}^{N-1} \frac{1}{\sqrt{2\pi \sigma^{2}}} \exp \left( -\frac{(x[n] - s[n])^{2}}{2\sigma^{2}} \right)}{\prod\limits_{n=0}^{N-1} \frac{1}{\sqrt{2\pi \sigma^{2}}} \exp \left( -\frac{x^{2}[n]}{2\sigma^{2}} \right)}  \\
&= \exp \left( - \frac{1}{2\sigma^{2}} \left( \sum_{n=0}^{N-1} s^{2}[n] - 2 \sum_{n=0}^{N-1} s[n] x[n] \right) \right)
\mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma 
\end{align}
$$
即
$$
\quad\mark{ T(\v{x}) = \sum_{n=0}^{N-1} s[n] x[n] } \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \frac{1}{2} \sum_{n=0}^{N-1} s^{2}[n] + \sigma^{2} \ln \gamma =: \gamma'\quad
$$
其中 $T(\v{x})$ 是检测统计量，$\gamma'$ 是对应的门限。

#### 仿形相关器、匹配滤波器

可以看出，检测统计量 $T(\v{x})$ 是输入信号 $\v{x}$ 与已知信号 $\v{s}$ 的相关积，因此这一检测器可实现为**仿形相关器 (replica-correlator)**。

```tikz
\usepackage{amsmath}

\begin{document}
\Large
\begin{tikzpicture}

\draw[thick, -latex] (0, 0) -- (2, 0) node[above, midway] {$x[n]$} node[circle, anchor=west, draw, thick, minimum size=0.8em] (times) {$\times$};
\draw[thick, latex-] (times.south) -- ++(0, -1) node[below] {$s[n]$};
\draw[thick, -latex] (times) -- (4, 0);
\draw[thick] (4, 0.8) rectangle (5.5, -0.8) node[pos=0.5] {$\sum\limits_{n=0}^{N-1}$};
\draw[thick, -latex] (5.5, 0) -- (7.5, 0) node[above, midway] {$T(\vec{\boldsymbol{x}})$};
\draw[thick] (7.5, 0) +(0, 1.2) rectangle +(2, -1.2) node[pos=0.5, align=right] {$\quad > \gamma'$ \\[10pt] $< \gamma'$};
\draw[thick, -latex] (7.5, 0) ++(2, 0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{1}$};
\draw[thick, -latex] (7.5, 0) ++(2, -0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{0}$};

\end{tikzpicture}
\end{document}
```

取 $h[n] = s[N-1-n]$ 的FIR滤波器，则其输出为
$$
y[n] = \sum_{m=0}^{N-1} h[m] x[n-m] = \sum_{m=0}^{N-1} s[N-1-m] x[n-m]
\implies y[N-1] = \sum_{n=0}^{N-1} s[n] x[n] = T(\v{x})
$$
因此这一检测器也可实现为**匹配滤波器 (matched-filter)**。

```tikz
\usepackage{amsmath}
\usepackage{circuitikz}

\begin{document}
\Large
\begin{tikzpicture}

\draw[thick, -latex] (0, 0) -- (2, 0) node[above, midway] {$x[n]$};
\draw[thick] (2, 0.8) rectangle (5.5, -0.8) node[pos=0.5] {$s[N-1-n]$};
\draw[thick] (5.5, 0) to[cspst] (7, 0) node[below] {$n=N-1$};
\draw[thick, -latex] (6.5, 0) -- (8.5, 0) node[above, midway] {$T(\vec{\boldsymbol{x}})$};
\draw[thick] (8.5, 0) +(0, 1.2) rectangle +(2, -1.2) node[pos=0.5, align=right] {$\quad > \gamma'$ \\[10pt] $< \gamma'$};
\draw[thick, -latex] (8.5, 0) ++(2, 0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{1}$};
\draw[thick, -latex] (8.5, 0) ++(2, -0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{0}$};

\end{tikzpicture}
\end{document}
```

#### 性能分析

可定义**输出信噪比 (output SNR)** 来分析检测器的性能：
$$
\begin{align} 
\mathrm{SNR} &= \frac{\mathbb{E}^{2} \left[ y[N-1] ; \mathcal{H}_1 \right]}{\mathrm{var} \left( y[N-1] ; \mathcal{H}_0 \right)} \\
&= \frac{ \left( \mathbb{E} \left[ \sum\limits_{k=0}^{N-1} (s[k] + w[k]) h[N-1-k] \right] \right)^{2} }{\mathbb{E} \left[ \left( \sum\limits_{k=0}^{N-1} w[k] h[N-1-k] \right)^{2} \right] } 
= \frac{ \left( \sum\limits_{k=0}^{N-1} s[k] h[N-1-k] \right)^{2} }{\mathbb{E} \left[ \left( \sum\limits_{k=0}^{N-1} w[k] h[N-1-k] \right)^{2} \right] } 
= \frac{(\v{h}^{\mathrm{T}} \v{s})^{2}}{\mathbb{E} \left[ (\v{h}^{\mathrm{T}} \v{w})^{2} \right] } \\
&= \frac{(\v{h}^{\mathrm{T}} \v{s})^{2}}{\v{h}^{\mathrm{T}} \mathbb{E} \left[ \v{w} \v{w}^{\mathrm{T}} \right]  \v{h}} = \frac{(\v{h}^{\mathrm{T}} \v{s})^{2}}{\sigma^{2} \v{h}^{\mathrm{T}} \v{h}}
\end{align}
$$
由Cauchy-Schwarz不等式，$(\v{h}^{\mathrm{T}} \v{s})^{2} \le \v{h}^{\mathrm{T}} \v{h} \cdot \v{s}^{\mathrm{T}} \v{s}$，因此 $\mathrm{SNR} \le \frac{\v{s}^{\mathrm{T}} \v{s}}{\sigma^{2}}$。此处等号成立当且仅当 $\v{h} = c \v{s}$，即匹配滤波的输出信噪比最大，且比例系数不会影响检测性能。


### Gauss色噪声情况

简化数学模型为
$$
\begin{align}
\mathcal{H}_0 &:\quad x[n] = w[n], \qquad n = 0, 1, 2, \cdots, N-1  \\
\mathcal{H}_1 &:\quad x[n] = s[n] + w[n], \qquad n = 0, 1, 2, \cdots, N-1
\end{align}
$$
其中 $s[n]$ 已知，$w[n]$ 是均值为0、协方差矩阵为 $\boldsymbol{C}$ 的Gauss色噪声。Neyman-Pearson准则检测器为
$$
\begin{align}
L(\v{x}) := \frac{p(\v{x} ; \mathcal{H}_1)}{p(\v{x} ; \mathcal{H}_0)} 
&= \frac{\frac{1}{(2\pi)^{N/2} |\boldsymbol{C}|^{1/2}} \exp \left( -\frac{1}{2} (\v{x} - \v{s})^{\mathrm{T}} \boldsymbol{C}^{-1} (\v{x} - \v{s}) \right)}{\frac{1}{(2\pi)^{N/2} |\boldsymbol{C}|^{1/2}} \exp \left( -\frac{1}{2} \v{x}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x} \right)}  \\
&= \exp \left( -\frac{1}{2} \left( (\v{x} - \v{s})^{\mathrm{T}} \boldsymbol{C}^{-1} (\v{x} - \v{s}) - \v{x}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x} \right) \right) \\
&= \exp \left( \v{x}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s} - \frac{1}{2} \v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s} \right)
\mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma 
\end{align}
$$
即
$$
\quad\mark{ T(\v{x}) = \v{x}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s} } \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \frac{1}{2} \v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s} + \ln \gamma =: \gamma'\quad
$$
其中 $T(\v{x})$ 是检测统计量，$\gamma'$ 是对应的门限。

可以看出，Gauss噪声有色时，相当于将所得数据与经过 $\boldsymbol{C}^{-1}$ 修正的已知信号 $\boldsymbol{C}^{-1} \v{s}$ 进行相关积，因此这一检测器实现为**广义仿形相关器 (generalized replica-correlator)**，或等价地为**广义匹配滤波器 (generalized matched-filter)**。

```tikz
\usepackage{amsmath}

\begin{document}
\Large
\begin{tikzpicture}

\draw[thick, -latex] (0, 0) -- (2, 0) node[above, midway] {$x[n]$} node[circle, anchor=west, draw, thick, minimum size=0.8em] (times) {$\times$};
\draw[thick, latex-] (times.south) -- ++(0, -1) coordinate (s);
\draw[thick] (s) +(-0.75, 0) rectangle +(0.75, -1.2) node[pos=0.5] {$\boldsymbol{C}^{-1}$};
\draw[thick, latex-] (s) ++(0, -1.2) -- ++(0, -1) node[below] {$s[n]$};
\draw[thick, -latex] (times) -- (4, 0);
\draw[thick] (4, 0.8) rectangle (5.5, -0.8) node[pos=0.5] {$\sum\limits_{n=0}^{N-1}$};
\draw[thick, -latex] (5.5, 0) -- (7.5, 0) node[above, midway] {$T(\vec{\boldsymbol{x}})$};
\draw[thick] (7.5, 0) +(0, 1.2) rectangle +(2, -1.2) node[pos=0.5, align=right] {$\quad > \gamma'$ \\[10pt] $< \gamma'$};
\draw[thick, -latex] (7.5, 0) ++(2, 0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{1}$};
\draw[thick, -latex] (7.5, 0) ++(2, -0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{0}$};

\end{tikzpicture}
\end{document}
```

若令 $\boldsymbol{C}^{-1} = \boldsymbol{D}^{\mathrm{T}} \boldsymbol{D}$，则 $T(\v{x}) = \v{x}^{\mathrm{T}} \boldsymbol{D}^{\mathrm{T}} \boldsymbol{D} \v{s} = (\boldsymbol{D} \v{x})^{\mathrm{T}} (\boldsymbol{D} \v{s})$，因此这一检测器等价于先对输入数据和已知信号进行**预白化 (pre-whitening)** 处理，再通过仿形相关器或匹配滤波器进行相关积，矩阵 $\boldsymbol{D}$ 称为**预白化矩阵 (pre-whitening matrix)**。

```tikz
\usepackage{amsmath}

\begin{document}
\Large
\begin{tikzpicture}

\draw[thick, -latex] (-2.5, 0) -- (-0.5, 0) node[above, midway] {$x[n]$} coordinate (x);
\draw[thick, -latex] (1, 0) -- (2, 0) node[circle, anchor=west, draw, thick, minimum size=0.8em] (times) {$\times$};
\draw[thick] (x) +(0, 0.6) rectangle +(1.5, -0.6) node[pos=0.5] {$\boldsymbol{D}$};
\draw[thick, latex-] (times.south) -- ++(0, -1) coordinate (s);
\draw[thick] (s) +(-0.75, 0) rectangle +(0.75, -1.2) node[pos=0.5] {$\boldsymbol{D}$};
\draw[thick, latex-] (s) ++(0, -1.2) -- ++(0, -1) node[below] {$s[n]$};
\draw[thick, -latex] (times) -- (4, 0);
\draw[thick] (4, 0.8) rectangle (5.5, -0.8) node[pos=0.5] {$\sum\limits_{n=0}^{N-1}$};
\draw[thick, -latex] (5.5, 0) -- (7.5, 0) node[above, midway] {$T(\vec{\boldsymbol{x}})$};
\draw[thick] (7.5, 0) +(0, 1.2) rectangle +(2, -1.2) node[pos=0.5, align=right] {$\quad > \gamma'$ \\[10pt] $< \gamma'$};
\draw[thick, -latex] (7.5, 0) ++(2, 0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{1}$};
\draw[thick, -latex] (7.5, 0) ++(2, -0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{0}$};

\end{tikzpicture}
\end{document}
```

## 通信信号检测

通信信号检测问题可以描述为如下假设：
+ $\mathcal{H}_{0}$ —— 通信信号中发送的是第 0 个符号 $s_{0}[n]$，其上叠加噪声。
+ ……
+ $H_{i}$ —— 通信信号中发送的是第 $i$ 个符号 $s_{i}[n]$ ($0 \le i < M$)，其上叠加噪声。

各个符号的先验概率通常已知，且各种判错情况的代价都相同，因此应当采用**最小错误概率准则**设计检测器。

### 等先验的通信系统

简化数学模型为
$$
H_i :\quad x[n] = s_i[n] + w[n], \qquad n = 0, 1, 2, \cdots, N-1
$$
其中 $s_i[n]$ ($0 \le i < M$) 已知，$w[n]$ 是均值为0、方差为 $\sigma^{2}$ 的Gauss白噪声，即
$$
\begin{align} 
p(\v{x} \mid H_{i}) &= \prod_{n=0}^{N-1} \frac{1}{\sqrt{2\pi \sigma^{2}}} \exp \left( -\frac{(x[n] - s_{i}[n])^{2}}{2\sigma^{2}} \right) 
= \frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - s_{i}[n])^{2} \right) \\
&= \frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} \left\lVert \v{x} - \v{s}_{i} \right\rVert^{2} \right), \qquad i = 0, 1, \cdots, M-1
\end{align}
$$
在先验相同的情况下，最小错误概率准则等价为**最大似然 (ML) 准则**，即为找 $D_{i}^{2} = \left\lVert \v{x} - \v{s}_{i} \right\rVert^{2}$ 最小的 $i$，而
$$
\begin{align}
& D_{i}^{2} = \left\lVert \v{x} - \v{s}_{i} \right\rVert^{2} = \sum_{n=0}^{N-1} (x[n] - s_{i}[n])^{2} = \sum_{n=0}^{N-1} x^{2}[n] + \sum_{n=0}^{N-1} s_{i}^{2}[n] - 2 \sum_{n=0}^{N-1} s_{i}[n] x[n] \\
& \implies T_{i}(\v{x}) := \sum_{n=0}^{N-1} s_{i}[n] x[n] - \frac{1}{2} \sum_{n=0}^{N-1} s_{i}^{2}[n] = \sum_{n=0}^{N-1} s_{i}[n] x[n] - \frac{1}{2} \varepsilon_{i}
\end{align}
$$
其中 $\varepsilon_{i}$ 为第 $i$ 个符号的能量，而ML检测器等价于**找 $T_{i}(\v{x})$ 最大的 $i$**，即将输入数据 $\v{x}$ 与每个符号 $\v{s}_{i}$ 进行相关积，并减去一个与符号能量成正比的常数项，最后选择相关积最大的符号作为检测结果。

### 不等先验的二元通信系统

在先验不相同的情况下，直接使用最小错误概率准则。在二元情况下，假定 $P(\mathcal{H}_{0}) = p_{0}$，$P(\mathcal{H}_{1}) = 1 - p_{0}$，判决器为
$$
\begin{align}
\frac{p(\v{x} \mid \mathcal{H}_1)}{p(\v{x} \mid \mathcal{H}_0)} &= \frac{\frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - s_{1}[n])^{2} \right)}{\frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - s_{0}[n])^{2} \right)} \\
&= \exp \left( -\frac{1}{2\sigma^{2}} \left( \sum_{n=0}^{N-1} (x[n] - s_{1}[n])^{2} - \sum_{n=0}^{N-1} (x[n] - s_{0}[n])^{2} \right) \right) \\
&= \exp \left( \frac{1}{\sigma^{2}} \left( \left( \sum_{n=0}^{N-1} x[n] s_{1}[n] - \frac{1}{2} \sum_{n=0}^{N-1} s_{1}^{2}[n] \right) - \left( \sum_{n=0}^{N-1} x[n] s_{0}[n] - \frac{1}{2} \sum_{n=0}^{N-1} s_{0}^{2}[n] \right) \right) \right) \\
&\mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \frac{P(\mathcal{H}_0)}{P(\mathcal{H}_1)} = \frac{p_{0}}{1 - p_{0}} =: \gamma
\end{align}
$$
即
$$
\left( \sum_{n=0}^{N-1} x[n] s_{1}[n] - \frac{1}{2} \varepsilon_{1} \right) - \left( \sum_{n=0}^{N-1} x[n] s_{0}[n] - \frac{1}{2} \varepsilon_{0} \right) \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \sigma^{2} \ln \gamma =: \gamma'
$$
当 $P(\mathcal{H}_{0}) = P(\mathcal{H}_{1})$ 时，$\gamma' = 0$，退化为前面等先验的情况。

## 随机信号检测

以上两类问题中，信号 $s[n]$ 是已知的确定性信号；而在随机信号检测问题中，信号 $s[n]$ 是一个随机过程，且其统计特性已知。

### 零均值Gauss信号检测

简化数学模型为
$$
\begin{align}
\mathcal{H}_0 &:\quad x[n] = w[n], \qquad n = 0, 1, 2, \cdots, N-1 \\
\mathcal{H}_1 &:\quad x[n] = s[n] + w[n], \qquad n = 0, 1, 2, \cdots, N-1
\end{align}
$$
其中：
+ 信号 $s[n]$ 是均值为0、协方差矩阵为 $\boldsymbol{C}_{s}$ 的Gauss过程；
+ 噪声 $w[n]$ 是均值为0、协方差矩阵为 $\sigma^{2} \boldsymbol{I}$ 的Gauss过程，且与信号 $s[n]$ 相互独立。

因此，$x[n]$ 可表为
$$
\v{x} \sim \begin{cases}
\mathcal{N}(\v{0}, \sigma^{2} \boldsymbol{I}), & \mathcal{H}_0 \\
\mathcal{N}(\v{0}, \boldsymbol{C}_{s} + \sigma^{2} \boldsymbol{I}), & \mathcal{H}_1
\end{cases}
$$
Neyman-Pearson准则检测器即为
$$
\begin{align}
&& L(\v{x}) := \frac{p(\v{x} ; \mathcal{H}_1)}{p(\v{x} ; \mathcal{H}_0)} 
= \frac{\frac{1}{(2\pi)^{N/2} \left\lvert \boldsymbol{C}_{s} + \sigma^{2} \boldsymbol{I} \right\rvert^{1/2}} \exp \left( -\frac{1}{2} \v{x}^{\mathrm{T}} (\boldsymbol{C}_{s} + \sigma^{2} \boldsymbol{I})^{-1} \v{x} \right)}{\frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \v{x}^{\mathrm{T}} \v{x} \right)} &\mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma \\
\implies \hspace{-4em} && -\frac{1}{2} \v{x}^{\mathrm{T}} \left( (\boldsymbol{C}_{s} + \sigma^{2} \boldsymbol{I})^{-1} - \frac{1}{\sigma^{2}} \boldsymbol{I} \right) \v{x} &\mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma'  \\
\implies \hspace{-4em} && \v{x}^{\mathrm{T}} \left( \frac{1}{\sigma^{2}} \left( \frac{1}{\sigma^{2}} \boldsymbol{I} + \boldsymbol{C}_{s}^{-1} \right)^{-1} \right) \v{x} = \v{x}^{\mathrm{T}} \boldsymbol{C}_{s} \left( \boldsymbol{C}_{s} + \sigma^{2} \boldsymbol{I} \right)^{-1} \v{x}
&\mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma''
\end{align}
$$
以上求逆中使用了 **Woodbury矩阵求逆公式**
$$
(\boldsymbol{A} + \boldsymbol{B}\boldsymbol{C}\boldsymbol{D})^{-1} = \boldsymbol{A}^{-1} - \boldsymbol{A}^{-1} \boldsymbol{B} (\boldsymbol{C}^{-1} + \boldsymbol{D} \boldsymbol{A}^{-1} \boldsymbol{B})^{-1} \boldsymbol{D} \boldsymbol{A}^{-1}
$$
得到的检测统计量为
$$
T(\v{x}) = \v{x}^{\mathrm{T}}  \cancelto{\boldsymbol{C}_{sx}}{\boldsymbol{C}_{s}} \big( \cancelto{\boldsymbol{C}_{xx}}{\boldsymbol{C}_{s} + \sigma^{2} \boldsymbol{I}} \big)^{-1} \v{x} 
= \mark{ \v{x}^{\mathrm{T}} \boldsymbol{C}_{sx} \boldsymbol{C}_{xx}^{-1} \v{x} }
= \v{x}^{\mathrm{T}} \hat{\v{s}}_{\text{MMSE}}
$$
其中 $\hat{\v{s}}_{\text{MMSE}} = \boldsymbol{C}_{sx} \boldsymbol{C}_{xx}^{-1} \v{x}$ 是MMSE估计量。因此，这一检测器等价于先对输入数据进行MMSE估计，再通过仿形相关器或匹配滤波器进行相关积，称为**估计器—相关器**结构。MMSE估计的部分可以由 [[#Wiener滤波]]实现。

```tikz
\usepackage{amsmath}
\usepackage{circuitikz}

\begin{document}
\Large
\begin{tikzpicture}

\draw[thick, -latex] (-4, 0) -- (-2, 0) node[above, midway] {$x[n]$} -- (2, 0) node[circle, anchor=west, draw, thick, minimum size=0.8em] (times) {$\times$};
\draw[thick, -latex] (-2, 0) to[short, *-] ++(0, -1.5) -- ++(1, 0) coordinate (x);
\draw[thick] (x) +(0, 0.6) rectangle +(2.5, -0.6) node[pos=0.5] {Wiener};
\draw[thick, -latex] (x) +(2.5, 0) -- (x -| times.south) -- (times.south) node[midway, right] {$\hat{s}[n]$};
\draw[thick, -latex] (times) -- (4, 0);
\draw[thick] (4, 0.8) rectangle (5.5, -0.8) node[pos=0.5] {$\sum\limits_{n=0}^{N-1}$};
\draw[thick, -latex] (5.5, 0) -- (7.5, 0) node[above, midway] {$T(\vec{\boldsymbol{x}})$};
\draw[thick] (7.5, 0) +(0, 1.2) rectangle +(2, -1.2) node[pos=0.5, align=right] {$\quad > \gamma'$ \\[10pt] $< \gamma'$};
\draw[thick, -latex] (7.5, 0) ++(2, 0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{1}$};
\draw[thick, -latex] (7.5, 0) ++(2, -0.5) -- ++(1, 0) node[right] {$\mathcal{H}_{0}$};

\end{tikzpicture}
\end{document}
```

### 一般Gauss信号检测

简化数学模型为
$$
\begin{align}
\mathcal{H}_0 &:\quad x[n] = w[n], \qquad n = 0, 1, 2, \cdots, N-1 \\
\mathcal{H}_1 &:\quad x[n] = s[n] + w[n], \qquad n = 0, 1, 2, \cdots, N-1
\end{align}
$$
其中：
+ 信号 $\v{s}$ 是均值为 $\v{\mu}_{s}$、协方差矩阵为 $\boldsymbol{C}_{s}$ 的Gauss过程；
+ 噪声 $\v{w}$ 是均值为 $\v{0}$、协方差矩阵为 $\boldsymbol{C}_{w}$ 的Gauss过程，且与信号 $\v{s}$ 相互独立。

因此，$x[n]$ 可表为
$$
\v{x} \sim \begin{cases}
\mathcal{N}(\v{0}, \boldsymbol{C}_{w}), & \mathcal{H}_0 \\
\mathcal{N}(\v{\mu}_{s}, \boldsymbol{C}_{s} + \boldsymbol{C}_{w}), & \mathcal{H}_1
\end{cases}
$$
由确定性部分和随机性部分共同组成。Neyman-Pearson准则检测器即为
$$
\frac{p(\v{x} ; \mathcal{H}_1)}{p(\v{x} ; \mathcal{H}_0)} 
= \frac{\frac{1}{(2\pi)^{N/2} \left\lvert \boldsymbol{C}_{s} + \boldsymbol{C}_{w} \right\rvert^{1/2}} \exp \left( -\frac{1}{2} (\v{x} - \v{\mu}_{s})^{\mathrm{T}} (\boldsymbol{C}_{s} + \boldsymbol{C}_{w})^{-1} (\v{x} - \v{\mu}_{s}) \right)}{\frac{1}{(2\pi)^{N/2} |\boldsymbol{C}_{w}|^{1/2}} \exp \left( -\frac{1}{2} \v{x}^{\mathrm{T}} \boldsymbol{C}_{w}^{-1} \v{x} \right)}
\mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma
$$
即检测统计量应取为
$$
\begin{align}
& \begin{aligned} 
T(\v{x}) &= \v{x}^{\mathrm{T}} \boldsymbol{C}_{w}^{-1} \v{x} - (\v{x} - \v{\mu}_{s})^{\mathrm{T}} (\boldsymbol{C}_{s} + \boldsymbol{C}_{w})^{-1} (\v{x} - \v{\mu}_{s}) \\
&= \underbrace{ \v{x}^{\mathrm{T}} \left( \boldsymbol{C}_{w}^{-1} - (\boldsymbol{C}_{s} + \boldsymbol{C}_{w})^{-1} \right) \v{x} + 2 \v{x}^{\mathrm{T}} (\boldsymbol{C}_{s} + \boldsymbol{C}_{w})^{-1} \v{\mu}_{s} }_{ \text{化为 }2T'(\v{x}) } - \underbrace{ \v{\mu}_{s}^{\mathrm{T}} (\boldsymbol{C}_{s} + \boldsymbol{C}_{w})^{-1} \v{\mu}_{s} }_{ \text{常数项} } 
\end{aligned} \\
& \begin{aligned}
T'(\v{x}) &= \frac{1}{2} \v{x}^{\mathrm{T}} \left( \boldsymbol{C}_{w}^{-1} - (\boldsymbol{C}_{s} + \boldsymbol{C}_{w})^{-1} \right) \v{x} + \v{x}^{\mathrm{T}} (\boldsymbol{C}_{s} + \boldsymbol{C}_{w})^{-1} \v{\mu}_{s} \\
&\stackrel{\text{Woodbury}}{=\!=\!=\!=\!=\!=} \mark{ \frac{1}{2} \v{x}^{\mathrm{T}} \boldsymbol{C}_{w}^{-1} \boldsymbol{C}_{s} \left( \boldsymbol{C}_{s} + \boldsymbol{C}_{w} \right)^{-1} \v{x} + \v{x}^{\mathrm{T}} (\boldsymbol{C}_{s} + \boldsymbol{C}_{w})^{-1} \v{\mu}_{s} }
\end{aligned}
\end{align}
$$

+ 当 $\boldsymbol{C}_{s} = \boldsymbol{O}$ 时，$T'(\v{x})$ 只保留后一部分，退化为 $\v{x}^{\mathrm{T}} \boldsymbol{C}_{w}^{-1} \v{\mu}_{s}$，即[[#Gauss色噪声情况|在Gauss色噪声中检测确定性信号]]的**广义匹配滤波器**。
+ 当 $\v{\mu}_{s} = \v{0}$ 时，$T'(\v{x})$ 只保留前一部分，退化为 $\frac{1}{2} \v{x}^{\mathrm{T}} \boldsymbol{C}_{w}^{-1} \underbrace{ \boldsymbol{C}_{s} \left( \boldsymbol{C}_{s} + \boldsymbol{C}_{w} \right)^{-1} \v{x} }_{ \hat{\v{s}}_{\mathrm{MMSE}} }$，即[[#零均值Gauss信号检测]]的**估计器—相关器**结构。

因此这一检测器可以看作是上述两类检测器的推广。



# 复合假设检验

## 含未知参数的检验问题

在[[#简单假设检验]]中，假设 $\mathcal{H}_0$ 和 $\mathcal{H}_1$ 下的概率密度函数是完全已知的。但在另一类检测问题中，概率密度函数中可能含有**未知参数**。例如，在Gauss噪声中检测信号
$$
\begin{align}
\mathcal{H}_0 &:\quad  x[n] = w[n], \qquad n = 0, 1, \cdots, N-1 \\
\mathcal{H}_1 &:\quad  x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
\end{align}
$$
其中 $A$ 未知，$w[n] \sim \mathcal{N}(0, \sigma^{2})$。此时似然比
$$
L(\v{x}) = \frac{p(\v{x}; A, \mathcal{H}_1)}{p(\v{x}; \mathcal{H}_0)} 
= \frac{\frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right)}{\frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} x^{2}[n] \right)} 
= \exp \left( \frac{N A^{2}}{2\sigma^{2}} - \frac{A}{\sigma^{2}} \sum_{n=0}^{N-1} x[n] \right)
$$
即NP检测器为
$$
T(\v{x}) = A \sum_{n=0}^{N-1} x[n] \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \sigma^{2} \ln \gamma + \frac{NA^{2}}{2}
$$
右侧门限中包含未知参数，但**这不一定意味着检测器无法实现**。

### 一致最大势 (UMP) 检验

当 $\mathcal{H}_1$ 下未知参数 $A$ 的符号已知时，如不妨设 $A > 0$，则上述检测器等价于
$$
T'(\v{x}) = \frac{1}{N} \sum_{n=0}^{N-1} x[n] \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \frac{\sigma^{2}}{NA} \ln \gamma + \frac{A}{2} =: \gamma'
$$
$T'(\v{x})$ 在 $\mathcal{H}_0$ 下服从 $\mathcal{N}\left( 0, \frac{\sigma^{2}}{N} \right)$，在 $\mathcal{H}_1$ 下服从 $\mathcal{N}\left( A, \frac{\sigma^{2}}{N} \right)$，因此得虚警概率为
$$
P_{\mathrm{FA}} = P(T(\v{x}) > \gamma' ; \mathcal{H}_0) = Q \left( \frac{\gamma'}{\sigma / \sqrt{N}} \right)
$$
即门限 $\gamma' = \frac{\sigma}{\sqrt{N}} Q^{-1}(P_{\mathrm{FA}})$ 可由给定的虚警概率确定，而**与信号幅度真值无关**，检测器可实现为
$$
T'(\v{x}) = \frac{1}{N} \sum_{n=0}^{N-1} x[n] \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \frac{\sigma}{\sqrt{N}} Q^{-1}(P_{\mathrm{FA}})
$$
这样的检验称为**一致最大势 (uniformly most powerful, UMP) 检验**。

类似地，$A < 0$ 时，检测器为
$$
T'(\v{x}) = \frac{1}{N} \sum_{n=0}^{N-1} x[n] \mathop{\gtrless}\limits_{\mathcal{H}_1}^{\mathcal{H}_0} -\frac{\sigma}{\sqrt{N}} Q^{-1}(P_{\mathrm{FA}})
$$
同样是 UMP 检验。

### 复合假设检验方法

上述两个**单边检验**的情形中，$\mathcal{H}_1$ 下未知参数 $A$ 的符号已知，因此可以构造UMP检验。但当 $A$ 的符号未知时，即 $\mathcal{H}_1: A \neq 0$，则**无法构造UMP检验**，此时需要使用其他方法来处理含未知参数的检测问题，即**复合假设检验**。

## Bayes方法

Bayes方法将未知参数 $\boldsymbol{\theta}$ 视为**随机变量**，赋予其**先验分布** $p(\boldsymbol{\theta})$，然后通过边缘化消除未知参数：
$$
\mark{ L(\v{x}) = \frac{p(\v{x} ; \mathcal{H}_1)}{p(\v{x} ; \mathcal{H}_0)} = \frac{\dint p(\v{x} \mid \boldsymbol{\theta}_{1}; \mathcal{H}_1) p(\boldsymbol{\theta}_{1}) \dif \boldsymbol{\theta}_{1}}{\dint p(\v{x} \mid \boldsymbol{\theta}_{0}; \mathcal{H}_0) p(\boldsymbol{\theta}_{0}) \dif \boldsymbol{\theta}_{0}} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma }
$$
边缘化后的 $p(\v{x} ; H_{i})$ 是仅依赖于观测数据 $\v{x}$ 的确定函数，此后即可按常规的似然比检验处理。

### 贝叶斯方法与GLRT的比较

+ **GLRT**：用 $\max_{\boldsymbol{\theta}} p(\v{x}; \boldsymbol{\theta})$ 代替含未知参数的似然函数，本质是频域方法
+ **贝叶斯方法**：用 $\int p(\v{x} \mid \boldsymbol{\theta}) p(\boldsymbol{\theta}) \dif \boldsymbol{\theta}$ 代替，本质是贝叶斯框架下的方法

贝叶斯方法需要指定未知参数的先验分布。当先验分布缺乏信息时，可使用**无信息先验 (non-informative prior)**（如均匀分布、Jeffreys先验等）。

> [!example] 使用Bayes方法检测直流电平：示例
> 考虑检测系统
> $$
> \begin{align}
> \mathcal{H}_{0} &: x[n] = w[n], \qquad n = 0, 1, \cdots, N-1 \\
> \mathcal{H}_{1} &: x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> \end{align}
> $$
> 其中 $\sigma^{2}$ 已知，信号电平的先验分布为 $A \sim \mathcal{N}(0, \sigma_{A}^{2})$ 且与 $w[n]$ 独立。给出Bayes方法的检测器设计。
>
> ---
>
> 在 $\mathcal{H}_{1}$ 下，将 $A$ 积分消除：
> $$
> \begin{align}
> p(\v{x} ; \mathcal{H}_{1}) &= \int_{-\infty}^{+\infty} p(\v{x} \mid A, \mathcal{H}_{1}) p(A \mid \mathcal{H}_{1}) \dif A \\
> &= \int_{-\infty}^{+\infty} \frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum_{n=0}^{N-1} (x[n] - A)^{2} \right) \cdot \frac{1}{\sqrt{2\pi \sigma_{A}^{2}}} \exp \left( -\frac{A^{2}}{2\sigma_{A}^{2}} \right) \dif A
> \end{align}
> $$
> 通过配平方完成积分（两个Gauss函数的卷积），得 $\mathcal{H}_{1}$ 下 $\v{x} \sim \mathcal{N}(\v{0}, \sigma^{2} \boldsymbol{I} + \sigma_{A}^{2} \v{1} \v{1}^{\mathrm{T}})$。
>
> 这与[[#简单假设检验#随机信号检测|随机信号检测]]的情形一致，检测统计量为
> $$
> T(\v{x}) = \left( \sum_{n=0}^{N-1} x[n] \right)^{2} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma'
> $$

## 广义似然比检验 (GLRT)

当似然函数中含有未知参数时，用其 **MLE** 代替，构造**广义似然比 (generalized likelihood ratio, GLR)**：
$$
\mark{ L_{G}(\v{x}) = \frac{\max\limits_{\boldsymbol{\theta}_{1}} p(\v{x}; \boldsymbol{\theta}_{1}, \mathcal{H}_1)}{\max\limits_{\boldsymbol{\theta}_{0}} p(\v{x}; \boldsymbol{\theta}_{0}, \mathcal{H}_0)} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma }
$$
其中：
+ $\boldsymbol{\theta}_{0}$ 是 $\mathcal{H}_0$ 下的未知参数，$\boldsymbol{\theta}_{1}$ 是 $\mathcal{H}_1$ 下的未知参数
+ 分子和分母分别是 $\mathcal{H}_1$ 和 $\mathcal{H}_0$ 下似然函数的最大值

门限 $\gamma$ 由给定的虚警概率 $P_{\mathrm{FA}} = \alpha$ 确定。

### GLRT的渐近性能

GLRT 虽然没有有限样本下的最优性保证，但具有以下优良的**渐近性质**（当样本量 $N \to \infty$ 时）：

+ GLRT 是**一致的 (consistent)**：当 $N \to \infty$ 时，$P_{\mathrm{FA}} \to 0$ 且 $P_{\mathrm{D}} \to 1$
+ 在满足一定正则条件下，$2 \ln L_{G}(\v{x})$ 在 $\mathcal{H}_0$ 下渐近服从 **$\chi^{2}$ 分布**（Wilks定理）：
  $$
  2 \ln L_{G}(\v{x}) \xrightarrow{\mathcal{H}_0} \chi_{r}^{2}
  $$
  其中自由度 $r = \dim(\boldsymbol{\theta}_{1}) - \dim(\boldsymbol{\theta}_{0})$

### GLRT 用于经典线性模型

考虑**经典线性模型 (classic linear model, CLM)**：
$$
\v{x} = \boldsymbol{H} \boldsymbol{\theta} + \v{w}
$$
其中：
+ $\boldsymbol{H}$ 是 $N \times p$ 的已知观测矩阵
+ $\boldsymbol{\theta}$ 是 $p \times 1$ 的未知参数向量
+ $\v{w} \sim \mathcal{N}(\v{0}, \sigma^{2} \boldsymbol{I})$

要检验：
$$
\mathcal{H}_{0}: \boldsymbol{A} \boldsymbol{\theta} = \v{b} \quad \text{vs.} \quad \mathcal{H}_{1}: \boldsymbol{A} \boldsymbol{\theta} \neq \v{b}
$$
其中 $\boldsymbol{A}$ 是 $r \times p$ 的已知矩阵（行满秩）。

在 $\mathcal{H}_{1}$ 下，$\boldsymbol{\theta}$ 的 MLE 即为 [[#最小方差无偏 (MVU) 估计#经典线性模型的MVU|CLM的MVU估计]]
$$
\hat{\boldsymbol{\theta}}_{1} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \v{x}
$$

在 $\mathcal{H}_{0}$ 下，$\boldsymbol{\theta}$ 满足约束 $\boldsymbol{A} \boldsymbol{\theta} = \v{b}$，其**约束MLE**为
$$
\hat{\boldsymbol{\theta}}_{0} = \hat{\boldsymbol{\theta}}_{1} - (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \left( \boldsymbol{A} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \right)^{-1} (\boldsymbol{A} \hat{\boldsymbol{\theta}}_{1} - \v{b})
$$

若 $\sigma^{2}$ 已知，则GLRT等价于
$$
\mark{ T(\v{x}) = \frac{(\boldsymbol{A} \hat{\boldsymbol{\theta}}_{1} - \v{b})^{\mathrm{T}} \left( \boldsymbol{A} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \right)^{-1} (\boldsymbol{A} \hat{\boldsymbol{\theta}}_{1} - \v{b})}{\sigma^{2}} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma }
$$
在 $\mathcal{H}_{0}$ 下，$T(\v{x}) \sim \chi_{r}^{2}$（中心），在 $\mathcal{H}_{1}$ 下 $T(\v{x}) \sim \chi_{r}^{2}(\lambda)$（非中心），其中 $\lambda = (\boldsymbol{A} \boldsymbol{\theta} - \v{b})^{\mathrm{T}} \left( \boldsymbol{A} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \right)^{-1} (\boldsymbol{A} \boldsymbol{\theta} - \v{b}) / \sigma^{2}$。

若 $\sigma^{2}$ 未知，则GLRT等价于
$$
\mark{ F(\v{x}) = \frac{N - p}{r} \cdot \frac{(\boldsymbol{A} \hat{\boldsymbol{\theta}}_{1} - \v{b})^{\mathrm{T}} \left( \boldsymbol{A} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \right)^{-1} (\boldsymbol{A} \hat{\boldsymbol{\theta}}_{1} - \v{b})}{(\v{x} - \boldsymbol{H} \hat{\boldsymbol{\theta}}_{1})^{\mathrm{T}} (\v{x} - \boldsymbol{H} \hat{\boldsymbol{\theta}}_{1})} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma }
$$
在 $\mathcal{H}_{0}$ 下 $F(\v{x}) \sim F_{r, N-p}$（中心 $F$ 分布），在 $\mathcal{H}_{1}$ 下 $F(\v{x}) \sim F'_{r, N-p}(\lambda)$（非中心 $F$ 分布）。

> [!example] GLRT检测直流电平：示例
> 考虑检测系统
> $$
> \begin{align}
> \mathcal{H}_{0} &: x[n] = w[n], \qquad n = 0, 1, \cdots, N-1 \\
> \mathcal{H}_{1} &: x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> \end{align}
> $$
> 其中 $A$ 未知，$w[n] \sim \mathcal{N}(0, \sigma^{2})$ 且 $\sigma^{2}$ **已知**。使用GLRT设计检测器。
>
> ---
>
> 在 $\mathcal{H}_{0}$ 下，无未知参数，似然函数即为 $p(\v{x}; \mathcal{H}_{0})$。
> 在 $\mathcal{H}_{1}$ 下，$A$ 未知，其 MLE 为 $\hat{A} = \bar{x} = \frac{1}{N} \sum\limits_{n=0}^{N-1} x[n]$。
> 代入似然函数，得GLRT：
> $$
> L_{G}(\v{x}) = \frac{\max\limits_{A} p(\v{x}; A, \mathcal{H}_{1})}{p(\v{x}; \mathcal{H}_{0})}
> = \frac{\frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum\limits_{n=0}^{N-1} (x[n] - \bar{x})^{2} \right)}{\frac{1}{(2\pi \sigma^{2})^{N/2}} \exp \left( -\frac{1}{2\sigma^{2}} \sum\limits_{n=0}^{N-1} x^{2}[n] \right)} 
> = \exp \left( \frac{N \bar{x}^{2}}{2\sigma^{2}} \right) \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma
> $$
> 取对数并化简，得检测统计量
> $$
> T(\v{x}) = |\bar{x}| \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma'
> $$
> 在 $\mathcal{H}_{0}$ 下，$\bar{x} \sim \mathcal{N}(0, \sigma^{2} / N)$，因此门限可由
> $$
> P_{\mathrm{FA}} = 2 Q \left( \frac{\gamma'}{\sigma / \sqrt{N}} \right) = \alpha
> \implies \gamma' = \frac{\sigma}{\sqrt{N}} Q^{-1} \left( \frac{\alpha}{2} \right)
> $$
> 确定。注意这里用了双侧检验（因为 $A$ 可正可负）。

> [!example] GLRT检测直流电平（方差未知）：示例
> 考虑检测系统
> $$
> \begin{align}
> \mathcal{H}_{0} &: x[n] = w[n], \qquad n = 0, 1, \cdots, N-1 \\
> \mathcal{H}_{1} &: x[n] = A + w[n], \qquad n = 0, 1, \cdots, N-1
> \end{align}
> $$
> 其中 $A$ 未知，$w[n] \sim \mathcal{N}(0, \sigma^{2})$ 且 $\sigma^{2}$ **也未知**。使用GLRT设计检测器。
>
> ---
>
> 此时 $\sigma^{2}$ 也是未知参数。在 $\mathcal{H}_{0}$ 下，$\sigma^{2}$ 的 MLE 为 $\hat{\sigma}_{0}^{2} = \frac{1}{N} \sum_{n=0}^{N-1} x^{2}[n]$。
> 在 $\mathcal{H}_{1}$ 下，$A$ 的 MLE 为 $\hat{A} = \bar{x}$，$\sigma^{2}$ 的 MLE 为 $\hat{\sigma}_{1}^{2} = \frac{1}{N} \sum_{n=0}^{N-1} (x[n] - \bar{x})^{2}$。
>
> 代入得GLRT：
> $$
> L_{G}(\v{x}) = \frac{\max\limits_{A, \sigma^{2}} p(\v{x}; A, \sigma^{2}, \mathcal{H}_{1})}{\max\limits_{\sigma^{2}} p(\v{x}; \sigma^{2}, \mathcal{H}_{0})}
> = \frac{\frac{1}{(2\pi \hat{\sigma}_{1}^{2})^{N/2}} \exp \left( -\frac{N}{2} \right)}{\frac{1}{(2\pi \hat{\sigma}_{0}^{2})^{N/2}} \exp \left( -\frac{N}{2} \right)} 
> = \left( \frac{\hat{\sigma}_{0}^{2}}{\hat{\sigma}_{1}^{2}} \right)^{N/2} \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma
> $$
> 注意到 $\hat{\sigma}_{0}^{2} = \hat{\sigma}_{1}^{2} + \bar{x}^{2}$，故
> $$
> L_{G}^{2/N}(\v{x}) = 1 + \frac{\bar{x}^{2}}{\hat{\sigma}_{1}^{2}} = 1 + \frac{1}{N-1} \cdot \frac{N \bar{x}^{2}}{\hat{\sigma}_{1}^{2} / (N-1) \cdot N}
> $$
> 等价于
> $$
> T(\v{x}) = \frac{\bar{x}}{\sqrt{\hat{\sigma}_{1}^{2} / N}} = \frac{\bar{x}}{\sqrt{\frac{1}{N} \sum_{n=0}^{N-1} (x[n] - \bar{x})^{2} / N}} \sim \frac{\bar{x}}{s / \sqrt{N}}
> $$
> 这一统计量在 $\mathcal{H}_{0}$ 下服从自由度为 $N-1$ 的 **Student $t$ 分布**。取绝对值后，GLRT 等价为
> $$
> |T(\v{x})| \mathop{\gtrless}\limits_{\mathcal{H}_0}^{\mathcal{H}_1} \gamma'
> $$
> 这即是经典的 **$t$ 检验 (t-test)**。
