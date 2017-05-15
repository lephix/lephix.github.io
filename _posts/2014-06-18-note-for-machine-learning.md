---
# layout: post
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

### Neural Network

#### Model Representation

\`a_i^((j)) \` = "activation" of unit \`i\` in layer \`j\`.

\`Theta^((j))\` = matrix of weights controlling function mapping from layer \`j\` to layer \`j + 1\`.

\`a_1^((2)) = g(Theta_10^((1)) x_0 + Theta_11^((1)) x_1 + Theta_12^((1)) x_2 + Theta_13^((1)) x_3)\`

\`a_2^((2)) = g(Theta_20^((1)) x_0 + Theta_21^((1)) x_1 + Theta_22^((1)) x_2 + Theta_23^((1)) x_3)\`

\`a_3^((2)) = g(Theta_30^((1)) x_0 + Theta_31^((1)) x_1 + Theta_32^((1)) x_2 + Theta_33^((1)) x_3)\`

\`h_Theta(x) = a_1^((3)) = g( Theta_10^((2)) a_0^((2)) + Theta_11^((2)) a_1^((2)) + Theta_12^((2)) a_2^((2)) + Theta_13^((2)) a_3^((2) ) )\`

If network has \`s_j\` units in layer \` j \`, and \`s _ (j+1)\` units in layer \`j + 1\`, then \`Theta^((j))\` will be of dimension \`s_(j+1) xx (s_j + 1)\`.

#### Forward propagation: Vectorized implementation

According to the last example, we have a \`x = [(x_0),(x_1),(x_2),(x_3)]\`, and \`z^((2)) = [(z_1^((2))),(z_2^((2))),(z_3^((2)))]\`.

\`z^((2)) = Theta^((1)) a^((1))\`

\`a^((2)) = g(z^((2)))\`

Add \`a_0^((2)) = 1\`. then, \`z^((3)) = Theta^((2)) a^((2))\`. and, \`h_Theta(x) = a^((3)) = g(z^((3)))\`.

#### Cost Function

\` J(Theta) = - 1/m [ sum _(i=1)^m sum _(k=1)^K y _k^((i)) log ( h_Theta( x^((i)) ))_k + ( 1 - y_k^((i)) ) log (1 - (h_Theta( x^((i)) ))_k)] + lambda / 2m sum _(l=1)^(L-1) sum _(i=1)^(S _l) sum _(j=1)^(S _(l+1)) (Theta _(ji)^((l))) ^2\`

#### Backward Propagation

Backward propagation algorithm is for minimizing the Cost function of Neural Network.

1. Traning set\`{(x^((1)), y^((1))), ... , (X^((m)),y^((m)))}\`
2. Set \`Delta _ij^((l)) = 0\` (for all l,i,j).
3. For \` i = 1\` to \`m\`
  : Set \`a^((1)) = x^((i))\`
  : Perform forward propagation to compute \`a^((l))\` for \`l = 2,3,...,L\`
  : Using \`y^((i))\`, compute \`delta^((L)) = a^((L)) - y^((i))\`
  : Compute \` delta^((L-1)), delta^((L-2)),...,delta^((2))\`. No \`delta^((1))\`.
  :\`Delta _(ij)^((l)) := Delta _(ij)^((l))+ a_j^((l)) delta _i^((l+1))\`

4. Compute following
  : \`D _(ij)^((l)) := 1/m Delta _(ij)^((l)) + lambda Theta _(ij)^((l))\` if \` j != 0\`
  : \`D _(ij)^((l)) := 1/m Delta _(ij)^((l))\` if \` j = 0\`

Following shows how to compute \`delta\` for each layer.

\` delta_j^((4)) = a_j^((4)) - y_i \`

\` delta^((3)) = (Theta^((3)))^T delta^((4)) .** g' (z^((3)))\`

\` delta^((2)) = (Theta^((2)))^T delta^((3)) .** g' (z^((2)))\`

#### Training a neural network

1. Randomly initialize weights
2. Implement forward propagation to get \`h_Theta(x^((i)))\` for any \`x^((i))\`
3. Implement code to compute cost function \`j(Theta)\`
4. Implement back propagation to compute partial derivatives \`del/(del Theta _(jk)^((l))) J(Theta)\`
  : for i=1:m
    : perform forward propagation and back propagation using example \`(x^((i)), y^((i)))\`
    : (Get activations \`a^((l))\` and delta terms \`delta^((l))\` for \`l=2,...,L\`).
5. Use gradient checking to compare \`del/(del Theta _(jk)^((l))) J(Theta)\` computed using back propagation vs. using numeral estimate of gradient of \`J(Theta)\`. Then disable gradient checking code.
6. Use gradient descent or advanced optimization method with back propagation to try to minimize \`J(Theta)\` as a function of parameters \`Theta\`

### Evaluation Machine Learning

High Bias 
  : Means under-fitting. More training data is not helpful. Add more features or reduce \`lambda\` could fix it.

High Variance
  : Means over-fitting. Training with more data is helpful, and increase \`lambda\` and reduce features could fix it.

#### Evaluating a hypothesis

Typically, we can divide data into 3 parts. Training data (60%), cross validation data (20%) and test data (20%). We use cross validation data to evaluate all the hypothesis, and to choose a best hypothesis. 

#### Error metrics

\`Precise = (Actual True)/(Actual True + Fake True)\`

\`Recall = (Actual True)/(Actual True + Fake False)\`
  : Fake True: The actual is false, prediction is true.
  : Fake False: The actual is true, prediction is false.

#### Trading off Precision and recall

\`0 <= h_theta(x) <= 1 \`, predict 1 if \`h_theta(x)>=0.5\`, and predict 0 if \`h_theta(x)<0.5\`. So 0.5 is the threshold.

If we increase threshold, than we will get a higher precision and a lower recall.

If we decrease threshold, than we will get a higher recall and a lower precision.

\`F_1 text(Score) = 2 (Precision ** Recall)/(Precision + Recall)\`, Higher \`F_1 text(Score)\` indicate better choice of the threshold.

### Support Vector Machine

SVM Hypothesis is like following.

\` min _theta C sum _(i=1)^m [ y^((i)) text(cost) _1 (theta^T x^((i))) + (1 - y^((i))) text(cost) _0 (theta^T x^((i)))] + 1/2 sum _(j=1)^n theta _j^2\`

#### Logistic regression vs. SVMs

n = number of features, m = number of training examples

If n is large , use logistic regression , or SVM without a kernel ("linear kernel").

If n is small, m is intermediate, use SVM with Gaussian kernel.

If n is small, m is large, than add more features and use logistic regression or SVM without a kernel.

Neural network likely to work well for most of these settings, but may be slower to train.

## Unsupervised Learning

### Cluster
