





The Classical Linear Regression Model

Assumption 1.1 (linearity) $\mathbf{y} =   X\boldsymbol{\beta} + \boldsymbol{\epsilon}$

Assumption 1.2 (strict exogeneity): $\text{E}(\epsilon_i | X) = 0$ for $i = 1,\dots,n$

Assumption 1.3 (no multicollinearity): The rank of the $n \times K$ data matrix, $X$, is $K$ with probability $1$.

Assumption 1.4 (spherical error variance): (homoskedasticity) 
$$
\text{E} (\epsilon_i^2\, | \, \mathbf{X}) = \sigma^2 >0 \quad (i=1,2,\dots, n)
$$


(no correlation between observations)
$$
\text{E}(\epsilon_i \epsilon_j \, |\, \mathbf{X}) = 0 \quad (i,j =1,2,\dots, n;\; i\neq j)
$$


The OLS Estimator
$$
\mathbf{b} = \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X} ^\top\mathbf{y}
$$


Finite-Sample Distribution of $\mathbf{b} $ ($\mathbf{b}$ is a statistic like the )


$$
\text{E}(\mathbf{b}\, |\, \mathbf{X})=  E\left( \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X} ^\top \mathbf{y}\, | \,\mathbf{X}\right)= \text{E} \left( \boldsymbol{\beta} + \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X} ^\top   \boldsymbol{\epsilon} \,|\,\mathbf{X}\right)
\\=  \boldsymbol{\beta} + \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top \; \text{E}( \boldsymbol{\epsilon}\, |\,\mathbf{X}) =  \boldsymbol{\beta}
$$



$$
\text{Var}(\mathbf{b}\, |\, \mathbf{X}) = \text{E} \left( (\mathbf{b} -\boldsymbol{\beta}) (\mathbf{b} -\boldsymbol{\beta})^\top \,|\,\mathbf{X} \right)
= \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top  \;\text{E}  \left(   \boldsymbol{\epsilon}  \boldsymbol{\epsilon}^\top  \,|\, \mathbf{X} \right) \mathbf{X}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\\
=  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top  \sigma^2 I \mathbf{X}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} = \sigma^2\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}
$$




(Gauss-Markov Theorem) Under Assumptions 1.1–1.4, the OLS estimator is efficient in the class of linear unbiased estimators. That is, for any unbiased estimator $\hat {\boldsymbol{\beta} }$ that is linear in $\mathbf{y}$,  $ \text{Var}(\hat {\boldsymbol{\beta} }|X)  \geq \text{Var}(\mathbf{b} |X) $ in the matrix sense.


$$
\text{Cov}(\mathbf{b}, \mathbf{e} \, | \, \mathbf{X}) = \text{E} \left(
\left[\mathbf{b} − \text{E}(\mathbf{b}\,|\, \mathbf{X}) \right]\left[\mathbf{e}  − \text{E}(\mathbf{e} \,|\, \mathbf{X})\right]^\top \, | \, \mathbf{X}\right)
\\=  \text{E} \left(
 \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top  \boldsymbol{\epsilon}  \boldsymbol{\epsilon} ^\top  \left( I-\mathbf{X}\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top  \right)  \, |\, \mathbf{X}\right)
 
 \\ =  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top  \; \text{E} \left(
      \boldsymbol{\epsilon}  \boldsymbol{\epsilon} ^\top  \,| \,\mathbf{X}\right) \left( I-\mathbf{X}\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top  \right)
      \\= \sigma^2  \left( \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top - \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top \mathbf{X}\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top  \right) = \mathbf{0}
$$






The sum of squared residuals is calculated as follows:

$$
\mathbf{e}^\top\mathbf{e}  = (\mathbf{y}- \hat{\mathbf{y}})^\top(\mathbf{y} - \hat{\mathbf{y}})\\

