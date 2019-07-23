# Mathematical Basics of Linear Regression and SVM

**Linear Regression** and **Supporting Vector Machine (SVM)** are two common supervised learning techniques.  
The purpose of this post is to make a minimal review of these two methods.

### Category
```
* Linear Regression
* Supporting Vector Machine
```

## Linear Regression

Linear regression model has form where $m$ is dimensionality: 
$$ f(x) = \beta_0 + \sum_{j=1}^{m} X_j \beta_j$$
The loss function to be minimized is the residual sum of squares (RSS):
$$ RSS(\beta) = \sum_{i=1}^{N} (y_i - f(x_i))^2 = \sum_{i=1}^{N} (y_i - \beta_0 - \sum_{j=1}^{m} x_{ij} \beta_j)^2 $$
In matrix notation (with $\beta_0$ merged into $\beta$):
$$ RSS(\beta) = (\mathbf{y}-\mathbf{X}\beta)^{T}(\mathbf{y}-\mathbf{X}\beta)$$
Differentiate w.r.t $\beta$:
$$ \frac{\partial{RSS}}{\partial{\beta}} = -2\mathbf{X}^T (\mathbf{y}-\mathbf{X}\beta)$$
$$ \frac{\partial^2{RSS}}{\partial{\beta}\partial{\beta}^T} = 2\mathbf{X}^T \mathbf{X}$$
Given that $\mathbf{X}$ has full column rank, which means each feature is independent, hence $\mathbf{X}^T \mathbf{X}$ is positive definite, then the stationary point is a minimum. Make the derivative to be zero:
$$\mathbf{X}^T (\mathbf{y}-\mathbf{X}\beta) = 0$$
$$\mathbf{X}^T \mathbf{y}= \mathbf{X}^T\mathbf{X}\beta$$
The unique solution:
$$ \widehat{\beta} = (\mathbf{X}^T\mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}$$
And the corresponding prediction:
$$ \widehat{\mathbf{y}} = \mathbf{X}\widehat{\beta} = \mathbf{X}(\mathbf{X}^T\mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}$$

### With Ridge Regularization ###

Linear regression model has form where $m$ is dimensionality: 
$$ f(x) = \beta_0 + \sum_{j=1}^{m} X_j \beta_j$$

## Supporing Vector Machine

Use a hyperplane to separate two classes. The hyperplane:
$$ x^T \beta + \beta_0 = 0$$
$\beta$ is scaled to satisfy the canonical representation: if $x^*$ is the closet point to the hyperplane, a.k.a $x$ is a on boundary:
$$ |{x^*}^T \beta + \beta_0| = 1$$
The distance from a boundary to the separating hyperplane is:
$$ M = \frac{1}{\lVert\beta\rVert}$$

### Hard Margin SVM ###

Hard margin SVM works only when the two classes are completely linearly separable. The problem of hard margin SVM is find the biggest margin $M$ for separation, equivalent to solve:
$$ \max_{\beta, \beta_0, \lVert\beta\rVert} M \Rightarrow \min_{\beta, \beta_0, \lVert\beta\rVert} \frac{1}{\lVert\beta\rVert}$$
subject to $$ y_i(x_i^T\beta + \beta) \geq 1, \forall i$$
The physical meaning of the above system of inequalities is maximize the margin while keeping all points classified beyond boundaries of prediction $\{+1, -1\}$, a.k.a, giving $+1$ labeled data a prediction $\geq 1$, and giving $-1$ labeled data a prediction $\leq -1$.
