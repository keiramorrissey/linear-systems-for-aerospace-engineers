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

Practically, you will not need to explicitly compute the integral in {eq}`eq-ch3-laplace` or {eq}`eq-ch3-inverse-laplace`. The inverse Laplace transform is beyond the scope of this course, and requires a more in-depth understanding in complex analysis. Instead, we have tables describing the Laplace transform of commonly used functions, and we can use those tables freely without the need to redrive the expressions each time.

While the Laplace transform may seem even more complicated, it actually will simplify a lot of the algebra and analysis. Specifically, the **the Laplace transform of the convolution of two functions in time is the product of the respective Laplace transform**.


:::{admonition} Theorem (Laplace transform of convolution)
:class: definition
The Laplace Transform of the convolution of two functions is given by

$$ \mathcal{L} \{ (f \ast g ) (t) \}(s) = F(s) G(s) $$

where $F(s) = \mathcal{L}\{f(t)\}$, $G(s) = \mathcal{L}\{g(t)\}$.
:::

This means that taking a convolution in time, is just equivalent to *multiplying* their Laplace transform! We go from solving nasty integrals to just plain old simple algebraic manipulation!
Ok, but after taking the Laplace transform and performing some algebraic manipulation, we need to perform the inverse Laplace transform to return back to the time domain. But this last step, we can use the Laplace tables to help us with it.

Conceptually, we take a ODE problem defined in the time domain. Computing the output response $x(t)$ given an input $u(t)$ is tricky because of the convolution integral. Instead we take the Laplace transform and jump to the $s$-domain and work with quantities $X(s)$ and $U(s)$. The convolution becomes a multiplication and we perform some algebraic manipulation to "massage the problem" until we are ready to return back to the time domain by taking the inverse Laplace transform of $X(s)$ to end up with $x(t)$, the quantity we want in the first place. A diagram illustrating these ideas is shown below.

(TODO: add flow chart.)



<i class="fas fa-pencil-alt"></i> **Example:** Evaluate the Laplace Transform of $f(t) = e^{-at}$

$$
\begin{eqnarray}
 \mathcal{L} \{ 1 \}(s) &=& \dfrac{1}{s}\\
 \Rightarrow \mathcal{L} \{e^{-at}1\}(s) &=& \mathcal{L}\{ 1\}|_{(s-(-a))}= \dfrac{1}{s+a}
 \end{eqnarray}
 $$


:::{exercise}
:class: example

 Evaluate the Laplace Transform of the ramp function $f(t) = t$

We show two ways of doing this. First, we note the integer transform pair:

$$ \mathcal{L}\{t^{n}\} = \dfrac{n!}{s^{n+1}} $$

Substituting for $n=1$, $ \mathcal{L}\{t\}(s) = \dfrac{1}{s^2}$

Alternatively, we can transform the original function and use the table rules for the time integral.

$ \mathcal{L}\{t\} = \mathcal{L}\left\{\int_{0}^{t} 1 dt\right\} = \dfrac{1}{s} \mathcal{L}\{1\} = \left(\dfrac{1}{s}\right) \left(\dfrac{1}{s}\right) = \dfrac{1}{s^2} $
:::


