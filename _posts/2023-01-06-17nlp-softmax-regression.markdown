---
title: "17 NLP: Softmax Regression"
tags:
  - NLP
  - Softmax Regression
  - One-Hot vector
  - Multi-Class Classification Problem
  - Cross Entropy Function
  - Artificial Neural Network Diagram
---
💡 This post introduces the Multi-Class Classification Problems, Softmax function, Randomness of One-Hot vectors, Cross Entropy function in Softmax Regression and in Binary Classification, and Artificial Neural Network Diagram
{: .notice--warning}

Previously, we solved `Binary Classification`, which selects one of two options through `logistic regression`. This time, we will learn about `Softmax regression` for `multi-class Classification problems` in which **one of three or more options** is selected.

## 1. Multi-class Classification
The `sigmoid function` used in `logistic regression` earlier outputs a value **between 0 and 1** for the input data so that the value can be interpreted as the `probability` that it belongs to one of the two. For example, if `0` is defined as normal mail and `1` is spam mail, the output value between 0 and 1 of the `sigmoid function` can be interpreted  as the `probability of spam mail`. If the probability value exceeds **0.5**, it is close to `1`, so it can be judges as spam mail, and if the opposite is true, it can be judged as normal mail.
<br>
<br>
If `binary classification` was a problem of **choosing one of two options**, the problem of **choosing one of three or more options** is called `multiclass classification`. The following iris variety prediction data is data for the problem of predicting which of three varieties, setosa, versicolor, and virginica, from sepal length, sepal width, petal length, and petal width, and is data for a `typical multi-class classification problem`.
<br>
![table7](https://user-images.githubusercontent.com/40441643/210967198-63d02987-671d-4346-9ffc-fceff3ee8e99.PNG)
<br>
<br>
How about using the `sigmoid function` you learned earlier here? Maybe you can apply the `sigmoid function` to each answer sheet for the input sample data. If you do that, you will get outputs such as setosa with a probability of `0.8`, versicolor with a probability of `0.2`, virginica with a probability of `0.4`, and so on. But can't we change it to a probability across all options by making the **sum of all these probabilities** equal **1**? For example, given the sample data as input, we want the model to get predictions that add up to **1**, such as setosa with a probability of `0.7`, versicolor with a probability of `0.05`, and virginica with a probability of `0.25`. And in this case, we want to consider it predicted by setosa with the highest probability value. In this case, the `softmax function` can be used.

## 2. Softmax function
When the total number of options to be selected is `k`, the `softmax function` receives a **k-dimensional vector** and estimates the `probability` for each class. First, I will explain the formula, and then I will understand it with pictures.

### 1) Understanding the Softmax function
If the i-th element in a `k-dimensional vector` is `zi` and the `probability` that the i-th class is **correct** is expressed as `pi`, the `softmax function` defines `pi` as follows.
<br>
![pi](https://user-images.githubusercontent.com/40441643/210972407-6fcbd624-6f17-455d-96b5-d91a04a6918a.PNG)
<br>
<br>
Let's apply the `softmax function` step by step to the problem to be solved above. In the case of the problem to be solved above, since `k = 3`, given an input of a `3-dimensional vector z = [z1 z2 z3]`, the `softmax function` returns the following output.
<br>
![softmax](https://user-images.githubusercontent.com/40441643/210975308-82fb475a-1ced-44c1-aace-00b264ddb3d3.png)
<br>
<br>
`p1, p2, p3` each represents the `probability` that `class 1` is **correct**, the `probability` that `class 2` is **correct**, and the `probability` that `class 3` is **correct**, each representing a value **between 0 and 1**, with a total sum of `1`. Since the three classes we want to classify here are `virginica`, `setosa`, and `versicolor`, these manes the values representing the `probability` that the given input is `virginica`, `setosa`, and `versicolor`. Here, let's say that when `i` is `1`, it represents the `probability of virgnica`, when it is `2`, it is `setosa`, and when it is `3`, it is specified as `versicolor`. This order of assignment is a random selection of people to solve the problem. So, if we rewrite the expression to suit the problem, we get:
<br>
![softmax2](https://user-images.githubusercontent.com/40441643/210976511-b242d690-a01f-44dd-a491-8c3240a47d35.PNG)
<br>
<br>
It's a bit complicated, but it's not a difficult concept, when there are `k classes ` to be classified, a `k-dimensional vector` is input, the value of all vector elements is changed to a value **between 0 and 1**, and a `k-dimensional vector` is returned again. Let's explain the concepts we just learned again with pictures.

### 2) Understaing through pictures
![softmax3](https://user-images.githubusercontent.com/40441643/210978405-7f7824e4-8376-4502-a5c5-e134b34a491e.png)
<br>
I explain by gradually adding flesh to the picture above. Here, lets' assume that we **receive and process sample data one by one as input**. That is, the **batch size** is `1`.
<br>
<br>
The picture above raises two questions. The first question is about the `input of softmax function`. One sample data is four independent variables x, which means the model takes a `4-dimensional vector` as input. However, sine the vector used as an `input for the softmax function` must have **the number of classes to be classified**, the vector must be converted into a `3-dimensional vector` through some **weighting operation**. In the figure above, the `3D vector` used as the input of the `softmax function z` is expressed as
<br>
![softmax4](https://user-images.githubusercontent.com/40441643/210979886-3c308ddf-922f-46a3-bb1d-d43985917ca4.PNG)
<br>
<br>
It is straightforward to dimensionally **reduce a vector of sample data** into an input vector for `the softmax function`. Input vector of softmax function `z` proceeds with **weight multiplication** so that the result is as many as the **number of dimensions of z**. In the figure above, there are a total of (4 x 3 = 12) 12 arrows, all of which have different weights, and the values are gradually changed to the weights that **minimises errors** during the learning process.
<br>
<br>
The second question is about **how the error is calculated**. The output of the `Softmac function` is a vector with as many dimensions as the number of classes to be classified, and each element has a **value between 0 and 1**. Each of these represents the `probability that a particular class is correct`. Here, the first element `p1` is the `probability` that **virginica is the correct answer**, the second element `p2` is the probability that `setosa` is the correct answer, the third element `p3` is the probability that `versicolor` is the correct answer. If so, there must be a way to represent the actual value that can be compared with this `predicted value`. `Softmax Regression` expresses real values as **one-hot vectors**.
<br>
![softmax5](https://user-images.githubusercontent.com/40441643/210986492-a474114f-9366-4b8f-b606-70d7034111ec.png)
<br>
<br>
The figure above shows the first element of the output vector of the `Softmax function`. When the first element`p1`is the probability that `virginica` is the correct answer, second element `p2` is the probability that setosa is correct, the third element `p3` is the probability that versicolor is correct, the `integer encoding` of each actual value becomes **1, 2, and 3**, and `one-hot encoding` is performed on this to show that the actual value is `digitised` as a **one-hot vector**.
<br>
![softmax6](https://user-images.githubusercontent.com/40441643/210991942-ade49ef8-e813-48f9-9688-83a125f5300e.png)
<br>
<br>
For example, if the `actual value` of the sample data being solved is `setosa`, the `one-hot vector of setosa` is **[0 1 0]**. In this case, the case where the `error between the predicted value and the actual value` is `0` is when the result of the `softmax function` is **[0 1 0]**. To calculate the `error` of these two vectors, `softmax regression` uses the **cross entropy function** as the `cost function`, which will be discussed later in the section on `cost function`.
<br>
<br>
![softmax7](https://user-images.githubusercontent.com/40441643/210997068-22bfef27-4343-4dd3-8a98-fa54a0dab232.png)
<br>
Just like the linear or logistic regression we learned earlier, we **update the weights from the error**.
<br>
<br>
![softmax8](https://user-images.githubusercontent.com/40441643/210998231-a4a1df7c-bc1d-4bed-b425-1b0320d153ba.png)
<br>
More precisely, like linear or logistic regression, `bias` is also a parameter to be updated. Let's understand `softmax regression` as **vector and matrix operations**. Input the input vector with as many dimensions as the number of features `x` and the eight matrix `W`, the bias `b`, the process of obtaining the `predicted value` in `softmax regression` is expressed as a **vector and matrix operations** as follows.
<br>
![softmax9](https://user-images.githubusercontent.com/40441643/210998942-6add8f11-7e81-4e44-9175-5cfc344b37b2.png)
<br>
where `4` is the **number of attributes** and `3` corresponds to the **number of classes**.

## 3. Randomness of one-hot vectors
It is not always possible to solve a `multi-class Classification problem` only by **expressing the real values as one-hot vector**, but in that most multi-class classification problems have an **equal relationship between classes**, a `one-hot vector` is an appropriate way to express this point. It is a way of expression.
<br>
<br>
Problems that **classify multiple classes** require as many `numeric labels` as there are `classes`, rather than two numerical labels as in `binary classification`. At this time, an intuitive `labelling method` is to **encode integers** for all classes to be classified. For example, if there are three labels to be classified, such as (red, green, blue), label them as `0`, `1`, and `2`, respectively. Or if you have 4 classes to classify and you want the `index` to start at number `1`, you could try `labelling` them as `1, 2, 3, 4` for {baby, child, adolescent, adult}. However, in a general `multi-class classification problem`, it can be said that using **one-hot encoding** rather than `integer encoding` as above expresses the properties of `classes` better as a **labelling method** . Let's find out why.
<br>
<br>
Let's say we have a problem with three `classes Banana, Tomato, and Apple`. The `labels` were given `1, 2, and 3` respectively using **integer encoding**. Using the mean square error `MSE` from the `linear regression` exercise as a `loss function`, you can see how misleading integer encoding can be. The equation below is the same as the `MSE` learned in `linear regression`. `^y` stands for the `predicted value`.
<br>
![loss](https://user-images.githubusercontent.com/40441643/211004008-766568ed-1a9a-41a3-8129-205162098841.PNG)
<br>
<br>
For an intuitive comparison of the `size of the error`, let's exclude the formula for **calculating the average and judge only the squared error**.
<br>
<br>
If the predicted value was `Banana` when the actual value was `Tomato`, the `square error` would be:
<br>
(2-1)^2 = 1
<br>
<br>
If the predicted value was `Banana` when the actual value was `Apple`, the `squared error` would be:
<br>
(3-1)^2 = 4
<br>
<br>
That is, the error between `Banana and Apple` is greater than between `Banana and Tomato`. This is `tantamount` to informing the machine that **Bananas are closer to Tomatoes than Apples**. Let's say we did **integer encoding** for `more classes`.
```
{Banana :1, Tomato :2, Apple :3, Strawberry :4, ... Watermelon :10}
```

This `integer encoding` implies that Banana is closer to Tomato than Watermelon. This is not information that user intended to give. There are, of course, `classification problems` where **ordering information** of these integer encoding is helpful. This is the case where each class has a meaning of `order`, so `classification problems` can be solved through **regression**. For example, {baby, child, adolescent, adult} or {1st, 2nd, 3rd, 4th} or {teens, 20s, 30s, 40s}. However, in a `general classification problem`, each class does not have an `order meaning`, so it is correct that the error between each class is equal. Unlike `integer encoding`,  **one-hot encoding** equally distributes the relationships among all classes of the `classification problem`.
<br>
<br>
The following shows that the `square error` between each class is equal when the **labels are encoded through one-hot encoding for the three categories**.
<br>
![onehot](https://user-images.githubusercontent.com/40441643/211005642-e80171e6-f648-474c-bcc1-b329239b9bda.PNG)
<br>
<br>
In other words, the `one-hot vectors` obtained through `one-hot encoding` for all classes have the **same Euclidean distance** even if the Euclidean distance is obtained for all pairs. The `one-hot vector` can express the **randomness of the representation method of each class**. As will be mentioned again later, the `randomness of the relationship of these one-hot vectors` is sometimes mentioned as a **disadvantage of not being able to obtain word similarity**.

## 4. Cost Function
`Softmax regression` uses the **cross entropy function** as the `cost function`. here, we will understand not only the cross entropy function in `Softmax Regression`, but also various notation methods.

### 1) Cross Entropy Function
Let be `k` the number of class and `y` represents he `actual value` below. `yi` means the actual value of one-hot vector's j-th index and `pj` is the probability that the sample data is j-th class. It is also represented as `^yj`.
<br>
<br>
![cost4](https://user-images.githubusercontent.com/40441643/211007121-424d52f5-17e7-4be3-9983-5924a0acb4a6.PNG)
<br>
Let's see why this function is a good `cost function`. Let `c` be the index of the element with `1` in the real-valued one-hot vector, then pc = 1. This is the case where `^y` correctly predicted `y`. Substituting this into the expression gives `-1 log(1) = 0`, and as a result, the value of the `cross entropy function` becomes `0` when `^y` correctly predicts `y`. That is, the above `cost function` should be trained in the direction of **minimising its value**.
<br>
<br>
If we assume that we find out the average of total data n, the final cost function follows as.
<br>
![cost5](https://user-images.githubusercontent.com/40441643/211008739-da3f865e-8c6a-4c01-b813-11fd5db352b3.PNG)

### 2) Cross Entropy Function in Binary Classification
Although it looks different from the `cross entropy function formula` learned in `logistic regression`, it is essentially the same function formula. Let's derive the `cross entropy function formula` of `softmax regression` from the cross entropy function formula of `logistic regression`.
<br>
![cost6](https://user-images.githubusercontent.com/40441643/211009528-346373ce-4342-4d75-a33a-920a898f799e.PNG)
<br>
<br>
The aove expression shows the `function expression of cross entropy` learned in `logistic regression` earlier. In the above equation, let's replace `y` with `y1`, `1-y` with `y2`, and `H(X)` with `p1`, `(1-H(X))` with `p2`. As a result, the following expression can be obtained.
<br>
![cost7](https://user-images.githubusercontent.com/40441643/211010217-2d5da1d4-cdb3-4de6-a9a3-e4853a417b3a.PNG)
<br>
<br>
This expression can be represented as follows.
<br>
![cost8](https://user-images.githubusercontent.com/40441643/211010360-16860919-f618-482b-9d2d-214bceffc177.PNG)
<br>
<br>
In the `softmax regression`, change `k` to `2` since the value of `k` is not fixed.
<br>
<br>
![cost9](https://user-images.githubusercontent.com/40441643/211010851-504faa34-448b-4fbf-80ae-c76e25c88400.PNG)
<br>
The above expression is consequently equivalent to that of `softmax regression`. Conversely, to obtain the **cross entropy function of logistic regression** in **softmax regression**, set `k` to `2`, replace `y1` and `y2` with `y` and `1-y`, respectively, and replace `p1` and `p2` with `H(X)` and `1-H(X)`, respectively.
<br>
<br>
In summary, assuming that **k is 2** in the `final cost function of the softmax function`, it is eventually equivalent to the `cost function of logistic regression`.
<br>
![finalcost](https://user-images.githubusercontent.com/40441643/211011911-831ede94-3cfe-4f94-a417-7cd9bab0547b.PNG)

## 5. Artificial Neural Network Diagram
`Softmax regression`, which classifies `m classes` with `n features`, is expressed in the form of an **Artificial Neural Network** that will be learned later as follows. `Softmax regression` can also be viewed as an artificial neural network.
<br>
![ann2](https://user-images.githubusercontent.com/40441643/211012501-163618b3-c6c9-4e7d-8aa1-241284d28d17.PNG)
<br>
<br>
In fact, the above figure can be seen as a more summary representation of the figure after generalizing the **number of features to n** and the **number of classes to m** in the figure below to use the `softmax function`.
<br>
![softmax8](https://user-images.githubusercontent.com/40441643/210998231-a4a1df7c-bc1d-4bed-b425-1b0320d153ba.png)
