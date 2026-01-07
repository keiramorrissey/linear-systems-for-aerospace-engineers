# Modal decomposition

In the previous chapter, the concept of state space representation and dynamics were introduced. As reminder, we are represent all relevant and necessary information about the system in the form of a **state vector** $\mathbf{x}(t)$ and describe how the the values of the state vector changes as a function of time and input $\mathbf{u}(t)$ using a **first-order ordinary differential equation** $\dot{\mathbf{x}} = f(\mathbf{x}, \mathbf{u})$. Yet, computing the solution $\mathbf{t}$ given the dynamics and control signal (see {eq}`eq-ch7-state-space-solution`) in closed form is not straightforward given the integral and matrix exponential.

To simplify the problem a little further, with the hopes of obtaining a closed-formed solution for describing the motion of the system, consider the case where there is no input $\mathbf{u}(t)=0$ for all $t$. But instead, the system will move due to non-zero initial conditions. For instance, consider a beam, we are interested in how it may behave given some initial displacement.
What we will discover is that the motion, while it may look complex at a first glance, is actually a result of a combination of simpler motions, each with a certain direction and frequency. In fact, given that we are concerned with *linear* time-invariant systems of multiple dimensions, these directions and frequences actually correspond to the eigenvectors and eigenvalues of the system respectively. 


## Free vibrations (Undamped): State-space approach

As mentioned above, we consider the case with zero input, and we are interested in how the system responds to non-zero initial conditions. The term *free vibrations* refer to this setting---the system is allowed to freely move (we will later see that the motion can be viewed as vibrations). Additionally, we will consider the undamped case, i.e., the damping term is zero.

Recall from {eq}`eq-ch7-general-2nd-order-dynamics`, a general second-order $n$-dimensional system can be written as,

$$ M\ddot{\mathbf{x}} + C\dot{\mathbf{x}} + K{\mathbf{x}} = B\mathbf{u} , \quad \mathbf{x}\in\mathbb{R}^n, \: \mathbf{u} \in \mathbb{R}^m,$$

With no damping and no control inputs, we end up with 

$$ M\ddot{\mathbf{x}}  + K{\mathbf{x}} = 0 $$

Now, the goal is to determine an expression for $\mathbf{x}(t)$ which describes how each of the variables (e.g., positions of each mass) behaves over time.
To do this, we leverage the closed-form expression for state space dynamics {eq}`eq-ch7-state-space-solution`. 
Hence first, we need to convert our second order system above into the standard state space dynamics form (first order equation).

Using state augmentation, we have


$$
\mathbf{q} = \begin{bmatrix} x \\ \dot{x} \end{bmatrix}, \qquad \dot{\mathbf{q}} = \begin{bmatrix} \dot{x} \\ \ddot{x} \end{bmatrix},  \quad \mathbf{q}\in\mathbb{R}^{2n}
$$

The equation of motion $ M \ddot{x} + K x = 0 $ can be rearranged to $\ddot{x} = -M^{-1}Kx$.
Thus the first order state space dynamics become

```{math}
    :label: eq-ch8-undamped-state-space-dynamics

\dot{\mathbf{q}} = \begin{bmatrix} \dot{x} \\ \ddot{x} \end{bmatrix} = \underbrace{\begin{bmatrix} 0 & I \\ -M^{-1}K & 0 \end{bmatrix}}_{A \text{ matrix}}\begin{bmatrix} x \\ \dot{x} \end{bmatrix}
```

Using {eq}`eq-ch7-state-space-solution`, we see that the solution is,

$$ \mathbf{q}(t) = e^{At}\mathbf{q}(0)$$

But notice that the matrix exponential term is non-trivial. Sure, we can call a function and it will spit out a number, but it doesn't quite provide any insight how $\mathbf{q}(t)$ changes over time. For instance, is there any structure or predictable patterns? Perhaps if we analyzed $e^{At}$ from a different perspective, or a different reference frame, the structure or pattern could become more obvious...

### Try a clever change of variables: Eigen decomposition!
Assume that the matrix $A$ is eigen decomposable, with eigenvectors and eigenvalues $(\mathbf{v}_1, \lambda_1), (\mathbf{v}_2, \lambda_2), \ldots, (\mathbf{v}_{2n}, \lambda_{2n})$. 
Note that there are $2n$ eigenvectors/values because since $\mathbf{x} \in \mathbb{R}^n$,  $A \in \mathbb{R}^{2n \times 2n}$.
Then we have $A = VDV^{-1}$ where $V = [\mathbf{v}_1,\ldots, \mathbf{v}_{2n}]$ and eigenvalues $D = \text{diag}([\lambda_1, \ldots, \lambda_{2n}])$. 

