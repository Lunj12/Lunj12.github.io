---
layout: post
mathjax: true
comments: true
title:  "Stochastic Gradient Descent, Momentum, Nestrov Accelerated Gradient"
date:   2019-07-23 20:13:07 -0700
categories: blog posts
---

**Stochastic Gradient Descent (SGD)** is a commom and simple way to solve convex optimzation problems. The purpose of this post is to concisely introduce this method and its two famous variants.

### Category
```
* Stochastic Gradient Descent
* SGD with Momentum
* Nestrov Accelerated Gradient
```

## Stochastic Gradient Descent

The vanilla version of Stochastic Gradient Descent (SGD):

$$ \theta = \theta - \eta \cdot \nabla_{\theta} J(\theta) $$

Where $\theta$ is the parameters to updated, $\eta$ is the learning rate, $J$ is the objective function.

## SGD with Momentum

The performance of SGD deteriorates when navigating ravines, *i.e. areas where the surface curves much more steeply in one dimension than in another*. In this case SGD will cause the $\theta$ value oscillate between the cliffs between the ravine bottom, with slow and hesitant progress.

The above behavior is due to the rapidly changing direction between two steps of SGD. To mitigate this problem, we can use SGD with Momentum, of which the basic idea is adding a **momentum** vector of old direction to undermine the direction change between cliffs.

The implementation is:

$$ v_{t} = \gamma v_{t-1} + \eta \cdot \nabla_{\theta_{t-1}} J(\theta_{t-1})$$

$$ \theta_t = \theta_{t-1} - v_{t}$$

Or equivalently,

$$ v_{t} = \beta v_{t-1} + (1 - \beta) \cdot \nabla_{\theta_{t-1}} J(\theta_{t-1})$$

$$ \theta_t = \theta_{t-1} - \alpha v_{t}$$

where $\alpha \beta = \gamma$ and $\alpha (1-\beta) = \eta$.

## Nestrov Accelerated Gradient

An improvement of SGD with Momentum to further damp unnecessary oscillations is Nestrov Accelerated Gradient (NAG):

$$ v_{t} = \gamma v_{t-1} + \eta \cdot \nabla_{\theta_{t-1}} J(\theta_{t-1} - \gamma v_{t-1})$$

$$ \theta_t = \theta_{t-1} - v_{t}$$

The principal difference between SGD with momentum and NAG is that NAG calculates gradient based on an **indicator** ($\theta_{t-1} - \gamma v_{t-1}$), rather than the true position $\theta_{t-1}$. In each step, we first obtain a prescience of the position where $\theta$ would go if following the old momentum $v_{t-1}$, then calculate gradient based on this prescience (indicator), and add this term as an correction vector to the old momentum vector.

### References

[1] *An overview of gradient descent optimization algorithms*: http://ruder.io/optimizing-gradient-descent/index.html#nesterovacceleratedgradient

[2] *Understanding Nesterov Momentum (NAG)* https://dominikschmidt.xyz/nesterov-momentum/
