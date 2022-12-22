---
title: "05 NLP: Logistic Regression "
tags:
  - NLP
  - Logistic Regression
  - Binary Classification
  - Sigmoid Function
  - Cross Entropy
---
💡 This post introduces the Binary Classification, Sigmoid Function and Cross Entropy Function, which are used in Logistic Regression.
{: .notice--warning}

Among the many problems we want to solve in our daily life, there are many problems where you have to chooses the correct answer from `two answers`. For example, you may take a test and wonder whether the test score is `pass` or `fail`, when you receive a certain mail, it is the problem of `classifying` it as `normal mail` or `spam mail`. **The problem of deciding between the two** is called `binary classification`. And a representative algorithm for solving this problem is **Logic Regression**.

### 1. Binary Classification
Under the hypothesis that `linear regression` is described above and the relationship between study time and grades is expressed as an equation of a straight line, we found the straight line that best represents the data by finding the `weights w` and the `bias b` from given the data. However, it is *not appropriate* to express the `binary classification` problem in which the **correct answer is chosen from one of the two options** to be learn this time as a `straight line`.
<br>
<br>
Let's assume that we have data in which students `pass` or `fail` according to their test scores. And let x be the score and y be the result. By creating a model that determines `pass` or `fail` when a certain score is obtained from this data.
<br>
<br>
 ![table2](https://user-images.githubusercontent.com/40441643/208659315-592d354b-90bc-4420-9f13-86ec2f502955.PNG)
<br>
<br>
In the above data, when `pass` is `1` and `fail` is `0`, the graph is drawn as follows.
<br>
<br>
![graph5](https://user-images.githubusercontent.com/40441643/208660331-5a7121ca-d98b-4825-a571-e2fa7e4ad6d9.PNG)
<br>
<br>
For this example, the actual value, in other words, the value of `y` only has either `0` or `1`, the predicted value should have the value between `0` and `1` to solve this problem. Interpreting values between `0` and `1` as probabilities makes the problem much easier to solve. If the final predicted value is **less than 0.5**, it is judged that it was predicted as `0`, and if is **greater than 0.5**, it is judged to be predicted as `1`. If we use a straight line of `y = wx +b`, then the values of `y` can also have large numbers, such as **negative infinity to positive infinity**, which is the second reason that **straight lines are not suitable for classification problems**.
<br>
<br>
A `Sigmoid Function` is a function whose output is drawn in an **S shape** with a value between `0` and `1`.
### 2. Sigmoid Function
The Sigmoid function is often abbreviated to `σ`. Let's formulate a `hypothesis` to solve `Logistic Regression`.
<br>
<br>
![sigmoid](https://user-images.githubusercontent.com/40441643/208821182-50244e8a-0f9c-41f7-b72a-74983112877c.PNG)
<br>
<br>
Here, e(e = 2.7818281...) is a number called a natural constant. What we still need to find here is the `weight w` and the `bias b` that best fit the given data. What Artificial Intelligence Algorithms do in the end is to **find weights w and b that are appropriate for the given data**.
<br>
<br>
Let's visualise the `sigmoid function` as a graph.
```
import numpy as np
import matplotlib.pylot as pylot
```

Let's visualise the `sigmoid function` as a graph. The graph below assumes that `w` is `1` and `b` is `0`.
```
def sigmoid(x):
    return 1/(1+np.exp(-x))

x = np.arange(-5.0, 5.0, 0.1)
y = sigmoid(x)

plt.plot(x, y, 'g')
plt.plot([0,0],[1.0,0.0], ':')
plt.title('Sigmoid Function')
plt.show()
```

![sigmoidgraph](https://user-images.githubusercontent.com/40441643/208823218-b65537a6-f82e-4c64-99da-64bce5c76bfa.PNG)
<br>
<br>
In the graph above, the `sigmoid function` returns an output scaled to a value between `0` and `1`. It reminds me of the **shape of an S**. When `x` is `0`, the output value is `0.5`. As **x increases**, it **converges to 1**. Let's visualise **how weight w and bias b affect the output**. First, let's change the value of `w` and check the graph accordingly.
 ```
 def sigmoid(x):
    return 1/(1+np.exp(-x))

x = np.arange(-5.0, 5.0, 0.1)
y1 = sigmoid(0.5*x)
y2 = sigmoid(x)
y3 = sigmoid(2*x)

plt.plot(x, y1, 'r', linestyle='--') # when w is 0.5
plt.plot(x, y2, 'g') # when w is 1
plt.plot(x, y3, 'b', linestyle='--') # when w is 2
plt.plot([0,0],[1.0,0.0], ':')
plt.title('Sigmoid Function')
plt.show()
```

![sigmoidfunction](https://user-images.githubusercontent.com/40441643/208824898-37cef50b-717a-4ea4-bf06-b8cdebb7fec4.PNG)
<br>
<br>
The graph above shows a red line when the value of `w` is `0.5`, a green line when the value of `w` is `1`, and a blue line when the value of `w` is `2`. Depending on the value of `w`, the `slope` of the graph **changes**. When expressing a straight line in `Linear Regression`, the `weight w` meant the slope of the straight line, but here it determines the `slope of the graph`. **Larger values of w** result in **larger slopes**, and **smaller values of w** result in **smaller slopes**.
<br>
<br>
Let's see the **change of graph by the value of b**.
```
def sigmoid(x):
    return 1/(1+np.exp(-x))

x = np.arange(-5.0, 5.0, 0.1)
y1 = sigmoid(x+0.5)
y2 = sigmoid(x+1)
y3 = sigmoid(x+1.5)

plt.plot(x, y1, 'r', linestyle='--') # x + 0.5
plt.plot(x, y2, 'g') # x + 1
plt.plot(x, y3, 'b', linestyle='--') # x + 1.5
plt.plot([0,0],[1.0,0.0], ':') #
plt.title('Sigmoid Function')
plt.show()
```

![sigmoidfunctionb](https://user-images.githubusercontent.com/40441643/208826410-ef47b94e-0fdb-485b-a5ea-76f994dd7de3.PNG)
<br>
<br>
The graph above shows that the graph moves according to the value of `b`. The `sigmoid function` **converges to 1** when the **input value increases**, and **converges to 0** when the **input value decreases**. It has a value from `0` to `1`. If the output value is `0.5` or `more`, `1(True)`, and if is `less than 0.5`, `0(False)`, you can use it to **solve the binary classification problem**.
### 3. Cost Function
Using `Logistic Regression` or `Gradient Descent`, we can find the value of `weight w` but `mse` is not used for the `Cost Function`. When using `mean squared error` as the `cost function` for `Logistic Regression`, it is difficult to troubleshoot because it is too likely to fall into an **unfavourable local minimum**.
<br>
<br>
![localmin](https://user-images.githubusercontent.com/40441643/208829286-3b96dc97-c80d-4e21-b792-45d7f285ecdb.png)
<br>
<br>
If `Logistic Regression` uses the `Mean Square Error` as the **cost function**, it's very likely that you'll end up with a **false minimum** rather than the `minimum` you're looking for when using **gradient descent**. This is said to have reached the `local minimum`, the **minimum value in a specific area**, rather than the `global minimum`, the **minimum value throughout the entire function**. A `Cost Function` that falls too easily into a `local minimum` is a poor choice for the purpose of **finding weights w that have the smallest possible cost**. And `Mean Square Error` in `Logistic Regression` is just that poor choice.
<br>
<br>
In a problem called `Logistic Regression`, we need to find an appropriate the `new cost function` that **minimises the weights w**. Let the `objective function` be any function below that **minimises the weights**. **J** stands for the **objective function**.
<br>
<br>
![objfunction](https://user-images.githubusercontent.com/40441643/208831292-fd1dd494-a390-4d1e-b7f2-8c3eb39973a2.PNG)
<br>
<br>
It is not a complete formula. In the above equation, when the number of sample data is `n` and a function `f` is a function representing the `error` between the `actual value yi` and the `predicted value H(xi)`, an appropriate `objective function` that **minimises the weight** depends on how the new function `f` is defined and after that, it is complete. The `objective function` is calculating **the average of the values of a function f** over all data. In order to find the **appropriate weights**, as a result, the `error` between the `actual value` and the `predicted value` **must be reduced**, so here `f` is called the `Cost Function`. If we rewrite the expression, we get:
<br>
<br>
![cost2](https://user-images.githubusercontent.com/40441643/209077688-2d7298fe-8229-4b66-8097-107db81bc4fd.PNG)
<br>
<br>
The `Sigmoid Function` returns **a value of H(x) between 0 and 1**. This means that **the error increases as the value of H(x) approaches 1** when the `actual value` is `0`, and the **error increases as the value of H(x) approaches 0** when the `actual value` is `1`. And a function that can reflect this can be expressed through a `Logarithmic Function`.
<br>
<br>
![log](https://user-images.githubusercontent.com/40441643/209078858-b334761c-0f41-4b7f-b7a9-c1f4373b3e1a.PNG)
<br>
<br>
When the `actual value y` is `1`, `-log(H(x))` is used and when the `actual value y` is 0, `-log(1-H(x))` is used. The above two expressions can be expressed graphically as follows.
<br>
<br>
![cross](https://user-images.githubusercontent.com/40441643/209080072-a5f3492c-c5fe-486f-ae2c-fa27e540db2a.PNG)
<br>
<br>
The graph when the `actual value y` is `1` is represented by a blue line, and the graph when the `actual value y` is `0` is represented by a red line. To briefly explain the graph above, when the `actual value y` is `1` and the `value of H(x)`, the `predicted value`, is `1`, the `error` is `0`, so of course, the `cost` is `0`.
On the other hand, when the `actual value y` is `1` and converges to `0`, the `cost`**diverges to infinity**. If the `actual value` is `0`, you can understand it in reverse. This can be expressed in one expression as:
<br>
<br>
![cost3](https://user-images.githubusercontent.com/40441643/209081503-34939c2c-2032-46f3-886c-c80aea695565.PNG)
<br>
<br>
If you look it closely, you can see that `y` and `(1-y)` are in the middle of the expression, and the existing two expressions are included except for two expression enclosed in `-`. If `y` is `0`, `log(H(x))` is disappeared, and if `y` is `1`, `(1-y)log(1-H(x))` is disappeared, which is the same as the previous expression when `y` is `1` and when `y` is `0`, respectively.
<br>
<br>
As a result, the **objective function** for **Logistic Regression** is:
<br>
<br>
![obj](https://user-images.githubusercontent.com/40441643/209083310-2c117a70-6aab-403f-9fc9-fae62fbd0c90.PNG)
<br>
<br>
At this time, the `cost function` found in `Logistic Regression` is called the **Cross Entropy Function**, In Conclusion, **Logistic Regression** uses the `Cross Entropy Function` as the `Cost Function`, and uses the **average of the cross entropy function** to **find the weights**. The `Cross Entropy Function` is also the **Cost Function of the Softmax Regression**.
