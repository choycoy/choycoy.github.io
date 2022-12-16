---
title: "03 NLP: Linear Regression"
tags:
  - NLP
  - Deep Learning
  - Linear Regression
  - Loss Function
  - Hypothesis
  - Mean Square Error
  - Cost Function
---
💡 This post is about understanding the concepts of hypothesis, loss function, and gradient descent, which are used in Machine Learning, and Linear Regression in order to understand Deep Learning.
{: .notice--warning}
### Linear Regression
The more time you spend studying for the test, the better your score will be. The more you walk per day, the less you weigh. The larger the square footage of the house, the higher the selling price of the house tends to be. If you think about it mathematically, you can say that the value of a certain factor is affected by the value of a certain factor. If you use a little more mathematical expression, you can see that the value of a certain variable is affected by the value of a certain variable. A variable `x` that changes the value of another variable, and let `y` be the dependent variable by a variable `x`.
<br>
<br>
Whereas the value of variable `x` can vary independently, the value of `y` is determined dependently and continuously by the value of `x` so `x` is called the `independent variable` and `y` is called the `dependent variable`, respectively. Linear Regression requires one or more independent variable `x` and `y` to model the `linear relationship`. If **the number of independent variable x is 1**, it is called `simple linear regression`.
## 1) Simple Linear Regression Analysis
![equation](https://user-images.githubusercontent.com/40441643/207516183-874f2a78-91da-490f-95ee-5acb18977ed2.PNG)
<br>
<br>
The formula above shows the formula for `simple linear regression`. In Machine Learning, the value `w` which is multiplied by the independent variable `x` is called the` weight` and a value `b` that is added separately is a `bias`. In the equation of a straight line, `w` is the `slope` and `b` is `y-intercept`, respectively. Without `w` and `b`, it can only express only one equation, which is `y = x`. On a graph, only one straight can be expressed. In other words, the equation can vary according to the value of `w` and `b`.
## 2) Multiple Linear Regression Analysis
![multi](https://user-images.githubusercontent.com/40441643/207519662-11dffa3f-ab9f-40b0-8a43-0952dd666dee.PNG)
<br>
<br>
The sale price of a house is not determined simply by the size of the house, but also by the number of floors, number of rooms, and distance from the subway station. I would like to predict the sale price of a house with these multiple factor. The number of `y` is still `1` but the number of x is `multiple`. This is called `Multiple Linear Regression Analysis`.
### Forming a hypothesis
Let's solve the problem with `Simple Linear Regression`. There is a data which shows that a student obtained the following scores according to the amount of study time.
<br>
<br>
![hours](https://user-images.githubusercontent.com/40441643/207520685-83a19a14-9453-41a4-8785-b4c7565aa35b.PNG)
<br>
<br>
If you plot this on the coordinate plane, it looks like this:
<br>
<br>
![score](https://user-images.githubusercontent.com/40441643/207520975-0028a4d5-2b1f-4d2c-bf5f-00b50be55225.PNG)
<br>
<br>
From known data, inferring the relationship of `x` and `y`, I want to predict the student's grades when he studied for 6 hours, 7 hours, and 8 hours. In Machine Learning, in order to predict the relationship `x` and `y`, an equation is called a `hypothesis`. Under `H(x)`, H stands for `Hypothesis`.
<br>
<br>
![h](https://user-images.githubusercontent.com/40441643/207521912-38e11730-34d0-49ae-99fc-b49d797187c6.PNG)
<br>
<br>
![graph2](https://user-images.githubusercontent.com/40441643/207522075-89b405d0-6557-4b94-a7a0-ba51d9d9533e.PNG)
<br>
<br>
The above picture shows the appearance of a straight line drawn depending on the value of `w` and `b`. In the above hypothesis , `w` is the slope of a straight line and `b` is the y-intercept. After all, Linear Regression from given data refers to drawing a straight line that best represents the relationship of `y` and `x`. Since determining a straight line is the value of `w` and `b`, to find appropriate `w` and `b` is important in Linear Regression eventually.
<br>
<br>
Suppose that we draw the straight line on the coordinate plane which shows the relationship of `y` and `x` using the appropriate value of `w` and `b` found by some method. If you just draw a straight line continuously when x is 6, 7, and 8 days, you can tell the expected score when this student studied 6 hours, 7 hours, and 8 hours because you just need to check the value when x is 6, 7, and 8.
### Cost Function: Mean Squared Error(MSE)
We just mentioned that the `Hypothesis` is to establish the equation using `w` and `b` with the relationship `x` and `y` from given data. And then all we have to do is find the best way to express the `rules` for the problem. Machine Learning is to find an equation for **calculating the error between the actual value and the predicted value obtained from the hypothesis** in order to get `w` and `b` and to find out the `optimal value` of `w` and `b` which **minimises the value of this equation** .
<br>
<br>
In this case, **the expression for the error of the actual value and the predicted value** is called the `objective function`, `cost function` or `loss function`. A function whose objective is to **minimise or maximise the value** of a function is called an `objective function`. And when we try to **minimise a value**, we call it a `cost function` or `loss function`. Although these three are not strictly synonymous, they are used interchangeably.
<br>
<br>
The `cost function` should be an expression optimised for **reducing the error of the predicted value**, not simply expressing the error between the actual value and the predicted value. There are various problems in learning and deep learning that we will learn in the future, and each problem has appropriate cost functions. For `regression` problems, `Mean Squared Error(MSE)` is often used.
<br>
<br>
![mse](https://user-images.githubusercontent.com/40441643/207535432-9120174b-7c39-407b-b786-eeb5cbcb6775.PNG)
<br>
<br>
In the graph above, the straight line is drawn with the arbitrary value of `w` and `b` which has 13 and 1, respectively. Now we need to find the correct straight line by **changing the value** of `w` and `b` slowly from this straight line.
<br>
<br>
Actually, drawing the straight line that **best represents the relationship of x and y** is the same as drawing the straight line that is **positionally closest to all the points** in the figure above. The `error` is the `difference` between the `actual value` of `y` at each `x` given the data and `the value of H(x) predicted` by the straight line above. Hence, `↕` shows the `size` of the `error at each point` in the figure above. We need to find the **size of the total error** to get the value of `w` and `b` while **reducing errors**.
<br>
<br>
The most basic way to measure the `size of an error` is to **add up all errors**. If you create a table by calculating the error from the actual value of each predicted value predicted by the straight line y = 13x + 1, it is as follows.
<br>
<br>
![table](https://user-images.githubusercontent.com/40441643/207779235-174f0bc6-8995-42ce-914a-c2709cbd57d3.PNG)
<br>
<br>
However, if you simply formulaically define `error = actual - predicted value` and then `add all the errors`, there are `negative errors` and `positive errors`, so the `absolute size of the errors` cannot be obtained. So we use the `squared sum of all errors`. In other words, **square the `↕` distances between all points and lines in the figure above and add them all together**. Expressing this as formula, it is:
Note that `n` means the number of data you have.
<br>
<br>
![n](https://user-images.githubusercontent.com/40441643/207780991-d1cc5de9-8c84-4f89-a776-d6b42c2f1105.PNG)
<br>
<br>
In this case, dividing by the number of data `n`, we can find the `average of the sum of squared errors`, which is called the `Mean Squared Error(MSE)`. The formula is shown below.
<br>
<br>
![mse2](https://user-images.githubusercontent.com/40441643/207781570-7efded06-23ec-4b4e-9721-d09536cfc863.PNG)
<br>
<br>
The value of the `mean square error` between the `predicted value` and the `actual value` of `y = 13x + 1` is 52.5. By finding the value of `w` and `b` which **minimise the value of the mse**, we can get the correct straight line. Redefining the `mean squared error` as a `cost function` by `b` and `w`, we get:
<br>
<br>
![cost](https://user-images.githubusercontent.com/40441643/207783499-f42d2ce3-a537-4ea7-808d-2accb225c2a7.PNG)
<br>
<br>
The larger the error from all points. the larger the mean square error, and the smaller the error, the smaller the mean squared error. Hence, this mean square error. In other words, if you find `w` and `b` that **minimises** `Cost(w, b)`, you can **draw a straight line that best represents the relationship** between `x` and `y` as a result.
<br>
<br>
![minimisecost](https://user-images.githubusercontent.com/40441643/207784832-90b6d528-9a52-4f09-938b-4f84a0d11717.PNG)
### Optimiser: Gradient descent
A lot of training in machine learning, deep learning, including linear regression, eventually works to find the `parameters` `w` and `b` that **minimise the cost function**. The algorithm used at this time is called an `optimiser` or `optimiser algorithm`.
<br>
<br>
And the process of finding appropriate `w` and `b` through this `optimiser` is called `training` or `learning` in machine learning. Here, you will learn about the most basic optimiser algorithm, `gradient descent`.
<br>
<br>
To understand `gradient descent`, let's understand the relationship between `cost` and `gradient w`. `w` is called a `weight` in machine learning terminology, but in terms of the equation of the straight line, it means the slope of the straight line. The graph below shows **how the error grows when the slope `w` is too high or too slow**.
<br>
<br>
![slope](https://user-images.githubusercontent.com/40441643/207787071-950f826c-1660-4f85-a835-67f3d1aebe32.PNG)
<br>
<br>
In the figure above, the orange line shows when the slope `w` is 20, and the green line shows when the slope `w` is 1. These are straight lines corresponding to `y = 20x` and `y = x`, respectively. `↕` shows the error between the actual value at each point and the predicted value of the two lines. These are significantly larger error values than the `y = 13x + 1` straight line used for prediction earlier. That is, if the `slope` is **too larger**, `the error` between the actual value and the predicted value is **increased** and if the `slope` is **too small**, `the error` between the actual value and the predicted value is **increased**. In fact, `b` is also the same, but if `b` is **too large** or **too small**, `the error` becomes **large**.
<br>
<br>
For the convenience of explanation, let's assume that gradient descent is performed with hypothesis `H(x)` of `y = wx` using only the weight `w` without bias. The value `cost(w)` of the cost function is abbreviated as `cost`. Accordingly, the graph of the relationship between `w` and `cost` is as follows.
<br>
<br>
![costnw](https://user-images.githubusercontent.com/40441643/207789534-56c7db3b-48e6-4d01-b3a3-b18c9d371ec2.PNG)
<br>
<br>
As the slope `w` increases to infinity, the value of `cost` also increases to infinity. Conversely, even if the slope `w` decreases to infinity, the value of `cost` increases to infinity. In the graph above, the `lowest cost` is at the **bottom of the convex part**. What the machine has to do is to find the `w` that makes the cost the `smallest`, so it needs to find the value of `w` at the **bottom of the convex part**.
<br>
<br>
![costnw2](https://user-images.githubusercontent.com/40441643/207790575-622548d7-17db-4858-a321-297a91271cc3.PNG)
<br>
<br>
After setting a random value `w`, the machine **gradually corrects** the `w` value toward the **convex part at the bottom**. The figure above shows the process of **gradually modifying the value** of `w`. And what makes this possible is `Gradient Descent`. To understand this, you need to understand the high school math course, Differentiation. `Gradient descent` uses the concept of `instantaneous rate of change` at a point, or, in other words, the `gradient at a tangent`, which is the first concept you learn when you learn differentiation.
<br>
<br>
![tangent](https://user-images.githubusercontent.com/40441643/207792651-87b79ba0-93c3-42f4-be15-d51b910342e3.PNG)
<br>
<br>
In the figure above, the green line shows `the slope of the tangent line` on the graph for the four cases where `w` has an arbitrary value. Note that the `slope of the tangent line` gradually **decreases** toward the `convex part at the bottom`. And at the `convex part at the bottom`, the `slope of tangent` eventually becomes `0`. On the graph, the green arrow is the horizontal point.
<br>
<br>
In other words, the point where the `cost` is `minimised` is the point where **the slope of the tangent becomes 0**, and the point where **the derivative becomes 0**. The idea of the `gradient descent` method is to **differentiate the cost function to find the slope of the tangent line at the current w**, **change the value** of `w` in the direction where the `slope of the tangent line` is `low`, **differentiate again**, and **repeat this process** when the `slope of the tangent line` is `0`. It is about repeating the operation of changing the value of `w` towards a place.
<br>
<br>
The `cost function` was as follows.
<br>
<br>
![costwb](https://user-images.githubusercontent.com/40441643/207794786-650b1012-c57a-4aa8-a438-90dc72b5158c.PNG)
<br>
<br>
Now, the expression to update `w` to find `w` that **minimises cost** is: **Repeat this until the slope of the tangent is zero**.
<br>
<br>
![updatew](https://user-images.githubusercontent.com/40441643/207795561-5ef9d00a-660d-4bc0-96c5-1c75688805ba.PNG)
<br>
<br>
The above expression means that the value multiplied by the slope of the tangent at the current `w` and `a` is subtracted from the current to be the new value of `w`. `a` is called the `learning rate`. First, let's look at what it means to **subtract the slope of the tangent at the current w from the current w without thinking about a**.
<br>
<br>
![tangent2](https://user-images.githubusercontent.com/40441643/207799081-d46e6451-c710-4bc1-9298-633f7f7afd99.PNG)
<br>
<br>
The figure above shows when the `slope of the tangent line` is `negative`, `zero`, and `positive`. When the slope of the tangent line is `negative`, the formula can be expressed as:
```
w := w - a (negative)
```
If the `slope` is `negative`, subtracting the negative number is the same as changing it to a positive number and adding it. After all, subtracting a **negative slope increases the value** of `w`, which in turn adjusts the value of `w` in the direction where the slope of the tangent is `0`. If the `slope of the tangent line` is `positive`, the above formula can b expressed as:
```
w := w - a (positive)
```
When the `slope` is `positive`, the value of `w` is **decreased**, which in turn adjusts the value of toward `zero slope`. After all, the formula below adjusts the value of `w` in the direction where the `tangent slope` is `0` when the `tangent slope` is `negative` or `positive`.
<br>
<br>
![w](https://user-images.githubusercontent.com/40441643/207802094-b31091b0-ac3b-4318-ab0d-e2a23bdfd5a3.PNG)
<br>
<br>
Then, what does `a`, which is referred to as the `learning rate`, mean here? The `learning rate a` determines **how much w changes when changing the value**, and takes a value `between 0 and 1`. For example, it could be 0.01. The `learning rate` determines **how far to move in terms of looking at** `w` as a point on the graph and **going down the slope until the slope of the tangent is zero**. Intuitively, it seems that if we arbitrarily increase the value of the `learning rate a`, we can quickly find `w` with the `minimum slope of the tangent line`, but it is not.
<br>
<br>
![graph3](https://user-images.githubusercontent.com/40441643/207803410-7cfca4d2-f883-43d1-9949-0e3ea8f2627c.PNG)
<br>
<br>
The figure above shows a situation in which the value of `w` **diverges** when the `learning rate a` has an excessively `high value`, rather than **finding the slope of the tangent to 0**. Conversely, if the `learning rate a` is too `low`, the `learning rate` **slows down**, so it is also important to find an appropriate value for `a`. So far, we have learned about the principle of `gradient descent` by excluding `b` and focusing only on finding the `optimal w`. In practice, the `gradient descent` method simultaneously performs the `gradient descent` on `w` and `b` and obtains the `optimal values` of `w` and `b`.
<br>
<br>
To summarise, `hypothesis`, `cost functions`, and `optimisers` are generic concepts used in machine learning. Depending on each problem to be solved, the `hypothesis`, `cost function`, and `optimiser` can all be different, and the most suitable `cost function` and `optimiser` for `linear regression` are known. `MSE` and `Gradient Descent` mentioned in this post correspond to each.
