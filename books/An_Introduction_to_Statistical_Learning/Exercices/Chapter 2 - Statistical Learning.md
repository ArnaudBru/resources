# Exercices for Chapter 2 - Statistical Learning

## Conceptual

1. For each of parts (a) through (d), indicate whether we would generally expect the performance of a flexible statistical learning method to be better or worse than an inflexible method. Justify your answer.

   a. The sample size n is extremely large, and the number of predictors p is small.\
   Answer: Since n is extremely large, the variance is low, and a flexible method is better because it can capture more complex relationships.

   b. The number of predictors p is extremely large, and the number of observations n is small.\
   Answer: Since n is small, the variance is high, and an inflexible method is better to avoid overfitting.

   c. The relationship between the predictors and the response is highly non-linear.\
   Answer: A flexible method is better because it can adapt to the non-linear relationship.

   d. The variance of the error term is extremely large.\
   Answer: A model with high flexibility will overfit to the noise so inflexible is better.

1. Explain whether each scenario is a classification or regression problem, and indicate whether we are most interested in inference or prediction. Finally, provide n and p.

   a. We collect a set of data on the top 500 firms in the US. For each firm, we record profit, number of employees, industry, and the CEO salary. We are interested in understanding which factors affect CEO salary.\
   Answer: Regression, Inference, n = 500, p = 3.

   b. We are considering launching a new product and wish to know whether it will be a success or a failure. We collect data on 20 similar products that were previously launched. For each product, we have recorded whether it was a success or failure, price charged for the product, marketing budget, competition price, and ten other variables.\
   Answer: Classification, Prediction, n = 20, p = 13.

   c. We are interested in predicting the % change in the USD/Euro exchange rate in relation to the weekly changes in the world stock markets. Hence, we collect weekly data for all of 2012. For each week, we record the % change in the USD/Euro, the % change in the US market, the % change in the British market, and the % change in the German market.\
   Answer: Regression, Prediction and Inference, n = 52, p = 3.

1. We now revisit the bias-variance decomposition.

   a. Provide a sketch of typical (squared) bias, variance, training error, test error, and Bayes (or irreducible) error curves, on a single plot, as we go from less flexible statistical learning methods towards more flexible approaches. The x-axis should represent the amount of flexibility in the method, and the y-axis should represent the values for each curve. There should be five curves. Make sure to label each one.

   b. Explain why each of the five curves has the shape displayed in part a.

   - **Bias**: Bias decreases monotonically as flexibility increases because the model can better capture the true relationship.
   - **Variance**: Variance increases monotonically as flexibility increases because the model becomes more sensitive to noise in the data.
   - **Training Error**: Training error decreases monotonically as flexibility increases because the model fits the training data more closely.
   - **Test Error**: Test error is U-shaped. It decreases initially as the model captures the true relationship but increases later due to overfitting (high variance).
   - **Bayes Error Curve**: This error is constant because it represents the irreducible error, independent of the model's flexibility.

1. You will now think of some real-life applications for statistical learning.

   a. Describe three real-life applications in which classification might be useful. Describe the response, as well as the predictors. Is the goal of each application inference or prediction? Explain your answer.\
   Answer:

   - We want to know if car tyres need to be replaced (Predict)
     Predictors would be brand, size, material, model, age of the tyre, distance driven
   - We want to know which components in cosmetics are allergens (Inference)
     Predictors would be cosmetics composition, did it create an allergic reaction, severity of the reaction
   - We want to know which industrial piece has a defect or not (Prediction)
     Predictors would be density, shape, mechanical properties, dimensions

   b. Describe three real-life applications in which regression might be useful. Describe the response, as well as the predictors. Is the goal of each application inference or prediction? Explain your answer.\
   Answer:

   - Predict the price of plane tickets (Prediction)
     Predictors would be departure, destination, company, class of the ticket, day of the week, date, time
   - Predict the sales of a company (Mixed) to adjust supply & storage
     Predictors would be sales of the previous years, with dates
   - Predict

   c. Describe three real-life applications in which cluster analysis might be useful.\
   Answer:

   - We want to identify a typology of clients in our portfolio
   - We want to identify the common factors of elected officials

1. What are the advantages and disadvantages of a very flexible (versus a less flexible) approach for regression or classification? Under what circumstances might a more flexible approach be preferred to a less flexible approach? When might a less flexible approach be preferred?

   Whether it is for classification or regression,
   A less flexible approach has more bias and less variance, in other words, its output will be more rigid but also less dependent on the sample given as a train set, also it is easier to interpret
   whereas a more flexible approach will be able to describe better the shape of the data but will be more sensitive to the training data and less interpretable

   A more flexible approach might be preferred when our training data is extensive and representative of the data and when interpretability/explainability is not very important
   A less flexible approach is to be preferred when the use case is inference and/or explainability is important

1. Describe the differences between a parametric and a non-parametric statistical learning approach. What are the advantages of a parametric approach to regression or classification (as opposed to a non-parametric approach)? What are its disadvantages?

   Parametric models are faster to train and to interpret, but they might not be able to model complex relationships

1. The table below provides a training data set containing six observations, three predictors, and one qualitative response variable.

   | Obs. | X1 | X2 | X3 | Y |
   |------|------|------|------|-------|
   | 1 | 0 | 3 | 0 | Red |
   | 2 | 2 | 0 | 0 | Red |
   | 3 | 0 | 1 | 3 | Red |
   | 4 | 0 | 1 | 2 | Green |
   | 5 | -1 | 0 | 1 | Green |
   | 6 | 1 | 1 | 1 | Red |

   Suppose we wish to use this data set to make a prediction for Y when X1 = X2 = X3 = 0 using K-nearest neighbors.
   a. Compute the Euclidean distance between each observation and the test point, X1 = X2 = X3 = 0.\
   Distance to X1: 3
   Distance to X2: 2
   Distance to X3: sqrt(10)
   Distance to X4: sqrt(5)
   Distance to X5: sqrt(2)
   Distance to X6: sqrt(3)

   b. What is our prediction with K = 1? Why?\
   With K=1 nearest neighbours = [X5] -> [Green] -> Green

   c. What is our prediction with K = 3? Why?\
   With K=3 nearest neighbours = [X5, X6, X2] -> [Green, Red, Red] -> Red

   d. If the Bayes decision boundary in this problem is highly non-linear, then would we expect the best value for K to be large or small? Why?\
   K = Big implies a decision boundary closer to linearity, If it is highly non-linear, then we would expect K to be small