= \left[\left( I- \mathbf{X}\left( \mathbf{X} ^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top  \right) ( \mathbf{X} \boldsymbol{\beta}+\boldsymbol{\epsilon})\right]^\top  \left[\left( I-\mathbf{X}\left( \mathbf{X} ^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top  \right) ( \mathbf{X} \boldsymbol{\beta}+\boldsymbol{\epsilon})\right] \\
= \boldsymbol{\epsilon} ^\top \mathbf{MM}  \boldsymbol{\epsilon} =\boldsymbol{\epsilon} ^\top \mathbf{M}  \boldsymbol{\epsilon}
$$


$M= I-X\left( X^\top X\right)^{-1}X^\top$ is idempotent


$$
\text{E}\left(\mathbf{e}^\top\mathbf{e} \,| \, \mathbf{X} \right)  =
\text{E}\left(\boldsymbol{\epsilon} ^\top \mathbf{M}   \boldsymbol{\epsilon}\,|\, \mathbf{X} \right)

\\= \text{E}\left(\sum_{i=1}^n \sum_{j=1}^n a_{ij}\epsilon_i\epsilon_j \,| \,\mathbf{X}\right)
=\text{E} \left(\sum_{i=1}^n  a_{ii} \epsilon_i^2 \,|\,\mathbf{X}\right) = \left(\sum_{i=1}^n  a_{ii}\right)\sigma^2
= (n-K)\sigma^2
$$


because the trace of $\mathbf{M}$ is $n-K$.



The intuitive reason is that $K$ parameters ($\boldsymbol{\beta}$) have to be estimated ($\mathbf{b}$) before obtaining the residual vector $\mathbf{e}$ used to calculate the sum of squared residuals. More specifically, $\mathbf{e}$ has to satisfy the $K$ normal equations
$$
X^\top(\mathbf{y}-\mathbf{X} \mathbf{b}) = \mathbf{X} ^\top \mathbf{e}= \mathbf{0}
$$
which limits the variability of the residual.





 Hypothesis Testing under Normality



Assumption 1.5 (normality of the error term): The distribution of $\boldsymbol{\epsilon}$ conditional on $\mathbf{X}$ is jointly normal



the normal distribution has several convenient features:

- The distribution depends only on the mean and the variance. Thus, once the mean and the variance are known, you can write down the density function. If the distribution conditional on $\mathbf{X}$ is normal, the mean and the variance can depend on $\mathbf{X}$. 

  Suppose $ \left[ \begin{matrix} \boldsymbol{\epsilon} \\ \text{flatten}(\mathbf{X}) \end{matrix}\right] \sim N (\boldsymbol{\mu} , \boldsymbol{\Sigma} )$ where $\boldsymbol{\mu} , \boldsymbol{\Sigma} $ can be partitioned accordingly.
  $$
  \boldsymbol{\epsilon} \, |\, \mathbf{X} \sim N_p(\boldsymbol{\mu} _1 +  \boldsymbol{\Sigma}_{12}\boldsymbol{\Sigma}_{22}^{-1}( \text{flatten}(\mathbf{X})-\boldsymbol{\mu} _2) , \; \boldsymbol{\Sigma}_{11} -\boldsymbol{\Sigma}_{12} \boldsymbol{\Sigma}_{22}^{-1} {\boldsymbol{\Sigma}_{12}}^\top)
  $$
  It follows that, if the distribution conditional on $\mathbf{X}$ is normal and if neither the conditional mean nor the conditional variance depends on $\mathbf{X}$, then the marginal (i.e., unconditional) distribution is the same normal distribution. 

  

- In general, if two random variables are independent, then they are uncorrelated, but the converse is not true. However, if two random variables are joint normal, the converse is also true, so that independence and a lack of correlation are equivalent. This carries over to conditional distributions: if two random variables are joint normal and uncorrelated conditional on X, then they are independent conditional on $\mathbf{X}$.

- A linear function of random variables that are jointly normally distributed is itself normally distributed. This also carries over to conditional distributions. If the distribution of $\boldsymbol{\epsilon}$ conditional on $\mathbf{X}$ is normal, then $A\boldsymbol{\epsilon}$, where the elements of matrix $A$ are functions of $\mathbf{X}$, is normal conditional on $\mathbf{X}$.





It is thanks to these features of normality that Assumption 1.5 delivers the following properties to be exploited in the derivation of test statistics: 

- The mean and the variance of the distribution of $\boldsymbol{\epsilon}$  conditional on X are already specified in Assumptions 1.2 and 1.4. Therefore, Assumption 1.5 together with Assumptions 1.2 and 1.4 implies that the distribution of $\boldsymbol{\epsilon}$  conditional on $\mathbf{X}$ is
  $$
  \boldsymbol{\epsilon} \, |\, \mathbf{X} \sim N(0, \sigma^2 I_n)
  $$
  Thus, the distribution of $\boldsymbol{\epsilon}$ conditional on $X$ does not depend on $X$. It then follows that $\boldsymbol{\epsilon}$ and $\mathbf{X}$ are independent. Therefore, in particular, the marginal or unconditional distribution of $\boldsymbol{\epsilon}$ is $N(0, \sigma^2 I_n)$.

- The sampling error $\mathbf{b} -\boldsymbol{\beta}= \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\mathbf{X}^\top \boldsymbol{\epsilon}$ is linear in $\boldsymbol{\epsilon}$ given $\mathbf{X}$. Since $\boldsymbol{\epsilon}$ is normal given $\mathbf{X}$, so is the sampling error. Thus, under Assumptions 1.1–1.5
  $$
  \mathbf{b} -\boldsymbol{\beta} \, | \,\mathbf{X} \sim N\left(\mathbf{0},  \sigma^2\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\right)
  $$
  





$\bar{ β}_k$  is some known value specified by the null hypothesis

Proposition 1.3 (distribution of the t-ratio): 

Suppose Assumptions 1.1–1.5 hold. Under the null hypothesis $H_0 : β_k = \bar{ β}_k$ , the $t$-ratio defined as
$$
t_k \equiv \frac{b_k-\bar{ β}_k}{\sqrt{s^2\left(\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\right)_{kk}}}
$$
where $s^2= \frac{\mathbf{e}^\top\mathbf{e} }{n-K}$, is distributed as $t(n − K)$ (the $t$ distribution with $n − K$ degrees of freedom)


$$
t_k = \frac{b_k-\bar{ β}_k}{\sqrt{s^2\left(\left( \mathbf{X}^\top\mathbf{X}\right)^{-1}\right)_{kk}}} 
= \frac{ \frac{b_k-\bar{ β}_k} {\sqrt{\sigma^2\left(\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\right)_{kk}}}} {\sqrt{\frac{s^2 }{ \sigma^2 }   }  }
\\ = \frac{ \frac{b_k-\bar{ β}_k} {\sqrt{\sigma^2\left(\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\right)_{kk}}}} {\sqrt{\frac{  \frac{\mathbf{e}^\top\mathbf{e} }{ \sigma^2 } }{n-K  }   }  }
$$
$ \frac{b_k-\bar{ β}_k} {\sqrt{\sigma^2\left(\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\right)_{kk}}}\sim N(0,1)$ and $ \frac{\mathbf{e}^\top\mathbf{e} }{ \sigma^2 } \sim \chi^2(n-K) $





Fact: If $\mathbf{x} \sim N(0,I_n)$ and $A$ is idempotent, then $\mathbf{x}^\top A \mathbf{x}$ has a $\chi^2$ distribution with degrees of freedom equal to the rank of $A$

Fact: If $x \sim N(0, 1)$, $y \sim \chi^2(m)$ and if $x$ and $y$ are independent, then the ratio $x/ \sqrt{y/m}$ has the $t$ distribution with $m$ degrees of freed

Fact: If $\mathbf{x}$ and $\mathbf{y}$ are independently distributed, then so are $f (\mathbf{x})$ and $g(\mathbf{y})$.



Both $\mathbf{b}$ and $\mathbf{e}$ are linear functions of $\boldsymbol{\epsilon}$, so they are jointly normal conditional on $X$. Also, they are uncorrelated conditional on $X$. So $\mathbf{b}$ and $\mathbf{e}$ are independently distributed conditional on $X$. But $ \frac{b_k-\bar{ β}_k} {\sqrt{\sigma^2\left(\left( \mathbf{X}^\top \mathbf{X}\right)^{-1}\right)_{kk}}}$ is a function of $\mathbf{b}$ and $\frac{\mathbf{e}^\top\mathbf{e} }{ \sigma^2 }$ is a function of $\mathbf{e}$. So they are independently distributed conditional on $X$.





Linear Hypotheses The null hypothesis we wish to test may not be a restriction about individual regression coefficients of the maintained hypothesis; it is often about linear combinations of them written as a system of linear equations:
$$
H_0: \mathbf{R} \boldsymbol{\beta} =\mathbf{r}
$$




where values of $R$ and $\mathbf{r}$ are known and specified by the hypothesis.



The $F$-Test To test linear hypotheses, we look for a test statistic that has a known distribution under the null hypothesis. 

Proposition 1.4 (distribution of the F-ratio): Suppose Assumptions 1.1–1.5 hold. Under the null hypothesis $H_0: \mathbf{R}\boldsymbol{\beta} =\mathbf{r}$, where $\mathbf{R}$ is a $D_r \times K$ full row rank matrix , the $F$-ratio defined as
$$
F \equiv \frac{ (\mathbf{R} \mathbf{b} -\mathbf{r})^\top \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} (\mathbf{R} \mathbf{b} -\mathbf{r}) /D_r }{s^2}
$$
is distributed as $F(D_r, n − K)$ (the $F$ distribution with $D_r$ and $n − K$ degrees of freedom).



1) 

