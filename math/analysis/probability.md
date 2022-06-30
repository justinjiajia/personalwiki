









Population:  infinitely many heads (proportion $p$) and tails (proportion $1-p$) or

a random variable $Y\sim \text{Bernoulli}(p)$ is defined on (used to describe) this population. Thus, the population distribution is:
$$
\begin{cases} \mathbb{P}(Y=\text{H}) = p  \\ \mathbb{P}(Y=\text{T}) =1-p\end{cases}
$$


An experiment is used in a very general way to refer to some process, procedure or natural phenomena that produces a random outcome, e.g.,  toss a coin twice to get a sample of 2 observasions

The sample space $\Omega $ of this experiment is the set of all possible outcomes: $\{\text{HH},\text{HT},\text{TH},\text{TT}\}$

Subsets of $\Omega$ is called **events**, e.g., the event that the first observation is heads $ \{\text{HH}, \text{HT}\}$







####  

**Independent Events**

If we flip a fair coin twice, the sample space $\Omega$ has 4 elements: $\Omega = \{(i, j)\colon i, j \in \{h, t\} \}$

then the probability of the event $\{(h,h)\}$ is 

$\{(h,h)\}=\{(h, j)\colon j \in \{h, t\} \} \bigcap \{(i, h)\colon i \in \{h, t\} \}$

{two heads} =  {head for the 1st toss} and {head for the 2nd toss}

the event that a head occurs on the 1st toss is independent of the event that a head occurs on the 2nd toss



A **random variable** is a mapping (technically, a measurable function): $X: \Omega \to \mathbb{R}$ that assigns a real number $X(\boldsymbol{\omega})$ to each outcome $\boldsymbol{\omega}$. We will usually denote random variables by capital roman letters





e.g., $X_1(\boldsymbol{\omega})$ is a mapping that keeps only the first observations in the sample outcome $\boldsymbol{\omega}$. Its distribution is the population distribution.

$\bar{X}(\boldsymbol{\omega})$ is a mapping that calculates the mean of observations in the sample outcome $\boldsymbol{\omega}$





A **statistic** is any function of the observations in a random sample

The probability distribution of a statistic is called a **sampling distribution**.



**Binomial distribution**

Suppose we have a coin which falls heads up with probability $p$ for some $0 ≤ p ≤ 1$. Flip the coin $n$ times (so the sample space is $n$-dimensional) and let $X$ be the number of heads. Assume that the tosses are independent. Let $f(x)= \mathbb{P}(X = x)$ be the mass function:
$$
f_X(x) = {n \choose x} p^x(1-p)^{n-p}
$$




**$\chi^2$ distribution**

If $Z_i \sim N(0,1)$ are independent,  $\chi^2(\boldsymbol{\omega}) = \sum_{i=1}^p \omega_i^2 = \sum_{i=1}^p Z_i^2 \sim \chi^2(p)$







What could be a sample space for a Bernoulli random variable?

Let $\Omega=[0,1]$ and define $\mathbb{P}(Y\in [a,b])=b-a$ for all $0\leq a\leq b\leq1$. Fixe $p \in [0,1]$ and define a mapping
$$
X(\omega) = \begin{cases} 1 \quad \text{if } \omega\leq p \\ 0 \quad \text{if } \omega > p\end{cases}
$$
Thus, $X(\omega) \sim  \text{Bernoulli}(p)$.

 A random variable is formally a mapping defined on some sample space.



Given a random variable $X$, we define the cumulative distribution function as the function $F_X(x)=\mathbb{P}(X\leq x)$



Let $\vec{X} =\left[\begin{matrix} X_1 \\ \vdots \\ X_n  \end{matrix} \right]$ where $X_1,\dots,X_n$ are random variables. We call $\vec{X}$ a **random vector**. Let $f_{X_1,\dots,X_n}(x_1,\dots ,x_n)$ denote the PDF. We say that $X_1,\dots,X_n$ are independent if, for every $A_1,\dots,A_n$
$$
\mathbb{P}(X_1 \in A_1, \dots X_n\in A_n) =\prod_{i=1}^n \mathbb{P}(X_i)
$$
It suffices to check that $f_{X_1,\dots,X_n}(x_1,\dots ,x_n) = \prod_{i=1}^n f_{X_i}(X_i)$.



If $X_1,\dots,X_n$ are independent and each has the same marginal distribution with CDF $F$, we say that $X_1,\dots,X_n$ are IID (independent and identically distributed). We also call $X_1,\dots,X_n$ a **random sample** of size $n$ from $F$.



#### Expectation of a Random Variable

The expected value,or mean,or first moment,of a random variable $X$ is defined to be
$$
\mathbb{E}(X)=\int x\,dF_X(x) =\mu_X
$$ { }
assuming that the sum (or integral) is well define.

To ensure that $\mathbb{E}(X)$ is well defined, we say that $\mathbb{E}(X)$ exists if $\int_x|x|\,dF_X(x) < \infty$. Otherwise, we say that the expectation does not exist.



Let $Y = r(X)$.  To compute $\mathbb{E}(Y)$, one way is to find $f_Y(y)$ and then compute $\mathbb{E}(Y)=\int y \,dF_Y(y)$. But there is an easier way



#### Theorem (The Rule of the Lazy Statistician). 

Let $Y = r(X)$. Then 
$$
\mathbb{E}(Y)=\int r(x) \,dF_X(x)
$$

##### Properties of Expectations

Theorem. 

If $X_1,\dots,X_n$ are random variables and $a_1,\dots,a_n$ are constants, then
$$
\mathbb{E }\left( \sum_{i=1}^n a_iX_i\right) = \sum_{i=1}^n a_i \mathbb{E }\left(X_i\right)
$$

$$
\mathbb{E }\left( \sum_{i=1}^n a_iX_i\right) = \int\dots\int \left( \sum_{i=1}^n a_i x_i\right) \, d F_{X_1,\dots,X_n}(x_1, \dots, x_n) = \sum_{i=1}^n a_i \int x_i \, dF_{X_i}(x_i) = \sum_{i=1}^n a_i \mathbb{E }\left(X_i\right)
$$



Theorem. 

Let $X_1,\dots,X_n$ be ***independent*** random variables. Then,
$$
\mathbb{E }\left( \prod_{i=1}^n X_i\right) = \prod_{i=1}^n   \mathbb{E }\left(X_i\right)
$$



$$
\mathbb{E }\left( \prod_{i=1}^n X_i\right) =  \int\dots\int \left( \prod_{i=1}^n  x_i\right)   f_{X_1,\dots,X_n}(x_1, \dots, x_n)\, dx_1 dx_2\dots dx_n \\=  \int\dots\int \left( \prod_{i=1}^n  x_i\right)  f_{X_1}(x_1) f_{X_2}(x_2)\dots f_{X_n}(x_n) \, dx_1 dx_2\dots dx_n = \prod_{i=1}^n   \mathbb{E }\left(X_i\right)
$$
iterated integrals (measure theory)



Notice that the summation rule does not require **independence** but the multiplication rule does.



#### Variance and Covariance

