<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

# Vector calculus

We use the **numerator layout** of the derivative.

**Chain rule**

It is easy to calculate gradients of $f: \mathbb{R} \to \mathbb{R}$. Now we consider the chain rule obtained by matrix multiplication:

$$
\frac{df}{d(s,t)}=\frac{\partial f}{\partial \boldsymbol x}\frac{\partial \boldsymbol x}{\partial(s,t)}
=
\underbrace{
\left[
\begin{matrix}
\frac{\partial f}{\partial x_1} & \frac{\partial f}{\partial x_2}
\end{matrix}
\right]
}_{\frac{\partial f}{\partial \boldsymbol x}}
\underbrace{
\left[
\begin{matrix}
\frac{\partial x_1}{\partial s} & \frac{\partial x_1}{\partial t} \\
\frac{\partial x_2}{\partial s} & \frac{\partial x_2}{\partial t}
\end{matrix}
\right]
}_{\frac{\partial \boldsymbol x}{\partial(s,t)}}
$$

**Gradients of Vector-Valued Functions and Jacobian**

Now, we consider gradients of Vector-Valued Functions. For function $\boldsymbol f: \mathbb{R}^n \to \mathbb{R}^m$, $\boldsymbol x=[x_1, \dots, x_n]^\top$, we can view this function in this way:

$$
\boldsymbol f(\boldsymbol x) = 
\left[
\begin{matrix}
f_1(\boldsymbol x) \\
f_2(\boldsymbol x) \\
\vdots \\
f_m(\boldsymbol x) \\
\end{matrix}
\right]
\in \mathbb{R}^m
$$

We have

$$
\frac{d \boldsymbol f(\boldsymbol x)}{d \boldsymbol x} = 
\left[
\begin{matrix}
\frac{\partial f_1(\boldsymbol x)}{\partial x_1} & \dots & \frac{\partial f_1(\boldsymbol x)}{\partial x_n} \\
\vdots & & \vdots \\
\frac{\partial f_m(\boldsymbol x)}{\partial x_1} & \dots & \frac{\partial f_m(\boldsymbol x)}{\partial x_n} \\
\end{matrix}
\right]
\in \mathbb{R}^m
$$

Now we can define $\textbf{Jacobian}$: $J=\nabla_{\boldsymbol x} \boldsymbol f=\frac{d \boldsymbol f(\boldsymbol x)}{d \boldsymbol x}$, $J(i, j)=\frac{f_i}{x_j}$. For special case $f:\mathbb{R}^n \to R$, we process a Jacobian matrix that is a row vector($\mathbb{R}^{1 \times n}$).

> For $\boldsymbol f(\boldsymbol x)=\boldsymbol{Ax}$, $\boldsymbol f(\boldsymbol x) \in \mathbb{R}^M, A \in R^{M \times N}, \boldsymbol x \in \mathbb{R}^N$, to compute $\frac{d \boldsymbol f}{d \boldsymbol x}$, we determine the partial derivatives of $f$ with respect to every $x_j$:
> 
> $$
> f_i(\boldsymbol x)=\sum_{j=1}^{N}A_{ij}x_{j}
> $$
> 
> We get $\frac{\partial f_i}{\partial x_j}=A_{ij}$, collect all the partial derivatives, we can obtain
> 
> $$
> \frac{d \boldsymbol f}{d \boldsymbol x}=
> \left[
> \begin{matrix}
> \frac{\partial f_1}{\partial x_1} & \dots & \frac{\partial f_1}{\partial x_N} \\
> \vdots & & \vdots \\
> \frac{\partial f_M}{\partial x_1} & \dots & \frac{\partial f_M}{\partial x_N}
> \end{matrix}
> \right]
> =
> \left[
> \begin{matrix}
> A_{11} & \dots & A_{1N}\\
> \vdots & & \vdots \\
> A_{M1} & \dots & A_{MN}
> \end{matrix}
> \right]
> =
> A
> $$

