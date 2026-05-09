## Wiener滤波

### Wiener滤波的任务

给定被噪声污染的观测信号 $x[n] = s[n] + w[n]$，并假定信号 $s[n]$ 和噪声 $w[n]$ 是零均值、**宽平稳**、相互独立的随机过程，这样 $x[n]$ 也是一个零均值、宽平稳的随机过程。Wiener滤波的任务是**从 $x[n]$ 中恢复出原始信号 $s[n]$**，即完成
1. **滤波**：用对应的已知观测值 $x[0], x[1], x[2], \cdots, x[n]$ 来估计对应时刻的信号值 $\theta = s[n]$；
2. **平滑**：用观测值 $\cdots, x[-1], x[0], x[1], x[2], \cdots$ 来估计范围内任一时刻的信号值 $\theta = s[n]$；
3. **预测**：用固定的观测值 $x[0], x[1], x[2], \cdots, x[N-1]$ 来估计未来某一时刻的信号值 $\theta = x[N-1+l]$，其中 $l > 0$。

Wiener滤波基于[[线性最小均方误差 (LMMSE) 估计]]计算上述三类问题的估计值。

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
由 $\boldsymbol{R}_{\v{x}, \v{x}}$ 的对称性，上述方程组又可以写成
$$
\begin{pmatrix}
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
\end{pmatrix}
$$
这称为 **Wiener-{Hopf|霍夫} 滤波方程**，可以通过 Levinson-Durbin算法高效求解。

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

## Kalman滤波

### 动态信号模型

考虑一个具有Markov性质的动态系统，其状态 $s[n]$ 满足如下递推关系
$$
s[n] = a s[n-1] + u[n], \qquad n \ge 0
$$
其中，$u[n]$ 是均值为0、方差为 $\sigma_{u}^{2}$ 的Gauss白噪声，称为**驱动噪声**，系统的初始状态 $s[-1] \sim \mathcal{N}(\mu_{s}, \sigma_{s}^{2})$ 与驱动噪声 $u[n]$ 互相独立。这个信号模型称为**一阶Gauss-Markov信号模型**，是一个具有记忆性的随机过程。

### Kalman滤波的任务

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

#### 估计量的分解计算

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

##### 先前数据估计

由信号模型，$s[n] = a s[n-1] + u[n]$，因此
$$
\begin{align}
\hat{s}[n \mid n-1] &\triangleq \mathbb{E} \left[ s[n] \mid x[0], x[1], \cdots, x[n-1] \right] = \mathbb{E} \left[ a s[n-1] + u[n] \mid x[0], x[1], \cdots, x[n-1] \right] \\
&= a \cdot \mathbb{E} \left[ s[n-1] \mid x[0], x[1], \cdots, x[n-1] \right] + \mathbb{E} \left[ u[n] \mid x[0], x[1], \cdots, x[n-1] \right] \\
&= a \hat{s}[n-1 \mid n-1] + 0 = a \hat{s}[n-1 \mid n-1]
\end{align}
$$
这部分估计量依赖于之前的估计量 $\hat{s}[n-1 \mid n-1]$，因此称为**预测**。

##### 新息估计

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

#### 估计性能分析

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