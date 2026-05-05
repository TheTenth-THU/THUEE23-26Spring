## Wiener滤波

### Wiener滤波的任务

给定被噪声污染的观测信号 $x[n] = s[n] + w[n]$，并假定信号 $s[n]$ 和噪声 $w[n]$ 是零均值、宽平稳、相互独立的随机过程，这样 $x[n]$ 也是一个零均值、宽平稳的随机过程。Wiener滤波的任务是**从 $x[n]$ 中恢复出原始信号 $s[n]$**，即完成
1. **滤波**：用对应的已知观测值 $x[0], x[1], x[2], \cdots, x[n]$ 来估计对应时刻的信号值 $\theta = s[n]$；
2. **平滑**：用固定的观测值 $x[0], x[1], x[2], \cdots, x[N-1]$ 来估计过去范围内的信号值 $\theta = s[n]$，其中 $n < N$；
3. **预测**：用固定的观测值 $x[0], x[1], x[2], \cdots, x[N-1]$ 来估计未来某一时刻的信号值 $\theta = x[N-1+l]$，其中 $l > 0$。

Wiener滤波基于[[线性最小均方误差 (LMMSE) 估计]]计算上述三类问题的估计值。

#### 滤波

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

#### 平滑

已知观测值 $x[0], x[1], x[2], \cdots, x[N-1]$，使用LMMSE估计 $\theta = s[n]$，其中 $n < N$。由于信号 $s[n]$ 和噪声 $w[n]$ 是零均值、宽平稳、相互独立的随机过程，LMMSE估计量为