>For $h: \mathbb{R} \to \mathbb{R}, h(t)=(f \circ g)(t)$, where $f: \mathbb{R}^2 \to \mathbb{R}$, $g: \mathbb{R} \to \mathbb{R}^2$, $f(\boldsymbol x)=\exp(x_1 x_2^2)$, $\boldsymbol x = \left[ \begin{matrix}x_1 \\ x_2\end{matrix}\right]$, $g(t)=\left[ \begin{matrix}t\cos t \\ t\sin t\end{matrix}\right]$, we are required to compute gradient of $h$ with respect to $t$.
>
>$$
>\begin{aligned}
>\frac{dh}{dt} &= \frac{\partial f}{\partial \boldsymbol x} \frac{\partial \boldsymbol x}{\partial t} \\
>&=
>\left[
>\begin{matrix}
>x_2^2\exp(x_1x_2^2) & 2x_1x_2\exp(x_1x_2^2)
>\end{matrix}
>\right]
>\left[
>\begin{matrix}
>\cos t-t \sin t \\
>\sin t+t \cos t
>\end{matrix}
>\right] \\
>&= (x_2^2\exp(x_1x_2^2)) \cdot (\cos t-t \sin t) + (2x_1x_2\exp(x_1x_2^2)) \cdot (\sin t+t \cos t)
>\end{aligned}
>$$
>
>Where $x_1=t\cos t$, $x_2=t\sin t$.

> Consider the linear model:
> 
> $$
> \boldsymbol y=\boldsymbol \Phi \boldsymbol \theta
> $$
> 
> Where $\boldsymbol \theta \in \mathbb{R}^D, \boldsymbol \Phi \in \mathbb{R}^{N \times D}, \boldsymbol y \in \mathbb{R}^N$, define functions:
> 
> $$
> \begin{aligned}
> L(\boldsymbol e)&:=\Vert \boldsymbol e \Vert^2 \\
> \boldsymbol e(\boldsymbol \theta)&:=\boldsymbol y-\boldsymbol \Phi \boldsymbol \theta
> \end{aligned}
> $$
> 
> For least-squares loss function, we seek $\frac{\partial L}{\partial \boldsymbol \theta}$.
> 
> $$
> \begin{aligned}
> \frac{\partial L}{\partial \boldsymbol \theta} &= \frac{\partial L}{\partial \boldsymbol e}\frac{\partial \boldsymbol e}{\partial \boldsymbol \theta} \\
> &= (2 \boldsymbol e^{\top})(-\boldsymbol \Phi) \\
> \end{aligned}
> $$

**Gradients of Matrices**

Because of the fact that there is a vector-space isomorphism between the space $\mathbb{R}^{m \times n}$ of $m \times n$ matrices and the space $\mathbb{R}^{mn}$ of $mn$ vectors, we can re-shape matrices into vectors of lengths $mn$.

> **Gradient of Vectors with Respect to Matrices**
>
> For $\boldsymbol f=\boldsymbol A \boldsymbol x$, $\boldsymbol f \in \mathbb{R}^M, \boldsymbol A \in \mathbb{R}^{M \times N}, \boldsymbol x \in \mathbb{R}^N$, we seek the gradient $\frac{d \boldsymbol f}{d \boldsymbol A}$.
> First of all, we should know the dimension: $\frac{d \boldsymbol f}{d \boldsymbol A} \in \mathbb{R}^{M \times (M \times N)}$.
> 
> $$
> \frac{d \boldsymbol f}{d \boldsymbol A} = 
> \left[
> \begin{matrix}
> \frac{df_1}{d \boldsymbol A} \\
> \vdots \\
> \frac{df_m}{d \boldsymbol A}
> \end{matrix}
> \right]
> $$
> 
> For $f_i$, we obtain $f_i=\sum_{j=1}^{N}\boldsymbol A_{ij}\boldsymbol x_j$, $\frac{\partial f_i}{\partial \boldsymbol A} \in \mathbb{R}^{1 \times (M \times N)}$.
>
> By vectorization, we get 
> 
> $$
> \frac{\partial f_i}{\partial \boldsymbol A}=\left[\begin{matrix} 0 & \dots & 0 \\ \vdots & & \vdots \\ 0 & \dots & 0 \\ x_1 & \dots & x_N \\ 0 & \dots & 0 \\ \vdots & & \vdots \\ 0 & \dots & 0 \end{matrix}\right] \in \mathbb{R}^{1 \times (M \times N)}
> $$
> 
> where $\boldsymbol x$ lays on the $i$ th row.

