---
layout: default
title: Implicit Method
parent: PDEs
grand_parent: Numerical Methods
has_children: false
permalink: /docs/numerical-methods/pde/implicit-method
---

# Implicit Method

Starting from the _implicit scheme_

$$
(1 - dt \nabla^{2})u^{(t+dt)} = u^{(t)} 
$$

and then expanding the left-hand side

$$
u^{(t+dt)} - dt \nabla^{2} u^{(t+dt)} = u^{(t)}
$$

We discretise the function $$u(x,y)$$ over the spatial dimensions

$$
u_{i,j}^{(t+dt)} - dt \nabla^{2} u_{i,j}^{(t+dt)} = u_{i,j}^{(t)}
$$

Then use the second-order approximation of the $$\nabla^{2}$$ operator (note that we assumed $$D = a^{2}$$)

$$
u_{i,j}^{(t+dt)} - dt(u_{i+1,j}^{(t+dt)} + u_{i-1,j}^{(t+dt)} + u_{i,j+1}^{(t+dt)} + u_{i,j-1}^{(t+dt)} - 4u_{i,j}^{(t+dt)}) = u_{i,j}^{(t)}
$$

Then isolate $$u_{i,j}^{(t+dt)}$$

$$
u_{i,j}^{(t+dt)} = \frac{1}{1 + 4 dt}( u_{i,j}^{(t)} + dt( u_{i+1,j}^{(t+dt)} + u_{i-1,j}^{(t+dt)} + u_{i,j+1}^{(t+dt)} + u_{i,j-1}^{(t+dt)} ) )
$$

This gives us a set of simultaneous equations that we can solve using the over-relaxed Gauss-Seidel method. The update step in pseudocode is given by

```c
k = 1.0 / (1.0 + 4.0 * dt)
unew[i][j] = k * (u[i][j] + dt * (unew[i+1][j] + unew[i-1][j] + unew[i][j+1] + unew[i][j-1]))
```

and the `bnorm` and `rnorm` are given by

$$
\begin{split}
\text{bnorm} &= \sqrt{\sum_{(i,j) \in B} (\text{u}[i][j])^{2}} \\
\text{rnorm} &= \sqrt{\sum_{i,j} (\text{LHS}(i,j) - \text{RHS}(i,j) )^{2}}
\end{split}
$$

where `B` is the set of indices of the boundary cells and

```c
LHS(i,j) = unew[i][j] - dt * (unew[i+1][j] + unew[i-1][j] + unew[i][j+1] + unew[i][j-1])
RHS(i,j) = u[i][j]
```
