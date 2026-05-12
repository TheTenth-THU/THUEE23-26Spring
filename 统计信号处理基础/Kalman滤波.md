## 动态信号模型

考虑一个具有Markov性质的动态系统，其状态 $s[n]$ 满足如下递推关系
$$
s[n] = a s[n-1] + u[n], \qquad n \ge 0
$$
其中，$u[n]$ 是均值为0、方差为 $\sigma_{u}^{2}$ 的Gauss白噪声，称为**驱动噪声**，系统的初始状态 $s[-1] \sim \mathcal{N}(\mu_{s}, \sigma_{s}^{2})$ 与驱动噪声 $u[n]$ 互相独立。这个信号模型称为**一阶Gauss-Markov信号模型**，是一个具有记忆性的随机过程。

## Kalman滤波的任务

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

Kalman滤波的任务是**从观测信号 $x[0], x[1], x[2], \cdots, x[n]$ 中恢复出原始信号 $s[n]$**。使用[[最小均方误差 (MMSE) 估计]]，Kalman滤波的估计量为
$$
\hat{\v{s}} = \mathbb{E} [ \v{s} \mid \v{x} ] = \mathbb{E} [\v{s}] + \boldsymbol{C}_{\v{s}, \v{x}} \boldsymbol{C}_{\v{x}, \v{x}}^{-1} \left( \v{x} - \mathbb{E} [\v{x}] \right) = \boldsymbol{C}_{\v{s}, \v{x}} \boldsymbol{C}_{\v{x}, \v{x}}^{-1} \v{x}
$$
由于各个随机变量均为Gauss分布，MMSE估计量等价于LMMSE估计量，加上状态方程的递推关系和Markov性，通过旧估计量可以**更新**得到新估计量，即可以**序贯**计算。

为了区分不同数据条件下所得估计量的不同，记 $\hat{s}[n \mid m]$ 表示在观测数据 $x[0], x[1], \cdots, x[m]$ 的条件下对 $s[n]$ 的估计量。这样，Kalman滤波的任务转换为：**已知上一估计量 $\hat{s}[n-1 \mid n-1]$ 及其最小MSE $M[n-1 \mid n-1]$，获得新观测数据 $x[n]$ 后，计算新的估计量 $\hat{s}[n \mid n]$ 及其最小MSE $M[n \mid n]$**。

### 估计量的分解计算

已知 [[最小均方误差 (MMSE) 估计#对独立Guass数据矢量可加性|MMSE估计量具有对独立Guass数据矢量的可加性]]，我们希望将 $\hat{s}[n \mid n]$ 分解为**分别依赖于 $x[n]$ 和 $x[0], x[1], \cdots, x[n-1]$ 的两部分**，便于使用之前的估计量 $\hat{s}[n-1 \mid n-1]$。

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
> \hat{s}[n \mid n] = a \hat{s}[n-1 \mid n-1] + K[n] \cdot (x[n] - a \hat{s}[n-1 \mid n-1])
> $$
> 这一估计的**最小MSE $M[n \mid n]$** 的更新公式为
> $$
> M[n \mid n] = (1 - K[n]) \cdot M[n \mid n-1] = (1 - K[n]) \cdot (a^{2} M[n-1 \mid n-1] + \sigma_{u}^{2})
> $$
> 其中，**Kalman增益 $K[n]$** 为
> $$
> K[n] = \frac{M[n \mid n-1]}{M[n \mid n-1] + \sigma_{\mathrm{n}}^{2}} = \frac{a^{2} M[n-1 \mid n-1] + \sigma_{u}^{2}}{a^{2} M[n-1 \mid n-1] + \sigma_{u}^{2} + \sigma_{\mathrm{n}}^{2}}
> $$
> 上述更新公式的初始条件为 $\hat{s}[-1 \mid -1] = 0$、$M[-1 \mid -1] = \sigma_{s}^{2}$。

对于非零均值信号模型，即起始条件 $s[-1] \sim \mathcal{N}(\mu_{s}, \sigma_{s}^{2})$，上面的Kalman滤波估计量更新公式依然适用，只是初始化条件变为 $\hat{s}[-1 \mid -1] = \mu_{s}$、$M[-1 \mid -1] = \sigma_{s}^{2}$。