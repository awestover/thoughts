Recently I consumed the content of the online course
"Introduction to Statistical Learning"
https://www.statlearning.com/ By Hastie, Tibshirani.

This book is mostly about statistical methods, which seem to
fall mostly into the supervised learning category of machine
learning, where you have labelled data. 
This is as opposed to 
- semi-supervised learning: you make your own labeled data
- unsupervised learning: no labels, do clustering

Some of the statistical methods discussed in the course are:

# basic models

- linear regression
    - one way to make this more powerful is "feature
    expansion" e.g., adding X^2 or XY as new variables
    - in a sense logistic regression is also a form of
    linear regression: you're doing linear regression to fit
    the log-likelihood 

- K nearest neighbors
 
# misc stuff

- There was this thing called Linear Discriminant Analysis
    - I think it just amounts to using Baye's rule with a gaussian prior

- Naive Bayes: make some independence assumption

# general ideas

- Bias variance tradeoff is a big deal:
    - This basically means, if you increase model complexity then you can 
    get better training set performance, but this has drawbacks.
    - If you overfit to the training set, then you get better
    training set performance at the cost of test set performance
    (which is what you actually care about)
- One approach to deciding how you're doing on this tradeoff
seems to be validation / cross-validation 
    - In validation you set apart some of the data, train on
    part of the data and then validate on the rest of the
    data.
    - In cross validation you partition your data, and try
    leaving out each part.
- This seems to be the method of choice for deciding how to tune hyper-parameters

- bootstrapping:
    - sample from your data with replacement
    - I think this is useful in getting an idea of the variance of your estimates
    - i.e., computing standard errors and confidence intervals

# tree based methods    

Note that in a lot of the tree based methods there is some
idea of building up a tree and then pruning it later.  The
intuition is that bushy trees are "overfitting", but if we
average many of these then it's fine because the averaging
helps our variance.

- Most obvious one is you just literally make a decision tree
    - This is nice because it has high interpretability

- bagging: form B different bootstrapped versions of the
data, and train a tree decision model on each, and then
average the predictions

- random forest: it's like bagging but in each of the
different tree decision models that you're training you
randomly select a small subset of coordinates that they are
allowed to split on at each step. This helps to de-correlate the trees.
Maybe similar intuition for why dropout works.

- boosting: iteratively fit trees to residuals

- BART: you form a sequence of many trees in parallel by
doing some kind of random walks (perturbing the trees). Then
you average over time and space (minus a short burn-in
period at the beginning).

# SVMs
Basic version is to make a "maximal margin separating
hyperplane".  That is, find the separating hyperplane that
maximizes the minimum distances from both classes to the
hyperplane.

This is a little brittle, because what if there's some noise
so you can't really actually separate.  So, more commonly is
to have a kind of "soft-margin": we have some budget of how
close people are allowed to be to the separating hyperplane.

rmk: using kernels (transforming the data) can be a good idea

# survival analysis
todo

# unsupervised learning
todo
- matrix completion
- PCA
- k-means

# things that are not covered in this course, but could be interesting to learn about later: 
- NLP
- CV
- RL
- causal inference
- time series stuff
- deep learning (encompasses RNNs and CNNs)

Anyways, I thought a good way to review some of these
statistical methods would be to see what they can do on
MNIST.

```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from sklearn.linear_model import LogisticRegression

df = pd.read_csv("mnist_train.csv", header=None)
df_test = pd.read_csv("mnist_test.csv", header=None)

"""
with a multi-class classification problem there are three main methods I know of:

- Do OVO (1v1 matches), then assign to the most commonly assigned class
- Do OVA (1 vs all matches). That is, for each class you try to detect is class X vs is not class X
- Estimate all the probabilities. This usually involves doing a softmax. 

For simplicity, let's start with a two-class problem
"""
fives = df[df[0] == 5].values[:, 1:]
threes = df[df[0] == 3].values[:, 1:]
X = np.concatenate((fives, threes))
Y = np.concatenate((np.zeros(len(fives)), np.ones(len(threes))))

fives_test = df_test[df_test[0] == 5].values[:, 1:]
threes_test = df_test[df_test[0] == 3].values[:, 1:]
X_test = np.concatenate((fives_test, threes_test))
Y_test = np.concatenate((np.zeros(len(fives_test)), np.ones(len(threes_test))))


def sigmoid(z):
    return 1 / (1 + np.exp(-z))


def loss(w, b):
    class1 = -Y @ np.log(sigmoid(X @ w.T + b))
    class0 = -(1 - Y) @ np.log(sigmoid(-(X @ w.T + b)))
    return round(np.average(class1 + class0), 2)


model = LogisticRegression()
# presumably this is doing gradient descent to optimize my loss function
model.fit(X, Y)
# LR_loss = loss(model.coef_, model.intercept_)
train_error = sum(model.predict(X) != Y) / len(Y)
test_error = sum(model.predict(X_test) != Y_test) / len(Y_test)

probs = model.predict_proba(X)[:, 0]
plt.title(
    f"train err: {round(train_error*100, 2)}%; test err: {round(test_error*100,2)}%"
)
plt.xlabel("probability")
plt.ylabel("class")
plt.scatter(probs, Y, marker="x", alpha=0.1)
plt.show()


"""
ok, so next I'd plan to run

- KNN
- some tree based models
- SVM 
- Neural Net

And probably I'd try to solve the multi-class version of the problem.

But that's enough for now. 
"""
```
