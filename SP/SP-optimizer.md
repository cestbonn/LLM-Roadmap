# Optimizer
$$g_t=\nabla_{\theta} L(\theta)$$
$$\theta = [\theta_1, \theta_2, ..., \theta_n]^T$$
$$\nabla_{\theta} L(\theta) = \left[ \frac{\partial L}{\partial \theta_1}, \frac{\partial L}{\partial \theta_2}, \ldots, \frac{\partial L}{\partial \theta_n} \right]^T$$
## SGD
$$\theta_{t+1}=\theta_t-\eta \cdot g_t$$
## Adam
Check the unit (量纲) and we can find a fun fact that in Adam, the step size is determined by the learning rate multiplied by a ratio, whereas in SGD, the step size equals the learning rate multiplied by the gradient. This design provides Adam with flexibility and facilitates easy adjustment of the time step.

$$m_t=\beta_1\cdot m_{t-1}+(1-\beta_1)\cdot g_{t}$$
$$v_t=\beta_2\cdot v_{t-1}+(1-\beta_2)\cdot g_t^2$$
$$\theta_{t+1} = \theta_t - \frac{\eta \cdot \hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}$$
## Adagrad
## RMSprop