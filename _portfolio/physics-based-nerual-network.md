---
title: "Physics-Informed Neural Networks"
excerpt: "A non-intrusive surrogate model for modeling PDEs<br/><img src='/images/500x300.png'>"
collection: portfolio
---

This project is a collection of Physics-Informed Neural Networks (PINNS) using TensorFlow for multiple different PDEs

[![GitHub](https://img.shields.io/badge/GitHub-Current%20Release-green.svg)](https://github.com/tofstie/PINNS)
[![Documentation](https://img.shields.io/badge/Documentation-blue.svg)](https://tofstie.github.io/PINNS/)
## Current Supported PDEs

Currently, there are multiple linear and non-linear ODEs/PDEs that are supported.
- 1D Damped Pendulum Equation
- 1D/2D Linear Advection
- 1D Inviscid Burgers Equation

## How does it work?
PINNs work by incorporating the physics of the problem into the training of the neural network. To do so,
the loss function includes this physics. Typically this involves including three additional terms to the loss function
in addition to the standard MSE.

$$Loss = MSE + c_{R}R_{MSE} + c_{I}I_{MSE} + c_{B}B_{MSE}$$

In each of these terms, a scaler, $c$, is used to dictate the importance of each of the additional terms into the loss.  

### Residual Loss
The first addition is the residual loss of the model. This model involves the mean square error of the residual that
can be calculated using the PINN. To do so, the given equation is solved. For example,
the residual of the inviscid Burgers equation has the following form.

$$\frac{du_{NN}}{dt} + u_{NN}\frac{du_{NN}}{dx} = R$$

### Initial Condition Loss
To ensure that the NN adheres to the initial conditions, the difference between the initial condition and the model is
added to the loss explicitly.
$$I = u(x,0) - u_{NN}(x,0)$$

While the initial condition loss is shown for an equation that only requires one initial condition, for multiple initial conditions
each would need to be added to the initial condition loss.

### Boundary Condition Loss
Lastly, to ensure that the boundary conditions are upheld, a loss associated with them is added into the Loss. For periodic
boundary conditions, this boundary condition loss is explicitly zero. For non-periodic boundary conditions, the loss would then 
be the models adherence to this boundary condition. For example, the boundary loss for a 1D euler equation with
reflective wall boundaries is shown below.

$$B = u_{NN}(0,t) + u_{NN}(L,t)$$


