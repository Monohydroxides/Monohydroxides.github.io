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
\frac{df}{d(s,t)}=\frac{\partial f}{\partial <b>.</b>}\frac{\partial <b>.</b>}{\partial(s,t)}
=
\underbrace{
\left[
\begin{matrix}
\frac{\partial f}{\partial x_1} & \frac{\partial f}{\partial x_2}
\end{matrix}
\mathbb{R}ight]
}_{\frac{\partial f}{\partial <b>.</b>}}

\underbrace{
\left[
\begin{matrix}
\frac{\partial x_1}{\partial s} & \frac{\partial x_1}{\partial t} \\
\frac{\partial x_2}{\partial s} & \frac{\partial x_2}{\partial t}
\end{matrix}
\mathbb{R}ight]
}_{\frac{\partial <b>.</b>}{\partial(s,t)}}
$$
**Gradients of Vector-Valued Functions and Jacobian**

Now, we consider gradients of Vector-Valued Functions. For function $<b>.</b>: \mathbb{R}^n \to \mathbb{R}^m$, $<b>.</b>=[x_1, \dots, x_n]^\top$, we can view this function in this way:
$$
<b>.</b>(<b>.</b>) = 

\left[
\begin{matrix}
f_1(<b>.</b>) \\
f_2(<b>.</b>) \\
\vdots \\
f_m(<b>.</b>) \\
\end{matrix}
\mathbb{R}ight]

\in \mathbb{R}^m
$$
We have
$$
\frac{d <b>.</b>(<b>.</b>)}{d <b>.</b>} = 

\left[
\begin{matrix}
\frac{\partial f_1(<b>.</b>)}{\partial x_1} & \dots & \frac{\partial f_1(<b>.</b>)}{\partial x_n} \\
\vdots & & \vdots \\
\frac{\partial f_m(<b>.</b>)}{\partial x_1} & \dots & \frac{\partial f_m(<b>.</b>)}{\partial x_n} \\
\end{matrix}
\mathbb{R}ight]

\in \mathbb{R}^m
$$
Now we can define $\textbf{Jacobian}$: $J=\nabla_{<b>.</b>} <b>.</b>=\frac{d <b>.</b>(<b>.</b>)}{d <b>.</b>}$, $J(i, j)=\frac{f_i}{x_j}$. For special case $f:\mathbb{R}^n \to R$, we process a Jacobian matrix that is a row vector($\mathbb{R}^{1 \times n}$).

> For $<b>.</b>(<b>.</b>)=\bold{Ax}$, $<b>.</b>(<b>.</b>) \in \mathbb{R}^M, A \in R^{M \times N}, <b>.</b> \in \mathbb{R}^N$, to compute $\frac{d <b>.</b>}{d <b>.</b>}$, we determine the partial derivatives of $f$ with respect to every $x_j$:
> $$
> f_i(<b>.</b>)=\sum_{j=1}^{N}A_{ij}x_{j}
> $$
> We get $\frac{\partial f_i}{\partial x_j}=A_{ij}$, collect all the partial derivatives, we can obtain
> $$
> \frac{d <b>.</b>}{d <b>.</b>}=
> 
> \left[
> \begin{matrix}
> \frac{\partial f_1}{\partial x_1} & \dots & \frac{\partial f_1}{\partial x_N} \\
> \vdots & & \vdots \\
> \frac{\partial f_M}{\partial x_1} & \dots & \frac{\partial f_M}{\partial x_N}
> \end{matrix}
> \mathbb{R}ight]
> =
> \left[
> \begin{matrix}
> A_{11} & \dots & A_{1N}\\
> \vdots & & \vdots \\
> A_{M1} & \dots & A_{MN}
> \end{matrix}
> \mathbb{R}ight]
> =
> A
> $$

>For $h: \mathbb{R} \to \mathbb{R}, h(t)=(f \circ g)(t)$, where $f: \mathbb{R}^2 \to \mathbb{R}$, $g: \mathbb{R} \to \mathbb{R}^2$, $f(<b>.</b>)=\exp(x_1 x_2^2)$, $<b>.</b> = \left[ \begin{matrix}x_1 \\ x_2\end{matrix}\mathbb{R}ight]$, $g(t)=\left[ \begin{matrix}t\cos t \\ t\sin t\end{matrix}\mathbb{R}ight]$, we are required to compute gradient of $h$ with respect to $t$.
>
>$$
>\begin{aligned}
>\frac{dh}{dt} &= \frac{\partial f}{\partial <b>.</b>} \frac{\partial <b>.</b>}{\partial t} \\
>&=
>\left[
>\begin{matrix}
>x_2^2\exp(x_1x_2^2) & 2x_1x_2\exp(x_1x_2^2)
>\end{matrix}
>\mathbb{R}ight]
>
>\left[
>\begin{matrix}
>\cos t-t \sin t \\
>\sin t+t \cos t
>\end{matrix}
>\mathbb{R}ight] \\
>
>&= (x_2^2\exp(x_1x_2^2)) \cdot (\cos t-t \sin t) + (2x_1x_2\exp(x_1x_2^2)) \cdot (\sin t+t \cos t)
>\end{aligned}
>$$
>Where $x_1=t\cos t$, $x_2=t\sin t$.

