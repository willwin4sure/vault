> Lagrange multipliers and the envelope theorem.

You should remember how to solve single-variable constrained optimization problems using first and second order methods. We will handle some more advanced cases.

## Lagrange's Method

Suppose you have a constrained optimization problem $\max_{\mathbf{x}}f(\mathbf{x})$ under $g(\mathbf{x})=c$.

> [!theorem] (Lagrange's method)
> Suppose some $\mathbf{x}^{*}$ solves the optimization problem at an interior point and the constraint is binding. Then, there exists some unique number $\lambda^{*}$ such that $\nabla f(\mathbf{x}^{*})=\lambda ^{*}\nabla g(\mathbf{x}^{*})$. 

> [!idea] (Geometric intuition)
> If the gradients didn't point in the same direction, you can move in some direction perpendicular to the gradient of $g$ that increases $f$.

Another way to interpret this is to note that extrema of $f$ under the constraint must also be critical points of the ==Lagrangian function==
$$
\mathcal{L}(\mathbf{x},\lambda)=f(\mathbf{x})+\lambda(c-g(\mathbf{x})).
$$
Note that if $c\neq g(\mathbf{x})$, then you could set $\lambda\to \pm \infty$ to blow up $\mathcal{L}$. Taking the gradient with respect to $\lambda$ gives $\nabla f(\mathbf{x}^{*})=\lambda^{*}\nabla g(\mathbf{x}^{*})$.

> [!idea] (Economic intuition)
> We are equating the ratio of the marginal benefit and marginal cost
> $$
> \frac{ \partial f(\mathbf{x}^{*}) }{ \partial x_{i} } \bigg/ \frac{ \partial g(\mathbf{x}^{*}) }{ \partial x_{i} }
> $$
> in all variables. If this wasn't true, we could tune one down and another up to increase the benefit without changing the cost.

This ratio of marginal benefit per marginal cost is precisely $\lambda ^{*}$, which we call the ==shadow value==: it's how much extra benefit $f$ you get per incurred cost $g$ (imagine $f$ was utility and $g$ budget). Again, all variables must have the same shadow value.

In other words: suppose you solved the optimization problem for various values of $c$ and got optimal values
$$
V(c)=f(\mathbf{x}^{*}(c)),
$$
which we call the ==value function==. Then, 
$$
\lambda ^{*}(c)=\frac{dV(c)}{dc}.
$$
> [!question]
> Prove the above identity.

## Envelope Theorem

Suppose you now have a parameterized optimization problem $\max_{x}f(x;a)$ for a given $a \in A$ and call the solution $x^{*}(a)$. We can denote by
$$
V(a)=f(x^{*}(a);a)
$$
the maximized value of the objective function.

> [!theorem] (Envelope theorem, unconstrained case)
> The *total derivative* with respect to $a$ is equal to the *partial derivative* with respect to $a$, i.e.
> $$
> \frac{dV(a)}{da}=\frac{df(x^{*}(a);a)}{da}=\frac{ \partial f(x^{*}(a);a) }{ \partial a }.
> $$

This is because $a$ appears in the total derivative additionally through
$$
\frac{ \partial f(x^{*};a) }{ \partial x } \frac{ \partial x^{*} }{ \partial a } ,
$$
but the first term is zero at the optimum $x^{*}$.

> [!idea] (Economic intuition)
> When looking at the marginal effect of changing an "exogenous" variable on a function, you can *ignore* the indirect effect via an "endogenous" variable if that variable is chosen to maximize the function either way.

> [!idea] (Graphical intuition, and why it's called the "envelope" theorem)
> Consider fixing some $x$ and varying $a$; you could imagine we get some downwards sloping curves $f(x;a)$.
> 
> ![[envelope.jpg|center|512]]
> 
> Now, the upper envelope of these curves is the value function $V(a)$. At any $a$, the derivative of the blue curve matches the derivative of the highest corresponding green curve!

> [!example]
> Suppose $f(x;a)=-x^{2}+ax$ and $x^{*}(a)$ is the $x$ that maximizes $f(x;a)$ given an $a$. Then, write $V(a)=f(x^{*}(a);a)$. Compute $\frac{dV(a)}{da}$.

> [!proof]- Solution
> The envelope theorem tells us we can just consider
> $$
> \frac{dV(a)}{da}=\frac{ \partial f(x^{*}(a);a) }{ \partial a }=x^{*}(a). 
> $$
> Now simply note that
> $$
> \frac{ \partial f(x;a) }{ \partial x } = -2x+a \implies x^{*}(a)=\frac{a}{2},
> $$
> which is indeed a maximum since the second derivative is negative. So the answer is just $\frac{a}{2}$.

Now, consider the more complicated parameterized constrained optimization problem
$$
\max_{x,y}f(x,y;a) \quad \text{s.t.} \quad g(x,y;a)=c.
$$

> [!theorem] (Envelope theorem, constrained case)
> The *total derivative* with respect to $a$ is equal to the *partial derivative* with respect to $a$ of the Lagrangian, i.e.
> $$
> \frac{df(x^{*},y^{*};a)}{da}=\frac{ \partial \mathcal{L}(x^{*},y^{*},\lambda ^{*};a) }{ \partial a }.
> $$

> [!proof]-
> The total derivative on the left hand side can be expanded as
> $$
> \frac{df(x^{*},y^{*};a)}{da}=\frac{ \partial f(x^{*},y^{*};a) }{ \partial x } \frac{ \partial x^{*}(a) }{ \partial a } +\frac{ \partial f(x^{*},y^{*};a) }{ \partial y } \frac{ \partial y^{*}(a) }{ \partial a } + \frac{ \partial f(x^{*},y^{*};a) }{ \partial a }.
> $$
> Now, we no longer have that the partials with respect to $x$ and $y$ are zero, since there is a constraint. Instead, Lagrange's method tells us that they are a multiple of the partial derivatives of $g$, i.e.
> $$
> \frac{df(x^{*},y^{*};a)}{da}=\lambda ^{*}\frac{ \partial g(x^{*},y^{*};a) }{ \partial x } \frac{ \partial x^{*}(a) }{ \partial a } +\lambda ^{*}\frac{ \partial g(x^{*},y^{*};a) }{ \partial y } \frac{ \partial y^{*}(a) }{ \partial a } +\frac{ \partial f(x^{*},y^{*};a) }{ \partial a }.
> $$
> However, the total derivative of $g$ with respect to $a$ is zero, since it should always evaluate to $c$ no matter what. Therefore,
> $$
> 0=\frac{dg(x^{*},y^{*};a)}{da}=\frac{ \partial g(x^{*},y^{*};a) }{ \partial x } \frac{ \partial x^{*}(a) }{ \partial a } + \frac{ \partial g(x^{*},y^{*};a) }{ \partial y } \frac{ \partial y^{*}(a) }{ \partial a } +\frac{ \partial g(x^{*},y^{*};a) }{ \partial a },
> $$
> which means that we can simplify the previous expression into
> $$
> \frac{df(x^{*},y^{*};a)}{da}=\frac{ \partial f(x^{*},y^{*};a) }{ \partial a } -\lambda ^{*}\frac{ \partial g(x^{*},y^{*};a) }{ \partial a }=\frac{ \partial \mathcal{L}(x^{*},y^{*},\lambda ^{*};a) }{ \partial a }, 
> $$
> as desired.

The replacement of $f$ with the Lagrangian $\mathcal{L}$ only does something if the constraint $g$ varies with $a$. This is what accounts for the difference, and we need to multiply by the shadow value $\lambda ^{*}$ to transition from units of constraint to units of value.

---

**Next:** [[⛺Consumer Theory Homepage]]