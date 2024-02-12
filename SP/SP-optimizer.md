# Optimizer
”Given the loss $g_t$ , how much should we update model's weight?“*


$$g_t=\nabla_{\theta} L(\theta)$$
$$\theta = [\theta_1, \theta_2, ..., \theta_n]^T$$
$$\nabla_{\theta} L(\theta) = \left[ \frac{\partial L}{\partial \theta_1}, \frac{\partial L}{\partial \theta_2}, \ldots, \frac{\partial L}{\partial \theta_n} \right]^T$$
## SGD
update the weights linearly based on learning rate $\eta$.
$$\theta_{t+1}=\theta_t-\eta \cdot g_t$$

## Adam
$$\theta_{t+1} = \theta_t - \frac{\eta \cdot \hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}$$

$$m_t=\beta_1\cdot m_{t-1}+(1-\beta_1)\cdot g_{t},\ \hat{m_t}=\frac{m_t}{1-\beta_1^t}$$

$$v_t=\beta_2\cdot v_{t-1}+(1-\beta_2)\cdot g_t^2,\ \hat{v_t}=\frac{v_t}{1-\beta_2^t}$$

To be more clear,
$$\theta_{t+1} = \theta_t - \left(\frac{\eta}{\sqrt{\hat{v}_t} + \epsilon}\right) \cdot \hat{m}_t$$
In this way, Adam makes both learning rate and learning step adaptively: weighted moving average instead of naive $g_t$; weighted moving variance at the denominator.

### $m_t$
- is called **momentum** because it updates in a way analogous to physical systems. In the equation $P' = P + F \cdot \Delta t$, we treat the gradient as a force $F$, and $\Delta t = 1$ for one iteration. By incorporating weight decay, the formula becomes $P' = \beta P + (1 - \beta)F$, where $\beta$ is the momentum coefficient.
  - help in overcoming local minima or saddle points
- is called **estimation of the first moment** because it calculates a value (weighted sum) related to the first moment (mean) of the distribution of gradients. Any reasonable function calculating this can be considered its estimation, such as the simple moving average (SMA). 
  - the $k^{th}$ moment (矩) of a random variable is $\mathbb{E}[(X - n)^k]$.
  - if $n = 0$, the moment is "raw" or "crude".
  - if $n = \mathbb{E}[X]$, it's "central". 
  - otherwise, it's "uncentered".
- not any arbitrary function or random variable $X'$ can be considered a suitable estimator. We should consider: 
  - **biased or unbiased**: An unbiased estimator satisfies $\mathbb{E}[X'] = \mu$, where $\mu$ is the true parameter. (无偏估计)
  - **consistent**: An estimator is consistent if it converges to the true parameter as the sample size increases.
  - **variance**: An estimator with low variance is preferred, as it reliably approximates the true parameter over time.
- is called **exponential moving average**， a kind of weighted moving average.
- when gradients suddenly changed, step size $\delta(\theta)$ will not fully follow since it is weightedly averaged by previous values.

### $v_t$
-  is called **estimation of the uncentered second moment** because it calculates a value (weighted sum of squared sample) related to the uncentered second moment of the distribution of gradients.
-  is called "adaptive learning rate term" as it adjusts the learning rate for each parameter based on the historical squared gradients.
-  can handle sparse gradients because it encourage bigger gradients if the gradient always near-zero or zero.
   -  sparse gradient: many near-zero / zero items in gradient vector.
   -  caused by sparse input data, ReLU, L1 and etc.
- when gradients are stable (with less variance) over a long period, $\sqrt{v_t}$ in the denominator is low so that encourage a bigger $\delta(\theta)$.


### $\hat{m_t}$

$\hat{m_t}$ is the bias-corrected version of $m_t$, ensuring that $\mathbb{E}[\hat{m}_t] = \mu$.

$$
m_t = (1-\beta_1)g_t + \beta_1[(1-\beta_1)g_{t-1} + \beta_1[(1-\beta_1)g_{t-2} + \ldots]]
$$

$$
m_t = \beta_1m_0+(1-\beta_1)\sum_{i=0}^{t-1} \beta_1^i g_{t-i}
$$

Given that $m_t$ is initialized at 0, this implies $m_0 = 0$. Therefore,

$$
\mathbb{E}[m_t] = \mathbb{E}\left[(1-\beta_1)\sum_{i=0}^{t-1} \beta_1^i g_{t-i}\right]
$$

Considering that $\mathbb{E}[g_t] = \mu$,

$$
\mathbb{E}[m_t] = (1-\beta_1)\sum_{i=0}^{t-1} \beta_1^i \mu + (1-\beta_1)\mathbb{E}\left[\sum_{i=0}^{t-1} \beta_1^i (g_{t-i}-g_t)\right]
$$

We aim to demonstrate that the second term is significantly smaller than the first, allowing us to neglect it, which indeed is the case. As $t$ increases, the gradient tends to stabilize, making $g_{t-i} - g_t \approx 0$. Alternatively, we can choose $\beta_1$ such that the sum result is smaller than that in the first term, since the weights are adjusted to assign less importance to historical values, making 
$$\sum{avg}>\sum{(most\ small, few\ big)}$$
Thus, the second term can be ignored.

$$
\mathbb{E}[m_t] = (1-\beta_1)\sum_{i=0}^{t-1} \beta_1^i \mu + \zeta
$$

Using the sum of a geometric series, we have

$$
\mathbb{E}[m_t] = (1-\beta_1)\mu\frac{1-\beta_1^t}{1-\beta_1}
$$

To make the estimation unbiased,

$$
\hat{m}_t=\frac{m_t}{1-\beta_1^t}
$$

so that

$$
\mathbb{E}[\hat{m}_t] = \frac{\mathbb{E}[m_t]}{1 - \beta_1^t}=\frac{\mu(1-\beta_1^t)}{1 - \beta_1^t} = \mu
$$

In this way, we discount the bias and make the estimation more accurate. As we can see, the bias is very apparent at the beginning of the iteration, where $m_1=(1-\beta_1^1)\mu$. With $t$ goes up, $1-\beta_1^t$ as well as bias decrease sharply.

### take away
I was confused by a question for a while, 

>We calculate an ESTIMATION of the gradients' expectation and correct the bias to push it closer to the true expectation. HOWEVER, why don't we just use the true expectation with $m_t = \frac{1}{t}(m_{t-1} \cdot (t-1) + g_t)$?

First, We use Exponential Weighted Moving Average (EWMA) only because its exponential weighting, which is more advantageous than the simple average, but not "the best try we could estimate the expectation".

Second, the simple moving average indeed closer to true expectation since it is unbiased. However, we can also make EWMA unbiased, while the simple moving average do not have the weighting advantages.

There are lots of estimator of expectation, but we care more about the instant value that affects the optimization progress. ADDITIONALLY, we can correct bias to make the estimator an unbiased estimator.



## Adagrad
## RMSprop

## Reference
[Adam](https://blog.csdn.net/qq_38204302/article/details/120065362)