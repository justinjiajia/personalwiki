









Examples of **convex cones**

- Norm cone: $\{(\mathbf{x}, t) \colon \; \Vert \mathbf{x} \Vert \leq t\}$, for a norm $\Vert \cdot \Vert $. Under the $\ell_2$ norm $\Vert \cdot \Vert _2$, called second-order cone

  

- Normal cone: given any set $C$ and point $\mathbf{x}\in C$, we can define $\mathcal{N}_C(\mathbf{x}) = \{ \mathbf{g} \colon \; \mathbf{g}^\top \mathbf{x} \geq  \mathbf{g}^\top \mathbf{y},\, \forall \, \mathbf{y} \in C \} $ .   
  - This is always a convex cone, regardless of $C$





#### Key properties of convex sets



**Separating hyperplane theorem**:  two disjoint convex sets have a separating between hyperplane them



Formally: if $C, D$ are nonempty convex sets with $C \cap D  = \empty $, then there exists $\mathbf{a}, \mathbf{b}$ such that $C \subseteq \{ \mathbf{x} \colon \; \mathbf{a}^\top \mathbf{x}\leq \mathbf{b} \} $ and $ D \subseteq \{ \mathbf{x} \colon \; \mathbf{a}^\top \mathbf{x} \geq  \mathbf{b} \} $



**Supporting hyperplane theorem**:  A boundary point of a convex set has a supporting hyperplane passing through it



Formally: if $C$ is a nonempty convex set, and $ \mathbf{x}_0 \in \text{bd}(C)$ (a boundary point), then there exists  $\mathbf{a}$ such that $C \subseteq \{ \mathbf{x} \colon \; \mathbf{a}^\top \mathbf{x}\leq \mathbf{a}^\top \mathbf{x}_0 \}$





Both of the above theorems (separating and supporting hyperplane theorems) have partial converses; see Section 2.5 of BV



#### Operations preserving convexity: Convex sets


- Intersection: the intersection of convex sets is convex

- Scaling and translation: if $C$ is convex, then $\alpha C + \beta = \{ \alpha \mathbf{x} + \beta \colon \; \mathbf{x} ∈ C\}$ is convex for any $\alpha, \beta$

- Affine images and preimages: if $f(\mathbf{x}) = \mathbf{A}\mathbf{x} + \mathbf{b}$ and $C$ is convex then $f(C) = \{f(\mathbf{x}) \colon \; \mathbf{x}\in  C\}$ is convex, and if $D$ is convex then $f^{-1}(D) = \{\mathbf{x} \colon \; f(\mathbf{x})\in  D\}$ is convex

- Perspective images and preimages: the perspective function is $P \colon \, \mathbb{R}^n \times \mathbb{R}_{++} \to \mathbb{R}^n$ (where $\mathbb{R}_{++} $ denotes positive reals), $P(\mathbf{x}, z) = \mathbf{x}/z$ for $z > 0$.

  If $C \subseteq \text{dom}(P)$ is convex then so is $P(C)$, and if $D$ is convex then so is $P^{−1}(D)$.

- Linear-fractional images and preimages: the perspective map composed with an affine function, 
  $$
  f(\mathbf{x}) = \frac{\mathbf{A}\mathbf{x} + \mathbf{b}} {\mathbf{c}^\top\mathbf{x} +d}
  $$
  is called a linear-fractional function, defined on $\mathbf{c}^\top\mathbf{x} +d >0$.
  If  $C \subseteq \text{dom}(P)$ is convex then so if $f(C)$, and if D is convex then so is $f^{−1}(D)$.

  



### Convex Functions



##### Example of Convex Functions

- Norm:

  $\Vert \cdot \Vert$ is convex for any norm; e.g., 

  - $\ell_p$ norms $\Vert \mathbf{x} \Vert_p = \sqrt[p]{\sum_{i=1}^n \vert x_i\vert^p } $ for $p \geq 1$
  - $\Vert \mathbf{x} \Vert_\infty = \underset{i=1,\,\dots,\,n}{\max} \: \vert x_i \vert $

  and also operator (spectral) and trace (nuclear) norms of a matrix $\mathbf{X}$:

  - $\Vert\mathbf{X} \Vert_{\text{op}} = \sigma_1(\mathbf{X})$

  - Trace norm: $\Vert \mathbf{X} \Vert_{\text{tr}} = \sum_{i=1}^r \sigma_i(\mathbf{X})$

   where $σ_1(\mathbf{X}) \geq \dots  \geq σ_r(\mathbf{X}) \geq 0$ are the singular values of the matrix $\mathbf{X}$.



- Indicator function of a convex set: if $C$ is convex, then its indicator function

  $$
  I_C(\mathbf{x})=\begin{cases} 0 \quad \; \;\mathbf{x} \in C \\ \infty  \quad \mathbf{x} \notin C\end{cases}
  $$
  is convex

  

- Support function: for any set $C$ (convex or not), its support function
  $$
  I^∗_C(\mathbf{x}) = \underset{\mathbf{y} \in C}{\max} \mathbf{x}^\top \mathbf{y}
  $$
  is convex

- Max function: $f(\mathbf{x}) = \max\{x_1, \dots , x_n\}$ is convex



##### Key properties of convex functions

- First-order characterization: 

  if $f$ is differentiable, then $f$ is convex if and only if $\text{dom}(f)$ is convex, and $f(\mathbf{y}) \geq f(\mathbf{x}) + \nabla f(\mathbf{x})^\top(\mathbf{y} − \mathbf{x})$ for all $\mathbf{x},\mathbf{y} \in \text{dom}(f)$ (the RHS is the tangent plane of the graph of $f$ at $\mathbf{x}$). Therefore, for a ***differentiable convex** function $\nabla f(\mathbf{x})=\mathbf{0} \Longleftrightarrow \mathbf{x}$ minimizes $f$




- Second-order characterization: 

  if $f$ is twice differentiable, then $f$ is convex if and only if $\text{dom}(f)$ is convex, and $\nabla^2  f(\mathbf{x}) \succeq 0$ for all $\mathbf{x}\in \text{dom}(f)$

  

- Jensen's inequality: if $f$ is convex, and $\mathbf{x}$ is a random variable supported on $\text{dom}(f)$, then $f( \mathbb{E}[\mathbf{x}]) ≤  \mathbb{E}[f(\mathbf{x})]$





#### Operations preserving convexity: Convex functions

- **Nonnegative linear combination**: $f_1, \, \dots , \, f_m$ convex implies $a_1f_1 + \dots + a_mf_m$ convex for any $a_1, \, \dots , \, a_m \geq 0 $ 
- **Pointwise maximization**: if $f_s$ is convex for any $s \in S$, then $f(\mathbf{x}) = \max_{s∈S} f_s(\mathbf{x})$ is convex. Note that the set $S$ here (number of functions $f_s$) can be infinite
- **Partial minimization**: if $g(\mathbf{x}, \mathbf{y} ) $ is convex in $\mathbf{x}, \mathbf{y}$, and $C$ is convex, then $f(\mathbf{x}) = \min_{\mathbf{y}∈C}\, g(\mathbf{x}, \mathbf{y})$ is convex





## first-order optimality

 

 Intuitively: says that gradient increases as we move away from $\mathbf{x}$.





## Gradient Descent





Consider unconstrained, smooth convex optimization

 $f\colon \, \mathbb{R}^n \to \mathbb{R}$ is convex and differentiable
$$
\underset{\mathbf{x}}{\min} f(\mathbf{x})
$$




Gradient descent: 

- Choose initial point $\mathbf{x}^{(0)} \in \mathbb{R}^n$

- Repeat:
  $$
  \mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \nabla f(\mathbf{x}^{(k-1)})  \quad k=1,2,\dots
  $$
  Stop at some point



#### Gradient descent interpretation

At each iteration, consider the expansion
$$
f(\mathbf{y}) \approx \underbrace{f(\mathbf{x}) + \nabla f(\mathbf{x})^\top(\mathbf{y}-\mathbf{x}) }_{\text{linear approximation to } f} \quad \quad + \underbrace{\frac{1}{2t} (\mathbf{y}-\mathbf{x})^\top (\mathbf{y}-\mathbf{x})}_{\text{proximity term to  } \mathbf{x} \text{,  with weight } 1/(2t)}
$$
This is a **quadratic approximation**, replacing usual Hessian $\nabla^2 f(\mathbf{x})$ by $\frac{1}{t}\mathbf{I}$.


Choose next point $ \mathbf{y}$ to minimize the quadratic approximation (Given $\mathbf{x}$, solve $\nabla f(\mathbf{y})=\frac{d}{d\mathbf{y}}f(\mathbf{y}) = \mathbf{0} $):

$ \nabla f(\mathbf{x})+\frac{1}{t}(\mathbf{y}-\mathbf{x})=\mathbf{0}$

$\mathbf{y} =  \mathbf{x} - t  \nabla f(\mathbf{x})$



<img src="https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/gradient_descent_approx.PNG" width=500/>







https://drive.google.com/drive/u/1/folders/1tn6kQe5BIbTxyKRYdXacqspYU9E-vxT9



#### Backtracking line search

One way to adaptively choose the step size is to use **backtracking line search**:

- First fix parameters $0 < \beta < 1$ and $0 < \alpha \leq 1/2$

- At each iteration, start with $t = t_{\text{init}}$, and while
  $$
  f\left( \mathbf{x} - t  \nabla f(\mathbf{x})\right)>f\left( \mathbf{x} \right)-\alpha t \Vert \nabla f(\mathbf{x})\Vert_2^2
  $$
  shrink $t = \beta t$. Else perform gradient descent update
  $$
  \mathbf{x}^+ =  \mathbf{x} - t  \nabla f(\mathbf{x})
  $$
  

Simple and tends to work well in practice (further simplification: just take $\alpha = 1/2$)





![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/backtracking_interpretation.PNG)





How to handle constraints: **projected gradient descent** (later)

How to handle nonsmooth objective functions: subgradient method, proximal gradient descent, etc.



## Subgradients



Recall that for convex and differentiable $f$,
$$
f(\mathbf{y}) \geq f(\mathbf{x}) + \nabla f(\mathbf{x})^\top (\mathbf{y}-\mathbf{x})
$$


That is, linear approximation always underestimates $f$

A subgradient of a convex function $f$ at $\mathbf{x}$ is any $\mathbf{g} \in \mathbb{R}^n$ such that
$$
f(\mathbf{y}) \geq f(\mathbf{x}) + \mathbf{g}^\top (\mathbf{y}-\mathbf{x}) \quad \text{for all} \quad \mathbf{y}
$$

- Always exists (on the relative interior of $\text{dom}(f) $)
- If $f$ differentiable at $\mathbf{x}$, then $\mathbf{g} = \nabla f(\mathbf{x})$ uniquely
- Same definition works for nonconvex $f$ (however, subgradients need not exist)



#### Examples of subgradients

Consider $f : \mathbb{R} \to \mathbb{R}$, $f(x) = |x|$

- For $x \neq 0$, unique subgradient $g = \text{sign} (x)$
- For $x=0$, subgradient $g$ is any element in $[-1,1]$







### Subdifferential

Set of all subgradients of convex $f$ is called the **subdifferential**:
$$
\part f(\mathbf{x})=\{\mathbf{g} \in \mathbb{R}^n \colon \; \mathbf{g} \text{ is a subgradient of } f \text{ at } \mathbf{x}\}
$$

- Nonempty (only for convex $f$)
- $\part f(\mathbf{x})$ is closed and convex (even for nonconvex $f$)
- $f$ is differentiable at $\mathbf{x}$ $\implies \part f(\mathbf{x}) = \{\nabla f( \mathbf{x})\}$
-  $\part f(\mathbf{x}) = \{\mathbf{g}\}$ (i.e., has a single element at $\mathbf{x}$) $\implies f$ is differentiable at $\mathbf{x}$ and $\nabla f(\mathbf{x}) =  \mathbf{g} $



##### Connection to convex geometry