> Consider the linear model:
> $$
> <b>.</b>=<b>.</b>Phi <b>.</b>theta
> $$
> Where $<b>.</b>theta \in \mathbb{R}^D, <b>.</b>Phi \in \mathbb{R}^{N \times D}, <b>.</b> \in \mathbb{R}^N$, define functions:
> $$
> \begin{aligned}
> L(<b>.</b>)&:=\Vert <b>.</b> \Vert^2 \\
> <b>.</b>(<b>.</b>theta)&:=<b>.</b>-<b>.</b>Phi <b>.</b>theta
> \end{aligned}
> $$
> For least-squares loss function, we seek $\frac{\partial L}{\partial <b>.</b>theta}$.
> $$
> \begin{aligned}
> \frac{\partial L}{\partial <b>.</b>theta} &= \frac{\partial L}{\partial <b>.</b>}\frac{\partial <b>.</b>}{\partial <b>.</b>theta} \\
> &= (2 <b>.</b>^{\top})(-<b>.</b>Phi) \\
> \end{aligned}
> $$

**Gradients of Matrices**

Because of the fact that there is a vector-space isomorphism between the space $\mathbb{R}^{m \times n}$ of $m \times n$ matrices and the space $\mathbb{R}^{mn}$ of $mn$ vectors, we can re-shape matrices into vectors of lengths $mn$.

> **Gradient of Vectors with Respect to Matrices**
>
> For $<b>.</b>=<b>.</b> <b>.</b>$, $<b>.</b> \in \mathbb{R}^M, <b>.</b> \in \mathbb{R}^{M \times N}, <b>.</b> \in \mathbb{R}^N$, we seek the gradient $\frac{d <b>.</b>}{d <b>.</b>}$.
> First of all, we should know the dimension: $\frac{d <b>.</b>}{d <b>.</b>} \in \mathbb{R}^{M \times (M \times N)}$.
> $$
> \frac{d <b>.</b>}{d <b>.</b>} = 
> 
> \left[
> \begin{matrix}
> \frac{df_1}{d <b>.</b>} \\
> \vdots \\
> \frac{df_m}{d <b>.</b>}
> \end{matrix}
> \mathbb{R}ight]
> $$
> For $f_i$, we obtain $f_i=\sum_{j=1}^{N}<b>.</b>_{ij}<b>.</b>_j$, $\frac{\partial f_i}{\partial <b>.</b>} \in \mathbb{R}^{1 \times (M \times N)}$.
>
> By vectorization, we get $\frac{\partial f_i}{\partial <b>.</b>}=\left[\begin{matrix} 0 & \dots & 0 \\ \vdots & & \vdots \\ 0 & \dots & 0 \\ x_1 & \dots & x_N \\ 0 & \dots & 0 \\ \vdots & & \vdots \\ 0 & \dots & 0 \end{matrix}\mathbb{R}ight] \in \mathbb{R}^{1 \times (M \times N)}$, where $<b>.</b>$ lays on the $i$ th row.

> **Gradient of Matrices with Respect to Matrices**
>
> Consider a matrix $<b>.</b> \in \mathbb{R}^{M \times N}$ and $<b>.</b>: \mathbb{R}^{M \times N} \to \mathbb{R}^{N \times N}$, $<b>.</b>(<b>.</b>)=<b>.</b>^\top <b>.</b>=:<b>.</b> \in \mathbb{R}^{N \times N}$, we seek $\frac{d <b>.</b>}{d <b>.</b>}$.
>
> First of all, $\frac{d <b>.</b>}{d <b>.</b>} \in \mathbb{R}^{(N \times N) \times (M \times N)}$.
>
> Similarly, for $<b>.</b>_{pq}$, we have $\frac{dK_{pq}}{d<b>.</b>} \in \mathbb{R}^{1 \times (M \times N)}$, and $<b>.</b>_{pq} = \sum_{j=1}^{N}R_{jp} \cdot R_{jq}$.
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

$\frac{\partial}{\partial <b>.</b>} <b>.</b>(<b>.</b>)^\top = \left(\frac{\partial <b>.</b>(<b>.</b>)}{\partial <b>.</b>}\mathbb{R}ight)^\top$

$\frac{\partial}{\partial <b>.</b>} \text{tr}(<b>.</b>(<b>.</b>)) = \text{tr}\left(\frac{\partial <b>.</b>(<b>.</b>)}{\partial <b>.</b>}\mathbb{R}ight)$

$\frac{\partial}{\partial <b>.</b>} \det(<b>.</b>(<b>.</b>)) = \det(<b>.</b>(<b>.</b>))\text{tr}\left(<b>.</b>(<b>.</b>)^{-1}\frac{\partial <b>.</b>(<b>.</b>)}{\partial <b>.</b>}\mathbb{R}ight)$

$\frac{\partial}{\partial <b>.</b>} <b>.</b>(<b>.</b>)^{-1} = -<b>.</b>(<b>.</b>)^{-1} \frac{\partial <b>.</b>(<b>.</b>)}{\partial <b>.</b>}<b>.</b>(<b>.</b>)^{-1}$

$\frac{\partial <b>.</b>^\top <b>.</b>^{-1} <b>.</b>}{\partial <b>.</b>}=-(<b>.</b>^{-1})^\top<b>.</b> <b>.</b>^\top(<b>.</b>^{-1})^\top$

$\frac{\partial <b>.</b>^\top <b>.</b>}{\partial <b>.</b>} = <b>.</b>^\top$

$\frac{\partial <b>.</b>^\top <b>.</b>}{\partial <b>.</b>} = <b>.</b>^\top$

$\frac{\partial <b>.</b>^\top <b>.</b> <b>.</b>}{\partial <b>.</b>}=<b>.</b> <b>.</b>^\top$

$\frac{\partial <b>.</b>^\top <b>.</b> <b>.</b>}{\partial <b>.</b>}=<b>.</b>^\top(<b>.</b> + <b>.</b>^\top)$

$\frac{\partial}{\partial <b>.</b>}(<b>.</b> - <b>As</b>)^\top<b>.</b>(<b>.</b> - <b>As</b>) = -2(<b>.</b> - <b>As</b>^\top<b>WA</b>)$, where $<b>.</b>$ is symmetric matrix.
