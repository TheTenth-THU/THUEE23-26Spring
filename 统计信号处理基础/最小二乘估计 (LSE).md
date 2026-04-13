## 最小二乘估计的含义

> [!definition] 最小二乘估计
> 已知信号模型 $s[n;\theta]$，假定观测数据 $x[n]$ 满足 $x[n] = s[n;\theta] + w[n]$，其中 $w[n]$ 是噪声，则 $\theta$ 的**最小二乘估计 (least squares estimation)** 为
> $$
> \hat{\theta} = \arg\min_{\theta} \left\{ \sum_{n=0}^{N-1} (x[n] - s[n;\theta])^{2} \right\} 
> $$
> 其中 $J(\theta) = \sum\limits_{n=0}^{N-1} (x[n] - s[n;\theta])^{2}$ 称为**最小二乘误差**。

与 [[最小方差无偏 (MVU) 估计|MVU]] 和 [[最大似然估计 (MLE)|MLE]] 相比，最小二乘估计没有对**观测数据的概率分布**做出任何假设，而是假定了**观测数据满足的信号模型**。

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
\hat{\v{\theta}} = (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H})^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}
$$
这个结果与 [[最小方差无偏 (MVU) 估计|MVU]]、[[最佳线性无偏估计 (BLUE)|BLUE]]、[[最大似然估计 (MLE)|MLE]] 对有色线性模型的估计结果及 [[最佳线性无偏估计 (BLUE)#^Gauss-Markov-Theorem|Gauss-Markov 定理]]的结论均相同。

### 约束最小二乘估计

部分情形下，未知参数 $\v{\theta}$ 满足某些约束条件，如
$$
\boldsymbol{A} \v{\theta} = \v{b}
$$
此时的最小二乘误差应进一步构成 Lagrange 函数
$$
J_{\mathrm{c}} = (\v{x} - \boldsymbol{H}\v{\theta})^{\mathrm{T}} (\v{x} - \boldsymbol{H}\v{\theta}) + \v{\lambda}^{\mathrm{T}} (\boldsymbol{A} \v{\theta} - \v{b})
$$
约束最小二乘估计量 $\hat{\v{\theta}}_{\mathrm{c}}$ 满足
$$
\frac{ \partial J_{\mathrm{c}} }{ \partial \v{\theta} } \Bigg|_{\hat{\v{\theta}}_{\mathrm{c}}} = \v{0}
\implies 
\hat{\v{\theta}}_{\mathrm{c}} = \hat{\v{\theta}} - (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \underbrace{ \left( \boldsymbol{A} (\boldsymbol{H}^{\mathrm{T}} \boldsymbol{H})^{-1} \boldsymbol{A}^{\mathrm{T}} \right)^{-1} (\boldsymbol{A} \hat{\v{\theta}} - \v{b}) }_{ \tfrac{\v{\lambda}}{2} }
$$
