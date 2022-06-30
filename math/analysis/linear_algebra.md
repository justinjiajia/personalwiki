

Good introduction to vectors and linear algebra

https://www.youtube.com/watch?v=fNk_zzaMoSs&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&index=1

$$
A\quad =\quad \left[\begin{matrix} -  \mathbf{a}_1^\top - \\  \vdots \\  -  \mathbf{a} _m^\top -\end{matrix}\right]_{m\times n}
=\quad \left[\begin{matrix}   \mathbf{b} _1  \; \dots   \; \mathbf{b}_n \end{matrix}\right]_{m\times n} \quad (\mathbf{a}_i\in \mathbb{R}^n, \; \mathbf{b}_j\in \mathbb{R}^m).
$$

$$
A\mathbf{x}=x_1 \mathbf{b} _1  + \dots  +x_n \mathbf{b}_n
$$
is a linear combination of $\{\mathbf{b}_1,\dots,\mathbf{b}_n\}$

The system $A\mathbf{x} = \mathbf{y}$ is solvable $\Leftrightarrow $ $\mathbf{y}$ is in the column space of $A$



If $\{\mathbf{b}_1,\dots,\mathbf{b}_r\}$ are linearly independent, 

each of  $\{\mathbf{b}_{r+1},\dots,\mathbf{b}_n\}$ can be represented by a linear combination of  $\{\mathbf{b}_1,\dots,\mathbf{b}_r\}$.

For $\mathbf{b}_{j}$ in $\{\mathbf{b}_{r+1},\dots,\mathbf{b}_n\}$
$$
\lambda_{1j}\mathbf{b}_1 +\dots +  \lambda_{rj}\mathbf{b}_r -\mathbf{b}_{j} = \mathbf{0}\\
\left[\begin{matrix}   \mathbf{b} _1  \; \dots   \; \mathbf{b}_n \end{matrix}\right]_{m\times n}\left[\begin{matrix} \lambda_{1j} \\ \vdots \\ \lambda_{rj} \\ 0 \\\vdots \\  -1 \\\vdots \\0\end{matrix} \right] = \mathbf{0}
$$
 $\boldsymbol{\lambda}_j$'s (the number of which is $n-r$) are independent, and span $\mathscr{N}(A)$

When $A\mathbf{x} = \mathbf{y}$ is solvable, $\mathbf{y}$ can be represented as a unique linear combination of independent column vectors $\{\mathbf{b}_1,\dots,\mathbf{b}_r\}$ 

Denote it by $\boldsymbol\lambda_p$ and call it the particular solution

The complete solution can be written as $\mathbf{x}=  \boldsymbol\lambda_p + x_{r+1} \boldsymbol{\lambda}_{r+1}+\dots+ x_{n} \boldsymbol{\lambda}_{n}$, and its dimension is the same as that of  $\mathscr{N}(A)$.





When the system $A\mathbf{x} = \mathbf{y}$ has no solution,

multiply by $A^\top$ and solve $A^\top A\hat{\mathbf{x}}=A^\top \mathbf{y}$ for $\hat{\mathbf{x}}$


$$
\left[\begin{matrix} -  \mathbf{b}_1^\top - \\  \vdots \\  -  \mathbf{b} _n^\top -\end{matrix}\right]\left[\mathbf{y}-A\hat{\mathbf{x}}\right]=\mathbf{0}\\
$$
$A \hat{\mathbf{x}}=A\left(A^\top A\right)^{-1}A^\top \mathbf{y}$



$A \hat{\mathbf{x}}$ is a linear combination of $\{\mathbf{b}_1,\dots,\mathbf{b}_n\}$
$$
\left[\begin{matrix} -  \mathbf{b}_1^\top - \\  \vdots \\  -  \mathbf{b} _n^\top -\end{matrix}\right]\left[\mathbf{y}-A\hat{\mathbf{x}}\right]=A^\top \left[\mathbf{y}-A\hat{\mathbf{x}}\right]=\left[\begin{matrix}   \mathbf{a} _1  \; \dots   \; \mathbf{a}_m \end{matrix}\right]\left[\mathbf{y}-A\hat{\mathbf{x}}\right]=\mathbf{0}\\
$$

So $\mathbf{y}-A\hat{\mathbf{x}}$ is in the nullspace of $A^\top$.











$\mathbf{x} \in \mathbb{R}^n$  is a vector in the row space of $A$