$$
F = \frac{ (\mathbf{R} \mathbf{b} -\mathbf{r})^\top \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} (\mathbf{R} \mathbf{b} -\mathbf{r}) /D_r }{s^2}
\\ = \frac{ \frac{(\mathbf{R} \mathbf{b} -\mathbf{r})^\top \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} (\mathbf{R} \mathbf{b} -\mathbf{r}) } { \sigma^2 }  /D_r}   { \frac{\mathbf{e}^\top\mathbf{e} }{ \sigma^2 }/(n-K) }
$$


$$
\frac{(\mathbf{R} \mathbf{b} -\mathbf{r})^\top \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} (\mathbf{R} \mathbf{b} -\mathbf{r}) } { \sigma^2 } 
=\frac{  (\mathbf{b}-\boldsymbol{\beta} )^\top\mathbf{R}^\top  \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} \mathbf{R}(\mathbf{b}-\boldsymbol{\beta} ) } {\sigma^2 } 
\\ = \frac{  \boldsymbol{\epsilon}^\top \mathbf{X } \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}  \mathbf{R}^\top  \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} \mathbf{R}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top  \boldsymbol{\epsilon}} {\sigma^2 }
$$


Let $\mathbf{A}=\mathbf{X } \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}  \mathbf{R}^\top  \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} \mathbf{R}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top $, $\mathbf{A}$ is idemponent as
$$
\mathbf{A}^2= \mathbf{X } \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}  \mathbf{R}^\top  \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} \mathbf{R}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top \mathbf{X } \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}  \mathbf{R}^\top  \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} \mathbf{R}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top 
\\= \mathbf{X } \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}  \mathbf{R}^\top  \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} \mathbf{R}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top =\mathbf{A}
$$

$$
\text{rank}(\mathbf{A}) = \text{trace}(\mathbf{A})= \text{trace}\left(  \mathbf{X } \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}  \mathbf{R}^\top  \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} \mathbf{R}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top \right) \\= \text{trace}\left(  \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} \mathbf{R}  \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{X}^\top  \mathbf{X } \left( \mathbf{X}^\top \mathbf{X}\right)^{-1}  \mathbf{R}^\top \right) = \text{trace}\left( I_{D_r}\right)
$$
so $\frac{(\mathbf{R} \mathbf{b} -\mathbf{r})^\top \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} (\mathbf{R} \mathbf{b} -\mathbf{r}) } { \sigma^2 } \sim \chi^2(D_r)$



Another proof:

Fact: Let $\mathbf{x}$ be an $m$ dimensional random vector. If $\mathbf{x} ∼ N(\boldsymbol{\mu}, \boldsymbol{\Sigma}) $ with $\boldsymbol{\Sigma}$ nonsingular, then $(\mathbf{x} -\boldsymbol{\mu})^\top \boldsymbol{\Sigma}^{-1}(\mathbf{x} -\boldsymbol{\mu}) \sim \chi^2(m)$.

