# The Laplace Transform

Although the convolution operator allows us to evaluate the response of an LTI system to any input, it requires computing an integral, which can be difficult for complicated inputs and impulse responses. Is there an alternative way to evaluate the response to an LTI system?

This section introduces the **Laplace transform**, which applies a change of variables to the original system that reduces the complexity of evaluating the convolution of two functions. By taking the Laplace transform, we will go from the time domain to the $s$-domain. Later, we will see that it also serves as a useful tool for solving linear ordinary differential equations. We begin by introducing its formal definition.



:::{admonition} Definition (Laplace Transform)
:class: definition

The *one-sided* **Laplace transform** $F(s)$ of a function $f(t)$ is given by

```{math}
:label: eq-ch3-laplace
F(s) = \mathcal{L}\{ f(t)\} := \int_0^{\infty} f(t)e^{-st}dt
```

The **inverse Laplace transform** $f(t)$ of a function $F(s)$ is given by

```{math}
:label: eq-ch3-inverse-laplace
f(t) = \mathcal{L}^{-1} \{F(s)\} = \dfrac{1}{2\pi i} \lim_{T \rightarrow \infty} \int_{\gamma - iT}^{\gamma + iT} e^{st} F(s)ds
```
:::

Practically, you will not need to explicitly compute the integral in {eq}`eq-ch3-laplace` or {eq}`eq-ch3-inverse-laplace`. Diving into the details of the inverse Laplace transform is beyond the scope of this course as it requires a more in-depth understanding in complex analysis. Instead, we have tables describing the Laplace transform of commonly used functions, and we can use those tables freely without the need to redrive the expressions each time.

While the Laplace transform may seem even more complicated, it actually will simplify a lot of the algebra and analysis. In fact, rather than working with convolution integrals, applying the Laplace transform results in an *algerbra* problem, things like solving quadratic equations, grouping terms, factorizing, completing the square, etc. 
This is becuase the **the Laplace transform of the convolution of two functions in time is the product of the respective Laplace transform**.


:::{admonition} Theorem (Laplace transform of convolution)
:class: definition
The Laplace Transform of the convolution of two functions is given by

$$ \mathcal{L} \{ (f \ast g ) (t) \}(s) = F(s) G(s) $$

where $F(s) = \mathcal{L}\{f(t)\}$, $G(s) = \mathcal{L}\{g(t)\}$.
:::

This means that taking the Laplace transform of a convolution in time, is just equivalent to *multiplying* their Laplace transform! We go from solving nasty integrals to just plain old simple algebraic manipulation!
Ok, but after taking the Laplace transform and performing some algebraic manipulation, we need to perform the inverse Laplace transform to return back to the time domain. We can again use the Laplace tables to complete this step.

Conceptually, we take a ODE problem defined in the time domain. Computing the output response $x(t)$ given an input $u(t)$ is tricky because of the convolution integral. Instead we take the Laplace transform and jump to the $s$-domain and work with quantities $X(s)$ and $U(s)$. The convolution becomes a multiplication and we perform some algebraic manipulation to "massage the problem" until we are ready to return back to the time domain by taking the inverse Laplace transform of $X(s)$ to end up with $x(t)$, the quantity we want in the first place. A diagram illustrating these ideas is shown below.

(TODO: add flow chart.)



:::{exercise} Example
:class: example
:label: ch3-laplace-exp
Evaluate the Laplace Transform of $f(t) = e^{-at}$

$$
\begin{eqnarray}
 \mathcal{L} \{ 1 \}(s) &=& \dfrac{1}{s}\\
 \Rightarrow \mathcal{L} \{e^{-at}1\}(s) &=& \mathcal{L}\{ 1\}|_{(s-(-a))}= \dfrac{1}{s+a}
 \end{eqnarray}
 $$

:::


:::{exercise} Example
:class: example
:label: ch3-laplace-ramp

 Evaluate the Laplace Transform of the ramp function $f(t) = t$

We show two ways of doing this. First, we note the integer transform pair:

$$ \mathcal{L}\{t^{n}\} = \dfrac{n!}{s^{n+1}} $$

Substituting for $n=1$, $ \mathcal{L}\{t\}(s) = \dfrac{1}{s^2}$

Alternatively, we can transform the original function and use the table rules for the time integral.

$ \mathcal{L}\{t\} = \mathcal{L}\left\{\int_{0}^{t} 1 dt\right\} = \dfrac{1}{s} \mathcal{L}\{1\} = \left(\dfrac{1}{s}\right) \left(\dfrac{1}{s}\right) = \dfrac{1}{s^2} $
:::

:::{exercise} Example
:class: example
:label: ch3-laplace-2nd-ramp-exp


Evaluate $ \mathcal{L}\{ te^{at} \}(s) $

$$
\begin{eqnarray}
 \mathcal{L}\{ e^{at}f(t) \} &=& F(s-a) \\
 \mathcal{L}\{t\}  &=& \dfrac{1}{s^2} \\
