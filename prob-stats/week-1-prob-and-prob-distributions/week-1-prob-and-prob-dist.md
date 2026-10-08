## Probability and Statistics Fundamentals

### Core Probability Rules
*   **Basic Probability:** Probability is defined as the ratio of a specific event to the total number of outcomes, expressed as $P = \frac{\text{event}}{\text{total}}$[cite: 14]. For example, the probability of selecting a child who plays soccer from a specific sample group is $3/10 = 0.3$[cite: 18, 20].
*   **Complement Rule:** The probability of an event *not* occurring is $1 - P(\text{event})$[cite: 119, 122]. If the probability of playing soccer is 0.3, the probability of not playing is $1 - 0.3 = 0.7$[cite: 111, 112, 120].
*   **Sum of Probabilities (Disjoint Events):** For mutually exclusive events, $P(A \text{ or } B) = P(A) + P(B)$[cite: 186, 342]. When rolling two dice, getting a sum of 7 ($6/36$) or 10 ($3/36$) is $9/36 = 1/4$[cite: 227, 228, 229, 230].
*   **Sum of Probabilities (Joint Events):** For overlapping events, subtract the intersection: $P(A \text{ or } B) = P(A) + P(B) - P(A \cdot B)$[cite: 347, 348]. If 60% play soccer, 50% play basketball, and 30% play both, the probability of playing either is $0.6 + 0.5 - 0.3 = 0.8$[cite: 331, 334, 336].
*   **Independent Events & Product Rule:** For independent events, $P(A \cdot B) = P(A) \cdot P(B)$[cite: 434, 600]. Flipping five heads in a row is $(1/2)^5 = 1/32$[cite: 449, 451, 452].
*   **Birthday Problem:** In a room of 23 people, the probability of a shared birthday is approximately 50%, and the probability of no shared birthdays is roughly 0.493[cite: 488, 511].

### Conditional Probability & Bayes Theorem
*   **Conditional Probability:** The probability of an event given another has occurred is $P(A|B) = \frac{P(A \cdot B)}{P(B)}$[cite: 811]. For two coin flips, the probability of getting two heads given the first is heads is $1/2$[cite: 527, 532]. This gives the general product rule: $P(A \cdot B) = P(A) \cdot P(B|A)$[cite: 623].
*   **Bayes Theorem:** Reverses conditional probabilities: $P(A|B) = \frac{P(A) \cdot P(B|A)}{P(B)}$[cite: 812]. The denominator expands to $P(A) \cdot P(B|A) + P(\text{not } A) \cdot P(B|\text{not } A)$[cite: 851]. In a spam filter where $P(\text{spam}) = 0.2$, $P(\text{lottery}|\text{spam}) = 0.7$, and $P(\text{lottery}|\text{ham}) = 0.125$, an email with "lottery" is spam with probability $\approx 0.583$[cite: 906, 907, 914, 915].
*   **Naive Bayes:** Extends Bayes by assuming features are independent given the outcome class[cite: 968, 969]. Adding a second word like "winning" updates the spam probability to $\approx 0.913$[cite: 983, 984].

### Random Variables & Discrete Distributions
*   **Random Variables:** Model uncertain outcomes numerically (e.g., $X$ representing defective items in a shipment)[cite: 1061, 1088]. They can be discrete (countable) or continuous (infinite values on an interval)[cite: 1078, 1079, 1081, 1082].
*   **Probability Mass Function (PMF):** For discrete variables, maps outcomes to probabilities, $P(X=x)$, where $\sum p_X(x) = 1$[cite: 1118, 1124].
*   **Binomial Distribution:** Models successes in $n$ independent trials[cite: 1126, 1128]. Formula: $P(X=x) = \binom{n}{x} p^x (1 - p)^{n-x}$[cite: 1159]. Example: exactly 2 heads in 5 tosses uses the coefficient $\binom{5}{2}$ for the 10 combinations[cite: 1146].

### The Normal Distribution
*   **Probability Density Function (PDF):** For a continuous, normally distributed random variable $X \sim N(\mu, \sigma^2)$, the probability density is given by:
    $$f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2}$$
*   **Intuition Behind the Formula:**
    *   **$\mu$ (Center):** Defines the mean. It shifts the peak of the bell curve left or right along the x-axis.
    *   **$\sigma$ (Spread):** Defines the standard deviation. A larger $\sigma$ stretches the curve wider and flatter; a smaller $\sigma$ makes it narrower and taller.
    *   **$e^{-\frac{1}{2}(\dots)^2}$ (Shape):** The exponential decay function is the engine of the bell shape. Since it squares the distance from the mean $(x-\mu)^2$, it creates perfect symmetry and ensures the probability drops off rapidly as you move away from the center.
    *   **$\frac{1}{\sigma\sqrt{2\pi}}$ (Scaling Down):** This is the normalization constant. Because the total area under any probability distribution curve must equal exactly 1, this fraction scales the exponential function down perfectly to achieve that total area.
*   **Binomial Converging to Normal:** As the number of trials $n$ in a Binomial experiment grows large, calculating exact combinations $\binom{n}{x}$ becomes computationally heavy. Thanks to the De Moivre-Laplace theorem (a special case of the Central Limit Theorem), the discrete Binomial distribution smoothly converges into a continuous Normal curve. 
    *   A $Binomial(n, p)$ distribution is closely approximated by $N(\mu = np, \sigma = \sqrt{np(1-p)})$.
    *   This approximation is generally considered highly accurate when both $np \geq 5$ and $n(1-p) \geq 5$.