Convex set $C \subseteq \mathbb{R}^n$, consider indicator function $I_C \colon \, \mathbb{R}^n \to \mathbb{R}$,
$$
I_C(\mathbf{x})=\begin{cases} 0 \quad \; \;\mathbf{x} \in C \\ \infty  \quad \mathbf{x} \notin C\end{cases}
$$
Recall the normal cone of $C$ at $\mathbf{x} \in C$ is $\mathcal{N}_C(\mathbf{x}) = \{\mathbf{g} \in  \mathbb{R}^n \colon \, \mathbf{g}^\top \mathbf{x} ≥ \mathbf{g}^\top
\mathbf{y} \text{ for any } \mathbf{y} \in C \}$

By definition of subgradient $\mathbf{g}$, 
$$
  I_C(\mathbf{y}) \geq I_C(\mathbf{x}) + \mathbf{g}^\top (\mathbf{y}-\mathbf{x}) \quad \text{for all } \mathbf{y}
$$


- For $\mathbf{y} \notin C$,  $I_C(\mathbf{x})=\infty$. The inequality holds trivially
- For $\mathbf{y} \in C$, $  0 \geq  \mathbf{g}^\top (\mathbf{y}-\mathbf{x})$

Therefore, for $\mathbf{x} \in C$, $ ∂I_C(\mathbf{x}) = \mathcal{N}_C(\mathbf{x})$.



#### Subgradient calculus

Basic rules for convex functions:

- Scaling: $\part (a f) = a  \part f$ provided $a > 0$

- Addition: $\part (f_1 + f_2) = \part f_1 + \part f_2$

- Affine composition: if $g(\mathbf{x}) = f(A \mathbf{x} + \mathbf{b})$, then $\part g(\mathbf{x}) = A^\top \part f(A \mathbf{x} + \mathbf{b})$

- Finite pointwise maximum: if $f(\mathbf{x}) = \underset{i=1,\,\dots,\,m}{\max}  f_i(\mathbf{x})$, then
  $$
  \part f(\mathbf{x}) =\text{conv}\left( \underset{i\colon f_i(\mathbf{x})= f(\mathbf{x})}{\bigcup}   \part f_i(\mathbf{x}) \right )
  $$
  **convex hull** of union of subdifferentials of active functions at $\mathbf{x}$.

  

- General composition: if $f(\mathbf{x}) = h (\mathbf{g}(\mathbf{x})) = h\left(g_1(\mathbf{x}), \,\dots \,, g_k(\mathbf{x})\right)$ where $ \mathbf{g}\colon \, \mathbb{R}^n \to \mathbb{R}^k$, $h\colon \, \mathbb{R}^k \to \mathbb{R}$, $ f\colon \, \mathbb{R}^n \to \mathbb{R}$, $h$ is convex
  and nondecreasing in each argument, $\mathbf{g}$ is convex, then
  $$
  \part f(\mathbf{x}) \subseteq \{ p_1 \mathbf{q}_1+ \dots+ p_k \mathbf{q}_k \colon \; \mathbf{p} \in \part h(g(\mathbf{x})),  \; \mathbf{q}_i \in \part g_i(\mathbf{x}), i=1,\, \dots, \,k\}
  $$
  
- General pointwise maximum: if $f(\mathbf{x}) = {\max}_{s \in S}  f_s(\mathbf{x})$ f(x) = , then
  $$
  \part f(\mathbf{x}) \supseteq \text{closure} \Bigg\{ \text{conv}\left( \underset{s\colon f_s(\mathbf{x})= f(\mathbf{x})}{\bigcup}   \part f_s(\mathbf{x}) \right ) \Bigg\}
  $$
  Under some regularity conditions (on $S, f_s$), we get equality ($S$ compact set, each $f_s$ is continuous)

  

- Norm: important special case. To each norm $\Vert \cdot \Vert$, there is a dual norm $\Vert \cdot \Vert_{*}$ such that
  $$
  \Vert \mathbf{x} \Vert = \underset{\Vert \mathbf{z} \Vert_{*}\leq1 }{\max}  \mathbf{z}^\top\mathbf{x}
  $$
  In fact, for $f(\mathbf{x} ) =\Vert \mathbf{x} \Vert$ and $f_{\mathbf{z}}(\mathbf{x} ) = \mathbf{z} ^\top \mathbf{x} $, we get equality (because $\part f_{\mathbf{z}}(\mathbf{x})= {\mathbf{z}}$ ):
  $$
  \part f(\mathbf{x}) = \text{closure} \Bigg\{ \text{conv}\left( \underset{\mathbf{z}\colon f_{\mathbf{z}}(\mathbf{x})= f(\mathbf{x})}{\bigcup}   \mathbf{z} \right ) \Bigg\}
  $$
  And if ${\mathbf{z}}_1, {\mathbf{z}}_2$ each achieve the max (i.e., are each active) at ${\mathbf{x}}$, which means that $ \mathbf{z}_1^\top\mathbf{x} =  \mathbf{z}_2^\top\mathbf{x}$​, 

  then by linearity, so will $t \, \mathbf{z}_1 + (1 − t)\, \mathbf{z}_2$ for any $t \in [0, 1]$. Thus
  $$
  \part f(\mathbf{x}) =  \underset{\Vert \mathbf{z} \Vert_{*}\leq1 }{\arg\max }\;  \mathbf{z}^\top\mathbf{x}
  $$
  Examples:

  - $\part \left(\Vert \mathbf{x} \Vert _2\right)= \underset{\Vert \mathbf{z} \Vert_{2}\leq1 }{\arg\max }\;  \mathbf{z}^\top\mathbf{x} = \begin{cases} \frac{\mathbf{x} \;}{\Vert \mathbf{x} \Vert_{2}  } \quad\quad\quad\quad\quad \text{if }  \mathbf{x} \neq \mathbf{0}\\ \{\mathbf{z}\colon \, \Vert \mathbf{z} \Vert_{2}\leq1 \}  \quad \text{if }  \mathbf{x} = \mathbf{0} \end{cases}$
  - $\part \left(\Vert \mathbf{x} \Vert _1\right)= \underset{\Vert \mathbf{z} \Vert_{\infty}\leq1 }{\arg\max }\;  \mathbf{z}^\top\mathbf{x} = \begin{cases}    \text{sign}(x_i) \quad \;\text{if }  x_i \neq 0\\ [-1,1]  \quad \quad \text{if }  x_i=0 \end{cases}$ , where $\text{sign}(x_i)= \begin{cases} 1 \quad \quad \text{if }  x_i> 0 \\ -1 \quad \; \text{if }  x_i< 0 \end{cases}$













#### Subgradient Optimality Condition

For any $f$ (convex or not),
$$
f(\mathbf{x}^\star)=\underset{\mathbf{x}}{\min}f(\mathbf{x}) \Longleftrightarrow \mathbf{0} \in \part f(\mathbf{x}^\star)
$$
That is, $\mathbf{x}^\star$ is a minimizer if and only if $ \mathbf{0}$ is a subgradient of $f$ at  $\mathbf{x}^\star$.

-  $\mathbf{0} \in \part f(\mathbf{x}^\star)$ means $f( \mathbf{y}) \geq  f(\mathbf{x}^\star) +  \mathbf{0}^\top( \mathbf{y}- \mathbf{x}^\star) =  f(\mathbf{x}^\star)$ for all $\mathbf{y}$
- the implication for a convex and differentiable function $f$ (which has $ \part f(\mathbf{x}) = \{\nabla f( \mathbf{x})\}$):  $\part f(\mathbf{x}^\star)=\{\mathbf{0}\}$



Example of the power of subgradients: 





##### Derivation of first-order optimality

Use the subgradient optimality condition to to derive the first-order optimality condition.

Recall that the first-order optimality condition is, for $f$ convex and differentiable
$$
\underset{\mathbf{x}}{\min}f(\mathbf{x}) \quad \text{ subject to } \mathbf{x} \in C
$$
is solved at $\mathbf{x} \Longleftrightarrow $ 

$$
\nabla f(\mathbf{x})^\top(f(\mathbf{y})-f(\mathbf{x}))\geq0 \quad \text{for all }\mathbf{y} \in C
$$
Recast the problem as 
$$
\underset{\mathbf{x}}{\min}f(\mathbf{x})+ I_C( \mathbf{x})
$$
where the indicator function $I_C \colon \; \mathbb{R}^n \to \mathbb{R}$ is defined as $I_C (\mathbf{x})= \begin{cases}0 \quad \;\; \text{if } \mathbf{x} \in C \\ \infty \quad  \text{otherwise} \end{cases}$


$$
\mathbf{0} \in \part \left(f(\mathbf{x})+ I_C( \mathbf{x})\right) =  \part  f(\mathbf{x})+ \part I_C( \mathbf{x})= \{\nabla f(\mathbf{x})\} + \mathcal{N}_c(\mathbf{x}) \\
\Longleftrightarrow \quad -\nabla f(\mathbf{x}) \in \mathcal{N}_c(\mathbf{x}) \\
\Longleftrightarrow \quad  -\nabla f(\mathbf{x})^\top\mathbf{x}\geq -\nabla f(\mathbf{x})^\top\mathbf{y} \quad \text{for all }\mathbf{y} \in C \\
\Longleftrightarrow \quad \nabla f(\mathbf{x})^\top(f(\mathbf{y})-f(\mathbf{x}))\geq0 \quad \text{for all }\mathbf{y} \in C
$$
Note: the condition $0 ∈ \part f(\mathbf{x}) + \mathcal{N}_c(\mathbf{x})$ is a fully general condition for optimality in convex problems. 

But it's not always easy to work with (KKT conditions, later, are easier)





#### Subgradient method

Consider $f$ convex, and $ \text{dom}(f) =\mathbb{ R}^n$, but not necessarily differentiable

Subgradient method: like gradient descent, but replacing gradients with subgradients. 

Initialize  $\mathbf{x}^{(0)} \in \mathbb{R}^n$ , repeat:
$$
\mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \mathbf{g}^{(k-1)}  \quad k=1,2,\dots
$$
where $\mathbf{g}^{(k-1)}  \in \part f(\mathbf{x}^{(k-1)})$ (any subgradient of $f$ at $\mathbf{x}^{(k-1)}$)

Subgradient method is not necessarily a descent method, thus we keep track of best iterate $\mathbf{x}^{(k)}_{\text{best}}$ among $\mathbf{x}^{(0)}, .\dots , \mathbf{x}^{(k)}$ so far, i.e.,
$$
f\left(\mathbf{x}^{(k)}_{\text{best}}\right)= \underset{i=0,\,\dots,\,k}{\min} f\left(\mathbf{x}^{(i)}\right)
$$



If $f$ is Lipschitz, then subgradient method has a convergence rate $O(1/\epsilon^2)$

Upside: very generic. Downside: can be slow







# Stochastic Gradient Descent



for minimizing an average of functions
$$
\underset{\mathbf{x}}{\min} \; \frac{1}{n} \sum_{i=1}^n f_i(\mathbf{x})
$$


gradient descent or GD repeats:
$$
\mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \, \frac{1}{n} \sum_{i=1}^n \nabla f_i(\mathbf{x}^{(k-1)})  \quad k=1,2,\dots
$$
whereas stochastic gradient descent or SGD repeats:
$$
\mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \,  \nabla f_{i_k} (\mathbf{x}^{(k-1)})  \quad k=1,2,\dots
$$
where $i_k \in \{1, \dots , n\}$ is a randomly chosen index at iteration $k$.

Note $E[∇f_{i_k}(\mathbf{x})] = \nabla f(\mathbf{x})$, unbiased estimate of full gradient. ()



#### Mini-batches

Also common is mini-batch stochastic gradient descent, where we choose a random subset $I_k ⊆ \{1, \dots, n\}$, of size $|I_k| = b \ll  n$, and
repeat:
$$
\mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \, \frac{1}{b} \sum_{i \in I_k} \nabla f_{i} (\mathbf{x}^{(k-1)})  \quad k=1,2,\dots
$$
Again, we are approximating full graident by an unbiased estimate: $E[\frac{1}{b} \sum_{i \in I_k} \nabla f_{i}(\mathbf{x})] = \nabla f(\mathbf{x})$

Using mini-batches reduces variance of the gradient estimate by a factor $1/b$, but is also $b$ times more expensive



#### Stochastic average gradient