$$
A\mathbf{x}=\left[\begin{matrix} -  \mathbf{a}_1^\top - \\  \vdots \\  -  \mathbf{a}_m^\top -\end{matrix}\right]\mathbf{x}=\left[\begin{matrix}  \mathbf{a}_1^\top \mathbf{x} \\  \vdots \\    \mathbf{a} _m^\top\mathbf{x}\end{matrix}\right]=\mathbf{0}
$$

Every $\mathbf{a}_i^\top \mathbf{x} =0$ gives a hyperplane of dimension $n-1$ that is normal to $\mathbf{a}_i$ 

So $\left[\begin{matrix}  \mathbf{a}_1^\top \mathbf{x} \\  \vdots \\    \mathbf{a} _m^\top\mathbf{x}\end{matrix}\right]=\mathbf{0}$ gives the intersection of these hyperplanes, which is the nullspace of $A$, $\mathscr{N}(A)$.

When $m\leq n$, If no two normal vectors, $\mathbf{a}_i$s, are parallel to each other (i.e., $ \{\mathbf{a}_1,\dots,\mathbf{a}_m\}$ are independent), $\dim \mathscr{N}(A) = n-m$

In particular, when $m=n$, $\mathscr{N}(A)=\mathbf{0}$, as the intersection of those hyperplanes contains $\mathbf{0}$ only.

$\mathbf{x} \notin \mathscr{N}(A)$ is transformed to a new vector $\mathbf{y}\in \mathbb{R}^m$ in the column space of $A$



 $\left[\begin{matrix}  \mathbf{a}_1^\top \mathbf{x} \\  \vdots \\    \mathbf{a} _m^\top\mathbf{x}\end{matrix}\right]=\mathbf{0}$ shows that the row space spanned by  $ \{\mathbf{a}_1,\dots,\mathbf{a}_m\}$ is perpendicular to $\mathscr{N}(A)$

$\mathscr{N}(A)$ is the orthogonal complement of the the row space of $A$ 

that is, if there's $r$ independent row vectors, the dimension of the row space is $r$, while $\dim \mathscr{N}(A) = n-r$

For any $\mathbf{ x} $ in the row space of $A$
$$
\left[\begin{matrix}  \mathbf{a}_1^\top \mathbf{x} \\  \vdots \\    \mathbf{a} _r^\top\mathbf{x}\end{matrix}\right] =\left[\begin{matrix}  \Vert \mathbf{a}_1 \Vert\Vert \mathbf{x}\Vert \cos\theta_1 \\  \vdots \\      \Vert \mathbf{a}_r \Vert\Vert \mathbf{x}\Vert \cos\theta_m \end{matrix}\right]
$$


Each $\mathbf{ x} \in \mathbb{R}^n$ has a unique representation of the form
$$
\mathbf{ x} =\mathbf{ x}_1+\mathbf{ x}_2
$$
where $\mathbf{ x}_1 $ in the row space of $A$, $\mathbf{ x}_2 \in \mathscr{N}(A)$.
$$
\mathbf{ x} =\mathbf{ x}_1+\mathbf{ x}_2
$$


If the system $A\mathbf{x} = \mathbf{y}$ is solvable and has a solution $\mathbf{ x}_1 $ in the row space of $A$

$\mathbf{ x}_1+\mathbf{ x}_2$ where $\mathbf{ x}_2 \in \mathscr{N}(A)$ also solves the system





#### Cramer's Rule

When $m=n$,  and $\det A \neq 0$, 

Observe
$$
A\left[\begin{matrix} x_1  && 0 && \dots && 0 \\  x_2 && 1 && \dots && 0 \\  \vdots &&\vdots && \ddots&& \vdots \\ x_n  && 0 && \dots && 1 \end{matrix}\right] = \left[\begin{matrix}   \mathbf{y} , \;   \mathbf{b}_2 ,   \; \dots ,  \; \mathbf{b}_n \end{matrix}\right]
$$
and $(\det A) \det \left[\begin{matrix} x_1  && 0 && \dots && 0 \\  x_2 && 1 && \dots && 0 \\  \vdots &&\vdots && \ddots&& \vdots \\ x_n  && 0 && \dots && 1 \end{matrix}\right]  = x_1  \det A = \det  \left[\begin{matrix}   \mathbf{y} , \;   \mathbf{b}_2 ,   \; \dots ,  \; \mathbf{b}_n \end{matrix}\right] =\det B_1$ 

