---
title: "04 NLP: Automatic Differentiation and Linear Regression Practice"
tags:
  - NLP
  - Deep Learning
  - Linear Regression
  - Automatic Differentiation
  - Keras
---
💡 This post introduces the Automatic Differentiation and Linear Regression Practice.
{: .notice--warning}
Let's implement Linear Regression with `TensorFlow` and `Keras`.
### 1. Automatic Differentiation
```
import tensorflow as tf
```
`tape_gradient()` performs Automatic Differentiation. Randomly, an expression`2w^2 +5` is established and let's differentiate `w`.
```
w = tf.Variable(2.)

def f(w):
  y = w**2
  z = 2*y + 5
  return z
```
If we print the `gradients`, we can see that the `differential value` is stored for `w`.
```
with tf.GradientTape() as tape:
  z = f(w)
gradients = tape.gradient(z, [w])
print(gradients)
```
```
[<tf.Tensor: shape=(), dtype = float32, numpy = 8.0>]
```
Let's implement `Linear Regression` using this `Automatic Differentiation function`.
### 2. Implementation of Linear Regression using Automatic Differentiation
First, we declare the weight values `w` and `b`. Since it is a value to be learned, it is initialised with random values of `4` and `1`.
```
# Declare the variables to be learned
w = tf.Variable(4.0)
b = tf.Variable(1.0)
```
Define `Hypothesis` as functions.
```
@tf.function
def hypothesis(x):
  return w*x + b
```
In the current `hypothesis`, `w` and `b` are `4` and `1`, respectively, so the result when inputting random inputs values is as follows.
```
x_test = [3.5, 5, 5.5, 6]
print(hypothesis(x_test).numpy())
```
```
[15. 21. 23. 25]
```
We define the `mean squared error` as a `loss function` as follows:
```
@tf.function
def mse_loss(y_pred, y):
  return tf.reduce_mean(tf.square(y_pred - y))
```
The data to be used here is data in which `x` and `y` differ by a factor of about 10.
```
x = [1, 2, 3, 4, 5, 6, 7, 8, 9] # study time
y = [11, 22, 33, 44, 53, 66, 77, 87, 95] # grade for each studying time
```
The `optimizer` uses `Gradient Descent` with a `learning rate` of 0.01.
```
optimizer = tf.optimizers.SGD(0.01)
```
We're going to do `Gradient Descent about 300 times`.
```
for i in range(301):
  with tf.GradientTape() as tape:
    # y_pred is the predicted value based on current parameter x
    y_pred = hypothesis(x)

    # Calculate the value of mse
    cost = mse_loss(y_pred, y)

    # Calculate the value of differentiation for cost function
    gradients = tape.gradient(cost, [w,b])

    # Update parameter
    optimizer.apply_gradients(zip(gradients, [w, b]))

    if i % 10 == 0:
      print("epoch : {:3} | w의 값 : {:5.4f} | b의 값 : {:5.4} | cost : {:5.6f}".format(i, w.numpy(), b.numpy(), cost))
```

```
epoch :   0 | The value of w : 8.2133 | The value of b: 1.664 | cost : 1402.555542
...To be continued...
epoch : 280 | The value of w : 10.6221 | The value of b : 1.191 | cost : 1.091434
epoch : 290 | The value of w : 10.6245 | The value of b : 1.176 | cost : 1.088940
epoch : 300 | The value of w : 10.6269 | The value of b : 1.161 | cost : 1.086645
```
As the values of `w` and `b` are **continuously updated**, we can see that the `cost` is **continuously decreased**. Let's check the `predicted value` when the random input is given the `learned values` of `w` and `b`.
```
x_test = [3.5, 5, 5.5, 6]
print(hypothesis(x_test).numpy())
```
```
[38.35479  54.295143 59.608593 64.92204 ]
```
There is no one way to implement a model, In the case of `TensorFlow`, implementing models is a bit easier than this using a high-level API called `Keras`. This time, let's implement a `Linear Regression` model in `Keras`.
### 3. Implementing Linear Regression in Keras
Let's simply implement `Linear Regression` using `Keras`. The basic format for **creating a model** with `keras` is: Create a named model with `Sequential`, and `add` necessary information such as the **dimensions of the input and output vectors** through `add`.
<br>
<br>
Let's see the example code below. The first argument, 1, defines the `dimension` of the `output`. An `argument`, usually expressed as `output_dim`. The second argument, input_dim, defines the `dimension` of the `input`. As in this exercise, if the number of real number `x` and `y` is both 1 when implementing simple `linear regression` to predict `y`.
```
model = Squential()
model.add(keras.layers.Dense(1, input_dim=1))
```
Let x be the time of studying and y be the grade by each studying time. `Activation` determines **what function will be used** and is called `linear` for using `Linear Regression`.
<br>
<br>
If you use `Simple Gradient Descent` for `optimiser`, it is named as `sgd`. The `Learning rate` is set by 0.01 and `mse` is used for `cost function`. Note the the number of training for the entire dataset is 300.
```
import numpy as np
import matplotib.pylot as pylot
from tensorfolw.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras import optimizers

x = [1, 2, 3, 4, 5, 6, 7, 8, 9] # study time
y = [11, 22, 33, 44, 53, 66, 77, 87, 95] # grade for each studying time

model = Sequential()

# The dimension of output y is 1, the dimension of input x (input_dim) is 1.
# Since it is a Linear Regression, Activation is `linear`.
model.add(Dense(1, input_dim=1, activation='linear'))

# sgd means Simple Gradient Descent and Learning rate, lr, is 0.01
sgd = optimizers.SGD(lr=0.01)

# MSE is used for Loss function
model.compile(optimizer = sgd, loss='mse', metrics=['mse'])

# Train given x and y to minimise the error through 300 times.
model.fit(x, y, epochs=300)
```
Let's draw the straight line which minimises the error chosen finally.
```
plt.plot(x, model.predict(x), 'b', x, y, 'k.')
```
<br>
![graph4](https://user-images.githubusercontent.com/40441643/208362411-466bffd8-1fa1-4be8-9bd3-dfbf6de303fe.PNG)
<br>
<br>
In the graph above, each point corresponds to the `actual value` we actually gave, and the straight line has the value of `w` and `b` which  **minimises the error** from the `actual value`. Let's predict the grade when studying 9 hours and 30 minutes through this straight line. `model.predict()` shows the model which is `completed learning` **predicts the value for the input data**.
```
print(model.predict([9.5]))
```
```
[[98.556465]]
```
If the student studies 9 hours and 30 minutes, they will get about 98.5.
