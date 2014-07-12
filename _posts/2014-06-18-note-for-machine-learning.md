---
layout: post
title: Note for machine learning course from Stanford on Coursera
---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>

## Supervisor Learning

  Given the "right answer" for each example in the data

### Regression

  Pridict **Real-Valued** output

#### Linear Regression

Find a linear function that best match the historical data trend, and can predict a future data through it.

Following hypethetical function means there are *n* features that will impact the result.

<div>
`h_theta(x) = theta_0 + theta_1 x_1 + theta_2 x_2 + ... + theta_n x_n`
</div>

We can vectorize the hypethetical function.

<div>
`h_theta(x) = theta_0 + theta_1 x_1 + theta_2 x_2 + ... + theta_n x_n = theta^T x`	
</div>

Our purpose is to find the best \`theta\` that can make the function can best match historical data. Than we have following cost function. We want to minimize the cost function.

<div>
`J(theta) = 1/(2m) sum_(i=1)^m ( h_theta ( x^((i)) ) - y^((i)) )^2`
</div>

#### Gradient Descent

One way to minimize the cost function is using gradiant descent algorithm. In batch gradient descent, each iteration performs the  following update **(simultaneously update \`theta_j\` for all j)**. With each step of gradient descent, your parameters \`theta_j\` come closer to the optimal values that will achieve the lowest cost function \`J ( theta )\`.

<div>
`theta_j = theta_j - alpha 1/m sum_(i=1)^m ( h_theta ( x^((i)) ) - y^((i)) ) x_j^((i))`
</div>

\`alpha\` will affect on the efficiency of gradient descent algorithm. Too small will take longer time to achieve the lowest cost function, but too big may cannot converge finally. 


### Classification
    
  Predict **Discrete-Value** output

#### Logistic Regression



## Unsupervisor Learning

### Cluster