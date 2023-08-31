# Vector calculus

We use the **numerator layout** of the derivative.

**Chain rule**

It is easy to calculate gradients of $f: \R \to \R$. Now we consider the chain rule obtained by matrix multiplication:
$$
\frac{df}{d(s,t)}=\frac{\partial f}{\partial \bold x}\frac{\partial \bold x}{\partial(s,t)}
=
\underbrace{
\left[
\begin{matrix}
\frac{\partial f}{\partial x_1} & \frac{\partial f}{\partial x_2}
\end{matrix}
\right]
}_{\frac{\partial f}{\partial \bold x}}

\underbrace{
\left[
\begin{matrix}
\frac{\partial x_1}{\partial s} & \frac{\partial x_1}{\partial t} \\
\frac{\partial x_2}{\partial s} & \frac{\partial x_2}{\partial t}
\end{matrix}
\right]
}_{\frac{\partial \bold x}{\partial(s,t)}}
$$
**Gradients of Vector-Valued Functions and Jacobian**

Now, we consider gradients of Vector-Valued Functions. For function $\bold f: \R^n \to \R^m$, $\bold x=[x_1, \dots, x_n]^\top$, we can view this function in this way:
$$
\bold f(\bold x) = 

\left[
\begin{matrix}
f_1(\bold x) \\
f_2(\bold x) \\
\vdots \\
f_m(\bold x) \\
\end{matrix}
\right]

\in \R^m
$$
We have
$$
\frac{d \bold f(\bold x)}{d \bold x} = 

\left[
\begin{matrix}
\frac{\partial f_1(\bold x)}{\partial x_1} & \dots & \frac{\partial f_1(\bold x)}{\partial x_n} \\
\vdots & & \vdots \\
\frac{\partial f_m(\bold x)}{\partial x_1} & \dots & \frac{\partial f_m(\bold x)}{\partial x_n} \\
\end{matrix}
\right]

\in \R^m
$$
Now we can define $\textbf{Jacobian}$: $J=\nabla_{\bold x} \bold f=\frac{d \bold f(\bold x)}{d \bold x}$, $J(i, j)=\frac{f_i}{x_j}$. For special case $f:\R^n \to R$, we process a Jacobian matrix that is a row vector($\R^{1 \times n}$).

> For $\bold f(\bold x)=\bold{Ax}$, $\bold f(\bold x) \in \R^M, A \in R^{M \times N}, \bold x \in \R^N$, to compute $\frac{d \bold f}{d \bold x}$, we determine the partial derivatives of $f$ with respect to every $x_j$:
> $$
> f_i(\bold x)=\sum_{j=1}^{N}A_{ij}x_{j}
> $$
> We get $\frac{\partial f_i}{\partial x_j}=A_{ij}$, collect all the partial derivatives, we can obtain
> $$
> \frac{d \bold f}{d \bold x}=
> 
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

>For $h: \R \to \R, h(t)=(f \circ g)(t)$, where $f: \R^2 \to \R$, $g: \R \to \R^2$, $f(\bold x)=\exp(x_1 x_2^2)$, $\bold x = \left[ \begin{matrix}x_1 \\ x_2\end{matrix}\right]$, $g(t)=\left[ \begin{matrix}t\cos t \\ t\sin t\end{matrix}\right]$, we are required to compute gradient of $h$ with respect to $t$.
>
>$$
>\begin{aligned}
>\frac{dh}{dt} &= \frac{\partial f}{\partial \bold x} \frac{\partial \bold x}{\partial t} \\
>&=
>\left[
>\begin{matrix}
>x_2^2\exp(x_1x_2^2) & 2x_1x_2\exp(x_1x_2^2)
>\end{matrix}
>\right]
>
>\left[
>\begin{matrix}
>\cos t-t \sin t \\
>\sin t+t \cos t
>\end{matrix}
>\right] \\
>
>&= (x_2^2\exp(x_1x_2^2)) \cdot (\cos t-t \sin t) + (2x_1x_2\exp(x_1x_2^2)) \cdot (\sin t+t \cos t)
>\end{aligned}
>$$
>Where $x_1=t\cos t$, $x_2=t\sin t$.

> Consider the linear model:
> $$
> \bold y=\bold \Phi \bold \theta
> $$
> Where $\bold \theta \in \R^D, \bold \Phi \in \R^{N \times D}, \bold y \in \R^N$, define functions:
> $$
> \begin{aligned}
> L(\bold e)&:=\Vert \bold e \Vert^2 \\
> \bold e(\bold \theta)&:=\bold y-\bold \Phi \bold \theta
> \end{aligned}
> $$
> For least-squares loss function, we seek $\frac{\partial L}{\partial \bold \theta}$.
> $$
> \begin{aligned}
> \frac{\partial L}{\partial \bold \theta} &= \frac{\partial L}{\partial \bold e}\frac{\partial \bold e}{\partial \bold \theta} \\
> &= (2 \bold e^{\top})(-\bold \Phi) \\
> \end{aligned}
> $$

