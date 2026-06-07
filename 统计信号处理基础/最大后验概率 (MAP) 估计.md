## MAP估计

### 成功失败型误差定义的Bayes风险

**成功失败型误差 (0-1 loss)** 的代价函数为
$$
C(\varepsilon) = \begin{cases}
0, & \text{if } |\varepsilon| < \delta \\
1, & \text{otherwise}
\end{cases} = \mathbb{1}(|\varepsilon| \geq \delta)
$$
因此得到的Bayes风险为
$$
\mathfrak{R} = \iint C(\hat{\theta} - \theta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\v{x}, \theta) \dif \v{x} \dif \theta
$$
最小化这一Bayes风险得到的估计量称为**最大后验概率 (maximum a posteriori, MAP) 估计**。

> [!definition] 最大后验概率 (MAP) 估计
> **最大后验概率 (maximum a posteriori, MAP) 估计**是指在所有估计量中，具有最小成功失败型误差定义的Bayes风险的估计量，即
> $$
> \hat{\theta} = \arg\min_{\hat{\theta}} \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\v{x}, \theta) \dif \v{x} \dif \theta
> $$

后面将看到，当 $\delta \to 0$ 时，MAP估计量是参数 $\theta$ 使后验分布 $p(\theta \mid \v{x})$ 取得最大值的点，即 **MAP估计量是参数 $\theta$ 基于后验分布 $p(\theta \mid \v{x})$ 的众数**。因此得名「最大后验概率估计」。

### MAP估计量

尝试分析MAP定义的Bayes风险，类似可有
$$
\begin{align} 
\mathfrak{R} &= \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\v{x}, \theta) \dif \v{x} \dif \theta = \iint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\theta \mid \v{x}) p(\v{x}) \dif \theta \dif \v{x}  \\
&= \int \left( \int \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\theta \mid \v{x}) \dif \theta  \right) p(\v{x}) \dif \v{x} 
\end{align}
$$
要最小化 $\mathfrak{R}$，等价于**在每一个 $\v{x}$ 处通过 $\hat{\theta}(\v{x})$ 最小化 $\dint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\theta \mid \v{x}) \dif \theta$**。将指示函数分段表示，即
$$
\dint \mathbb{1}(|\hat{\theta} - \theta| \geq \delta) p(\theta \mid \v{x}) \dif \theta = \int_{-\infty}^{\hat{\theta} - \delta} p(\theta \mid \v{x}) \dif \theta + \int_{\hat{\theta} + \delta}^{\infty} p(\theta \mid \v{x}) \dif \theta
= 1 - \int_{\hat{\theta} - \delta}^{\hat{\theta} + \delta} p(\theta \mid \v{x}) \dif \theta
$$
因此要求最大化 $\dint_{\hat{\theta} - \delta}^{\hat{\theta} + \delta} p(\theta \mid \v{x}) \dif \theta$，当 $\delta \to 0$ 时，MAP估计量是参数 $\theta$ **使后验分布 $p(\theta \mid \v{x})$ 取得最大值**的点，即
$$
\hat{\theta} = \arg\max_{\hat{\theta}} p(\hat{\theta} \mid \v{x})
= \arg\max_{\hat{\theta}} \frac{p(\v{x} \mid \hat{\theta}) p(\hat{\theta})}{p(\v{x})}
= \arg\max_{\hat{\theta}} p(\v{x} \mid \hat{\theta}) p(\hat{\theta})
= \arg\max_{\hat{\theta}} \left( \ln p(\v{x} \mid \hat{\theta}) + \ln p(\hat{\theta}) \right)
$$

### Bayes MLE估计

当数据量极大时，先验信息的作用会被观测数据的作用所淹没，此时MAP估计量趋近于
$$
\hat{\theta} = \arg\max_{\hat{\theta}} p(\v{x} \mid \hat{\theta}) = \arg\max_{\hat{\theta}} \ln p(\v{x} \mid \hat{\theta})
$$
与经典[[最大似然估计 (MLE)]] 类似，而似然函数是条件概率密度函数 $p(\v{x} \mid \theta)$，因此称这一估计量为 **Bayes MLE估计**，又称**极大似然后验 (maximum a posteriori likelihood, MAPLE) 估计**。

