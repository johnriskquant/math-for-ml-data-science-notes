## Week 3: Sampling, Estimation, and Statistical Philosophies

### Population vs. Sample
*   **Population ($N$):** The entire group or set of items being studied. 
*   **Sample ($n$):** A smaller, measurable subset of the population. In Machine Learning, this represents our training data.
*   **Sampling without Replacement:** Once an item is drawn, it is not returned to the pool. While common, this creates dependence between draws, which can complicate experimental designs assuming Independent and Identically Distributed (i.i.d.) variables.
*   **Sample Size:** A larger sample size generally provides a more accurate estimate of the population parameters.
*   **Proportions:** 
    *   Population Proportion: $p = \frac{X}{N}$ (where $X$ is the number of items with a specific characteristic).
    *   Sample Proportion: $\hat{p} = \frac{x}{n}$ (an estimate of the population proportion).

### Sample Variance & Bessel's Correction
The population variance is calculated as $\sigma^2 = \frac{1}{N}\sum_{i=1}^{N}(X_i - \mu)^2$. However, in practice, the true population mean ($\mu$) is almost always unknown, so we must estimate variance using the sample mean ($\bar{x}$).

*   **The Bias Problem:** If we naively calculate sample variance as $\frac{1}{n}\sum_{i=1}^{n}(x_i - \bar{x})^2$, we systematically underestimate the true variance. This happens because the sample mean $\bar{x}$ is derived from the data itself, meaning the data points are mathematically closer to $\bar{x}$ than they are to the true, unknown $\mu$.
*   **The Correction:** To remove this bias, we divide the sum of squared differences by $n-1$ instead of $n$. This accounts for the loss of one "degree of freedom" used to calculate $\bar{x}$.
*   **Unbiased Sample Variance Formula:** 
    $$s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i - \bar{x})^2$$

### Law of Large Numbers (LLN)
The LLN states that as your sample size grows, the sample mean will converge to the true population mean. 
*   **Condition:** Requires random draws, sufficiently large $n$, and independent observations.
*   **Formal Definition:** As $n \to \infty$, the sample average $\bar{X}_n = \frac{1}{n}\sum_{i=1}^{n} X_i \to E[X] = \mu$.


### Law of Large Numbers (LLN)
The LLN states that as your sample size grows, the sample mean will converge to the true population mean. 
*   **Condition:** Requires random draws, sufficiently large $n$, and independent observations.
*   **Formal Definition:** As $n \to \infty$, the sample average $\bar{X}_n = \frac{1}{n}\sum_{i=1}^{n} X_i \to E[X] = \mu$. $\bar{X}_n = \frac{1}{n}\sum_{i=1}^{n} X_i \to E[X] = \mu$.

### Central Limit Theorem (CLT)
While the LLN tells us *where* the sample average is going, the CLT tells us what the *distribution* of those averages looks like.
*   **The Theorem:** If you draw repeated samples of size $n$ from a population, the distribution of those sample averages will approximate a Normal Distribution as $n$ gets larger—**regardless of the underlying population's original distribution** (whether uniform, binomial, highly skewed, etc.).
*   **Rule of Thumb:** A sample size of $n \ge 30$ is generally sufficient to assume normality for well-behaved distributions. Highly skewed data requires larger samples.
*   **Moments of the Sample Mean:**
    *   Mean of sample averages: $\mu_{\bar{X}} = \mu$
    *   Variance of sample averages: $\sigma^2_{\bar{X}} = \frac{\sigma^2}{n}$. (Notice that as $n$ increases, the variance shrinks, tightening the bell curve around the true mean).
*   **Formal Standardization:** 
    $$\lim_{n \to \infty} \frac{\bar{X}_n - \mu}{\sigma/\sqrt{n}} \sim N(0,1)$$

---

### Maximum Likelihood Estimation (MLE)
MLE is a frequentist method for estimating the parameters of a statistical model. It asks: *"Given the data we observed, which model parameters make this data the most likely to have occurred?"* We maximize the Likelihood function $L(\theta | \text{data}) = P(\text{data} | \theta)$.

Because products of many small probabilities cause computer underflow and are difficult to differentiate, we maximize the **Log-Likelihood** ($\ell$), turning products into sums: $\log(a \cdot b) = \log(a) + \log(b)$.

#### MLE for a Bernoulli Distribution (e.g., Coin Flips)
Suppose we flip a coin 10 times and get 8 Heads and 2 Tails. We want to find the parameter $p$ (probability of heads).
1.  **Likelihood:** $L(p) = p^8(1-p)^2$
2.  **Log-Likelihood:** $\ell(p) = 8\log(p) + 2\log(1-p)$
3.  **Derivative:** Set the derivative with respect to $p$ to zero:
    $$\frac{d}{dp} \ell(p) = \frac{8}{p} - \frac{2}{1-p} = 0$$