So $A\mathbf{x} = \mathbf{y}$ can be solved by determinants. 
$$
x_j=\frac{\det B_j}{\det A}
$$
The matrix $B_j$ has the $j$-th column of $A$ replaced by the vector $\mathbf{y}$.





The ***rank*** of $A$ is

- the number of pivots

- the number of independent row vectors or column vectors of $A$

- the dimension of the row space or column space

- the dimension of  of $\mathscr{R}(A)$

  



An efficient way to describe the nullspace, i.e., the solution to $A\mathbf{x}=\mathbf{0}$, when the rank of $A$ is $r$, is to alternately set one of $x_{r+1}, \dots , x_n$ to $1$ and the remaining to $0$ and solving

$A_{m\times r} \mathbf{x}_{r \times 1}= -\mathbf{b}_j$ for $ r+1 \leq j\leq n$ 







https://drive.google.com/file/d/1oDIkKMwBBvoF2YSOSFw2MZIcDVAg70Bk/view?usp=sharing

> use geogebra to open it

example the complete solution (Strange P.154)
$$
\left[\begin{matrix}1 && 1 && 1\\1&&2&&-1\end{matrix}\right]\left[\begin{matrix}x_1  \\x_2 \\x_3\end{matrix}\right]=\left[\begin{matrix}3\\4\end{matrix}\right]
$$
$\left[\begin{matrix}1 && 1 && 1\\1&&2&&-1\end{matrix}\right]$ has full row rank

 Setting  $ x_3=0$,  the particular solution that sovles $\left[\begin{matrix}1 && 1 && 1\\1&&2&&-1\end{matrix}\right]\left[\begin{matrix}x_1  \\x_2 \\0\end{matrix}\right]=\left[\begin{matrix}3\\4\end{matrix}\right]$ is $\left[\begin{matrix}2  \\1 \\0\end{matrix}\right]$

Setting $ x_3=1$, the special solution that solves $\left[\begin{matrix}1 && 1 && 1\\1&&2&&-1\end{matrix}\right]\left[\begin{matrix}x_1 \\x_2 \\1\end{matrix}\right]=\left[\begin{matrix}0\\0\end{matrix}\right]$ is $\left[\begin{matrix}-3  \\2 \\1 \end{matrix}\right]$

The complete solution is $\left[\begin{matrix}2  \\1 \\0\end{matrix}\right]+x_3\left[\begin{matrix}-3  \\2 \\1 \end{matrix}\right]$

If we want to restrict the particular solution to the row space

we can solve
$$
\left[\begin{matrix}1 && 1 && 1\\1&&2&&-1\end{matrix}\right]\left(\lambda_1\left[\begin{matrix}1 \\1\\1\end{matrix}\right]+\lambda_2 \left[\begin{matrix}1 \\2\\-1\end{matrix}\right]\right)=\left[\begin{matrix}3\\4\end{matrix}\right]
$$
for $\lambda_1, \lambda2$.

$\lambda_1=\frac{5}{7}, \lambda_1=\frac{3}{7}$

Then the complete solution is $\lambda_1\left[\begin{matrix}1 \\1\\1\end{matrix}\right]+\lambda_2 \left[\begin{matrix}1 \\2\\-1\end{matrix}\right] +x_3\left[\begin{matrix}-3  \\2 \\1 \end{matrix}\right]=\left[\begin{matrix}\frac{8}{7} \\\frac{11}{7}\\\frac{2}{7}\end{matrix}\right]+x_3\left[\begin{matrix}-3  \\2 \\1 \end{matrix}\right]$

These two forms are equivalent as $\left[\begin{matrix}\frac{8}{7} \\\frac{11}{7}\\\frac{2}{7}\end{matrix}\right]-\left[\begin{matrix}2  \\1 \\0\end{matrix}\right]=\left[\begin{matrix}-\frac{6}{7} \\\frac{4}{7}\\\frac{2}{7}\end{matrix}\right]$ is a multiple of $\left[\begin{matrix}-3  \\2 \\1 \end{matrix}\right]$