Thus using this represention in our state space dynamics, we get,

$$
\dot{\mathbf{q}} = \underbrace{A}_{V D V^{-1}} \mathbf{q} = 
\underbrace{V}_{\text{transform back} \quad} 
\underbrace{D}_{\text{scale in eigenbasis} \quad} 
\underbrace{V^{-1} \mathbf{q}}_{\text{transform to eigenbasis}}
$$

The interpretation, reading from right to left, is that $V^{-1} \mathbf{q}$ performs a change of basis on the state, from whatever basis $\mathbf{q}$ is defined in, to the eigenbasis. Let $\mathbf{z} = V^{-1} \mathbf{q}$. 
Now that we have $\mathbf{z}$, which is $\mathbf{q}$ expressed in the eigenbasis, we need to "apply the dynamics". But in the eigenbasis, this is equivalent multiplying $\mathbf{z}$ by $D$, i.e., scaling along each (eigen) basis vector by each diagonal entry (the eigenvalues). Essentially, we have $\dot{\mathbf{z}} = D\mathbf{z}$. 
Then finally, we want to project back into the original basis, which presumably is in a frame that is more natural to interpret (but many not be more mathematically convinient to work in).
Thus the last step is to project $\dot{\mathbf{z}}$ back to the original basis, $\dot{\mathbf{q}} = V\dot{\mathbf{z}}$.

So hopefully by now, you are convinced that this eigen decomposition can be viewed as a *change-of-variable* and does not actually change the value of what $A$ is.

The eigen decomposition is also useful in computing matrix exponentials. Recall that 

$$ e^{At} = I + At + \frac{1}{2!}A^2t^2 + \frac{1}{3!}A^3t^3 + ... $$

Using $A = VDV^{-1}$, we have

$$ 
e^{VDV^{-1}t} &= I + VDV^{-1}t + \frac{1}{2!}(VDV^{-1})^2t^2 + \frac{1}{3!}(VDV^{-1})^3t^3 + ...\\ 
&= VV^{-1} + VDV^{-1}t + \frac{1}{2!}(VDV^{-1})(VDV^{-1})t^2 + \frac{1}{3!}(VDV^{-1})(VDV^{-1})(VDV^{-1})t^3 + ...\\ 
&= VV^{-1} + VDV^{-1}t + \frac{1}{2!}(VD^2V^{-1})t^2 \frac{1}{3!}(VD^3V^{-1})t^3 + ...\\ 
&= V \left(I + Dt + \frac{1}{2!}D^2t^2 + \frac{1}{3!}D^3t^3 + ... \right) V^{-1}\\
&= Ve^{Dt}V^{-1}
$$

Since $D$ is a diagonal matrix, we have

$$ e^{Dt} = \text{diag} ([e^{\lambda_1 t}, e^{\lambda_2 t}, \ldots, e^{\lambda_{2n} t}]) $$

So the solution can be written as

$$ \mathbf{q}(t) = Ve^{Dt}V^{-1}\mathbf{q}(0)$$

We will now interpret each of these operations (like above), from right to left.

The first operation $V^{-1}\mathbf{q}(0)$ projects the initial state to the eigenbasis. Mathematically, we have

$$\mathbf{z}(0) = V^{-1}\mathbf{q}(0) = [\alpha_1, \ldots, \alpha_{2n}]^T$$ 

In the next operation, $e^{Dt}$ is applied, which is interpreted as computing the solution at time $t$ but in the eigenbasis. Thus we have

$$ 
e^{Dt}V^{-1}\mathbf{q}(0) &= e^{Dt}\mathbf{z}(0)\\ 
&= e^{Dt}[\alpha_1, \ldots, \alpha_{2n}]^T \\ 
& = [\alpha_1e^{\lambda_1 t}, \ldots, \alpha_{2n}e^{\lambda_{2n} t}]\\ 
&= \mathbf{z}(t)
$$ 


Finally, we project $\mathbf{z}(t)$ back to the original basis, $\mathbf{q}(t) = V\mathbf{z}(t)$.
Using the expression we have for $\mathbf{z}(t)$, we have

