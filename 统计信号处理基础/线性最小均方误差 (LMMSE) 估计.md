## LMMSE估计

类似于[[最佳线性无偏估计 (BLUE)]] 与[[最小方差无偏 (MVU) 估计]]的关系，**线性最小均方误差估计**是**线性**的**最小均方误差**估计，即在满足线性约束的前提下使均方误差最小的估计方法。

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

类似于[[最佳线性无偏估计 (BLUE)]]，LMMSE估计量也**只与待估计参数的一阶矩、二阶矩有关**，因此在实际应用中不需要知道参数 $\theta$ 的完整分布信息。

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

LMMSE仍是MMSE，因此类似于[[最小均方误差 (MMSE) 估计#矢量参数MMSE估计|矢量参数MMSE估计]]通过积分消除无关参数的结论，有
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