> **Gradient of Matrices with Respect to Matrices**
>
> Consider a matrix $\boldsymbol R \in \mathbb{R}^{M \times N}$ and $\boldsymbol f: \mathbb{R}^{M \times N} \to \mathbb{R}^{N \times N}$, $\boldsymbol f(\boldsymbol R)=\boldsymbol R^\top \boldsymbol R=:\boldsymbol K \in \mathbb{R}^{N \times N}$, we seek $\frac{d \boldsymbol K}{d \boldsymbol R}$.
>
> First of all, $\frac{d \boldsymbol K}{d \boldsymbol R} \in \mathbb{R}^{(N \times N) \times (M \times N)}$.
>
> Similarly, for $\boldsymbol K_{pq}$, we have $\frac{dK_{pq}}{d\boldsymbol R} \in \mathbb{R}^{1 \times (M \times N)}$, and $\boldsymbol K_{pq} = \sum_{j=1}^{N}R_{jp} \cdot R_{jq}$.
>
> So we have
> 
> $$
> \frac{\partial K_{pq}}{\partial R_{rs}}=
> \begin{equation}
> \begin{cases}
> 2R_{rq}, & s = p \wedge p = q; \\
> R_{rq}, & s = p \wedge p \neq q; \\
> R_{rp}, & s = q \wedge p \neq q; \\
> 0, & \text{otherwise}
> \end{cases}
> \end{equation}
> $$

**Useful Identities for Computing Gradients**

$\frac{\partial}{\partial \boldsymbol X} \boldsymbol f(\boldsymbol X)^\top = \left(\frac{\partial \boldsymbol f(\boldsymbol X)}{\partial \boldsymbol X}\right)^\top$

$\frac{\partial}{\partial \boldsymbol X} \text{tr}(\boldsymbol f(\boldsymbol X)) = \text{tr}\left(\frac{\partial \boldsymbol f(\boldsymbol X)}{\partial \boldsymbol X}\right)$

$\frac{\partial}{\partial \boldsymbol X} \det(\boldsymbol f(\boldsymbol X)) = \det(\boldsymbol f(\boldsymbol X))\text{tr}\left(\boldsymbol f(\boldsymbol X)^{-1}\frac{\partial \boldsymbol f(\boldsymbol X)}{\partial \boldsymbol X}\right)$

$\frac{\partial}{\partial \boldsymbol X} \boldsymbol f(\boldsymbol X)^{-1} = -\boldsymbol f(\boldsymbol X)^{-1} \frac{\partial \boldsymbol f(\boldsymbol X)}{\partial \boldsymbol X}\boldsymbol f(\boldsymbol X)^{-1}$

$\frac{\partial \boldsymbol a^\top \boldsymbol X^{-1} \boldsymbol b}{\partial \boldsymbol X}=-(\boldsymbol X^{-1})^\top\boldsymbol a \boldsymbol b^\top(\boldsymbol X^{-1})^\top$

$\frac{\partial \boldsymbol x^\top \boldsymbol a}{\partial \boldsymbol x} = \boldsymbol a^\top$

$\frac{\partial \boldsymbol a^\top \boldsymbol x}{\partial \boldsymbol x} = \boldsymbol a^\top$

$\frac{\partial \boldsymbol a^\top \boldsymbol X \boldsymbol b}{\partial \boldsymbol X}=\boldsymbol a \boldsymbol b^\top$

$\frac{\partial \boldsymbol x^\top \boldsymbol B \boldsymbol x}{\partial \boldsymbol x}=\boldsymbol x^\top(\boldsymbol B + \boldsymbol B^\top)$

$\frac{\partial}{\partial \boldsymbol s}(\boldsymbol x - \boldsymbol{As})^\top\boldsymbol W(\boldsymbol x - \boldsymbol{As}) = -2(\boldsymbol x - \boldsymbol{As}^\top\boldsymbol{WA})$, where $\boldsymbol W$ is symmetric matrix.