Stochastic average gradient or SAG (Schmidt, Le Roux, and Bach 2013) is a breakthrough method in stochastic optimization:

- Maintain table, containing gradient $g_i$ of $f_i$, $i = 1, \dots, n$

- Initialize $\mathbf{x}^{(0)} $, and $\mathbf{g}^{ (0)}_i = \nabla f_i(\mathbf{x}^{(0)})$,  $i = 1, \dots, n$

- At steps $k = 1, 2, 3, \dots$, pick random  $i_k \in \{1, \dots , n\}$, then let $\mathbf{g}^{ (k)}_{i_k}  =  \nabla f_{i_k}(\mathbf{x}^{(k-1)})$ i.e., most recent graident of $f_{i_k}$. 
  
  Set all other $\mathbf{g}^{ (k)}_i  = \mathbf{g}^{ (k-1)}_i$, $i \neq i_k$, i.e., these stay the same
  
- Update
  $$
  \mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \, \frac{1}{n} \sum_{i =1}^n \mathbf{g}_{i}^{(k) }
  $$
  

Notes:

- Key of SAG is to allow each $f_i $, $i = 1, \dots , n$ to communicate a part of the gradient estimate at each step

- This basic idea can be traced back to incremental aggregated gradient (Blatt, Hero, Gauchman, 2006)

- SAG gradient estimates are ***no longer unbiased***, but they have ***greatly reduced variance***.

- Is it expensive to average all these gradients? Basically just as efficient as SGD:
  $$
  \mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \, \underbrace{\left(\frac{\mathbf{g}^{ (k)}_{i_k}}{n\,}-\frac{\mathbf{g}^{ (k-1)}_{i_k}}{n\quad}+  \underbrace{\frac{1}{n}\sum_{i =1}^n \mathbf{g}_{i}^{(k-1) }}_{\text{old table average}} \right)}_{\text{new table average}}
  $$





##### SAGA

SAGA (Defazio, Bach, and Lacoste-Julien 2014) is a follow-up on the SAG work:

- Maintain table, containing gradient $g_i$ of $f_i$, $i = 1, \dots, n$

- Initialize $\mathbf{x}^{(0)} $, and $\mathbf{g}^{ (0)}_i = \nabla f_i(\mathbf{x}^{(0)})$,  $i = 1, \dots, n$

- At steps $k = 1, 2, 3, \dots$, pick random  $i_k \in \{1, \dots , n\}$, then let $\mathbf{g}^{ (k)}_{i_k}  =  \nabla f_{i_k}(\mathbf{x}^{(k-1)})$

- Update
  $$
  \mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \,  \left(\mathbf{g}^{ (k)}_{i_k}- \mathbf{g}^{ (k-1)}_{i_k}+ \frac{1}{n} \sum_{i =1}^n  \mathbf{g}_{i}^{(k-1) }\right)
  $$
  

Note:

- SAGA gradient estimate $\left(\mathbf{g}^{ (k)}_{i_k}- \mathbf{g}^{ (k-1)}_{i_k}+ \frac{1}{n}\sum_{i =1}^n  \mathbf{g}_{i}^{(k-1) }\right)$, versus SAG gradient estimate $ \frac{1}{n}\left(\mathbf{g}^{ (k)}_{i_k}-\mathbf{g}^{ (k-1)}_{i_k}+  \sum_{i =1}^n \mathbf{g}_{i}^{(k-1) } \right)$.

- Recall, SAG estimate is biased; remarkably, SAGA estimate is ***unbiased***. Simple explanation: consider family of estimators
  $$
  θ_\alpha = \alpha(X − Y ) + \mathbb{E}(Y)
  $$
  for $\mathbb{E}(X)$, where $\alpha \in [0,1]$, and $X, Y$ are presumed correlated.
  $$
  \mathbb{E}(θ_\alpha)=\alpha\mathbb{E}(X) +(1−\alpha) \mathbb{E}(Y)\\
  \text{Var}(θ_\alpha)=\alpha^2\left(\text{Var}(X)+\text{Var}(Y)-2\text{Cov}(X,Y)\right)
  $$
  SAGA uses $\alpha = 1$ (unbiased), SAG uses $\alpha = 1/n$ (biased). 

- SAGA matches convergence rates of SAG

- Interestingly, the SAGA criterion curves look like SGD curves (realizations being jagged and highly variable); SAG looks very
  different, and this really emphasizes the fact that its updates have ***much lower variance***.



#### AdaGrad



Another big topic in stochastic optimization these days: **adaptive step sizes**.

To motivate, let’s consider a logistic regression problem, where $x_{ij}$ are binary, and many of them are zero. For example, classifying if a
given movie review is positive or negative:



> Piece <span style="color:blue;" >of</span> <span style="color:green;" >subtle art</span>. <span style="color:blue;" >Maybe a</span> <span style="color:green;" >masterpiece</span>. Doubtlessly a <span style="color:green;">special story</span> <span style="color: blue;" >about the</span> ambiguity <span style="color:blue;" >of</span> existence.



Some words are common (<span style="color:blue;" >blue</span>) and uninformative and some rare (<span style="color:green;" >green</span>) and informative. Here:

- $x_{ij}$ represents whether the $j$-th word is present in $i$-th review
- $y_i$ represents the ith review is positive or negative (sentiment)

AdaGrad (Duchi, Hazan, and Singer 2010): very popular adaptive method. 



So what does SGD do?

- Gives equal weight to common and to rare informative words
- Diminishing step sizes $t_k$ means the rare informative features are learned very slowly ...



At steps $k = 1, 2, 3, \dots$, pick random  $i_k \in \{1, \dots , n\}$. Then let $\mathbf{g}^{ (k)} =  \nabla f_{i_k}(\mathbf{x}^{(k-1)})$, and update for $j = 1, \dots , p$:
$$
x_j^{(k)} =x_j^{(k-1)}-\alpha \frac{g_j^{(k)}}{\sqrt{ \sum_{l=1}^k \left({g_j}^{(l)}\right)^2+ \epsilon}}
$$


Notes:

- AdaGrad does not require tuning learning rate: $\alpha > 0$ is fixed constant, learning rate decreases naturally over iterations
- Learning rate of rare informative features diminishes slowly (since if the $j$-th feature is rare,  ${g_j}^{(k)}=0$ for many $k$)
- Can drastically improve over SGD in ***sparse problems***
- Main weakness is monotonic accumulation of gradients in the denominator ... AdaDelta, Adam, AMSGrad, etc. improve on
  this, popular in training deep nets






## Proximal Gradient Descent



https://drive.google.com/drive/u/1/folders/1tn6kQe5BIbTxyKRYdXacqspYU9E-vxT9



Consider minimizing a composite function $f(\mathbf{x}) = g(\mathbf{x}) + h(\mathbf{x})$

- $g$ is convex, differentiable,  $\text{dom}(f)=\mathbb{ R}^n$
- $h$ is convex, not necessarily differentiable

If $f$ were differentiable, then gradient descent update $\mathbf{x}^+ =  \mathbf{x} - t  \nabla f(\mathbf{x})$ minimizes a quadratic approximation to $f$ around $ \mathbf{x}$:
$$
f(\mathbf{x}) + \nabla f(\mathbf{x})^\top(\mathbf{z}-\mathbf{x})+\frac{1}{2t}\Vert\mathbf{z}-\mathbf{x}\Vert^2_2
$$

$$
\mathbf{x}^+ = \underset{\mathbf{z}}{\arg \min} \; \;f(\mathbf{x}) + \nabla f(\mathbf{x})^\top(\mathbf{z}-\mathbf{x})+\frac{1}{2t}\Vert\mathbf{z}-\mathbf{x}\Vert^2_2
$$



In our case $f$ is not differentiable, but $f = g + h$, $g$ differentiable.

We make a quadratic approximation to $g$ while leaving $h$ alone:
$$
\mathbf{x}^+ = \underset{\mathbf{z}}{\arg \min} \; \;g(\mathbf{x}) + \nabla g(\mathbf{x})^\top(\mathbf{z}-\mathbf{x})+\frac{1}{2t}\Vert\mathbf{z}-\mathbf{x}\Vert^2_2 + h(\mathbf{z}) \\
= \underset{\mathbf{z}}{\arg \min} \; \; \frac{1}{2t} \Vert\mathbf{z}-\left(\mathbf{x}-t_k\nabla g(\mathbf{x})\right)\Vert^2_2+ h(\mathbf{z})
$$


Define proximal mapping:
$$
\text{prox}_{h,t}(\mathbf{x}) = \underset{\mathbf{z}}{\arg \min} \; \; \frac{1}{2t} \Vert\mathbf{z}-\mathbf{x}\Vert^2_2+ h(\mathbf{z})
$$
Note:

- Both $\frac{1}{2t} \Vert\mathbf{z}-\mathbf{x}\Vert^2_2$ and $ h(\mathbf{z})$ are convex

- $\text{prox}_{h,t}(\cdot) $ has a closed-form for many important functions $h$.

  - Mapping $\text{prox}_{h,t}(\cdot) $ doesn't depend on $g$ at all, only on $h$
  - Smooth part $g$ can be complicated, we only need to compute its gradients (see below)

  

#### Proximal gradient descent

choose initial point $\mathbf{x}^{(0)} \in \mathbb{R}^n$ , repeat:
$$
\mathbf{x}^{(k)} \gets  \text{prox}_{h,\,t_k}\left(\mathbf{x}^{(k-1)}-t_k \nabla g(\mathbf{x}^{(k-1)})\right)   \quad k=1,2,\dots
$$


Proximal gradient descent also called **composite gradient descent** or **generalized gradient descent**

- $h = 0$: gradient descent
- $h = I_C$: projected gradient descent
- $g = 0$: proximal minimization algorithm









#### Projected gradient descent


$$
\underset{\mathbf{x} \in C}{\min}f(\mathbf{x}) \Longleftrightarrow  \underset{\mathbf{x} }{\min}f(\mathbf{x})+I_C(\mathbf{x})
$$
where $I_C$ is the indicator function of set $C$

The proximal mapping is
$$
\text{prox}_{t}(\mathbf{x}) = \underset{\mathbf{z}}{\arg \min} \; \; \frac{1}{2t} \Vert\mathbf{z}-\mathbf{x}\Vert^2_2+ I_C(\mathbf{z}) = \underset{\mathbf{z} \in C}{\arg \min} \; \; \frac{1}{2t} \Vert\mathbf{z}-\mathbf{x}\Vert^2_2
$$
so $\text{prox}_{t}(\mathbf{x}) = P_C(\mathbf{x})$ (projection operator onto $C$)

Therefore proximal gradient update step is:
$$
\mathbf{x}^{(k)} \gets  P_C\left(\mathbf{x}^{(k-1)}-t_k \nabla g(\mathbf{x}^{(k-1)})\right)   \quad k=1,2,\dots
$$
It performs usual gradient update and then project back onto $C$.  Called **projected gradient descent**

<img src="https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/projected_gradient_descent.PNG" width=400/>





#### Proximal minimization algorithm



Consider for $h$ convex (not necessarily differentiable),

$$
\underset{\mathbf{x}  }{\min} h(\mathbf{x})
$$
Because $g\equiv0$, $\nabla g =0$. Consequently, Proximal gradient update step is just:
$$
\mathbf{x}^+ = \underset{\mathbf{z}}{\arg \min} \; \; \frac{1}{2t} \Vert\mathbf{z}- \mathbf{x} \Vert^2_2+ h(\mathbf{z})
$$
Called **proximal minimization algorithm**. Faster than subgradient method, but not implementable unless we know $\text{prox}$ in closed form





#### Accelerated proximal gradient method



Turns out we can accelerate proximal gradient descent in order to achieve the optimal $O(1/\sqrt \epsilon)$ convergence rate. Four ideas (three acceleration methods) by Nesterov:

- 1983: original acceleration idea for smooth functions
- 1988: another acceleration idea for smooth functions
- 2005: smoothing techniques for nonsmooth functions, coupled with original acceleration idea
- 2007: acceleration idea for composite functions (Each step uses entire history of previous steps and makes two prox calls)

