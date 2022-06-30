



the graph of a function $f\colon  \to \mathbb{R}$ is a curve in $ \mathbb{R}^2$
$$
\left[\begin{matrix}
x \\
f(x)
\end{matrix}\right]
$$
the derivative of $f(x)$ at $x_0$ is

$f'(x_0) = \lim_{t\to x_0} \frac{f(t)-f(x_0)}{t-x_0}$

the tagent vector at $x_0$ is 
$$
\left[\begin{matrix}
1 \\
f'(x_0)
\end{matrix}\right]
$$
The tagent plane to the graph of the function $f$ at the point $(x_0,f(x_0))$ is 

the graph of a function $g$

that satisfies  $\lim_{x\to x_0} \frac{f(x)-g(x)}{x-x_0}=0$

$g(x)=f(x_0) + f'(x_0)(x-x_0)$



its graph is a straight line in $ \mathbb{R}^2$

$y =f(x_0) + f'(x_0)(x-x_0)$

whose normal vector is $(-f'(x_0),1)$





Suppose that $\mathcal{O}$ is an open subset of the plane $\mathbb{R}^2$ that contains point $(x_0, y_0)$ and that the function $f \colon  \mathcal{O}  \to \mathbb{R}$ is continuously differentiable. 

The tangent plane to the graph of the function $f \colon  \mathcal{O}  \to \mathbb{R}$ at the point $(x_0, y_0, f (x_0, y_0))$. This tangent plane is the graph of the function $g\colon \mathbb{R}^2 \to \mathbb{R}$ 

that satisfies
$$
\lim_{(x,y)\to (x_0,y_0)} \frac{f(x,y)-g(x,y)}{\sqrt{(x-x_0)^2+(y-y_0)^2}}=0
$$
So it can be defined by

$g(x,y)=f(x_0,y_0) + \frac{\partial f}{\partial x}(x_0,y_0)(x-x_0)+ \frac{\partial f}{\partial y}(x_0,y_0)(y-y_0)$



its graph is a plane in $ \mathbb{R}^3$

$z =f(x_0,y_0) + \frac{\partial f}{\partial x}(x_0,y_0)(x-x_0)+ \frac{\partial f}{\partial y}(x_0,y_0)(y-y_0)$

whose normal vector is $(-\frac{\partial f}{\partial x}(x_0,y_0),-\frac{\partial f}{\partial y}(x_0,y_0),1)$

The partial derivatives depits two lines in $\mathbb{R}^3$ that are tangent to the graph of $f$

$\left( 1, 0, \frac{\partial f}{\partial x}(x_0,y_0) \right)$ and $ \left( 0, 1, \frac{\partial f}{\partial y}(x_0,y_0) \right)$


the cross-product $ \eta= \left( 1, 0, \frac{\partial f}{\partial x}(x_0,y_0) \right)\times  \left( 0, 1, \frac{\partial f}{\partial y}(x_0,y_0) \right) =\left(-\frac{\partial f}{\partial x}(x_0,y_0), -\frac{\partial f}{\partial y}(x_0,y_0), 1\right)$

is normal to the plane that passes through the point $(x_0 , y_0 , f (x_0 , y_0))$ 

defined by the equation

$ \langle \eta, (x - x_0, y- y_0, z - f (x_0, y_0)) \rangle = 0$



**9.5 Theorem**  A linear operator $A$ on a finite-dimensional vector space $X$ is one-to-one if and only if the range of $A$ is all of $X$ (i.e., $\mathscr{R}(A)=X$).



**9.6 Definition**  

- (a) Let $L(X, Y)$ be the set of all linear transformations of the vector space $X$ into the vector space $Y$. Instead of $L(X, X)$, we shall simply write $L(X)$.  If $A_1, A_2 \in L(X, Y)$   and if $c_1, c_2$ are scalars, define $c_1A_1+ c_2A_2$ by
  $$
  (c_1A_1+ c_2A_2)\mathbf{x} = c_1A_1\mathbf{x} + c_2 A_2\mathbf{x}   \quad (\mathbf{x}\in X).
  $$
   It is then clear that $c_1A_1+ c_2A_2 \in L(X,Y)$.



- (b) If $X, Y, Z$ are vector spaces, and if $A \in L(X, Y)$ and $B \in L(Y, Z)$, we define their product $BA$ to be the composition of $A$ and $B$:
  $$
  (BA)\mathbf{x} = B(A\mathbf{x}) \quad (\mathbf{x} \in X).
  $$
  Then $BA \in L( X, Z)$. Note that $BA$ need not be the same as $AB$, even if $X = Y = Z$.



- (c) For $A \in L(\mathbb{R}^n, \mathbb{R}^m)$, define the norm $ \Vert A \Vert$ of $A$ to be the sup of all numbers $| A\mathbf{x} |$, where $\mathbf{x}$ ranges over all vectors in $R^n$ with  $| \mathbf{x} | \leq 1$.

  Observe that the inequality
  $$
  \vert A\mathbf{x} \vert \leq \Vert A\Vert\vert \mathbf{x}\vert
  $$
  holds for all $\mathbf{x} \in \mathbb{R}^n$. Also, if $\lambda$ is such that $| A\mathbf{x} | \leq \lambda|\mathbf{x}| $ for all $\mathbf{x} \in \mathbb{R}^n$, then $\Vert A\Vert \leq \lambda$.









**9.19 Theorem** 

Suppose $\mathbf{f}$ maps a convex open set $E  \subset \mathbb{R}^n$ into $\mathbb{R}^m$,  $\mathbf{f}$  is differentiable in $E$, and there is a real number $M$ such that
$$
\Vert \mathbf{f} '(\mathbf{x})\Vert \leq M
$$
for every $\mathbf{x} \in  E$. Then
$$
\vert \mathbf{f} (\mathbf{b}) -\mathbf{f} (\mathbf{a}) \vert\leq M\vert \mathbf{b} -\mathbf{a}\vert
$$
for all $\mathbf{a}, \mathbf{b}\in  E$.



**9.24 Theorem** 

Suppose $\mathbf{f}$ is a $\mathscr{C}'$-mapping of an open set $E \subset \mathbb{R}^n $ into $\mathbb{R}^n$, and $\mathbf{f}'(\mathbf{a})$  is ***invertible*** for some $\mathbf{a} \in E$, and $\mathbf{b}=\mathbf{f}(\mathbf{a})$. Then

- (a) there exist open sets $U$ and $V$ in $\mathbb{R}^n $ such that  $\mathbf{a} \in U, \; \mathbf{b} \in V$,  $\mathbf{f}$ is one-to-one on $U$, and $\mathbf{f}(U)=V$;

- (b) if $\mathbf{g}$ is the inverse of $\mathbf{f}$ (which exists by (a)), defined in $V$ by
  $$
  \mathbf{g}(\mathbf{f}(\mathbf{x}))=\mathbf{x} \quad (\mathbf{x}\in U),
  $$
  then $\mathbf{g} \in \mathscr{C}'(V)$.

  



**9.28 Theorem** 

$\mathbf{f}$ is a $\mathscr{C}'$-mapping of an open set $E \subset \mathbb{R}^{n+m} $ into $\mathbb{R}^n$, , such that $\mathbf{f}(\mathbf{a}, \mathbf{b}) = \mathbf{0}$ for some point $(\mathbf{a}, \mathbf{b}) \in E$. Put $A = \mathbf{f}'(\mathbf{a}, \mathbf{b}) $ and assume that $A_{\mathbf{x}}$ is invertible. 

Then there exist open sets $U \subset \mathbb{R}^{n+m} $ and $W \subset \mathbb{R}^{m}$, with $(\mathbf{a}, \mathbf{b}) \in U$ and $\mathbf{b} \in W$, having the following property: 

To every $\mathbf{y} \in W$ corresponds a unique $\mathbf{x}$ such that
$$
(\mathbf{x}, \mathbf{y}) \in U \quad \text{and} \quad \mathbf{f}(\mathbf{x}, \mathbf{y}) = \mathbf{0}
$$
If this $\mathbf{x}$  is defined to be $\mathbf{g}(\mathbf{y})$, then $\mathbf{g}$ is a $\mathscr{C}'$-mapping of $W$ into $\mathbb{R}^n$, $\mathbf{g}(\mathbf{b})=\mathbf{a}$, 
$$
\mathbf{f}(\mathbf{g}(\mathbf{y}), \mathbf{y})) =\mathbf{0} \quad (y \in W),
$$
and
$$
\mathbf{g}'(\mathbf{b})=-\left(A_{\mathbf{x}}\right)^{-1}A_{\mathbf{y}}
$$


