## 1. The Perceptron: The Simplest Neural Network
The simplest form of a Neural Network is the **Perceptron**, which has a "1-0-1" architecture: 1 input layer, 0 hidden layers, and 1 output layer.

A simple linear regression model can be perfectly described as a single perceptron. 
*   **Forward Propagation:** We calculate the prediction $\hat{y}$ using our weights ($w$) and bias ($b$):
    $$\hat{y} = wx + b$$
*   **The Cost Function:** We measure the average squared error across all $m$ training examples. 
    $$\mathcal{L}(w,b) = \frac{1}{2m} \sum_{i=1}^{m} (\hat{y}^{(i)} - y^{(i)})^2$$
    *Note: The division by $2$ is an algebraic trick. When we take the derivative during backpropagation, the exponent $2$ drops down and perfectly cancels it out, leaving a clean equation.*
*   **Backward Propagation:** We calculate the partial derivatives to find the gradient, then update the parameters to minimize the cost:
    $$w_{new} = w - \alpha \frac{\partial \mathcal{L}}{\partial w}$$

### The Universal Neural Network Workflow
Regardless of complexity, building a neural network follows this strict methodology:
1.  **Define Structure:** Number of input units, hidden layers, and output units.
2.  **Initialize Parameters:** Set weights to random small values and biases to zero.
3.  **Training Loop:**
    *   *Forward Propagation:* Calculate the network's predictions.
    *   *Backward Propagation:* Calculate gradients (the required corrections).
    *   *Update Parameters:* Nudge weights and biases using the learning rate.
4.  **Inference:** Make predictions on unseen data.

---

## 2. Multiple Inputs & Matrix Formulation
When we scale up to multiple independent variables (e.g., $x_1, x_2$), we use the exact same perceptron, but with multiple input nodes. 

Instead of writing out long algebraic sums, we organize our data into matrices and use the dot product. If $X$ is a $(2 \times m)$ matrix of inputs and $W$ is a $(1 \times 2)$ vector of weights:
$$Z = WX + b$$
*(Where $b$ is broadcasted across the entire $1 \times m$ output vector).*

**The Elegance of Linear Algebra in Calculus:**
When we calculate the partial derivatives across the entire matrix simultaneously, the math condenses into beautiful, highly efficient matrix multiplication. Where $A$ is our prediction vector and $Y$ is our actual vector:
$$\frac{\partial \mathcal{L}}{\partial W} = \frac{1}{m} (A - Y) X^T$$
$$\frac{\partial \mathcal{L}}{\partial b} = \frac{1}{m} (A - Y) \mathbf{1}$$

---

## 3. Classification & Activation Functions
If we want to predict a category (e.g., Red=1, Blue=0) instead of a continuous number, a standard linear output $Z$ fails. We must apply an **Activation Function** to squash the output into a probability between 0 and 1.

The **Sigmoid Activation Function**:
$$a = \sigma(z) = \frac{1}{1 + e^{-z}}$$
Once passed through the sigmoid, we use a simple threshold (e.g., $> 0.5$) to output a hard class prediction.

### Log Loss (Cross-Entropy)
For classification, the squared error cost function creates a non-convex surface full of local minima. Instead, we use **Log Loss**:
$$\mathcal{L}(W,b) = -\frac{1}{m} \sum_{i=1}^{m} \left[ y^{(i)}\log(a^{(i)}) + (1 - y^{(i)})\log(1 - a^{(i)}) \right]$$

*The Magic Trick:* Because of how the derivative of the logarithm perfectly interacts with the derivative of the sigmoid function, the final partial derivatives for Log Loss are **exactly the same** as the ones for simple linear regression: $\frac{1}{m}(A-Y)X^T$.

---

## 4. Advanced Optimization: Newton's Method
Gradient Descent relies purely on the first derivative (slope). **Newton's Method** is a more advanced optimization algorithm that uses the *second derivative* (curvature) to find the minimum much faster.

### 1D Newton's Method
Instead of a fixed learning rate $\alpha$, Newton's method calculates the optimal step size dynamically:
$$x_{k+1} = x_k - \frac{f'(x_k)}{f''(x_k)}$$

**Python Implementation:**
```python
import numpy as np
import matplotlib.pyplot as plt

# 1. Define the function and its derivatives
def f(x): return np.exp(x) - np.log(x)
def dfdx(x): return np.exp(x) - 1/x
def d2fdx2(x): return np.exp(x) + 1/(x**2)

# 2. Newton's Method Engine
def newtons_method(dfdx, d2fdx2, x, num_iterations=25):
    for i in range(num_iterations):
        # The update rule using both 1st and 2nd derivatives
        x = x - dfdx(x) / d2fdx2(x) 
    return x

x_min = newtons_method(dfdx, d2fdx2, x=1.6)
