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
* Primal and Dual 
* Physical Meanings 
* Hinge Loss
```

## Classical SVM Problems

### Separable Case ###

In separable case, the hard margin SVM problem is a convex minimization problem as:

$$ \min_{\mathbf{w}, b} \frac{1}{2} \lVert \mathbf{w} \rVert^2 $$

subject to constraints:

$$ y_i(\mathbf{w} \cdot \mathbf{x_i} + b) \geq 1, \quad \forall i \in [N] $$

Where $\mathbf{w}$ and $b$ are the weights and intercept of the separating hyperplane, $x_i$ and $y_i$ is the features and label of the data points, $N$ is total number of points.

### Non-Separable Case ###

In non-separable case, the soft margin SVM problem is a convex minimization problem as:

$$ \min_{\mathbf{w}, b} \frac{1}{2} \lVert \mathbf{w} \rVert^2 + C \sum_{i=1}^{N} \xi_i^p$$

subject to constraints:

$$ y_i(\mathbf{w} \cdot \mathbf{x_i} + b) \geq 1 - \xi_i \quad \land \quad \xi_i \geq 0, \quad \forall i \in [N] $$

The meanings of variables follow the separable case, with $\xi$ as slack variables describing the violations of margin. And $p$ is the order of slack penalty. If $p = 1$, the loss function of the soft margin SVM is hinge loss; If $p = 2$ the loss is quadratic hinge loss.