Definition. 

Let $X$ be a random variable with mean $\mu$. The variance of $X$ — denoted by $\sigma_X^2$ or $\mathbb{V}(X)$— is defined by
$$
\sigma_X^2 = \mathbb{E}\left[(X-\mu)^2 \right] = \int (x-\mu)^2\,dF_X(x)
$$
assuming this expectation exists. 



Theorem. 

Assuming the variance is well defined, it has the following properties: 

- $\mathbb{V}(X) = \mathbb{E}(X^2) −\mu^2$. 

- If $a$ and $b$ are constants, then $\mathbb{V}(aX + b)= a^2\mathbb{V}(X)$.

- If  $X_1,\dots,X_n$ are independent and  $a_1,\dots,a_n$ are constants, then
  $$
  \mathbb{V }\left( \sum_{i=1}^n a_iX_i\right) = \sum_{i=1}^n a_i^2 \, \mathbb{V }\left(X_i\right)
  $$
  




$$
\mathbb{V}(X) = \int(x-\mu_X)^2f_X(x)\,dx =    \int x^2 f_X(x)\,dx −\mu_X^2  =  \mathbb{E}(X^2) −\mu_X^2
$$

$$
\mathbb{V }\left( \sum_{i=1}^n a_iX_i\right)  = \mathbb{E}\left[\left(\sum_{i=1}^n a_i(X_i- \mu_{X_i}) \right)^2 \right]
\\=
\int\dots\int  \left(\sum_{i=1}^n a_i(x_i-  \mu_{X_i}) \right)^2  f_{X_1,\dots,X_n}(x_1, \dots, x_n)\, dx_1 dx_2\dots dx_n \\

\\=  \int\dots\int \left(\sum_{i=1}^n a_i(x_i-  \mu_{X_i}) \right)^2 f_{X_1}(x_1) f_{X_2}(x_2)\dots f_{X_n}(x_n) \, dx_1 dx_2\dots dx_n
\\= \sum_{i=1}^n a_i^2 \int\dots\int \left( x_i-  \mu_{X_i}  \right)^2 f_{X_1}(x_1) f_{X_2}(x_2)\dots f_{X_n}(x_n) \, dx_1 dx_2\dots dx_n\\+  \sum_{i=1}^{n-1}  \sum_{j=i+1}^n 2 a_ia_j\int\dots\int \left(  x_i-  \mu_{X_i}  \right)\left(  x_j-  \mu_{X_j} \right) f_{X_1}(x_1) f_{X_2}(x_2)\dots f_{X_n}(x_n) \, dx_1 dx_2\dots dx_n \\=  \sum_{i=1}^n a_i^2 \, \mathbb{V }\left(X_i\right)
$$



If $X_1,\dots,X_n$ are random variables then we define the sample mean to be  $\bar{X}_n =\frac{1}{n}\sum_{i=1}^nX_i$ and the **sample variance** to be $S^2_n=\frac{1}{n-1}\sum_{i=1}^n(X_i-\bar{X})^2$.

Theorem. Let $X_1,\dots,X_n$ be IID (i.e., a random sample of size $n$ from a distribution with CDF $F$) and let $\mu = \mathbb{E}(X_i)$, $σ^2 = \mathbb{V}(X_i)$. Then $\mathbb{E}(\bar{X}_n) = \mu$, $\mathbb{V}(\bar{X}_n) =\frac{ \sigma^2 }{n}$ and $E(S^2_n) = σ^2$.




Considering determining the sampling distribution of the sample mean $\bar{X}_n$ has the mean
$$
\mathbb{E}(\bar{X}_n) = \mathbb{E}\left[\frac{1}{n} X_i \right]=\frac{1}{n}  \sum_{i=1}^n \mathbb{E}(X_i)=\mu \tag{1}
$$

Only linearity is needed to derive equation (1)

and variance
$$
\sigma_{\bar{X}}^2 = \mathbb{V}(\bar{X}_n) =  \mathbb{E} \left[\left(\bar{X}-\mu\right)^2\right]= \frac{1}{n^2}  \mathbb{E}\left(\left[\sum_{i=1}^n \left( X_i- \mu \right) \right]^2\right) = \frac{1}{n^2} \sum_{i=1}^n  \, \mathbb{V }\left(X_i\right) = \frac{\sigma^2}{n}
$$



