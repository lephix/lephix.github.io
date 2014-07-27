---
layout: post
title: Note for machine learning course from Stanford on Coursera
---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>

## Supervised Learning

  Given the "right answer" for each example in the data

### Regression

  Predict **Real-Valued** output

#### Linear Regression

Find a linear function that best match the historical data trend, and can predict a future data through it.

Following hypothetical function means there are *n* features that will impact the result.

\`h_theta(x) = theta_0 + theta_1 x_1 + theta_2 x_2 + ... + theta_n x_n\`

We can vectorize the hypothetical function.

\`h_theta(x) = theta_0 + theta_1 x_1 + theta_2 x_2 + ... + theta_n x_n = theta^T x\`	

Our purpose is to find the best \`theta\` that can make the function can best match historical data. Than we have following cost function. We want to minimize the cost function.

\`J(theta) = 1/(2m) sum_(i=1)^m ( h_theta ( x^((i)) ) - y^((i)) )^2\`

#### Gradient Descent

One way to minimize the cost function is using gradient descent algorithm. In batch gradient descent, each iteration performs the  following update **(simultaneously update \`theta_j\` for all j)**. With each step of gradient descent, your parameters \`theta_j\` come closer to the optimal values that will achieve the lowest cost function \`J ( theta )\`.

\`theta_j = theta_j - alpha 1/m sum_(i=1)^m ( h_theta ( x^((i)) ) - y^((i)) ) x_j^((i))\`

\`alpha\` will affect on the efficiency of gradient descent algorithm. Too small will take longer time to achieve the lowest cost function, but too big may cannot converge finally. 

#### Normal Equation

Normal Equation is another way to compute \`theta\` much more efficient. Following is the formula:

\`theta = (X^T X)^(-1) X^T y\`

#### Gradient Descent VS Normal Equation

Gradient Descent:

  + Need to choose \`alpha\`.
  + Needs many iterations.
  + Works well even when has a large number of features.

Normal Equation:

  + No need to choose \`alpha\`.
  + Don't need to iterate.
  + Need to compute \`(X^T X)^(-1)\`.
  + Slow if has a large number of features.


### Classification
    
Predict **Discrete-Value** output

#### Logistic Regression

Logistic regression hypothesis is defined as: \`h_theta(x) = g(theta^T x)\`, and want \` 0 <= h_theta(x) <= 1\`, so we have function \`g\` a sigmoid function. the sigmoid function is defined as: \`g(z) = 1 / (1 + e^(-z))\`.

Then \`h_theta(x)\` looks like this: \` h_theta(x) = 1 / (1 + e^(-theta^T x))\`.

The output of \`h_theta(x)\` means the probability that \`y = 1\` on input \`x\`. For example, \`h_theta(x) = 0.7\` means this \`x\` has 70% chance to be classified to \`y = 1\`, has 30% chance to be classified to \`y = 0\`.

Defined cost function as the same as Linear Regression with a slight different way: \`J(theta) = 1/m sum_(i=1)^m 1/2 (h_theta(x^(i)) - y^(i))^2\`. 

Defined \`Cost(h_theta(x), y) = 1/2 (h_theta(x) -y)^2\`. Because \`h_theta(x) = 1 / (1 + e^(-theta^T x))\` will cause the **non-convex** problem, so we change it like this: \`Cost(h_theta(x), y) = {(-log(h_theta(x)) if y = 1) , (-log(1 - h_theta(x)) if y = 0):}\`.

Finally, we have our cost function like this: 

\` J(theta) = 1/m sum_(i=1)^m Cost(h_theta(x^((i))), y^((i))) \`

We can translate it to this:

\`J(theta) = - 1/m [ sum_(i=1)^m y^((i)) log h_theta(x^((i))) + (1-y^((i))) log(1-h_theta(x^((i)))) ] \`

#### Gradient Descent

Want to \`min_theta J(theta)\`, then repeat \`{ theta_j := theta_j - alpha d/(d theta_j) J(theta) = theta_j - alpha sum_(i=1)^m ( h_theta(x^((i)) - y^((i)) ))x_j^((i))}\`.

Although this is looks the same as linear regression, but \`h_theta(x) = 1/(1+e^(-theta^T x))\` is not the same as linear regression.

#### One vs All

Train a logistic regression classifier \`h_theta^((i)) (x)\` for each class \`i\` to predict the probability that \`y = i\`.

On a new input \`x\`, to make a prediction, pick the class \`i\` that maximizes \`max_i h_theta^((i)) (x)\`.

#### Regularization

First we will talk about overfitting, after that we will have our regularization cost function.

#### Overfitting

If we have too may features, the learned hypothesis may fit the training set very well(\`J(theta) = 1/(2m) sum_(i=1)^m ( h_theta ( x^((i)) ) - y^((i)) )^2\`), but fail to generalize to new examples (predict prices on new examples.)

Fixing overfitting:

1.Reduce number of features

+ Manually select which features to keep
+ Model selection algorithm

2.Regularization

+ keep all the features, but reduce magnitude/values of parameters \`theta_j\`.
+ Works well when we have a lot of features, each of which contributes a bit to predicting \`y\`.

#### Regularization

Add a regularization parameter at the end of cost function like following, will reduce the \`theta\`'s affection.
<p>
`J(theta) = 1/(2 m) [ sum_(i=1)^m ( h_theta(x^((i))) - y^((i)) )^2 + lambda sum_(j=1)^n theta_j^2 ]`
</p>

Notice: By convention, we don't include \`theta_0\` in the regularization parameter.

If \`lambda\` is too small, then it will be no affection for the regularization parameter, but if it too big, will cause underfitting.

### Neuron Network

#### Model Representation

\`a_i^((j)) \` = "activation" of unit \`i\` in layer \`j\`.

\`Theta^((j))\` = matrix of weights controlling function mapping from layer \`j\` to layer \`j + 1\`.

\`a_1^((2)) = g(Theta_10^((1)) x_0 + Theta_11^((1)) x_1 + Theta_12^((1)) x_2 + Theta_13^((1)) x_3)\`

\`a_2^((2)) = g(Theta_20^((1)) x_0 + Theta_21^((1)) x_1 + Theta_22^((1)) x_2 + Theta_23^((1)) x_3)\`

\`a_3^((2)) = g(Theta_30^((1)) x_0 + Theta_31^((1)) x_1 + Theta_32^((1)) x_2 + Theta_33^((1)) x_3)\`

\`h_Theta(x) = a_1^((3)) = g( Theta_10^((2)) a_0^((2)) + Theta_11^((2)) a_1^((2)) + Theta_12^((2)) a_2^((2)) + Theta_13^((2)) a_3^((2) ) )\`

If network has \`s_j\` units in layer \` j \`, and \`s_(j+1)\` units in layer \`j + 1\`, then \`Theta^((j))\` will be of dimension \`s_(j+1) xx (s_j + 1)\`.

#### Forward propagation: Vectorized implementation

According to the last example, we have a \`x = [(x_0),(x_1),(x_2),(x_3)]\`, and \`z^((2)) = [(z_1^((2))),(z_2^((2))),(z_3^((2)))]\`.

\`z^((2)) = Theta^((1)) a^((1))\`

\`a^((2)) = g(z^((2)))\`

Add \`a_0^((2)) = 1\`. then, \`z^((3)) = Theta^((2)) a^((2))\`. and, \`h_Theta(x) = a^((3)) = g(z^((3)))\`.

## Unsupervised Learning

### Cluster