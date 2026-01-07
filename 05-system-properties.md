# System response properties

The output response will be excited due to an input, such as an impulse or step. The nature of the excitation will depend on transfer function, specifically, the poles and zeros.
This response can be decomposed the response into two parts: the transient response, and the steady-state response.

Assuming the system is *stable* (more on this in a bit), let us introduce and describe the transient response and steady-state response.

## Transient response
The transient response refers to the short-term response of the output; how it immediately responds to the input, such as how quickly it tracks the input, how much it overshoots, or much it oscillates.
For general transfer functions, finding a closed form solution for the transient response can be difficult. One would solve for the time response of the output by taking the inverse Laplace transform, and then extract properties from that analytic expression.
However, for second-order systems, key transient response properties can be immediately inferred by looking at the characteristic equation.

Recall that a standard second-order system (from {eq}`eq-ch4-msd`), is

$$G(s) = K \frac{\omega_n^2}{s^2 + 2 \zeta \omega_n s + \omega_n^2}.$$

Let us complete the square in the denominator,


$$G(s) = K \frac{\omega_n^2}{(s + \sigma)^2 + \omega_n^2(1 - \zeta^2)}, \qquad \sigma = \zeta\omega_n.$$


Let us consider the step response $U(s) = \frac{1}{s}$. Then we have
$X(s) = \frac{1}{s}\frac{K\omega_n^2}{(s + \sigma)^2 + \omega_n^2(1 - \zeta^2)}$

Applying partial fractions, we have

```{math}
:label: eq-5-step-response-time
X(s) = K \left( 1 - e^{-\sigma t} \cos (\omega_n\sqrt{1 - \zeta^2}t) - \frac{\zeta}{\sqrt{1 - \zeta^2}} e^{-\sigma t} \sin(\omega_n\sqrt{1 - \zeta} t) \right)
```

The step response is shown below for various values of $0 < \zeta \leq 1$.

TOOD: Add figure

<!-- Consider the step response for some value of $0 < \zeta \leq 1$.  -->
We can describe some key characteristics of the transient response.

- **Settling time $t_s$:** The time it takes for the step response to reach $X$\% of the final value, where $X$ is usually 1 or 2. For a 1\% settling time, we have $t_s \approx \frac{4.6}{\sigma}$. More generally, for $X$\% settling time, $t_s = \frac{\ln\frac{X}{100}}{-\sigma}$.
- **Rise time $t_r$:** The time it takes for the response to from $10\%$ to $90\%$ of the final value. One could find this value exactly by doing a lot of algebra on {eq}`eq-5-step-response-time` and computing the time for $0.1K$ and $0.9K$. A *rough* estimate for rise time is $t_r \approx \frac{1.8}{\omega_n}$.
- **Peak time $t_p$:** The time it takes for the response it reach its maximum value. Peak time can found by finding the first stationary point of {eq}`eq-5-step-response-time`. That is, take the derivative, set it zero, and find the smallest time $t$ for the stationary points. The peak time has a nice closed-form solution $t_p = \frac{\pi}{\omega_n \sqrt{1 - \zeta^2}}$
- **Maximum overshoot $M_p$:** The magnitude of the system’s maximum
value normalized to its final value (e.g. $M_p$ = 0.3 indicates that the system’s maximum value exceeds its final value by 30%). Similar to peak time, this can be solved of analytically. The resulting closed-form expression is $M_p = e^{-\frac{\pi\zeta}{\sqrt{1 - \zeta^2}}}$.

These properties describe the transient properties of a system's response to a step input. When designing parameters or controllers for a system, you may want the response to have some constraints on, for example, the settling time, and/or maximum overshoot. Using the equations and constraining them based on design requirements, we can effectively constrain value for $\zeta$ and $\omega_n$, or alternatively, the poles of the system. You will see more of this in your feedback controls course.


```{exercise} Impulse response
:label: impulse response derivation

What is the impulse response? That is, find $x(t)$ when $U(s) = 1$.

```


## Steady-state response
After the transient response and the system is settling to its final value, we want to determine what the final value that the system settles to. For a stable system, we can compute the final value of the system response by using the final value theorom.

$$ x_\mathrm{ss} = \lim_{t\rightarrow \infty} x(t) = \lim_{s\rightarrow 0} s X(s)$$

For example, if $G(s) = K \dfrac{\omega_n^2}{s^2 + 2\zeta \omega_n s + \omega_n^2}$, and $U(s) = \frac{1}{s}$ (i.e., a step input), then, assuming response is stable, the final value is,

$$ x_\mathrm{ss} = \lim_{s\rightarrow 0} s U(s)G(s) = \lim_{s\rightarrow 0} s \frac{1}{s} K \dfrac{\omega_n^2}{s^2 + 2\zeta \omega_n s + \omega_n^2} = K$$


## Stability

A key property engineers use to design mechanical systems is system stability. Stability characterizes whether as $t \rightarrow \infty$ the system response will converge to some *equilibrium point*, or will cause the system to diverge, i.e., "shoot to infinity".
Understanding the stability of a system is crucial for understanding the response of mechanical systems to inputs and disturbances.

While there are various notions of stability (e.g., asymptotic, Lyapunov, exponential), we will focus on bounded-input bounded-output (BIBO) stability in this course.


:::{admonition} Definition (BIBO stability)
:class: definition
A system is **bounded-input bounded output (BIBO) stable** if the system response $x(t)$ to any bounded input $u(t) \leq C$ is bounded, i.e. there exists some constant $B$ satisfying
$ \lvert x(t) \rvert \leq B$
:::

For LTI systems, there is a simple check to determine whether it is BIBO stable.

:::{admonition} Definition (BIBO stability for LTI systems)
:class: definition
An LTI system is BIBO stable if *all* of its poles satisfy $\mathrm{Re}\{p_{i}\} < 0$.

Recall that the poles of an LTI system are given by the roots of $D(s)$, or the solutions to $D(s) = 0$, where $D(s)$ is the denominator of the transfer function $ G(s) = \dfrac{N(s)}{D(s)}.$
:::


:::{exercise} Example
:class: example

Is the system characterized by $G(s) = \dfrac{1}{s+a}$ stable for $a > 0$?

We observe that the poles of the system are given by $s + a = 0 \implies p = - a$. Since $\mathrm{Re}\{p\} < 0$, the system is BIBO stable.

Is the system characterized by $G(s) = \dfrac{1}{s} \dfrac{1}{s+a}$ stable for $a > 0$?

We observe that the poles of the system are given by $s(s+a) = 0 \implies p = 0, -a$. Since $\mathrm{Re}\{p\} \nless 0$, the system is not BIBO stable.

Is the system characterized by $G(s) = \dfrac{s}{s^{2} + \omega^{2}}$ stable?

We observe that the poles of the system are given by $s^{2} + \omega^{2} = 0 \implies p = -\pm i\omega$ Since $\mathrm{Re}\{p\} \nless 0$, the system is not BIBO stable.

Is the system characterized by $G(s) = \frac{(s-2)(s+5)}{(s^2 + 6s + 13)(s+4)(s-1)}$ stable?

We observe that the poles of the system are given by $(s^2 + 6s + 13)(s+4)(s-1)=0.$ We know that due to the $(s-1)$ term, one of the poles is $p=1$. Since $\mathrm{Re}\{p\} > 0$, the system is not BIBO stable.

:::


