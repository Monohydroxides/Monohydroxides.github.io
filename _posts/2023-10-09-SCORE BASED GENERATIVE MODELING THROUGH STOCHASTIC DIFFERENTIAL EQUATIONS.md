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

## SCORE-BASED GENERATIVE MODELING THROUGH STOCHASTIC DIFFERENTIAL EQUATIONS

### From discrete to continuous

For $x_{t}$：

When $t$ is fixed, $x_{t}$ can be viewed as a random variable: $x_{t} \sim \mathcal{N}(\sqrt{\bar{\alpha}_{t}}x_{0}, (1 - \bar{\alpha_{t}})I)$;

When $x$ is fixed, we get $x_{T} \to x_{T- 1} \to \cdots \to x_{1} \to x_{0}$, which is a sampling sequence.

We can apply SDE to describe diffusion models.

From the discrete perspective, the processes of diffusion and reconstruction in the diffusion model can be obtained: $\{ x_{t} \}, (t \in \{0, 1, 2, \dots, T\})$;

From the continuous perspective $t \in [0, 1]$, we have: $x_{t} \to x_{t + \delta t}$, $x_{t + \delta t} \to x_{t}$, $\delta t \to 0$.

### Diffuse process under the SDE view

Diffuse process under the SDE view:

$$
dx = f(x, t) dt + g(t) dw,
$$

$f(x, t)$: drift coefficient, $g(t)$: diffusion coefficient, $w$: Brownian motion.

$f(x, t) dt$ denotes a determined transformation process, $g(t) dw$ denotes an random transformation process(based on dynamics).

### The derivation of the reverse process under the SDE language

$$
\begin{aligned}
x_{t + \delta t} - x_{t} &= f(x_{t}, t) \delta t + g(t) \sqrt{\delta t} \varepsilon \\
x_{t + \delta t} &= x_{t} + f(x_{t}, t) \delta t + g(t) \sqrt{\delta t} \varepsilon
\end{aligned}
$$

where $\varepsilon \sim \mathcal{N}(0, I)$.

We obtain $p(x_{t + \delta t} | x_{t}) | \mathcal{N}( x_{t} + f(x_{t}, t) \delta t, g^{2}(t) \delta t I)$.

$$
\begin{aligned}
p(x_{t} | x_{t + \delta t}) &= \frac{p(x_{t + \delta t} | x_{t}) p(x_{t})}{p(x_{t + \delta t})} \\
&= p(x_{t + \delta t} | x_{t}) \exp (\log p(x_{t}) - \log p(x_{t + \delta t})).
\end{aligned}
$$

By using Taylor series, we have:

$$
\begin{aligned}
\log p(x_{t + \delta t}) \approx \log p(x_{t}) + (x_{t + \delta t} - x_{t}) \nabla_{x_{t}} \log p(x_{t}) + \delta t \frac{\partial}{\partial t} \log p(x_{t}).
\end{aligned}
$$

So we get:

$$
\begin{aligned}
p(x_{t} | x_{t + \delta t}) &= \frac{p(x_{t + \delta t} | x_{t}) p(x_{t})}{p(x_{t + \delta t})} \\
&= p(x_{t + \delta t} | x_{t}) \exp (-(x_{t + \delta t} - x_{t}) \nabla_{x_{t}} \log p(x_{t}) - \delta t \frac{\partial}{\partial t} \log p(x_{t})) \\
&\propto \exp \biggr( - \frac{\Vert x_{t + \delta t} - x_{t} - f(x_{t}, t) \delta t \Vert_{2}^{2}}{2g^{2}(t) \delta t} - (x_{t + \delta t} - x_{t}) \nabla_{x_{t}} \log p(x_{t}) - \delta t \frac{\partial}{\partial t} \log p(x_{t}) \biggr) \\
&=  \exp \biggr( - \frac{1}{2g^{2}(t) \delta t} \biggr( (x_{t + \delta t} - x_{t})^{2} - \big( 2f(x_{t}, t) \delta t - 2g^{2}(t)\delta t \nabla_{x_{t}} \log p(x_{t}) \big) (x_{t + \delta t} - x_{t}) \biggr) \\
& \ \ \ \ - \delta t \frac{\partial}{\partial t} \log p(x_{t}) - \frac{f^{2}(x_{t}, t) \delta t}{2g^{2}(t)} \biggr) \\
&= \exp \biggr( - \frac{1}{2g^{2}(t) \delta t} \Vert (x_{t + \delta t} - x_{t}) - \big(f(x_{t}, t) - g^{2}(t) \nabla_{x_{t}} \log p(x_{t}) \big) \delta t \Vert_{2}^{2} - \delta t \frac{\partial}{\partial t} \log p(x_{t}) \\
& \ \ \ \ - \frac{f^{2}(x_{t}, t) \delta t}{2g^{2}(t)} + \frac{\big( f(x_{t}, t) - g^{2}(t) \nabla_{x_{t}} \log p(x_{t}) \big)^{2} \delta t}{2g^{2}(t)} \biggr).
\end{aligned}
$$

