
- [Introduction](#introduction)
- [Embeddings and Hyperbolic Geo](#embeddings-and-hyperbolic-geo)
- [Poincaré Embeddings](#poincaré-embeddings)

## Introduction

This paper explores using Poincare Embeddings to more efficiently represent symbolic data such as text and graphs, where embedding refers to how we choose to encode high level data to low level in machine learning models. For example, embeddings are used to translate image data to ML-applicable data; because images lose out on the locality property (where significance is placed on pixels closer to each other) images can't just be translated into pixels into a 1d array by going column by column and then row by row and should be pooled (like in a CNN) for greater efficiency.

One of the problems with modern embedding applications though is their inability to deal with high order dimension data e.g. large graph structures. Thus, this paper deals with data that can be organized in a latent hierarchy where we can utilize the fact that continous trees can be embedded into a finite hyperbolic space i.e. the Poincaré ball model. 

## Embeddings and Hyperbolic Geo

Hyperbolic space is helpful because it allows the modelling of exponential growth rates in space and is perfect for modelling something like where the # of nodes at level $l$ of a tree is $k^l$. This is due to the negative curvature in space, where distance grows exponentially as you approach the border of the circle. Think of it as looking down on a sphere - the many points on the circumference look infinitely close to each other, but make it a morphed sphere. This fits perfectly in power-law distributions, already seen helpful in things like natural language and greedy routing in geography (Kleinberg).

![hi](./img/Poincare_Embeddings_1.png)

## Poincaré Embeddings

More specfiically, we can describe the Poincare ball model in relation to the Riemannian manifold: $$g_x=(\frac{2}{1-||x||^2})^2g^E$$ where $x\in \beta^d$ and $g^E$ denotes the Euclidean [metric tensor](https://www.youtube.com/watch?v=Hf-BxbtCg_A&ab_channel=Dialect). Notice how as ||x|| approaches 1, $g_x$ scales by a factor quickly approaching to infinity. The distance formula follows by plugging $u$,$v$ and using properties of manifolds: $$d(u,v) = cosh^{-1}(1+2\frac{||{u}-{v}||^2}{(1-||{u}^2||)(1-||v||^2)})$$

Now, to translate $S={\{x_i\}}_{i=1}^n$, for a given problem-specific loss function, we attempt to minimize: $$\Theta'\leftarrow argmin_\Theta L(\Theta)$$

Optimizations for finding this follow in section 3.1. Intuitively, they also use gradient descent just like in neural networks attempting to minimize loss functions. The gradient is a little different though, where $$\nabla = \frac{\partial{\mathcal{L(\theta)}}}{\partial{d(\theta,x)}}\frac{\partial{d(\theta,x)}}{\partial\theta}$$

An interesting observation to be made also is constraining embeddings to ensure computers can still easily compute calculations. They use the projection: $$proj(\theta) = ||\theta||\ge1? \frac{\theta}{||\theta||}-\epsilon:\theta$$
where $\epsilon=10^{-5}$.

For training details, see section 3.2 and 4, but it is interesting to see that Poincaré embeddings can already greatly reduce dimensionality in large-scale data. We see that compared to traditional Euclidean embeddings, Poincaré embeddings can logarithimically decrease dimension size.  

![hi](./img/Poincare_Embeddings_2.png)

![hi](./img/Poincare_Embeddings_3.png)