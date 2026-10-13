## Week 2: Expected Value, Variance, and Multivariate Distributions

### Expected Value (EV)
The expected value represents the theoretical mean of a random variable over an infinite number of trials. It is not necessarily the most likely outcome, nor is it the point that splits the data in half (that is the median). 

*   **Discrete Case:** $E[X] = \sum_{x} x P(x)$
*   **Continuous Case:** $E[X] = \int_{-\infty}^{\infty} x f(x) dx$

**The Physics Intuition (Probability Torque):** 
A common misconception is that the EV splits the distribution's probability mass perfectly in half. In reality, the EV acts as the center of mass, balancing the distribution similarly to rotational torque in physics ($\text{Torque} = \text{Force} \times \text{Distance}$). Outcomes further from the center exert more "probabilistic torque." 
*   *Casino/Poker Example:* If a poker strategy gives you a 99% chance to lose $10 and a 1% chance to win $2,000, the median outcome is losing $10. However, the EV is $(0.99 \times -10) + (0.01 \times 2000) = -9.9 + 20 = \$10.10$. The extreme distance of the $2,000 payout pulls the balancing point (EV) up, offsetting the massive probability weight of the $10 loss. Because the mean is sensitive to these outliers, risk analytics frequently look at the median or mode to understand central tendency without extreme tail distortion.

**Properties of Expectation:**
*   **EV of a Function:** To find the expected value of a function of a random variable, replace the realization with the function: $E[g(X)] = \sum g(x) P(x)$.
*   **Linearity of Expectation:** $E[X_1 + X_2] = E[X_1] + E[X_2]$.
    *   *The "So What":* This holds true **even if the variables are dependent**. In a credit risk portfolio, the expected loss of the total portfolio is always simply the sum of the expected losses of the individual loans, regardless of how correlated their default probabilities are.

### Variance and the Moments of a Distribution
While expected value measures the center, moments describe the shape and spread of the distribution.

*   **1st Moment (Mean):** $E[X]$. Measures central tendency.
*   **2nd Central Moment (Variance):** $Var(X) = E[(X-\mu)^2] = E[X^2] - (E[X])^2$. 
    *   *Risk Context:* Variance measures how far data points disperse from the expected outcome. It is the foundational proxy for uncertainty, volatility, and dispersion in loss forecasting.
*   **Standard Deviation:** $\sigma = \sqrt{Var(X)}$. Motivation: Variance squares the units (e.g., "squared dollars"), which is unintuitive. Taking the square root scales the dispersion metric back to the exact same units as the original random variable.
*   **3rd Central Moment (Skewness):** $E[\left(\frac{X-\mu}{\sigma}\right)^3]$. Measures asymmetry. A positive skew indicates a long right tail (rare massive gains); a negative skew indicates a long left tail (rare massive losses).
*   **4th Central Moment (Kurtosis):** $E[\left(\frac{X-\mu}{\sigma}\right)^4]$. Measures tail density. High kurtosis (fat tails) is critical in financial risk because it indicates extreme outlier events (like market crashes or systemic defaults) occur more frequently than a normal distribution predicts.

### The Normal Distribution & Sum of Gaussians
*   **The Empirical Rule (68-95-99.7):** For a normal distribution, ~68% of data falls within $1\sigma$ of the mean, ~95% within $2\sigma$, and ~99.7% within $3\sigma$.
*   **Sum of Gaussians:** When you add two independent normally distributed variables, the result is exactly Gaussian. 
    *   **Independent:** If $Z = X + Y$, then $\mu_Z = \mu_X + \mu_Y$ and $\sigma^2_Z = \sigma^2_X + \sigma^2_Y$.
    *   **Dependent:** If $X$ and $Y$ are correlated, $\sigma^2_Z = \sigma^2_X + \sigma^2_Y + 2Cov(X,Y)$.
    *   *Quant Risk Example:* When calculating an analytical normal Value at Risk (VaR) for a two-asset portfolio, you sum the distributions. The mean expected return is additive, but the portfolio variance relies heavily on the covariance term. If the assets have negative covariance, the total portfolio risk ($\sigma^2_Z$) shrinks—this is the mathematical engine behind portfolio diversification.