$$
\left[\begin{matrix}1 && 1 && 1\\1&&2&&-1 \\ 1 &&3 &&-3\end{matrix}\right]\left[\begin{matrix}x_1  \\x_2 \\x_3\end{matrix}\right]=\left[\begin{matrix}3\\4\\5\end{matrix}\right]
$$
$-\mathbf{a}_1+2\mathbf{a}_2=-\left[\begin{matrix}1 \\1\\1\end{matrix}\right]+2\left[\begin{matrix}1 \\2\\-1\end{matrix}\right]=\left[\begin{matrix}1 \\3\\-3\end{matrix}\right]=\mathbf{a}_3$



The row space is the same as that of the previous system

the complete solution is the same as that of the previous sytem $\left[\begin{matrix}\frac{8}{7} \\\frac{11}{7}\\\frac{2}{7}\end{matrix}\right]+x_3\left[\begin{matrix}-3  \\2 \\1 \end{matrix}\right]$

the column space is $y_3=-y_1+2y_2$
$$
\left[\begin{matrix}1 && 1 && 1\\1&&2&&-1 \\ 1 &&3 &&-3\end{matrix}\right]\left[\begin{matrix}x_1  \\x_2 \\x_3\end{matrix}\right]=\left[\begin{matrix} x_1+x_2+x_3\\x_1+2x_2-x_3\\x_1+3x_2-3x_3\end{matrix}\right]
$$
$-3\mathbf{b}_1+2\mathbf{b}_2=-\mathbf{b}_3$



$\mathbf{b}_1$ and $\mathbf{b}_2$ are linearly independent and $\mathbf{b}_1^\top \mathbf{y}=0$ and $\mathbf{b}_2^\top \mathbf{y}=0$ determines the left nullspace, which is all multiples of $(1, -2, 1)$

it is orthogonal to the column space spanned by $\mathbf{b}_1$ and $\mathbf{b}_2$ 





$\frac{\lang \mathbf{v},\mathbf{w}\rang}{\lang \mathbf{w},\mathbf{w}\rang}\mathbf{w}$ gives the projection of $\mathbf{v}$ onto $\mathbf{w}$ 

$\mathbf{v}-\frac{\lang \mathbf{v},\mathbf{w}\rang}{\lang \mathbf{w},\mathbf{w}\rang}\mathbf{w}$ is orthogonal to $\mathbf{w}$







If we project $\left[\begin{matrix}2  \\1 \\0\end{matrix}\right]$ onto the subspace formed by $\left[\begin{matrix}1 &&1 \\1 &&2 \\1&&-1\end{matrix}\right]$
$$
\left[\begin{matrix}1 &&1 \\1 &&2 \\1&&-1\end{matrix}\right]\left(\left[\begin{matrix}1 && 1 && 1\\1&&2&&-1\end{matrix}\right]\left[\begin{matrix}1 &&1 \\1 &&2 \\1&&-1\end{matrix}\right]\right)^{-1} \left[\begin{matrix}1 && 1 && 1\\1&&2&&-1\end{matrix}\right]\left[\begin{matrix}2  \\1 \\0\end{matrix}\right]=\left[\begin{matrix}\frac{8}{7} \\\frac{11}{7}\\\frac{2}{7}\end{matrix}\right]
$$




Suppose $\mathbf{u},\mathbf{v},\mathbf{w} \in \mathbb{R}^3$ and $A=\left[\mathbf{u},\mathbf{v},\mathbf{w}\right]$

$\det A=\lang\mathbf{u},\mathbf{v}\times\mathbf{w}\rang$

$\Vert \mathbf{v}\times \mathbf{w}\Vert$ = $\Vert \mathbf{w}\Vert \Vert \mathbf{v}-\frac{\lang \mathbf{v},\mathbf{w}\rang}{\lang \mathbf{w},\mathbf{w}\rang}\mathbf{w}\Vert$

For the system of linear equations $A\mathbf{x}=\mathbf{y}$, when $\det A \neq 0$, there is a unique solution of $\mathbf{x}$
$$
x_1=\frac{\det \left[\mathbf{y},\mathbf{v},\mathbf{w}\right]}{\det A}, \quad x_2=\frac{\det \left[\mathbf{u},\mathbf{y},\mathbf{w}\right]}{\det A}, \quad x_3=\frac{\det \left[\mathbf{u},\mathbf{v},\mathbf{y}\right]}{\det A}
$$







### Eigenvectors & Eigenvalues (for Square Matrices)



$\mathbb{S}^n$ is the space of $n \times n$ real symmetric matrices



