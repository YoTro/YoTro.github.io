---
title: Box-Muller
date: 2020-06-15 11:23:00 +0800
tags: [random,Algorithm]
music-id: 
youtube-video-ID: 
bilibili-video-ID: 
mathjax: true
---

# [Box-Muller](https://projecteuclid.org/journalArticle/Download?urlId=10.1214%2Faoms%2F1177706645)

公式：$$\begin{matrix}
\text{设, }
U_1,U_2 \sim Uniform(0,1)\\
\left\{\begin{matrix}
z_1=\sqrt{-2 \ln U_1} \cos (2 \pi U_2)\\
z_2=\sqrt{-2 \ln U_1} \sin (2 \pi U_2)
\end{matrix}\right.
\end{matrix}$$

# 为什么它服从[标准正态分布](https://en.wikipedia.org/wiki/Normal_distribution)

证：设$$z_1和z_2$$服从联合概率分布，则 $$\iint P(X=z_1, Y=z_2) = 1$$

$$\begin{align*}
\text{设, }
U_1,U_2 \sim Uniform(0,1)\\
\left\{\begin{matrix}
r &= \sqrt{-2 \ln U_1} \\
\Theta &=2 \pi U_2 \tag{0}
\end{matrix}\right.\\
\text{根据联合概率分布，}P(X=z_1, Y=z_2) &= f(z_1, z_2) \\
&= \frac{1}{\sqrt{2\pi}}e^{-\frac{z_1^2}{2}}\cdot \frac{1}{\sqrt{2\pi}}e^{-\frac{z_2^2}{2}}\\
&= \frac{1}{2\pi}e^{-\frac{z_1^2+z_2^2}{2}}\tag{1}\\
\text{因为，}\left\{\begin{matrix}
 z_1^2=r^2\cos^2(\Theta)\\
z_2^2=r^2\sin^2(\Theta)
\end{matrix}\right.\\
\Longrightarrow z_1^2+z_2^2=r^2\\
\text{所以， } f(z_1, z_2)=\frac{1}{2\pi}e^{-\frac{r^2}{2}}\tag{2}\\
\text{因为， }\left\{\begin{matrix}
 dz_1=d(r\cos\Theta)=\cos\Theta dr + rd\cos\Theta=\cos\Theta dr - r\sin\Theta d\Theta\\
dz_2=d(r\sin\Theta)=\sin\Theta dr + rd\sin\Theta=\sin\Theta dr + r\cos\Theta d\Theta
\end{matrix}\right.\\
\text{所以， }
dz_1 \wedge dz_2&=\cos\Theta \sin\Theta(dr \wedge dr) +  r\cos\Theta \cos\Theta (dr \wedge d\Theta) - r\sin\Theta \sin\Theta (d\Theta \wedge dr) - r\sin\Theta r\cos\Theta (d\Theta \wedge d\Theta) \\
&= 0 + r\cos^2\Theta (dr \wedge d\Theta) - r\sin^2\Theta (d\Theta \wedge dr) - 0\\
&= r\cos^2\Theta (dr \wedge d\Theta) - r\sin^2\Theta (-dr \wedge d\Theta)\\
&= r (dr \wedge d\Theta)\\
\text{对(2)式求积分， }\\
\iint_{\mathbb{R} }^{}  f(z_1, z_2)(dz_1 \wedge dz_2)&=\iint_{\mathbb{R}}^{} \frac{1}{2\pi}e^{-\frac{r^2}{2} }(dz_1 \wedge dz_2)\\
&=\iint_{\mathbb{R}}^{} \frac{1}{2\pi}e^{-\frac{r^2}{2}}r (dr \wedge d\Theta) \\
&=\frac{1}{2\pi}\int_{0}^{2\pi} d\Theta\int_{0}^{\infty } e^{-\frac{r^2}{2}}(r)dr\\
&=\int_{0}^{\infty } e^{-\frac{r^2}{2}}(r)dr\\
\text{令 ， } u = -\frac{r^2}{2} \text{，则 } du = -rdr\\
\text{因为} r \in [0, \infty) \text{, 所以} u = (-\infty, 0]\\
\int_{0}^{\infty } e^{-\frac{r^2}{2}}(r)dr &= -\int_{0}^{-\infty} e^{u}du \\
&=1
\end{align*}$$


# 如何验证某数集符合某种分布

在统计学中，这种叫做拟合优度检验，分为图像法和假设检验法

根据方法的出现时间升序列表，

<div style="overflow-x:auto;">

| 年份 | 方法名称 (中英文) | 类型 | 主要用途 | 作者 | 国籍 | 论文/原始地址 | 概述与优缺点 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1900** | **[皮尔逊卡方检验](https://en.wikipedia.org/wiki/Pearson%27s_chi-squared_test)**<br/>Pearson's Chi-squared Test(χ² test 或 Chi-squared) | 统计检验 | 离散与分组连续分布 | 卡尔·皮尔逊<br/>Karl Pearson | 英国 | *Pearson, K. (1900). On the criterion that a given system of deviations from the probable in the case of a correlated system of variables is such that it can be reasonably supposed to have arisen from random sampling. Philosophical Magazine.* | **最古老、最通用的拟合优度检验**。比较观测频数与期望频数。<br/>**优**: 非常通用，可用于任何分布。<br/>**缺**: 结果依赖分组方式，对连续分布是近似，需要大样本。 |
| **1933** | **[柯尔莫戈洛夫-斯米尔诺夫检验]((https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test))**<br/>Kolmogorov–Smirnov Test (K-S test 或 KS test) | 统计检验 | 完全指定的连续分布 | 安德雷·柯尔莫戈洛夫<br/>Andrey Kolmogorov<br/>尼古拉·斯米尔诺夫<br/>Nikolai Smirnov | 俄罗斯<br/>俄罗斯/苏联 | *Kolmogorov, A. (1933). Sulla determinazione empirica di una legge di distribuzione. G. Ist. Ital. Attuari.* <br/> *Smirnov, N. (1948). Table for estimating the goodness of fit of empirical distributions. Ann. Math. Statist.* | **基于经验分布函数的检验**。计算样本ECDF与理论CDF的最大距离。<br/>**优**: 非参数，不依赖分组，适用于任何连续分布。<br/>**缺**: 对分布中心敏感，对尾部不敏感；参数需已知。 |
| **1952** | **Q-Q图**<br/>Q-Q Plot (Quantile-Quantile Plot) | 可视化 | 所有分布 | 推广使用，无单一发明者 | - | *在统计教材中广泛出现，例如：Wilk, M.B., & Gnanadesikan, R. (1968). Probability plotting methods for the analysis of data. Biometrika.* | **直观的图形化比较工具**。将数据分位数与理论分布分位数画图比较。<br/>**优**: 极其直观，能揭示偏离的具体模式（如偏度、厚尾）。<br/>**缺**: 主观性强，无法给出定量结论。 |
| **1955** | **安德森-达林检验**<br/>[Anderson-Darling Test](https://en.wikipedia.org/wiki/Anderson%E2%80%93Darling_test)(A-D test 或 AD test) | 统计检验 | 多种分布（正态、指数等） | T. W. 安德森<br/>D. A. 达林<br/>T. W. Anderson <br/>D. A. Darling | 美国<br/>美国 | *Anderson, T.W., & Darling, D.A. (1954). A Test of Goodness of Fit. J. of the American Statistical Association.* | **K-S检验的加权改进版**。对分布的**尾部差异**赋予更大权重。<br/>**优**: 对尾部比K-S更敏感，功效通常更高。<br/>**缺**: 对于不同的理论分布，其临界值表不同。 |
| **1965** | **夏皮罗-威尔克检验**<br/>[Shapiro-Wilk Test](https://en.wikipedia.org/wiki/Shapiro%E2%80%93Wilk_test)(S-W test 或 SW test) | 统计检验 | **专门用于正态性检验** | S. S. 夏皮罗<br/>M. B. 威尔克<br/>S. S. Shapiro <br/>M. B. Wilk | 美国<br/>美国 | *Shapiro, S. S., & Wilk, M. B. (1965). An analysis of variance test for normality (complete samples). Biometrika.* | **公认的功效最高的正态性检验之一**。评估数据与理想正态分位数的线性相关性。<br/>**优**: 对小样本尤其有效，是检验正态性的黄金标准。<br/>**缺**: 仅用于正态分布，样本量通常限制在n<5000。 |
| **1967** | **Lilliefors检验**<br/>[Lilliefors Test](https://en.wikipedia.org/wiki/Lilliefors_test) | 统计检验 | 正态分布、指数分布（参数估计） | 休伯特·利利efors<br/>Hubert Lilliefors | 美国 | *Lilliefors, H. W. (1967). On the Kolmogorov-Smirnov test for normality with mean and variance unknown. J. of the American Statistical Association.* | **针对K-S检验的修正**。专门用于当理论分布的参数（如μ, σ）是从样本中估计的情况。<br/>**优**: 解决了K-S检验在参数估计时的滥用问题。<br/>**缺**: 主要用于正态和指数分布。 |
| **1974** | **P-P图**<br/>P-P Plot (Probability-Probability Plot) | 可视化 | 所有分布 | 推广使用，无单一发明者 | - | *与Q-Q图同期在统计文献中被讨论和比较。* | **另一种图形化工具**。比较数据的累积概率与理论分布的累积概率。<br/>**优**: 对分布的位置和尺度参数变化不敏感。<br/>**缺**: 对尾部拟合问题不如Q-Q图敏感，因此**不如Q-Q图常用**。 |
| **1980s** | **克雷默-冯·米塞斯检验**<br/>Cramér–von Mises Criterion(C-vM test 或 CvM test) | 统计检验 | 完全指定的连续分布 | 哈拉尔德·克雷默<br/>Harald Cramér<br/>理查德·冯·米塞斯<br/>Richard Edler von Mises | 瑞典<br/>奥地利/美国 | *历史悠久，其用于拟合优度检验的形式在20世纪下半叶被完善。* | **另一种基于ECDF的检验**。计算ECDF与CDF之间差异的平方积分。<br/>**优**: 利用了整个分布上的差异信息，而不仅是最大差异（如K-S）。<br/>**缺**: 其敏感度介于K-S和A-D之间。 |

</div>

## 图像法

### [Q–Q plot](https://en.wikipedia.org/wiki/Q%E2%80%93Q_plot)

1. 从小到大排序
2. 计算经验累积概率

    为每个有序数据点 $$x_{(i)}$$ 分配一个经验累积概率（或称为“绘图位置”），记为 $$p_i$$。这个概率 $$p_i$$ 代表了 $$x_{(i)}$$ 在整个样本分布中的位置。最常用的绘图位置计算公式是中位数秩次 (Median Rank) 公式或一般形式：$$\text{绘图位置 } p_i = \frac{i - a}{n - 2a + 1}$$

    其中：
    * i 是数据点的秩次，从 1 到 n。
    * n 是样本大小。
    * a 是一个常数，不同的统计软件和方法可能会使用不同的a 值：
        * 标准或平均绘图位置：a = 0.5（最常用，尤其在统计教科书中）$$\text{公式：} p_i = \frac{i - 0.5}{n}$$
        * 中位数秩次（近似）：a = 0.3175（通常用于正态分布的更精确拟合）
        * Weibull 绘图位置：a = 0 $$\text{公式：} p_i = \frac{i}{n + 1}$$

3. 计算理论分位数

    理论分位数函数 Q(p) 是累积分布函数（CDF）F(x) 的逆函数：$$Q(p) = F^{-1}(p)$$

    **常见概率分布：PDF，CDF 与分位数函数**

    <div style="overflow-x:auto;">


    | 分布名称 | 概率密度函数 (PDF) $f(x)$ | 累积分布函数 (CDF) $F(x)$ | 理论分位数函数 $Q(p)=F^{-1}(p)$ |
    | :--- | :--- | :--- | :--- |
    | **[均匀分布](https://en.wikipedia.org/wiki/Continuous_uniform_distribution)** <br/> $$Uniform(a,b)$$ | $$f(x) = \begin{cases} \frac{1}{b-a} & \text{for } a \le x \le b \\ 0 & \text{otherwise} \end{cases}$$ | $$F(x) = \begin{cases} 0 & \text{for } x < a \\ \frac{x-a}{b-a} & \text{for } a \le x < b \\ 1 & \text{for } x \ge b \end{cases}$$ | $$Q(p) = a + p(b-a)$$ |
    | **[指数分布](https://en.wikipedia.org/wiki/Exponential_distribution)** <br/> $$Exp(\lambda)$$ | $$f(x) = \begin{cases} \lambda e^{-\lambda x} & \text{for } x \ge 0 \\ 0 & \text{for } x < 0 \end{cases}$$ | $$F(x) = \begin{cases} 1 - e^{-\lambda x} & \text{for } x \ge 0 \\ 0 & \text{for } x < 0 \end{cases}$$ | $$Q(p) = -\frac{\ln(1-p)}{\lambda}$$ |
    | **[正态分布](https://en.wikipedia.org/wiki/Normal_distribution)** <br/> $$Normal(\mu, \sigma^2)$$ | $$f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$ | $$F(x) = \Phi(\frac{x-\mu}{\sigma})$$ <br/> $$\Phi$$是标准正态CDF，需查表或数值计算 | $$Q(p) = \mu + \sigma \Phi^{-1}(p)$$ <br/> **无封闭形式**，需数值计算 |
    | **标准正态分布** <br/> $$Normal(0, 1)$$ | $$f(x) = \frac{1}{\sqrt{2\pi}} e^{-x^2/2}$$ | $$F(x) = \Phi(x)$$ | $$Q(p) = \Phi^{-1}(p)$$ <br/> **无封闭形式**，需数值计算 |
    | **[帕累托分布](https://en.wikipedia.org/wiki/Pareto_distribution)** <br/> $$Pareto(\alpha, x_m)$$ | $$f(x) = \begin{cases} \frac{\alpha x_m^\alpha}{x^{\alpha+1}} & \text{for } x \ge x_m \\ 0 & \text{for } x < x_m \end{cases}$$ | $$F(x) = \begin{cases} 1 - (\frac{x_m}{x})^\alpha & \text{for } x \ge x_m \\ 0 & \text{for } x < x_m \end{cases}$$ | $$Q(p) = \frac{x_m}{(1-p)^{1/\alpha}}$$ |
    | **[（双参数）威布尔分布](https://en.wikipedia.org/wiki/Weibull_distribution)** <br/> $$Weibull(\alpha, \beta)$$ | $$f(x) = \begin{cases} \frac{\beta}{\alpha} ( \frac{x}{\alpha} )^{\beta-1} e^{-(x/\alpha)^\beta} & \text{for } x \ge 0 \\ 0 & \text{for } x < 0 \end{cases}$$ | $$F(x) = \begin{cases} 1 - e^{-(x/\alpha)^\beta} & \text{for } x \ge 0 \\ 0 & \text{for } x < 0 \end{cases}$$ | $$Q(p) = \alpha [ -\ln(1-p) ]^{1/\beta}$$ |
    | **[标准柯西分布](https://en.wikipedia.org/wiki/Cauchy_distribution)** <br/> $$Cauchy(0, 1)$$ | $$f(x) = \frac{1}{\pi (1 + x^2)}$$ | $$F(x) = \frac{1}{\pi} \arctan(x) + \frac{1}{2}$$ | $$Q(p) = \tan ( \pi (p - \frac{1}{2}) )$$ |
    | **[逻辑斯蒂分布](https://en.wikipedia.org/wiki/Logistic_distribution)** <br/> $$Logistic(\mu, s)$$ | $$f(x) = \frac{e^{-(x-\mu)/s}} {s(1 + e^{-(x-\mu)/s})^2}$$ | $$F(x) = \frac{1}{1 + e^{-(x-\mu)/s}}$$ | $$Q(p) = \mu + s \ln ( \frac{p}{1-p} )$$ |

    </div>


4. 绘图 (Plotting)，点坐标为 (理论分位数, 样本值)
5. 判断拟合度：如果数据符合该理论分布，则图上的点应该大致落在一条直线上

### [P-P plot](https://en.wikipedia.org/wiki/P%E2%80%93P_plot)

比较累积概率： 评估数据的累积分布函数（Cumulative Distribution Function, CDF）与指定的理论分布的 CDF 是否一致。

步骤：
1. 数据排序
2. 计算经验累积概率ECDF (y轴)
$$p_i = \frac{i-a}{N}$$
3. 计算理论累积概率CDF (X 轴坐标)
$$q_i = F(x_i)$$
4. 绘图$$(p_i, q_i)$$再绘制参考线 y=x

```py
import numpy as np
import scipy.stats as stats
import matplotlib.pyplot as plt

# 样本
data = np.random.normal(0, 1, 100)
data_sorted = np.sort(data)
n = len(data)

# 经验CDF概率
p = (np.arange(1, n+1) - 0.5) / n

# 理论CDF（假设标准正态）
q = stats.norm.cdf(data_sorted, loc=0, scale=1)

# 绘图
plt.scatter(p, q)
plt.plot([0,1],[0,1],'r--')
plt.xlabel('Empirical CDF')
plt.ylabel('Theoretical CDF')
plt.title('P–P Plot')
plt.show()
```
### [直方图 (Histogram)](https://en.wikipedia.org/wiki/Histogram)

把数据按区间（bins）划分，再统计每个区间的数据数量。可理解为用矩形近似概率密度。

1. 确定数据的总范围Range
2. 确定箱子数量(k)和宽度(h)
· $$\text{箱子宽度 } h = \frac{Range}{k}$$
· 确定方法
    * 平方根规则适用于各种情况，但粗糙： $$k = \sqrt{n}$$
    * [Scott's rule](https://en.wikipedia.org/wiki/Scott%27s_rule): $$h = 3.5 \cdot \sigma / \sqrt[3]{n}$$
    * [Sturges's rule](https://en.wikipedia.org/wiki/Sturges%27s_rule): $$k = 1 + log_2{n}$$
    * [Freedman–Diaconis rule](https://en.wikipedia.org/wiki/Freedman%E2%80%93Diaconis_rule): $$h = 2 \times \text{IQR}(X) / \sqrt[3]{n}$$
    
    如何计算 [IQR](https://en.wikipedia.org/wiki/Interquartile_range)

    1. 排序: 将所有数据按从小到大排列。
    2. 求中位数 Q2: 找到数据集的中位数。
    3. 求第一四分位数 Q1: 找到中位数左侧一半数据的中位数。
    4. 求第三四分位数 Q3: 找到中位数右侧一半数据的中位数。
    5. 计算 IQR: 用 Q3 减去 Q1，即可得到四分位数间距。
    
· 如何选择

    * 如果数据可能有异常值或明显偏斜，请优先使用 Freedman–Diaconis 规则。
    * 如果数据接近正态分布，Scott's rule是一个非常好的选择。
    * 对于小型数据集或快速探索，平方根规则可以提供一个快速的近似值。
    * Sturges's rule适用于分布近似对称且样本量较小的数据

3. 定义箱子边界$$[b_j, b_{j+1})$$

    箱子j的边界：$$[ \text{Min} + (j-1)h, \text{Min} + j h )，其中 j=1, 2, \dots, k$$

4. 统计频率

    * 遍历数据集中的每个数据点 x_i。
    * 将 x_i 归入其所属的箱子 j 中（通常约定左闭右开，即 $$b_j \le x_i < b_{j+1}$$）。
    * 记录每个箱子的计数 (Count) 或频率 (Frequency)。

5. 绘制直方图

### 密度图 (Density Plot)


## 假设检验法

通过计算一个检验统计量和对应的 p-值来做出客观决策

### [Pearson's Chi-squared Test](https://en.wikipedia.org/wiki/Pearson%27s_chi-squared_test)

一种常用的非参数检验方法，用于检验分类变量之间的关系，或者检验一个分类变量的观察频率是否与预期频率显著不同。

假设我们想研究**性别**（男性/女性）和对某个新产品的**偏好程度**（喜欢/不喜欢）之间是否存在关联。

示例数据：列联表（观察频率 $$O_{ij}$$）

我们随机调查了 100 人，得到以下观察频率的列联表：

| 性别 | 喜欢 (C_1) | 不喜欢 (C_2) | 行总和 |
| :---: | :---: | :---: | :---: |
| **男性 (R_1)** | 30 | 20 | **50** |
| **女性 (R_2)** | 35 | 15 | **50** |
| **列总和** | **65** | **35** | **N = 100** |

---

卡方检验计算流程

1. 设定假设

    * **[零假设](https://en.wikipedia.org/wiki/Null_hypothesis) (H_0)：** 性别和产品偏好之间**相互独立**（没有关联）。
    * **[备择假设](https://en.wikipedia.org/wiki/Alternative_hypothesis) (H_a)：** 性别和产品偏好之间**不独立**（存在关联）。

2. 计算期望频率 $$E_{ij}$$

    期望频率 $$E_{ij}$$ 是假设 H_0 成立时，每个单元格应有的频数。
    $$E_{ij} = \frac{(\text{行总和}) \times (\text{列总和})}{N}$$

    | 单元格 | 行总和 x 列总和 | 除以总数 N=100 | 期望频率 E_{ij} |
    | :---: | :---: | :---: | :---: |
    | **男性/喜欢 ($$E_{11}$$)** | $$50 \times 65$$ | $$3250 / 100$$ | **32.5** |
    | **男性/不喜欢 ($$E_{12}$$)** | $$50 \times 35$$ | $$1750 / 100$$ | **17.5** |
    | **女性/喜欢 ($$E_{21}$$)** | $$50 \times 65$$ | $$3250 / 100$$ | **32.5** |
    | **女性/不喜欢 ($$E_{22}$$)** | $$50 \times 35$$ | $$1750 / 100$$ | **17.5** |

    > **条件检查：** 所有期望频率（32.5, 17.5）都大于 5，检验有效。

3. 计算卡方统计量 $$\chi^2$$

    计算每个单元格的贡献值 $$\frac{(O_{ij} - E_{ij})^2}{E_{ij}}$$，然后求和。

    $$\mathbf{\chi^2 = \sum \frac{(O_{ij} - E_{ij})^2}{E_{ij}}}$$

    <div style="overflow-x:auto;">

    | 单元格 | $$O_{ij}$$ | $$E_{ij}$$ | $$(O_{ij} - E_{ij})$$ | $$(O_{ij} - E_{ij})^2$$ | 贡献值 $$\frac{(O_{ij} - E_{ij})^2}{E_{ij}}$$ |
    | :---: | :---: | :---: | :---: | :---: | :---: |
    | **$R_1C_1$** | 30 | 32.5 | -2.5 | 6.25 | $$6.25 / 32.5 \approx 0.1923$$ |
    | **$R_1C_2$** | 20 | 17.5 | 2.5 | 6.25 | $$6.25 / 17.5 \approx 0.3571$$ |
    | **$R_2C_1$** | 35 | 32.5 | 2.5 | 6.25 | $$6.25 / 32.5 \approx 0.1923$$ |
    | **$R_2C_2$** | 15 | 17.5 | -2.5 | 6.25 | $$6.25 / 17.5 \approx 0.3571$$ |
    | **总计 ($$\chi^2$$)** | - | - | - | - | $$\mathbf{1.0988}$$ |

    </div>

    $$\chi^2 \approx 0.1923 + 0.3571 + 0.1923 + 0.3571 = \mathbf{1.0988}$$

4. 确定自由度 df

    这是一个 R=2 行、C=2 列的列联表。
    $$\mathbf{\text{df} = (R - 1) \times (C - 1) = (2 - 1) \times (2 - 1) = \mathbf{1}}$$

5. 做出统计决策

    我们通常选择显著性水平 $$\alpha = 0.05$$。

    1.  **查找临界值：** 查阅[卡方分布](https://en.wikipedia.org/wiki/Chi-squared_distribution)表，在 df=1 且 $$\alpha = 0.05$$ 时的临界值为 $$\mathbf{3.841}$$。

    2.  **比较：**
        * 计算值 $$\chi^2 = 1.0988$$
        * 临界值 $$\chi^2_{\text{critical}} = 3.841$$

由于 $$\mathbf{1.0988 < 3.841}$$，即计算得到的卡方值小于临界值。

**结论：**

我们**不拒绝零假设 ($$H_0$$)**。在 $$\alpha=0.05$$ 的显著性水平下，没有足够的证据表明性别和产品偏好之间存在统计学上的显著关联。观察到的差异（如男性喜欢 30 人 vs 女性喜欢 35 人）可能是随机抽样误差造成的。

### [Kolmogorov–Smirnov Test](https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test)

一种非参数检验方法，对中心差异最敏感, 用于比较一个样本的经验累积分布函数（[Empirical Cumulative Distribution Function, EDF](https://en.wikipedia.org/wiki/Empirical_distribution_function)）与一个理论分布（单样本 KS 检验）或两个样本的 EDF 之间（双样本 KS检验）的最大差异。

KS检验的算法核心在于计算这个最大垂直距离，即KS统计量D。

KS检验计算流程

假设已排序样本数据 X = { -1.2, -0.5, 0.1, 0.8, 1.5}

1. 设定假设
    * 零假设 (H_0)： 样本数据来自标准正态分布 $$F(x) = \Phi(x)$$。
    * 备择假设 (H_a)： 样本数据不来自标准正态分布。
2. 计算经验 CDF ($$F_n(x)$$) 和理论 CDF ($$F(x)$$)
    
    <div style="overflow-x:auto;">

    | i | 排序数据 x(i) | 经验 CDF Fn(x) 之前 $$\frac{i-1}{n}$$ | 经验 CDF Fn(x) 之后 $$\frac{i}{n}$$ | 理论 CDF F(x(i)) = Φ(x(i)) |
    |---|----------------|-----------------------------|---------------------------|-----------------------------|
    | 1 | -1.2 | 0/5 = 0.0 | 1/5 = 0.2 | P(Z ≤ -1.2) ≈ 0.1151 |
    | 2 | -0.5 | 1/5 = 0.2 | 2/5 = 0.4 | P(Z ≤ -0.5) ≈ 0.3085 |
    | 3 | 0.1  | 2/5 = 0.4 | 3/5 = 0.6 | P(Z ≤ 0.1)  ≈ 0.5398 |
    | 4 | 0.8  | 3/5 = 0.6 | 4/5 = 0.8 | P(Z ≤ 0.8)  ≈ 0.7881 |
    | 5 | 1.5  | 4/5 = 0.8 | 5/5 = 1.0 | P(Z ≤ 1.5)  ≈ 0.9332 |

    </div>

3. 计算KS统计量D_n

    KS统计量 D_n 是经验CDF和理论CDF之间最大垂直距离的绝对值。我们需要检查两个方向的最大距离：

    $$\mathbf{D_n = \max \left\{ \max_{i} [ \frac{i}{n} - F(x_{(i)}) ], \quad \max_{i} [ F(x_{(i)}) - \frac{i-1}{n} ] \right\}}$$

    | 序号i | $$\frac{i}{n}$$ | F(x(i)) | D+距离 |
    |:----:|:--------:|:---:|:----:|
    | 1 | 0.2 | 0.1151 | 0.0849 |
    | 2 | 0.4 | 0.3085 | **0.0915** |
    | 3 | 0.6 | 0.5398 | 0.0602 |
    | 4 | 0.8 | 0.7881 | 0.0119 |
    | 5 | 1.0 | 0.9332 | 0.0668 |

    $$\max(D^+) = \mathbf{0.0915}$$

    | i | F(xᵢ) | (i−1)/n | D⁻ = F(xᵢ) − (i−1)/n |
    |---|-------|----------|-----------------------|
    | 1 | 0.1151 | 0.0  | 0.1151 |
    | 2 | 0.3085 | 0.2  | 0.1085 |
    | 3 | 0.5398 | 0.4  | 0.1398 |
    | 4 | 0.7881 | 0.6  | **0.1881** |
    | 5 | 0.9332 | 0.8  | 0.1332 |

    $$\max(D^-) = \mathbf{0.1881}$$

    $$\mathbf{D_n} = \max(\max(D^+), \max(D^-)) = \max(0.0915, 0.1881) = \mathbf{0.1881}$$

4. 做出统计决策 (以 $\alpha=0.05$ 为例)

    * 查找临界值： 对于样本大小 n=5 和显著性水平 $$\alpha=0.05$$ 的单样本 KS 检验，查 [KS 表](https://real-statistics.com/statistics-tables/kolmogorov-smirnov-table/)得到的临界值 $$D_{\text{critical}}$$ 约为 0.563。
    * 比较：计算值 $$D_n = 0.1881$$临界值 $$D_{\text{critical}} = 0.563$$由于 $$\mathbf{0.1881 < 0.563}$$，即计算得到的 KS 统计量远小于临界值。

**结论**：我们不拒绝零假设H_0。在 $$\alpha=0.05$$ 的显著性水平下，没有足够的证据表明样本数据不服从标准正态分布 $$N(0, 1)$$。

### [Anderson-Darling Test](https://en.wikipedia.org/wiki/Anderson%E2%80%93Darling_test)

它是一种二次经验分布函数 (EDF) 检验，与 Kolmogorov-Smirnov (KS) 检验和 Cramér-von Mises (CVM) 检验相似，但它通过对分布的尾部赋予更大的权重来增强其敏感性。

AD公式： $$A^2 = -n - \frac{1}{n} \sum_{i=1}^{n} (2i - 1) [ \ln F(x_i) + \ln ( 1 - F(x_{n+1-i}) ) ]$$

假设已排序样本数据 X = { -1.2, -0.5, 0.1, 0.8, 1.5}

1. 设定假设
    * 零假设 (H_0)： 样本数据来自标准正态分布 $$F(x) = \Phi(x) = \frac{1}{2} [ 1 + \text{erf} ( \frac{x}{\sqrt{2}} ) ]$$。
    * 备择假设 (H_a)： 样本数据不来自标准正态分布。

2. 计算理论 CDF ($$F(x)$$)
    
    <div style="overflow-x:auto;">

    | 序号i | 排序数据 $$x_i$$ | $$\frac{i}{n}$$ | 理论CDF $$F(x_i)$$ | $$\ln F(x_i)$$ | $$x_{n+1-i}$$ | $$1 - F(x_{n+1-i})$$ | $$\ln(1 - F(x_{n+1-i}))$$ | $$\frac{2i−1}{n}$$ |$$[ \ln F(Y_i) + \ln ( 1 - F(Y_{n+1-i}) ) ]$$ | $$S_i$$ |
    |------|------------------|-------------------|--------------------|----------------|----------------|------------------------|---------------------------|---------------------------|---------------------------|---------------------------|
    | 1 | -1.2 | 0.2 | 0.1151 | -2.1627 | 1.5 | 1 - 0.9332 = 0.0668 | -2.7067 | 0.2 | -4.8694 | -0.9739 |
    | 2 | -0.5 | 0.4 | 0.3085 | -1.1759 | 0.8 | 1 - 0.7881 = 0.2119 | -1.5510 | 0.6 | -2.7269 | -1.6361 |
    | 3 | 0.1 | 0.6 | 0.5398 | -0.6166 | 0.1 | 1 - 0.5398 = 0.4602 | -0.7760 | 1.0 | -1.3926 | -1.3926 | 
    | 4 | 0.8 | 0.8 | 0.7881 | -0.2381 | -0.5 | 1 - 0.3085 = 0.6915 | -0.3689 | 1.4 | -0.6070 | -0.8498 |
    | 5 | 1.5 | 1.0 | 0.9332 | -0.0690 | -1.2 | 1 - 0.1151 = 0.8849 | -0.1223 | 1.8 | -0.1913 | -0.3443 |

    </div>

    $$\mathbf{S} = \sum_{i=1}^{5} S_i = -0.97388 + (-1.63614) + (-1.39260) + (-0.84980) + (-0.34434) = \mathbf{-5.19676}$$

    $$A^2 = -n - S = -5 - (-5.19676) = 0.19676$$

3. 做出统计决策

    * 查找临界值： 假设我们使用标准正态分布（参数已知），并在 $$\alpha=0.05$$ 的显著性水平下进行检验。查 AD 表可得，此时的临界值 $$A_{\text{critical}}^2$$ 约为 0.787。
    * 比较：由于0.197 < 0.787，即计算得到的 AD 统计量小于临界值。

**结论**：我们不拒绝零假设H_0。在 $$\alpha=0.05$$ 的显著性水平下，没有足够的证据表明样本数据不服从标准正态分布 $$N(0, 1)$$。 (AD 统计量越小，拟合越好)。

### [Shapiro-Wilk Test](https://en.wikipedia.org/wiki/Shapiro%E2%80%93Wilk_test)

公式：$$W = \frac{(\sum_{i=1}^{n} a_i x_i)^2}{\sum_{i=1}^{n} (x_i - \bar{x})^2}$$

系数 $$a_i$$ 由以下公式给出：$$(a_{1},\dots ,a_{n})={m^{\mathsf {T}}V^{-1} \over C}$$

其中，C是一个向量范数（向量的长度）：$$C=\left\|V^{-1}m\right\|={(m^{\mathsf {T}}V^{-1}V^{-1}m)}^{1/2}$$

向量 m，$$m=(m_{1},\dots ,m_{n})^{\mathsf {T}}$$由从标准正态分布中抽取出的独立同分布随机变量的顺序统计量的期望值构成,通过数值积分近似（[Harter (1961)](https://gwern.net/doc/statistics/order/1961-harter.pdf)）；

最后，V 是这些正态顺序统计量的协方差矩阵。

关于 W 分布的特性：统计量 W 的分布没有名称。该统计量的**临界值（截止值，cutoff values）**是通过 蒙特卡洛模拟 计算得出的。

假设已排序样本数据 X = { -1.2, -0.5, 0.1, 0.8, 1.5}

1. 设定假设

    * 零假设 (H_0)： 样本数据来自标准正态分布。
    * 备择假设 (H_a)： 样本数据不来自标准正态分布。

2. 计算均值

    $$\bar{x} = \frac{-1.2 + (-0.5) + 0.1 + 0.8 + 1.5}{5} = \frac{0.7}{5} = \mathbf{0.14}$$

3. 计算分母

    | i | xi | xi − x̄ | (xi − x̄)² |
    |---|----|---------|------------|
    | 1 | -1.2  | -1.34  | 1.7956   |
    | 2 | -0.5  | -0.64  | 0.4096   |
    | 3 | 0.1   | -0.04  | 0.0016   |
    | 4 | 0.8   | 0.66   | 0.4356   |
    | 5 | 1.5   | 1.36   | 1.8496   |
    | **总和** | 0.7 | 0.00 | **4.4920** |

4. 计算分子

    查表[Shapiro-Wilk Tables](https://real-statistics.com/statistics-tables/shapiro-wilk-table/)

    公式：
    $$(\sum a_i Y_i)^2$$
    $$其中，\mathbf{a} = \frac{\mathbf{V}^{-1} \mathbf{m}}{\sqrt{\mathbf{m}^{\mathsf{T}} \mathbf{V}^{-2} \mathbf{m}}}$$

    1. 期望值向量 m

        $$\mathbf{m} = \begin{pmatrix} m_1 \\ m_2 \\ m_3 \\ m_4 \\ m_5 \end{pmatrix} \approx \begin{pmatrix} -1.1630 \\ -0.4950 \\ 0.0000 \\ 0.4950 \\ 1.1630 \end{pmatrix}$$

        理论公式：有序统计量 $$Y_k$$ 的概率密度函数 (PDF) 为：$$f_{Y_k}(x) = \frac{n!}{(k-1)! (n-k)!} [\Phi(x)]^{k-1} [1-\Phi(x)]^{n-k} \phi(x)$$

        其中 $$\phi(x)$$ 和 $$\Phi(x)$$ 分别是标准正态分布的 PDF 和 CDF。

        $$m_1 = \mathbb{E}[Y_1] = \int_{-\infty}^{\infty} x \cdot f_{Y_1}(x) dx$$

    2. 协方差矩阵V

        V 是一个 5x5 的对称矩阵，其元素 $$V_{ij} = \text{Cov}(Y_i, Y_j)$$。

        $$\mathbf{V} = \begin{pmatrix} V_{11} & V_{12} & V_{13} & V_{14} & V_{15} \\ V_{21} & V_{22} & V_{23} & V_{24} & V_{25} \\ V_{31} & V_{32} & V_{33} & V_{34} & V_{35} \\ V_{41} & V_{42} & V_{43} & V_{44} & V_{45} \\ V_{51} & V_{52} & V_{53} & V_{54} & V_{55} \end{pmatrix} \approx \begin{pmatrix} 
        0.3804 & 0.1983 & 0.1168 & 0.0687 & 0.0526 \\
        0.1983 & 0.2862 & 0.1345 & 0.1066 & 0.0687 \\
        0.1168 & 0.1345 & 0.1706 & 0.1345 & 0.1168 \\
        0.0687 & 0.1066 & 0.1345 & 0.2862 & 0.1983 \\
        0.0526 & 0.0687 & 0.1168 & 0.1983 & 0.3804 
        \end{pmatrix}$$

        $$V_{11} = \text{Var}(Y_1) = \mathbb{E}[Y_1^2] - (\mathbb{E}[Y_1])^2$$

    3. 逆协方差矩阵 $$\mathbf{V}^{-1}$$

        $$\mathbf{V}^{-1} \approx \begin{pmatrix} 
        4.2269 & -2.1387 & -0.3703 & 0.4908 & -0.1601 \\
        -2.1387 & 5.9220 & -2.9739 & -1.2599 & 0.4908 \\
        -0.3703 & -2.9739 & 8.9482 & -2.9739 & -0.3703 \\
        0.4908 & -1.2599 & -2.9739 & 5.9220 & -2.1387 \\
        -0.1601 & 0.4908 & -0.3703 & -2.1387 & 4.2269 
        \end{pmatrix}$$

        可以使用两种方法求逆协方差矩阵

        1. [高斯-约旦法](https://en.wikipedia.org/wiki/Gaussian_elimination)

            构造增广矩阵 $$[\mathbf{V} \| \mathbf{I}]$$，然后通过行变换将左侧的V转化为单位矩阵I

        2. [LU 分解](https://en.wikipedia.org/wiki/LU_decomposition)

            将一个方阵V分解为一个下三角矩阵 L (Lower Triangular) 和一个上三角矩阵 U (Upper Triangular) 的乘积

            1. 计算乘数 $$L_{i1} (i=2, 3, 4, 5)$$：

                $$L_{i1} = \frac{V_{i1}}{V_{11}}$$ $$L_{21} = \frac{0.1983}{0.3804} \approx \mathbf{0.5213}$$ 
                $$L_{31} = \frac{0.1168}{0.3804} \approx \mathbf{0.3070}$$
                $$L_{41} = \frac{0.0687}{0.3804} \approx \mathbf{0.1806}$$
                $$L_{51} = \frac{0.0526}{0.3804} \approx \mathbf{0.1383}$$

            2. 消元：用 $$V_{i\cdot} \leftarrow V_{i\cdot} - L_{i1} V_{1\cdot}$$ 更新矩阵，得到新的矩阵 $$\mathbf{V}^{(2)}$$。

                * U 的第 1 行就是 V 的第 1 行：

                $$U_{1\cdot} = (0.3804, 0.1983, 0.1168, 0.0687, 0.0526)$$

                * 更新 $$\mathbf{V}^{(2)}$$：

                $$\mathbf{V}^{(2)} \approx \begin{pmatrix}
                0.3804 & 0.1983 & 0.1168 & 0.0687 & 0.0526 \\
                \mathbf{0} & 0.1837 & 0.0739 & 0.0717 & 0.0416 \\
                \mathbf{0} & 0.0739 & 0.1350 & 0.1132 & 0.1009 \\
                \mathbf{0} & 0.0717 & 0.1132 & 0.2737 & 0.1884 \\
                \mathbf{0} & 0.0416 & 0.1009 & 0.1884 & 0.3731
                \end{pmatrix}$$

            3. 消元第 2 列（得到 $$L_{i2}$$ 和 $$U_{2j}$$）目标是利用 $$\mathbf{V}^{(2)}$$ 的第 2 行消去第 3 到第 5 行第 2 列的元素。

            4. 计算乘数 $L_{i2}$ ($i=3, 4, 5$)：

                $$L_{i2} = \frac{V^{(2)}_{i2}}{V^{(2)}_{22}}$$
                $$L_{32} = \frac{0.0739}{0.1837} \approx \mathbf{0.4023}$$
                $$L_{42} = \frac{0.0717}{0.1837} \approx \mathbf{0.3903}$$
                $$L_{52} = \frac{0.0416}{0.1837} \approx \mathbf{0.2265}$$

            5. 消元：U 的第 2 行就是 $$\mathbf{V}^{(2)}$$ 的第 2 行：

                $$U_{2\cdot} = (0, 0.1837, 0.0739, 0.0717, 0.0416)$$
                更新 $$\mathbf{V}^{(3)}$$：

                $$\mathbf{V}^{(3)} \approx \begin{pmatrix}
                \dots & \dots & \dots & \dots & \dots \\
                0 & 0.1837 & 0.0739 & 0.0717 & 0.0416 \\
                \mathbf{0} & \mathbf{0} & 0.1052 & 0.0844 & 0.0842 \\
                \mathbf{0} & \mathbf{0} & 0.0844 & 0.2455 & 0.1720 \\
                \mathbf{0} & \mathbf{0} & 0.0842 & 0.1720 & 0.3636
                \end{pmatrix}$$

            6. 继续消元阶段 

                计算 $$L_{43}, L_{53}$$，并更新 $$\mathbf{V}^{(4)}$$，得到 $$U_{3\cdot}$$。

                $$L_{43} = \frac{V^{(3)}_{43}}{V^{(3)}_{33}} = \frac{0.0844}{0.1052} \approx \mathbf{0.8023}$$
                $$L_{53} = \frac{V^{(3)}_{53}}{V^{(3)}_{33}} = \frac{0.0842}{0.1052} \approx \mathbf{0.8004}$$

            7. 计算 $$L_{54}$$，并更新 $$\mathbf{V}^{(5)}$$，得到 $$U_{4\cdot}$$。

                $$L_{54} = \frac{V^{(4)}_{54}}{V^{(4)}_{44}}$$

                这个步骤会得到 $U_{55}$。 

            8. 结果组合

                * U 矩阵（上三角矩阵）U 矩阵就是高斯消元法最终得到的上三角形式，其元素是各阶段的主元和更新后的非零元素：

                $$\mathbf{U} = \begin{pmatrix}
                V_{11} & V_{12} & V_{13} & V_{14} & V_{15} \\
                0 & V^{(2)}_{22} & V^{(2)}_{23} & V^{(2)}_{24} & V^{(2)}_{25} \\
                0 & 0 & V^{(3)}_{33} & V^{(3)}_{34} & V^{(3)}_{35} \\
                0 & 0 & 0 & V^{(4)}_{44} & V^{(4)}_{45} \\
                0 & 0 & 0 & 0 & V^{(5)}_{55}
                \end{pmatrix}$$

                * L 矩阵（下三角矩阵）L 矩阵是由计算过程中记录的乘数 $L_{ij}$ 组成的，主对角线元素为 1：

                $$\mathbf{L} = \begin{pmatrix}
                1 & 0 & 0 & 0 & 0 \\
                L_{21} & 1 & 0 & 0 & 0 \\
                L_{31} & L_{32} & 1 & 0 & 0 \\
                L_{41} & L_{42} & L_{43} & 1 & 0 \\
                L_{51} & L_{52} & L_{53} & L_{54} & 1
                \end{pmatrix} \approx \begin{pmatrix}
                1 & 0 & 0 & 0 & 0 \\
                \mathbf{0.5213} & 1 & 0 & 0 & 0 \\
                \mathbf{0.3070} & \mathbf{0.4023} & 1 & 0 & 0 \\
                \mathbf{0.1806} & \mathbf{0.3903} & \mathbf{0.8023} & 1 & 0 \\
                \mathbf{0.1383} & \mathbf{0.2265} & \mathbf{0.8004} & L_{54} & 1
                \end{pmatrix}$$

    4. 计算中间向量 $$\mathbf{g} = \mathbf{V}^{-1} \mathbf{m}$$

        $$g_1 = V^{11} m_1 + V^{12} m_2 + V^{13} m_3 + V^{14} m_4 + V^{15} m_5$$
        $$g_1 \approx (4.2269)(-1.16296) + (-2.1387)(-0.49502) + (-0.3703)(0) + (0.4908)(0.49502) + (-0.1601)(1.16296) \approx -3.70023$$

    5. 计算归一化常数 C'

        归一化常数 C' 是 g 向量的欧几里得范数 $$\sqrt{\mathbf{g}^{\mathsf{T}}\mathbf{g}}$$

        $$C' = \sqrt{g_1^2 + g_2^2 + g_3^2 + g_4^2 + g_5^2}$$

        由于对称性，g 向量的元素满足 g_1 = -g_5，g_2 = -g_4，且 g_3 = 0。

        $$\mathbf{g} \approx \begin{pmatrix} -3.70023 \\ -1.36070 \\ 0.00000 \\ 1.36070 \\ 3.70023 \end{pmatrix}$$

        $$C' \approx \sqrt{2 \cdot (-3.70023)^2 + 2 \cdot (-1.36070)^2 + 0^2} \approx \mathbf{5.57552}$$

    6. 计算 a_1

        $$a_1 = \frac{g_1}{C'} \approx \frac{-3.70023}{5.57552} \approx \mathbf{-0.6636}$$

### [Lilliefors Test](https://en.wikipedia.org/wiki/Lilliefors_test)

| 特性 | K–S 检验 (Kolmogorov–Smirnov) | Lilliefors 检验 |
|:--|:--|:--|
| **假设分布** | 必须指定完整的分布参数（例如 $$\mu=0$$, $$\sigma^2=1$$） | 分布参数（$$\mu$$ 和 $$\sigma^2$$）是未知的 |
| **参数估计** | 不允许从样本估计参数 | 允许从样本数据中估计 $$\hat{\mu}$$ 和 $$\hat{\sigma}^2$$ |
| **统计量** | D 统计量（样本与理论 CDF 的最大垂直距离） | D 统计量（定义与 K–S 相同） |
| **临界值** | 使用标准 K–S 临界值表 | 使用 Lilliefors 校正后的临界值表 |
| **应用场景** | 检验样本是否来自**已知参数的分布** | 检验样本是否来自**未知参数的正态分布或指数分布** |
| **常见用途** | 拟合优度检验（参数已知） | 正态性检验（参数未知） |

Lilliefors 检验并不是神奇地在不知道参数的情况下就能工作，而是采取了以下两步：
* 容忍估计： 允许并鼓励使用样本均值 $$\bar{X}$$ 和样本标准差 S 来估计总体参数 $$\mu$$ 和 $$\sigma^2$$。
* 校正标准： 采用一套更高的 Lilliefors 临界值来补偿因使用估计参数而导致的 D 统计量偏小，从而保持检验的真实显著性水平。


### [Cramér–von Mises Criterion](https://en.wikipedia.org/wiki/Cram%C3%A9r%E2%80%93von_Mises_criterion)

与 Kolmogorov–Smirnov (K-S) 检验只关注 ECDF ($$F_n(x)$$) 和理论 CDF ($$F_0(x)$$) 之间最大垂直距离不同，Cramér–von Mises 准则关注的是两者之间平方差异的积分。

它衡量的是整个分布范围内的整体差异，而不是仅仅在某一个点上的最大差异，因此被认为对分布尾部和中心的偏差都较为敏感，在许多情况下比 K-S 检验更有检验力 (Power)

$$W^2 = n \int_{-\infty}^{\infty} [F_n(x) - F_0(x)]^2 dF_0(x) = \sum_{i=1}^{n} \left( U_{(i)} - \frac{2i - 1}{2n} \right)^2 + \frac{1}{12n} \tag{0}$$

* $$X_{(i)}$$ 为已排序样本数据
* 其中 $$F_n(x)$$ 是经验累积分布函数（ECDF）为阶梯函数，从0到n分段，$$F_0(x)$$ 是理论累积分布函数（CDF）
* $$U_{(i)} = F_0(X_{(i)})$$ 是将排序后的数据代入假设理论分布的 CDF 得到的值（这些值应近似均匀分布）。

**推导过程**

$$\begin{align*}
\left\{\begin{matrix}
令u = F_0(x)\text{，则}x = F_0^{-1}(u)\text{，其中}u \in (0,1)\\
F_n(F^{-1}_0(u)) = \frac{i}{n} u \in [U_{(i)}, U_{(i+1)})  \\
令U_{0}=0, U_{n+1} =1
\end{matrix}\right.\\
\text{代入原式，}\\
W^2 &= n\int_{0}^{1} [\frac{i}{n} - u]^2du\\
&= n\sum_{i = 0}^{n}\int_{U_{(i)}}^{U_{(i+1)}}[(\frac{i}{n})^2 - 2\frac{i}{n}u + u^2]du\\
&= n[\sum_{i = 0}^{n}\int_{U_{(i)}}^{U_{(i+1)}}  (\frac{i}{n})^2du - \sum_{i = 0}^{n}\int_{U_{(i)}}^{U_{(i+1)}}2\frac{i}{n}udu  + \sum_{i = 0}^{n}\int_{U_{(i)}}^{U_{(i+1)}}u^2du]\\
&= n[\sum_{i = 0}^{n}(\frac{i}{n})^2[U_{(i+1)}-U_{(i)}] - \sum_{i = 0}^{n}\frac{i}{n}[U^2_{(i+1)}-U^2_{(i)}] + \sum_{i = 0}^{n}\frac{[U^3_{(i+1)}-U^3_{(i)}]}{3}]\\
\text{使用错位法，令整个关于U(i+1)的因式项往上挪一位，则提出}U_{(0)},U_{(n+1)}\\
&= n[1+\sum_{i = 1}^{n}[(\frac{i-1}{n})^2 -(\frac{i}{n})^2]U_{(i)} - 1 - \sum_{i = 1}^{n}[\frac{i-1}{n}-\frac{i}{n}]U^2_{(i)} + \frac{1}{3}]\\
&= n[\sum_{i = 1}^{n}(\frac{1-2i}{n^2})U_{(i)} + \frac{1}{n}\sum_{i = 1}^{n}U^2_{(i)} + \frac{1}{3}]\\
&= \sum_{i = 1}^{n}(\frac{1-2i}{n})U_{(i)} + \sum_{i = 1}^{n}U^2_{(i)} + \frac{n}{3}]\\
\text{把原式(0)右边展开}W^2 &= \sum_{i=1}^{n} \left( U_{(i)} - \frac{2i - 1}{2n} \right)^2 + \frac{1}{12n}\\
&= \sum_{i=1}^{n} \left( U^2_{(i)} -2U_{(i)}\frac{2i - 1}{2n} + (\frac{2i - 1}{2n})^2 \right) + \frac{1}{12n}\\
&= \sum_{i=1}^{n} \left( U^2_{(i)} + U_{(i)}\frac{1 - 2i}{n} \right) + \sum_{i=1}^{n}[\frac{i^2}{n^2} - \frac{i}{n^2} + \frac{1}{4n^2}]  + \frac{1}{12n}\\
&= \sum_{i=1}^{n} \left( U^2_{(i)} + U_{(i)}\frac{1 - 2i}{n} \right) + [\frac{n(n+1)(2n+1)}{6n^2} - \frac{n(n+1)}{2n^2} + \frac{1}{4n}]  + \frac{1}{12n}\\
&= \sum_{i=1}^{n} \left( U^2_{(i)} + U_{(i)}\frac{1 - 2i}{n} \right) + [\frac{(n+1)(2n-2)}{6n} + \frac{1}{3n}]\\
&= \sum_{i=1}^{n} \left( U^2_{(i)} + U_{(i)}\frac{1 - 2i}{n} \right) + \frac{n}{3}\\

\end{align*}$$