**Example The general idea of local rectification of curves** (Zorich 2011 P.503)

New coordinates are usually introduced for the purpose of simplifying the analytic expression for the objects that occur in a problem and making them easier to visualize in the new notation.

Suppose for example, a curve in the plane $$\mathbb{R}^2$$ is defined by the equation $F(x,y) = 0$. Assume that $F$ is a smooth function, that the point $(x_0,y_0)$ lies on the curve (i.e., $F(x_0,y_0) = 0$), and that this point is not a critical point of $F$. E.g., suppose 
$$
F_y '(x,y) \neq 0
$$
Let us try to choose coordinates $\xi ,\eta$ so that in these coordinates a closed interval of a coordinate line (e.g., the line $\eta = 0$, corresponds to an arc of this curve):
$$
\begin{dcases}
\xi = x-x_0 \\ \eta = F(x,y)
\end{dcases}
$$
 The Jacobi matrix  of this transformation 
$$
\left[\begin{matrix}
1&&0\\
F_x '(x,y)&&F_y '(x,y)
\end{matrix}\right]
$$


has its determinant $F_y '(x,y) \neq 0$

So this mapping is a diffeomorphism of a neighborhood of $(x_0,y_0)$ onto a neighborhood of the point $(\xi, \eta) = (0, 0)$

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/local%20rectification.PNG)