For a square matrix $ A  \in \mathbb{S}^n$

We want "eigenvectors" $\mathbf{x}$ that don't change direction when multiplying it by $A$.

The basic equation is $A\mathbf{x}= \lambda \mathbf{x}$. The number $\lambda$ is an eigenvalue of $A$. 



Special matrices $P$:

- $P$ is a projection matrix, i.e., $P^2=P$ (a property known as idempotentness)
  - $P$ fixes every vector in $ \mathscr{R}(P)$ (i.e., the column space of $P$)
  
  - $P\mathbf{x} $ is parallel to $\mathbf{x} \Rightarrow \mathbf{x} \in \mathscr{R}(P)$ (i.e., $\mathbf{x}$ in the column space of $P$) 

  - When $\mathbf{x} \in \mathscr{R}(P)$, $P\mathbf{x} = \mathbf{x}$, i.e., the column space doesn't move. The nullspace goes to zero ($P\mathbf{x} = 0 \mathbf{x}$ ).
  
  - The eigenvectors for $\lambda = 1$ and $\lambda = 0$ fill the column space and nullspace. 
  
  - So If $A$ is idempotent, then $\text{rank}(A) = \text{trace}(A)$, because the sum of the diagonal entries equals the sum of the eigenvalues.
  
    another proof:
    $$
    \operatorname{trace}(P)  = \operatorname{trace}\left( X\left( X^\top X\right)^{-1}X^\top  \right)  = \operatorname{trace}\left(\left( X^\top X\right)^{-1}X^\top   X\right)  = \operatorname{trace}(I_K ) = K
    $$
    this proof uses the fact that $\operatorname{trace}\left( AB \right)  = \operatorname{trace}\left(BA\right)$
  
  - $P=P^\top$ (***symmetric***)  $\Leftrightarrow$ $\mathscr{N}(P)$ and  $\mathscr{R}(P)$ are complements of each other in $\mathbb{R}^n$. (Recall:  $\mathscr{N}(A)$ and the row space of $A$ are always orthogonal complements)
  
    - another proof: 
  
      If $P$ is a projection in $\R^n$, then every $\mathbf{x} \in \mathbb{R}^n$ has a unique representation of the form
      $$
      \mathbf{x}=\mathbf{x}_1+\mathbf{x}_2
      $$
       where  $\mathbf{x}_1 \in \mathscr{R}(P),  \mathbf{x}_2  \in \mathscr{N}(P)$ 
  
      For any vectors $\mathbf{x},\mathbf{y}$
  
      $\lang P\mathbf{x},\mathbf{y}\rang=\lang  \mathbf{x}_1,\mathbf{y}_1+\mathbf{y}_2\rang=\lang  \mathbf{x}_1,\mathbf{y}_1\rang=\lang  \mathbf{x}_1+\mathbf{x}_2,\mathbf{y}_1\rang = \lang  \mathbf{x}, P\mathbf{y}\rang$. That is, $\mathbf{x}^\top P^\top\mathbf{y}=\mathbf{x}^\top P\mathbf{y}$. 
  
      So $P=P^\top$
  
      
  
- Markov matrix, also called stochastic matrix

  - Each column adds to 1
  - A Markov matrix always has an eigenvalue $1$. All other eigenvalues are in absolute value smaller or equal to $1$.
    - Because $\det P = \det P^\top $ as well as $\det (P-\lambda I) = \det (P-\lambda I)^\top $, the eigenvalues of $P$ and $P^\top$ are the same.
    - $ \left[ \begin{matrix}1\\1\\\vdots\\1\end{matrix} \right]$ is an eigenvalue of $P^\top$ (whose rows each add to 1), and the corresponding eigenvalue is $1$.

- Singular matrix $P$

  -  The $\dim \mathscr{N}(P) \geq 1$. so  $\lambda = 0$ is an eigenvalue. 

