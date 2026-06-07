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

> [!note] 三种Bayes估计量的比较
> MMSE、MAE、MAP三种Bayes估计量分别是**后验分布的均值、中值、最值**，一般并不相等。的别地，对于对称的单峰分布，如 **Gauss分布**，三者相等，**三种估计方法等价**。

## Bayes估计的性能改进

对于白噪声中电平估计的问题，既可以[[最小方差无偏 (MVU) 估计#^Example-MVU-CRLB|使用CRLB求解MVU估计量]]，得到
$$
\hat{A}_{\mathrm{MVU}} = \frac{1}{N} \sum_{n=0}^{N-1} x[n] = \bar{x}, \qquad \mathrm{mse}(\hat{A}_{\mathrm{MVU}}) = \frac{\sigma^{2}}{N}
$$
也可以[[最小均方误差 (MMSE) 估计#^Example-MMSE|基于先验求解MMSE估计量]]，得到
$$
\hat{A}_{\mathrm{MMSE}} = \frac{N\sigma_{A}^{2} \bar{x} + \sigma^{2} \mu_{A}}{N\sigma_{A}^{2} + \sigma^{2}}, \qquad \mathrm{Bmse}(\hat{A}_{\mathrm{MMSE}}) = \frac{\sigma^{2} \sigma_{A}^{2}}{N\sigma_{A}^{2} + \sigma^{2}}
$$
可以看出，MMSE估计量 $\hat{A}_{\mathrm{MMSE}}$ 是 **MVU估计量 $\hat{A}_{\mathrm{MVU}}$ 和先验均值 $\mu_{A}$ 的加权平均**
$$
\begin{align} 
& \hat{A}_{\mathrm{MMSE}} = \cfrac{\frac{N}{\sigma^{2}}}{\frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}}} \hat{A}_{\mathrm{MVU}} + \cfrac{\frac{1}{\sigma_{A}^{2}}}{\frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}}} \mu_{A} = \alpha \hat{A}_{\mathrm{MVU}} + (1-\alpha) \mu_{A}  \\
& \mathrm{Bmse}(\hat{A}_{\mathrm{MMSE}}) = \cfrac{1}{\frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}}}  = \alpha \frac{\sigma^{2}}{N} = \alpha \cdot \mathrm{mse}(\hat{A}_{\mathrm{MVU}})
\end{align}
$$
其权重 $\alpha = \cfrac{\frac{N}{\sigma^{2}}}{\frac{N}{\sigma^{2}} + \frac{1}{\sigma_{A}^{2}}}$ 反映了数据和先验信息的相对可靠性。

对某一次估计，MMSE估计量的MSE为
$$
\begin{align}
\mathrm{mse}(\hat{A}_{\mathrm{MMSE}}) &= \mathbb{E} \left[ (\hat{A}_{\mathrm{MMSE}} - A)^{2} \right] = \mathbb{E} \left[ (\hat{A}_{\mathrm{MMSE}} - \mathbb{E}[\hat{A}_{\mathrm{MMSE}}] + \mathbb{E}[\hat{A}_{\mathrm{MMSE}}] - A)^{2} \right] \\
&= \mathrm{var}(\hat{A}_{\mathrm{MMSE}}) + (\mathbb{E}[\hat{A}_{\mathrm{MMSE}}] - A)^{2}  \\
&= \alpha^{2} \mathrm{var}(\hat{A}_{\mathrm{MVU}}) + (\alpha A + (1-\alpha) \mu_{A} - A)^{2} \\
&= \alpha^{2} \frac{\sigma^{2}}{N} + (1-\alpha)^{2} (A - \mu_{A})^{2}
\end{align}
$$
由于 $\alpha < 1$，显然有
$$
\mathrm{mse}(\hat{A}_{\mathrm{MMSE}}) \Big|_{|A - \mu_{A}| \ll \frac{\sigma}{\sqrt{ N }}} < \mathrm{Bmse}(\hat{A}_{\mathrm{MMSE}}) < \mathrm{mse}(\hat{A}_{\mathrm{MVU}}) \ll \mathrm{mse}(\hat{A}_{\mathrm{MMSE}}) \Big|_{|A - \mu_{A}| \gg \frac{\sigma}{\sqrt{ N }}}
$$

```tikz
\begin{document}
\large
\begin{tikzpicture}

\draw[-latex] (-3, 0) -- (3, 0) node[below] {$A$}; 
\draw[-latex] (-2.5, -0.5) -- (-2.5, 4) node[left] {MSE};

\draw[thick, blue!50] plot[domain=-3:3] (\x, {0.3*pow(\x, 2) + 0.72}) node[right] {$\mathrm{mse}(\hat{A}_{\mathrm{MMSE}})$};
\draw[thick, red!50] plot[domain=-3:3] (\x, {1.2}) node[right] {$\mathrm{Bmse}(\hat{A}_{\mathrm{MMSE}})$};
\draw[thick, orange!50] plot[domain=-3:3] (\x, {2}) node[right] {$\mathrm{mse}(\hat{A}_{\mathrm{MVU}})$};

\draw[dashed] (0, 0.72) -- (0, 0) node[below] {$\mu_{A}$};

\end{tikzpicture}
\end{document}
```

可见，Bayes估计量对估计性能的改进是**平均意义上的改进**。
+ 就某一次估计而言，视真值与先验的匹配程度，MMSE估计量的性能可能优于或劣于MVU估计量；
+ **Bayes MSE是对经典MSE依先验分布的加权平均**，就这一平均意义而言，MMSE估计量的性能优于MVU估计量；
+ 当先验信息不准确时，会起到负面作用，MMSE估计量的性能将远劣于MVU估计量。
