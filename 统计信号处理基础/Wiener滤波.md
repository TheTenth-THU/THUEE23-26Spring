## Wiener滤波的任务

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

