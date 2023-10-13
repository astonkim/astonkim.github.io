---
layout: post
title: 데이터마이닝 Study Note - 2
subtitle: 02. Linear Regression
author: astonkim
categories: DGU_Bachelor_of_Data_Science
banner:
  image: /assets/images/20230926.jpg
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-weight: bold; text-shadow: 5px 6px 5px black;"
  subheading_style: "font-weight: bold; color: gold"
tags: [data_mining]
top: 1
sidebar: []
use_math: true
---

## Linear Regression

- A linear regression model assumes that the regression function $E(Y \vert X)$ is linear in the inputs $X_1, \dots ,X_p$

- Linear models were largely developed in the pre-computer age of statistics, but even in today’s computer era there are still good reasons to study and use them

## Linear Regression Models and Least Squares
### Basic Settings

- We have a input vector $X^T = (X_1, X_2, \dots , X_p)$ and want to predic a real-valued output $Y$

- The linear regression model has the form
$$
f(X) = \beta_0 + \sum_{j=1}^p X_j \beta_j
$$

- The linear model either assumes that the regression function $E(Y \vert X)$ is linear, or that the linear model is a reasonable approximation.

- Here the $\beta_j$'s are unknown parameters or coefficients, and the vatiables $X_j$ can come from different sources:

	- quantitative inputs

	- transformations of quantitative inputs, such as log, square-root or square

	- basic expansions, such as $X_2 = X^2, X_3 = X^3$, leading to a polynomial representation

	- numeric or "dummy" coding of the levels of qualitative inputs

	- interactions between variables, for example, $X_3 = X_1 \cdot X_2$

- No matter the source of the $X_j$, the model is linear in the parameters

- Typically we have a set of training data $(x_1,y_1) \dots (x_N, y_N)$ from which to estimate the parameters $\beta$

- Each $x_i = (x_{i1}, x_{i2}m, \dots ,x_{ip})^T$ is a vector of feature measurements for the $i$-th case

- $\beta_0, \beta_1, \dots ,\beta_p$ : model parameters

### Probabilistic Model

- Assume that $Y_i \sim P(Y\vert X = x_i)$ and $E(Y\vert X = x_i) = f(x_i) = \beta_0 + \sum^p_{j=1}x_{ij}\beta_j$

- $Y_i = \beta_0 + \sum^p_{j=1}x_{ij}\beta_j + \epsilon_i$ where i.i.d. $\epsilon_i$ with $E(\epsilon_i) = \sigma^2$ for $i = 1, 2, \dots, N$

- i.e., $\epsilon_i \sim ^{i.i.d.} N(0, \sigma^2)$

### LEast Square

- The most popular estimation method is **least squares**, in which we pick the coefficients $\beta = (\beta_0, \beta_1, \dots , \beta_p)^T$ to minimize the **residual sum of squares (RSS)**

$$
RSS(\beta) = \sum^N_{i=1}(y_i - f(x_i))^2 = \sum^N_{i=1}(y_i - \beta_0 - \sum^p_{j=1}x_{ij}\beta_j)^2
$$

### Probabilistic view

- Least square is equivalent to the maximum likelihood estiatation if we assmue Normal distribution for the errors!

- For the input values are given (fixed), $y_i$ become normally distributed

- $logP(y_i \vert x_i;\beta) =  -\frac{1}{2\sigma^2}(y_i - f(x_i))^2 + const$

### Visualization

- Figure 3.1 illustrates the geometry of least-squares fitting in the $p+1$ dimensional space occupied by the pairs $(X,Y)$

### Optimization

- How do we minimize RSS?

- Denote by **X** the N x (p+1) matrix with each row an input vector (with a 1 in the first position), and similarly let **y** be the N-vector of outputs in the training set. Then we can write the risidual sum of squares as
  $$
  RSS(\beta) = (y-X\beta)^T(y-X\beta)
  $$

- THis is a quadratic function in the $p+1$ parameters. Differentiating with respect to $\beta$ we obtain
  $$
  \frac{\partial RSS}{\partial \beta} = -2X^T(y-X\beta)
  $$
  $$
  \frac{\partial^2 RSS}{\partial \beta \partial \beta^T} = 2X^TX
  $$

- Assuming (for the moment) that X has full column rank, and hence $X^TX$ is positive definite, we set the first derivative to zero 

- $X^T(y-X\beta)=0$

- To obtain the unique solution

- $\hat\beta = (X^TX)^{-1}X^Ty$

- The predicted values at an input vector $x_{te}$ are given by $\hat f (x_{te}) = (1 : x_{te})^T \hat\beta$ ; the fitted values at the training inputs are

- $\hat y = X\hat\beta = X(X^TX)^{-1}X^Ty

## Geometrical representation

- Figure 3.2 shows a different geometrical representation of the least squares estimate, this time in $R^N$

