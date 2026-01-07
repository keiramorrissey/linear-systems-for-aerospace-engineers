# State-space representation

So far, we have been concerned with SISO systems, systems with single input and single output. What if there were multiple inputs that could influence the system and multiple outputs we are interested in keeping track of? Well first, we need to have some way to represent all the inputs and outputs in a concise manner and also describe how these quantities interact with each other.


## Motivation

First, let us consider a more complicated mechanical system: two masses connected by parallel springs and dampers with an external forcing $u$.

![Two masses connected by parallel springs and dampers.](figs/two_mass_damp_spring.png)


We define the origin of $x_{1}$ and $x_{2}$ as the positions of the masses when the springs are in their unforced states. We can draw the FBDs for both masses assuming no friction with the surface (we can thus leave out vertical forces).

![FBD for two masses connected by parallel springs and dampers.](figs/two_mass_damp_spring_FBD.png)


The equation of motion of the first mass $m_{1}$:

$$
    m_{1} \ddot{x}_{1} &= -F_{k1} - F_{b1} + F_{k2} + F_{b2} \\
    & = -k_{1}x_{1} - b_{1}\dot{x_{1}} + k_{2}(x_{2}-x_{1}) + b_{2}(\dot{x}_{2} - \dot{x}_{1})\\
    \implies & m_{1} \ddot{x_{1}} + k_{1}x_{1} + b_{1}\dot{x_{1}} - k_{2}(x_{2}-x_{1}) - b_{2}(\dot{x}_{2} - \dot{x}_{1}) = 0
$$

The equation of motion of the second mass $m_{2}$:

$$
    m_{2} \ddot{x}_{2} &= - F_{k2} - F_{b2} + u \\
    &= -k_{2}(x_{2}-x_{1}) - b_{2}(\dot{x_{2}} - \dot{x_{1}}) + u\\
    \implies & m_{2} \ddot{x}_{2} + k_{2}(x_{2}-x_{1}) + b_{2}(\dot{x}{2} - \dot{x}_{1}) = u
$$


Applying the Laplace transform to the above equations of motion with zero initial conditions:

$$m_{1}s^{2}X_{1}(s) + k_{1}X_{1} + b_{1}sX_{1} - k_{2}(X_{2} - X_{1}) - b_{2}s(X_{2} - X_{1}) = 0 $$
$$\implies X_{1} (m_{1}s^{2} + (b_{1} + b_{2}) s + (k_{1} + k_{2})) = X_{2} (b_{1}s + k_{2}) $$

$$m_{2} s^{2} X_{2}(s) + k_{2} (X_{2} - X_{1}) + b_{2}s(X_{2} - X_{1}) = U $$

We can substitute one of the above equations into the other to solve for $\dfrac{X_{1}}{U}$ and $\dfrac{X_{2}}{U}$. 

How scalable is the above strategy for more complicated systems? For example, suppose we had 100 mass-spring dampers connected in parallel. Imagine going though the algebra like the one above 100 times! 
Is there another way to analyze linear systems with many components? 

Yes there is! We do this by essentially stacking all the variables into a vector $\mathbf{x} = [x_1, \ldots, x_n]^T$. We will refer to this vector as the **state vector**, which represents the minimal set of variables necessary to describe the system. Now that the variables are written as a vector, we can also similarly consider the first and second derivative of the state vector, $\dot{\mathbf{x}} = [\dot{x}_1, \ldots, \dot{x}_n]^T$ and $\ddot{\mathbf{x}} = [\ddot{x}_1, \ldots, \ddot{x}_n]^T$.
In doing so, we can now combine the two equations of motion into a single matrix-vector equation.


Let's start by rewriting the two equations in matrix form:


```{math}
:label: eq-ch7-two-msd-state-equation

\underbrace{
    \begin{bmatrix}
        m_{1} & 0 \\ 0 & m_{2}
    \end{bmatrix}}_{M = \text{Mass matrix}}
   \begin{bmatrix}
    \ddot{x_{1}} \\ \ddot{x_{2}}
   \end{bmatrix} +
   \underbrace{
   \begin{bmatrix}
    b_{1} + b_{2} & -b_{2} \\ -b_{2} & b_{2}
   \end{bmatrix}}_{C = \text{Damping matrix}}
   \begin{bmatrix}
    \dot{x_{1}} \\ \dot{x_{2}}
   \end{bmatrix} +
   \underbrace{
   \begin{bmatrix}
    k_{1} + k_{2} & -k_{2} \\ -k_{2} & k_{2}
   \end{bmatrix}}_{K = \text{Stiffness matrix}}
   \underbrace{
   \begin{bmatrix}
    x_{1} \\ x_{2}
   \end{bmatrix}}_{x = \text{State}} = 
   \underbrace{
   \begin{bmatrix}
       0 \\ 1
   \end{bmatrix}}_{B = \text{Control matrix}} u 
```



```{math}
:label: eq-ch7-general-2nd-order-dynamics

\implies M\ddot{\mathbf{x}} + C\dot{\mathbf{x}} + K{\mathbf{x}} = B\mathbf{u} 
```

Hmmmm, so it appears that the two equations of motion that were derived could actually be combined together to be written in a somewhat compact and simple form.

```{exercise}
:label: ch7-n-chain-msd
Try writing out the equation of motion for a chain of 3 and 4 MSD systems. Do you notice a patter forming? What would the equation for a 100-chain MSD system look like?
```


## State-space dynamics

