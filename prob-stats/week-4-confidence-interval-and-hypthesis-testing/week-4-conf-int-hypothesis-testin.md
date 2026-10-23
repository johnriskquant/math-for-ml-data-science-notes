## Week 4: Confidence Intervals and Hypothesis Testing

### Confidence Intervals
A confidence interval (CI) is an estimated range of values that is likely to include an unknown population parameter (like the population mean, $\mu$). 
*   **Key Distinction:** The population parameter ($\mu$) is fixed and unknown. The *interval itself* is what is randomly generated from our sample data.
*   **Interpretation:** A 95% confidence interval means that if you were to repeat the sampling process infinitely, 95% of the constructed intervals would successfully contain the true population parameter. It does **not** mean there is a 95% probability that the fixed parameter falls within any single, specific interval.
*   **Structure:** $\text{Confidence Interval} = \text{Point Estimate (e.g., } \bar{x}) \pm \text{Margin of Error}$
*   **Trade-offs:** 
    *   To shrink the margin of error (make the interval tighter) while keeping the same confidence level, you must increase the sample size ($n$). As sample size increases, the standard error shrinks.
    *   Decreasing the confidence level (e.g., from 99% to 90%) will also shrink the interval, but at the cost of a higher error rate.

#### CI for Mean (Known Variance)
If the population standard deviation ($\sigma$) is known, and assuming $n \ge 30$ or the population is normal:
*   $\bar{X} \sim N(\mu, \frac{\sigma^2}{n})$
*   **Formula:** $\bar{x} \pm Z_{1-\alpha/2} \left( \frac{\sigma}{\sqrt{n}} \right)$
*   *Note: $`Z_{1-\alpha/2}`$ is the Z-score corresponding to the desired confidence level (e.g., 1.96 for 95% confidence, where $`\alpha = 0.05`$).*
*   **Calculating Minimum Sample Size:** To achieve a specific Margin of Error (MOE): 
    $$n \ge \left( \frac{Z_{1-\alpha/2} \cdot \sigma}{\text{MOE}} \right)^2$$

#### CI for Mean (Unknown Variance)
When the population $\sigma$ is unknown, we substitute it with the sample standard deviation ($s$). This introduces extra uncertainty, requiring us to swap the Normal distribution for the **Student's t-distribution**.
*   **Formula:** $\bar{x} \pm t_{n-1, 1-\alpha/2} \left( \frac{s}{\sqrt{n}} \right)$
*   **The t-Distribution:** Bell-shaped but with "fatter tails" than a normal distribution, reflecting the added uncertainty of estimating variance from a sample. 
*   **Degrees of Freedom (df):** Parameterized by $\nu = n - 1$. As sample size ($n$) grows, the t-distribution converges to become identical to the standard normal distribution.

#### CI for Proportions
Used when estimating the population proportion ($p$) for categorical data (e.g., click-through rates).
*   **Formula:** $\hat{p} \pm Z_{1-\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$

---

### Hypothesis Testing
A formal framework for evaluating competing claims about a population parameter based on sample data.
*   **Null Hypothesis ($H_0$):** The baseline or status quo assumption (e.g., "The coin is fair, $p = 0.5$").
*   **Alternative Hypothesis ($H_1$):** The claim you are trying to prove (e.g., "The coin is biased, $p \neq 0.5$").

#### Types of Errors
Because we make decisions based on samples, we never know the absolute ground truth and risk making errors.
*   **Type I Error (False Positive):** Rejecting $H_0$ when $H_0$ is actually true. (e.g., Classifying a legitimate email as spam).
*   **Type II Error (False Negative):** Failing to reject $H_0$ when $H_1$ is actually true. (e.g., Letting a spam email into the inbox).
*   **Significance Level ($\alpha$):** The maximum probability of committing a Type I error we are willing to tolerate (usually set at 0.05). It acts as our decision threshold. Lowering $\alpha$ decreases Type I errors but inevitably increases Type II errors.

#### The P-Value and Critical Values
*   **P-Value:** Assuming the null hypothesis is true, the p-value is the probability of obtaining a test statistic at least as extreme as the one observed. It measures how compatible your data is with $H_0$.
    *   **Decision Rule:** If p-value < $\alpha$, reject $H_0$. If p-value $\ge \alpha$, fail to reject $H_0$.
    *   *Misconception:* A p-value is not the probability that $H_0$ is true. It is the probability of seeing your sample data *if* $H_0$ were true.
*   **Critical Value ($K_\alpha$):** The boundary value on the test statistic's distribution that corresponds exactly to the significance level $\alpha$. You define this boundary *before* collecting data. If your observed test statistic falls past the critical value (into the rejection region), you reject $H_0$.

#### Power of a Test
The Power of a test is the probability of correctly making the right call—rejecting a false null hypothesis. 
*   **Formula:** $\text{Power} = 1 - \beta$, where $\beta$ is the probability of a Type II error.
*   Power increases as the true parameter moves further away from the null hypothesis, as the sample size increases, or as you tolerate a higher $\alpha$.

---

### Two-Sample Testing & A/B Testing
A/B testing is an experimental methodology used extensively in Data Science to compare two variations (e.g., a baseline website vs. a new design) by randomly assigning subjects and evaluating the outcome metrics. 

#### Two-Sample Test for Proportions
Used to compare the conversion rates or proportions between two independent groups.
*   **Hypotheses:** $H_0: p_1 = p_2 \quad \text{vs.} \quad H_1: p_1 \neq p_2$ (or directional $>$, $<$)
*   **Pooled Proportion:** If $H_0$ is true, both groups share the same underlying proportion $p$. We estimate this using the pooled sample proportion:
    $$\hat{p} = \frac{x + y}{n_1 + n_2}$$
*   **Test Statistic ($Z$):** 
    $$Z = \frac{\frac{x}{n_1} - \frac{y}{n_2}}{\sqrt{\hat{p}(1-\hat{p})\left(\frac{1}{n_1} + \frac{1}{n_2}\right)}} \sim N(0,1)$$
*   **Conditions:** Populations must be at least 20x the sample size, samples must be independent, and there must be at least 10 successes/failures in both samples.

#### Paired T-Test
Used when the two samples are dependent or paired (e.g., measuring the weight of the *same* 10 people before and after a diet program). 
*   Instead of treating them as two independent distributions, calculate the exact difference ($D_i = X_i - Y_i$) for every pair.
*   The test then reduces to a standard one-sample t-test on the mean of those differences ($\bar{D}$).
*   **Hypotheses:** $H_0: \mu_D = 0 \quad \text{vs.} \quad H_1: \mu_D > 0$
*   **Test Statistic ($T$):** 
    $$T = \frac{\bar{D} - \mu_{D_0}}{S_D / \sqrt{n}}$$
    Where $S_D$ is the sample standard deviation of the differences, and degrees of freedom = $n-1$.