Here we present a method by Beck and Teboulle (2008), an extension of Nesterov (1983) to composite functions (Each step uses information from two last steps and makes one prox call)



Consider minimizing a composite function



$$
\underset{\mathbf{x}  }{\min} \;g(\mathbf{x}) + h(\mathbf{x})
$$
where $g$ is convex, differentiable,  and $h$ is convex.

Accelerated proximal gradient method: choose initial point $\mathbf{x}^{(0)} = \mathbf{x}^{(-1)}  \in \mathbb{R}^n$, repeat:
$$
\mathbf{v} = \mathbf{x}^{(k-1)} + \underbrace{\frac{k-2}{k+1} }_{\text{momentum weight} }  \underbrace{\left( \mathbf{x}^{(k-1)} -\mathbf{x}^{(k-2)}\right)}_{\text{momentum} }\\

\mathbf{x}^{(k)} \gets  \text{prox}_{t_k}\left(\mathbf{x}^{(k-1)}-t_k \nabla g(\mathbf{v})\right)   \quad k=1,2,\dots
$$


- First step $k = 1$ is just usual proximal gradient update
- After that, $\mathbf{v} = \mathbf{x}^{(k-1)} + \frac{k-2}{k+1} \left( \mathbf{x}^{(k-1)} -\mathbf{x}^{(k-2)}\right)$ carries some "momentum" from previous iterations
- When $h = 0$ we get accelerated gradient method
- Accelerated proximal gradient is not a descent method



<img src="https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/momentum_weight.PNG" width=500/>

## Langrange dual problem



https://drive.google.com/drive/u/1/folders/1tn6kQe5BIbTxyKRYdXacqspYU9E-vxT9



#### duality in linear programs

Given $\mathbf{c} \in \mathbb{R}^n$, $A \in \mathbb{R}^{m×n}$, $\mathbf{b}\in \mathbb{R}^m$, $ G \in \mathbb{R}^{r×n}$,  $\mathbf{h} \in \mathbb{R}^{r}$ , the primal LP and dual LP are as follows:
$$
\underset{\mathbf{x}  }{\min} \; \mathbf{c}^\top \mathbf{x}  &&&&&&  \underset{\mathbf{u},\,  \mathbf{v} }{\max}  \; -\mathbf{b}^\top \mathbf{u}  -\mathbf{h}^\top \mathbf{v}  \\
A\mathbf{x} = \mathbf{b} &&&&&& -A^\top \mathbf{u} -G^\top\mathbf{v} = \mathbf{c} \\
G \mathbf{x} \leq  \mathbf{h}  &&&&&& \mathbf{v}\geq \mathbf{0}
$$


- Explanation 1: for any $\mathbf{u}$ and $\mathbf{v}\geq \mathbf{0}$, and $\mathbf{x}$ primal feasible,
  $$
  -\left(A \mathbf{x}\right)^\top\mathbf{u} - \left(G \mathbf{x}\right)^\top \mathbf{v} \geq -\mathbf{b}^\top \mathbf{u}  -\mathbf{h}^\top \mathbf{v}\\ \Longleftrightarrow \quad
  \mathbf{x}^\top \left(-A^\top \mathbf{u}-G^\top\mathbf{v}\right) \geq -\mathbf{b}^\top \mathbf{u}  -\mathbf{h}^\top \mathbf{v}
  $$
  So if $-A^\top \mathbf{u}-G^\top\mathbf{v}=\mathbf{c}$, $\underset{\mathbf{u},\,  \mathbf{v} }{\max}  \; -\mathbf{b}^\top \mathbf{u}  -\mathbf{h}^\top \mathbf{v} $ gives a bound on primal optimal value.



- Explanation 2: for any $\mathbf{u}$ and $\mathbf{v}\geq \mathbf{0}$, and $\mathbf{x}$ primal feasible
  $$
  \mathbf{c}^\top \mathbf{x} \geq \mathbf{c}^\top \mathbf{x}+ \mathbf{u}^\top \left(A \mathbf{x} -  \mathbf{b}\right) + \mathbf{v}^\top\left(G \mathbf{x} -  \mathbf{h}\right) := \mathcal{L}(\mathbf{x},\mathbf{u},\mathbf{v})
  $$
  So if $C$ denotes primal feasible set, $f^\star = \underset{\mathbf{x} \in C }{\min} \,\mathbf{c}^\top \mathbf{x} $ primal optimal value, then for any $\mathbf{u}$ and $\mathbf{v}$,
  $$
  f^\star\geq \underset{\mathbf{x} \in C }{\min} \,\mathcal{L}(\mathbf{x},\mathbf{u},\mathbf{v})\geq \underset{\mathbf{x}  }{\min} \,\mathcal{L}(\mathbf{x},\mathbf{u},\mathbf{v}) := g(\mathbf{u},\mathbf{v})
  $$
  In other words, $ g(\mathbf{u},\mathbf{v})$ is a lower bound on $f^\star$ for any $\mathbf{u}$ and $\mathbf{v}\geq \mathbf{0}$.

  Note that
  $$
  g(\mathbf{u},\mathbf{v}) = \begin{cases} -\mathbf{b}^\top \mathbf{u}  -\mathbf{h}^\top \mathbf{v} \quad \text{if } \mathbf{c}=-A^\top \mathbf{u} -G^\top\mathbf{v} \\ -\infty \quad \quad \quad \quad \quad \text{otherwise} \end{cases}
  $$
  Explanation 2 reproduces the same dual, but is actually completely general and applies to arbitrary optimization problems



### Lagrangian

Consider general minimization problem
$$
\underset{\mathbf{x}  }{\min} \; f( \mathbf{x})       \\
\text{subject to } \quad h_i(\mathbf{x}) \leq 0  \quad  i=1,\dots, m \\
\quad \quad \quad \quad \quad l_j (\mathbf{x}) = 0  \quad j=1,\dots, r
$$
Need not be convex, but of course we will pay special attention to convex case

We define the Lagrangian as
$$
\mathcal{L}(\mathbf{x},\mathbf{u},\mathbf{v}) = f(\mathbf{x}) + \sum_{i=1}^{m} u_ih_i(\mathbf{x})+ \sum_{j=1}^{r} v_j l_j(\mathbf{x})
$$
New variables $\mathbf{u} \in \mathbb{R}^m, \mathbf{v} \in   \mathbb{R}^r$, with $ \mathbf{u}\geq \mathbf{0}$ (else $\mathcal{L}(\mathbf{x},\mathbf{u},\mathbf{v}) = −\infty$)



Our dual function $g(\mathbf{u},\mathbf{v})$ satisfies $f^\star \geq g(\mathbf{u},\mathbf{v})$ for all $\mathbf{u}$ and $\mathbf{v}\geq \mathbf{0}$. 

Hence best lower bound: maximize $g(\mathbf{u},\mathbf{v})$  over dual feasible $\mathbf{u},\mathbf{v}$ yielding Lagrange dual problem:
$$
\underset{\mathbf{u},\, \mathbf{v} }{\max} \; g(\mathbf{u}\geq \mathbf{0},\mathbf{v})\\
\mathbf{u}\geq \mathbf{0}
$$
Note:

- Key property, called weak duality: if dual optimal value is $g^\star$, then $f^\star \geq g^\star$. This always holds (even if primal problem is nonconvex)
- Another key property: the dual problem is always a convex optimization problem (as written, it is a concave maximization problem) (even when primal problem is not convex)
  - the dual function $g(\mathbf{u},\mathbf{v})$  is the pointwise infimum of a family of affine functions of $(\mathbf{u},\mathbf{v})$. It's concave (see pointwise maximization for preserving convexity)







**Slater's condition**: 

if the primal is a convex problem (i.e., $f$ and $h_1, \, \dots , \, h_m$ are convex, $l_1, \, \dots , \, l_r$ are affine), and there exists at least one strictly feasible $\mathbf{x} \in \mathbb{ R}^n$, meaning

$h_1(\mathbf{x}) < 0, \, \dots,\, h_m(\mathbf{x}) < 0$ and $l_1(\mathbf{x}) = 0, \, \dots ,\,  l_r(\mathbf{x})= 0$ 

then **strong duality** holds (i.e., $f^\star =g^\star$)
Refinement: actually only need strict inequalities for non-affine $h_i$





#### Duality gap

Given primal feasible $\mathbf{x}$ and dual feasible $\mathbf{u}, \mathbf{v}$, the quantity

$$
f(\mathbf{x}) − g(\mathbf{u}, \mathbf{v})
$$


is called the **duality gap** between $\mathbf{x}$  and $\mathbf{u}, \mathbf{v}$. Note that
$$
f(\mathbf{x}) - f^\star \leq f(\mathbf{x}) − g(\mathbf{u}, \mathbf{v})
$$




#### KKT conditions

for the problem
$$
\underset{\mathbf{x}  }{\min} \; f( \mathbf{x})       \\
\text{subject to } \quad h_i(\mathbf{x}) \leq 0  \quad  i=1,\dots, m \\
\quad \quad \quad \quad \quad l_j (\mathbf{x}) = 0  \quad j=1,\dots, r
$$
The KKT conditions are

- stationarity: $ 0 \in \part_{\mathbf{x}}\left( f(\mathbf{x}) + \sum_{i=1}^{m} u_i h_i(\mathbf{x}) + \sum^r_{j=1} v_j l_j (\mathbf{x}) \right) $ 
- complementary slackness:  $u_i \, h_i(\mathbf{x}) = 0 $ for all $i$ 
- primal feasibility: $h_i(\mathbf{x}) \leq 0, l_j (\mathbf{x}) = 0$ for all $i, j$ 
- dual feasibility: $u_i \geq 0$ for all $i$

Necessary for optimality under **strong duality**, and always sufficient.





An important consequence of **stationarity**: under **strong duality**, given a dual solution $\mathbf{u}^\star, \mathbf{v}^\star$,  any primal solution $\mathbf{x}^\star$ solves the following unconstraint problem:
$$
\underset{\mathbf{x}  }{\min} \; f(\mathbf{x}) + \sum_{i=1}^{m} u_i^\star h_i(\mathbf{x})+ \sum_{j=1}^{r} v_j^\star l_j(\mathbf{x})
$$

- Often, solutions of this unconstrained problem can be expressed explicitly, giving an explicit characterization of primal solutions from dual solutions

- Furthermore, suppose the solution of this problem is unique; then it must be the primal solution $\mathbf{x}^\star$ 

This can be very helpful when the dual is easier to solve than the primal

 



Key facts about primal-dual relationship:

- Dual has complementary number of variables: recall, number of primal constraints
- Dual involves complementary norms: $\Vert \cdot \Vert$ becomes $\Vert \cdot \Vert_{*}$
- Dual has “identical” smoothness: L/m (Lipschitz constant of gradient by strong convexity parameter) is unchanged between
  $f$ and its conjugate $f^*$
- Dual can “shift” linear transformations between terms ... this leads to key idea: dual decomposition



#### Dual norms

Let $\Vert \mathbf{x} \Vert$ be a norm, e.g.,

- $\ell_p$ norm: $\Vert \mathbf{x} \Vert_p = \sqrt[p]{\sum_{i=1}^n \vert x_i\vert^p }  $ for $p \geq 1$
- Trace norm: $\Vert X \Vert_{\text{tr}} = \sum_{i=1}^r \sigma_i(X)$

We define its dual norm $\Vert \mathbf{x} \Vert_{*}$ as
$$
\Vert \mathbf{x} \Vert_{*} = \underset{\Vert \mathbf{z} \Vert\leq1 }{\max}  \mathbf{z}^\top\mathbf{x}
$$

