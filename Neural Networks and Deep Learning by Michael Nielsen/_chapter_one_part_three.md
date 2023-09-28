# Chapter One pt 3

## Learning with gradient descent

The dataset used in the digit classification model is the [MNIST dataset](http://yann.lecun.com/exdb/mnist/). It contains tens of thousands of handwritten digits.

- Handwriting samples from 250 people.
- Half Census Bureau employees, and half high school students.
- images are grayscale and 28x28 pixels in size.

Use $x$ to denotate a training input. Each training input $x$ is treated as a 784 (28x28) dimensional vector. Eact entry represents the gray value for a single pixel. the desired output is denotated as $y = y(x)$ where $y$ is a 10-dimensional vector each index representing a digit. For example: If $x$ depicts a 6, then $y(x) = (0,0,0,0,0,0,1,0,0,0)^T$[^1]

[^1]: T is the transpose operation.

The *quadratic cost function* or *mean squared error* quantifies how well the network approximates $y(x)$ for all training inputs:
$$C(w,b) = \cfrac{1}{2n}\sum_x||y(x)-a||^2$$

- $w$ denotes the collection of all weights in the network, $b$ all the biases.
- $n$ is the total number of training inputs.
- $a$ is the vector of all outputs when $x$ is the input. $a$ depends on $x$, $w$, and $b$.
- $||v||$ is the usual length function for a vector $\vec{v}$.[^2]

[^2]: The length, norm, or magnitude of a vector is given by $\sqrt{\sum_nv_n^2}$

$C(w, b)$ is always non-negative. $C(w, b) \approx 0$ when $y(x)$ is close to the output $a$ for all training inputs $x$. This indicates a well-trained algorithm. Therefore the aim of training is minimizing the cost function $C(w, b) \approx 0$. This algorithm used to achieve this is known as **gradient descent**.

Minimizing the *quadratic cost* is only a minimization of a proxy measure. The accuracy of image classification can only be examined after the cost is minimized. The cost function we are using is an *ad hoc* choice that can be modified to get different sets of weights and biases.

### Abandon some of the specifics for now, focus specifically on gradient descent

Minimizing an arbitrary function $C(v)$ where $v$ is any set of variables. In this case, $v_1$ and $v_2$. This creates a 3-dimensional graph:

![Alt text](images/valley.png)

We must find the global minimum of $C$.

Calculus can be used to find a minimum analytically by computing derivatives to find the extremum. However the process becomes extremely intensive the more variables are included in $C$. Neural networks often depend on billions of weights and biases making calculus infeasible.

Instead use the analogy of treating the slope of the graph as a valley, we can choose a starting point on the graph to simulate the motion of an imaginary ball rolling to the bottom. This can be simulated by calculating the derivative of a local area of $C$ and moving in the direction of a negative slope.

Moving the ball a small amount ($\Delta v_1$, $\Delta v_2$) in a random direction ($v_1$, $v_2$) changes $C$ as
$$\Delta C \approx \cfrac{\partial C}{\partial v_1}\Delta v_1 + \cfrac{\partial C}{\partial v_2}\Delta v_2$$
The goal is to change $v$, $\Delta v = (\Delta v_1, \Delta v_2)^T$[^1], such that $\Delta C$ is negative. We define the *gradient* of $C$ as $(\frac{\partial C}{\partial v_1}, \frac{\partial C}{\partial v_2})^T$[^1] denoted as $\nabla C$

$\Delta C$ can be written as: $\Delta C \approx \nabla C \cdot \Delta v$

Here we see that $\nabla C$ relates $\Delta v$ to $\Delta C$. We can also see how to choose $\Delta v$ to make $\Delta C$ negative: $\Delta v = -\eta \nabla C$ where $\eta$ is a small positive parameter called the *learning rate*. Therefore, $\Delta C \approx \nabla C \cdot \Delta v = - \eta \nabla C \cdot \nabla C = - \eta ||\nabla C||^2$. $||\nabla C||^2 \geq 0$ ensures that $\Delta C \leq 0$. We can then compute the motion for $v$ as:
$$v \rarr v'= v - \eta \nabla C$$
Computing this operation over and over will eventually lead to a local minimum.

One caveat is tha the *learning rate* $/eta$ needs to be small enough, or adjusted to be small enough, so that it doesn't overshoot the minima $\Delta C > 0$.

This equation can now be scaled up so that $C$ is a function of $m$ variables, $v_1, ... ,v_m$. Now $\Delta v = (\Delta v_1, ... ,\Delta v_m)^T$ and $\Delta C$ is still $\Delta C \approx \nabla C \cdot \Delta v$.

Gradient descent is not the "end all be all" of minimizing the cost function, however it is one optimized strategy for doing so. When trying to move $v$ so that $C$ is decreased as much as possible, with the constraint $||\Delta v|| = \epsilon$ where $\epsilon > 0$, $\Delta v = -\eta \nabla C$ where $\eta = \epsilon / ||\nabla C||$ proves that gradient can be viewed as a way to most immediately decrease $C$.
