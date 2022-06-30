## Neural Network Basics

https://www.coursera.org/learn/classification-vector-spaces-in-nlp/supplement/afcaR/optional-logistic-regression-gradient



### This is an optional reading where I explain gradient descent in more detail. Remember, previously I gave you the gradient update step, but did not explicitly explain what is going on behind the scenes. 

The general form of gradient descent is defined as:

Repeat {
$$
\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j}J(\theta)
$$


}




For all j*j*. We can work out the derivative part using calculus to get:
$$
\begin{align*} & Repeat \; \lbrace \newline & \; \theta_j := \theta_j - \frac{\alpha}{m} \sum_{i=1}^m ( h(x^{(i)}, \theta) - y^{(i)}) x_j^{(i)} \newline & \rbrace \end{align*}
$$




A vectorized implementation is:

$\theta := \theta - \frac{\alpha}{m} X^{T} (H(X , \theta ) - Y)$ 




$$
J(\boldsymbol{\theta})=-\frac{1}{m}\sum_{i=1}^m \left [ y^{(i)} \log ( h(\mathbf{x}^{(i)}, \boldsymbol{\theta})  + (1-y^{(i)}) \log (1 -  h(x^{(i)}, \boldsymbol{\theta}) \right ]
$$


### Partial derivative of $\mathcal{J}(\boldsymbol{\theta})$

First calculate derivative of the sigmoid function (it will be useful while finding partial derivative of $\mathcal{J}(\boldsymbol{\theta})$:


$$
\begin{align*}  \sigma'(z)&=\left(\frac{1}{1+e^{-z}}\right)'=\frac{-(1+e^{-z})'}{(1+e^{-z})^2}=\frac{e^{-x}}{(1+e^{-x})^2} \newline &=\left(\frac{1}{1+e^{-x}}\right)\left(\frac{e^{-x}}{1+e^{-x}}\right)=\sigma(z)(1 -\sigma(z))\end{align*}
$$

Note that we computed the partial derivative of the sigmoid function. 

If we were to derive  $\sigma\left(\mathbf{x}^{(i)}, \boldsymbol{\theta}\right)$ with respect to $\theta_j$, you would get  $ \sigma(\mathbf{x}^{(i)}, \boldsymbol{\theta})\left(1-\sigma\left(\mathbf{x}^{(i)}, \boldsymbol{\theta} \right)\right)x^{(i)}_j$ .







Now we are ready to find out resulting partial derivative:
$$
\begin{align*}\frac{\partial}{\partial \theta_j} J(\boldsymbol{\theta}) &= \frac{\partial}{\partial \theta_j} \frac{-1}{m}\sum_{i=1}^m \left [ y^{(i)} \log ( h(\mathbf{x}^{(i)}, \boldsymbol{\theta})  + (1-y^{(i)}) \log (1 -  h(x^{(i)}, \boldsymbol{\theta}) \right ] \\&= - \frac{1}{m}\sum_{i=1}^m \left [     y^{(i)} \frac{\partial}{\partial \theta_j} \log ( h(\mathbf{x}^{(i)}, \boldsymbol{\theta})   + (1-y^{(i)}) \frac{\partial}{\partial \theta_j} \log (1 -  h(\mathbf{x}^{(i)}, \boldsymbol{\theta})\right ] \newline&= - \frac{1}{m}\sum_{i=1}^m \left [     \frac{y^{(i)} \frac{\partial}{\partial \theta_j}  h(\mathbf{x}^{(i)}, \boldsymbol{\theta})}{ h(\mathbf{x}^{(i)}, \boldsymbol{\theta})}   + \frac{(1-y^{(i)})\frac{\partial}{\partial \theta_j} (1 -  h(\mathbf{x}^{(i)}, \boldsymbol{\theta}))}{1 -  h(\mathbf{x}^{(i)}, \boldsymbol{\theta})}\right ]  \newline&= - \frac{1}{m}\sum_{i=1}^m \left [     \frac{y^{(i)}  h(\mathbf{x}^{(i)}, \boldsymbol{\theta}) (1 -  h(\mathbf{x}^{(i)}, \boldsymbol{\theta})) \frac{\partial}{\partial \theta_j} \boldsymbol{\theta}^\top \mathbf{x}^{(i)}}{ h(\mathbf{x}^{(i)}, \boldsymbol{\theta})}   - \frac{(1-y^{(i)}) h(\mathbf{x}^{(i)}, \boldsymbol{\theta}) (1 -  h(\mathbf{x}^{(i)}, \boldsymbol{\theta})) \frac{\partial}{\partial \theta_j} \boldsymbol{\theta}^\top \mathbf{x}^{(i)}}{1 -  h(\mathbf{x}^{(i)}, \boldsymbol{\theta}))}\right ] \newline&= - \frac{1}{m}\sum_{i=1}^m \left [     y^{(i)} (1 -  h(\mathbf{x}^{(i)}, \boldsymbol{\theta})) x^{(i)}_j - (1-y^{(i)})  h(\mathbf{x}^{(i)}, \boldsymbol{\theta}) x^{(i)}_j\right ]   \newline&= - \frac{1}{m}\sum_{i=1}^m \left [     y^{(i)} - y^{(i)}  h(\mathbf{x}^{(i)}, \boldsymbol{\theta}) -  h(\mathbf{x}^{(i)}, \boldsymbol{\theta}) + y^{(i)}  h(\mathbf{x}^{(i)}, \boldsymbol{\boldsymbol{\theta}}) \right ] x^{(i)}_j  \newline&= \frac{1}{m}\sum_{i=1}^m \left [  h(\mathbf{x}^{(i)}, \boldsymbol{\theta}) - y^{(i)} \right ] x^{(i)}_j\end{align*}
$$



Let $\hat{\mathbf{y}}=\left[ \begin{matrix}  h(\mathbf{x}^{(1)}, \boldsymbol{\theta}) \\ \vdots\\ h(\mathbf{x}^{(m)}, \boldsymbol{\theta})\end{matrix}\right]$be a column vector of shape $(m,1)$.

Then the vectorized version: $\nabla J(\boldsymbol{\theta}) = \frac{1}{m} X  \left(  \hat{\mathbf{y}}-   \mathbf{y} \right)$  

 

Here, the gradient is a column vector of shape $(k, 1)$ and the shape of $X$ is $k \times m$ ($m$: no. of examples, $k$: no. of features)









Tanh: The tanh function is an alternative to the sigmoid function that is often found to converge faster in practice. The primary difference between tanh and sigmoid is that tanh output ranges from −1 to 1 while the sigmoid ranges from 0 to 1.

$$
\tanh(z) = \frac{\exp(z)-\exp(-z)} {\exp(z)+\exp(-z)} =  \frac{1-\exp(-2z)} {1+\exp(-2z)}=  \frac{2-\left(1+\exp(-2z)\right)} {1+\exp(-2z)}= 2\sigma(2z)-1 
$$
The gradient of $\tanh(z)$ is:
$$
\tanh'(z) =1-  \left(\frac{\exp(z)-\exp(-z)} {\exp(z)+\exp(-z)} \right)^2= 1-\left(\tanh(z)\right)^2
$$




ReLU: The ReLU (Rectified Linear Unit) function is a popular choice of activation since it does not saturate even for larger values of $z$ and
has found much success in computer vision applications:


$$
\operatorname{rect}(z) = \max(z, 0)
$$



The derivative is then the piecewise function:
$$
\operatorname{rect}'(z) = \begin{cases} 1 \quad \text{if } z>0\\0  \quad \text{otherwise}  \end{cases}
$$






#### Useful Identities 





http://web.stanford.edu/class/cs224n/readings/gradient-notes.pdf



The Jacobian matrix (Jacobean formulation) is useful because we can apply the chain rule to a vector-valued function just by multiplying Jacobians.



This section will now go over how to compute the Jacobian for several simple functions. It will provide some useful identities you can apply when taking neural network gradients. 

Suppose $W \in \mathbb{R}^{n\times m}$

- $\mathbf{z} = W \mathbf{x}$, where $\mathbf{x}$ is a column vector of shape $(m, 1)$, what is $\frac{\partial \mathbf{z}}{\partial \mathbf{x}}$? 

  Note
  $$
  z_i = \sum_{k=1}^m W_{ik}x_k \quad \text{and} \quad \frac{\part z_i}{\part \mathbf{x}} = [W]_{  (\text{row }i)}\\
  \frac{\part \mathbf{z}}{\part \mathbf{x}}=W
  $$

  which has a shape of $(n,m)$. Transpose is needed to update parameters.

- $\mathbf{z} = \mathbf{x} W $, where $\mathbf{x}$ is a row vector of shape $(1, n)$, what is $\frac{\partial \mathbf{z}}{\partial \mathbf{x}}$?

  Note
  $$
  z_i = \sum_{k=1}^n x_k W_{ki} \quad \text{and} \quad \frac{\part z_i}{\part \mathbf{x}}= { [W]_{  (\text{column }i)} }^\top \\
  \frac{\part \mathbf{z}}{\part \mathbf{x}}=W^\top
  $$
  
  which has a shape of $(m,n)$. 

- A vector w.r.t. itself, i.e., $\mathbf{z} =  \mathbf{x}$,  $\frac{\partial \mathbf{z}}{\partial \mathbf{x}}=I$

- $\mathbf{z} = W \mathbf{x}$, where $\mathbf{x}$ is a column vector  shape $(m,1)$,  what are $\frac{\partial \mathbf{z}}{\partial W}$ and $\frac{\partial J}{\partial W} $?

  In the deep learning context,  we have a loss function $J$ (a scalar) and want to compute its gradient with respect to a matrix $W \in \mathbb{R} ^{n\times m}$. And if we can arrange the derivatives of $J(W)$ in a $n \times m$ matrix like this:
  $$
  \frac{\partial J} {\partial W} = \left[ \begin{matrix} \frac{\partial J} {\partial W_{11}} &\dots & \frac{\partial J} {\partial W_{m1}} \\ \vdots &\ddots &\vdots \\ \frac{\partial J} {\partial W_{n1}}  & \dots& \frac{\partial J} {\partial W_{nm}}  \end{matrix} \right]
  $$
  Since this matrix has the same shape as $W$, we could just subtract it (times the learning rate) from $W$ when doing gradient descent. 

  And $ \frac{\partial J}{\partial W} = \frac{\partial J}{\partial \mathbf{z}} \frac{\partial \mathbf{z}}{\partial W}$

  This way of arranging the gradients becomes complicated when computing $\frac{\partial \mathbf{z}}{\partial W}$ . Unlike $J$, z is a vector. 

  So if we are trying to rearrange the gradients like with $\frac{\partial J}{\partial W} $ , $\frac{\partial \mathbf{z}}{\partial W}$ would be an $n \times m \times n$ tensor! Luckily, we can avoid the issue by taking the gradient with respect to a single weight $W_{ij}$ instead, because $\frac{\partial \mathbf{z}}{\partial W_{ij}}$ is just a vector,
  $$
  z_i = \sum_{k=1}^m W_{ik}x_k \\
  
  \frac{\part \mathbf{z}}{\part W_{ij}} = \left[\begin{matrix}0 \\ \vdots \\x_j \\ \vdots \\0 \end{matrix}\right] \leftarrow i\text{ th element }
  $$


  where only the $i$-th element ($x_j$) is non-zero.

  Now let’s compute $\frac{\partial J}{\partial W_{ij}}=\frac{\partial J}{\partial \mathbf{z}} \frac{\partial \mathbf{z}}{\partial W_{ij}}$  
$$
  \frac{\part J}{\part W_{ij}}=\frac{\part J}{\part \mathbf{z}} \frac{\part \mathbf{z}}{\part W_{ij}}= x_j  \frac{\part J}{\part z_i}
$$
  To get $\frac{\partial J}{\partial W} $, we want a matrix where entry $(i, j)$ is $x_j  \frac{\partial J}{\partial z_i}$. Since $\frac{\partial J}{\partial \mathbf{z}}$ is a row vector while $\mathbf{x}$ is a column vector,
$$
  \frac{\part J}{\part W} ={ \frac{\part J}{\part \mathbf{z}} } ^\top \mathbf{x}^\top
$$
  which has a shape of $(n, m)$.

- Given $\mathbf{z} = \mathbf{x} W $ where is  $\mathbf{x}$ is a row vector of shape $(1, n)$, a similar computation shows that
  $$
  \frac{\part J}{\part W} = \mathbf{x}^\top{ \frac{\part J}{\part \mathbf{z}} }
  $$
  which has a shape of $(n, m)$.
  
- Cross-entropy loss with respect to logits; that is, $\hat{y} = \operatorname{softmax}(\mathbf{z})$ and $J = \operatorname{CrossEntropy}(y, \hat{y})$, what is $\frac{\partial J}{\partial \mathbf{z}}$?

  

  

Jacobean formulation is great for applying the chain rule: you just have to multiply the Jacobians. 



However, when doing SGD it’s more convenient to follow the convention “the shape of the gradient equals the shape of the parameter” (as we did when computing $\frac{\partial J}{\partial W}$ ). That way subtracting the gradient times the learning rate from the parameters is easy. 

Then, if you compute the gradient of a column vector using Jacobian formulation, you should take the transpose when reporting your final answer so the gradient is a column vector.







### Backward Propagation for Stochastic Gradient Descent





For example $\mathbf{x}^{(i)}$, for a neural network with $L-1$ hidden layer
$$
\mathcal{J} = y^{(i)}\log\left(a^{[L] (i)}\right) + (1-y^{(i)})\log\left(1- a^{[L] (i)}\right)
$$
Given $\frac{\partial \mathcal{J}}{\partial z^{[L] (i)}}$
$$
z^{[L] (i)}= W^{[L]}\mathbf{a}^{[L-1] (i)} + b^{[L-1]}  \\

\frac{\partial \mathcal{J}}{\partial W^{[L]}}  = \frac{\partial \mathcal{J}} {\partial z^{[L] (i)}} \frac{ \partial z^{[L] (i)} }{\partial W^{[L]} } = \frac{\partial \mathcal{J}} {\partial z^{[L] (i)}}  \left( \mathbf{a}^{[L-1] (i)} \right)^\top \\

\frac{\partial \mathcal{J}}{\partial b^{[L]}} = \frac{\partial \mathcal{J}} {\partial z^{[L] (i)}}
\\

\frac{\partial \mathcal{J}}{\partial \mathbf{a}^{[L-1] (i)}}  = \frac{\partial \mathcal{J}} {\partial z^{[L] (i)}} \frac{ \partial z^{[L] (i)} }{\partial\mathbf{a}^{[L-1] (i)} }= \frac{\partial \mathcal{J}} {\partial z^{[L] (i)}}  W^{[L]} \\

\frac{\partial \mathcal{J}}{\partial \mathbf{z}^{[L-1] (i)}}  = \frac{\partial \mathcal{J}}{\partial \mathbf{a}^{[L-1] (i)}}  \frac{ \partial \mathbf{a}^{[L-1] (i)} }{\partial\mathbf{z}^{[L-1] (i)} }
$$


$\frac{ \partial \mathbf{a}^{[L-1] (i)} }{\partial\mathbf{z}^{[L-1] (i)} }$ is a diagonal matrix. multiplication by a diagonal matrix is the same as doing elementwise multiplication by the diagonal.



$\mathbf{z}^{[L-1] (i)}$ is a column vector. but $\frac{\partial \mathcal{J}}{\partial \mathbf{z}^{[L-1] (i)}}$ is a row vector. To report the gradient of a column vector, we take the transpose to make the gradient a column vector as well:  
$$
\boldsymbol{\delta}^{L-1} = \frac{ \partial \mathbf{a}^{[L-1] (i)} }{\partial\mathbf{z}^{[L-1] (i)} } \circ \left( W^{[L]} \right)^\top\frac{\partial \mathcal{J}} {\partial z^{[L] (i)}}
$$
Here, we abuse the notation a bit. Use $\frac{ \partial \mathbf{a}^{[L-1] (i)} }{\partial\mathbf{z}^{[L-1] (i)} }$ to denote a column vector filled by the diagonal elements of the matrix $\frac{ \partial \mathbf{a}^{[L-1] (i)} }{\partial\mathbf{z}^{[L-1] (i)} }$.




$$
\mathbf{z}^{[L-1] (i)}  = W^{[L-1]}\mathbf{a}^{ [L-2](i)} + \mathbf{b}^{[L-1]}\\

\frac{d \mathcal{J}}{d W^{[L]}}  = \frac{\partial \mathcal{J}} {\partial \mathbf{z}^{[L-1] (i)}} \frac{ d\mathbf{z}^{[L-1] (i)} }{d W^{[L]} } = \left( \frac{\partial \mathcal{J}} {\partial \mathbf{z}^{[L-1] (i)}}\right)  ^\top  \left( \mathbf{a}^{[L-2] (i)} \right)^\top = \boldsymbol{\delta}^{L-1}   \left( \mathbf{a}^{[L-2] (i)} \right)^\top \\

\frac{d \mathcal{J}}{d  \mathbf{b}^{[L-1]}} =\frac{\partial \mathcal{J}} {\partial \mathbf{z}^{[L-1] (i)}} I= \frac{\partial \mathcal{J}} {\partial \mathbf{z}^{[L-1] (i)}}
\\

\frac{\partial \mathcal{J}}{\partial \mathbf{a}^{[L-2] (i)}}  = \frac{\partial \mathcal{J}} {\partial \mathbf{z}^{[L-1] (i)}} \frac{ \partial \mathbf{z}^{[L-1] (i)} }{\partial\mathbf{a}^{[L-2] (i)} }= \frac{\partial \mathcal{J}} {\partial \mathbf{z}^{[L] (i)}}  W^{[L-1]} \\

\frac{\partial \mathcal{J}}{\partial \mathbf{z}^{[L-2] (i)}}  = \frac{\partial \mathcal{J}}{\partial \mathbf{a}^{[L-2] (i)}}  \frac{ \partial \mathbf{a}^{[L-1] (i)} }{\partial\mathbf{z}^{[L-1] (i)} }
$$
Similarly, we can define the transpose of $\frac{\partial \mathcal{J}}{\partial \mathbf{z}^{[L-2] (i)}} $ as
$$
\boldsymbol{\delta}^{L-2} = \frac{ \partial \mathbf{a}^{[L-2] (i)} }{\partial\mathbf{z}^{[L-2] (i)} } \circ \left( W^{[L-1]} \right)^\top \boldsymbol{\delta}^{L-1}
$$
$ \mathbf{b}^{[L-1]}$  is a column vector. To report its gradient, we also take the transpose to make the gradient a column vector. It is just $ \boldsymbol{\delta}^{L-1}$.



Recursively multiplying by $W$ at different layers

Gradients often get smaller and smaller as the algorithm progresses down to the lower layers. As a result, the Gradient Descent update leaves the lower layers’ connection weights virtually unchanged, and training never converges to a good solution. We call this the vanishing gradients problem. In some cases, the oppo‐ site can happen: the gradients can grow bigger and bigger until layers get insanely large weight updates and the algorithm diverges. This is the exploding gradients prob‐ lem, which surfaces in recurrent neural networks (see Chapter 15). More generally, deep neural networks suffer from unstable gradients; different layers may learn at widely different speeds.

This unfortunate behavior was empirically observed long ago, and it was one of the reasons deep neural networks were mostly abandoned in the early 2000s. It wasn’t clear what caused the gradients to be so unstable when training a DNN, but some light was shed in a 2010 paper by Xavier Glorot and Yoshua Bengio. The authors found a few suspects, including the combination of the popular logistic sigmoid acti‐ vation function and the weight initialization technique that was most popular at the time (i.e., a normal distribution with a mean of 0 and a standard deviation of 1).

In short, they showed that:

-  with this activation function and this initialization scheme, the variance of the outputs of each layer is much greater than the variance of its inputs. Going forward in the network, the variance keeps increasing after each layer until the activation function saturates at the top layers. 
- This saturation is actually made worse by the fact that the logistic function has a mean of 0.5, not 0 (the hyper‐ bolic tangent function has a mean of 0 and behaves slightly better than the logistic function in deep networks).
- With the logistic activation function, when inputs become large (negative or positive), the function saturates at 0 or 1, with a derivative extremely close to 0. Thus, when backpropagation kicks in it has virtually no gradient to propagate back through the network; and what little gradient exists keeps getting diluted as backpropagation progresses down through the top layers, so there is really nothing left for the lower layers.



### (Batch) Gradient Descent




$$
J = - \frac{1}{m} \sum_{i = 1}^{m} \left(  y^{(i)}\log\left(a^{[2] (i)}\right) + (1-y^{(i)})\log\left(1- a^{[2] (i)}\right)   \right)
$$




for a network with one hidden layer
$$
\frac{\part J}{\part  z^{[2] (i)}} = \frac{\part J}{\part a^{[2] (i)}} \frac{\part a^{[2] (i)}}{\part z^{[2] (i)}} = \frac{1}{m} \left( \frac{1-y^{(i)}}{1-a^{[2] (i)}} - \frac{y^{(i)}}{a^{[2] (i)}}    \right) \frac{\part a^{[2] (i)}}{\part z^{[2] (i)}} \\

\frac{\part a}{\part z}=\frac{\part }{\part z} \left( \frac{e^z}{1+e^z} \right)= a(1-a) \\

\frac{\part J}{\part z^{[2] (i)}} = \frac{1}{m} \left[\left(1-y^{(i)}\right)a^{[2] (i)}- y^{(i)}\left(1-a^{[2] (i)}\right)\right] =\frac{1}{m}\left( a^{[2] (i)}-y^{(i)} \right)
$$

$$
\frac{\part J}{\part \mathbf{ z} ^{[2]}} = \frac{1}{m} \left(  \mathbf{ a} ^{[2]} -\mathbf{y} \right)
$$



the dimensionalty of $\mathbf{a}^{[2]}$ and $\mathbf{y}$ is $1 \times m$
$$
z^{[2] (i)}= W^{[2]}\mathbf{a}^{[1] (i)} + b^{[2]}\\

\frac{\part J}{\part W^{[2]}}=   \sum_{i = 1}^{m} \frac{\part J}{\part z^{[2] (i)}}\frac{\part z^{[2] (i)}}{\part W^{[2]}}=\frac{1}{m} \sum_{i = 1}^{m} (a^{[2] (i)}-y^{(i)}) \left(\mathbf{a}^{[1] (i)}\right)^\top = \frac{1}{m} \left(\mathbf{a}^{[2]}-\mathbf{y}\right) \left(A^{[1]}\right)^\top
$$
the dimensionality of $\frac{dJ}{dW^{[2]}}$ is $1\times k^{[1]}$, the same as that of $ W^{[2]}$.  
$$
\frac{\part J}{\part b^{[2]}}=  \sum_{i = 1}^{m} \frac{\part J}{\part z^{[2] (i)}}\frac{\part z^{[2] (i)}}{\part b^{[2]}} =\frac{1}{m} \sum\limits_{i = 1}^{m} (a^{[2] (i)}-y^{(i)}) =  \frac{1}{m} \left(\mathbf{a}^{[2]}-\mathbf{y}\right) \mathbf{1}
$$
$\mathbf{1}$ is $m \times 1$





the dimensionality of ${A^{[1]}}$ is $k^{[1]} \times m $. $\mathbf{a}^{[1] (i)}$ is $k^{[1]} \times 1$


$$
\frac{\part J}{\part \mathbf{a}^{[1] (i)}}= \frac{ \part J}{\part z^{[2] (i)}}\frac{\part z^{[2] (i)}}{\part \mathbf{a}^{[1] (i)} } =\frac{1}{m}  \left(a^{[2] (i)}-y^{(i)}\right) W^{[2]} \\

\frac{\part J}{\part \mathbf{z}^{[1] (i)}}= \frac{ \part J}{\part \mathbf{a}^{[1] (i)}} \frac{\part \mathbf{a}^{[1] (i)}}{\part \mathbf{z}^{[1] (i)} } =\frac{1}{m}  \left(a^{[2] (i)}-y^{(i)}\right) W^{[2]} \frac{\part \mathbf{a}^{[1] (i)}}{\part \mathbf{z}^{[1] (i)} }
$$
 the dimensionality of $W^{[2]}$ is $1 \times k^{[1]}$ and $\frac{\partial \mathbf{a}^{[1] (i)}}{\partial \mathbf{z}^{[1] (i)} }$ is a $k^{[1]} \times k^{[1]} $ diagonal matrix. 

Note: multiplication by a diagonal matrix is the same as doing elementwise multiplication by the diagonal



Condense $\frac{\partial \mathbf{a}^{[1] (i)}}{\partial \mathbf{z}^{[1] (i)} }$ to a  $k^{[1]} \times 1 $ vector. Stack $m$ derivatives into a $k^{[1]} \times m$ matrix denoted by $\frac{\partial A^{[1]}}{\partial Z^{[1]}}$

organize the derivative matrix w.r.t $\mathbf{Z}^{[1]  }$ to a $k^{[1]} \times m$ matrix
$$
\frac{\part J}{\part \mathbf{Z}^{[1]  }}= \left[ \begin{matrix} \left( \frac{\part J}{\part \mathbf{z}^{[1] (1)}}\right)^\top & \dots   & \left( \frac{\part J}{\part \mathbf{z}^{[1] (m)}}\right)^\top \end{matrix}\right] =\frac{1}{m} \left(W^{[2]}\right)^\top  \left(\mathbf{a}^{[2]}-\mathbf{y}\right) \circ \frac{\part A^{[1]}}{\part Z^{[1]} }
$$
$\circ$ denotes an element-wise multiplication


$$
\mathbf{z}^{[1] (i)}  = W^{[1]}\mathbf{x}^{ (i)} + \mathbf{b}^{[1]}\\

\frac{\part J}{\part W^{[1]}}=   \sum_{i = 1}^{m} \frac{\part J}{\part \mathbf{z}^{[1] (i)}}\frac{\part  \mathbf{z}^{[1] (i)}}{\part W^{[1]}}=  \sum_{i = 1}^{m} \left( \frac{\part J}{\part \mathbf{z}^{[1] (i)}}\right)^\top \left(\mathbf{x}^{(i)}\right)^\top =  \left[ \begin{matrix} \left( \frac{\part J}{\part \mathbf{z}^{[1] (1)}}\right)^\top & \dots   & \left( \frac{\part J}{\part \mathbf{z}^{[1] (m)}}\right)^\top \end{matrix}\right] \left[\begin{matrix}  \left(\mathbf{x}^{(1)}\right)^\top   \\\vdots \\ \left(\mathbf{x}^{(m)}\right)^\top \end{matrix}\right] =  \frac{\part J}{\part  \mathbf{Z}^{[1]  }} X^\top \\
$$
the dimensionality of $X$ is $k \times m$.

 the dimensionality of $W^{[1]}$ is $  k^{[1]}  \times k  $ 

the dimensionality of $\frac{\partial J}{\partial W^{[1]}}$ is the same as  $W^{[1]}$
$$
\frac{\part J}{\part \mathbf{b}^{[1]}}=  \sum_{i = 1}^{m} \frac{\part J}{\part \mathbf{z}^{[1] (i)}}\frac{\part  \mathbf{z}^{[1] (i)}}{\part  \mathbf{b}^{[1]}} =  \sum_{i = 1}^{m} \frac{\part J}{\partial \mathbf{z}^{[1] (i)}}  = \mathbf{1}^\top \left( \frac{\part J}{\part  \mathbf{Z}^{[1]  }} \right)^\top
$$
$\frac{\partial  \mathbf{z}^{[1] (i)}}{\partial  \mathbf{b}^{[1]}} =I$

$\frac{\partial J}{\partial \mathbf{b}^{[1]}}$ is $ 1 \times k^{[1]} $. Take the transpose to make the gradient have the same shape as $\mathbf{b}^{[1]}$.

$\mathbf{1}$ is $m \times 1$

---





for a network with 2 hidden layer $ \mathbf{ a} ^{[3]}  = \hat{\mathbf{y} } $


$$
\frac{\part J}{\part \mathbf{ z} ^{[3]}} = \frac{1}{m} \left(  \mathbf{ a} ^{[3]} -\mathbf{y} \right)
$$


the dimensionality of $\mathbf{a}^{[3]}$ and $\mathbf{y}$ is $1 \times m$
$$
z^{[3] (i)}= W^{[3]}\mathbf{a}^{[2] (i)} + b^{[3]}\\

\frac{\partial J}{\part W^{[3]}}=   \sum_{i = 1}^{m} \frac{\partial J}{\part z^{[3] (i)}}\frac{\partial  z^{[3] (i)}}{\partial  W^{[3]}}=\frac{1}{m} \sum_{i = 1}^{m} \left(a^{[3] (i)}-y^{(i)} \right) \left(\mathbf{a}^{[2] (i)}\right)^\top = \frac{1}{m} \left(\mathbf{a}^{[3]}-\mathbf{y}\right) \left(A^{[2]}\right)^\top
$$
the dimensionality of $\frac{dJ}{dW^{[3]}}$ is $1\times k^{[2]}$, the same as that of $ W^{[3]}$. 
$$
\frac{\part J}{\part b^{[3]}}=  \sum_{i = 1}^{m} \frac{\part J}{\part z^{[3] (i)}}\frac{\part z^{[3] (i)}}{\part b^{[3]}} =\frac{1}{m} \sum\limits_{i = 1}^{m} (a^{[3] (i)}-y^{(i)}) =  \frac{1}{m} \left(\mathbf{a}^{[3]}-\mathbf{y}\right) \mathbf{1}
$$
$\mathbf{1}$ is $m \times 1$



the dimensionality of ${A^{[2]}}$ is $k^{[2]} \times m $. $\mathbf{a}^{[2] (i)}$ is $k^{[2]} \times 1$


$$
\frac{\part J}{\part \mathbf{a}^{[2] (i)}}= \frac{ \part J}{\part z^{[3] (i)}}\frac{\part z^{[3] (i)}}{\part \mathbf{a}^{[2] (i)} } =\frac{1}{m}  \left(a^{[3] (i)}-y^{(i)}\right) W^{[3]} \\

\frac{\part J}{\part \mathbf{z}^{[2] (i)}}= \frac{ \part J}{\part \mathbf{a}^{[2] (i)}} \frac{\part \mathbf{a}^{[2] (i)}}{\part \mathbf{z}^{[2] (i)} } =\frac{1}{m}  \left(a^{[3] (i)}-y^{(i)}\right) W^{[3]} \frac{\part \mathbf{a}^{[2] (i)}}{\part \mathbf{z}^{[2] (i)} }
$$
 the dimensionality of $W^{[3]}$ is $1 \times k^{[2]}   $ and $\frac{\partial \mathbf{a}^{[2] (i)}}{\partial \mathbf{z}^{[2] (i)} }$ is a $k^{[2]} \times k^{[2]} $ diagonal matrix. 



Condense $\frac{\partial \mathbf{a}^{[2] (i)}}{\partial \mathbf{z}^{[2] (i)} }$ to a  $k^{[2]} \times 1 $ vector. Stack $m$ derivatives into a $k^{[2]} \times m$ matrix denoted by $\frac{\partial A^{[2]}}{\partial Z^{[2]} } $

organize the derivative matrix w.r.t $\mathbf{Z}^{[2]  }$ to a $k^{[2]} \times m$ matrix
$$
\frac{\part J}{\part \mathbf{Z}^{[2]  }}= \left[ \begin{matrix} \left( \frac{\partial J}{\part \mathbf{z}^{[2] (1)}}\right)^\top & \dots   & \left( \frac{\part J}{\part \mathbf{z}^{[2] (m)}}\right)^\top \end{matrix}\right] =\frac{1}{m} \left(W^{[3]}\right)^\top  \left(\mathbf{a}^{[3]}-\mathbf{y}\right) \circ \frac{\part A^{[2]}}{\part Z^{[2]} }
$$



$$
\mathbf{z}^{[2] (i)}  = W^{[2]}\mathbf{a}^{ [1](i)} + \mathbf{b}^{[2]}\\

\frac{\partial J}{\part W^{[2]}}=   \sum_{i = 1}^{m} \frac{\part J}{\part \mathbf{z}^{[2] (i)}}\frac{\part  \mathbf{z}^{[2] (i)}}{\part W^{[2]}}=  \sum_{i = 1}^{m} \left( \frac{\part J}{\part \mathbf{z}^{[2] (i)}}\right)^\top \left(\mathbf{a}^{[1] (i)}\right)^\top =  \left[ \begin{matrix} \left( \frac{\part J}{\partial \mathbf{z}^{[1] (1)}}\right)^\top & \dots   & \left( \frac{\part J}{\part \mathbf{z}^{[1] (m)}}\right)^\top \end{matrix}\right] \left[\begin{matrix} \left(\mathbf{a}^{[1] (1)}\right)^\top \\\vdots \\\left(\mathbf{a}^{[1] (m)}\right)^\top \end{matrix}\right] =  \frac{\part J}{\part  \mathbf{Z}^{[2]  }}  \left( A^{[1]}\right)^\top \\ 

\frac{\part J}{\part \mathbf{b}^{[2]}}=  \sum_{i = 1}^{m} \frac{\part J}{\part \mathbf{z}^{[2] (i)}}\frac{\part  \mathbf{z}^{[2] (i)}}{\part  \mathbf{b}^{[2]}} = \mathbf{1}^\top \left( \frac{\part J}{\part  \mathbf{Z}^{[2]  }} \right)^\top
$$




$\frac{\partial J}{\partial \mathbf{z}^{[2] (i)}}$ is $1 \times k^{[2]}$
$$
\frac{\part J}{\part \mathbf{a}^{[1] (i)}}= \frac{ \part J}{\part \mathbf{z}^{[2] (i)}}\frac{\part \mathbf{z}^{[2] (i)}}{\part \mathbf{a}^{[1] (i)} } =\frac{1}{m}  \left(a^{[3] (i)}-y^{(i)}\right) W^{[3]} \frac{\part \mathbf{a}^{[2] (i)}}{\part \mathbf{z}^{[2] (i)} } W^{[2]} \\

\frac{\part J}{\part \mathbf{z}^{[1] (i)}}= \frac{ \part J}{\part \mathbf{a}^{[1] (i)}} \frac{\part \mathbf{a}^{[1] (i)}}{\part \mathbf{z}^{[1] (i)} } =\frac{1}{m}  \left(a^{[3] (i)}-y^{(i)}\right) W^{[3]} \frac{\part \mathbf{a}^{[2] (i)}}{\part \mathbf{z}^{[2] (i)} }  W^{[2]}  \frac{\part \mathbf{a}^{[1] (i)}}{\part \mathbf{z}^{[1] (i)} }
$$




 the dimensionality of $W^{[2]}$ is $  k^{[2]} \times k^{[1]}  $ and $\frac{\partial \mathbf{a}^{[2] (i)}}{\partial \mathbf{z}^{[2] (i)} }$ is a $k^{[1]} \times k^{[1]} $ diagonal matrix. 

Condense $\frac{\partial \mathbf{a}^{[1] (i)}}{\partial \mathbf{z}^{[1] (i)} }$ to a  $k^{[1]} \times 1 $ vector. Stack $m$ derivatives into a $k^{[1]} \times m$ matrix denoted by $\frac{\partial A^{[1]}}{\partial Z^{[1]} } $

organize the derivative matrix w.r.t $\mathbf{Z}^{[1]  }$ to a $k^{[1]} \times m$ matrix
$$
\frac{\part J}{\part \mathbf{Z}^{[1]  }}= \left[ \begin{matrix} \left( \frac{\part J}{\part \mathbf{z}^{[1] (1)}}\right)^\top & \dots   & \left( \frac{\part J}{\part \mathbf{z}^{[1] (m)}}\right)^\top \end{matrix}\right] =\frac{1}{m} \left(W^{[2]}\right)^\top  \left[ \left(W^{[3]}\right)^\top  \left(\mathbf{a}^{[3]}-\mathbf{y}\right) \circ \frac{\part A^{[2]}}{\part Z^{[2]} }\right] \circ \frac{\part A^{[1]}}{\part Z^{[1]} }
$$

$$
\mathbf{z}^{[1] (i)}  = W^{[1]}\mathbf{x}^{ (i)} + \mathbf{b}^{[1]}\\

\frac{\part J}{\part W^{[1]}}=   \sum_{i = 1}^{m} \frac{\part J}{\part \mathbf{z}^{[1] (i)}}\frac{\part  \mathbf{z}^{[1] (i)}}{\part W^{[1]}}=  \sum_{i = 1}^{m} \left( \frac{\part J}{\part \mathbf{z}^{[1] (i)}}\right)^\top \left(\mathbf{x}^{(i)}\right)^\top =  \left[ \begin{matrix} \left( \frac{\part J}{\part \mathbf{z}^{[1] (1)}}\right)^\top & \dots   & \left( \frac{\part J}{\part \mathbf{z}^{[1] (m)}}\right)^\top \end{matrix}\right]\left[\begin{matrix}  \left(\mathbf{x}^{(1)}\right)^\top   \\\vdots \\ \left(\mathbf{x}^{(m)}\right)^\top \end{matrix}\right]  =  \frac{\part J}{\part  \mathbf{Z}^{[1]  }} X^\top \\

\frac{\part J}{\part \mathbf{b}^{[1]}}=  \sum_{i = 1}^{m} \frac{\part J}{\part \mathbf{z}^{[1] (i)}}\frac{\part  \mathbf{z}^{[1] (i)}}{\part  \mathbf{b}^{[1]}}   = \mathbf{1}^\top \left( \frac{\part J}{\part  \mathbf{Z}^{[1]  }} \right)^\top
$$
the dimensionality of $X$ is $k \times m$.

 the dimensionality of $W^{[1]}$ is $  k^{[1]}  \times k  $ 

the dimensionalty of $\frac{\partial J}{\partial W^{[1]}}$ is the same as  $W^{[1]}$

$\mathbf{1}$ is $m \times 1$




$$
\operatorname{softmax}(x)\ =\ \left[\begin{matrix}\  \frac{\exp\left(x_1 \right) }{\sum_{c=1}^C \exp\left(x_c \right)} \\\vdots \\ \frac{\exp\left(x_C \right)}{\sum_{c=1}^n\ \exp\left(x_c \right)  }\ \end{matrix}\right]
$$








**Calibrating the variances with 1/sqrt(n)**. 



One problem with the above suggestion is that the distribution of the outputs from a randomly initialized neuron has a variance that grows with the number of inputs. It turns out that we can normalize the variance of each neuron’s output to 1 by scaling its weight vector by the square root of its *fan-in* (i.e. its number of inputs). That is, the recommended heuristic is to initialize each neuron’s weight vector as: `w = np.random.randn(n) / sqrt(n)`, where `n` is the number of its inputs. This ensures that all neurons in the network initially have approximately the same output distribution and empirically improves the rate of convergence.

The sketch of the derivation is as follows: Consider the inner product $s = \sum_i^n w_i x_i$ between the weights ww and input xx, which gives the raw activation of a neuron before the non-linearity. We can examine the variance of ss:


$$
\begin{align}
\text{Var}(s) &= \text{Var}(\sum_i^n w_ix_i) \\\\
&= \sum_i^n \text{Var}(w_ix_i) \\\\
&= \sum_i^n [E(w_i)]^2\text{Var}(x_i) + [E(x_i)]^2\text{Var}(w_i) + \text{Var}(x_i)\text{Var}(w_i) \\\\
&= \sum_i^n \text{Var}(x_i)\text{Var}(w_i) \\\\
&= \left( n \text{Var}(w) \right) \text{Var}(x)
\end{align}
$$
 

where in the first 2 steps we have used [properties of variance](http://en.wikipedia.org/wiki/Variance). 

In third step we assumed zero mean inputs and weights, so $E[x_i] = E[w_i] = 0$. Note that this is not generally the case: For example ReLU units will have a positive mean. 

In the last step we assumed that all $w_i, x_i$ are identically distributed. From this derivation we can see that if we want $s$ to have the same variance as all of its inputs $x$, then during initialization we should make sure that the variance of every weight ww is $1/n$. And since $\text{Var}(aX) = a^2\text{Var}(X)$ for a random variable $X$ and a scalar $a$, this implies that we should draw from unit gaussian and then scale it by $a = \sqrt{1/n}$, to make its variance $1/n$. This gives the initialization `w = np.random.randn(n) / sqrt(n)`.



A similar analysis is carried out in [Understanding the difficulty of training deep feedforward neural networks](http://jmlr.org/proceedings/papers/v9/glorot10a/glorot10a.pdf) by Glorot et al. In this paper, the authors end up recommending an initialization of the form $\text{Var}(w) = 2/(n_{\text{in}} + n_{\text{out}})$ where $n_{\text{in}},n_{\text{out}}$  are the number of units in the previous layer and the next layer. This is based on a compromise and an equivalent analysis of the backpropagated gradients. A more recent paper on this topic, [Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification](http://arxiv-web3.library.cornell.edu/abs/1502.01852) by He et al., derives an initialization specifically for ReLU neurons, reaching the conclusion that the variance of neurons in the network should be $2.0/n$. This gives the initialization `w = np.random.randn(n) * sqrt(2.0/n)`, and is the current recommendation for use in practice in the specific case of neural networks with ReLU neurons.



**Sparse initialization**. Another way to address the uncalibrated variances problem is to set all weight matrices to zero, but to break symmetry every neuron is randomly connected (with weights sampled from a small gaussian as above) to a fixed number of neurons below it. A typical number of neurons to connect to may be as small as 10.

**Initializing the biases**. It is possible and common to initialize the biases to be zero, since the asymmetry breaking is provided by the small random numbers in the weights. For ReLU non-linearities, some people like to use small constant value such as 0.01 for all biases because this ensures that all ReLU units fire in the beginning and therefore obtain and propagate some gradient. However, it is not clear if this provides a consistent improvement (in fact some results seem to indicate that this performs worse) and it is more common to simply use 0 bias initialization.

**In practice**, the current recommendation is to use ReLU units and use the `w = np.random.randn(n) * sqrt(2.0/n)`, as discussed in [He et al.](http://arxiv-web3.library.cornell.edu/abs/1502.01852).



**Batch Normalization**. A recently developed technique by Ioffe and Szegedy called [Batch Normalization](http://arxiv.org/abs/1502.03167) alleviates a lot of headaches with properly initializing neural networks by explicitly forcing the activations throughout a network to take on a unit gaussian distribution at the beginning of the training. The core observation is that this is possible because normalization is a simple differentiable operation. In the implementation, applying this technique usually amounts to insert the BatchNorm layer immediately after fully connected layers (or convolutional layers, as we’ll soon see), and before non-linearities. We do not expand on this technique here because it is well described in the linked paper, but note that it has become a very common practice to use Batch Normalization in neural networks. In practice networks that use Batch Normalization are significantly more robust to bad initialization. Additionally, batch normalization can be interpreted as doing preprocessing at every layer of the network, but integrated into the network itself in a differentiable manner. Neat!


$$
\boldsymbol{\mu}_B = \frac{1}{m_B} \sum_{i=1}^{m_B} \mathbf{z}^{(i)} \\
\boldsymbol{\sigma}_B^2 = \frac{1}{m_B} \sum_{i=1}^{m_B} \left( \mathbf{z}^{(i)}- \boldsymbol{\mu}_B \right)^2 \\


\mathbf{z}^{(i)}_{\text{norm}}=\frac{\mathbf{z}^{(i)} -\boldsymbol{\mu}_B }{\sqrt{\boldsymbol{\sigma}_B^2 +\epsilon}}\\

\tilde{\mathbf{z}}^{(i)}= \boldsymbol{\gamma} \circ \mathbf{z}^{(i)}_{\text{norm}} + \boldsymbol{\beta}
$$


In this algorithm: 

- $\boldsymbol{\mu}_B$ is the vector of input means, evaluated over the whole mini-batch B (it con‐ tains one mean per input). 
- $\boldsymbol{\sigma}_B$ is the vector of input standard deviations, also evaluated over the whole minibatch (it contains one standard deviation per input). 
- $m_B$ is the number of instances in the mini-batch
- $\mathbf{z}^{(i)}_{\text{norm}}$ is the vector of zero-centered and normalized inputs for instance $i$.
- $\boldsymbol{\gamma}$ is the output scale parameter vector for the layer (it contains one scale parameter per input). 
- $\circ$ represents element-wise multiplication (each input is multiplied by its corresponding output scale parameter). 
- $\boldsymbol{\beta}$ is the output shift (offset) parameter vector for the layer (it contains one offset parameter per input). Each input is offset by its corresponding shift parameter. 
- $\epsilon$ is a tiny number that avoids division by zero (typically $10^{–5}$). This is called a smoothing term.
- $\tilde{\mathbf{z}}^{(i)}$ is the output of the BN operation. It is a rescaled and shifted version of the inputs.

$\boldsymbol{\gamma}$ and $\boldsymbol{\beta}$ are parameters to learn.





### Regularization

There are several ways of controlling the capacity of Neural Networks to prevent overfitting:

**L2 regularization** is perhaps the most common form of regularization. It can be implemented by penalizing the squared magnitude of all parameters directly in the objective. That is, for every weight ww in the network, we add the term $\frac{1}{2} \lambda w^2$ to the objective, where λλ is the regularization strength. It is common to see the factor of $\frac{1}{2}$ in front because then the gradient of this term with respect to the parameter $w$ is simply $\lambda w$ instead of $2 \lambda w$. The L2 regularization has the intuitive interpretation of heavily penalizing peaky weight vectors and preferring diffuse weight vectors. As we discussed in the Linear Classification section, due to multiplicative interactions between weights and inputs this has the appealing property of encouraging the network to use all of its inputs a little rather than some of its inputs a lot. Lastly, notice that during gradient descent parameter update, using the L2 regularization ultimately means that every weight is decayed linearly: `W += -lambda * W` towards zero.

**L1 regularization** is another relatively common form of regularization, where for each weight ww we add the term λ∣w∣λ∣w∣ to the objective. It is possible to combine the L1 regularization with the L2 regularization: $\lambda_1 \mid w \mid + \lambda_2 w^2$ (this is called [Elastic net regularization](http://web.stanford.edu/~hastie/Papers/B67.2 (2005) 301-320 Zou & Hastie.pdf)). The L1 regularization has the intriguing property that it leads the weight vectors to become sparse during optimization (i.e. very close to exactly zero). In other words, neurons with L1 regularization end up using only a sparse subset of their most important inputs and become nearly invariant to the “noisy” inputs. In comparison, final weight vectors from L2 regularization are usually diffuse, small numbers. In practice, if you are not concerned with explicit feature selection, L2 regularization can be expected to give superior performance over L1.

**Max norm constraints**. Another form of regularization is to enforce an absolute upper bound on the magnitude of the weight vector for every neuron and use projected gradient descent to enforce the constraint. In practice, this corresponds to performing the parameter update as normal, and then enforcing the constraint by clamping the weight vector w⃗ w→ of every neuron to satisfy $\Vert \vec{w} \Vert_2 < c$. Typical values of cc are on orders of 3 or 4. Some people report improvements when using this form of regularization. One of its appealing properties is that network cannot “explode” even when the learning rates are set too high because the updates are always bounded.

**Dropout** is an extremely effective, simple and recently introduced regularization technique by Srivastava et al. in [Dropout: A Simple Way to Prevent Neural Networks from Overfitting](http://www.cs.toronto.edu/~rsalakhu/papers/srivastava14a.pdf) (pdf) that complements the other methods (L1, L2, maxnorm). While training, dropout is implemented by only keeping a neuron active with some probability pp (a hyperparameter), or setting it to zero otherwise.