- We have $\vert \mathbf{z}^\top\mathbf{x} \vert \leq \Vert \mathbf{z} \Vert \Vert \mathbf{x} \Vert_{*} $ ((like generalized Holder)

- $\ell_p$ norm dual: $ \left(\Vert \mathbf{x} \Vert_p\right)_{*} = \Vert \mathbf{x} \Vert_q$ where $\frac{1}{p} + \frac{1}{q} =1$

- Trace norm dual: $\left( \Vert X \Vert_{\text{tr}} \right)_{*} = \Vert X \Vert_{\text{op}}=\sigma_1 $ (the largest singular value)

- Dual norm of dual norm:  $\Vert \mathbf{x} \Vert_{**} =\Vert \mathbf{x} \Vert$

  - consider the (trivial-looking) problem (Slater's condition holds, and thereby we have strong duality)
    $$
    \underset{\mathbf{y}  }{\min}\; \Vert \mathbf{y} \Vert \quad \text{subject to } \;
    \mathbf{y} = \mathbf{x}
    $$
    whose optimal value $f^\star$ is $\Vert \mathbf{x} \Vert$. Lagrangian:
    $$
    \mathcal{L}(\mathbf{y},\mathbf{u}) =   \Vert \mathbf{y} \Vert + \mathbf{u}^\top( \mathbf{x} - \mathbf{y} )
    $$
    Using the definition of $\Vert \cdot \Vert_{*}$

    - if $\Vert \mathbf{u} \Vert_{*} \leq 1$,  $\underset{\mathbf{y}  }{\min}\;  \Vert \mathbf{y} \Vert - \mathbf{u}^\top   \mathbf{y} =0 $ by Holder inequality $\mathbf{u}^\top   \mathbf{y} \leq  \Vert \mathbf{y} \Vert \Vert \mathbf{u} \Vert_{*} \leq \Vert \mathbf{y} \Vert $ 
    
    - otherwise, $\underset{\mathbf{y}  }{\min}\;  \Vert \mathbf{y} \Vert - \mathbf{u}^\top   \mathbf{y} = -\infty $ (as we can always find $\Vert \mathbf{z} \Vert =1$ that makes $\mathbf{u}^\top   \mathbf{z} = \Vert \mathbf{u} \Vert_{*} $. Then choose $\mathbf{y}=t \mathbf{z}$, then $\Vert \mathbf{y} \Vert - \mathbf{u}^\top   \mathbf{y}=t-t\Vert \mathbf{u} \Vert_{*} < 0$ where $t$ can be arbitrarily large.)
    
      
    
    therefore Lagrangian dual problem is 
    $$
    \underset{\mathbf{u}  }{\max} \;  \mathbf{u}^\top   \mathbf{x} \quad \text{subject to } \; \Vert \mathbf{u} \Vert_{*} \leq 1
    $$
  
    whose optimal value $g^\star$ is $\Vert \mathbf{x} \Vert_{**}$. And by strong duality, we have $f^\star=g^\star$





### Conjugate function



Given a function $f \colon \, \mathbb{R}^n \to \mathbb{R}$, we define its conjugate $f^∗ \colon \, \mathbb{R}^n \to \mathbb{R}$,
$$
f^*(\mathbf{y})=\underset{ \mathbf{x} }{\max}\;  \mathbf{y}^\top\mathbf{x} -f(\mathbf{x})
$$


Conjugates appear frequently in dual programs, since
$$
-f^*(\mathbf{y})=\underset{ \mathbf{x} }{\min}\; f(\mathbf{x}) - \mathbf{y}^\top\mathbf{x}
$$


Properties and examples:

- Conjugate $f^∗$ is always convex regardless of convexity of $f$ 

  - $\mathbf{y}^\top\mathbf{x} -f(\mathbf{x})$ is convex in $\mathbf{y}$ and pointwise maximization 

- Fenchel's inequality: for any $\mathbf{x}, \mathbf{y}$, $f^*(\mathbf{y}) + f(\mathbf{x}) \geq  \mathbf{y}^\top\mathbf{x} $, because $f^*(\mathbf{y}) \geq  \mathbf{y}^\top\mathbf{x} -f(\mathbf{x})$.

- Conjugate of conjugate $f^{**}$ satisfies $f^{**}\leq f$

  - $f^{**}(\mathbf{x})=\underset{ \mathbf{z} }{\max}\;  \mathbf{x}^\top\mathbf{z} -f^*(\mathbf{z}) \leq \underset{ \mathbf{z} }{\max} \; f(\mathbf{x}) =f(\mathbf{x})  $. The inequality follows from Fenchel's inequality

- When $f$ is a quadratic in $Q  \succ 0$, $f^*$ is a quadratic in $Q^{−1}$

- When $f$ is a norm, $f^*$ is indicator of the dual norm unit ball

- If $f$ is closed and convex, then $f^{**}=f$

- If $f$ is closed and convex, then for any $\mathbf{x}, \mathbf{y}$, $\mathbf{x} \in \part f^∗(\mathbf{y})  \Longleftrightarrow  \mathbf{y} \in \part f(\mathbf{x})  \Longleftrightarrow \mathbf{x} \in \underset{ \mathbf{z} }{\arg\min}\; f(\mathbf{z}) - \mathbf{y}^\top\mathbf{z}$ (i.e., $f^*(\mathbf{y}) + f(\mathbf{x}) = \mathbf{y}^\top\mathbf{x} $) 

  - $\mathbf{y} \in \part f(\mathbf{x})   \Longleftrightarrow f(\mathbf{z}) \geq f (\mathbf{x}) + \mathbf{y}^\top(\mathbf{z}-\mathbf{x})$ for all $\mathbf{z}$. Equivalenty, it means  $ \mathbf{y}^\top \mathbf{x} -f(\mathbf{x}) \geq   \mathbf{y}^\top  \mathbf{z} -f(\mathbf{z})$ for all $\mathbf{z}$. So $\mathbf{x} \in \underset{ \mathbf{z} }{\arg\min}\; f(\mathbf{z}) - \mathbf{y}^\top\mathbf{z}$
  - $\mathbf{x} \in \part f^∗(\mathbf{y})   \Longleftrightarrow f^∗(\mathbf{z}) \geq f^∗(\mathbf{y}) + \mathbf{x}^\top(\mathbf{z}-\mathbf{y})$ for all $\mathbf{z}$. Equivalenty, it means  $ \mathbf{x}^\top \mathbf{y} -f^∗(\mathbf{y}) \geq   \mathbf{x}^\top  \mathbf{z} -f^∗(\mathbf{z})$ for all $\mathbf{z}$. So by definition, $ f^{**}(\mathbf{x}) = \mathbf{x}^\top \mathbf{y} -f^∗(\mathbf{y}) = f(\mathbf{x})$
  
  - Because $\mathbf{x}$ is a maximizer of $f^∗(\mathbf{y}) = \underset{ \mathbf{z} }{\max}\;  \mathbf{y}^\top\mathbf{z} -f(\mathbf{z})  $ and $\part f^∗(\mathbf{y}) = \text{closure} \Bigg\{ \text{conv}\left( \underset{\mathbf{z}\colon f^∗(\mathbf{y})= \mathbf{y}^\top\mathbf{z} -f(\mathbf{z})}{\bigcup}  \mathbf{z} \right ) \Bigg\}$. So $\mathbf{x} \in \part f^∗(\mathbf{y}) $ 
  - If $f$ is convex ($f^*$ is always convex) and both $f, f^*$ differentiable, the two subdifferentials $\part f$  and $\part f^*$ contain only 1 element, i.e., $\mathbf{x} = \nabla f^∗(\mathbf{y})  \Longleftrightarrow  \mathbf{y} = \nabla f(\mathbf{x}) $. So $\mathbf{x} = \nabla  f^∗\left(\nabla f(\mathbf{x})\right) $; that is $\nabla  f^∗ = \left(\nabla f\right)^{-1}$



- If $f$ is strictly convex, then the minimizer $\underset{ \mathbf{z} }{\arg\min}\; f(\mathbf{z}) - \mathbf{y}^\top\mathbf{z}$ is unique, $\part f^∗(\mathbf{y})$ contains a unique element, and $f^∗(\mathbf{y})$ is differentiable at $\mathbf{y}$. So put all together $ \nabla f^*(y) = \underset{ \mathbf{z} }{\arg\min}\; f(\mathbf{z}) - \mathbf{y}^\top\mathbf{z}$

  



Relationship to duality (also called **Fenchel duality**):

Primal: $\underset{\mathbf{x}  }{\min} \; f(\mathbf{x}) + g(\mathbf{x}) $

Dual: $\underset{\mathbf{u}  }{\max} \; -f^*(\mathbf{u}) - g^*(\mathbf{-u}) $



Consider the problem
$$
\underset{\mathbf{x}  }{\min} \; f( \mathbf{x})       \quad \text{subject to}\quad  A\mathbf{x}=\mathbf{b}
$$

$$
- \underset{\mathbf{x}  }{\max} \;   - f( \mathbf{x})  -\mathbf{u}^\top (A\mathbf{x}-\mathbf{b}) = - \underset{\mathbf{x}  }{\max} \;  -\mathbf{u}^\top A \mathbf{x} - f( \mathbf{x})  + \mathbf{u}^\top \mathbf{b} = -f^*( -A^\top \mathbf{u} ) - \mathbf{u}^\top \mathbf{b}
$$

So its dual problem is $\underset{\mathbf{u}  }{\max} \; -f^*( -A^\top \mathbf{u} ) - \mathbf{u}^\top \mathbf{b}$






## Newton's method



Given ***unconstrained, smooth convex optimization*** $f^*(\mathbf{x})=\underset{ \mathbf{x} }{\min}\;  f(\mathbf{x})$, where $f$ is convex, twice differentable, and $\text{dom}(f) =\mathbb{R}^n$.

Recall that gradient descent chooses initial $x^{(0)} \in \mathbb{R}^n$, and repeats
$$
\mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}-t_k \nabla f(\mathbf{x}^{(k-1)})  \quad k=1,2,\dots
$$
In comparison, Newton’s method repeats
$$
\mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}- \left(\nabla^2  f(\mathbf{x}^{(k-1)})\right)^{-1} \nabla f(\mathbf{x}^{(k-1)})  \quad k=1,2,\dots
$$
Here $\nabla^2  f(\mathbf{x}^{(k-1)})$ is the Hessian matrix of $f$ at $\mathbf{x}^{(k-1)}$



Recall, to get the gradient descent step at $\mathbf{x}$ , we minimize the quadratic approximation (over $\mathbf{y}$):
$$
f(\mathbf{y}) \approx  f(\mathbf{x}) + \nabla f(\mathbf{x})^\top(\mathbf{y}-\mathbf{x})   +  \frac{1}{2t}\Vert \mathbf{y}-\mathbf{x}\Vert_2^2
$$
Newton’s method uses in a sense a better quadratic approximation:
$$
f(\mathbf{y}) \approx  f(\mathbf{x}) + \nabla f(\mathbf{x})^\top(\mathbf{y}-\mathbf{x})   +  \frac{1}{2} ( \mathbf{y}-\mathbf{x})^\top \nabla^2 f(\mathbf{x})( \mathbf{y}-\mathbf{x})
$$





#### Affine invariance of Newton's method

Given $f$, nonsingular square $A ∈ \mathbb{R}^{n×n}$. 

Let $\mathbf{x} = A \mathbf{y}$, and $g(\mathbf{y}) = f(A\mathbf{y})$ ($\nabla g(\mathbf{y})=\nabla f(\mathbf{x}) \frac{d (A\mathbf{y})}{d\mathbf{y}}$ ???). Newton steps on $g$ are
$$
\mathbf{y}^+ = \mathbf{y}- \left(\nabla^2  g(\mathbf{y})\right)^{-1} \nabla g(\mathbf{y}) \\
= \mathbf{y}- \left( A^\top \nabla^2  f(A\mathbf{y}) A\right)^{-1} A^\top \nabla f(\mathbf{Ay}) = \mathbf{y}- A^{-1} \left(  \nabla^2  f(A\mathbf{y})  \right)^{-1} \nabla f(\mathbf{Ay})
$$
Hence
$$
A \mathbf{y}^+ = A\mathbf{y}-   \left(  \nabla^2  f(A\mathbf{y})  \right)^{-1} \nabla f(\mathbf{Ay})
$$
i.e.,
$$
\mathbf{x}^+ = \mathbf{x}-   \left(  \nabla^2  f(\mathbf{x})  \right)^{-1} \nabla f(\mathbf{x})
$$
So progress is independent of problem scaling. 

- This is not true of gradient descent!