$$
\mathbf{q}(t) &= V\mathbf{z}(t)\\ 
&= V[\alpha_1e^{\lambda_1 t}, \ldots, \alpha_{2n}e^{\lambda_{2n} t}]\\ 
&= \alpha_1e^{\lambda_1 t}\mathbf{v}_1 + \ldots + \alpha_{2n}e^{\lambda_{2n} t}\mathbf{v}_{2n}\\ 
&= \sum_{i=1}^{2n} \alpha_ie^{\lambda_i t}\mathbf{v}_i
$$

So what can we conclude after all this? Well, the solution $\mathbf{q}(t)$ is simply the linear combination of the eigenvalues where the coefficients $\alpha_ie^{\lambda_i t}$ depend on the initial conditions projected into the eigenbasis and eigenvalue. So finding the solution to $ M\ddot{\mathbf{x}}  + K{\mathbf{x}} = 0 $ boils down to finding the eigenvalues and eigenvectors of the $A$ matrix with size $2n \times 2n$ from {eq}`eq-ch8-undamped-state-space-dynamics`.

So are we done? Is that as much structure we can possibly extract from the problem? Not quite. There are a few things to note.
First, the original problem was to find $\mathbf{x}(t)$, but now, with the state space augmentation, we are now solving for $\mathbf{q}(t)$ which is twice is big as $\mathbf{x}(t)$. Furthermore, $\mathbf{q}(t)$ consists of $\dot{\mathbf{x}}(t)$ which can alternatively be obtained by differentiating $\mathbf{x}(t)$. So essentially, by augmenting the state to conform to the standard state space dynamics formulation, we have made the problem slightly redundant.

Let's take a closer look at $A$ and see if there anything we can do to hopefully simplfy the problem further.

### Long derivation

We have 
$ A = \begin{bmatrix} 0 & I \\ -M^{-1}K & 0 \end{bmatrix}$ and we wish to find the eigenvalue and vectors.
Let $(\lambda, \mathbf{v})$ be an eigenvalue-vector pair. In particular, let's divide $\mathbf{v}$ into two equal components, $\mathbf{v} = \begin{bmatrix} \mathbf{v}^{x} \\ \mathbf{v}^{\dot{x}} \end{bmatrix}$. Then the eigenpairs should satisfy

$$
\begin{bmatrix} 0 & I \\ -M^{-1}K & 0 \end{bmatrix}\begin{bmatrix} \mathbf{v}^{x} \\ \mathbf{v}^{\dot{x}} \end{bmatrix} = \lambda \begin{bmatrix} \mathbf{v}^{x} \\ \mathbf{v}^{\dot{x}} \end{bmatrix} \\ \\
\implies \begin{matrix} \mathbf{v}^{\dot{x}} = \lambda \mathbf{v}^{x} \\ -M^{-1}K \mathbf{v}^{x} = \lambda \mathbf{v}^{\dot{x}} = \lambda^2 \mathbf{v}^{x} \end{matrix}
$$

If we let $\mu = -\lambda^2$, then we have a familiar looking eigenvalue problem at hand, $M^{-1}K \mathbf{v}^{x} = \mu \mathbf{v}^{x}$.
So rather than solving the eigenvalue problem on $A$ directly (a $2n \times 2n$ matrix), we can solve the eigenvalue problem on $M^{-1}K$ instead which is only a $n \times n$ matrix. 

It turns out that for most physically meaningful cases, $M$ and $K$ are positive definite, and this implies that $\mu >0 $.
This means that

$$ \mu >0 \implies \lambda^2 <0 \implies \pm \lambda = \pm i\sqrt{\mu}$$

So the eigenvalues $\lambda_i$ are *purely imaginary*. We could then go ahead and compute the corresponding eigenvectors which again, would consist of imaginary components. Then we would plug these values into $\sum_{i=1}^{2n} \alpha_ie^{\lambda_i t}\mathbf{v}_i$ and obtain our solution. But does this physically make sense? How can the motion of a real physical system contain imaginary numbers?! Well, let think a bit more about this result.