The thrid equality follows independence (the terms of the form $E\left(\left(X_i-\mu\right)\left(X_j-\mu\right)\right)=0$ and thereby don't appear)



The **sum of squared residuals** $\sum_{i=1}^{n}(X_i-\bar{X})^2 $ has less variability than $\sum_{i=1}^{n}(X_i-\mu)^2 $:
$$
\mathbb{E}\left[\sum_{i=1}^{n}(X_i-\bar{X})^2 \right]= \mathbb{E}\left[\sum_{i=1}^{n} \left(X_i^2-2X_i\bar{X} +\bar{X}^2 \right) \right]
\\ = \mathbb{E}\left[\sum_{i=1}^{n}  X_i^2- n \bar{X}^2   \right]  = \sum_{i=1}^{n} \mathbb{E}( X_i^2) - n \mathbb{E}(\bar{X}^2 ) \\

=n\sigma^2+n \mu^2 - n \mu^2 - \sigma^2 =(n-1)\sigma^2
$$




$ \mathbb{E}(X_i^2) = E\left[\left(X_i-\mu\right)^2\right] + \mu^2 = \sigma^2 + \mu^2  $ and $\mathbb{E}(\bar{X}^2 )= \mathbb{E}\left[\left(\bar{X}-\mu\right)^2\right] +\mu^2 =  \frac{\sigma^2}{n}+\mu^2$.



So the unbiased estimator of $\sigma^2$ is  $S^2_n=\frac{\sum_{i=1}^{n}(X_i-\bar{X})^2}{n-1}$.





Intuitively,$\sum_{i=1}^{n}(X_i-\bar{X})^2 $ is smaller than $\sum_{i=1}^{n}(X_i-\mu)^2 $, because we have to estimate $\mu$ using $\bar{X}$ before calculating it. More specifically, the residual $e_i=X_i-\bar{X}$ has to satisfy the equation $\sum_{i=1}^n e_i =0$, which limits the variability of the residual.



![](https://raw.githubusercontent.com/justinjiajia/img/master/prob_stats/bias_of_sample_variance.PNG)



this figure Illustration of how bias arises in using maximum likelihood to determine the variance of a Gaussian ($\hat{\sigma}^2 = \frac{\sum_{i=1}^{n}(X_i-\bar{X})^2}{n} $ ). The green curve shows the true Gaussian distribution from which data is generated, and the three red curves show the Gaussian distributions obtained by fitting to three data sets, each consisting of two data points shown in blue, using the maximum likelihood results.  Averaged across the three data sets, the mean is correct, but the variance is systematically under-estimated because it is measured relative to the sample mean and not relative to the true mean.

The bias is $E(\hat{\sigma}^2 ) -\sigma^2= -\frac{\sigma^2} {n}$





Definition. 

Let $X$ and $Y$ be random variables with means $\mu_X$ and $\mu_Y$ and standard deviations $\sigma_X$ and $\sigma_Y$. Define the **covariance** between $X$ and $Y$ by
$$
\text{Cov}(X,Y) = \mathbb{E}\left[(X-\mu_X)(Y-\mu_Y)\right]
$$



and the **correlation** by 
$$
\rho_{X,Y} = \frac{\text{Cov}(X,Y)}{\sigma_X\sigma_Y}
$$

$$
\text{Cov}(X,Y) = \mathbb{E}\left[(X-\mu_X)(Y-\mu_Y)\right] =\int\int (x-\mu_X)(y-\mu_Y)\, f_{X,Y}(x,y)\,  dxdy
\\=\int\int xy \, f_{X,Y}(x,y)\,  dxdy - \int\int x\, \mu_Y \, f_{X,Y}(x,y)\,  dxdy - \int\int y \,\mu_X \, f_{X,Y}(x,y)\,  dxdy  + \mu_X\mu_Y =\mathbb{E}(XY)-\mathbb{E}(X)\mathbb{E}(Y)
$$




If $Y = aX + b$ for some constants $a$ and $b$ then 

- $\rho(X,Y)=1$ if $a> 0$ 
- $\rho(X,Y)=-1$ if $a< 0$ 
- If $X$ and $Y$ are independent, then $\text{Cov}(X,Y)= \rho = 0$. The converse is not true in general.





Normal Distribution

multivariate normal distribution

A $p$-dimensional random vector Let $\vec{X} =\left[\begin{matrix} X_1 \\ \vdots \\ X_p  \end{matrix} \right]$ where $X_1,\dots,X_p$ are random variables.
$$
f_{\vec{X}}(\mathbf{x}) = \left(2 \pi  \right)^{-\frac{p}{2}} \vert \boldsymbol{\Sigma}\vert^{-\frac{1}{2}} \exp \left[-\frac{1}{2} \left( \mathbf{x} -   \boldsymbol{\mu} \right)^\top \boldsymbol{\Sigma} ^{-1} \left( \mathbf{x} -     \boldsymbol{\mu}   \right) \right]
$$


https://en.wikipedia.org/wiki/Gaussian_integral






Integral of vector = vector of integrals of each entry

Integral of matrix = matrix of integrals of each entry


$$
\mathbb{E}(X_i) =\int   x_i f_{\vec{X}}(\mathbf{x}) \, d\mathbf{x} \\
\mathbb{E}(\vec{X}) =\int   \mathbf{x} f_{\vec{X}}(\mathbf{x}) \, d\mathbf{x} \\

\mathbb{E} \left[(X_i- \mu_{X_i})(X_j-\mu_{X_j}) \right]=\int (x_i-\mu_{X_i})(x_j-\mu_{X_j}) f_{\vec{X}}(\mathbf{x}) \, d\mathbf{x}  =\Sigma_{ij}

\\
\\

\mathbb{E}  \left[ \left( \vec{X} -   \boldsymbol{\mu}_{\vec{X}} \right)\left(\vec{X} -   \boldsymbol{\mu}_{\vec{X}} \right)^\top \right] = \mathbb{E}  \left[ \begin{matrix} (X_1- \mu_{X_1})^2 & (X_1- \mu_{X_1})(X_2- \mu_{X_2}) & \dots &  (X_1- \mu_{X_1})(X_p- \mu_{X_p}) \\  (X_2- \mu_{X_2})(X_1- \mu_{X_1}) & (X_2- \mu_{X_2})^2 &  \dots & (X_2- \mu_{X_2})(X_p- \mu_{X_p}) \\ \vdots &\vdots&\ddots &\vdots\\ (X_p- \mu_{X_p})(X_1- \mu_{X_1})  & (X_p- \mu_{X_p})(X_2- \mu_{X_2})  &\dots & (X_p- \mu_{X_p})^2
\end{matrix}\right]
\\ 
\\= \left[ \begin{matrix} \mathbb{V}(X_1) & \text{Cov}(X_1, X_2) & \dots &  \text{Cov}(X_1, X_p) \\ \text{Cov}(X_2, X_1) & \mathbb{V}(X_2) & \dots & \text{Cov}(X_2, X_p) \\ \vdots &\vdots&\ddots &\vdots\\ \text{Cov}(X_p, X_1) &  \text{Cov}(X_p, X_2) &\dots &  \mathbb{V}(X_p)\end{matrix} \right]
\\
\\= \int  \left( \mathbf{x} -   \boldsymbol{\mu}_{\vec{X}} \right)\left( \mathbf{x} -   \boldsymbol{\mu}_{\vec{X}} \right)^\top f_{\vec{X}}(\mathbf{x}) d\mathbf{x}=  \boldsymbol{\Sigma}
$$




The Gaussian integral, also known as the Euler–Poisson integral, is the integral of the Gaussian function $e^{-x^2}$ over the entire real line

$\int_{ - \infty}^{ \infty} e^{-x^2}dx = \sqrt{\pi}$

For $f(x)=\frac{1}{\sqrt{2\pi}  \sigma }e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ 
$$
\int_{ - \infty}^{ \infty} \frac{1}{\sqrt{2\pi  } \sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}dx= \frac{1}{\sqrt{ \pi  }  }\int_{ - \infty}^{ \infty}e^{-\frac{(x-\mu)^2}{2\sigma^2}} d \frac{x-\mu}{\sqrt{2}\sigma} =1
$$




The following are true for a normal vector $\mathbf{x}$ having a multivariate normal distribution: 

1. Linear combination of the components of  $\mathbf{x}$  are normally distributed. 
2. All subsets of the components of  $\mathbf{x}$  have a (multivariate) normal distribution. 
3. Zero covariance implies that the corresponding components are independently distributed. 
4. The conditional distributions of the components are normal.





If $ \boldsymbol{\Sigma}$ is a diagonal matrix
$$
\left[\begin{matrix}
\sigma_1^2 & 0 & \dots & 0 \\
0 & \sigma_2^2 & \dots & 0 \\
\vdots   & \vdots   & \ddots   &    \vdots \\
0 & 0 & \dots & \sigma_p^2

\end{matrix}
\right]
$$



$$
\vert \boldsymbol{\Sigma}\vert^{-1}=\prod_{i=1}^p \frac{1}{\sigma^2_i}
$$

$$
f(\mathbf{x})= \left(2 \pi  \right)^{-\frac{p}{2}}\left( \prod_{i=1}^p \frac{1}{\sigma^2_i}\right)^{\frac{1}{2}} \exp \left[\sum_{i=1}^{p}-\frac{1}{2}  \frac{(x_i-\mu_i)^2}{\sigma^2_i} \right] = \prod_{i=1}^p \left(  \left(2 \pi \sigma^2_i \right)^{-\frac{1}{2 }}   \exp \left[ -\frac{1}{2}  \frac{(x_i-\mu_i)^2}{\sigma^2_i} \right] \right) =\prod_{i=1}^p f(x_i)
$$

which means $x_1, \dots , x_n$ are independent.





Marginal and Conditional distributions 

$\boldsymbol{\Sigma} $ is positive definite.

Theorem. Let $\vec{X} \sim N_n(\boldsymbol{\mu} , \boldsymbol{\Sigma} )$ and $\vec{X}$ is partitioned as $\vec{X} = \left[ \begin{matrix} \vec{X}_1 \\\vec{X}_2 \end{matrix}\right]$,  where $ \vec{X}_1 $ is of dimension $p \times 1$ and $\vec{X}_2$ is of dimension $ (n−p) \times  1$. 

Suppose the corresponding partitions for $\boldsymbol{\mu} $ and $\boldsymbol{\Sigma} $ are given by
$$
\boldsymbol{\mu}  = \left[ \begin{matrix} \boldsymbol{\mu} _1 \\ \boldsymbol{\mu} _2 \end{matrix}\right]
$$
and 
$$
\boldsymbol{\Sigma} =  \left[ \begin{matrix} \boldsymbol{\Sigma}_{11} &  \boldsymbol{\Sigma}_{12} \\ {\boldsymbol{\Sigma}_{12}}^\top
&  \boldsymbol{\Sigma}_{22} \end{matrix}\right]
$$

- Marginal distribution. $ \vec{X}_1 $ is multivariate normal $\sim N_p(\boldsymbol{\mu} _1 , \boldsymbol{\Sigma}_{11} )$.

- Conditional distribution. The distribution of   $ \mathbf{x}_1 \,| \, \mathbf{x}_2 $ is $p$-variate normal
  $$
  \sim N_p(\boldsymbol{\mu} _1 +  \boldsymbol{\Sigma}_{12}\boldsymbol{\Sigma}_{22}^{-1}(\vec{X}_2-\boldsymbol{\mu} _2) , \; \boldsymbol{\Sigma}_{11} -\boldsymbol{\Sigma}_{12} \boldsymbol{\Sigma}_{22}^{-1} {\boldsymbol{\Sigma}_{12}}^\top)
  $$
  



- For a vector $\mathbf{a}$,   $\mathbf{a}^\top\vec{X} \sim N_n( \mathbf{a}^\top\boldsymbol{\mu} , \mathbf{a}^\top \boldsymbol{\Sigma} \,\mathbf{a})$
- $ \left(\vec{X}-\boldsymbol{\mu}\right)^\top \boldsymbol{\Sigma}^{-1}  \left(\vec{X}-\boldsymbol{\mu}\right) \sim \chi^2  (n) $





**reproductive property of normal distribution**: Normality ensures that a linear combination of normally and independently distributed random variables is also a normal random variable (applied statistics and probability for engineering p119 ).





#### Conditional Expectation

Suppose that X and Y are random variables. 

The conditional expectation of $X$ given $Y = y$ is 
$$
 \mathbb{E} (X \,|\, Y=y)=\mu_{X|Y}=\int x \, f_{X|Y}(x\,|\,y)dx
$$

If $r(X, Y)$ is a function of $X$ and $Y$ then 
$$
\mathbb{E} (r(X,Y) \,|\, Y=y)=\int r(x,y) \, f_{X|Y}(x\,|\,y)dx
$$


Note:

Whereas $\mathbb{E} (X)$ is a number, $\mathbb{E} (X \,|\, Y=y)$ is a function of $y$. Before we observe $Y$, we don't know the value of $\mathbb{E} (X \,|\, Y=y)$ so it is a random variable which we denote $\mathbb{E} (X \,|\, Y)$. In other words, $\mathbb{E} (X \,|\, Y)$ is a random variable whose value is $\mathbb{E} (X \,|\, Y=y)$ when $Y=y$.





##### Theorem (The Rule of Iterated Expectations). 

For random variables X and Y, assuming the expectations exist, we have that
$$
\mathbb{E} [\mathbb{E} (Y \,|\, X)]= \mathbb{E}(Y)
$$
More generally, for any function $r(X, Y)$ we have
$$
\mathbb{E} \left[\mathbb{E} (r(X,Y )\,|\, X)\right]= \mathbb{E}(r(X,Y))
$$

$$
\mathbb{E} [\mathbb{E} (Y \,|\, X)] =\int \left( \int y \,f_{Y|X}(y\,|\,x) dy \right)f_X(x)\,dx = \int\int y\, f_{Y|X}(y\,|\,x)f_X(x)\, dxdy = \int\int y\, f_{X,Y}(x,y) \, dxdy =\mathbb{E} (Y)
$$



The conditional variance is defined as 

$$
\mathbb{V}(Y|X = x)= \mathbb{E}\left[(Y-\mu_{Y|X})^2 \,|\,X = x\right] = \int (y-\mu_{Y|X})^2\, f_{Y|X}(y|x)dy
$$


Theorem.

For random variables $X$ and $Y$, 
$$
\mathbb{V}(Y) = \mathbb{E}\left[\mathbb{V}(Y|X )\right] + \mathbb{V}\left[\mathbb{E}(Y|X )\right]
$$

$$
\mathbb{V}(Y) = \mathbb{E}(Y^2)-\mu_Y^2 
\\
\mathbb{E}\left[\mathbb{V}(Y|X )\right] + \mathbb{V}\left[\mathbb{E}(Y|X )\right]= \mathbb{E}\left[\mathbb{E}(Y^2|X ) - \mu_{Y|X}^2\right] +\mathbb{E}\left[\mu_{Y|X}^2-\mu_Y^2 \right] =\mathbb{E}\left[\mathbb{E}(Y^2|X ) \right]-\mu_Y^2= \mathbb{E}(Y^2)-\mu_Y^2
$$
because $\mathbb{E}\left[\mathbb{E}(Y^2|X ) \right]=\int \int y^2 f_{Y|X}(y|x) f_X(x) \, dxdy =\int \int y^2 f_{X,Y}(x,y)   \, dxdy = \mathbb{E}(Y^2)$



Full derivation:


$$
\begin{align*} \mathbb{E}\left[\mathbb{V}(Y|X )\right] & = \int \left[\int (y-\mu_{Y|X})^2\, f_{Y|X}(y|x)\,dy\right] f_X(x) \, dx 
 = \int \left[\int (y^2-2y\,\mu_{Y|X}+\mu_{Y|X}^2) \, f_{Y|X}(y|x)\,dy\right] f_X(x) \, dx
 \\
 & = \int \int y^2 f_{Y|X}(y|x) f_X(x) \, dxdy - 2\int  \mu_{Y|X}^2 f_X(x) \, dx+ \int  \mu_{Y|X}^2 f_X(x) \, dx=  \int \int y^2 f_{X,Y}(x,y) -  \int  \mu_{Y|X}^2 f_X(x) \, dx
  
 \\
 
  \mathbb{V}\left[\mathbb{E}(Y|X )\right] &=  \int  \left( \mu_{Y|X} -\mu_Y \right)^2 f_X(x) \, dx 
 = \int  \mu_{Y|X}^2 f_X(x) \, dx -2 \mu_Y\int \mu_{Y|X} f_X(x)\,dx+  \mu_Y^2 
  = \int  \mu_{Y|X}^2 f_X(x) \, dx -  \mu_Y^2\\
  
\mathbb{V}(Y) &=\mathbb{E}(Y^2) -\mu_Y^2 = \int \int y^2 f_{X,Y}(x,y)  \, dxdy -\mu_Y^2=  \mathbb{E}\left[\mathbb{V}(Y|X )\right] +  \mathbb{V}\left[\mathbb{E}(Y|X )\right] 
   
   \end{align*}
$$









The most important aspect of probability theory concerns the behavior of **sequences of random variables**. This part of probability is called **large sample theory**,or limit theory,or **asymptotic theory**.  

![](https://raw.githubusercontent.com/justinjiajia/img/master/prob_stats/convergence_in_distribution.PNG)

Definition. Let $X_1, X_2, \dots $ be a sequence of random variables and let $X$ be another random variable. Let $F_n$ denote the CDF of $X_n$ and let $F$ denote the cdf of $X$

- $X_n$ converges to $X$ in probability, written $X_n\to_{\text{p}}X$, if, for every $\epsilon>0$, $ \lim_{n\to \infty}\mathbb{P}(|X_n −X| > \epsilon) = 0$.
- $X_n$ converges to $X$ in distribution, written $X_n\to_{\text{d}}X$, if $\lim_{n\to \infty} F_n(t) = \lim_{n\to \infty} \mathbb{P}(X_n \leq t)= F(t)$ at all $t$ for which $F$ is continuous. 
- $X_n$ converges to $X$ in quadratic mean (also called convergence in mean squre or in $\mathscr{l}_2$), written $X_n\to_{\text{q.m.}}X$, if $\lim_{n\to \infty} \mathbb{E} \left[(X_n −X)^2 \right] = 0$.



![](https://raw.githubusercontent.com/justinjiajia/img/master/prob_stats/relationships_among_different_convergence_modes.PNG)



Theorem. The following relationships hold:

- (a) $X_n\to_{\text{q.m.}}X \Rightarrow X_n\to_{\text{p}}X$.
- (b) $ X_n\to_{\text{p}}X \Rightarrow X_n\to_{\text{d}}X$.
- (c) If $X_n\to_{\text{d}}X$ and if $P(X = c)=1$ (almost surely) for some real number $c$, then $ X_n\to_{\text{p}}X$.

In general, none of the reverse implications hold except the special case in (c) ($X_n\to_{\text{p}}c \Leftrightarrow X_n\to_{\text{d}}c$).  



Theorem. 

Let $X_n,X,Y_n,Y$ be random variables and $c$ a constant. Let $g$ be a continuous function.

- (a) $X_n\to_{\text{q.m.}}X,\, Y_n\to_{\text{q.m.}}Y \Rightarrow X_n +Y_n\to_{\text{q.m.}}X+Y$
- (b)  $X_n\to_{\text{p}}X,\, Y_n\to_{\text{p}}Y \Rightarrow X_n +Y_n\to_{\text{p}}X+Y$
- (c) $X_n\to_{\text{d}}X,\, Y_n\to_{\text{d}}c \Rightarrow X_n +Y_n\to_{\text{d}}X+c$
- (d)  $X_n\to_{\text{p}}X,\, Y_n\to_{\text{p}}Y \Rightarrow X_n  Y_n\to_{\text{p}}XY$
- (e) $X_n\to_{\text{d}}X,\, Y_n\to_{\text{d}}c \Rightarrow X_n  Y_n\to_{\text{d}} cX $
- (f) $X_n\to_{\text{p}} X \Rightarrow g(X_n)  \to_{\text{p}}g(X)$
- (g) $X_n\to_{\text{d}} X \Rightarrow g(X_n)  \to_{\text{d}}g(X)$

Parts (c) and (e) are know as **Slutzky’s theorem**. 

Note: $X_n\to_{\text{d}}X,\, Y_n\to_{\text{d}}Y$ does not in general imply that $X_n +Y_n\to_{\text{d}}X+Y$.



The Law of Large Numbers says that the mean of a large sample is close to the mean of the distribution.  



#### Theorem (The Central Limit Theorem (CLT)). 

Let $X_1, \dots, X_n$ be IID with mean $\mu$ and variance $σ^2$ (also called $X_1,\dots,X_n$ a **random sample** of size $n$ from $F$ with mean $\mu$ and variance $σ^2$ (or taken from a population (either finite or infinite)))

Let $ \bar{X}_n = \frac{1}{n} \sum_{i=1}^n X_i$ (sample mean). 
$$
\frac{ \bar{X}_n -\mu}{\mathbb{V}(\bar{X}_n)}= \frac{ \sqrt{n}(\bar{X}_n -\mu)}{\sigma} \to_{\text{d}} N(0,1)
$$
As a general guideline, if $n > 30$, the central limit theorem will almost always apply. There are exceptions to this guideline, but they are relatively rare.



Theorem. 

Assume the same conditions as the CLT. Then, 
$$
\frac{ \sqrt{n}(\bar{X}_n -\mu)}{S_n} \to_{\text{d}} N(0,1)
$$
where $S_n^2= \frac{1}{n-1}\sum_{i=1}^n(X_i-\bar{X}_n)^2$.


$$
S_n^2= \frac{1}{n-1}\sum_{i=1}^n(X_i-\bar{X}_n)^2=\frac{n}{n-1} \frac{1}{n} \sum_{i=1}^n(X_i^2-2 X_i\bar{X}_n+\bar{X}_n^2)\\
= \frac{n}{n-1} \frac{1}{n} \left[\sum_{i=1}^n X_i^2  -n\bar{X}_n^2 \right]
$$


because $\bar{X}_n \to_{\text{p}} \mu $, $\bar{X}_n^2 \to_{\text{p}} \mu^2 $

Because, the sequence of $X_i^2$ is IID with the mean  $\mathbb{E}(X_i^2)=\mu^2+\sigma^2$, the LLN indicates $\frac{1}{n}\sum_{i=1}^n X_i^2 \to_{\text{p}} \mu^2+\sigma^2 $

It proves that $S_n^2$  is a  consistent estimator for $\sigma^2$
$$
S_n^2= \frac{n}{n-1} \frac{1}{n} \left[\sum_{i=1}^n X_i^2  -n\bar{X}_n^2 \right] = \frac{n}{n-1}  \left[\frac{1}{n}\sum_{i=1}^n X_i^2  -\bar{X}_n^2 \right]  \to_{\text{p}}  \sigma^2
$$
By Slusky's theorem and (f):
$$
\frac{ \sqrt{n}(\bar{X}_n -\mu)}{S_n} = \frac{ \sqrt{n}(\bar{X}_n -\mu)}{\sigma}\frac{\sigma}{S_n} \to_{\text{d}} N(0,1)
$$




Theorem (Multivariate central limit theorem). 

Let $\vec{X}_1,...,\vec{X}_n$ be IID random vectors where $\vec{X}_i= \left[\begin{matrix} X_{1i} \\ \vdots \\ X_{ki}  \end{matrix} \right]$ with mean $\boldsymbol{\mu}= \left[\begin{matrix} \mu_1 \\ \vdots \\ \mu_k  \end{matrix} \right]$and variance $\boldsymbol{\Sigma}$

Let $\vec{\bar{X}}_n= \left[\begin{matrix}\bar{X}_1 \\ \vdots \\ \bar{X}_k \end{matrix} \right]$  where $\bar{X}_j =\frac{1}{n}\sum_{i=1}^n X_{ji}$. Then
$$
\sqrt{n}(\vec{\bar{X}}_n -\boldsymbol{\mu}) \to_{\text{d}} N(0,\boldsymbol{\Sigma})
$$


##### The Delta Method



If $Y_n$ has a limiting Normal distribution then the delta method allows us to find the limiting distribution of g(Yn) where g is any smooth function

Theorem (The Delta Method). 

Suppose that 
$$
\frac{ \sqrt{n}(Y_n -\mu)}{\sigma} \to_{\text{d}} N(0,1)
$$
and that $g$ is a differentiable function such that $g'(\mu)\neq 0$. Then
$$
\frac{ \sqrt{n}(g(Y_n) -g(\mu))}{|g'(\mu)|\sigma} \to_{\text{d}} N(0,1)
$$
Theorem (The Multivariate Delta Method). Suppose that $\vec{Y}_n= \left[\begin{matrix} Y_{n1} \\ \vdots \\ Y_{nk}  \end{matrix} \right]$ is a sequence of random vectors such that
$$
\sqrt{n}(\vec{Y}_n -\boldsymbol{\mu}) \to_{\text{d}} N(0, \boldsymbol{\Sigma})
$$
Let $g : \mathbb{R}^k \to \mathbb{R}$ and let $\nabla g(\mathbf{y}) $ denote the gradient of $g$ and $\nabla g(\boldsymbol{\mu}) $ denote $\nabla g(\mathbf{y}) $ evaluated at $\mathbf{y} = \boldsymbol{\mu} $ and assume that the elements of $\nabla g(\boldsymbol{\mu}) $ are nonzero. Then 
$$
\sqrt{n}\left(g\left(\vec{Y}_n\right) -g(\boldsymbol{\mu})\right) \to_{\text{d}} N\left(0,   \nabla g(\boldsymbol{\mu})^\top  \boldsymbol{\Sigma}  \nabla g(\boldsymbol{\mu})\right)
$$


**Point estimation** refers to providing a single “best guess” of some quantity of interest. The quantity of interest could be a parameter in a parametric model, a CDF $F$, a probability density function $f$, a regression function $r$,or a prediction for a future value $Y$ of some random variable.

More formally, 

let $X_1,\dots,X_n$ be $n$ IID data points from some distribution $F$. A point estimator $\hat{\theta}_n $of a parameter $\theta$ is some function of $X_1,\dots,X_n$:



The bias of an estimator is defined by
$$
\text{bias}=\mathbb{E}(\hat{\theta}_n)-\theta
$$


We say that $\hat{\theta}_n$ is unbiased if $\mathbb{E}(\hat{\theta}_n)= \theta$. **Unbiasedness** used to receive much attention but these days is considered less important; many of the estimators we will use are biased. 

A reasonable requirement for an estimator is that it should converge to the true parameter value as we collect more and more data. This requirement is quantified by consistency

A point estimator $\hat{θ}_n$ of a parameter $θ$ is **consistent** if $\hat{θ}_n \to_{\text{d}} \theta$



The distribution of $\hat{θ}_n$ is called the sampling distribution. The standard deviation of $\hat{θ}_n$ is called the standard error, denoted by $\text{se}$.



The quality of a point estimate is sometimes assessed by the **mean squared error**, or mse defined by
$$
\operatorname{MSE}(\hat{θ}_n) =  \mathbb{E} \left[ (\hat{θ}_n-\theta )^2\right]
$$



The mean squared error can be rewritten as follows:
$$
\operatorname{MSE}(\hat{θ}_n) =  \mathbb{E} \left[ (\hat{θ}_n-\theta )^2\right]= \mathbb{E} \left[ (\hat{θ}_n- \mathbb{E}(\hat{θ}_n) +\mathbb{E}(\hat{θ}_n)  -\theta )^2\right] \\
 = \mathbb{V}(\hat{θ}_n)+ (\operatorname{bias})^2
$$
where we have used the fact that $\mathbb{E}(\hat{\theta}_n − \mathbb{E}(\hat{θ}_n))=0$.

Keep in mind that this expectation $ \mathbb{E} \left[ (\hat{θ}_n-\theta )^2\right]$ is taken with respect to the distribution $f_{X_1,\dots,X_n}(x_1,\dots,x_n; \theta)=\prod_{i=1}^n f(x_i; \theta)$ (the sampling is IID).

Many of the estimators we will encounter will turn out to have, approximately, a Normal distribution.

An estimator is asymptotically Normal if
$$
\frac{\sqrt{n}(\hat{\theta}_n-\theta)}{\text{se}} \to_{\text{d}}N(0,1)
$$




Sometimes we find that biased estimators are preferable to unbiased estimators because they have smaller mean squared error. That is, we may be able to reduce the variance of the estimator considerably by introducing a relatively small amount of bias.

![](https://raw.githubusercontent.com/justinjiajia/img/master/prob_stats/mse_comparison.PNG)

<caption> A biased estimator $\hat{\Theta}_1$  that has smaller variance than the unbiased estimator $\hat{\Theta}_2$ .</caption>

An estimate based on $\hat{\Theta}_1$ would more likely be close to the true value of $\theta$ than would an estimate based on $\hat{\Theta}_2$. Linear regression analysisis an example of an application area in which biased estimators are occasionally used.

An estimator Θ̂ that has a mean squared error that is less than or equal to the mean squared error of any other estimator, for all values of the parameter θ, is called an optimal estimator of θ. Optimal estimators rarely exist.













Using the fact that linear combinations of independent normal random variables follow a normal distribution, the sampling distribution of $\bar{X}_1-\bar{X}_2$ is normal with mean and variance:


$$
\mu_{\bar{X}_1-\bar{X}_2} =\mu_1-\mu_2\\
\sigma_{\bar{X}_1-\bar{X}_2}^2=E\left(\left(\bar{X}_1-\bar{X}_2-(\mu_1-\mu_2) \right)^2\right) = E\left(\left(\bar{X}_1-\mu_1\right)^2\right) + E\left(\left(\bar{X}_2-\mu_2\right)^2\right) = \frac{\sigma_1^2}{n_1} +\frac{\sigma_2^2}{n_2}
$$




**Approximate Sampling Distribution of a Difference in Sample Means**

 If we have $2$ independent populations with means $\mu_1$ and $\mu_2$ and variances $\sigma^2_1$ and $\sigma^2_2$ and if $\bar{X}_1$ and $\bar{X}_2$ are the sample means of two independent random samples of sizes $n_1$ and $n_2$ from these populations, then the sampling distribution of
$$
Z = \frac{\bar{X}_1-\bar{X}_2-(\mu_1-\mu_2)}{\sqrt{\frac{\sigma^2_1}{n_1} +\frac{\sigma^2_2}{n_2}} }
$$
is approximately standard normal if the conditions of the central limit theorem apply. If the two populations are normal, the sampling distribution of Z is exactly standard normal.











#### methods for obtaining point estimators

the method of moments and the method of maximum likelihood. Maximum likelihood estimates are generally preferable to moment estimators because they have better efficiency properties. 





#### Maximum Likelihood
The most common method for estimating parameters in a parametric model is the maximum likelihood method. 

Let $X_1, \dots, X_n$  be IID with PDF $f(x; \theta)$. The likelihood function is defined by 
$$
L_n(\theta)= \prod_{i=1}^n f(X_i;\theta)
$$
The likelihood function is just the joint density of the data, except that we treat it is a function of the parameter $\theta$. Thus, $L_n \colon \; \Theta\to [0,\infty) $

The likelihood function is not a density function: in general, it is not true that $L_n(\theta)$ integrates to $1$ (with respect to $\theta$).







Note: to use maximum likelihood estimation, remember that the distribution of the population must be either known or assumed.



#### Properties of a Maximum Likelihood Estimator



Under certain conditions (very general and not restrictive) on the model, the maximum likelihood estimator $\hat{θ}_n$ possesses many properties that make it an appealing choice of estimator. The main properties of the MLE are:

- The MLE is consistent: $\hat{θ}_n \to_{\text{p}} \theta_*$ where $\theta_*$	denotes the true value of the parameter $\theta$;
- The MLE is equivariant: if $\hat{θ}_n$ is the MLE of $\theta$, then $g(\hat{θ}_n)$ is the MLE of $g(\theta)$; 
- The MLE is asymptotically Normal: $(\hat{\theta}_n − \theta_*	)/\hat{\text{se} } \sim N(0, 1)$; also, the estimated standard error $\hat{\text{se} }$ can often be computed analytically; 
- The mle is asymptotically optimal or efficient: roughly, this means that among all well-behaved estimators, the mle has the smallest variance, at least for large samples; 
- The mle is approximately the Bayes estimator. 



the ML estimator for $\sigma^2$, the variance of the normal distribution, is $\hat{\sigma}^2=\frac{\sum_{i=1}^{n}(X_i-\bar{X})^2}{n}$
$$
E(\hat{\sigma}^2)= \frac{n-1}{n}\sigma^2
$$


So $\hat{\sigma}^2$ tends to underestimate the true variance $\sigma^2$. 

But the bias $E(\hat{\sigma}^2) -\sigma^2 = -\frac{1}{n}\sigma^2$ approaches zero as $n$ increases. Therefore,  $\hat{\sigma}^2$ is an asymptotically unbiased estimator for $\sigma^2$  (the “asymptotic” means “when $n$ is large”).

 



##### Consistency of Maximum Likelihood Estimators 



Consistency means that the mle converges in probability to the true value. 

To proceed, we need a definition. If f and g are pdf’s, define the Kullback-Leibler distance (this is not a distance in the formal sense because $D(f, g)$ is not symmetric) between $f$ and $g$ to be $D(f,g) = \int f(x) \log \frac{ f(x)} {g(x)}\,  dx$. It can be shown that $D(f,g) \geq 0$ and $D(f,f) = 0$. For any $\theta, \psi \in \Theta$ write $D(\theta, \psi)$ to mean $D(f(x; \theta), f(x; \psi))$.



We will say that the model $\mathfrak{F}$ is identifiable if $\theta \neq \psi$ implies that $D(\theta, \psi)>0$. This means that different values of the parameter correspond to different distributions. We will assume from now on the the model is identifiable

Let $\theta_*$ denote the true value of $\theta$. Maximizing $L_n(\theta)$ is equivalent to maximizing 
$$
M_n(\theta) = \frac{1}{n} \sum_{i=1}^n \log \frac{f(X_i; \theta)}{ f(X_i; \theta_*	)}
$$
This follows since $M_n(\theta) = \frac{1}{n}  \sum_{i=1}^n \left(\log f(X_i; θ)-\log f(X_i; \theta_*	)\right)$ where $ \frac{1}{n}  \sum_{i=1}^n \log f(X_i; \theta_*	)$ is a constant.

By the law of large numbers, $M_n(\theta) $ converges to
$$
\mathbb{E}_{\theta_*}\left[    \log \frac{f(X_i; θ)}{ f(X_i; \theta_*	)}\right]= \int   - \log\left( \frac{f(x; \theta_*)}{ f(x;\theta 	)}\right) f(x; \theta_*	)\, dx =-D (\theta_*, \theta)
$$
Hence $M_n(\theta) \approx -D (\theta_*, \theta)$ which is maximized at $\theta_*$ since $-D (\theta_*, \theta)=0$ and $-D (\theta_*, \theta)<0$ for $ \theta \neq \theta_*$. 

Therefore, we expect that the maximizer will tend to $\theta_*$. 

To prove this formally, we need more than $M_n(\theta) \to_{\text{p}} -D (\theta_*, \theta)$. 

We need this convergence to be uniform over $\theta$. We also have to make sure that the function $D (\theta_*, \theta)$ is well behaved. Here are the formal details



Theorem. Let $\theta_*$ denote the true value of $\theta$. Define $M_n(\theta) = \frac{1}{n} \sum_{i=1}^n \log \frac{f(X_i; \theta)}{ f(X_i; \theta_*	)}$ and $M(\theta) = -D (\theta_*, \theta)$. 

Suppose that 
$$
\sup_{θ\in \Theta} |M_n(\theta) − M(θ)| \to_{\text{p}}  0
$$


and that, for every $\epsilon  > 0$, ($D (\theta_*, \theta)$ is well behaved)
$$
\sup_{\theta\colon |\theta-\theta_*|\geq \epsilon} M(\theta) < M(\theta_*)
$$
Let $\hat{θ}_n$ denote the mle. Then $\hat{θ}_n \to_{\text{p}} \theta_*$.





**Invariance Property**

Let Θ̂ 1, Θ̂ 2,…, Θ̂ k be the maximum likelihood estimators of the parameters θ1, θ2, … , θk. Then the maximum likelihood estimator of any function h(θ1, θ2, … , θk) of these parameters is the same function h(Θ̂ 1, Θ̂ 2,…, Θ̂ k) of the estimators Θ̂ 1, Θ̂ 2,…, Θ̂ k.



Let $X_1, \dots, X_n$ be a random sample from a population with ***unknown*** mean $\mu$ and variance $\sigma^2$. Now if the sample size n is large, the central limit theorem implies that $\bar{X}$ has approximately a normal distribution with mean $\mu$ and variance $\frac{\sigma^2}{n}$

Large-Sample Confidence Interval on the Mean 

When $n$ is large, the quantity
$$
\frac{\bar{X}-\mu}{\frac{S}{\sqrt{n}}}
$$


has an approximate standard normal distribution. Consequently, 
$$
\bar{X} - Z_{\alpha/2} \frac{S}{\sqrt{n}} \leq \mu \leq \bar{X}+Z_{\alpha/2} \frac{S}{\sqrt{n}}
$$
is a large-sample confidence interval for $\mu$, with confidence level of approximately $1 − \alpha$.

Note: $S^2$ is the sample variance; that is, $S^2 =\frac{\sum_{i=1}^{n}(X_i-\bar{X})^2}{n-1}$

The inequalities hold regardless of the shape of the population distribution. Generally, $n$ should be at least 40 to use this result reliably. The central limit theorem generally holds for $n \geq 30$, but the larger sample size is recommended here because using the sample standard deviation $S$ in $Z$ results in additional variability.





when the sample is small and σ2 is unknown, we must make an assumption about the form of the underlying distribution to obtain a valid CI procedure. A reasonable assumption in many cases is that the underlying distribution is normal.



Suppose that the population of interest has a normal distribution with unknown mean μ and unknown variance $\sigma^2$. Assume that a random sample of size n, say, $X_1, \dots, X_n$, is available, and let $\bar{X}$ and $S^2$$ be the sample mean and variance, respectively. We wish to construct a two-sided CI on μ

- If the variance $\sigma^2$ is known, we know that $Z = \frac{\bar{X} − \mu}{\frac{\sigma}{\sqrt{n}}}$ has a standard normal distribution. 
- When  $\sigma^2$  is unknown, a logical procedure is to replace σ with the sample standard deviation $S$. The random variable $Z$ now becomes T = (X − μ)∕(S∕ √n). A logical question is what effect replacing σ with S has on the distribution of the random variable T. 
  - If $n$ is large, the answer to this question is “very little,” and we can proceed to use the confidence interval based on the normal distribution from as shown above. 
  - When $n$ is small, a $t$ distribution with $n − 1$ degrees of freedom must be employed to construct the confidence interval. 



$t$ Distribution  (p180)

Let $X_1, \dots, X_n$ be a random sample from a ***normal distribution*** with unknown mean $\mu$ and unknown variance $\sigma^2$. The random variable 
$$
t=\frac{\bar{X}-\mu}{\frac{S}{\sqrt{n}}}
$$


has a $t$ distribution with $n − 1$ degrees of freedom.

Note: the $t$ distribution has heavier tails than the normal; that is, it has more probability in the tails than does the normal distribution. As the number of degrees of freedom $k \to \infty$, the limiting form of the t distribution is the standard normal distribution.





Confidence Interval on the Variance and Standard Deviation of a Normal Distribution

Sometimes confidence intervals on the population variance or standard deviation are needed.

$\chi^2$ Distribution 

Let $X_1, \dots, X_n$ be a random sample from a normal distribution with mean $\mu$ and variance σ2, and let $S^2$ be the sample variance. Then the random variable 
$$
\chi^2 = \frac{(n-1)S^2}{\sigma^2}
$$



has a chi-square ($\chi^2$) distribution with $n − 1$ degrees of freedom



The relationship between


$$
t=\frac{\bar{X}-\mu}{\frac{S}{\sqrt{n}}}= \frac{\frac{\bar{X}-\mu}{ \frac{\sigma}{\sqrt{n}} }}{ \sqrt{\frac{(n-1)S^2}{\sigma^2}/(n-1)}}
$$
the nominator $\frac{\bar{X}-\mu}{ \frac{\sigma}{\sqrt{n}} } \sim N(0,1)$ and $\frac{(n-1)S^2}{\sigma^2} \sim \chi^2(n-1)$ 

Fact: If $x \sim N(0, 1)$, $y \sim \chi^2(m)$ and if $x$ and $y$ are independent, then the ratio $x/ \sqrt{y/m}$ has the $t$ distribution with $m$ degrees of freed

Fact: If $\mathbf{x}$ and $\mathbf{y}$ are independently distributed, then so are $f (\mathbf{x})$ and $g(\mathbf{y})$.

if normally distributed random variabes are uncorrelated, they are indenpendent.

To prove that $\frac{\bar{X}-\mu}{ \frac{\sigma}{\sqrt{n}} }$ and $\frac{(n-1)S^2}{\sigma^2}$ are independent, the remaining is to show $\bar{X}$ and $X_i - \bar{X}$ are uncorrelated.





type-I: falsely reject the null hypothesis

type-II: falsely accept the null hypothesis

Sometimes the type I error probability is called the significance level, the α-error, or the size of the test



The **P-value** is the smallest level of significance that would lead to rejection of the null hypothesis H0 with the given data.

The P-value tells us that if the null hypothesis H0 = 50 is true, the probability of obtaining a random sample whose mean is at least as far from 50 as 51.3 (or 48.7) is 0.038. Therefore, an observed sample mean of 51.3 is a fairly rare event if the null hypothesis H0 = 50 is really true.



General Procedure for Hypothesis Tests 

The following sequence of steps in applying hypothesis-testing methodology is recommended:

1. Parameter of interest: From the problem context, identify the parameter of interest. 
2. Null hypothesis, H0: State the null hypothesis, H0. 
3. Alternative hypothesis, H1: Specify an appropriate alternative hypothesis, H1.
4. Test statistic: Determine an appropriate test statistic. 
5. Reject H0 if: State the rejection criteria for the null hypothesis. 
6. Computations: Compute any necessary sample quantities, substitute these into the equation for the test statistic, and compute that value. 
7. Draw conclusions: Decide whether or not H0 should be rejected and report that in the problem context.



Inference on the Variances of Two Normal Distributions



Let $W$ and $V$ be independent chi-square random variables with $u$ and $v$ degrees of freedom, respectively. Then the ratio 
$$
F= \frac{W/u}{V/v}
$$
is said to follow the $F$ distribution with $u$ degrees of freedom in the numerator and $v$ degrees of freedom in the denominator. It is usually abbreviated as $F_{u,v}$.

The $F$ distribution looks very similar to the $\chi^2$ distribution; however, the two parameters $u$ and $v$ provide extra flexibility regarding shape.



Distribution of the Ratio of Sample Variances from Two Normal Distributions

Let $X_{11}, X_{12}, \dots, X_{1n_1}$ be a random sample from a normal population with mean $\mu_1$ and variance $\sigma_1^2$, and let $X_{21}, X_{22},\dots, X_{2n_2}$  be a random sample from a second normal population with mean $\mu_2$ and variance $\sigma_2^2$. Assume that both normal populations are independent. Let $S^2_1$ and $S^2_2$ be the sample variances. Then the ratio 
$$
F = \frac{S_1^2/\sigma_1^2} {S_2^2/\sigma_2^2}
$$
has an $F$ distribution with $n_1 − 1$ numerator degrees of freedom and $n_2 − 1$ denominator degrees of freedom.

Tests on the Equality of Variances from Two Normal Distributions
$$
H_0: \sigma_1^2=\sigma_2^2
$$
Under $H_0$, $\frac{S_1^2} {S_2^2}$ has an $F(n_1-1,\, n_2-1)$ distribution





- 