Newton’s method: choose initial  $x^{(0)} \in \mathbb{R}^n$, repeat
$$
\mathbf{x}^{(k)} \gets \mathbf{x}^{(k-1)}- t_k\left(\nabla^2  f(\mathbf{x}^{(k-1)})\right)^{-1} \nabla f(\mathbf{x}^{(k-1)})  \quad k=1,2,\dots
$$


Step sizes $t_k$ chosen by backtracking line search



If $\nabla f $ Lipschitz, $f$ strongly convex, $\nabla^2  f(\mathbf{x})$ Lipschitz, then Newton’s method has a local convergence rate $O(\log \log(1/\epsilon))$



#### Equality-constrained Newton's method

Consider now a problem with equality constraints, as in
$$
\underset{\mathbf{x}  }{\min}\;  f( \mathbf{x)}  \quad \text{subject to } \; A\mathbf{x} = \mathbf{b}
$$
Several options:

- Eliminating equality constraints: write $\mathbf{x} = F\mathbf{y} +  \mathbf{x} _0$, where $F$ spans  $\mathscr{N}(A)$, and $A\mathbf{x} _0 = \mathbf{b}$. Solve in terms of $\mathbf{y}$
- Deriving the dual: can check that the Lagrange dual function is  $ -f^*(-A^\top \mathbf{v}) - \mathbf{b}^\top \mathbf{v}$, and strong duality holds. With luck, we can express $\mathbf{x}^\star$ in terms of $\mathbf{v}^\star$.
- Equality-constrained Newton: in many cases, this is the most straightforward option





In equality-constrained Newton’s method, we start with $x^{(0)} \in \mathbb{R}^n$ such that $A \mathbf{x}^{(0)}  = \mathbf{b}$. Then we repeat the updates
$$
\mathbf{x}^+=  \mathbf{x} +t\, \Delta\mathbf{x}
$$
where $\Delta\mathbf{x}= \mathbf{z}-\mathbf{x}$ and $\mathbf{z}$ solves
$$
\underset{\mathbf{z}  }{ \min}\;   \nabla f(\mathbf{x})^\top(\mathbf{z}-\mathbf{x})   +  \frac{1}{2} ( \mathbf{z}-\mathbf{x})^\top \nabla^2 f(\mathbf{x})( \mathbf{z}-\mathbf{x})   \quad \text{subject to } \; A\mathbf{z} = \mathbf{b}
$$
This keeps $\mathbf{x}^+$ in feasible set, since $A\mathbf{x}^+ = A\mathbf{x} + t\, A \,\Delta\mathbf{x} = \mathbf{b} + \mathbf{0} =  \mathbf{b} $

So Newton direction $ \Delta\mathbf{x}$ is the solution to minimizing a quadratic subject to equality constraints: 
$$
\underset{ \Delta\mathbf{x}  }{ \min}\;   \nabla f(\mathbf{x})^\top  \Delta\mathbf{x}  +  \frac{1}{2}  \Delta\mathbf{x}^\top \,\nabla^2 f(\mathbf{x}) \,\Delta\mathbf{x}  \quad \text{subject to } \; A\, \Delta\mathbf{x} = \mathbf{0}
$$
From KKT conditions, $ \Delta\mathbf{x}$ satisfies
$$
\nabla f(\mathbf{x})  +    \nabla^2 f(\mathbf{x})\, \Delta\mathbf{x} +  A^\top \mathbf{w} =0 \\
A\, \Delta\mathbf{x}= \mathbf{0}
$$
Rearranging them into a matrix form:
$$
\left[ \begin{matrix}   \nabla^2 f(\mathbf{x}) &  A^\top   \\ A &  \mathbf{0} \end{matrix}\right]\left[ \begin{matrix} \Delta\mathbf{x} \\\mathbf{w} \end{matrix}\right]=\left[ \begin{matrix} -\nabla f(\mathbf{x}) \\ \mathbf{0} \end{matrix}\right]
$$
for some $\mathbf{w}$. Hence Newton direction $ \Delta\mathbf{x}$ is again given by solving a linear system in the Hessian (albeit a bigger one).

- For unconstrained one, Newton direction $  \Delta\mathbf{x}$ solves $\nabla^2 f(\mathbf{x})\,  \Delta\mathbf{x} = -\nabla f(\mathbf{x}) $.



#### Downsides of Newton's method:

- Can only handle equality constraints. 
  - Barrier Method can address this
- Requires solving systems in Hessian. 
  - Compute Hession $ \nabla^2 f(\mathbf{x})$
  - Solve the system $\nabla^2 f(\mathbf{x})\,  \Delta\mathbf{x} = -\nabla f(\mathbf{x})$
  - Each of these 2 steps could be expensive
  - Quasi-Newton can address this





### Barrier Method 





##### Log barrier function
Consider the f convex optimization problem
$$
\underset{\mathbf{x}  }{\min} \; f( \mathbf{x})       \\
\text{subject to } \; \quad \quad \; h_i(\mathbf{x}) \leq 0  \quad  i=1,\dots, m \\
 A\mathbf{x} = \mathbf{b}
$$
Assume that $f, \, h_1, \, \dots , \, h_m$ are convex, twice differentiable, each with domain $\mathbb{R}^n$.

Ignoring equality constraints for now, our problem can be written as
$$
\underset{\mathbf{x}  }{\min} \; f( \mathbf{x})+ \sum_{i=1}^{m}I_{\{h_i(\mathbf{x}) \leq 0\} }(\mathbf{x})
$$
We can approximate the sum of indicators by the following function
$$
\phi(\mathbf{x}) =-\sum_{i=1}^{m} \log\left(-h_i(\mathbf{x})\right)
$$
is called the **log barrier** for the above problem. Its domain is the set of strictly feasible points, $\{\mathbf{x} \colon \; h_i(\mathbf{x} ) < 0, \, i = 1, \, \dots , \, m\}$, which we assume is nonempty. (Note this implies **strong duality** holds). Accordingly, we have


$$
\underset{\mathbf{x}  }{\min} \; f( \mathbf{x}) - \frac{1}{t} \sum_{i=1}^{m} \log\left(-h_i(\mathbf{x})\right)
$$
where $t>0$ is a large number.

This approximation is more accurate for larger $t$. But for any value of $t$, the log barrier approaches $\infty$ if any $h_i(\mathbf{x}) \to 0$



The gradient of the **log barrier** function $\phi(\mathbf{x}) =-\sum_{i=1}^{m} \log\left(-h_i(\mathbf{x})\right)$ is
$$
\nabla\phi(\mathbf{x}) =-\sum_{i=1}^{m}  \frac{1}{h_i(\mathbf{x})} \nabla h_i(\mathbf{x})
$$
and its Hessian is
$$
\nabla^2 \phi(\mathbf{x}) = \sum_{i=1}^{m}  \frac{1}{h_i(\mathbf{x})^2} \nabla h_i(\mathbf{x})  \nabla h_i(\mathbf{x})^\top-\sum_{i=1}^{m}  \frac{1}{h_i(\mathbf{x})} \nabla^2 h_i(\mathbf{x})
$$

##### Central path

Consider barrier problem:
$$
\underset{\mathbf{x}  }{\min} \; tf( \mathbf{x}) +\phi(\mathbf{x}) \\
\text{subject to } \quad
 A\mathbf{x} = \mathbf{b}
$$


The central path is defined by solution $\mathbf{x}^\star(t)$ with respect to $t > 0$

- Hope is that, as $t \to \infty$, we will have $\mathbf{x}^\star(t) \to \mathbf{x}$, solution to our original problem
- Why don’t we just set t to be some huge value, and solve the above problem? Directly seek solution at end of central path?
  - Problem is that this is seriously inefficient in practice
  - Much more efficient to traverse the central path, as we will see



Central path is characterized by its KKT conditions:
$$
t\nabla f( \mathbf{x}^\star(t))-\sum_{i=1}^{m}  \frac{1}{h_i(\mathbf{x}^\star(t))} \nabla h_i(\mathbf{x}^\star(t)) +  A^\top \mathbf{w} =0\\
 A\,\mathbf{x} ^\star(t)= \mathbf{b}, \quad  h_i(\mathbf{x}^\star(t)) < 0, \, i = 1, \, \dots , \, m