**Gradients of Matrices**

Because of the fact that there is a vector-space isomorphism between the space $\R^{m \times n}$ of $m \times n$ matrices and the space $\R^{mn}$ of $mn$ vectors, we can re-shape matrices into vectors of lengths $mn$.

> **Gradient of Vectors with Respect to Matrices**
>
> For $\bold f=\bold A \bold x$, $\bold f \in \R^M, \bold A \in \R^{M \times N}, \bold x \in \R^N$, we seek the gradient $\frac{d \bold f}{d \bold A}$.
> First of all, we should know the dimension: $\frac{d \bold f}{d \bold A} \in \R^{M \times (M \times N)}$.
> $$
> \frac{d \bold f}{d \bold A} = 
> 
> \left[
> \begin{matrix}
> \frac{df_1}{d \bold A} \\
> \vdots \\
> \frac{df_m}{d \bold A}
> \end{matrix}
> \right]
> $$
> For $f_i$, we obtain $f_i=\sum_{j=1}^{N}\bold A_{ij}\bold x_j$, $\frac{\partial f_i}{\partial \bold A} \in \R^{1 \times (M \times N)}$.
>
> By vectorization, we get $\frac{\partial f_i}{\partial \bold A}=\left[\begin{matrix} 0 & \dots & 0 \\ \vdots & & \vdots \\ 0 & \dots & 0 \\ x_1 & \dots & x_N \\ 0 & \dots & 0 \\ \vdots & & \vdots \\ 0 & \dots & 0 \end{matrix}\right] \in \R^{1 \times (M \times N)}$, where $\bold x$ lays on the $i$ th row.

> **Gradient of Matrices with Respect to Matrices**
>
> Consider a matrix $\bold R \in \R^{M \times N}$ and $\bold f: \R^{M \times N} \to \R^{N \times N}$, $\bold f(\bold R)=\bold R^\top \bold R=:\bold K \in \R^{N \times N}$, we seek $\frac{d \bold K}{d \bold R}$.
>
> First of all, $\frac{d \bold K}{d \bold R} \in \R^{(N \times N) \times (M \times N)}$.
>
> Similarly, for $\bold K_{pq}$, we have $\frac{dK_{pq}}{d\bold R} \in \R^{1 \times (M \times N)}$, and $\bold K_{pq} = \sum_{j=1}^{N}R_{jp} \cdot R_{jq}$.
>
> So we have
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

$\frac{\partial}{\partial \bold X} \bold f(\bold X)^\top = \left(\frac{\partial \bold f(\bold X)}{\partial \bold X}\right)^\top$

$\frac{\partial}{\partial \bold X} \text{tr}(\bold f(\bold X)) = \text{tr}\left(\frac{\partial \bold f(\bold X)}{\partial \bold X}\right)$

$\frac{\partial}{\partial \bold X} \det(\bold f(\bold X)) = \det(\bold f(\bold X))\text{tr}\left(\bold f(\bold X)^{-1}\frac{\partial \bold f(\bold X)}{\partial \bold X}\right)$

$\frac{\partial}{\partial \bold X} \bold f(\bold X)^{-1} = -\bold f(\bold X)^{-1} \frac{\partial \bold f(\bold X)}{\partial \bold X}\bold f(\bold X)^{-1}$

$\frac{\partial \bold a^\top \bold X^{-1} \bold b}{\partial \bold X}=-(\bold X^{-1})^\top\bold a \bold b^\top(\bold X^{-1})^\top$

$\frac{\partial \bold x^\top \bold a}{\partial \bold x} = \bold a^\top$

$\frac{\partial \bold a^\top \bold x}{\partial \bold x} = \bold a^\top$

$\frac{\partial \bold a^\top \bold X \bold b}{\partial \bold X}=\bold a \bold b^\top$

$\frac{\partial \bold x^\top \bold B \bold x}{\partial \bold x}=\bold x^\top(\bold B + \bold B^\top)$

$\frac{\partial}{\partial \bold s}(\bold x - \bold{As})^\top\bold W(\bold x - \bold{As}) = -2(\bold x - \bold{As}^\top\bold{WA})$, where $\bold W$ is symmetric matrix.