- We have just identified that the eigenvalues of $A$ are *purely imaginary*. Recall that the eigenvalues a state space dynamics correspond to the *poles* of the system. If the poles are purely imaginary, this corresponds to a system that is purely oscillatory, i.e., no damping. This is consistent with our problem where at the very beginning, we set damping to be zero. So in this regard, having purely imaginary eigenvalues make sense.
- The final solution for $\mathbf{q}(t)$ or $\mathbf{x}(t)$ should not contain any imaginary components because it does not make any physical sense. Imaginary numbers are used more for mathematical convenience, but they do not actually exist for a real physical system. Note that the solution is of the form $\sum_{i=1}^{2n} \alpha_ie^{\lambda_i t}\mathbf{v}_i$ and that anytime there is an imaginary number, either from the eigenvalue, vector, or coefficient, they come in conjugate pairs. It turns out that once you write out all the terms, you will find that all the imaginary parts will cancel out, leaving only real parts. If you do go through the details and find that the imaginary parts are not canceling out, it is likely that you have made an error along the way. 
- Building upon the two points above, we make use of Euler's formula $e^{ix} = \cos x + i\sin x$,  $e^{ix} + e^{-ix} = 2\cos x $, and $e^{ix} - e^{-ix}=2i\sin x$ when simplifying our the summation . Trusting that all the imaginary terms will cancel out, we will find that we end up with a linear combination of $\sin(\omega_i t)\mathbf{v}_i$ and $\cos(\omega_i t)\mathbf{v}_i$ terms where $\omega_i = \sqrt{\mu}$ which is interpreted as the *angular frequency* of the system. 

$$\mathbf{q}(t) = \sum_{i=1}^{n} (a_i\cos(\omega_i t) + b_i \sin(\omega_i t))\mathbf{v}_i \\ 
\mathbf{x}(t) = \sum_{i=1}^{n} (a_i\cos(\omega_i t) + b_i \sin(\omega_i t))\mathbf{v}^{x}_i 
$$

Note that the summation is now over $n$ and not $2n$. The $2n$ came from summing over the positive and negative eigenvalues, and after the canceling out of imaginary parts, it reduces to a summation over $n$ but each term in the summation has a $\cos$ and $\sin$ term. The coefficients $a_i$ and $b_i$ result in the algebra manipulation of terms. The values of them can be computed based on initial conditions. More on that later.


**Summarizing what we have just done so far**: 
We had a free vibration (undamped) problem and using state space representation, we attempted to solve the problem using the state space dynamics solution. 
In doing so, we found that we needed to solve an eigenvalue problem.
Looking a bit more closely at the structure of our $A$ matrix from our state space dynamics, the eigenvalue problem could be broken down into a smaller eigenvalue problem, and that the eigenvalues are purely imaginary. 
With this in mind, and using Euler's formula and trusting that the imaginary parts will cancel out, the final solution is of the form

$$\mathbf{x}(t) = \sum_{i=1}^{n} (a_i\cos(\omega_i t) + b_i \sin(\omega_i t))\mathbf{v}_i $$

where $\omega_i$ and $\mathbf{v}_i$ are the solutions to the following eigenvalue problem,

$$M^{-1}K \mathbf{v} = \omega^2 \mathbf{v} $$

The eigenvectors are referred to as **mode shapes** or the **modes** of the system, while the eigenvalues are referred to as the  **natural (angular) frequencies** of the system.



### Practical approach

So that derivation above was somewhat long, and a little redundant in that the we are mainly interested in finding $\mathbf{x}(t)$ and not $\mathbf{q}(t)$.
So now let's summarize the steps needed to compute the solution, taking into the account all the insights we gained from the above derivation.

We wish to solve the problem $ M\ddot{\mathbf{x}}  + K{\mathbf{x}} = 0 $, with initial conditions $\mathbf{x}(0) = \mathbf{x}_0$ and $\dot{\mathbf{x}}(0) = \dot{\mathbf{x}}_0$.

We "guess" that the solution is of the form $\mathbf{x}(t) = \alpha e^{-i\omega t}\mathbf{v}$. 
(Perhaps in your previous differential equations course, you were just told a similar thing, that you should just "guess" that the solution is of a particular form, and you just went along with it. Well, with the derivation above, you can see that the guess is not really a guess, but is in fact grounded in linear algebra and eigen decomposition!)

We plug out "guess" back into the ODE,

$$
 M\ddot{\mathbf{x}}  + K{\mathbf{x}} = 0\\ 
  M(-\omega^2  \alpha e^{-i\omega t}\mathbf{v})  + K\alpha e^{-i\omega t}\mathbf{v} = 0\\ 
  \alpha e^{-i\omega t}(-\omega^2 M + K)\mathbf{v} = 0
$$

In order for the above equation to solve, we could have
- $\alpha = 0$ or $\mathbf{v}=0$, but then our guess will be trivial and that does not get us anywhere.
- $e^{-i\omega t} = 0$, but that never equals to zero.
- $-\omega^2 M + K = 0$, but this is typically not possible since $M$ and $K$ are determined by the system properties.