Let $\mathbf{v} ≡ \mathbf{R} \mathbf{b} -\mathbf{r}$, under $H_0$, $\mathbf{R} \mathbf{b} -\mathbf{r}=\mathbf{R} (\mathbf{b} -\boldsymbol{\beta})$, 

$E(\mathbf{v}\,|\,\mathbf{X}) =\mathbf{0}$ and 

$ \text{ Var} ( \mathbf{v} \, |\, \mathbf{X}) =  \text{ Var}\left( \mathbf{R} (\mathbf{b} -\boldsymbol{\beta}) \, |\, \mathbf{X} \right)  = \mathbf{R} \text{ Var}\left(  \mathbf{b} -\boldsymbol{\beta} \, |\, \mathbf{X} \right) \mathbf{R}^\top = \sigma^2 \mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1} \mathbf{R}^\top $



2) $\frac{\mathbf{e}^\top\mathbf{e} }{ \sigma^2 } \sim \chi^2(n-K)$



3) $\frac{(\mathbf{R} \mathbf{b} -\mathbf{r})^\top \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} (\mathbf{R} \mathbf{b} -\mathbf{r}) } { \sigma^2 }$ is a function of $\mathbf{b} $ and $\frac{\mathbf{e}^\top\mathbf{e} }{ \sigma^2 }$ is a function of $\mathbf{e} $. But $\mathbf{b} $ and $\mathbf{e} $ are independently distributed conditional on $ \mathbf{X}$. So $\frac{(\mathbf{R} \mathbf{b} -\mathbf{r})^\top \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} (\mathbf{R} \mathbf{b} -\mathbf{r}) } { \sigma^2 } $ and $\frac{\mathbf{e}^\top\mathbf{e} }{ \sigma^2 }$ are independently distributed conditional on  $ \mathbf{X}$.



If the null hypothesis $\mathbf{R}\boldsymbol{\beta} =\mathbf{r}$ is true, we expect $\mathbf{R}\boldsymbol{\beta} -\mathbf{r}$ to be small, so large values of F should be taken as evidence for a failure of the null.

 This means that we look at only the upper tail of the distribution in the F-statistic (the alternative hypothesis is one-sided)



##### restricted least squares





$$
\underset{\hat{\boldsymbol{\beta}}}{\min} \; \frac{1}{2}\left( \mathbf{y}-\mathbf{X} \tilde{\mathbf{b}}\right)^\top\left( \mathbf{y}-\mathbf{X} \tilde{\mathbf{b}}\right) \quad \text{s.t.} \quad  \mathbf{R}  \tilde{\mathbf{b}} =\mathbf{r}
$$
Finding the $ \tilde{\mathbf{b}}$ that achieves this constrained minimization is called the restricted regression or restricted least squares. Analytically, we can show $F =\frac{ (SSR_R-SSR_U)/D_r}{SSR_U/(n-K)}$.




$$
\mathcal{L}=  \frac{1}{2} \left( \mathbf{y}-\mathbf{X} \tilde{\mathbf{b}}\right)^\top\left( \mathbf{y}-\mathbf{X} \tilde{\mathbf{b}} \right) + \boldsymbol{\lambda}^\top \left( \mathbf{R} \tilde{\mathbf{b}} -\mathbf{r} \right)
$$
The first order condition is
$$
- \mathbf{X}^\top \mathbf{y}+ \mathbf{X}^\top \mathbf{X} \tilde{\mathbf{b}}^c + \mathbf{R}^\top \boldsymbol{\lambda}= \mathbf{0} 
\\ \Longleftrightarrow \; - \mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{X}^\top \mathbf{y}+ \mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{X}^\top \mathbf{X} \tilde{\mathbf{b}}^c + \mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \boldsymbol{\lambda}= \mathbf{0}

\\ \Longleftrightarrow \;  \mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \boldsymbol{\lambda}= \mathbf{Rb} - \mathbf{R} \tilde{\mathbf{b}}^c

\\ \Longleftrightarrow \;  \mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \boldsymbol{\lambda}=   \mathbf{Rb} -   \mathbf{r}

\\ \Longleftrightarrow \;  \boldsymbol{\lambda}=  \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1} ( \mathbf{Rb} -   \mathbf{r})
$$
Subsititute the equation for $ \boldsymbol{\lambda}$ into the first order condition to solve for the constrained $ \tilde{\mathbf{b}}^c $:
$$
- \mathbf{X}^\top \mathbf{y}+ \mathbf{X}^\top \mathbf{X} \tilde{\mathbf{b}}^c + \mathbf{R}^\top \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1} ( \mathbf{Rb} -   \mathbf{r})= \mathbf{0}

\\ \Longleftrightarrow \;  - (\mathbf{X}^\top\mathbf{X})^{−1} \mathbf{X}^\top \mathbf{y}+(\mathbf{X}^\top\mathbf{X})^{−1}  \mathbf{X}^\top \mathbf{X} \tilde{\mathbf{b}}^c +(\mathbf{X}^\top\mathbf{X})^{−1}  \mathbf{R}^\top \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1} ( \mathbf{Rb} -   \mathbf{r})= \mathbf{0}

\\ \Longleftrightarrow \;  - \mathbf{b}+ \tilde{\mathbf{b}}^c +(\mathbf{X}^\top\mathbf{X})^{−1}  \mathbf{R}^\top \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1} ( \mathbf{Rb} -   \mathbf{r})= \mathbf{0}