- Symmetric matrix $P$

  - Eigenvectors of a real symmetric matrix (when they correspond to different $\lambda$'s) are always perpendicular.
    -  Suppose $P\mathbf{x} = \lambda_1\mathbf{x}$  and $P\mathbf{y} = \lambda_2 \mathbf{y}$ and $\lambda_1 \neq \lambda_2$.  $ \left(\lambda_1\mathbf{x}\right)^\top\mathbf{y} = \left(P\mathbf{x}\right)^\top\mathbf{y} = \mathbf{x}^\top P \mathbf{y} = \mathbf{x}^\top \left(\lambda_2 \mathbf{y} \right)$. So $\mathbf{x}^\top\mathbf{y}=0$.



The product of the $n$ eigenvalues equals the determinant. The sum of the $n$ eigenvalues equals the sum of the $n$ diagonal entries (trace).



Suppose the $n \times n$ matrix $A$ (a square matrix) has $n$ ***linearly independent*** eigenvectors $\mathbf{x}_1, \dots , \mathbf{x}_n $. Put them into the columns of an eigenvector matrix $X$

$AX=A \left[\begin{matrix}\mathbf{x}_1 & \mathbf{x}_2 &\dots& \mathbf{x}_n\end{matrix}\right] =  \left[\begin{matrix}\mathbf{x}_1 & \mathbf{x}_2 &\dots& \mathbf{x}_n\end{matrix}\right]\left[\begin{matrix}\lambda_1 & 0 & \dots & 0 \\ 0 & \lambda_2 &\dots & 0\\ \vdots & \vdots &\ddots &\vdots \\0 &0 &\dots & \lambda_n\end{matrix}\right] = X\Lambda$

Then the eigenvalue matrix $\Lambda=X^{-1}AX $ 

and $A=X\Lambda X^{-1}$



Eigenvectors for $n$ different $\lambda$'s are independent. Then we can diagonalize $A$.





1. (Geometric Multiplicity = GM) Count the independent eigenvectors for $\lambda$. Then GM is the dimension of the nullspace of $A -\lambda I$. 
2. (Algebraic Multiplicity = AM) AM counts the repetitions of $ \lambda$ among the eigenvalues. Look at then roots of $\det(A - \lambda I)=0$ .
3. Always GM $  \leq  $ AM for each $\lambda$.
4. if  GM $<$ AM, $A$ is not diagonalizable



When no eigenvalues of $A$ are repeated, the eigenvectors are sure to be independent. Then A can be diagonalized. 

But a repeated eigenvalue (AM) can produce a shortage of eigenvectors. This sometimes happens for nonsymmetric matrices. It never happens for symmetric matrices. 

There are always enough eigenvectors to diagonalize a symmetric matrix (i.e., $S = S^\top$). 

Every symmetric matrix $S= Q\Lambda Q^\top =\lambda_1 \mathbf{q}_1   \mathbf{q}_1^\top + \dots +\lambda_n \mathbf{q}_n   \mathbf{q}_n^\top$









**Positive Definitive Matrices**



When $S \in \mathbb{S}^n$ has one of these five properties, it has them all:

- All $n$ pivots of $S$ are positive. 
- All $n$ upper left determinants are positive. 
- All $n$ eigenvalues of $S$ are positive. 
- $\mathbf{x}^\top S\mathbf{x}$ is positive except at $\mathbf{x} = 0$. This is the energy-based definition. 
- $S=  A^\top A$ for a matrix $A$ with independent columns.



$\mathbb{S}^n_+ = \{X\in \mathbb{S}^n\colon \; \mathbf{u}^\top X \mathbf{u}\geq0 \text{ for all } \mathbf{u}\in \mathbb{R}^n\}$, i.e., the space of $n \times n$ positive semidefinite matrices 

$\mathbb{S}^n_{++} = \{X\in \mathbb{S}^n\colon \; \mathbf{u}^\top X \mathbf{u}>0 \text{ for all } \mathbf{u}\in \mathbb{R}^n \setminus \{\mathbf{0}\}\}$, i.e., the space of  $n \times n$ positive definite matrices 





All the eigenvalues $\lambda$'s of a real symmetric matrix are real.

- i.e., $X \in \mathbb{S}^n \implies \lambda_1, \dots, \lambda_n \in \mathbb{R}^n$

 

$ X \in \mathbb{S}^n_+ \Longleftrightarrow  \lambda_1, \dots, \lambda_n \in \mathbb{R}^n_+$ (nonnegative)

$ X \in \mathbb{S}^n_{++} \Longleftrightarrow  \lambda_1, \dots, \lambda_n \in \mathbb{R}^n_{++}$ (positive)





Think of a tilted ellipse $\mathbf{x}^\top S\mathbf{x} = 1$ with $S$ positive definitive

Since $S= Q\Lambda Q^\top$ with $Q$ orthonormal

$\mathbf{x}^\top Q\Lambda Q^\top \mathbf{x} = \left[\begin{matrix}\mathbf{x}^\top\mathbf{q}_1 & \mathbf{x}^\top\mathbf{q}_2 \end{matrix} \right] \left[\begin{matrix}\lambda_1 & 0  \\ 0 & \lambda_2 \end{matrix}\right] \left[\begin{matrix} \mathbf{q}_1^\top \mathbf{x}   \\   \mathbf{q}_2^\top \mathbf{x} \end{matrix}\right] =  \lambda_1 \left(\mathbf{x}^\top\mathbf{q}_1 \right)^2  + \lambda_2 \left(\mathbf{x}^\top\mathbf{q}_2 \right)^2  =1 $

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tilted%20ellipse.PNG)