#### EQUATIONS OF SURFACES AND PATHS IN $\mathbb{R}^3$

Proposition 17 .8 Advanced Calculus ()

Let $\mathcal{O}$ be an open subset of $\mathbb{R}^3$ and suppose that the function $f\colon \mathcal{O} \to \mathbb{R}$ is continuously differentiable. Assume that $(x_0 , y_0 , z_0)$ is a point in $\mathcal{O}$ at which 
$$
f(x_0 , y_0 , z_0)=0 \quad \text{and} \quad \nabla f (x_0 , y_0 , z_0) \neq \mathbf{0}
$$


Then at the point $(x_0 , y_0 , z_0)$ the vector $\nabla f (x_0 , y_0 , z_0)$ is normal to the surface consisting of the solutions, in a neighborhood of the point $(x_0 , y_0 , z_0) $, of the equation 
$$
f(x , y , z)=0, \quad (x, y, z) \; \text{in} \; \mathcal{O}.
$$

Zorich P.488

In $\mathbb{R}^3$, the equation $f(x,y,z) = 0$ defines a two-dimensional surface in a neighborhood of a noncritical point  $(x_0 , y_0 , z_0)$ satisfying the equation, which, when the condition $\frac{\partial f}{\partial z}(x_0,y_0,z_0) \neq 0$ holds, can be locally written in the form 
$$
z= g(x,y)
$$
As we know, the equation of the plane tangent to the graph of this function at the point $(x_0, y_0, z_0)$  ($z_0 = g (x_0, y_0)$) has the form
$$
z=g(x_0,y_0) + \frac{\partial g}{\partial x}(x_0,y_0)(x-x_0)+ \frac{\partial g}{\partial y}(x_0,y_0)(y-y_0)
$$
where
$$
\frac{\partial g}{\partial x}(x_0,y_0) =-\frac{f_x'(x_0,y_0,z_0)}{f_z'(x_0,y_0,z_0)}, \quad \frac{\partial g}{\partial y}(x_0,y_0) =-\frac{f_y'(x_0,y_0,z_0)}{f_z'(x_0,y_0,z_0)}.
$$
and therefore the equation of the tangent plane can be rewritten as 
$$
f_x'(x_0,y_0,z_0)(x-x_0)+ f_y'(x_0,y_0,z_0) + f_z'(x_0,y_0,z_0)(z-z_0)=0
\\\langle\nabla f(x_0,y_0,z_0), \; (x-x_0,y-y_0,z-z_0)\rangle=0
$$
Similarly, in the general case we obtain the equation 
$$
\langle\nabla f(\mathbf{a}), \; \mathbf{x}-\mathbf{a}\rangle=0
$$
of the hyperplane in $\mathbb{R}^m$ tangent at the point $\mathbf{a} \in \mathbb{R}^m$ (i.e., $f(\mathbf{a}) = 0$) to the surface given by the equation $f(\mathbf{x}) = 0$

 $ \nabla f$ is orthogonal to the $r$-level surface $f(\mathbf{x}) = r$



Proposition 17 .10

