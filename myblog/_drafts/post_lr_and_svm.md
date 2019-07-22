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