\\ \Longleftrightarrow \;    \tilde{\mathbf{b}}^c = \mathbf{b} - (\mathbf{X}^\top\mathbf{X})^{−1}  \mathbf{R}^\top \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1} ( \mathbf{Rb} -   \mathbf{r})
$$




Let $\tilde{\mathbf{e}}= \mathbf{y}-\mathbf{X} \tilde{\mathbf{b}}$, the residuals from the restricted regression
$$
\tilde{\mathbf{e}}= \mathbf{y}-\mathbf{Xb} +\mathbf{X} (\mathbf{b} -\tilde{\mathbf{b}}^c)
$$

$$ {SSR_R}
\begin{align*}
SSR_R &= \left(  \mathbf{y}-\mathbf{Xb} +\mathbf{X} (\mathbf{b} -\tilde{\mathbf{b}}^c) \right)^\top  \left(  \mathbf{y}-\mathbf{Xb} +\mathbf{X} (\mathbf{b} -\tilde{\mathbf{b}}^c) \right)
\\&= \left(  \mathbf{y}-\mathbf{Xb}  \right)^\top \left(  \mathbf{y}-\mathbf{Xb}  \right)+ (\mathbf{b} -\tilde{\mathbf{b}}^c)^\top \mathbf{X} ^\top  \mathbf{X} (\mathbf{b} -\tilde{\mathbf{b}}^c) 
\\ &= SSR_U + \left( (\mathbf{X}^\top\mathbf{X})^{−1}  \mathbf{R}^\top \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1} ( \mathbf{Rb} -   \mathbf{r})\right)^\top \mathbf{X} ^\top    \mathbf{X} \left( (\mathbf{X}^\top\mathbf{X})^{−1}  \mathbf{R}^\top \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1} ( \mathbf{Rb} -   \mathbf{r})\right)
\\ &= SSR_U +  ( \mathbf{Rb} -   \mathbf{r})^\top  \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1}  ( \mathbf{Rb} -   \mathbf{r}) 



\end{align*}
$$

$$
SSR_R-SSR_U=  ( \mathbf{Rb} -   \mathbf{r})^\top  \left(\mathbf{R}(\mathbf{X}^\top\mathbf{X})^{−1}\mathbf{R}^\top \right)^{-1}  ( \mathbf{Rb} -   \mathbf{r})
$$
So
$$
F = \frac{ (\mathbf{R} \mathbf{b} -\mathbf{r})^\top \left[ \mathbf{R} \left( \mathbf{X}^\top \mathbf{X}\right)^{-1} \mathbf{R}^\top \right]^{-1} (\mathbf{R} \mathbf{b} -\mathbf{r}) /D_r }{s^2} =\frac{ (SSR_R-SSR_U)/D_r}{SSR_U/(n-K)}
$$
This derivation of the F-ratio is analogous to how the likelihoodratio statistic is derived in maximum likelihood estimation as the difference in log likelihood with and without the imposition of the null hypothesis. For this reason, this second derivation of the F-ratio is said to be by the **Likelihood-Ratio principle**.



t versus F 

Because hypotheses about individual coefficients are linear hypotheses, the t-test of $H_0 : β_k = \bar{ β}_k$ is a special case of the F-test

Since a random variable distributed as $F(1, n − K)$ is the square of a random variable distributed as $t(n − K)$, the t- and F-tests give the same test result.



