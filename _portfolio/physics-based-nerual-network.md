---
title: "Physics-Informed Neural Networks"
excerpt: "A non-intrusive surrogate model for modeling PDEs<br/><img src='/images/PINN/pendulum.gif' style='max-width: 100%; height: auto;'>"
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

### Example
To offer an example, take the 1D damped pendulum equation like the gif shown on the portfolio page.

$$\frac{d^2\theta}{dt^2} + k\frac{d\theta}{dt} + \theta = 0$$

The inputs in this case are $t$ and $k$, with the expected output being $\theta$.
The discretization has the following parameters:
- Number of timesteps: 200
- Final time: 5s
- Number of damping coefficients: 50 (Randomly distributed between 0.035 and 0.06)
- Initial Position: $\frac{2\pi}{5}$
- Initial Rotational Velocity: $0$

The equation is solved for each of the damping coefficients until the final time using
a two-step explict euler method.

The NN is then made with a dense input layer with 6 hidden layers and one output layer. Each uses the tanh activation function.

For the loss, the residual factor is set to $0.1$ and the initial condition factor is set to $1.0$.
The model is trained over 2000 iterations with a batch size of 64. $20\%$ of the data is used for testing and
$5\%$ of the training data is used for validation. All of the inputs are scaled based on the mean and 
standard deviation of the training data. The training data has the following representation of the input space.

<img src="/images/PINN/Example_Parameter_Space.png"> 

The inital learning rate is $1E-3$ with a exponential decay with a rate of $0.98$ over $400$ steps.
After running the training, the loss levels out after 125 iterations.

<img src="/images/PINN/Example_Iterations.png">

To test the accuracy of the model, a damping value of $0.05$ is chosen and the value of the model over the time range is compared
against the discretized solution. The result of the model can be seen below.

<img src="/images/PINN/Example_Output.png">

<img src="/images/PINN/pendulum.gif">