For $\delta t \to 0$, we have:

$$
\begin{aligned}
p(x_{t} | x_{t + \delta t}) &= \frac{p(x_{t + \delta t} | x_{t}) p(x_{t})}{p(x_{t + \delta t})} \\
&\approx \exp \biggr( - \frac{1}{2g^{2}(t) \delta t} \Vert (x_{t + \delta t} - x_{t}) - \big(f(x_{t}, t) - g^{2}(t) \nabla_{x_{t}} \log p(x_{t}) \big) \delta t \Vert_{2}^{2} \biggr) \\
&\approx \exp \biggr( - \frac{1}{2g^{2}(t + \delta t) \delta t} \Vert (x_{t + \delta t} - x_{t}) -\big( f(x_{t + \delta t}, t + \delta t) - g^{2}(t + \delta t) \nabla_{x_{t + \delta t}} \log p(x_{t + \delta t}) \delta t \big) \Vert_{2}^{2} \biggr).
\end{aligned}
$$

We can directly get mean: $x_{t + \delta t} - \big( f(x_{t + \delta t}, t + \delta t) - g^{2}(t + \delta t) \nabla_{x_{t + \delta t}} \log p(x_{t + \delta t}) \big) \delta t$, variance: $g^{2}(t + \delta t) \delta t$.

So we get the reverse process under the SDE language:

$$
dx = \biggr( f(x, t) - g^{2}(t) \nabla_{x_{t}} \log p(x_{t}) \biggr) dt + g(t) dw.
$$

### VE-SDE and VP-SDE

VE(variance exploding)-SDE and VP(variance preserving)-SDE have something in common.

VE-SDE: Noise Conditional Score Networks(NCSN), VP-SDE: Denoising diffusion probabilistic model(DDPM).

For NCSN, we have $x_{t} = x_{0} + \sigma_{t} \varepsilon$, we assume $x_{T}$ is gaussian noise, so the $\sigma_{T}$ should be large enough, but for DDPM, $x_{T} = \sqrt{\bar{\alpha_{T}}} x_{0} + \sqrt{1 - \bar{\alpha_{t}}} \varepsilon$, when $x_{T}$ is gaussian noise, we have $\sqrt{\bar{\alpha_{T}}} \approx 0, \sqrt{1 - \bar{\alpha_{t}}} \approx 1$.

For NCSN, the SDE form can be obtained:

$$
\begin{aligned}
x_{t + \delta t} &= x_{t} + \sqrt{\sigma^{2}_{t + \delta t} - \sigma_{t}^{2}} \\
&= x_{t} + \sqrt{\frac{\sigma_{t + \delta t}^{2} - \sigma_{t}^{2}}{\delta t}} \sqrt{\delta t} \varepsilon \\
&= x_{t} + \sqrt{\frac{\delta \sigma_{t}^{2}}{\delta t}} \sqrt{\delta t} \varepsilon.
\end{aligned}
$$

Compare the form with $x_{t + \delta t} = x_{t} + f(x_{t}, t) \delta t + g(t) \sqrt{\delta t} \varepsilon$, we have $f(x_{t}, t) = 0, g(t) = \sqrt{\frac{\delta \sigma_{t}^{2}}{\delta t}}$.

For DDPM, the SDE form can be obtained:

$$
x_{t + 1} = \sqrt{1 - \beta_{t + 1}} x_{t} + \sqrt{\beta_{t + 1}} \varepsilon.
$$

Define $\bar{\beta}_{i} = T\beta_{i}$ and function $\beta: t \to \bar{\beta_{i}}, \beta(\frac{i}{T}) = \bar{\beta_{i}}, \delta = \frac{1}{T}$, we have:

$$
x_{t + 1} = \sqrt{1 - \frac{\bar{\beta_{t + 1}}}{T}} x_{t} + \sqrt{\frac{\bar{\beta_{t + 1}}}{T}} \varepsilon.
$$

So we have:

$$
\begin{aligned}
x_{t + \delta t} &= \sqrt{1 - \beta(t + \delta t) \delta t} x_{t} + \sqrt{\beta(t + \delta t) \delta} \varepsilon \\
&\approx (1 - \frac{1}{2} \beta(t + \delta t) \delta t) x_{t} + \sqrt{\beta(t + \delta t)} \sqrt{\delta t} \varepsilon \\
&\approx (1 - \frac{1}{2}\beta(t) \delta t) x_{t} + \sqrt{\beta(t)} \sqrt{\delta t} \varepsilon.
\end{aligned}
$$

Compare the form with $x_{t + \delta t} = x_{t} + f(x_{t}, t) \delta t + g(t) \sqrt{\delta t} \varepsilon$, we have $f(x_{t}, t) = -\frac{1}{2}\beta(t)x_{t}, g(t) = \sqrt{\beta(t)}$.
