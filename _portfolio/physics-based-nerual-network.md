---
title: "Physics-Informed Neural Networks"
excerpt: "A non-intrusive surrogate model for modeling PDEs<br/><img src='/images/500x300.png'>"
collection: portfolio
---

This project is a collection of Physics-Informed Neural Networks (PINNS) using TensorFlow for multiple different PDEs

## Current Supported PDEs

Currently, there are multiple linear and non-linear ODEs/PDEs that are supported.
- 1D Damped Pendulum Equation
- 1D/2D Linear Advection
- 1D Inviscid Burgers Equation

## How does it work?
PINNs work by incorporating the physics of the problem into the training of the neural network. To do so,
the loss function includes this physics. Typically this involves including three additional terms to the loss function
in addition to the standard MSE.

$$Loss = MSE + R_{MSE} + IC_{MSE} + BC_{MSE}$$

### Residual Loss
The first addition is the residual loss of the model. This model involves the mean square error of the residual that
can be calculated using the PINN. To do so, the given equation is solved. For example,
the residual of the inviscid Burgers equation has the following form.

$$\frac{du}{dt} + u\frac{du}{dx} = R$$

### Initial Condition Loss


