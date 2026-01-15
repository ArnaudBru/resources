# An Introduction to Statistical Learning

## 1 Introduction (done)

## 2 Statistical Learning

### Multiple objectives:

- Prediction:
  - Predict the value of a response variable based on the values of other variables.
  - Example: Predicting house prices based on features like size, location, etc.
- Inference:
  - Understand the relationship between the response variable and the predictors.
  - Example: Understanding how different factors affect house prices.
- Combination of both:
  - Predicting house prices while also understanding the impact of different features.

### Two types of method:

#### Parametric methods:

- Make assumptions about the functional form of the relationship between predictors and response.

- Example: Linear regression assumes a linear relationship (y = β0 + β1x1 + β2x2 + ... + ε).

- Assumptions:

  - The relationship between predictors and response is linear.
  - The errors are normally distributed.
  - Homoscedasticity (constant variance of errors).
  - Independence of errors.

- Advantages:

  - Simplicity and interpretability.
  - Fewer parameters to estimate, leading to less overfitting.
  - Easier to compute and faster to train.

- Disadvantages:

  - May not capture complex relationships.
  - Assumptions may not hold true in practice.
  - Limited flexibility in modeling.

#### Non-parametric methods:

- Do not make strong assumptions about the functional form of the relationship.

- Advantages:

  - Flexibility to capture complex relationships.
  - Can model non-linear relationships without specific assumptions.
  - Can adapt to the data structure.

- Disadvantages:

  - More parameters to estimate, leading to potential overfitting.
  - Computationally intensive and slower to train.
  - Less interpretable than parametric methods.

![flexibility_vs_interpretability.png](images/flexibility_vs_interpretability.png)

#### Measuring the quality of the fit of a model:

Regression:

Mean Squared Error $$MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{f}(x_i))^2$$


Classification:

- Classification error rate $$\frac{1}{n} \sum_{i=1}^{n} I(y_i \neq \hat{y}_i)$$

#### Bias-Variance Tradeoff:

Variance refers to the amount by which our predictions (f chapeau) would change if we estimated it using a different training data set
In general, more flexible statistical methods have higher variance

Bias refers to the error that is introduced by approximating a real-life problem, which may be extremely complicated, by a much simpler model.
Generally, more flexible methods result in less bias.

As a general rule, as we use more flexible methods, the variance will increase and the bias will decrease

As we increase the flexibility of a class of methods, the bias tends to initially decrease faster than the variance increases. Consequently, the expected test MSE declines. However, at some point increasing flexibility has little impact on the bias but starts to significantly increase the variance

#### Classification setting

Bayes Classifier:

- The Bayes classifier is a theoretical model that minimizes the expected classification error. It serves as a benchmark for evaluating other classifiers.

- It is based on the true underlying distribution of the data and is often not feasible to compute in practice.

- The Bayes classifier is not a specific algorithm but rather a concept that represents the best possible performance in terms of classification error.

- The Bayes classifier assigns a new observation to the class with the highest posterior probability max P(Y=j|X=x0)

The Bayes error rate is the minimum possible error that can be achieved by any classifier, given the true distribution of the data.
Bayes error rate formula latex: 
$$\text{Bayes Error Rate} = 1 - E\left( \max_{j} Pr(Y=j|X)\right)$$

## 3 Linear Regression (WIP)

Linear Regression is the basic building block of many regression methods.
It is a simple yet powerful technique used for modeling the relationship between a dependent variable (response) and one or more independent variables (predictors).

### Simple Linear Regression

The relationship we suspect between the response and the predictor:
$$Y \approx \beta_0 + \beta_1 X$$

The model we fit to the data:
$$\hat{y} = \hat{\beta}_0 + \hat{\beta}_1 x$$
Where:
- $Y$ is the response variable
- $X$ is the predictor variable
- $\hat{y}$ is the predicted value of the response variable
- $\hat{\beta}_0$ is the estimated intercept
- $\hat{\beta}_1$ is the estimated slope

Least Squares Estimation:

We want to choose the parameters $\hat{\beta}_0$ and $\hat{\beta}_1$ such that the sum of squared differences between the observed values $y_i$ and the predicted values $\hat{y}_i$ is minimized.

$$\hat{\beta}_0, \hat{\beta}_1 = \arg\min_{\beta_0, \beta_1} \sum_{i=1}^{n} (y_i - (\beta_0 + \beta_1 x_i))^2$$

The minimizers are
$$\hat{\beta}_1 = \frac{Cov(X,Y)}{Var(X)} = \frac{\sum_{i=1}^{n}(x_i -\bar{x})(y_i -\bar{y})}{\sum_{i=1}^{n}(x_i -\bar{x})^{2}}$$
$$\hat{\beta}_0 = \bar{y} - \hat{\beta}_1 \bar{x}$$

#### Accuracy of the coefficients:
We typically assume that the errors are independent of $X$

We compute the standard errors of the coefficients using the formula:

$$SE(\hat{\beta}_0)^{2} = \sigma^{2}\left( \frac{1}{n} + \frac{\bar{x}^{2}}{\sum_{i=1}^{n}(x_i -\bar{x})^{2}} \right)$$
$$SE(\hat{\beta}_1)^{2} = \frac{\sigma^{2}}{\sum_{i=1}^{n}(x_i -\bar{x})^{2}}$$

Where:
$\sigma^{2} = Var(\epsilon)$ is the standard deviation of the errors
For these formulas to be strictly valid, we need to assume that the errors $\epsilon_i$ for each observation have common variance $\sigma^{2}$ and are uncorrelated

We do not need to estimate $\sigma$ to compute both $\hat{\beta_1}$ and $\hat{\beta_2}$.
However, it can be estimated with 
$$ \hat{\sigma}^{2} = \frac{1}{n-2} \sum_{i=1}^{n} (y_i - \hat{y}_i)^{2}$$


The 95% confidence interval for $\hat{\beta}_1$ is given by:
$$\hat{\beta}_1 \pm 2 \cdot SE(\hat{\beta}_1)$$

Similarly, the 95% confidence interval for $\hat{\beta}_0$ is given by:
$$\hat{\beta}_0 \pm 2 \cdot SE(\hat{\beta}_0)$$


## 4 Classification

## 5 Resampling Methods

## 6 Linear Model Selection and Regularization

## 7 Moving Beyond Linearity

## 8 Tree-Based Methods

## 9 Support Vector Machines

## 10 Deep Learning

## 11 Survival Analysis and Censored Data

## 12 Unsupervised Learning

## 13 Multiple Testing

## Topics to look into:

- Degrees of freedom of a model
- Semi-supervised learning
-
