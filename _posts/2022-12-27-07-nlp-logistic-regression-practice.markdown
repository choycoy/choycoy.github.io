---
title: "07 NLP: Logistic Regression Practice "
tags:
  - NLP
  - Logistic Regression
  - Keras
---
💡 This post introduces Logistic Regression Practice in Keras.
{: .notice--warning}
Let's implement `Logistic Regression` with `Keras`.

### 1. Logistic Regression implemented in Keras
Let's denote x is the independent variable, y is the `label data` given `1` if x is **greater than or equal to** `10`, and `0` if is it is **less than** `10`.
<br>
<br>
Since this data has a mapping relationship that predicts one real number `y` from one real number `x`, as in the case of simple `linear` `regression`, `1` is specified as the `output_dim` and `input_dim` factor of Dense, respectively. The argument of `activation` is `sigmoid` because the `sigmoid function` will be used.
<br>
<br>
As an optimiser, the most `simple gradient descent` method is used, which is `sgd`. When using the `cross entropy function` as the cost function for a `Binary Classification` problem using the `sigmoid function`, you can specify **binary_crossentropy**. `Epoch` is 200.
```
import numpy as np
import matplotlib.pyplot as plt
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras import optimizers

x = np.array([-50, -40, -30, -20, -10, -5, 0, 5, 10, 20, 30, 40, 50])
y = np.array([0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1])

model = Sequential()
model.add(Dense(1, input_dim=1, activation='sigmoid'))

sgd = optimizers.SGD(lr=0.01)
model.compile(optimizer=sgd, loss='binary_crossentropy', metrics=['binary_accuracy'])

model.fit(x, y, epochs=200)
```

This task is to find `w` and `b` that **minimises the error** for the entire data over a total of 200 times. In the case of me, 100% accuracy started to come out form about 190 times. Let's draw a graph using a model with the values of `w` and `b` modified to **minimise the error** with the actual values.
```
plt.plot(x, model.predict(x), 'b', x,y, 'k.')
```
![graph6](https://user-images.githubusercontent.com/40441643/209624870-4d75bce3-1567-48b1-aed1-395e8bf3de4d.PNG)
<br>
<br>
When the value of x is somewhere between 5 and 10, it looks like it starts going over `0.5`. Since the accuracy was 100%, at least when the value of `x` is `5`, the value of `y` will be **less than 0.5**, and when the value of `x` is `10`, the value of `y` will be **over 0.5**. Now, let's print the value of `y` when the value of `x` is **less than 5** and the value of `x` is **greater than 10**.
```
print(model.predict([1, 2, 3, 4, 4.5]))
print(model.predict([11, 21, 31, 41, 500]))
```

```
[[0.21071826]
 [0.26909265]
 [0.33673897]
 [0.41180944]
 [0.45120454]]
[[0.86910886]
 [0.99398106]
 [0.99975663]
 [0.9999902 ]
 [1.        ]]
 ```

 You can see that it outputs the value of `y` **less than 0.5** when the value of `x` is **less than 5** and the value of `y` **greater than 0.5** when the value of `x` is **greater than 10**.
