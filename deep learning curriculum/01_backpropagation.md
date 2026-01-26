# Backpropagation

## Stochastic gradient descent (SGD)

Stochastic Gradient Descent (SGD) updates the weights (model parameters) by moving them in the opposite direction of the gradient of the loss, computed on a batch of data.

$$
\theta_{t+1} = \theta_t - \eta \, \nabla_\theta \mathcal{L}(\theta_t)
$$

- $\theta_{t+1}$ → updated parameters
- $\theta_t$ → model parameters at iteration $t$
- $\eta$ → learning rate (step size controlling how big the update is)


- $\mathcal{L}$ → loss function
- $\nabla_\theta \mathcal{L}(\theta_t)$ → gradient of the loss w.r.t. parameters (computed via backpropagation)

## Momentum

Momentum extends the stochastic gradient descent by accumulating the previous gradients into a momentum term.
This allows to speed up convergence and also to reduce oscillation

If the current gradient points towards the same direction as the previous ones → go further than what the current gradient indicates
If the current gradient points towards the opposite direction as the previous ones → go closer than what the current gradient indicates

**Momentum optimizer**

Update equations:

$$
\theta_{t+1} = \theta_t - \eta \, v_t
$$

$$
v_t = \mu v_{t-1} + \nabla_\theta \mathcal{L}(\theta_t)
$$


where:

- $\theta_{t+1}$ → updated model parameters  
- $\theta_t$ → model parameters at step $t$  
- $\eta$ → learning rate


- $v_t$ → velocity (accumulated gradient at step $t$)  
- $v_{t-1}$ → velocity from the previous step  
- $\mu$ → momentum coefficient (typically 0.9)  
- $\nabla_\theta \mathcal{L}(\theta_t)$ → gradient of the loss w.r.t. parameters (from backpropagation)  


**Practical rule of thumb**
- Start with $\mu = 0.9$
- Increase toward 0.95–0.99 only if training is unstable or very noisy
- Values below 0.8 are rare in modern deep learning
In practice, $\mu = 0.9$ is the canonical choice unless you have a specific reason to change it.

## Adam


## Learning rate schedules


## Regularization