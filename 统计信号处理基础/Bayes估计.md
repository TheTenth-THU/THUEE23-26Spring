## Bayes风险

### 均方误差定义的Bayes风险

为了体现**真值 $\theta$ 范围对估计量选取的影响**，将待估计参数 $\theta$ 视为随机变量，并使用其分布加权经典均方误差，得到的加权均方误差称为 **Bayes均方误差 (Bayes mean square error, Bayes MSE)**，即
$$
\begin{align}
\mathrm{Bmse} (\hat{\theta}) &= \dint \mathbb{E} \left[ (\hat{\theta} - \theta)^{2} \right] p(\theta) \dif \theta
= \dint \left( \dint (\hat{\theta} - \theta)^{2} p(\v{x}; \theta) \dif \v{x} \right)_{\theta} p(\theta) \dif \theta \\
&= \iint (\hat{\theta} - \theta)^{2} p(\v{x} \mid \theta) p(\theta) \dif \v{x} \dif \theta 
= \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta
\end{align}
$$
最小化Bayes MSE得到 $\theta$ 的估计量，称为[[最小均方误差 (MMSE) 估计]]。

Bayes MSE相当于**以 $C(\varepsilon) = \varepsilon^{2}$ 为代价函数**，对所有可能的 $\theta$ 和 $\hat{\theta}$ 的组合进行加权平均得到期望代价
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint (\hat{\theta} - \theta)^{2} p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
称为 **Bayes风险 (Bayes risk)**。

### 绝对误差定义的Bayes风险

同样地，若以 $C(\varepsilon) = |\varepsilon|$ 为代价函数，则得到
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint |\hat{\theta} - \theta| p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
最小化这一Bayes风险得到的估计量称为[[最小绝对误差 (MAE) 估计]]。

### 成功失败型误差定义的Bayes风险

**成功失败型误差 (0-1 loss)** 的代价函数为
$$
C(\varepsilon) = \begin{cases}
0, & \text{if } |\varepsilon| < \delta \\
1, & \text{otherwise}
\end{cases}
$$
因此得到的Bayes风险为
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
最小化这一Bayes风险得到的估计量称为[[最大后验概率 (MAP) 估计]]。