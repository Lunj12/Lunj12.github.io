--
layout: post
mathjax: true
comments: true
title:  "SVM Solution and Quadratic Programming"
date:   2019-08-04 21:29:52 -0700
categories: blog posts
---

The classical **Supporting Vector Machine (SVM)** is a convex optimization problem able to be solved by quadratic programming. The solution is unique and physically meaningful. Here is this post, a method of primal and dual optimazation is reviewed to solved the SVM problem.

### Category
```
* Classical SVM Problems
* Lagrange Multiplier
* KKT Conditions
* Dual Problem
* Hinge Loss
```

## Classical SVM Problems

### Separable Case ###

In separable case, the hard margin SVM problem is a convex minimization problem as:

$$ f(\mathbf{w}) = \min_{\mathbf{w}, b} \frac{1}{2} \lVert \mathbf{w} \rVert^2 $$

subject to constraints:

$$ y_i(\mathbf{w} \cdot \mathbf{x_i} + b) \geq 1, \quad \forall i \in [N] $$

Where $\mathbf{w}$ and $b$ are the weights and intercept of the separating hyperplane, $x_i$ and $y_i$ is the features and label of the data points, $N$ is total number of points.

### Non-Separable Case ###

In non-separable case, the soft margin SVM problem is a convex minimization problem as:

$$ f(\mathbf{w}, \mathbf{\xi}) = \min_{\mathbf{w}, b} \frac{1}{2} \lVert \mathbf{w} \rVert^2 + C \sum_{i=1}^{N} \xi_i^p$$

subject to constraints:

$$ y_i(\mathbf{w} \cdot \mathbf{x_i} + b) \geq 1 - \xi_i \quad \land \quad \xi_i \geq 0, \quad \forall i \in [N] $$

The meanings of variables follow the separable case, with $\xi$ as slack variables describing the violations of margin. And $p$ is the order of slack penalty. If $p = 1$, the loss function of the soft margin SVM is hinge loss; If $p = 2$ the loss is quadratic hinge loss.

## Lagrange Multiplier ##

For a convex optimization problem with affine constrains, Lagrange Multiplier can be used to find local optima. Take Separable Case as example, the Lagrangian function:

$$ L(\mathbf{w}, b, \alpha) = \frac{1}{2} \lVert \mathbf{w}\rVert^2 - \sum_{i=1}^{m} \alpha_i[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1] $$

In which each $\alpha_i \geq 0$ is a Lagrange variable. The mathematical meaning of lagrange multiplier follows the intuition that: the local optima of the original object function may locate inside or outside the constrained region, such that

1. if an unconstrained local optimum is inside the feasible region of $i-$th constraint, then this constraint is inactivate. $\alpha_i$ can be set to $0$ in the above Lagrangian function.

2. if an unconstrained local optimum is outside the feasible region of $i-$th constraint, then this constraint is activate, and the constrained optimum locates on the boundary of the feasible region where the contour of $f(\mathbf{w})$ intersects the boundary line. In this case, $\alpha_i > 0$ but $[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1] = 0$. 

In both cases, each term $\alpha_i[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1]$ vanishes in $L(\mathbf{w}, b, \alpha)$ and the optima of $L(\mathbf{w}, b, \alpha)$ is also the optima of $f(\mathbf{w})$.


## KKT Conditions ##

The obtain the optima of the Lagragian function $ L(\mathbf{w}, b, \alpha)$, we take derivatives of $L$ *w.r.t* $\mathbf{w}$ and $b$, and add the condition for $\alpha_i[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1]$ discussed above, then we get the Karush–Kuhn–Tucker (KKT) conditions:

$$ \nabla_\mathbf{w} L = w - \sum_{i=1}^{m} \alpha_i y_i \mathbf{x}_i = 0 \quad \Rightarrow \quad \mathbf{w} = \sum_{i=1}^{m} \alpha_i y_i \mathbf{x}_i = 0$$

$$ \nabla_{b} L = - \sum_{i=1}^{m} \alpha_i y_i = 0 \quad \Rightarrow \quad \sum_{i=1}^{m} \alpha_i y_i = 0 $$

$$ \forall i, \alpha_i[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1] = 0  \quad \Rightarrow \quad \alpha_i = 0 \lor y_i(\mathbf{w} \cdot \mathbf{x_i} + b) = 1$$


## Dual Problem ##
Why do the substitution in (5.12), given that the second part is $0$.


