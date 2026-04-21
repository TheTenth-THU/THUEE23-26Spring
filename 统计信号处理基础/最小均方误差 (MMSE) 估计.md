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
> 注意 $\mathbb{E} \left[ (\theta - \hat{\theta})^{2} \right]$ 是一种带有约定俗成性质的简单记号，若交换 $\theta$ 和 $\hat{\theta}$ 的位置则一般表示经典MSE。

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

> [!example]- 求解 MMSE 估计量：示例
> 
> 考虑白噪声中电平估计问题
> $$
> x[n] = A + w[n], \quad n = 0, 1, \dots, N-1
> $$
> 其中待估计参数为信号幅度 $A$，噪声 $w[n]$ 是均值为0、方差为 $\sigma^{2}$ 的Gauss白噪声。假设 $A$ 的先验分布为Gauss分布 $\mathcal{N}(\mu_{A}, \sigma^{2}_{A})$，求 $A$ 的MMSE估计量。
> 
> ---
> 
> 欲求解 $A$ 的MMSE估计量 $\hat{A}$，首先写出 $A$ 的后验分布 $p(A \mid \v{x}) = \frac{p(\v{x} \mid A) p(A)}{\dint_{-\infty}^{+\infty} p(\v{x} \mid A) p(A) \dif A}$，其中
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

### MMSE的性能分析

## 矢量参数MMSE估计

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
