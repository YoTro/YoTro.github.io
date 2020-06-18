---
title: Latent Dirichlet allocation
date: 2020-06-18 10:40:00
tags: [Graph,python, Probablility]
music-id: 
youtube-video-ID: 
bilibili-video-ID: 
mathjax: true
---

# [LDA](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation)

[EN](#EN)|[CN](#CN)

<span id="EN"> 

My Undergraduate Thesis is about LDA, Exploring the hidden factors online food shopping from Amazon reviews by LDA

Now, let me sum up LDA again.

LDA (latent Dirichlet allocation) is a topic model, which can give the topic of each document in the document set in the form of probability distribution. At the same time, it is an unsupervised learning algorithm, which does not need manual annotation of the training set, but only the document set and the number of specified topics K. Another advantage of LDA is that it can find some words to describe each topic. LDA was first proposed by BLEI, David M., Wu Enda and Jordan, Michael I in 2003. At present, LDA has applications in text mining, including text topic recognition, text classification and text similarity calculation.

$$p(\theta | \alpha) = \frac { \Gamma(\sum_{ i = 1 } ^ { k } \alpha_{ i } ) } { \prod_{ i = 1 } ^ { k } \Gamma ( \alpha_{ i } ) } \prod_{ i = 1 } ^ { k } \theta_{ i } ^ { \alpha_{ i } - 1}$$

This is LDA function, you need to learn these knowledage firstly before study:

1. Distribution: [Multinomial distribution](https://en.wikipedia.org/wiki/Multinomial_distribution), [Binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution), [Poisson distribution](https://en.wikipedia.org/wiki/Poisson_distribution), [Gamma distribution](https://en.wikipedia.org/wiki/Gamma_distribution), [Beta distribution](https://en.wikipedia.org/wiki/Beta_distribution), [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution)

2. [Bayes' theorem basic](https://en.wikipedia.org/wiki/Bayes%27_theorem): [Prior probability](https://en.wikipedia.org/wiki/Prior_probability), [Posterior probability](https://en.wikipedia.org/wiki/Posterior_probability), [Conjugate prior](https://en.wikipedia.org/wiki/Conjugate_prior), [Likelihood function](https://en.wikipedia.org/wiki/Likelihood_function)


3. [Markov chain Monte Carlo](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo), [Gibbs Sampling](https://en.wikipedia.org/wiki/Gibbs_sampling)

## [Bayes' theorem basic](https://en.wikipedia.org/wiki/Bayes%27_theorem)

p(\theta|x)={p(x|\theta)p(\theta)\over p(x)}

### [Prior probability](https://en.wikipedia.org/wiki/Prior_probability)

p(\theta)

### [Posterior probability](https://en.wikipedia.org/wiki/Posterior_probability)

p(\theta|x)

### [Likelihood](https://en.wikipedia.org/wiki/Likelihood_function)

p(x|\theta)

### [Conjugate prior](https://en.wikipedia.org/wiki/Conjugate_prior)

### [cdf](https://en.wikipedia.org/wiki/Cumulative_distribution_function)

|:--:|:--:|
|cdf(k, mu, loc=0) | Cumulative distribution function.|

### [pmf](https://en.wikipedia.org/wiki/Probability_mass_function)

|:--:|:--:|
|pmf(k, mu, loc=0) | Probability mass function|

## [Binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution)

$$f(k,n,p)=P(X=k)={n \choose k}p^{k}(1-p)^{n-k}$$

In probability theory and statistics, binomial distribution is a discrete probability distribution of n independent yes / no test success times, in which the success probability of each test is p. Such a single success / failure test is also known as the Bernoulli test. In fact, when n = 1, binomial distribution is Bernoulli distribution. Binomial distribution is the basis of two experiments with significant difference.

[scipy.stats.binom](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.binom.html?highlight=bin#scipy.stats.binom)
```python
from scipy.stats import binom
import matplotlib.pyplot as plt
import numpy as numpy

fig, ax = plt.subplots(1, 1)
n, p = 5, 0.4
#moments(矩)
mean, var, skewm, kurt = binom.stats(n, p, moments = 'mvsk')
#Display the probability mass function(pmf)
x = np.arange(binom.ppf(0.01, n, p), binom.ppf(0.99, n, p))
ax.plot(x, binom.pmf(x, n, p), 'bo', ms = 8, label = 'binom pmf')
ax.vlines(x, 0, binom.pmf(x, n, p), colors = 'b', lw = 5, alpha = 0.5)
rv = binom(n, p)
ax.vlines(x, 0, rv.pmf(x), colors = 'k', linestyles = '-', lw = 1, label = 'frozen pmf')
ax.legend(loc = 'best', frameon = False)
plt.show()
```
![binom](https://docs.scipy.org/doc/scipy/reference/_images/scipy-stats-binom-1_00_00.png)

## [Poisson distribution](https://en.wikipedia.org/wiki/Poisson_distribution), [Gamma distribution](https://en.wikipedia.org/wiki/Gamma_distribution)

$$P(X=k)=\frac{\lambda^k}{k!}e^{-\lambda}$$

The number of such events that occur during a fixed time interval is, under the right circumstances, a random number with a Poisson distribution.

such as the number of laser photons hitting a detector in a particular time interval

The Poisson distribution is also the limit of a binomial distribution, for which the probability of success for each trial equals λ divided by the number of trials, as the number of trials approaches infinity 

[scipy.stats.poisson](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.poisson.html?highlight=poisson#scipy.stats.poisson)
```python
from scipy.stats import poisson
import matplotlib.pyplot as plt
import numpy as numpy

fig, ax = plt.subplots(1, 1)

mu = 0.6
mean, var, skew, kurt = poisson.stats(mu, moments='mvsk')
x = np.arange(poisson.ppf(0.01, mu),
              poisson.ppf(0.99, mu))
ax.plot(x, poisson.pmf(x, mu), 'bo', ms=8, label='poisson pmf')
ax.vlines(x, 0, poisson.pmf(x, mu), colors='b', lw=5, alpha=0.5)
rv = poisson(mu)
ax.vlines(x, 0, rv.pmf(x), colors='k', linestyles='-', lw=1,
        label='frozen pmf')
ax.legend(loc='best', frameon=False)
plt.show()
```
![poisson](https://docs.scipy.org/doc/scipy/reference/_images/scipy-stats-poisson-1_00_00.png)

## [Gamma distribution](https://en.wikipedia.org/wiki/Gamma_distribution)

$$\Gamma(x) = \int_0^\infty t^{x-1}e^{-t}dt$$

$$\Gamma(n) = (n-1)!$$

In probability theory and statistics, the gamma distribution is a two-parameter family of continuous probability distributions. 

From the point of view of significance: the problem solved by the exponential distribution is "how long does it take to wait for a random event to happen", and the problem solved by the gamma distribution is "how long does it take to wait for N random events to happen".

[scipy.stats.gamma](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.gamma.html?highlight=gamma#scipy.stats.gamma)
```python
from scipy.stats import gamma
import matplotlib.pyplot as plt
import numpy as numpy

fig, ax = plt.subplots(1, 1)
a = 1.99
mean, var, skew, kurt = gamma.stats(a, moments='mvsk')
#Display the probability density function (pdf):

x = np.linspace(gamma.ppf(0.01, a),
                gamma.ppf(0.99, a), 100)
ax.plot(x, gamma.pdf(x, a),
       'r-', lw=5, alpha=0.6, label='gamma pdf')

rv = gamma(a)
ax.plot(x, rv.pdf(x), 'k-', lw=2, label='frozen pdf')
vals = gamma.ppf([0.001, 0.5, 0.999], a)
np.allclose([0.001, 0.5, 0.999], gamma.cdf(vals, a))
#Generate random numbers
r = gamma.rvs(a, size=1000)
ax.hist(r, density=True, histtype='stepfilled', alpha=0.2)
ax.legend(loc='best', frameon=False)
plt.show()
```
![gamma](https://docs.scipy.org/doc/scipy/reference/_images/scipy-stats-gamma-1.png)

## [Beta distribution](https://en.wikipedia.org/wiki/Beta_distribution)

$$\begin{align} f(x; \alpha, \beta) = \frac{1}{B(\alpha, \beta)} x^{\alpha - 1} {(1-x)}^{\beta-1} \end{align}$$

$$\begin{align} \frac{1}{B(\alpha, \beta)} = \frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)} \end{align}$$

[Conjugate prior] of binomial distributions [Likelihood function](https://en.wikipedia.org/wiki/Likelihood_function) of binomial distribution)

You can understand it as observing the probability distribution of a group of experiments from different angles. The beta distribution is to study the distribution of theta given a set of data x, while the binomial distribution is to study the distribution of X given theta

[scipy.stats.beta](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.beta.html?highlight=beta#scipy.stats.beta)
```python
from scipy.stats import beta
import matplotlib.pyplot as plt
import numpy as numpy

fig, ax = plt.subplots(1, 1)
a, b = 2.31, 0.627
mean, var, skew, kurt = beta.stats(a, b, moments='mvsk')
x = np.linspace(beta.ppf(0.01, a, b),
                beta.ppf(0.99, a, b), 100)
#pdf: Probability density function.
ax.plot(x, beta.pdf(x, a, b),'r-', lw=5, alpha=0.6, label='beta pdf')
rv = beta(a, b)
ax.plot(x, rv.pdf(x), 'k-', lw=2, label='frozen pdf')
#ppf: Percent point function (inverse of cdf — percentiles)
vals = beta.ppf([0.001, 0.5, 0.999], a, b)
np.allclose([0.001, 0.5, 0.999], beta.cdf(vals, a, b))
#Generate random numbers
r = beta.rvs(a, b, size=1000)
ax.hist(r, density=True, histtype='stepfilled', alpha=0.2)
ax.legend(loc='best', frameon=False)
plt.show()

```
![beta](https://docs.scipy.org/doc/scipy/reference/_images/scipy-stats-beta-1.png)

`Expectation`

$$\mu =\operatorname {E}(X)={\frac  {\alpha }{\alpha +\beta }}$$

`Variance`

$$\operatorname {Var}(X)=\operatorname {E}(X-\mu )^{2}={\frac  {\alpha \beta }{(\alpha +\beta )^{2}(\alpha +\beta +1)}}$$

`Skewness`

$$\frac{\operatorname {E}(X-\mu)^{3}}{[\operatorname {E}(X-\mu)^{2}]^{{3/2}}}=\frac{2(\beta -\alpha ){\sqrt  {\alpha +\beta +1}}}{(\alpha +\beta +2){\sqrt{\alpha \beta }}}$$

[scipy.stats.skew](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.skew.html)
```python
from scipy.stats import skew
skew([2, 8, 0, 4, 1, 9, 9, 0]) #return 0.2650554122698573
```

`Kurtosis`

$${\frac  {\operatorname {E}(X-\mu )^{4}}{[\operatorname {E}(X-\mu )^{2}]^{{2}}}}-3={\frac  {6[\alpha ^{3}-\alpha ^{2}(2\beta -1)+\beta ^{2}(\beta +1)-2\alpha \beta (\beta +2)]}{\alpha \beta (\alpha +\beta +2)(\alpha +\beta +3)}}$$

[scipy.stats.kurtosis](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html)
```python
import scipy.stats as stats
from scipy.stats import kurtosis
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-5, 5, 100)
ax = plt.subplot()

distnames = ['laplace', 'norm', 'uniform']
for distname in distnames:
    if distname == 'uniform':
        dist = getattr(stats, distname)(loc=-2, scale=4)
    else:
        dist = getattr(stats, distname)
	data = dist.rvs(size=1000)
	kur = kurtosis(data, fisher=True)
	y = dist.pdf(x)
    ax.plot(x, y, label="{}, {}".format(distname, round(kur, 3)))
    ax.legend()
plt.show()
```
![kurtosis](https://docs.scipy.org/doc/scipy/reference/_images/scipy-stats-kurtosis-1.png)

## [Multinomial distribution](https://en.wikipedia.org/wiki/Multinomial_distribution)

$$\begin{aligned}f(x_{1},\ldots ,x_{k};n,p_{1},\ldots ,p_{k})&{}=\Pr(X_{1}=x_{1}{\text{ and }}\dots {\text{ and }}X_{k}=x_{k})\\&{}={\begin{cases}{\displaystyle {n! \over x_{1}!\cdots x_{k}!}p_{1}^{x_{1}}\times \cdots \times p_{k}^{x_{k}}},\quad &{\text{when }}\sum _{i=1}^{k}x_{i}=n\\\\0&{\text{otherwise,}}\end{cases}}\end{aligned}$$

Express using gamma:

$$f(x_1,\dots, x_{k}; p_1,\ldots, p_k) = \frac{\Gamma(\sum_i x_i + 1)}{\prod_i \Gamma(x_i+1)} \prod_{i=1}^k p_i^{x_i}$$

A generalization of the binomial distribution.

[scipy.stats.multinomial¶
](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.multinomial.html)
```python
from scipy.stats import multinomial
rv = multinomial(8, [0.3, 0.2, 0.5])
print(rv.pmf([1, 3, 4]))#return 0.04200000000000007

multinomial.pmf([3, 4], n=7, p=[.3, .7])#0.22689449999999986

```

## [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution)

$$f(x_1, x_2, ..., x_k; \alpha_1, \alpha_2,...,\alpha_k)=\frac{1}{B(\alpha)}\prod_{i=1}^{k}{x_i}^{\alpha^i-1}$$

$$B(\alpha) = \frac{\prod_{i=1}^{k}\Gamma(\alpha^i)}{\Gamma(\sum_{i=1}^{k}\alpha^i)}, \sum_{i=1}^{k}x^i = 1$$

Dirichlet distribution is a conjugate distribution of multinomial distribution

[scipy.stats.dirichlet](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.dirichlet.html)
```python
from scipy.stats import dirichlet
import numpy as np

quantiles = np.array([0.2, 0.2, 0.6])  # specify quantiles
alpha = np.array([0.4, 5, 15])  # specify concentration parameters
dirichlet.pdf(quantiles, alpha)
#The same PDF but following a log scale
dirichlet.logpdf(quantiles, alpha)
print(dirichlet.mean(alpha))  # get the mean of the distribution 
#[0.01960784 0.24509804 0.73529412]
print(dirichlet.var(alpha)) # get variance
#[0.00089829 0.00864603 0.00909517]
print(dirichlet.entropy(alpha)) # calculate the differential entropy
#-4.3280162474082715
print(dirichlet.rvs(alpha, size=1, random_state=1))
#[[0.00766178 0.24670518 0.74563305]]
print(dirichlet.rvs(alpha, size=2, random_state=2))
#[[0.01639427 0.1292273  0.85437844]
# [0.00156917 0.19033695 0.80809388]]

```

![Smoothed_LDA](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4d/Smoothed_LDA.png/377px-Smoothed_LDA.png 1.5x, //upload.wikimedia.org/wikipedia/commons/4/4d/Smoothed_LDA.png)

# References

1. [Scipy.stats](https://docs.scipy.org/doc/scipy/reference/stats.html?highlight=scipy%20stats#module-scipy.stats)

2. Wikipedia

3. zhihu

4. [LDA数学八卦](http://www.52nlp.cn/lda-math-%e6%b1%87%e6%80%bb-lda%e6%95%b0%e5%ad%a6%e5%85%ab%e5%8d%a6)

![参数说明](https://pic4.zhimg.com/80/v2-231d7ba3c5144be574cbb590cc415c17_1440w.png)