Left part:

The axes point along eigenvectors of $S$. The half-lengths are $1 / \sqrt{\lambda_1}$ and $1 / \sqrt{\lambda_2}$.

Right part:

$\lambda_1 X^2  + \lambda_2  Y^2  =1$ where $X=\mathbf{x}^\top\mathbf{q}_1$ and $Y=\mathbf{x}^\top\mathbf{q}_2$





The eigenvectors in $X$ have three big problems: 

- They are usually not orthogonal,
- there are not always enough eigenvectors, 
- and $A\mathbf{x}= \lambda \mathbf{x}$ requires $A$ to be a square matrix.





### Singular Value Decomposition



A is any $m \times n$ matrix, square or rectangular. $\rank (A) =r$



$A \mathbf{v}_i = \sigma_i \mathbf{u}_i$



$AV=A \left[\begin{matrix}\mathbf{v}_1 & \mathbf{v}_2 &\dots& \mathbf{v}_n\end{matrix}\right] =  \left[\begin{matrix}\mathbf{u}_1 & \mathbf{u}_2 &\dots& \mathbf{u}_m\end{matrix}\right]\left[\begin{matrix}\sigma_1 & 0 & \dots & 0 & \dots & 0 \\ 0 & \sigma_2 &\dots & 0 & \dots & 0\\ \vdots & \vdots &\ddots &\vdots &\ddots &\vdots  \\0 &0 &\dots & \sigma_m &\dots & 0\end{matrix}\right] = U \Sigma$

$\mathbf{u}_1, \dots , \mathbf{u}_r$ is an orthonormal basis for the column space 

$\mathbf{v}_1, \dots , \mathbf{v}_r$ is an orthonormal basis for the row space V

We haven $n - r$ more $\mathbf{v}$'s and $m - r$ more $\mathbf{u}$'s, from the nullspace $\mathscr{N}(A)$ and the left nullspace $\mathscr{N}(A^\top)$ . They are automatically orthogonal to the former $\mathbf{v}$'s and $\mathbf{u}$'s

- $\mathbf{u}_{r+1}, \dots , \mathbf{u}_m$ is an orthonormal basis for the left nullspace $\mathscr{N}(A^\top)$
- $\mathbf{v}_{r+1}, \dots , \mathbf{v}_n$ is an orthonormal basis for the nullspace $\mathscr{N}(A)$. 

The singular values can be put in descending order, $\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_r >0$



$A = U \Sigma V^\top = \sigma_1 \mathbf{u}_1 \mathbf{v}_1^\top +\dots + \sigma_r \mathbf{u}_r \mathbf{v}_r^\top$



$A^\top A= (U\Sigma V^\top )^\top (U\Sigma V^\top ) = V \Sigma^\top\Sigma V^\top$ and $AA^\top= U \Sigma  \Sigma^\top U^\top$

Both $A^\top A$ and $A A^\top$ are symmetric



$\underset{\mathbf{x}}{\max}  \frac{\Vert A\mathbf{x} \Vert}{\Vert \mathbf{x} \Vert}$

is equivalent to $ \underset{\mathbf{x}}{\max}  \frac{\mathbf{x}^\top A^\top A\mathbf{x} }{\mathbf{x}^\top \mathbf{x}} = \frac{\mathbf{x}^\top  V \Sigma^\top\Sigma V^\top \mathbf{x} }{\mathbf{x}^\top \mathbf{x}}$



