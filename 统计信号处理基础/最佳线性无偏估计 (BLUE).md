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
\hat{\theta} = \v{a}_{\mathrm{opt}}^{\mathrm{T}} \v{x} = \frac{\v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}}{\v{s}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{s}}
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
\mathrm{var} (\hat{\theta}_{i}) = \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1}_{[i,i]}
$$

注意到，**线性模型的MVU估计量**与**矢量参数BLUE**的表达式基本相同。事实上，有以下定理：

> [!theorem] {Gauss|高斯}-{Markov|马尔可夫} 定理 ^Gauss-Markov-Theorem
> 如果数据具有一般线性模型的形式 
> $$
> \v{x} = \boldsymbol{H} \v{\theta} + \v{w}
> $$
> 其中 $\boldsymbol{H}$ 是一个 $N \times p$ 的已知矩阵，$\v{\theta}$ 是一个 $p$ 维的待估计参数，$\v{w}$ 是一个零均值、协方差矩阵为 $\boldsymbol{C}$ 的{随机|不一定为 Gauss}噪声，则 $\v{\theta}$ 的**BLUE估计量**是
> $$
> \hat{\v{\theta}} = \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \v{x}
> $$
> 其**协方差矩阵**为 $\boldsymbol{C}_{\hat{\v{\theta}}} = \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1}$，即每个分量的方差为 $\mathrm{var} (\hat{\theta}_{i}) = \left( \boldsymbol{H}^{\mathrm{T}} \boldsymbol{C}^{-1} \boldsymbol{H} \right)^{-1}_{[i,i]}$。
> 
> 若 $\v{w}$ 为Gauss噪声，则上述BLUE估计量也是$\v{\theta}$的MVU估计量。