$$
for some $\mathbf{w} \in \mathbb{R}^m$ (But we don't really care about dual variable for barrier problem)

From central path points, we can derive feasible dual points for our original problem. 

Given $\mathbf{x} ^\star(t)$ and corresponding $\mathbf{w} $ we define
$$
u_i ^\star(t) =-\frac{1}{t\,h_i(\mathbf{x} ^\star(t))}, \, i = 1, \, \dots , \, m ,\quad   \mathbf{v} ^\star(t) =\frac{\mathbf{w} }t
$$
$\mathbf{u} ^\star(t)$ and $ \mathbf{v} ^\star(t)$ are dual feasible for original problem, whose Lagrangian is

$$
\mathcal{L}(\mathbf{x},\mathbf{u},\mathbf{v}) = f(\mathbf{x}) + \sum_{i=1}^{m} u_ih_i(\mathbf{x})+ \mathbf{v}^\top (A\mathbf{x}-\mathbf{b})
$$

- Note that $u_i ^\star(t)> 0$ since $h_i(\mathbf{x} ^\star(t)) < 0$ for all $i = 1,\,\dots , \, m$.

- Further, the point $\left(\mathbf{u} ^\star(t),  \mathbf{v} ^\star(t)\right)$ lies in domain of Lagrange dual function $g(\mathbf{u}, \mathbf{v})$, since by definition
  $$
  \nabla f( \mathbf{x}^\star(t)) + \sum_{i=1}^{m}  u_i ^\star(t) \nabla h_i(\mathbf{x}^\star(t)) +  A^\top  \mathbf{v} ^\star(t) \\
  = \nabla f( \mathbf{x}^\star(t)) - \sum_{i=1}^{m}  \frac{1}{t\,h_i(\mathbf{x} ^\star(t))} \nabla h_i(\mathbf{x}^\star(t)) +  A^\top  \frac{\mathbf{w} }t = 0
  $$
  The last equality follows the stationarity condition of the central path.

  That is, $\mathbf{x} ^\star(t)$ minimizes $\mathcal{L}(\mathbf{x},\mathbf{u}^\star,\mathbf{v}^\star)$ over $\mathbf{x}$. So $g\left(\mathbf{u} ^\star(t),  \mathbf{v} ^\star(t)\right) > -\infty$.

  



We compute
$$
g\left(\mathbf{u} ^\star(t),  \mathbf{v} ^\star(t)\right) = f\left(\mathbf{x} ^\star(t)\right) + \sum_{i=1}^{m} u_i^\star(t)\, h_i(\mathbf{x} ^\star(t))+ \mathbf{v}^\star(t) ^\top \left(A \mathbf{x} ^\star(t)-\mathbf{b}\right) = f\left(\mathbf{x} ^\star(t)\right) -m/t
$$
It shows that $m/t=f\left(\mathbf{x} ^\star(t)\right) -g\left(\mathbf{u} ^\star(t),  \mathbf{v} ^\star(t)\right)$ is a duality gap. 

This allows us to bound suboptimality of $f\left(\mathbf{x} ^\star(t)\right)$, with respect to original problem (the optimal value $f^\star$), via the duality gap $m/t$. I.e., $ f\left(\mathbf{x} ^\star(t)\right) - f^\star \leq  m/t$.

- This will be very useful as a stopping criterion;
- It also confirms the hope that $\mathbf{x} ^\star(t) \to x^\star$ as $ t \to \infty$.





##### Perturbed KKT conditions

We can motivate barrier method iterates $(\mathbf{x} ^\star(t),\mathbf{u} ^\star(t),  \mathbf{v} ^\star(t))$ in terms of the perturbed KKT conditions:
$$
\nabla f( \mathbf{x}) + \sum_{i=1}^{m}  u_i  \nabla h_i(\mathbf{x}) +  A^\top  \mathbf{v}  =0 \\
u_i   \,h_i(\mathbf{x}) =-\frac{1}{t} , \; h_i(\mathbf{x})\leq0 , \; u_i \geq 0, \; i=1,\dots,m \\
A\mathbf{x} = \mathbf{b}
$$


Only difference between these and actual KKT conditions for our original problem is complementary slackness, $u_i   \,h_i(\mathbf{x}) =0$, being replaced by $u_i   \,h_i(\mathbf{x}) =-\frac{1}{t}$.







### Barrier method

The barrier method solves a sequence of problems
$$
\underset{\mathbf{x}  }{\min} \; tf( \mathbf{x}) +\phi(\mathbf{x}) \\
\text{subject to } \quad
 A\mathbf{x} = \mathbf{b}
$$

for increasing values of $t > 0$, until duality gap satisfies $m/t \leq \epsilon$ .



Fix $t^{(0)} > 0$, $\mu > 1$. We use Newton to compute $ \mathbf{x}^{(0)} = \mathbf{x}^\star\left(t\right)$, which is the solution to barrier problem at $t = t^{(0)}$. For $k = 1, \, , 2, \, \dots $ :

- A centering step (that brings $\mathbf{x}^{(k)}$ onto the central path): solve the barrier problem at $t = t^{(k)}$, using Newton initialized at $\mathbf{x}^{(k-1)}$, to yield $ \mathbf{x}^{(k)} = \mathbf{x}^\star\left(t\right)$ ($t = t^{(k)}$)
- Stop if $m/t \leq \epsilon$ , else update $t^{(k+1)} = \mu\, t^{(k)}$

Considerations:

- Choice of $\mu$ : if $\mu$ is too small, then many outer iterations might be needed; if $\mu$ is too big, then Newton’s method (each centering step) might take many iterations
- Choice of $t^{(0)}$: if $t^{(0)}$ is too small, then many outer iterations might be needed; if $t^{(0)}$ is too big, then the first Newton solve (first centering step) might require many iterations
- Fortunately, the performance of the barrier method is often quite robust to the choice of $\mu$ and  $t^{(0)}$ in practice
- However, note that the appropriate range for these parameters is scale dependent



#### Feasibility methods

We have implicitly assumed that we have a strictly feasible point for the first centering step, i.e., for computing $ \mathbf{x}^{(0)} = \mathbf{x}^\star\left(t\right)$, solution of barrier problem at $t^{(0)} > 0$. 

This is $\mathbf{x}$ such that $h_i(\mathbf{x} ) < 0, \, i = 1, \, \dots , \, m$, $A \mathbf{x} = \mathbf{b}$

How to find such a feasible  $\mathbf{x}$? By solving
$$
\underset{\mathbf{x} ,\,s }{\min} \; s       \\
\text{subject to } \; \quad \quad \; h_i(\mathbf{x}) \leq s  \quad  i=1,\dots, m \\
 A\mathbf{x} = \mathbf{b}
$$
The goal is for $s$ to be negative at the solution. This is known as a feasibility method. 

We can apply the barrier method to the above problem, since it is easy to find a strictly feasible starting point

- Note that we do not need to solve this problem to high accuracy. Once we find a feasible $(\mathbf{x}, s)$ with $s < 0$, we can terminate early





##### Perturbed KKT as nonlinear system


$$
\nabla f( \mathbf{x}) + \sum_{i=1}^{m}  u_i  \nabla h_i(\mathbf{x}) +  A^\top  \mathbf{v}  =0 \\


u_i   \,h_i(\mathbf{x}) =-\frac{1}{t} , \; h_i(\mathbf{x})\leq0 , \; u_i \geq 0, \; i=1,\dots,m \\
A\mathbf{x} = \mathbf{b}
$$
Can view this as a nonlinear system of equations, written as
$$
\mathbf{r}(\mathbf{x},\mathbf{u},\mathbf{v})=\left[ \begin{matrix} \nabla f( \mathbf{x}) +     D \mathbf{h}(\mathbf{x})^\top  \,\mathbf{u}+  A^\top  \mathbf{v} \\ -\text{diag}(\mathbf{u}) \, \mathbf{h}(\mathbf{x}) -\frac{1}{t} \mathbf{1} \\ A\mathbf{x} - \mathbf{b} 
\end{matrix} \right] = \mathbf{0}
$$
where $\mathbf{h}(\mathbf{x}) = \left[ \begin{matrix}  h_1(\mathbf{x})  \\ \vdots\\   h_m(\mathbf{x})   \end{matrix} \right]$ and $ D \mathbf{h}(\mathbf{x}) = \left[ \begin{matrix} \nabla h_1(\mathbf{x})^\top \\ \vdots\\ \nabla h_m(\mathbf{x})^\top  \end{matrix} \right] $

Recall Newton’s method is generally a root-finder for a nonlinear system $F(\mathbf{y}) = \mathbf{0}$. 

Approximating $ \mathbf{0} = F(\mathbf{y}+ \Delta \mathbf{y}) \approx  F(\mathbf{y}) + DF(\mathbf{y}) \,\Delta y$ leads to $\Delta y =   - \left(DF(\mathbf{y}) \right)^{-1} F(\mathbf{y})$



Approach 1 (v1): 

from middle equation (relaxed comp slackness), note that $u_i  =-\frac{1}{t\,h_i(\mathbf{x} )}, \; i = 1,\, \dots ,\, m$. 

So after eliminating $\mathbf{u}$, we get
$$
\mathbf{r}(\mathbf{x},\mathbf{v})=\left[ \begin{matrix} \nabla f( \mathbf{x}) -      \sum_{i=1}^{m}  \frac{1}{t\,h_i(\mathbf{x} )}  \nabla h_i(\mathbf{x})  +  A^\top  \mathbf{v}  \\ A\mathbf{x} - \mathbf{b} 
\end{matrix} \right] = \mathbf{0}
$$

Thus the Newton root-finding update $( \Delta \mathbf{x} , \Delta \mathbf{v})$ is determined by

$$
\nabla f( \mathbf{x}) -      \sum_{i=1}^{m}  \frac{1}{t\,h_i(\mathbf{x} )}  \nabla h_i(\mathbf{x})  +  A^\top  \mathbf{v} + \left(\nabla^2 f( \mathbf{x}) +  \sum_{i=1}^{m}  \frac{1}{t\,h_i(\mathbf{x} )^2}  \nabla h_i(\mathbf{x})\nabla h_i(\mathbf{x})^\top  - \sum_{i=1}^{m}  \frac{1}{t\,h_i(\mathbf{x} )}  \nabla^2 h_i(\mathbf{x}) \right)\Delta  \mathbf{x} +  A^\top \Delta  \mathbf{v} = \mathbf{0} \\
A \mathbf{x} - \mathbf{b} + A   \Delta \mathbf{x} = \mathbf{0}
$$
Rearraigning it into a matrix form:

$$
\left[ \begin{matrix} H_{\text{bar}}(\mathbf{x}) & A^\top  \\ A   &  \mathbf{0}
\end{matrix} \right] \left[ \begin{matrix} \Delta \mathbf{x} \\ \Delta \mathbf{v} \end{matrix} \right] =- \mathbf{r}(\mathbf{x},\mathbf{v})
$$

where $H_{\text{bar}}(\mathbf{x}) = \nabla^2 f( \mathbf{x}) +  \sum_{i=1}^{m}  \frac{1}{t\,h_i(\mathbf{x} )^2}  \nabla h_i(\mathbf{x})\nabla h_i(\mathbf{x})^\top  - \sum_{i=1}^{m}  \frac{1}{t\,h_i(\mathbf{x} )}  \nabla^2 h_i(\mathbf{x})$

This is just the KKT system solved by one iteration of Newton's method for minimizing the barrier problem



Approach 2 (v2): 

directly apply Newton root-finding update, without eliminating  $\mathbf{u}$. Introduce notation
$$
\mathbf{r}_{\text{dual}} = \nabla f( \mathbf{x}) +     D \mathbf{h}(\mathbf{x})^\top  \,\mathbf{u}+  A^\top  \mathbf{v} \\
\mathbf{r}_{\text{cent}} = -\text{diag}(\mathbf{u}) \, \mathbf{h}(\mathbf{x}) -\frac{1}{t} \mathbf{1}  \\
\mathbf{r}_{\text{prim}} =  A\mathbf{x} - \mathbf{b}
$$
called the dual, central, and primal residuals at $\mathbf{y} = (\mathbf{x},\mathbf{u},\mathbf{v})$. Now root-finding update $\Delta\mathbf{y} = (\Delta \mathbf{x},\Delta\mathbf{u},\Delta\mathbf{v}) $ is given by
$$
\left[ \begin{matrix} H_{\text{pd}}(\mathbf{x}) & D \mathbf{h}(\mathbf{x})^\top  & A^\top \\  -\text{diag}(\mathbf{u}) \,D \mathbf{h}(\mathbf{x})&  -\text{diag}\left(\mathbf{h}(\mathbf{x})\right) & \mathbf{0} \\ A & \mathbf{0}  &  \mathbf{0}
\end{matrix} \right] \left[ \begin{matrix} \Delta \mathbf{x}\\ \Delta \mathbf{u} \\ \Delta \mathbf{v} \end{matrix} \right] =- \left[ \begin{matrix}  \mathbf{r}_{\text{dual}}  \\ \mathbf{r}_{\text{cent}}  \\ \mathbf{r}_{\text{prim}}  \end{matrix} \right]
$$
where $H_{\text{pd}}(\mathbf{x}) =  \nabla^2 f( \mathbf{x}) + \sum_{i=1}^m u_i\nabla^2 h_i(\mathbf{x})$



Some notes:

- In v2, update directions for the primal and dual variables are inexorably linked together
- Also, v2 and v1 leads to different (nonequivalent) updates
- As we saw, one iteration of v1 is equivalent to inner iteration in the barrier method
- And v2 defines a new method called primal-dual interior-point method
- One complication: in v2, the dual iterates are not necessarily feasible for the original dual problem ...





#####  Surrogate duality gap

For barrier method, we have simple duality gap: $m/t$, since we set $u_i = - \frac{1}{t\, h_i(\mathbf{x}))}, \, i = 1, \, \dots , \, m$ and saw this was dual feasible

For primal-dual interior-point method, we can construct **surrogate duality gap**:
$$
\eta = −h(\mathbf{x})^\top \mathbf{u} = −\sum^m_{i=1} u_ih_i(\mathbf{x})
$$
This would be a bonafide duality gap if we had feasible points $(\tilde{\mathbf{x}},\tilde{\mathbf{u}},\tilde{\mathbf{v}} )$, i.e.,

- $\mathbf{r}_{\text{dual}} =\mathbf{ 0}$; that is, $ \nabla f(  \tilde{\mathbf{x}} ) +     D \mathbf{h}(\tilde{\mathbf{x}})^\top  \,\tilde{\mathbf{u}}+  A^\top  \tilde{\mathbf{v}} =\mathbf{ 0}$
- $ \mathbf{r}_{\text{prim}} =  A \tilde{\mathbf{x}} - \mathbf{b}= \mathbf{0}$.

If so, consequently, we have 
$$
g\left(\tilde{\mathbf{u}}  ,  \tilde{\mathbf{v}}  \right) = f\left(\tilde{\mathbf{x}}  \right) + \sum_{i=1}^{m} \tilde{u}_i \, h_i(\tilde{\mathbf{x}} )+ \tilde{\mathbf{v}} ^\top \left(A \tilde{\mathbf{x}}  -\mathbf{b}\right) \\
f\left(\tilde{\mathbf{x}}  \right) -g\left(\tilde{\mathbf{u}}  ,  \tilde{\mathbf{v}}  \right)= −\sum^m_{i=1} \tilde{u}_ih_i(\tilde{\mathbf{x}} )
$$
the first equality follows $ g\left(\tilde{\mathbf{u}}  ,  \tilde{\mathbf{v}}  \right)  = \max_{\mathbf{x}} \; f\left(\mathbf{x}  \right) + \sum_{i=1}^{m} \tilde{u}_i \, h_i(\mathbf{x} )+ \tilde{\mathbf{v}} ^\top \left(A \mathbf{x}  -\mathbf{b}\right)$ and $\mathbf{r}_{\text{dual}} =\mathbf{ 0}$

but we don’t, so it’s not

What value of parameter $t$  in perturbed KKT conditions does this correspond to? This is $t = m/\eta$





#### Primal-dual interior-point method

Putting it all together, we now have our primal-dual interior-point method. 