Let $\mathcal{O}$ be an open subset of $\mathbb{R}^3$ and suppose that the function $g\colon \mathcal{O} \to \mathbb{R}$ and  $h\colon \mathcal{O} \to \mathbb{R}$ are  continuously differentiable. Let $(x_0 , y_0 , z_0)$ be a point in $\mathcal{O}$  at which 
$$
g(x_0 , y_0 , z_0)=h(x_0 , y_0 , z_0)=0
$$
and 
$$
 \nabla g (x_0 , y_0 , z_0) \times \nabla h (x_0 , y_0 , z_0) \neq \mathbf{0}  
$$
Then there is a neighborhood of the point $(x_0 , y_0 , z_0)$ in which the set of solutions of the system { g(x, y, z) = 0 h(x, y, z) = 0, (x, y, z) in 0, (17.29) consists of a path that at the point (

holds, can be locally written in the form z = f (x,y). As we know, the equation of the plane tangent to the graph of this function at the point (x0,y0,z0) has the form z − z0 = ∂f ∂x (x0,y0)(x − x0) + ∂f ∂y (x0,y0)(y − y0). But by formula (8.89) ∂f ∂x (x0,y0) = −F x (x0,y0,z0) F z(x0,y0,z0) , ∂f ∂y (x0,y0) = −F y (x0,y0,z0) F z(x0,y0,z0) , and therefore the equation of the tangent plane can be rewritten as F x (x0,y0,z0)(x − x0) + F y (x0,y0,z0)(y − y0) + F z(x0,y0,z0)(z − z0) = 0









properties of the cross-product can be found in Appendix B. of Advanced Calculus ()



**9.30 Definitions** 

Suppose $X$ and $Y$ are vector spaces, and $A \in L( X, Y)$, as in Definition 9.6. 

The ***null space*** of $A$, $\mathscr{N}(A)$, is the set of all $\mathbf{x} \in X $ at which $A\mathbf{x} = \mathbf{0}$. It is clear that $\mathscr{N}(A)$ is a vector space in $X$.

Likewise, the ***range*** of $A$, $\mathscr{R}(A)$, is a vector space in $Y$. 

The ***rank*** of $A$ is defined to be the dimension of $\mathscr{R}(A)$. For example, the invertible elements of $L(\mathbb{R}^n)$ are precisely those whose
rank is $n$. This follows from Theorem 9.5.

If $A \in L(X, Y)$ and $A$ has rank $0$, then $A\mathbf{x} = \mathbf{0}$ for all $\mathbf{ x} \in X$, hence $\mathscr{N}(A) = X$.



**9.31 Projections**

Let $X$ be a vector space. An operator $P \in L(X)$ is said to be a ***projection*** in $X$ if $P^2=P$.

More explicitly, the requirement is that $P(P\mathbf{x}) = P\mathbf{x}$ for every  $\mathbf{ x} \in X$. In other words, $P$ fixes every vector in its range $\mathscr{R}(P)$.

Here are some elementary properties of projections:

- (a) If $P$ is a projection in $X$, then every  $\mathbf{ x} \in X$ has a unique representation of the form
  $$
  \mathbf{ x} =\mathbf{ x}_1+\mathbf{ x}_2
  $$
  where $\mathbf{ x}_1 \in \mathscr{R}(P)$, $\mathbf{ x}_2 \in \mathscr{N}(P)$.

  To obtain the representation, put $\mathbf{x}_1= P\mathbf{x}$, $\mathbf{x}_2 = \mathbf{x} - \mathbf{x}_1$. Then $P\mathbf{x}_2 = P\mathbf{x} - P\mathbf{x}_1 = P\mathbf{x} - P^2\mathbf{x} = \mathbf{0}$. 

  As regards the uniqueness, suppose there's another representation $\mathbf{ x} =\widetilde{\mathbf{ x}}_1+\widetilde{\mathbf{ x}}_2$ such that  $\widetilde{\mathbf{ x}}_1 \in \mathscr{R}(P)$,  $\widetilde{\mathbf{ x}}_2 \in \mathscr{N}(P)$, apply $P$ to it: 

  - Since $\widetilde{\mathbf{ x}}_1 \in \mathscr{R}(P)$, $P\widetilde{\mathbf{ x}}_1  = \widetilde{\mathbf{ x}}_1$;
  
  - since $\widetilde{\mathbf{ x}}_2 \in \mathscr{N}(P)$, $P\widetilde{\mathbf{ x}}_2 = \mathbf{ 0}$;
  
  - It follows that $\widetilde{\mathbf{ x}}_1= P\mathbf{x}$.
  
    
  
- (b) If $X$ is a finite-dimensional vector space and if $X_1$ is a vector space in $X$, then there is a projection $P$ in $X$ with $\mathscr{R}(P)= X_1$.

    Assume $\dim X_1=k>0$. By Theorem 9.3, $X$ has then a basis $\{ \mathbf{u}_1, \dots , \mathbf{u}_n \}$ such that $\{ \mathbf{u}_1, \dots , \mathbf{u}_k \}$ is a basis of $X_1$. Define 
    $$
    P(c_1\mathbf{u}_1+ \dots + c_n\mathbf{u}_n) = c_1\mathbf{u}_1+ \dots + c_k\mathbf{u}_k
    $$
    for arbitrary scalars $c_1, \dots , c_n$. 

    Then $ P\mathbf{x} =\mathbf{x}$ for every $\mathbf{x} \in X_1$ , and $X_1 = \mathscr{R}(P)$.
    Note that $\{ \mathbf{u}_{k+1}, \dots , \mathbf{u}_n \}$ is a basis of $\mathscr{N}(P)$. Note also that there are infinitely many projections in $X$, with range $X_1$ , if $0 < \dim X_1
    < \dim X$.





**9.32 Theorem** 

Suppose $m, n, r$ are nonnegative integers, $m \geq r$, $n \geq r$, $\mathbf{F}$ is a $\mathscr{C}'$-mapping of an open set $E \subset \mathbb{R}^n $ into $\mathbb{R}^m$, and $\mathbf{F}'(\mathbf{x})$ has rank $r$ for every  $\mathbf{x} \in E$.

Fix $\mathbf{a} \in E$ , put $A=\mathbf{F}'(\mathbf{a})$

let $Y_1$ be the range of $A$, and let $P$ be a projection in $$\mathbb{R}^m$$ whose range is $Y_1$. Let $Y_2$ be the null space of $P$. Then there are open sets $U$ and $V$ in $ \mathbb{R}^n$, with $\mathbf{a}\in U$, $U \subset E$, and there is a 1-1 $\mathscr{C}'$-mapping $\mathbf{H}$ of $V$ onto $U$ (whose inverse is also of class $\mathscr{C}'$) such that 
$$
\mathbf{F} \left(\mathbf{H} \left(\mathbf{x}\right)\right)=A\mathbf{x}+\varphi  \left(A\mathbf{x}\right)
$$
where $\varphi$ is a $\mathscr{C}'$-mapping of the open set $A(V)\subset Y_1$ into $Y_2$.



> **Proof**
>
> Assume $r>0$. Since $\dim Y_1 = r$, $Y_1$ has a basis $\{ \mathbf{y}_1, \dots , \mathbf{y}_r \}$.
>
> Choose $\mathbf{z}_i \in \mathbb{R}^n$ so that $A\mathbf{z}_i = \mathbf{y}_i \;(1 \leq i \leq r)$, and define a linear mapping $S$ of $Y_1$ into $ \mathbb{R}^n$ by setting 
> $$
> S(c_1 \mathbf{y}_1+\dots + c_r\mathbf{y}_r )=c_1\mathbf{z}_1+\dots + c_r\mathbf{z}_r
> $$
> for all scalars $c_1,\dots, c_r$.
>
> Then $AS\mathbf{y}_i = A\mathbf{z}_i= \mathbf{y}_i$ for $1 \leq i \leq r$. Thus 
> $$
> AS\mathbf{y}=\mathbf{y} \quad (\mathbf{y}\in Y_1).
> $$
> Define a mapping $\mathbf{G}$ of $E$ into  $\mathbb{R}^n$ by setting
> $$
> \mathbf{G}(\mathbf{x}) =\mathbf{x} + SP\left[\mathbf{F}(\mathbf{x}) - A\mathbf{x}\right] \quad (\mathbf{x}\in E).
> $$
> Since $\mathbf{F}'(\mathbf{a})=A$, differentiating it shows that $\mathbf{G}'(\mathbf{a}) = I$, the identity operator on $\mathbb{R}^n$. 
>
> By the inverse function theorem, there are open sets $U$ and $V$ in $\mathbb{R}^n$, with $\mathbf{a} \in U$, such that $\mathbf{G}$  is a 1-1 mapping of $U$ onto $V$ whose inverse $\mathbf{H}$ is also of class $\mathscr{C}'$. Moreover, by shrinking $U$ and $V$, if necessary, we can arrange it so that $V$ is convex and $ \mathbf{H}'(\mathbf{x})$ is invertible for every $ \mathbf{x} \in V$.
>
> Since $PA=A$ (as $\mathscr{R}(A)= Y_1$) and $AS\mathbf{y}=\mathbf{y} \; (\mathbf{y}\in Y_1)$,
> $$
> A\mathbf{G}(\mathbf{x}) =A\mathbf{x} + ASP\left[\mathbf{F}(\mathbf{x}) - A\mathbf{x}\right]\\=A\mathbf{x} -ASPA\mathbf{x} + ASP \mathbf{F}(\mathbf{x}) = ASP \mathbf{F}(\mathbf{x}) = P \mathbf{F}(\mathbf{x}) \quad (\mathbf{x}\in E).
> $$
> In particular, it holds for $\mathbf{x}\in U$. If we replace $\mathbf{x} $ by $\mathbf{H}(\mathbf{x})$, we obtain
> $$
> P \mathbf{F}(\mathbf{H}(\mathbf{x}))=A\mathbf{G}(\mathbf{H}(\mathbf{x}))=A\mathbf{x} \quad (\mathbf{x} \in V).
> $$
> Define 
> $$
> \psi(\mathbf{x})=\mathbf{F}(\mathbf{H}(\mathbf{x}))-A\mathbf{x}\quad (\mathbf{x} \in V).
> $$
> Since $PA = A$, $P\psi(\mathbf{x}) = \mathbf{0}$ for all $\mathbf{x}\in V$. Thus $\psi$ is a $\mathscr{C}'$-mapping of $V$ into $Y_2$.
>
> Since $V$ is open, it is clear that $A(V)$ is an open subset of its range $\mathscr{R}(A) = Y_1$.
>
> To complete the proof, we have to show that there is a $\mathscr{C}'$-mapping $\varphi$, of $A(V)$ into $Y_2$ which satisfies 
> $$
> \varphi(A\mathbf{x})=\psi(\mathbf{x}) \quad (\mathbf{x} \in V).
> $$
> we will first prove that
> $$
> \psi(\mathbf{x}_1)=\psi(\mathbf{x_2})
> $$
> if $\psi(\mathbf{x_1}) \in V, \;\psi(\mathbf{x_2}) \in V,\; A\psi(\mathbf{x_1})=A\psi(\mathbf{x_2})$.
>
> Put $\mathbf{\Phi}(\mathbf{x})=\mathbf{F}(\mathbf{H}(\mathbf{x}))$, for $\mathbf{x} \in V$. Since $\mathbf{H}'(\mathbf{x})$ has rank $n$ for every $\mathbf{x} \in V$, and  $\mathbf{F}'(\mathbf{x})$ has rank $r$ for every  $\mathbf{x} \in U$, it follows that 
> $$
> \rank \mathbf{\Phi}'(\mathbf{x}) =\rank \mathbf{F}'(\mathbf{H}(\mathbf{x}))  \mathbf{H}'(\mathbf{x}) =r \quad (\mathbf{x} \in V).
> $$
> Fix  $\mathbf{x} \in V$. Let $M$ be the range of $\mathbf{\Phi}'(\mathbf{x})$. Then $M \subset \mathbb{R}^n, \; \dim M=r$.
>
> 
>
> dddd



the theorem tells us about the geometry of the mapping F. If ye F(U) then y = F(H(x)) for some x e V, and (66) shows that Py = Ax. Therefore (81) y =Py+ q,(Py) (ye F(U))

Thus F(U) is an ''r-dimensional surface'' with precisely one point ''over'' each point of A(V). We may also regard F( V) as the graph of 




$A\mathbf{z}_i = \mathbf{y}_i \;(1 \leq i \leq r)$


$$
A\left[\mathbf{x}_1,\dots,\mathbf{x}_n\right]\begin{bmatrix}
    \alpha_{1i}       \\
    \vdots     \\
     \alpha_{ni}
\end{bmatrix} =\left[\mathbf{y}_1,\dots,\mathbf{y}_m\right][A]\begin{bmatrix}
    \alpha_{1i}       \\
    \vdots     \\
     \alpha_{ni}
\end{bmatrix}
$$

$$
\mathbf{z}_i =\left[\mathbf{x}_1,\dots,\mathbf{x}_n\right]\begin{bmatrix}
    \alpha_{1i}       \\
    \vdots     \\
     \alpha_{ni}
\end{bmatrix}
$$

with 
$$
[A]\begin{bmatrix}
    \alpha_{1i}       \\
    \vdots     \\
     \alpha_{ni}
\end{bmatrix} = \mathbf{e}_i
$$