\Rightarrow \mathcal{L}\{ e^{at} t \} &=& \dfrac{1}{(s-a)^2} \\
 \end{eqnarray}
 $$

:::

```{exercise} Second derivative
:label: ch3-laplace-2nd-derivative

Evaluate $ \mathcal{L} \left\{ \dfrac{d^{2}f}{dt^2}\right\}$
```


```{exercise} Sine and Cosine
:label: ch3-laplace-trig

Evaluate $ \mathcal{L} \{ f(t) \}$ for $f(t) = t \cos(\omega t)$ and $f(t) = t\sin(\omega t)$
```


:::{exercise} Partial fractions
:class: example
:label: ch3-laplace-partial-fractions


Remember that $x(t)=e^{-at}$ and its Laplace transform is $X(s)=\frac{1}{s+a}$.
Now, take the inverse Laplace transform of the following: $X(s) = \frac{5}{s+3} + \frac{2}{s^2+16}$

Rewrite to match the format that we see in Laplace tables:

$X(s) = 5\frac{1}{s+3} + \frac{1}{2}\frac{4}{s^2+16}$

$$
\begin{align*}
    X(s) &= 5\frac{1}{s+3} + \frac{1}{2}\frac{4}{s^2+16}\\
    \mathcal{L}^{-1}\{X(s)\} &=  \mathcal{L}^{-1}\left\{5\frac{1}{s+3} + \frac{1}{2}\frac{4}{s^2+16}\right\} \\
    &=  5e^{-3t} + \frac{1}{2} \sin{4t}
\end{align*}
$$

:::

:::{exercise} Partial fractions vs complete the square
:class: example
:label: ch3-laplace-partial-fractions-complete-square

Given $X(s)$ in the Laplace domain, we can try the following:

1. rewrite it into partial fractions then take the inverse Laplace transform
2. complete the square of the denominator and then take the inverse Laplace transform

You will see that both (1) and (2) give easy-to-use forms of the function for taking the inverse Laplace transform.

We have: $ X(s) = \frac{7}{(s+2)(s+1)}$

**(1) Partial fraction method**

$$
\begin{eqnarray}
 X(s) &=& \frac{7}{(s+2)(s+1)}\\
      &=& \frac{A}{s+2} + \frac{B}{s+1}\\
      &=&\frac{A(s+1)+B(s+2)}{(s+2)(s+1)}\\
\Rightarrow  7 &=& A(s+1)+B(s+2)\\
\Rightarrow && B=7, A = -7\\
\therefore X(s) &=& -\frac{7}{s+2} + \frac{7}{s+1}\\
\Rightarrow x(t) &=& -7e^{-2t} + 7e^{-t}
\end{eqnarray}
$$


**(2) Complete the square method**

$$
\begin{align*}
(s+2)(s+1) &= s^2 + 3s + 2 \\
           &= s^2 + 2 \cdot \frac{3}{2}s + 2 \\
           &= s^2 + 2 \cdot \frac{3}{2}s + \left( \frac{3}{2} \right)^2 - \left( \frac{3}{2} \right)^2 + 2 \\
           &= \left(s+\frac{3}{2}\right)^2 - \frac{1}{4}
\end{align*}
$$

then $ X(s) = \frac{7}{\left(s+\frac{3}{2}\right)^2 - \frac{1}{4}}$. The Laplace transform of $\sinh(at)$ is given by:

$$
\mathcal{L}(\sinh(at)) = \frac{a}{s^2 - a^2}, \quad \text{for } |s| > |a|.
$$

The shift property for \(e^{at}\) is given by:

$$
\mathcal{L}(e^{at}f(t)) = F(s - a),
$$

where $F(s)$ is the Laplace transform of $f(t)$.

Let's use these two properties to compute the Laplace transform:

$$
x(t) = \mathcal{L}^{-1} (X(s)) = \mathcal{L}^{-1} \left( \frac{7}{\left(s + \frac{3}{2}\right)^2 - \frac{1}{2}} \right) = \left( 14 \frac{\frac{1}{2}}{\left(s + \frac{3}{2}\right)^2 - \frac{1}{4}^2} \right) = 14 e^{-\frac{3}{2}t} \sinh\left(\frac{t}{2}\right)
$$

You can convince yourself with further trigonometric properties that:

$$14 e^{-\frac{3}{2}t} \sinh\left(\frac{t}{2}\right) = -7e^{-2t} + 7e^{-t}$$

So, as expected the partial fraction and complete the square methods give the same result.

:::

```{exercise} 2nd order ODE
:label: ch3-laplace-2nd-ODE

1. Solve initial value problem: $\ddot{x} + 5\dot{x} + 6x = 0$ where $x(0)=1, \dot{x}=1$.
2. Solve initial value problem: $\ddot{x} + 4\dot{x} + 5x = 0$, $x(0)=1$, $\dot{x}(0)=1$

```