Start with $\mathbf{x}^{(0)}$ such that $h_i\left(\mathbf{x}^{(0)}\right) < 0, i = 1, \, \dots ,\, m$, and $\mathbf{u}^{(0)} > 0, \,\mathbf{v} ^{(0)}$.  Define $\eta^{(0)} = −\mathbf{h}\left(\mathbf{x}^{(0)}\right) ^\top \mathbf{u}^{(0)}$. We fix $\mu > 1$, repeat for $k = 1, 2, 3 \dots$

- Define t = μm/η(k−1)
- Compute primal-dual update direction $\Delta\mathbf{y} = (\Delta \mathbf{x},\Delta\mathbf{u},\Delta\mathbf{v}) $
- Use backtracking to determine step size $s$
- Update $\mathbf{y} ^{(k)} = \mathbf{y} ^{(k-1)} + s \, \Delta\mathbf{y} $
- Compute $\eta^{(k)} = −\mathbf{h}\left(\mathbf{x}^{(k)}\right) ^\top \mathbf{u}^{(k)}$
- Stop if $\eta^{(k)} \leq \epsilon$  and $\left(\Vert \mathbf{r}_{\text{prim}} \Vert_2^2 +\Vert \mathbf{r}_{\text{dual}} \Vert_2^2 \right)^{1/2} \leq \epsilon$

Note that we stop based on surrogate duality gap, and approximate feasibility. (Line search maintains $h_i(\mathbf{x}) < 0, \, u_i > 0,\, i = 1, \,\dots ,\, m$, by choosing step size $s$)



#### Quasi-Newton methods

If the Hessian is too expensive (or singular), then a quasi-Newton method can be used to approximate $\nabla^2 f(\mathbf{x})$ with $H \succ 0$.

And we update according to
$$
\mathbf{x}^+=  \mathbf{x}  - t H^{-1}  \nabla f(\mathbf{x})
$$


- Approximate Hessian $H$ is recomputed at each step. Goal is to make  $H^{-1}$ cheap to apply (possibly, cheap storage too)
- Convergence is fast: ***superlinear***, but not the same as Newton. Roughly $n$ steps of quasi-Newton make same progress as one
  Newton step
- Very wide variety of quasi-Newton methods; common theme is to “propagate” computation of $H$ across iterations







## Coordinate descent



##### Coordinatewise optimality




We now focus on a very simple technique that can be surprisingly efficient, scalable: **coordinate descent**, or more appropriately called coordinatewise minimization

- Given convex, differentiable $f\colon \, \mathbb{R}^n \to \mathbb{R}$, if we are at a point $\mathbf{x}$ such that $f(\mathbf{x})$ is minimized along each coordinate axis, then we jave a global minimize. That is, $f(\mathbf{x} + \delta \mathbf{e}_i) ≥ f(\mathbf{x})$ for all $\delta, i \implies f(\mathbf{x} ) = \min_{\mathbf{z} } f(\mathbf{z} )$

- However, for $f$ convex and not differentiable, $f(\mathbf{x} + \delta \mathbf{e}_i) ≥ f(\mathbf{x})$ for all $\delta, i \nRightarrow  f(\mathbf{x} ) = \min_{\mathbf{z} } f(\mathbf{z} )$

- If $f(\mathbf{x} ) = g(\mathbf{x} ) + \sum_{i=1}^n h_i(x_i)$, with $g$ convex, smooth, and each $h_i$ convex (Here the nonsmooth part is called separable)

  Let $F_i(x_i ) = g(x_i,x_{-i} ) +h_i(x_i) $, 

  $f(\mathbf{x} + \delta \mathbf{e}_i) ≥ f(\mathbf{x})$ for all $\delta, i \implies  \mathbf{0}\in \part F_i(x_i ) \; \forall i \implies -\nabla_i g(\mathbf{x}) \in \part h_i(x_i ) \; \forall i\implies  h_i(y_i) \geq  h_i(x_i)   -\nabla_i g(\mathbf{x})(y_i-x_i) \; \forall i$
  $$
  f(\mathbf{y} )  - f(\mathbf{x} ) = g(\mathbf{y} )- g(\mathbf{x} ) + \sum_{i=1}^n h_i(y_i)  - \sum_{i=1}^n h_i(x_i)\\
  \geq g(\mathbf{y} )- g(\mathbf{x} )  -\nabla g(\mathbf{x})^\top(\mathbf{y} -\mathbf{x} ) \geq 0
  $$
  the last inequality comes from the convexity of $g$ (first-order characterization of a differentiable convex function).



### Coordinate descent

This suggests that for the problem
$$
\underset{\mathbf{x} }{\min} \;f(\mathbf{x} ) = g(\mathbf{x} ) + \sum_{i=1}^n h_i(x_i)
$$
with $g$ convex and differentiable and each $h_i$ convex, we can use coordinate descent: 

let  $\mathbf{x}^{(0)} \in \mathbb{R}^n$, and repeat
$$
x^{(k)}_i = \underset{x_i}{\arg \min} \; f\left(x^{(k)}_1, \, \dots , \, x^{(k)}_{i-1}, \, x_i, \, x^{(k−1)}_{i+1} ,\, \dots ,\, x^{(k−1)}_n \right), \quad i = 1,\, \dots , \, n \quad \text{ for } k = 1, 2,  \dots
$$
Important note: we always use most recent information possible

- Very simple and easy to implement
- Careful implementations can achieve state-of-the-art
- Scalable, e.g., don’t need to keep full data in memory





##### Example: linear regression

Given $\mathbf{y} \in \mathbb{R}^n$, and $ X \in \mathbb{R}^{n\times p}$ with columns $\mathbf{x}_1, \dots , \mathbf{x}_p$

consider the linear regression problem:
$$
\underset{\mathbf{x} }{\min} \; \frac{1}{2} \Vert \mathbf{y} - X \boldsymbol{\beta}\Vert^2_2
$$
Minimizing over $\beta_i$, with all $\beta_j , j \neq i$ fixed:
$$
0 = \nabla_ if(\boldsymbol{\beta}) = \mathbf{x}_i^\top \left(  X \boldsymbol{\beta}-\mathbf{y}\right)=\mathbf{x}_i^\top \left( \mathbf{x}_i \beta_i+  X_{-i} \boldsymbol{\beta}_{-i}-\mathbf{y} \right) \\
\beta_i=\frac{\mathbf{x}_i ^\top\left(  \mathbf{y} - X_{-i} \boldsymbol{\beta}_{-i} \right)}{\mathbf{x}_i ^\top\mathbf{x}_i }
$$
It does univariate regression of the partial residual $\mathbf{y} - X_{-i} \boldsymbol{\beta}_{-i} $ on $\mathbf{x}_i$

Coordinate descent repeats this update for $i = 1, 2, \dots , p, 1, 2, \dots$
Note that this is exactly Gauss-Seidl for the system $X^\top X \boldsymbol{\beta}= X^\top \mathbf{y}$ ???







#### Coordinate gradient descent

For a smooth function $f$, the iterations
$$
x^{(k)}_i = x^{(k-1)}_i - t_{ki} \, \nabla_if\left(x^{(k)}_1, \, \dots , \, x^{(k)}_{i-1}, \, x_i, \, x^{(k−1)}_{i+1} ,\, \dots ,\, x^{(k−1)}_n \right), \quad i = 1,\, \dots , \, n \quad \text{ for } k = 1, 2,  \dots
$$
are called coordinate gradient descent, and when $f = g + h$, with $g$ smooth and $h = \sum^n_{i=1} h_i $, the iterations
$$
x^{(k)}_i = \text{prox}_{h_i , t_{ki}} \left( x^{(k-1)}_i - t_{ki} \, \nabla_i g\left(x^{(k)}_1, \, \dots , \, x^{(k)}_{i-1}, \, x_i, \, x^{(k−1)}_{i+1} ,\, \dots ,\, x^{(k−1)}_n \right)\right), \quad i = 1,\, \dots , \, n \quad \text{ for } k = 1, 2,  \dots
$$
are called coordinate proximal gradient descent







## Dual first-order methods

Even if we can’t derive dual (conjugate) in closed form, we can still use dual-based gradient or subgradient methods

Consider the problem
$$
\underset{\mathbf{x}  }{\min} \; f( \mathbf{x})       \quad \text{subject to}\quad  A\mathbf{x}=\mathbf{b}
$$

$$
\underset{\mathbf{x}  }{\max} \;   - f( \mathbf{x})  -\mathbf{u}^\top (A\mathbf{x}-\mathbf{b}) = - \underset{\mathbf{x}  }{\max} \;  -\mathbf{u}^\top A \mathbf{x} - f( \mathbf{x})  + \mathbf{u}^\top \mathbf{b} = -f^*( -A^\top \mathbf{u} ) - \mathbf{u}^\top \mathbf{b}
$$



Its dual problem is 
$$
\underset{\mathbf{u}  }{\max} \; -f^*( -A^\top \mathbf{u} ) - \mathbf{u}^\top \mathbf{b}
$$
Let $g( \mathbf{u})= -f^*( -A^\top \mathbf{u} ) - \mathbf{u}^\top \mathbf{b}$. Note $\part g( \mathbf{u}) = A \, \part f^*( -A^\top \mathbf{u} ) - \mathbf{b}$ 



using what we know about conjugates ($\mathbf{x} \in \part f^∗(\mathbf{y})  \Longleftrightarrow \mathbf{x} \in \underset{ \mathbf{z} }{\arg\min}\; f(\mathbf{z}) - \mathbf{y}^\top\mathbf{z}$):
$$
\mathbf{x} \in \underset{\mathbf{z}  }{\min} \;    f( \mathbf{z})  - \left(- A^\top \mathbf{u} \right)^\top \mathbf{z} - \mathbf{u}^\top\mathbf{b}  \Longleftrightarrow \mathbf{x} \in \part f^*( -A^\top \mathbf{u} )
$$
So $A \mathbf{x} - \mathbf{b} \in \part g( \mathbf{u})  $ where $\mathbf{x} \in \underset{\mathbf{z}  }{\min} \;    f( \mathbf{z})  + \left( A^\top \mathbf{u} \right)^\top \mathbf{z} $



##### Dual subgradient method

The dual subgradient method (for maximizing the dual objective) starts with an initial dual guess $\mathbf{u}^{(0)} $, and repeats for $k = 1, 2, \dots$
$$
\mathbf{x}^{(k)} \in \underset{\mathbf{z}  }{\min} \;    f( \mathbf{z})  + \left(  \mathbf{u}^{(k-1)} \right)^\top A \,\mathbf{z}\\
\mathbf{u}^{(k)} =\mathbf{u}^{(k-1)} +t_k \, \left( A\, \mathbf{x}^{(k)} -\mathbf{b} \right)
$$
Note:

- updates to $\mathbf{u}$ move in the direction of subgradient rather than its opposite because the dual problem does maximization
- step size $t_k$ is chosen in the standard way



#### Dual gradient ascent

Recall that if $f$ is strictly convex, then $f^*$ is differentiable, and so this becomes dual gradient ascent, which repeats for $k = 1, 2, \dots$

$$
\mathbf{x}^{(k)} = \underset{\mathbf{z}  }{\min} \;    f( \mathbf{z})  + \left(  \mathbf{u}^{(k-1)} \right)^\top A \,\mathbf{z}\\
\mathbf{u}^{(k)} =\mathbf{u}^{(k-1)} +t_k \, \left( A\, \mathbf{x}^{(k)} -\mathbf{b} \right)
$$


Now each $\mathbf{x}^{(k)}$ is unique.  And proximal gradients and acceleration can be applied as they would usually



#### Dual decomposition

Consider
$$
\underset{\mathbf{x}  }{\min} \; f( \mathbf{x})       \quad \text{subject to}\quad  A\mathbf{x}=\mathbf{b}
$$


min
x
X
B
i=1
fi(xi) subject to Ax = b





## Augmented Lagrangian method
(also known as: method of multipliers)



Dual ascent disadvantage: convergence requires strong conditions.

Augmented Lagrangian method transforms the primal problem:

min
x

f(x) + ρ
2
kAx − bk
2
2

subject to Ax = b