![](https://raw.githubusercontent.com/justinjiajia/img/master/prob_stats/t_vs_F.PNG)

The F-test should be preferred to the test using two t-ratios for two reasons. First, if the size (significance level) in each of the two t-tests is α, then the overall size (the probability that (1, 0) is outside the rectangular region) is not α. Second, as will be noted in the next section (see (1.5.19)), the F-test is a likelihood ratio test and likelihood-ratio tests have certain desirable properties. So even if the significance level in each t-test is controlled so that the overall size is α, the test is less desirable than the F-tes





The Log Likelihood for the Regression Model



Assumption 1.5 (the normality assumption) together with Assumptions 1.2 and 1.4 imply that the distribution of $\boldsymbol{\epsilon}$ conditional on X is $N(\mathbf{0}, \sigma^2 \mathbf{I}_n)$

By assumption 1.1, we have 
$$
\mathbf{y} \, |\, \mathbf{X} \sim N(\mathbf{X} \boldsymbol{\beta}, \sigma^2 \mathbf{I}_n)
$$


Recall from basic probability theory that the density function for an n-variate normal distribution with mean µ and variance matrix 6 is
$$
\left(2 \pi  \right)^{-\frac{n}{2}} \vert \boldsymbol{\Sigma}\vert^{-\frac{1}{2}} \exp \left[-\frac{1}{2} \left( \mathbf{y} -   \boldsymbol{\mu} \right)^\top \boldsymbol{\Sigma} ^{-1} \left( \mathbf{y} -     \boldsymbol{\mu}   \right) \right]
$$
set  $\boldsymbol{\mu} = \mathbf{X} \boldsymbol{\beta}$ and $ \boldsymbol{\Sigma} = \sigma^2 \mathbf{I}_n$, the conditional density of $\mathbf{y}$ given $\mathbf{X}$ is
$$
f(\mathbf{y} \, |\, \mathbf{X}) = \left(2 \pi \sigma^2 \right)^{-\frac{n}{2}} \exp \left[-\frac{1}{2\sigma^2} \left( \mathbf{y} -  \mathbf{X}\boldsymbol{\beta} \right)^\top \left( \mathbf{y} -    \mathbf{X} \boldsymbol{\beta} \right) \right]
$$


Replacing the true parameters $(\boldsymbol{\beta} , \sigma^2)$ by their hypothetical values $(\tilde{\boldsymbol{\beta}} , \tilde{\sigma}^2)$ and taking logs, we obtain the log likelihood function:
$$
\log L(\tilde{\boldsymbol{\beta}} , \tilde{\sigma}^2) =  {-\frac{n}{2}} \log\left(2 \pi \right) - {\frac{n}{2}} \log\left(\tilde{\sigma}^2 \right)    -\frac{1}{2 \tilde{\sigma}^2} \left( \mathbf{y} -  \mathbf{X} \tilde{\boldsymbol{\beta} } \right)^\top \left( \mathbf{y} -    \mathbf{X} \tilde{\boldsymbol{\beta} } \right)
$$


The ML estimator $(\hat{\boldsymbol{\beta}} , \hat{\sigma}^2)$ of $(\boldsymbol{\beta} , \sigma^2)$ is the $(\tilde{\boldsymbol{\beta}} , \tilde{\sigma}^2)$ that maximizes this log likelihood.

Maximizing over $\tilde{\boldsymbol{\beta}}$ for any given $ \tilde{\sigma}^2$ produces $\hat{\boldsymbol{\beta}}$, which is just the OLS estimator $\mathbf{b}$.

Note: The $\tilde{\boldsymbol{\beta}}$  that maximizes the objective function could depend on $ \tilde{\sigma}^2$ but does not, in the present case of Assumptions 1.1–1.5.

Substituting $\hat{\boldsymbol{\beta}}$ for $\tilde{\boldsymbol{\beta}}$  and  maximize the **concentrated log likelihood function** over $ \tilde{\sigma}^2$ produces $ \hat{\sigma}^2$ (hayashi P49):
$$
\frac{\partial \log L(\tilde{\boldsymbol{\beta}} , \tilde{\sigma}^2)}{\partial \tilde{\sigma}^2} \vert_{\tilde{\boldsymbol{\beta}}=\hat{\boldsymbol{\beta}}} =  - {\frac{n}{2 \tilde{\sigma}^2}}     + \frac{\mathbf{e}^\top \mathbf{e}}{2 \left(\tilde{\sigma}^2 \right)^2} = 0 \\
\hat{\sigma}^2 =  \frac{\mathbf{e}^\top \mathbf{e}}{n}
$$
so that the maximized likelihood is 
$$
\log L(\tilde{\boldsymbol{\beta}} , \tilde{\sigma}^2) \vert_{\tilde{\boldsymbol{\beta}}=\mathbf{b}, \,  \tilde{\sigma}^2= \hat{\sigma}^2}=  {-\frac{n}{2}} \log\left(2 \pi \right) - {\frac{n}{2}} \log\left(  \frac{SSR}{n}\right)    - {\frac{n}{2}}
\\ \Longleftrightarrow 

 L(\tilde{\boldsymbol{\beta}} , \tilde{\sigma}^2) \vert_{\tilde{\boldsymbol{\beta}}=\mathbf{b}, \,  \tilde{\sigma}^2= \hat{\sigma}^2}=\left(\frac{2\pi}{n}\right)^{-\frac{n}{2}}\cdot  \exp \left(-\frac{n}{2}\right) \cdot \left(SSR \right)^{-\frac{n}{2}}
$$


The restricted likelihood $L_R$ is given by replacing this $SSR$ by the restricted sum of squared residuals, $SSR_R$:
$$
L_R =\left(\frac{2\pi}{n}\right)^{-\frac{n}{2}}\cdot  \exp \left(-\frac{n}{2}\right) \cdot \left(SSR_R \right)^{-\frac{n}{2}}
$$


The **likelihood ratio test** of the null hypothesis compares $L_U$ , the maximized likelihood without the imposition of the restriction specified in the null hypothesis, with $L_R$, the likelihood maximized subject to the restriction.

The likelihood ratio is
$$
\lambda= \frac{L_U}{L_R} = \left(\frac{SSR_U}{SSR_R }\right)^{-\frac{n}{2}}
$$
So we have
$$
F = \frac{n-K} {D_r}\left( \lambda^{2/n} -1\right)
$$


All these results assume the normality of the error term. Without normality, there is no guarantee that the ML estimator of β is OLS (Proposition 1.5) or that the OLS estimator b achieves the Cramer-Rao bound (Proposition 1.6).



### Generalized Least Squares (GLS)

Relax Assumption 1.4
$$
\text{E} (\boldsymbol{\epsilon}\boldsymbol{\epsilon}^\top\, | \, \mathbf{X}) = \sigma^2 \mathbf{V}(\mathbf{X})
$$
where $\mathbf{V}(\mathbf{X})$ is nonsingular and known (by construction symmetric and positive definite)



For economy of notation, we use $\mathbf{V}$ for the value $\mathbf{V}(\mathbf{X})$.

Because of symmetry and positive definiteness, $\mathbf{V} $ can be decomposed to 
$$
\mathbf{P}\boldsymbol{\Lambda} \mathbf{P}^\top
$$
where $\mathbf{P}$ is an orthonormal matrix

There exists a nonsingular $n\times n$ matrix $\mathbf{C}$ such that
$$
\mathbf{V}^{-1} = \mathbf{P}\boldsymbol{\Lambda}^{-1} \mathbf{P}^\top = \mathbf{C}^\top\mathbf{C}
$$
This decomposition is not unique, with more than one choice for $\mathbf{C}$, but, the choice of $\mathbf{C}$ doesn’t matter.

consider creating a new regression model by transforming $(\mathbf{y}, \mathbf{X}, \boldsymbol{\epsilon})$ by $\mathbf{C}$ as
$$
\tilde{\mathbf{y}} = \mathbf{C} \mathbf{y} \\

\tilde{\mathbf{X}} = \mathbf{C} \mathbf{X}\\
\tilde{\boldsymbol{\epsilon}} = \mathbf{C} \boldsymbol{\epsilon}
$$
The transformed model satisfies the assumptions of the classical linear regression model: 

1. Linearity is satisfied.

2. Strict exogeneity is also satisfied because
   $$
   E( \tilde{\boldsymbol{\epsilon}} \,|\,  \tilde{\mathbf{X}}) = E( \tilde{\boldsymbol{\epsilon}} \,|\,  \mathbf{X}) = E(  \mathbf{C} \boldsymbol{\epsilon} \,|\,  \mathbf{X}) = \mathbf{C} E(   \boldsymbol{\epsilon} \,|\,  \mathbf{X}) =\mathbf{0}
   $$
   (since $\mathbf{C} $ is nonsingular, $ \mathbf{X}$ and $ \tilde{\mathbf{X}}$ contain the same information)

3. Because $\mathbf{V} $ is positive definite, the no-multicollinearity assumption is also satisfied

4. Assumption 1.4 is satisfied for the transformed model because
   $$
   \text{E} ( \tilde{\boldsymbol{\epsilon}}\tilde{\boldsymbol{\epsilon}}^\top\, | \,  \tilde{\mathbf{X}}) =\text{E} (\tilde{\boldsymbol{\epsilon}} \tilde{\boldsymbol{\epsilon}}^\top\, | \, \mathbf{X}) =\text{E} (  \mathbf{C} \boldsymbol{\epsilon} \boldsymbol{\epsilon}^\top  \mathbf{C}^\top \, | \, \mathbf{X}) =\sigma^2   \mathbf{C}  \mathbf{V} \mathbf{C}^\top 
   $$
   because $\mathbf{C}  \mathbf{V} \mathbf{C}^\top$ is nonsingular 
   $$
   \left(\mathbf{C}  \mathbf{V} \mathbf{C}^\top\right)^{-1}= \left(\mathbf{C}^\top\right)^{-1}  \mathbf{V} ^{-1} \mathbf{C}^{-1}   =  \left(\mathbf{C}^\top\right)^{-1} \mathbf{C}^\top\mathbf{C} \mathbf{C}^{-1} =\mathbf{I}_n
   $$

5. Finally, $ \tilde{\boldsymbol{\epsilon}} \,|\,  \tilde{\mathbf{X}}$ is normal because the distribution of $ \tilde{\boldsymbol{\epsilon}} \,|\,  \tilde{\mathbf{X}}$   is the same as $ \tilde{\boldsymbol{\epsilon}} \,|\,  \mathbf{X}$ and $ \tilde{\boldsymbol{\epsilon}}$ is a linear transformation of $ \boldsymbol{\epsilon}$.



The Gauss-Markov Theorem for the transformed model implies that the BLUE of $ \boldsymbol{\beta}$ for the generalized regression model is the OLS estimator applied to $\tilde{\mathbf{y}} = \tilde{\mathbf{X}}  \boldsymbol{\beta} +\tilde{\boldsymbol{\epsilon}}$:
$$
\mathbf{b}_{\text{GLS}}  = \left(\tilde{\mathbf{X}}^\top\tilde{\mathbf{X}}\right)^{-1}\tilde{\mathbf{X}}^\top\tilde{\mathbf{y}}
=\left( \mathbf{X}^\top\mathbf{V}^{-1} \mathbf{X}\right)^{-1}\mathbf{X} ^\top \mathbf{V}^{-1}\mathbf{y}
$$
This is the GLS estimator. Its conditional variance is
$$
\text{Var}(\mathbf{b}_{\text{GLS}} \, |\, \mathbf{X}) = 
\left( \mathbf{X}^\top\mathbf{V}^{-1} \mathbf{X}\right)^{-1}\mathbf{X} ^\top \mathbf{V}^{-1} \text{Var}(\mathbf{y}\, |\, \mathbf{X}) \mathbf{V}^{-1} \mathbf{X}  \left( \mathbf{X}^\top\mathbf{V}^{-1} \mathbf{X}\right)^{-1}
\\=
\left( \mathbf{X}^\top\mathbf{V}^{-1} \mathbf{X}\right)^{-1}\mathbf{X} ^\top \mathbf{V}^{-1} \sigma^2\mathbf{V}  \mathbf{V}^{-1} \mathbf{X}  \left( \mathbf{X}^\top\mathbf{V}^{-1} \mathbf{X}\right)^{-1}
= \sigma^2\left( \mathbf{X}^\top\mathbf{V}^{-1} \mathbf{X}\right)^{-1}
$$
the OLS estimator $\mathbf{b}$ is unbiased without Assumption 1.4, but nevertheless the GLS estimator $\mathbf{b}_{\text{GLS}}$ should be preferred (provided $ \mathbf{V}$ is known) because the latter is more efficient in that the variance is smaller in the matrix sense. The gain in efficiency is achieved by exploiting the heteroskedasticity and correlation between observations in the error term



Proposition 1.7 (finite-sample properties of GLS): 

- (a) (unbiasedness) Under Assumption 1.1–1.3, E(bβGLS | X) = β.
- (b) (expression for the variance) Under Assumptions 1.1–1.3 and the assumption $\text{E} (\boldsymbol{\epsilon}\boldsymbol{\epsilon}^\top\, | \, \mathbf{X}) = \sigma^2 \mathbf{V}(\mathbf{X})$ ($\mathbf{V}(\mathbf{X})$ is nonsingular and known ) that the conditional second moment is proportional to $\mathbf{V}(\mathbf{X})$,  $\text{Var}(\mathbf{b}_{\text{GLS}} \, |\, \mathbf{X}) = \sigma^2\left( \mathbf{X}^\top\mathbf{V}^{-1} \mathbf{X}\right)^{-1}$
- (c) (efficiency of GLS) Under the same set of assumptions as in (b), the GLS estimator is efficient in that the conditional variance of any unbiased estimator that is linear in y is greater than or equal to $\text{Var}(\mathbf{b}_{\text{GLS}} \, |\, \mathbf{X}) $ in the matrix sense.



 

### Laws of Large Numbers and Central Limit Theorems





##### Lemma 2.2 (relationship among the four modes of convergence):

1. $\mathbf{z}_n  \to_{\text{m.s.}} \boldsymbol{\alpha}  \Rightarrow \mathbf{z}_n  \to_{\text{p}} \boldsymbol{\alpha}$. So $\mathbf{z}_n  \to_{\text{m.s.}} \mathbf{z}  \Rightarrow \mathbf{z}_n  \to_{\text{p}} \mathbf{z}$.
2. $\mathbf{z}_n  \to_{\text{a.s.}} \boldsymbol{\alpha}  \Rightarrow \mathbf{z}_n  \to_{\text{p}} \boldsymbol{\alpha}$. So $\mathbf{z}_n  \to_{\text{a.s.}} \mathbf{z}  \Rightarrow \mathbf{z}_n  \to_{\text{p}} \mathbf{z}$.
3. $\mathbf{z}_n  \to_{\text{d}} \boldsymbol{\alpha} \Leftrightarrow \mathbf{z}_n  \to_{\text{p}} \boldsymbol{\alpha}   $. It means that the limiting random variable is a constant [a trivial random variable], convergence in distn'bution is the same as convergence in probability.



By convention, if the limiting random variable is a constant, use the notation $ \to_{\text{p}}$; otherwise, use $ \to_{\text{d}}  $



##### Lemma 2.3 (preservation of convergence for continuous transformation):

Suppose $\mathbf{a}(\cdot)$ is a vector-valued continuous function that does not depend on $n$. 

- (a) $\mathbf{z}_n  \to_{\text{p}} \boldsymbol{\alpha} \Rightarrow \mathbf{a}( \mathbf{z}_n)  \to_{\text{p}} \mathbf{a}( \boldsymbol{\alpha}) $. Statedifferently, $\text{plim}_{n\to \infty} \,  \mathbf{a}(\mathbf{z}_n) = \mathbf{a}(\text{plim}_{n\to \infty} \, \mathbf{z}_n)$ provided the $\text{plim}$ exists.
-  (b) $\mathbf{z}_n  \to_{\text{d}}\mathbf{z}\Rightarrow \mathbf{a}( \mathbf{z}_n)  \to_{\text{d}} \mathbf{a}( \mathbf{z}) $.



For a sequence of random scalars $\{z_i\}$, the sample mean $\bar{z}_n$ is defined as  $\bar{z}_n \equiv \frac{1}{n} \sum_{i=1}^n z_i$ . Consider the sequence  $\{\bar{z}_n\}$

###### A Version of Chebychev's Weak LLN:]

$$
\lim_{n\to \infty}\mathbb{E}(\bar{z}_n) =\mu,  \lim_{n\to \infty} \text{Var}(\bar{z}_n) =0 \Longrightarrow   \bar{z}_n\to_{\text{p}} \mu
$$

It can be shown that $\bar{z}_n\to_{\text{m.s.}} \mu$. So we have $\bar{z}_n\to_{\text{p}} \mu$.

###### Kolmogorov's Second Strong Law of Large Numbers

Let $\{z_i\}$ be IID with $\text{E}({z}_i) = \mu$ (it implies the mean exists and is finite). Then $\bar{z}_n\to_{\text{a.s.}} \mu$. 

In this theorm, the variance does not need to be finite.

These LLNs extend readily to random vectors by requiring element-by-element convergence.

###### Lindeberg-Levy CLT: 

Let $\{\mathbf{z}_i\}$ be IID with $\text{E}(\mathbf{z}_i) = \boldsymbol{\mu}$ and $ \text{Var}(\mathbf{z}_i)=\boldsymbol{\Sigma}$ . Then 
$$
\sqrt{n} (\bar{\mathbf{z}}_n -\boldsymbol{\mu}) \to_{\text{d}} N(\mathbf{0}, \boldsymbol{\Sigma})
$$


###### Ergodic Theorem: (See, e.g., Theorem 9.5.5 of Karlin and Taylor (1975).) 

Let $\{\mathbf{z}_i\}$  be a stationary and ergodic process with $\text{E}(\mathbf{z}_i) = \boldsymbol{\mu}$. Then $\bar{\mathbf{z}}_n \to_{\text{a.s.}} \boldsymbol{\mu}$

The Ergodic Theorem, therefore, is a substantial generalization of Kolmogorov's LLN. Serial dependence, which is ruled out by the i.i.d. assumption in Kolmogorov's LLN, is allowed in the Ergodic Theorem, provided that it disappears in the long run.

