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