### Visualizing Data Distributions
*   **Quantiles & Quartiles:** The $K$% quantile splits the data so $K$% is to the left and $(100-K)$% is to the right. The 25% quantile is the 1st Quartile (Q1), 50% is the Median (Q2), and 75% is the 3rd Quartile (Q3).
*   **Box Plots:** Standardized display using 5 statistics: Minimum, Q1, Median, Q3, and Maximum. 
    *   **Interquartile Range (IQR):** $Q3 - Q1$. This captures the middle 50% of the data.
    *   **Whiskers & Outliers:** Whiskers extend to $Q1 - 1.5 \times IQR$ and $Q3 + 1.5 \times IQR$, capped at the actual min/max of the data. Any points falling outside the whiskers are flagged as outliers.
*   **Kernel Density Estimation (KDE):** Approximates a continuous Probability Density Function (PDF) from discrete data. It places a small Gaussian curve (a "kernel") over every single data point, scales them by $1/n$, and sums them up to create a smooth density curve.
*   **QQ Plots (Quantile-Quantile):** Used to check if data is normally distributed. It plots the standardized quantiles of your data against the theoretical quantiles of a standard normal distribution. If the data is Gaussian, the points will form a straight diagonal line. Deviations at the edges indicate heavy/light tails.
*   **Violin Plots:** A hybrid visualization combining the summary statistics of a Box Plot in the center with the smooth, mirrored distribution shape of a KDE curve on the edges.

### Multivariate Distributions
*   **Joint Distribution:** The probability of two variables occurring simultaneously, denoted $P(X=x, Y=y)$ for discrete data, or mapped as a 3D density surface $f(x,y)$ for continuous data (e.g., mapping call waiting times against customer satisfaction ratings). If independent, $P(X,Y) = P(X)P(Y)$.
*   **Marginal Distribution:** Isolates one variable by collapsing the other. You aggregate (sum or integrate) all probabilities across the variable you want to ignore.
*   **Conditional Distribution:** The distribution of one variable given a specific, fixed value of the other. You take a "slice" of the joint distribution at that fixed value and normalize the remaining probabilities so they sum to 1: $P(Y|X) = \frac{P(X,Y)}{P(X)}$.

### Covariance, Correlation, and the Multivariate Gaussian
*   **Covariance:** Measures the directional relationship between two variables. 
    *   Formula: $Cov(X,Y) = E[(X-\mu_X)(Y-\mu_Y)]$. 
    *   Positive covariance means variables move together; negative means they move inversely; zero implies no linear relationship.
*   **Covariance Matrix ($\Sigma$):** For multidimensional data, variances are placed on the main diagonal, and the covariances between variable pairs fill the off-diagonal slots.
*   **Correlation Coefficient ($\rho$):** Standardizes covariance into a strict bound between -1 and 1, allowing you to gauge the *strength* of the relationship regardless of the variables' underlying units.
    *   Formula: $\rho = \frac{Cov(X,Y)}{\sigma_X \sigma_Y}$
*   **Multivariate Gaussian Distribution:** Extends the bell curve into multiple dimensions. 
    *   Instead of a scalar mean $\mu$, it uses a mean vector $\boldsymbol{\mu}$. 
    *   Instead of a scalar variance $\sigma^2$, it relies on the covariance matrix $\Sigma$. 
    *   The determinant of the covariance matrix ($|\Sigma|$) captures the total n-dimensional volume of the spread, while the inverse matrix ($\Sigma^{-1}$) scales the exponent.
    *   Formula: $$f(\mathbf{x}) = \frac{1}{(2\pi)^{n/2}|\Sigma|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T \Sigma^{-1} (\mathbf{x}-\boldsymbol{\mu})\right)$$