Recall from Chapter 1 when we introduced the notion of *state* as a minimal list of quantities that fully describes a dynamical system. How the state changes over time is described by a **first-order ordinary differential equation** $\dot{\mathbf{x}} = f(\mathbf{x}, \mathbf{u})$ where $f$ is vector-valued function which takes in state and control as inputs, and the control $\mathbf{u}$ is an external input into the system whose value can be chosen by, for example, a human or algorithm.
While the dynamics function $f$ can be generally complex and nonlinear, in this course, we are restricting our analysis to LTI systems as this class of system exhibits nice properties that make analysis and control synthesis tractable. 
Thus we define LTI state space dynamics, which can be viewed as a generalized representation to many of the MSD systems we have seen so far.

:::{admonition} Definition (LTI state space dynamics)
:class: definition
The state-space representation for a linear time-invariant dynamical system is given by
```{math}
    :label: eq-ch7-state-space-dynamics

    \dot{\mathbf{x}} &= A\mathbf{x} + B\mathbf{u} \\
    \mathbf{y} &= C\mathbf{x} + D\mathbf{u} 
```
where $x \in \mathbb{R}^{n}$ is the **state** of the system, $u \in \mathbb{R}^{m}$ is the system **input**, and $y \in \mathbb{R}^{p}$ is the system **output**.
- $A \in \mathbb{R}^{n \times n}$ is known as the **system matrix** (which governs the system's passive dynamics)
- $B \in \mathbb{R}^{n \times m}$ is known as the **control matrix** (which governs the system dynamics with respect to the control input)
- $C \in \mathbb{R}^{p \times n}$ is known as the **output matrix** (which governs the relationship between the state and the measured output)
- $D \in \mathbb{R}^{p \times m}$ is known as the **feedthrough matrix** (which governs the relationship between the input and output; often this is 0)

:::



We will now show how to obtain the solution for the state $\mathbf{x}$ from a first-principles approach. Once you understand how this solution is obtained, you can use it moving forward.

Taking the Laplace transform of both sides, we obtain:

$$
s X(s) - x_0 &= A X(s) + B U(s)\\
Y(s) &= C X(s) + D U(s)
$$

Rearrange the first equation:

$$
(sI - A) X(s) = x_0 + B U(s)\\
\implies X(s) = (sI - A)^{-1} B U(s) + (sI - A)^{-1} x_0
$$

In the above, the first term is the response due to input and the second term is the response due to initial conditions.

Substituting $X(s)$ into the output equation:

$$
Y(s) = C X(s) + D U(s)\\
Y(s) = C \left[ (sI - A)^{-1} B U(s) + (sI - A)^{-1} x_0 \right] + D U(s)
$$

$$
\implies Y(s) = C (sI - A)^{-1} B U(s) + C (sI - A)^{-1} x_0 + D U(s)
$$


:::{admonition} Definition (Inverse Laplace of $(sI - A)^{-1}$)
:class: definition
```{math}
    (sI-A)^{-1} &= \frac{1}{s} \left(I - \frac{A}{s}\right)^{-1}\\ 
                &= \frac{I}{s} + \frac{A}{s^2} + \frac{A^2}{s^3} + \ldots\\
    \mathcal{L}^{-1}\lbrace (sI-A)^{-1} \rbrace &= I + tA + \frac{(tA)^2}{s!} + \ldots\\
    & e^{At} \quad \text{(Matrix exponential)} 
```

:::
As such, taking the inverse Laplace transform, we have,

```{math}
    :label: eq-ch7-state-space-solution

\mathbf{x}(t) = \int_0^t e^{A(t-\tau)} B \mathbf{u}(\tau) d \tau + e^{A t} \mathbf{x}(0)\\ 
\mathbf{y}(t)=\int_0^t Ce^{A(t-\tau)} B \mathbf{u}(\tau) d \tau + Ce^{A t} \mathbf{x}(0) + D\mathbf{u}(t)\\ 

```


What have we shown? For any state space dynamics, the state (and output) at any time can be determined by the above equations which require knowledge of the system matrices, initial state, and control signal.


## Converting n-order systems into a first-order system

Notice that the state space dynamics are given as a *first-order* ODE. However, when are are deriving the equation of motion, we result in a *second-order* ODE. So how do we convert our second-order ODEs into state space dynamics?
We do this via state augmentation. Note that for LTI systems, the ODE is of the form $a_nx^{(n)}(t) + a_{n-1}x^{(n-1)}(t) + \ldots a_1x^\prime(t) + a_0x(t) = 0$. This means that $x^{(n)}(t)$ term is a linear combination of all the lower order terms. As such, we redefine our state to also include all the derivative terms up until the $n-1$ derivative. In the case of the second order system we considered at the start of this chapter, we redefine the state,



$$ \mathbf{q} = \begin{bmatrix} x_{1} \\ x_{2} \\ \dot{x}_{1} \\ \dot{x}_{2} \end{bmatrix} , \quad \dot{\mathbf{q}} = \begin{bmatrix} \dot{x}_{1} \\ \dot{x}_{2} \\ \ddot{x}_{1} \\ \ddot{x}_{2} \end{bmatrix} $$

With this new state representation, considering the $C$, $K$, and $B$ matrices defined in {eq}`eq-ch7-two-msd-state-equation`, we can rewrite the dynamics as

$$
\dot{\mathbf{q}} = 
\underbrace{\begin{bmatrix}
0 & I \\ -M^{-1}C & -M^{-1}K
\end{bmatrix}}_{A \text{ matrix}} + \underbrace{\begin{bmatrix}
0 \\ -M^{-1}B 
\end{bmatrix}}_{B \text{ matrix}} \mathbf{u}
$$