Let $S\equiv A^\top A =Q \Lambda Q^\top$ and $r(\mathbf{x})=\frac{\mathbf{x}^\top S \mathbf{x} }{\mathbf{x}^\top \mathbf{x}}$ The derivatives of $r(\mathbf{x}) =0$ when $S\mathbf{x} = r(\mathbf{x})\mathbf{x}$.

The winning vector $\mathbf{q}_1$ is the eigenvector that corresponds to the largest eigenvalue $\lambda_1$ of $S $

Since the eigenvalue matrix $\Lambda = \Sigma^\top\Sigma$ and the eigenvector matrix $Q=V$

The winning vector is the singular vector that corresponds to the largest singular value $\sigma_1 = \sqrt{\lambda_1}$



The second largest eigenvalue 

$\lambda_2 =$  maximum ratio $r(\mathbf{x})=\frac{\mathbf{x}^\top S \mathbf{x} }{\mathbf{x}^\top \mathbf{x}} $  among all $ \mathbf{x}$'s with $\mathbf{q}_1^\top \mathbf{x} = 0$. 

So $\mathbf{x}= \mathbf{q}_2$ will win





### PCA



https://drive.google.com/drive/u/1/folders/1E_gKojbiqSCMz6ldieKRhZZgbN12wmmG

https://setosa.io/ev/principal-component-analysis/



For $m = 2$ variables like age and height, the $n$ points lie in the plane $\mathbb{R}^ 2$ . 



each feature has zero mean, unit variance, otherwise normalize it



Subtract the average age and height to center the data. 

So the point $(0, 0)$  is now the center of then points.



These two attributes are strongly correlated or even almost linearly dependent



having normalized our data, how do we compute the “major axis of variation” u—that is, the direction on which the data approximately lies? 

 

$A_{n\times 2}$

find a vector so that $A$'s projections onto this vector has the maximum variance

the variance of $A$'s projections onto a vector is $ \frac{\mathbf{x}^\top A^\top A\mathbf{x} }{\mathbf{x}^\top \mathbf{x}}$

$ \underset{\mathbf{x}}{\max}  \frac{\mathbf{x}^\top A^\top A\mathbf{x} }{\mathbf{x}^\top \mathbf{x}}$

The vector is the singular vector that corresponds to the largest singular value $\sigma_1 = \sqrt{\lambda_1}$



$\Sigma_{n \times 2}$

$\mathbf{v}_1$ in $V$

So if we project data on $\mathbf{v}_1$, $ A \mathbf{v}_1$

$\mathbf{v}_1^\top A^\top A\mathbf{v}_1= \mathbf{v}_1^\top V \Sigma^\top\Sigma V^\top \mathbf{v}_1= \mathbf{v}_1^\top  \left[\begin{matrix} \mathbf{v}_1 & \mathbf{v}_2 \end{matrix} \right]  \Sigma^\top\Sigma \left[\begin{matrix} \mathbf{v}_1^\top \\   \mathbf{v}_2^\top   \end{matrix}\right] \mathbf{v}_1 = \sigma_1^2$

Project data on both  $\mathbf{v}_1$ and  $\mathbf{v}_2$

$ \left[\begin{matrix} \mathbf{v}_1^\top \\   \mathbf{v}_2^\top   \end{matrix}\right] A^\top A \left[\begin{matrix} \mathbf{v}_1 & \mathbf{v}_2 \end{matrix}\right]= V^\top V \Sigma^\top\Sigma V^\top V=    \Sigma^\top\Sigma $





To summarize, we have found that if we wish to find a 1-dimensional subspace with with to approximate the data, 

we should choose $\mathbf{v}$ to be the principal eigenvector of  $A^\top A$. 

More generally, if we wish to project our data into a $k$-dimensional subspace, we should choose the top $k$ eigenvectors of $A^\top A$ 

They are called the first $k$ principal components of the data



PCA can also be derived by picking the basis that minimizes the approximation error arising from projecting the data onto the $k$-dimensional subspace spanned by them



$\mathbf{x}^\top\mathbf{v}$ gives the length of $\mathbf{x}$'s projection on $\mathbf{v}$. the projection vector is $\mathbf{x}^\top\mathbf{v}\mathbf{v}$ 

So to revert to the standard bases

$ \left[\begin{matrix} \mathbf{v}_1 & \mathbf{v}_2 \end{matrix}\right] \left(A \left[\begin{matrix} \mathbf{v}_1 & \mathbf{v}_2 \end{matrix}\right]\right)^\top$
