### Why measure？ 

(1) extension of lengths of intervals.  

already in 19th century：Peano, Jordan, Cantor, Borel (only finite additivity)  

(2) H. Lebesgue (1875-1941), a pupil of Borel.  

- Lebesgue measure: countable additivity 

(3) measure the set of discontinuities or the extent of integrability.  (Lebesgue, 1904) 

$f$ bdd on , Riemann int egrable [ , ]: discontinuous at x. =0 a b Then f m x a b f   (a) Lebesgue integral  range of function [c,d] divided into subintervals  (b) Riemann integral  domain [a,b] divided into subintervals  Advantage of Lebesgue integral over Riemann integral:  (1) more applicability:  f Riemann int egrable    Lebesgue integrable 





 3 Ex. Dirichlet function: 1 if rational ( ) on [0,1] 0 if irrational x f x x     Then f not Riemann integrable, but Lebesgue integrable.  1 0  f x dx () 0  (2) more unified:  (i) domain of integrand:  Riemann integral: finite interval (proper), infinite interval (improper of typeⅠ)  Lebesgue integral: Lebesgue measurable set E E f ( ) x dx (ii) range of integrand： Riemann integral：bdd integrand (proper)  unbdd integrand (improper of typeⅡ)  Lebesgue integral: arbitrary  (3) simplicity in application:  (i) Convergence:        n n n (a) pointwise on , , M n on , , Riemann integrable on , lim (Arzela's Thm) b b a a n f f ab f ab f f ab f f              (b) pointwise on , n on , Lebesgue integrable on , Lebesgue integrable & lim  (Lebesgues' bdd convergence thm) n n n b b a a n f f ab f M ab f ab f ff        Note: 1. Proof of (a) is difficult.  2. f must be assumed integrable.   1 2  1 1 0 Ex. Let , ,... rational no's in [0,1] 1 if ,... Let ( ) on [0,1] 0 otherwise Then ( ) 0 n 1 if rational But ( )= on [0,1] 0 otherwise not Riema n n n r r xr r f x f x dx x f x            nn integrable 實變函數論─應用數學系 吳培元老師 4 (ii) Fundamental thm. of calculus:  1. Riemann integral:      0 0 00 (a) Riemann integrable on , . ( ) ( ) for , Then conti. at x ( , ) ( ) exists & ( )= ( ). x a f ab F x f t dt x a b f ab F x F x f x        (b) g exists on , & g Riemann integrable on , .     Then ( ) ( ) - ( ). b a ab ab g x dx g b g a      2. Lebesgue integral:        (a) Lebesgue integrable on , . ( ) ( ) for , Then ( )= ( ) a.e. on , . (b) g exists & bdd on [a,b] Then g Lebesgue integrable & ( ) ( ) ( ) (cf. p.78, Ex.2.14.7) x a b a f ab F x f t dt x a b F x f x ab g x dx g b g a          



Note： $f$ bdd measurable on X, ( ) integrable on X i.e., ( ) ( ) uX f L X LX   



Conclusion

- Riemann integral essentially for conti. function.  

- Lebesgue integral for more general function 





Another use:  Probability theory based on real analysis:  

Ex. Toss a die,  $ \# \{events\} =  2^6$

- $\sigma$ -algebra $\longleftrightarrow$ $\{events\}$
- probability $\longleftrightarrow$ measure 
- measurable function $\longleftrightarrow$  random variable $  \begin{cases} \text{discrete r.v.}\\ \text{(unif.) continuous r.v.}\end{cases} $
- Radon-Nikodym derivative $\longleftrightarrow$ density function



### Chap.1. Measure theory  

A set $X$, a sequence subsets of $X$ , $\{ E_n\}$ 

Def. 