4.  **Solve:** $\hat{p} = \frac{8}{10}$. (This generalizes to $\hat{p} = \frac{k}{n}$).

#### MLE for a Gaussian Distribution
Suppose we draw a sample $\mathbf{X} = (x_1, \dots, x_n)$ from $N(\mu, \sigma^2)$.
1.  **Likelihood:**
    $$L(\mu, \sigma | \mathbf{X}) = \prod_{i=1}^{n} \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x_i - \mu)^2}{2\sigma^2}\right)$$
2.  **Log-Likelihood:**
    $$\ell(\mu, \sigma) = -\frac{n}{2}\log(2\pi) - n\log(\sigma) - \sum_{i=1}^{n} \frac{(x_i - \mu)^2}{2\sigma^2}$$
3.  **MLE for $\mu$:** Take the partial derivative with respect to $\mu$ and set to zero:
    $$\frac{\partial}{\partial \mu}\ell = \frac{1}{\sigma^2} \sum_{i=1}^{n} (x_i - \mu) = 0 \implies \hat{\mu} = \frac{1}{n}\sum_{i=1}^{n} x_i = \bar{x}$$
4.  **MLE for $\sigma$:** Take the partial derivative with respect to $\sigma$ and set to zero:
    $$\frac{\partial}{\partial \sigma}\ell = -\frac{n}{\sigma} + \frac{1}{\sigma^3}\sum_{i=1}^{n}(x_i - \mu)^2 = 0 \implies \hat{\sigma}^2 = \frac{1}{n}\sum_{i=1}^{n}(x_i - \bar{x})^2$$
    *(Note: The MLE for Gaussian variance yields the biased estimator divided by $n$, not $n-1$.)*

#### MLE and Linear Regression
Linear regression seeks to fit a line $y = mx + b$. If we assume the errors (residuals) around the true line are normally distributed, we can place a Gaussian distribution centered on the line for every $x_i$. 
Maximizing the likelihood of generating the observed points means maximizing $\exp\left(-\frac{1}{2} \sum d_i^2\right)$, where $d_i$ is the distance from the point to the line. Because of the negative exponent, **maximizing this likelihood is mathematically identical to minimizing the sum of squared distances.** Therefore, Ordinary Least Squares (OLS) regression is exactly MLE under the assumption of Gaussian noise.

---

### Frequentist vs. Bayesian Philosophies
*   **Frequentist:** Probabilities represent long-term frequencies of events. Data is random, but the underlying parameters are fixed. Models are judged solely by how well they explain the observed evidence (MLE).
*   **Bayesian:** Probabilities represent a degree of belief. You start with a **Prior** belief before seeing any data. Once data is gathered, you use Bayes' Theorem to update your belief, resulting in a **Posterior**.
    $$P(\text{Model} | \text{Data}) = \frac{P(\text{Data} | \text{Model}) \times P(\text{Model})}{P(\text{Data})}$$
    *Posterior $\propto$ Likelihood $\times$ Prior*

### MAP (Maximum A Posteriori)
MAP is the Bayesian equivalent to MLE. Instead of just finding the parameters that maximize the likelihood of the data, MAP finds the parameters that maximize the posterior distribution (the mode of the updated belief).

*   **Uninformative Priors:** If you assume all parameters are equally likely (a flat, uniform prior), the prior acts as a constant. The math reduces down perfectly, and **MAP = MLE**.
*   **Informative Priors:** If you have strong prior beliefs (e.g., assuming a coin is fair, $p \approx 0.5$), the MAP estimate will balance the likelihood of the data against the weight of the prior. If you flip 8 heads out of 10, MLE says $\hat{p}=0.8$, but MAP might pull that estimate down to $0.7$, constrained by the prior.
*   **Convergence:** As sample size $n \to \infty$, the likelihood of the data overwhelms the prior. The MAP estimate will eventually converge exactly to the MLE estimate.

### Bayesian Regularization in Machine Learning
A complex model fitting every data point perfectly is not inherently better; it often fails to generalize (overfitting). We apply **Regularization** to penalize model complexity. 
*   **The Bayesian Connection:** Adding a regularization penalty (like Ridge/L2) is mathematically identical to applying MAP estimation with a Gaussian prior centered at zero. You are encoding a *prior belief* that the model weights should be small and simple.
*   **New Loss Function:** 
    $$\text{Regularized Error} = \text{Loss Error} + \lambda \sum a_i^2$$
    Where $\lambda$ controls the strength of your prior belief against the complexity of the coefficients $a_i$.
