## MLE的含义

对观测数据 $\v{x}$，将使得 $p(\v{x}; \theta)$ 达到最大值的参数 $\theta$ 作为估计量，称为 $\theta$ 的**最大似然估计 (maximum likelihood estimation, MLE)**。

最大似然估计直接给出了估计量
$$
\mark{ \hat{\theta} = \arg\max\limits_{\theta} p(\v{x}; \theta) }
$$

### MLE与MVU的关系

假定似然函数可导，则MLE估计量 $\hat{\theta}$ 满足
$$
\left. \frac{\partial \ln p(\v{x}; \theta)}{\partial \theta} \right|_{\theta = \hat{\theta}} = 0
$$
如果存在一个有效统计量 $g(\v{x})$，则由 [[最小方差无偏 (MVU) 估计#标量参数 CRLB 定理|CRLB定理]]知 $g(\v{x})$ 满足
$$
\frac{\partial \ln p(\v{x}; \theta)}{\partial \theta} = \mathcal{I}(\theta) (g(\v{x}) - \theta)
$$
因此 $g(\v{x})$ 也是MLE估计量，即**当存在达到CRLB的MVU估计量时，MLE可以求得该有效统计量**。

### MLE的性能

> [!example] 求解MLE估计量：示例
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
^Example-1

检查[[#^Example-1|上例]]中的MLE估计量 $\hat{A}$ 的性能，首先计算其期望
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
> 如果 $p(\v{x}; \theta)$ 满足一定正则条件，对足够多的数据记录，MLE估计量 $\hat{\theta}$ **渐近服从正态分布**
> $$
> \hat{\theta} \xrightarrow{d} \mathcal{N} \left( \theta, \frac{1}{\mathcal{I}(\theta)} \right)
> $$
> 其中 $\mathcal{I}(\theta)$ 是 Fisher 信息量。
> 

这一性质给出了MLE估计量的渐近性能：
1. MLE是**渐近无偏**的，即 $\mathbb{E} \left[ \hat{\theta} \right] \xrightarrow{N \to \infty} \theta$；
2. MLE是**渐近有效**的，其方差**渐近达到CRLB**，即 $\mathrm{var}(\hat{\theta}) \xrightarrow{N \to \infty} \cfrac{1}{\mathcal{I}(\theta)}$，但有限样本下可大于、等于或小于CRLB。