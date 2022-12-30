---
title: "09 NLP: Practice on multiple inputs"
tags:
  - NLP
  - Multiple Linear Regression
  - Multiple Logistic Regression
  - Artificial Neural Network Diagram
---
💡 This post introduces Multiple Linear Regression, Multiple Logistic Regression in Keras and Artificial Neural Network Diagram.
{: .notice--warning}

Learn about the case where there are more than two independent variables `x`. How to use `cost function` and `optimiser` is the same as single one.
```
import numpy as np
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras import optimizers
```

### 1. Multiple Linear Regression
Going into the deep learning chapter, most inputs have **two or more independent variables**. From the point of view of directly coding the model, we can say the `input vectors` have a **dimension of 2 or more**. Let's solve a `Linear Regression` with three independent variables in determining `y`. There is data that calculates the final score through a formula for the midterm exam, final exam, and extra points.
<br>
<br>
![score2](https://user-images.githubusercontent.com/40441643/210042947-5f321c86-5d05-4f16-8358-fd2538cf374c.PNG)
<br>
<br>
Let `X` be the vector `[x1,x2,x3]` which has 3 characteristics.
<br>
<br>
Of the samples in the data above, we will use only the top `5` for `training` and the other `2` for `testing`. As the **dimension of the input** changes to `3`,  the **argument** value of `input_dim` changes to `3`. This means that the number of elements in the input vector `X` is `3`, and the `dimension` of the input vector `X` is `3`.

```
X = np.array([[70,85,11], [71,89,18], [50,80,20], [99,20,10], [50,10,10]])
y = np.array([73, 82 ,72, 57, 34])

model = Sequential()
model.add(Dense(1, input_dim=3, activation='linear'))

sgd = optimizers.SGD(lr=0.0001)
model.compile(optimizer=sgd, loss='mse', metrics=['mse'])
model.fit(X, y, epochs=2000)
```

Model training is done. Let the trained model make `predictions` on input `X`.

```
print(model.predict(X))
```

```
[[73.15294 ]
 [81.98001 ]
 [71.93192 ]
 [57.161617]
 [33.669353]]
 ```

 It can be seen that the `predictions` are close to the `actual values`. Let's make a `prediction` with data that we did not use for `training`.
 ```
 X_test = np.array([[20,99,10], [40,50,20]])
print(model.predict(X_test))
```

```
[[58.08134 ]
 [55.734634]]
```

### 2. Multiple Logistic Regression
Let's solve `Logistic Regression` with two independent variables, `x` in determining `y`. If you have sepal length, petal length, and data indicating whether the flower is A or B, you want to create a model that can predict what kind of flower it is from the newly investigated sepal length and petal length. The number of independent variables `x` is `2`.
<br>
<br>
![graph7](https://user-images.githubusercontent.com/40441643/210046106-705fc1c1-a72d-4fa6-b5a6-50b9e1bc05a6.PNG)
<br>
<br>
Let `X` be the vector which has two characteristics.
<br>
<br>
Let's take a new and simpler example with two independent variables and implement it in `Keras`. Let's implement a `logic Regression` in which becomes the output value `y` becomes `1` when the sum of the two inputs `x1 and x2` is **greater than or equal to** `2`, and the output value becomes `0` only when the sum of the two inputs is **less than** `2`.
<br>
<br>
It is almost the same as the `Logistic Regression` code practiced earlier, but the difference is the value of `input_dim` is `2` as the **dimension of the input** changes to `2`. This means that **the dimension is the input vector is 2**.
```
X = np.array([[0, 0], [0, 1], [1, 0], [0, 2] [1, 1],[2, 0]])
y = np.array([0, 0, 0, 1, 1, 1])
model = Sequential()
model.add(Dense(1, input_dim=2, activation='sigmoid'))
model.compile(optimizer='sgd', loss='binary_crossentropy', metrics=['binary_accuracy'])

model.fit(X, y, epochs = 2000)
```

Stopping the training during 2000 epochs, let's print out whether the output is greater than 0.5 or not for each input.

```
print(model.predict(X))
```

```
[[0.23379876]
 [0.48773268]
 [0.4808667 ]
 [0.7481605 ]
 [0.74294543]
 [0.7376603 ]]
```

We can check that the sum of inputs which is greater than or equal to 2 is over 0.5.

### 3. Artificial Neural Network Diagram
Expressing `Multiple Logistic Regression` in the form of an `artificial neural network`: Even though I have not yet learned artificial neural networks, the reason for expressing them as diagrams is to show that it is okay to **interpret Logistic Regression** as a kind of **artificial neural network structure**.
<br>
<br>
![ann](https://user-images.githubusercontent.com/40441643/210047613-863a37be-7d7e-42e6-bb6b-079e9ce64eed.PNG)