$ \varlimsup_{n \to \infty} E_n = \{x\in X \colon \;x \text{ belongs to infinitely many } E_n \text{'s} \} = \bigcap_{k=1}^{\infty} \bigcup_{n=k}^{\infty} E_n$  

$ \varliminf_{n \to \infty} E_n = \{x\in X \colon \;x \text{ belongs to all but finitely many } E_n \text{'s} \}= \bigcup_{k=1}^{\infty} \bigcap_{n=k}^{\infty} E_n$  

Note: $\varliminf_{n \to \infty} E_n \subseteq \varlimsup_{n \to \infty} E_n$ 



Def. 

$\{ E_n\}$ has a limit if $\varliminf_{n \to \infty} E_n  = \varlimsup_{n \to \infty} E_n $ . In this case, denote this set by $\lim_{n \to \infty} E_n $



Ex. 

$ E_n= \begin{cases} A \quad \text{if } n \text{ is even}  \\ B \quad \text{if } n \text{ is odd}\end{cases} $

 Then $\varlimsup_{n \to \infty} E_n = A\cup B$,  $\varliminf_{n \to \infty} E_n = A \cap B$.  $\lim_{n \to \infty} E_n $  exists $\Longleftrightarrow A=B$.

 Analogue: $a, \;b,\;a,\;b,\;\dots$ . Then limit exists $\Longleftrightarrow a=b$.





Def.

measure space $(X,\mathfrak{A} \normalsize, \mu)$ and $X_0 \in \mathfrak{A} $

$f\colon X_0\to \mathbb{R}$  measurable if $\forall$ open $ M \subseteq \mathbb{R} $, $f^{-1}(M) \in \mathfrak{A}$ & $f^{-1}(\{+\infty\}) \in \mathfrak{A}$ & $f^{-1}(\{-\infty\}) \in \mathfrak{A}$

Note. In probability, meansurable function $=$ random variable







Note: $f,g\colon \; X \to [-\infty, \infty]$, $\mu$ complete measure,  $f=g$ a.e. & $f$ measurable  $\Rightarrow$ $g$ measurable



Thm. $\{f_n\}$ measurable

- $f_n \rightarrow g$ (pointwise) $\Rightarrow $ $g$ measurable.
- $f_n \rightarrow g$ (pointwise) a.e. & $\mu$ complete $\Rightarrow $ $g$ measurable.



Thm. $f \geq 0$ measurable  $\Rightarrow $     $\exists \{f_n\}$   such that  $f_n$ simple and $0 \leq f_n \uparrow f$.

Cor. $f$ measurable   $\Rightarrow $ $\exists \{f_n\}$  such that  $f_n$ simple and $f_n \rightarrow f$.





Convergence



Def. Almost Uniform Convergence

$\{f_n\}$ measurable on $(X,\mathfrak{A} \normalsize, \mu)$, real-valued a.e., $f$ measurable

$f_n \rightarrow f$ ***almost uniformly*** if $\forall \varepsilon > 0$, $\exists E$ such that $\mu(E)<\varepsilon$ & $f_n \rightarrow f$ ***uniformly*** on $X\setminus E$ 

- i.e., $\sup_{x\in X\setminus E} \vert f_n(x)-f(x)\vert \rightarrow 0$ as $n\rightarrow \infty$

Note:

- this definition concerns measurable functions.

- if $f_n \rightarrow f$ ***almost uniformly***, $f$ is real-valued a.e. 







**Theorem.**

$\{f_n\}$ measurable, real-valued a.e., $f$ measurable

If $f_n \rightarrow f$ almost uniformly $\Rightarrow f_n \rightarrow f$ a.e.



**Theorem. (Egoroff's Theorem)**   

- $\mu(X)<\infty$

- $\{f_n\}$ measurable, real-valued a.e.

- $f$ measurable, real-valued a.e.

-  $f_n \rightarrow  f $ a.e.

$\Rightarrow $ $f_n \rightarrow  f $ almost uniformly

 

Ex.

$f_n=x^n $ on $[0,1]$

Then $f_n \rightarrow f = \begin{cases} 0 \quad  0\leq x<1 \\1 \quad x=1 \end{cases}$

Since $\sup_{x\in [0,1]} \vert f_n(x)-f(x)\vert \rightarrow 1$ as $n\rightarrow \infty$, $f_n$ is not uniformly convergent to $f$.

But $f_n \rightarrow  f $ almost uniformly, as $f_n \rightarrow  f $ uniformly  on $[0, 1-\varepsilon]$  for any $0<\varepsilon <1$.



<img src="https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/f_convergence.png" width=450/>



Ex. 

$X$ Lebesgue measure space on the real line and let $f_n$ be the characteristic function of $(n, \infty)$.

 $f_n \rightarrow  0 $ a.e. but not almost uniformly





Def. Convergence in measure

$\{f_n\}$ measurable on $(X,\mathfrak{A} \normalsize, \mu)$, real-valued a.e., $f$ measurable

$f_n \rightarrow f$ ***in measure*** if $\forall \varepsilon > 0$, $\lim_{n \to \infty} \mu \left(\{x\colon\vert f_n(x)-f(x)\vert \geq \varepsilon \}\right)=0$

- i.e., $\forall \varepsilon, \forall \delta$, $\exists N$ such that $n\geq N$ $\Rightarrow \mu\left(\{x\colon\vert f_n(x)-f(x)\vert \geq \varepsilon \}\right)<\delta$

Note:

- This definition concerns measurable functions.
- if $f_n \rightarrow f$ in measure and $f_n \rightarrow g$ in measure, then $f=g$ a.e.
- if $f_n \rightarrow f$ in measure, $f$ is a.e. real-valued



**Theorem.**

$\{f_n\}$ measurable, real-valued a.e., $f$ measurable

If $f_n \rightarrow f$ almost uniformly $\Rightarrow f_n \rightarrow f$ in measure



**Corollary**

- $\mu(X)<\infty$

- $\{f_n\}$ measurable,  a.e. real-valued

- $f$ measurable, a.e. real-valued

- $f_n \rightarrow  f $ a.e.

$\Rightarrow $ $f_n \rightarrow  f $ in measure

Note:  In general, $f_n \rightarrow  f $ a.e. $\nRightarrow $  $f_n \rightarrow  f $ in measure in measure. 

Ex. 

$X$ Lebesgue measure space on the real line and $f_n=\chi_{(n, \infty)} , \; f\equiv 0$.

 $f_n \rightarrow  f $ a.e. but not in measure



Def. Cauchy in measure

$\{f_n\}$ measurable on $(X,\mathfrak{A} \normalsize, \mu)$, a.e. real-valued

$f_n$ ***Cauchy in measure*** if $\forall \varepsilon > 0$, $\mu \left(\{x\colon\vert f_n(x)-f_m(x)\vert \geq \varepsilon \}\right) \rightarrow 0$ as $m,n \rightarrow \infty$

- i.e., $\forall \varepsilon, \forall \delta$, $\exists N$ such that $m,n\geq N$ $\Rightarrow \mu\left(\{x\colon\vert f_n(x)-f_m(x)\vert \geq \varepsilon \}\right)<\delta$

Note: $f_n \rightarrow f$ ***in measure***  $ \Rightarrow f_n$ ***Cauchy in measure*** 



Theorem

$\{f_n\}$ measurable, a.e. real-valued, Cauchy in measure

$\Rightarrow \exists $ measurable $f$  and $ \{f_{n^k}\}$  such that  $f_{n^k} \rightarrow f$ almost uniformly

Note: 

- implication: $f_n \rightarrow f$ ***in measure***  $ \Rightarrow$  $f_{n^k} \rightarrow f$ almost uniformly



**Corollary**

$\{f_n\}$ measurable, a.e. real-valued, Cauchy in measure

$\Rightarrow \exists $ measurable $f$  such that  $f_n \to f$ ***in measure*** 





**Simple Function**

$(X,\mathfrak{A} \normalsize, \mu)$

$f$ is a simple function if $f=\sum_{i=1}^{n} \alpha_i \chi_{E_i} $ such that  $\{E_i\} \subseteq \mathfrak{A}$, a partition of $X$, and $\alpha_i \in \mathbb{R} $ (not necessarily distinct)



Denote the sum $\sum_{i=1}^{n} \alpha_i \mu({E_i}) $ by $\int f(x)d\mu(x)$ or $\int fd\mu$, and call this sum the integral of the simple function $f$



Def.

A simple function is integrable 

- if $\alpha_i \neq 0 \Rightarrow \mu(E_i)<\infty$ or
- $\mu(E_i)<\infty \quad \forall i$ with $\alpha_1 \neq 0$

Note: the definition of the integral is independent of the representation of $f$

- i.e., $\sum_{i=1}^{n} \alpha_i \chi_{E_i}=\sum_{j=1}^{m} \beta_j \chi_{F_j} \Rightarrow \sum_{i=1}^{n} \alpha_i \mu({E_i})=\sum_{j=1}^{m} \beta_j \mu({F_j}) $





Def. Cauchy in the mean 

$\{f_n\}$ integrable simple functions

$\{f_n\}$ ***Cauchy in the mean*** if $\int \vert f_n - f_m \vert d\mu \to 0$ as $n,m \rightarrow \infty$

- i.e., $\forall \varepsilon>0, \exists N $ such that $ m,n \geq N \Rightarrow \int \vert f_n - f_m \vert d\mu < \varepsilon $



Lemma

$\{f_n\}$ integrable simple functions, Cauchy in the mean $\Rightarrow \exists f$ a.e. real-valued, measurable such that $f_n \to f$  in measure.





Def.

 $(X,\mathfrak{A} \normalsize, \mu)$

$f\colon X \to [-\infty, \infty]$, measurable

$f $ is ***integrable*** if $\exists \{f_n\}$ integrable simple functions such that

- (a) $\{f_n\}$ Cauchy in the mean

- (b) $\lim_{n\to \infty} f_n = f$ a.e. or $f_n\to f$ in measure



Then the integral of $f$, $\int fd\mu $, is defined as $\lim_{n\to \infty} \int f_n d\mu$

Note:

-  $\lim_{n\to \infty} \int f_n d\mu$ exists, because $\vert \int f_md\mu - \int f_n d\mu\vert \leq \int \vert f_m-f_n\vert d\mu \to 0$ as $m,n \to \infty$
- $\mu(X)$ may be infinite & $f$ may be ***unbounded***. Therefore, the definition encompasses improper integrals.
- both $\lim_{n\to \infty} f_n = f$ a.e. and $f_n\to f$ in measure do not imply $ \int f_n d\mu \to \int f d\mu$
- if $f$ integrable, $f$ is a.e. real-valued



Theorem

When $f $ is integrable,

 $\int fd\mu =  \lim_{n\to \infty} \int f_n d\mu$ is independent of the choice of integrable simple $\{f_n\}$ satisfying (a) & (b)



Note: $\forall E\in \mathfrak{A} $, $f$ integrable $\Rightarrow \chi_Ef$ integrable

Def. $\int_Efd\mu =\int\chi_Efd\mu$ if $E\in \mathfrak{A}$ and $f$ integrable





Def

$\{f_n\}$ integrable

$\{f_n\}$ Cauchy in the mean if $\int \vert f_n- f_m\vert d\mu \to 0$ as $n,m \to \infty$



Def. 

$\{f_n\}$ and $f$ integrable

$f_n\to f$ in the mean if $\int \vert f_n- f\vert d\mu \to 0$ as $n \to \infty$



Theorem

$\{f_n\}$ and $f$ integrable 

$f_n\to f$ in the mean $\Rightarrow f_n\to f$ in measure



Ex. 

in measure or almost uniformly or a.e. $\nRightarrow$ in the mean.   

$f_n(x)=n\chi_{\left[1-\frac{1}{n},1\right]}$ on $\left[0,1\right]$ and $f(x)\equiv 0$ on $\left[0,1\right]$ 

$f_n(x) \to f$ a.e., almost uniformly, and in measure, but not in the mean





Ex. 

in the mean  $\nRightarrow$ almost uniformly or a.e.

notes: real-analysis-class18.





Lemma

 $f$ integrable

$\{f_n\}$ is a sequence of integrable simple functions satisfying 

- (a) $\{f_n\}$ Cauchy in the mean
- (b) $\lim_{n\to \infty} f_n = f$ a.e. (or $f_n\to f$ in measure)

$\Rightarrow$ $f_n\to f$ in the mean 

Note:

- $f_n\to f$ a.e. or in measure $\nRightarrow$ $f_n\to f$ in the mean. 

- By definition of an integrable function,  $\int   f_n d\mu- \int f d\mu \to 0$ ; now stronger: $\int \vert f_n- f \vert d\mu \to 0$





Thm 2. 

$\{f_n\}$  integrable, Cauchy in the mean, & $f_n\to f$ a.e. (or in measure)

$\Rightarrow$  $f$ integrable and $\int f d\mu = \lim_{n\to \infty} \int f_n d\mu$





Theorem

$\{f_n\}$  integrable, Cauchy in the mean

$\Rightarrow \exists f$ integrable such that $f_n\to f$ in the mean





**Dominated Convergence theorem**

- $\{f_n\}, g$  integrable

- $f_n$ converges either a.e. or in measure to measurable $f$

- $\vert f_n(x)\vert \leq g(x)$  a.e.

$\Rightarrow  $ $ f$ integrable and $f_n\to f$ in the mean



Ex.

$f_n(x)=\begin{cases} \frac{1}{x} \quad x\in \left[\frac{1}{n}, 1\right] \\ 0 \quad \; x\in \left[0,\frac{1}{n}\right)\end{cases}$ and $f(x)= \begin{cases} \frac{1}{x} \quad \text{if } \frac{1}{n}\leq x\leq 1 \\ 0 \; \quad \text{if } x=0\end{cases} $ 

$\{f_n\}$ integrable, $f_n\to f$ a.e. and in measure

But $f$ is not integrable ($\int_0^1 f(x) dx = \infty$)



Ex.

$f_n(x)=n\chi_{\left[1-\frac{1}{n},1\right]}$ on $\left[0,1\right]$ and $f(x)\equiv 0$ on $\left[0,1\right]$ 

$f_n(x) \to f$ a.e., almost uniformly, and in measure

But $\int f_n d\mu \nrightarrow \int f d\mu$ as $\int f_n d\mu =1$ while $\int f d\mu =0$





Applications of DCT. (easier to apply than def. of ). 



**Theorem** 

$f$ measuarable, $g$ integrable

$\vert f \vert \leq g$ a.e. 

$\Rightarrow f$ integrable

Note: this is false for Riemann integrals: 

E.g., $f(x)=\begin{cases} 1 \quad \text{if } x \text{ rational} \\ 0 \quad   \text{if } x \text{ irrational} \end{cases}$  on $\left[0,1\right]$

Then $\vert f(x)\vert \leq 1$  on $\left[0,1\right]$ But its not Riemann integrable



Corollary

$f$ integrable; $g$ measurable, essentially bounded

 $\Rightarrow fg$ integrable



Corollary

$f$ measurable, essentially bounded on $E$ with finite measure

$\Rightarrow f$ integrable over $E$



**Monotone Convergence Theorem**

 monotone increasing $\{ f_n \} $  nonnegative integrable. Let $f=\lim_{n\to \infty} f_n$

$\Rightarrow \int fd\mu = \lim_{n\to \infty} \int f_nd\mu$

Note: In general, $f$ may not be integrable

- $f_n(x)=\begin{cases} \frac{1}{x} \quad x\in \left[\frac{1}{n}, 1\right] \\ 0 \quad \; x\in \left[0,\frac{1}{n}\right)\end{cases}$ and $f(x)= \begin{cases} \frac{1}{x} \quad \text{if } \frac{1}{n}\leq x\leq 1 \\ 0 \; \quad \text{if } x=0\end{cases} $ 



**Fatou's Lemma**

 $\{ f_n \} $  nonnegative integrable

$\Rightarrow \int \left(\varliminf_{n \to \infty}f_n \right)d\mu \leq \varliminf_{n \to \infty} \left( \int f_n d\mu \right)$

In particular, if $ \varliminf_{n \to \infty} \left( \int f_n d\mu \right) < \infty$, $\varliminf_{n \to \infty}f_n$ is integrable





**Definition. Absolute Continuity**

$\mu,\upsilon$ signed measures in a measure space $(X,\mathscr{A} \normalsize)$ 

$\upsilon \ll \mu$ if $\vert \mu\vert(E)=0 $  for some $E\in \mathscr{A}$ $\Rightarrow  \upsilon(E) =0$





Theorem

 $\mu$ is a signed measure

 $\upsilon$ a signed measure such that $\vert \mu\vert(E)<\infty$ for some $E \in  \mathscr{A} \Rightarrow  \upsilon(E) <\infty$

$\upsilon \ll \mu \Longleftrightarrow$ $\forall \varepsilon, \; \exists \delta $ such that $\vert \mu\vert(E)<\delta$ for some $E\in \mathscr{A} \Rightarrow \vert \upsilon\vert(E)<\varepsilon$







**Radon-Nikodym Theorem**

$\mu,\upsilon$  $\sigma$-finite signed measures in a measure space $(X,\mathscr{A} \normalsize)$ 

 $\upsilon\ll\mu$ (i.e., $\vert \mu\vert(E)=0 $  for some $E\in \mathfrak{A}$ $\Rightarrow  \upsilon(E) =0$)

$\Longrightarrow$

- $\exists $ measurable $f$ on $X$ such that $\upsilon(E)=\int_E f d\mu \; \forall E \in \mathfrak{A}$  for which $\vert \upsilon\vert(E)<\infty$
- $f$ is unique a.e. w.r.t. $\mu$

Note:

- if $\upsilon$ finite signed measure (i.e., $\vert \upsilon\vert(X)<\infty$) &  $\upsilon\ll\mu$, then $\upsilon(X)=\int f d\mu <\infty$, which means $f$ is integrable w.r.t. $\mu$. 

- if $\mu$ not $\sigma$-finite, R-N theorem does not hold. 

  - Ex. $\left[0,1\right]$, $\mathfrak{A} = \{ $Lebesgue measurable sets$\}$, $\mu = $ counting measure, $\upsilon =$ Lebesgue measure

    Then  $\mu$ not $\sigma$-finite (since $[0,1]$ uncountable), $\upsilon$ finite measure

    Suppose that  $\exists $ measurable $f$ on $[0,1]$ such that $\upsilon(E)=\int_E f d\mu \; \forall E \in \mathfrak{A}$ such that $\vert \upsilon\vert(E)<\infty$

    Let $E=\{x\}$, then $ \upsilon(\{x\})=\int_{\{x\}} f d\mu =f(x)$.

    Because $ \upsilon(\{x\})=0 \; \forall x\in [0,1]$,  $f(x) \equiv0 $ and thereby $\upsilon \equiv 0$. A contradiction. 







**Fundamental Theorem of Calculus on $\mathbb{R}$**

$f$ Lebesgue integrable on $\left[a,b\right]$

Let $g(x)=\int_a^xf(t)dt+c$ for $x \in[a,b]$ (indefinite integral of $f$)

$\Longrightarrow$

$g'(x)$ exists a.e. (or $g(x)$ differentiable) & $g'(x)=f(x)$ a.e.

Note:

- $g'(x)=\frac{1}{h} \lim_{ h\to 0} \int_x^{x+h} f(t)dt  $
- $f$ Riemann integrable on $[a,b]$ and is continuous at $x_0\in (a,b) \Rightarrow g'(x_0)$ exists & $g'(x_0)=f(x_0)$



Ex.

 $f(x)=\begin{cases} 1 \quad \text{if } x \text{ rational} \\ 0 \quad   \text{if } x \text{ irrational} \end{cases}$  on $\left[0,1\right]$  

Then the Fundamental Theorem of Calculus for Riemann integrals is not applicable 

But this theorem can apply.  $f(x)=0$ a.e.,  $g(x)\equiv0$ and $g'(x)\equiv 0$. Therefore, $g'=f$ a.e.





measure spaces $(X,\mathscr{A} \normalsize)$ and $(Y, \mathscr{B})$

Def. The Cartesian product of $X$ and $Y$ $= \{(x,y)\colon \; x\in X, y\in Y\}$. Denote it by $X\times Y$

Def. The Cartesian product of $\mathscr{A}$ and $\mathscr{B}$ $= \sigma$-algebra generated by $\{A\times B\colon \; A\in \mathscr{A}, B\in \mathscr{B} \}$.  Denote it by $\mathscr{A} \times \mathscr{B}$



Lemma

$E\in  \mathscr{A} \times \mathscr{B}  \Rightarrow E_x \in   \mathscr{B} $ and $E^y \in \mathscr{A}$



Lemma

$\mathscr{F}=\{\bigcup_{i=1}^{n} A_i \times B_i \colon\; \text{disjoint } A_i\times B_i \in   \mathscr{A}\times   \mathscr{B},  A_i\times B_i \subseteq E \subseteq X\times Y\}  \Rightarrow \mathscr{F}$ is a ring



Theorem

$(X,\mathscr{A}, \mu)$ and $(Y, \mathscr{B},\upsilon)$ $\sigma$-finite measure spaces

any set $E\in \mathscr{A} \times \mathscr{B} $ 

Define $f(x)=\upsilon(E_x) \; \forall x \in X$ and $g(y)=\mu(E^y) \; \forall y \in Y$

 $\Longrightarrow$ 

- $f$ and $g$ are measurable functions
- $\int f d\mu =\int g d\upsilon$



Theorem

$(X,\mathscr{A}, \mu)$ and $(Y, \mathscr{B},\upsilon)$ $\sigma$-finite measure spaces

Define $\lambda(E)=\int \upsilon(E_x)d\mu=\int \mu(E^y) d\upsilon$ for every set $E\in \mathscr{A} \times \mathscr{B}$ 

$\Longrightarrow$ 

- (1) $\lambda(E)$ is a $\sigma$-finite measure on $ \mathscr{A} \times \mathscr{B}$ 
- (2) for any rectangles $A \times B \in \mathscr{A} \times \mathscr{B} $, $\lambda(A\times B)= \mu(A) \upsilon(B)$ 
- (3) $\lambda$ is uniquely determined by (2) 



Def. the measure $\lambda$, denoted by $\mu \times \upsilon$, is called the product of the measures $\mu$ and $\upsilon$.

