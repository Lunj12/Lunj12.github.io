---
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
* Solution of SVM
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

$$ L(\mathbf{w}, b, \mathbf{\alpha}) = \frac{1}{2} \lVert \mathbf{w}\rVert^2 - \sum_{i=1}^{m} \alpha_i[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1] $$

In which each $\alpha_i \geq 0$ is a Lagrange variable. The mathematical meaning of lagrange multiplier follows the intuition that: the local optima of the original object function may locate inside or outside the constrained region, such that

1. if an unconstrained local optimum is inside the feasible region of $i-$th constraint, then this constraint is inactivate. $\alpha_i$ can be set to $0$ in the above Lagrangian function.

2. if an unconstrained local optimum is outside the feasible region of $i-$th constraint, then this constraint is activate, and the constrained optimum locates on the boundary of the feasible region where the contour of $f(\mathbf{w})$ intersects the boundary line. In this case, $\alpha_i > 0$ but $[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1] = 0$. 

In both cases, each term $\alpha_i[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1]$ vanishes in $L(\mathbf{w}, b, \alpha)$ and the optima of $L(\mathbf{w}, b, \alpha)$ is also the optima of $f(\mathbf{w})$.


## KKT Conditions ##

In the SVM case, strong duality holds, which means the best lower bound of the Lagragian function

$$\max_{\alpha} \min_{\mathbf{w},b} L(\mathbf{w}, b, \alpha)$$

 is one of minima of the original problem.

The obtain the minima $\min_{\mathbf{w},b} L(\mathbf{w}, b, \alpha)$, we take derivatives of $L$ *w.r.t* $\mathbf{w}$ and $b$, and add the condition for $\alpha_i[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1]$ discussed above, then we get the Karush–Kuhn–Tucker (KKT) conditions:

$$ \nabla_\mathbf{w} L = w - \sum_{i=1}^{m} \alpha_i y_i \mathbf{x}_i = 0 \quad \Rightarrow \quad \mathbf{w} = \sum_{i=1}^{m} \alpha_i y_i \mathbf{x}_i$$

$$ \nabla_{b} L = - \sum_{i=1}^{m} \alpha_i y_i = 0 \quad \Rightarrow \quad \sum_{i=1}^{m} \alpha_i y_i = 0 $$

$$ \forall i, \alpha_i[y_i(\mathbf{w} \cdot \mathbf{x_i} + b) - 1] = 0  \quad \Rightarrow \quad \alpha_i = 0 \lor y_i(\mathbf{w} \cdot \mathbf{x_i} + b) = 1$$


## Dual Problem ##
Substitute minima satisfying KKT conditions into lagragian function $L$, yields $g$:

$$ g(\mathbf{\alpha}) = \min_{\mathbf{w},b} f(\mathbf{w})=\frac{1}{2} \lVert \sum_{i=1}^{m} \alpha_i y_i \mathbf{x}_i \rVert^2 - \sum_{i=1}^{m} \alpha_i[y_i(\sum_{i=j}^{m} \alpha_j y_j \mathbf{x}_j \cdot \mathbf{x_i} + b) - 1] $$

expands to:

$$ g(\mathbf{\alpha}) = \frac{1}{2} \lVert \sum_{i=1}^{m} \alpha_i y_i \mathbf{x}_i \rVert^2 - \sum_{i=1,j=1}^{m} \alpha_i \alpha_j y_i y_j \mathbf{x_i} \cdot \mathbf{x}_j - \sum_{i=1}^{m} \alpha_i y_i b + \sum_{i=1}^{m} \alpha_i $$

intentionally combine the first and the second term, notice that the third term is $0$ due to KKT condition:

$$ g(\mathbf{\alpha}) = \sum_{i=1}^{m} \alpha_i - \frac{1}{2} \sum_{i=1,j=1}^{m} \alpha_i \alpha_j y_i y_j (\mathbf{x_i} \cdot \mathbf{x}_j) $$


Here $g(\alpha)$ is already the mimimal value of $f(\mathbf{w})$, regardless of $\mathbf{\alpha}$. But we still need to find the mimima solution points expressed in $\mathbf{\alpha}$. To obtain a feasible solution in $\mathbf{\alpha}$, we solve for the **dual problem** of the lagragian function:

