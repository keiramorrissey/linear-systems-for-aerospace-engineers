# Linear Time-Invariant Systems

As discussed in Chapter 1, we are restricting our focus to finite-dimensional **linear time-invariant (LTI)** dynamical systems.
To further simplify our dynamics model, let us consider the case without a disturbance input. For now, let the control input $\mathbf{u}$ represent some external input and this can be used to account of any inputs to the system either from a human, controller, or the environment.

Additionally, while our definitions is applicable to multi-dimensional systems, let us for now consider one-dimension system with only a single control input.

It is often helpful to view dynamical systems via an *input-output* map $T\{\cdot\}$ that accepts an input signal $u(t)$ and generates an output signal $x(t)$, where $t \in \mathbb{R}_{\geq 0}$ is the time index. For example, we can apply some force to a mass attached to a spring for a 5 seconds and measure how much distance the mass displaces over the next 10 seconds.
Pictorially, we can represent this input-output relationship via a block diagram.

(TODO: add block diagram figure)


:::{admonition} Definition (LTI systems)
:class: definition

A **linear, time-invariant (LTI) system** $T\{\cdot\}$ is an input-output mapping that has the following properties:
- **Homogeneity.** Let $x(t)$ be the system response to input $u(t)$: $x(t) = T\{u(t)\}$. Then $\alpha u(t) \rightarrow \alpha x(t) ,\, \alpha \in \mathbb{R}$. Equivalently, $T\{\alpha u(t)\} = \alpha x(t)$. This means scaling the input by $\alpha$ will scale the output by $\alpha$ as well.
- **Superposition.** Let $x_{1}(t)$ and $x_{2}(t)$ be the responses to $u_{1}(t)$, $u_{2}(t)$ respectively. Then $u_{1}(t) + u_{2}(t) \rightarrow x_{1}(t) + x_{2}(t)$. Equivalently: $T\{u_{1}(t)$ + $u_{2}(t)\} = x_{1}(t) + x_{2}(t)$. This means summing two inputs will output the sum of the corresponding outputs of each input.
- **Time-invariance.** Let $x(t)$ be the system response to input $u(t)$. Then $u(t - \tau) \rightarrow x(t - \tau) ,\, \tau \in \mathbb{R}$. Equivalently: $T\{u(t - \tau)\} = x(t - \tau)$. This means that delaying the input also delays the output and would have behaved exactly the same had there been not delay.
:::



<i class="fas fa-video"></i> Check out this [Youtube video](https://www.youtube.com/watch?v=3eDDTFcSC_Y) by Brian Douglas which offers a really nice visualization as to why LTI properties are nice!


## Convolution integral

We are interested in what the output $x(t)$ of an LTI system is like given an input signal $u(t)$. Before we can write down what $x(t)$ is as a function of $u(t)$, let us first consider the response of $x(t)$ when $u(t)$ is a "small tap" which can be represented by a Dirac delta function.

:::{admonition} Definition (Dirac delta function)
:class: definition

The Dirac delta function, denoted as $\delta(t)$, is a mathematical construct that models an idealized impulse or "tap" at a specific point in time, typically $t = 0$. It is defined to be zero everywhere except at $t = 0$, where it is infinitely high in such a way that its integral over the entire real line is equal to one. In other words, $\int_{-\infty}^{\infty} \delta(t) \, dt = 1$.

The Dirac delta function is a generalized function, or distribution, rather than a regular function in the traditional sense. It is defined by how it acts under an integral, rather than by its value at individual points.

The Dirac delta function is widely used in engineering and physics to represent instantaneous impulses and to analyze systems' responses to such inputs.
:::

If $u(t) = \delta(t)$, then let $\tilde{x}(t)$ denote the output response of an LTI due to an impulse input.

(TODO: add figure)

Now, consider what the response would be to the following scenarios:

- Doubled the magnitude of the input $u(t) = 2 \delta(t)$. By to the homogeneity property, the output would double, $2\tilde{x}(t)$.
- Delayed the input by $\tau$, $u(t) = \delta(t - \tau)$. By the time-invariance property, the output would be delayed by $\tau$, $\tilde{x}(t - \tau)$.
- Two taps at $t=0$ and $t=\tau$ were provided, $u(t) = \delta(t) + \delta(t - \tau)$. By the superposition property, the output would be $\tilde{x}(t) + \tilde{x}(t - \tau)$.

Given this, now consider what would happen if we the input was $N$ number of impulses with various magnitudes $u_k$ and at different times $\tau_k$.

$$ u(t) = \sum_{k=0}^N u_k \delta(t - \tau_k) $$

Given the LTI properties, the output would be,

$$ x(t) = \sum_{k=0}^N u_k \tilde{x}(t - \tau_k) $$

Now, if instead of summation, let us consider an integral. Intuitively, we can represent a smooth input signal $u(t)$ as the sum of many many many impulses of varying magnitudes all stacked up together.

$$u(t) = \int_0^t u(\tau) \delta(t-\tau) d\tau$$

Then passing this input signal into our LTI system,

$$
\begin{eqnarray}
x(t) &=& T\{ u(t) \} \\
    &=& T \left\{ \int_0^t u(\tau) \delta(t-\tau) d\tau \right\} \\
    &=& \int_0^t u(\tau) T\{ \delta(t-\tau)\} d\tau\\
    &=& \int_0^t u(\tau) \tilde{x}(t - \tau) d\tau\\
    &=& u(t) * \tilde{x}(t)
\end{eqnarray}
$$

The operator $*$ is known as the **convolution** operator. This derivation shows that the *impulse response* $\tilde{x}(t)$ of the LTI system completely characterizes its output behavior given any arbitrary input signal $u(t)$.

:::{admonition} Definition (Convolution)
:class: definition
Given two functions $g(t)$ and $h(t)$, the **convolution** is,

$$ (g * h)(t) = g(t) * h(t) = \int_0^t g(t) h(t - \tau) d\tau$$
:::


```{exercise}
:label: ch2-convolution-commutative
Show that the convolution operator is commutative, i.e., show that $(g * h)(t) = (h * g)(t)$.
```

**Remark.** You may have come across the convolution operator in the context of machine learning or computer vision (i.e. *convolutional* neural networks (CNNs)"). Indeed, the principles behind CNNs come from LTI theory: they encode the fact that the neural network's outputs to image inputs should be translation-invariant, i.e. a cat that has been moved 5 pixels to the right is still a cat.


## So I just need to compute the convolution to solve LTI systems?!

Suppose that we have an LTI system, and you knew what its impulse response was. Then to model how the system would respond to any input signal $u(t)$, all we need to to do is to compute the convolution integral, and then we are done.
Well, computing the convolution can be hard. Even if we could do solve it (e.g., numerically), we would have to perform the computation every time for each different input $u(t)$, and additionally, we do not have much insight into what the response may look like before we start solving it.

Ideally, we can avoid solving this somewhat nasty convolution integral, and be able to analyze and extract intrinsic properties of the system independent of the kinds of input we feed in.

Thinking back to your calculus class, what did you do when you encountered a nasty integral? You tried to see if you could perform a *change-of-variables*! We will do this, but with a very special choice for a change of variable, and this is known as the Laplace Transform...to the next chapter!