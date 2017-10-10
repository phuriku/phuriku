<span style="color:red">THIS PAGE IS PENDING COMPLETION!</span>
--

Introduction to Regression
==========================

Regression is one of the most important topics of machine learning, for its simplicity and wide usage. For our purposes, the topic can be subdivided into two subtopics: 1) *linear regression* and 2) *logistic regression* (the latter of which could have been more aptly named binary regression). More general regression (*e.g.* polynomial) is also used in practiced, although not covered in this elementary introduction.

Training Data and Test Data
---------------------------
When creating ML models, there are two sets of data: 1) *labeled data*, or data for which we already have a correct classification, and 2) newly arriving data, or *unlabeled data*, for which it is our responsibility to predict a classification. The labeled, pre-classified data from which we generate our model is called the *training dataset*, while the newly arriving data for which our generated model will be used to predict a classification is called the *test dataset*.

What is Regression?
-------------------
Regression is a modeling technique by which a predictive function is derived to classify all future incoming data. Specifically, if we have a training dataset in ℝ<sup>n</sup>, we derive a modeling function *f*: ℝ<sup>n</sup> → ℝ that approximates the training dataset. Test data lacking classification can be plugged into *f* for prediction.

The Loss Function
------------------
When we create a model from the training data, how closely does the *training data* fit the model? It is the responsibility of the *loss function*, or *cost function*, **L**(f), to inform us how much information is lost (on the training set) by using the predictive model *f*.

A loss function often used in ML is the *mean square error (MSE)*. Minimizing the MSE is a very common technique to perform linear regression. If *x<sub>i</sub>* represents the training dataset of *n* points with respective classifications *y<sub>i</sub>*, the mean square error for the model *f* is defined to be (1/*n*) Σ<sub>*n*</sub> [f(*x<sub>i</sub>*) - *y<sub>i</sub>*)]<sup>2</sup>.

*Questions:*
1. A training set is composed of points (*1*,*2*,*3*) with respective labels of (*2*,*3*,*6*). What is MSE(*f*) for *f(x) = 2x*?

Overfitting and Model Performance
---------------------------------
A natural question arises when the subject comes up of training data being used to create a model. How do we measure how well the model performs on incoming (i.e. test) data? Since test data lacks classification, we cannot tell if the model is correctly classifying the test data or not.

A naive first attempt might be to find a continuous function that minimizes the loss function. There are a few problems with this approach:
1. There are many continuous functions that produce zero loss, and it would be impossible to choose between them. For example, if our training set is (*1*,*2*,*3*) with respective labels (*1*,*2*,*3*), we might choose *f(x) = x* as our predictive function since it produces zero loss. But a function that ascends from 1 to 10 and back down to 2 between 1 and 2 is also continuous with zero loss.
2. To fit the data exactly or near-exactly with a continuous function is often quite easy. However, the fitting function is often extremely undulant and unnatural. A model should be simple, natural, and (for our purposes) not computation-intensive -- complex, twisting models rarely fit test data well.

The second point above describes *overfitting*. It minimizes the loss function for the training data, but at the cost of being a poor predictor for test data.

Linear Regression
-----------------
*Linear regression* in ℝ is a type of regression that attempts to model data along the line *y = ax + b* for suitable *a* and *b* in ℝ. In our case of *m* data points in ℝ<sup>n</sup>, *a* is no longer in ℝ but is an (*m* x *n*)-dimensional matrix **A**, and *y* is an (*m* x 1) column matrix.

To simplify things, let us ignore the intercept *b* for a moment. In this case, we have the equation
<p align="center">**A** **x** = **y**.</p>

Before attempting to solve for **x** (noting above that **A** and **y** are provided by the training dataset), let's take a step back and ask what **x** actually is. **x** is an (*n* x 1)-dimensional column matrix. Remember that we are working in ℝ<sup>n</sup>, implying that our dataset is *n*-dimensional. Since the data point matrix **A** is multiplied by **x** to get the label **y**, **x** must be the *weight* applied to each dimension of the data point in order to apply the correct label.

In other words, the first data point (*a<sub>11</sub>, a<sub>12</sub>, ..., a<sub>1n</sub>*) corresponds to the label *y<sub>1</sub>*. By the equation above, Σ<sub>k</sub>a<sub>1k</sub>x<sub>k</sub> = y<sub>1</sub>. The matrix equation above produces *m* of these equations, one for each data point. The *x<sub>k</sub>*'s must therefore be the weight applied to each dimension of the data point (and then summed) in order to get the desired label. 

In our situation where we have

There exist many varieties

Although we won't go into detail here


The Iris Dataset
----------------

An historically famous dataset constructed in the 30s, the *Iris Dataset* is a small set of data points (150 entries) of species of the Iris plant, together with measurements of individual plants' sepal and petal length/widths. We can express the entries of the dataset using the following Scala case class:

```
// An enumeration-like Scala-specific construct.
trait Species
case object Setosa      extends Species
case object Versicolor  extends Species
case object Virginica   extends Species

case class Iris(
  sepalLength:  Float,
  sepalWidth:   Float,
  petalLength:  Float,
  petalWidth:   Float,
  species:      Species)
  ```