$$ \max_{\mathbf{\alpha}} g(\mathbf{\alpha}) = \max_{\mathbf{\alpha}}  \sum_{i=1}^{m} \alpha_i - \frac{1}{2} \sum_{i=1,j=1}^{m} \alpha_i \alpha_j y_i y_j (\mathbf{x_i} \cdot \mathbf{x}_j) $$
 
subject to: 

$$ \alpha_i \geq 0 \land \sum_{i=1}^{m} \alpha_i y_i = 0 \quad \forall i \in [m]$$

This dual problem is a convex **Quadratic Programming** problem of which the objective function $g(\mathbf{\alpha})$ has a negative semidefinite Hessian. And all constrains are affine and convex. So the maxima can be obtained through many classic solvers, e.g. **Conjugate Gradient** method. The solution $\mathbf{\alpha^*}$ of this problem actually give us the best lower bound of the original problem.

## Solution of SVM ##

### Hard Margin ###

And all variables in the original SVM problem can be expressed in $\mathbf{\alpha}$. 

Prediction $h(\mathbf{x})$:

$$h(\mathbf{x}) = sgn(\mathbf{w} \cdot \mathbf{x} + b) = sgn(\sum_{i=1}^{m} (\alpha_i y_i \mathbf{x}_i) \cdot \mathbf{x} + b) $$

Intercept $b$ can be absorbed into $\mathbf{w}$. If not, each support vector $x_i$ has its own $b_i$:

$$ b_i = y_i - \sum_{j=1}^{m} \alpha_i y_i (\mathbf{x_j} \cdot \mathbf{x_i})$$

For the hyperplane parameters $\mathbf{w}$, we can go back to the expanded expression of 

$$ g(\mathbf{\alpha}) = \frac{1}{2} \lVert \sum_{i=1}^{m} \alpha_i y_i \mathbf{x}_i \rVert^2 - \sum_{i=1,j=1}^{m} \alpha_i \alpha_j y_i y_j \mathbf{x_i} \cdot \mathbf{x}_j - \sum_{i=1}^{m} \alpha_i y_i b + \sum_{i=1}^{m} \alpha_i $$

Here the last 3 terms sum to $0$:

$$ - \sum_{i=1,j=1}^{m} \alpha_i \alpha_j y_i y_j \mathbf{x_i} \cdot \mathbf{x}_j - \sum_{i=1}^{m} \alpha_i y_i b + \sum_{i=1}^{m} \alpha_i = 0 $$

and the third term is $0$:

$$\sum_{i=1}^{m} \alpha_i y_i b = 0 $$

Yields:

$$ - \sum_{i=1,j=1}^{m} \alpha_i \alpha_j y_i y_j \mathbf{x_i} \cdot \mathbf{x}_j + \sum_{i=1}^{m} \alpha_i = 0 \quad \Rightarrow \quad \lVert \mathbf{w} \rVert^2 - \sum_{i=1}^{m} \alpha_i = 0 $$

$$ \quad\lVert \mathbf{w} \rVert^2 = \sum_{i=1}^{m} \alpha_i$$

And the margin $M$ of SVM:

$$ M = \frac{1}{\lVert \mathbf{w} \rVert} = \frac{1}{\sqrt{\lVert \mathbf{w} \rVert^2}} = \frac{1}{\sqrt{\sum_{i=1}^{m} \alpha_i}} $$ 


### Soft Margin ###

The derivation of soft margin SVM problem is almost the same as the hard margin case, in addition with the slack variables $\xi_i$. 

The detailed derivation is not presented here, and the resultant dual problem is:

$$ \max_{\mathbf{\alpha}} g(\mathbf{\alpha}) = \max_{\mathbf{\alpha}}  \sum_{i=1}^{m} \alpha_i - \frac{1}{2} \sum_{i=1,j=1}^{m} \alpha_i \alpha_j y_i y_j (\mathbf{x_i} \cdot \mathbf{x}_j) $$
 
subject to: 

$$ 0 \leq \alpha_i \leq C \land \sum_{i=1}^{m} \alpha_i y_i = 0 \quad \forall i \in [m]$$

The only difference is that the constraints $\alpha_i \leq C$ induced by the lagrage multipliers $\beta_i$ of slack variables $\xi_i$. Thus this problem can be solved in the same way as the hard margin case above.