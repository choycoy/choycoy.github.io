---
title: "02 NLP: What is Machine Learning? "
tags:
  - NLP
  - Machine Learning
---
💡 This post is about the concept of machine learning, a concept that includes deep learning, understanding the characteristics of machine learning and familiarising ourselves with key terms in advance.
{: .notice--warning}

### Limitations of Non-Machine Learning Approaches
Let's take an example of a problem that is difficult to solve using traditional programming methods other than machine learning.

```
e.g. Determining whether a given picture is a cat picture or a dog picture.
```
The above problem is actually a problem of DGIST's Deep Learning Contest held in 2017. It's so easy for humans to look at a picture and decide whether it's a cat picture or a dog picture. However, writing a program that can solve this problem is quite difficult. How can I write code that can differentiate between a dog and a cat from an input image?
```
def prediction(image as input):
    How can I code?
    return result
```
<br>
Photographs vary so much depending on the viewing angle, lighting, and target deformation (cat's pose) that it's not easy to pick out a clear common feature from a photograph. In fact, as a conclusion, the program does not have a clear algorithm for sorting numbers in the first place.
<br>
<br>
There have been many attempts to define rules and capture features in the field of image recognition. I tried to algorithmise by finding things like borders in the image, and when other photo image came in, I tried to classify them by comparing their overall condition. However, such attempts eventually had to be limited in capturing the characters. After all, finding an object from a photo these days is not defined by human rules, but `machine learning` is solving the problem.

### Machine Learning method
![mlmethod](https://user-images.githubusercontent.com/40441643/207233566-f635a613-36c4-42f4-9749-adf35ce88cb0.PNG)
<br>
<br>
Machine learning can solve the example problem mentioned above because the approach to solving it is different from traditional programming. In the image above, the top shows a traditional programming approach and the bottom shows a machine learning approach. Machine learning focuses on **finding regularities from data on its own**, given data. The process of `finding regularities` from given data is called `training` or `learning`.
<br>
<br>
Once the regularity is discovered, the correct answer is found based on the regularity found for new incoming data, which is also a solution to problems that were difficult to approach with the existing programming method. I took an image as an example, but natural language processing has many problems just as difficult as image processing. Recently, deep learning, which is a branch of machine learning, has shown great performance in natural language processing.
<br>
<br>
In short, machine translators such as Google Translate are like that, and these translators can achieve much better performance by implementing a deep learning model to find the rules by itself than by defining and creating rules directly.

### Evaluation of machine learning models
![data](https://user-images.githubusercontent.com/40441643/207237492-183a983d-e1b0-4ba6-98f8-8fc8b7f6ce93.PNG)
<br>
<br>
When preparing data for machine learning, it is common to separate that data into three parts before machine learning: for training, for validation, and for testing. Training data is for training a machine learning model. `Test data` is intended to **evaluate the performance** of a trained machine learning model. SO, what is the purpose of the `validation data`?
<br>
<br>
The `validation data` is not intended to evaluate the model's performance, but rather to **tune the model's performance**. More precisely, it is used to determine if the model is `overfitting` the training data or to **adjust hyperparameters**. Let's clear up the terms `hyperparameters` and `parameters`.
<ul>
<li> Hyperparameters:` Human-specified variables `that affect the performance of a model.</li>
<li> Parameters: weights and biases. A `number` whose value continuously `changes` during training.</li>
</ul>
The biggest difference between two variables is that hyperparameters are usually variables that can be directly `set` by the `user`. Representative hyperparameters such as the `learning rate` in gradient descent, which will be learnt in linear regression, and the `the number of neurons` or `layers` in deep learning.
<br>
<br>
On the other hand, parameters such as weights and biases are not values determined by the user, but rather `obtained` during the `training process` of the model. Models that have been trained entirely with training data use `validation` to `verify accuracy` and `tune hyperparameters`. It is to change the value of the hyperparameters to obtain `high accuracy` for the `validation data`. During this `tuning process`, the model is gradually modified to **improve the accuracy of the validation data**.
<br>
<br>
If you want to complete the tuning process and do the final evaluation of the model, it is not appropriate to evaluate the model with validation data. This is because the model has been modified to increase the accuracy of the validation data. `Evaluation` of the model now falls on the `test data`, which is **data the model has never seen**. If you compare it to an examinee preparing for the math ability test, the `training data` is a `question` for actual study, the `verification data` is a `mock test`, and the `test data` is a test for the `final evaluation` of ability.

### Classification and Regression
Many, but not all, problems in machine learning fall under `Classification` or `regression` problems. This chapter deals with `linear regression` and `logistic regression` among machine learning techniques. Linear regression belongs to a representative classification problem (although it is named regression).
<br>
<br>
`Classification` is also divided into `binary classification` and `multi-class classification`. There is another problem, technically `multi-label classification`.

## 1) Binary classification
`Binary Classification` is when you **have to choose one answer out of two options for a given input**. This includes issues of determining pass or fail looking at the score report of the comprehensive exam, and issue of determining whether the e-mail is normal or spam after viewing the e-mail.

## 2) Multi-class Classification
`Multi-class Classification` is when **an answer must be selected from among three or more options for a given input**. For example, let's say a bookstore clerk works and has five bookshelves labelled Science, English, IT, Workbooks, and Manga. When a new book arrives, it must be placed on the appropriate shelf for its field among the five shelves. This case can be considered a multi-class classification problem in the real world.

## 3) Regression
`Regression` problems are not cases in which the correct answer is fixed among a set number of choices, such as in the cases of having to choose one of two as in a classification problem or in the case of choosing one bookshelf out of 5 bookshelves when a book is received, but **within a certain range of continuous values**. It refers to the case where **predicted value comes from**.
<br>
<br>
For example, let's say you have a machine learning model that predicts the price of a property given the distance from a station, population density, number of rooms, etc. In some cases, the machine learning model will predict a property price of $784,563,450, and in other cases, it may predict  $812,573,000. Any result in a `continuous value` rather than a `discrete(non-continuous) solution`, as in classical classification problems, are called `regression problems`. Examples of regression problems include stock price forecasting, production forecasting, and index forecasting using time series data.

### Supervised and Unsupervised Learning
Machine learning is largely divided into `Supervised Learning`, `Unsupervised Learning` and `Reinforcement Learning`. And as a major branch, I will also mention `Self-Supervised Learning(SSL)`, which is not often mentioned, but is one of the important learning methods in deep learning natural language processing.

## 1) Supervised Learning
Supervised Learning refers to learning with `correct answers` called `labels`. `NLP` is mostly under `Supervised Learning`. Many of the problems in NLP we will be solving in the future are due to the existence of labels. Besides the label, y or actual value can be used and they are used interchangeably depending on the situation. The machine learns **by reducing the error**, which is the `difference` between `predicted value` and `actual value`. The predicted value can be expressed as y^.
## 2) Unsupervised Learning
`Unsupervised Learning` refers to **learning without labelling the data**. For example, LSA or LDA, which are topic modelling algorithms in text processing, belong to unsupervised learning.
## 3) Self-Supervised Learning(SSL)
**Given unlabelled data**, the data where the model learns **by creating labels** from the data for learning is called `self-supervised learning`. Representative examples include word embedding algorithms such as Word2Vec, and language model learning methods such as BERT.
### Sample and feature
Many machine learning problems have one or more `independent variables` x and is a problem to predict the `dependent variable` y. Among machine learning models, especially artificial neural networks, independent variables, dependent variables, weights and biases are often computed through `matrix operations`. In the future, when learning artificial networks, you will see a lot of cases where `training data` is expressed as a `matrix`. When X is the matrix of independent variable x, the matrix X with n independent variables and m data is as follows:
<br>
![sample](https://user-images.githubusercontent.com/40441643/207252884-67d59558-e4f2-4313-a772-dedf7ab17f2b.PNG)
<br>
<br>
At this time, in machine learning, one data. In matrix terms, one `row` is called a `sample`. And one `column` is called `feature` which is **each independent variables x to predict the dependent variable y**.
### Confusion Matrix
In machine learning, the number of question you got correct divided by the total number of questions is called `Accuracy`. Accuracy, however, does not tell you the details of whether you are correct or incorrect. A `confusion matrix` is used for this purpose. For example, suppose the problem to predict either `True` or `False`. In the confusion matrix below, each `column represents` a `predicted value` and each `row` represents an `actual value`.
<br>
![matrix](https://user-images.githubusercontent.com/40441643/207256886-6a9a060d-88e1-4652-933c-8be16069489b.PNG)
<br>
<br>
Machine learning defines `TP`, `FP`, `FN` and `TN` for each of the following four cases:
<ul>
<li> True Positive(TP): Predict that answer that is actually True is True(correct answer) </li>
<li> False Positive(FP): Predict that an answer is actually False is True(incorrect answer)</li>
<li>False Negative(FN): Predict that the answer that is actually True is False (wrong answer)</li>
<li>True Negative(TN): Predict that an answer that is actually False is False(correct answer)</li>
</ul>
Using this concept, it becomes `Precision` and `Recall`.
## 1) Precision
`Precision` is the ratio of what **the model classifies as true to what is actually true**.
<br>
<br>
![precision](https://user-images.githubusercontent.com/40441643/207259844-bc92d415-4eff-438a-b3a2-0505f495f761.PNG)
## 2) Recall
`Recall` is the ratio of what **the model predicts to be true out of what is actually true**.
<br>
![recall](https://user-images.githubusercontent.com/40441643/207260616-1ca08a7a-9dff-4825-9c29-80d11651ff4e.PNG)
<br>
<br>
Both `Precision` and `Recall` are `True` when the model predicts a `True answer`. That is, we are interested in `TP`. Note that the numerator in both equations is TP.
## 3) Accuracy
`Accuracy` is the most commonly used metric in real life. The percentage of `correct answers `out of `all predicted data`. If you explain the formula with TP, FP, FN, and TN, it is as follows.
<br>
<br>
![accuracy](https://user-images.githubusercontent.com/40441643/207261955-53fbe84d-ff49-4c6a-ac38-cac8be78d05a.PNG)
<br>
<br>
However, there are times when predicting the performance with `Accuracy` is not appropriate. Let's say you built a model to predict rainy days, and it rained only 6 days out of 200 days. However, the model predicted clear weather for all 200 days. This model was wrong a total of 6 out of 200 times. 194/200 = 0.97, so the accuracy is 97%. But on a rainy day, I could not get it right.
<br>
<br>
Let's take an another example. I created a spam mail classifier to classify spam mail. Out of 100 emails, 5 were spam. The spam mail classifier detected all mail as legitimate. Accuracy is 95%. However, I could not find a single spam email.
<br>
<br>
Accuracy is not a good metric if data for these more materially important cases make up **too little of the total data**. In this case, we use the `F1-Score`.
### Overfitting and Underfitting
Suppose you put yourself in the shoes of a student and solve too many problems on one sheet so that you can guess the correct answer just by looking at the number of the problem. However, if you solve one problem over and over again for a long time, or if you get a bad score on a test, is it meaningful?
<br>
<br>
In machine learning, `overfitting` refers to the case of `overlearning the training data` as in the metaphor above. The training data that machine learning models use to learn is actually only a fraction of the many real-world problems that machines will have to solve in the future. If the machine `overlearns` only on `training data`, `poor accuracy` occurs in the `test data` or actual service, which is data for `performance measurement`.
<br>
<br>
Under `overfitting`, the error is low on the training data, but large on the test data. The graph below shows the change in the training data error and test data error(also called `loss`) according to the number of training on the training data that can occur in an overfitting situation.
<br>
<br>
![graph](https://user-images.githubusercontent.com/40441643/207327676-1be83f5f-625b-43ce-8e05-270126c4f2cd.PNG)
<br>
<br>
The graph above is a graph in which overfitting was intentionally caused by giving the training number of training data as 30 epochs in the spam mail classification exercise in the text classification using RNN. The y-axis is the `error(loss)`, the x-axis `epoch` means **the number of times of training for the entire training data**, and compared to a human, it is the number of times the same question paper(training data) is repeatedly solved. **Too large epochs will overfit the training data.**
<br>
<br>
In the spam classification exercise, the accuracy on the test data is highest at epochs 3 to 4 , and overfitting occurs when the **epoch exceeds that**. The graph above shows how the error on the test data gradually increases as the number of epochs increases. Another way to describe **overfitting is to have high accuracy on the training data but low accuracy on the test data**. To avoid this situation, it is best to stop training before the error on the test data increases or before the accuracy decreases.
<br>
<br>
On the other hand, `underfitting` is a condition where there is room for improvement in the performance of the test data but `undertraining`. Underfitting is a condition in which `training itself` is `insufficient`, so it can occur when **the number of training epochs is too small**. Unlike overfitting, underfitting is characterised by `low accuracy` even on the `training data` because the `training itself` is `too little`.
<br>
<br>
The reason these two phenomena are called `overfitting` and `underfitting` is because the process called `learning` or `training` in machine learning is also called `fitting`. This is because the model is being fitted to the `given data`. For this reason, Keras calls `fit()` when training the machine.
<br>
<br>
In deep learning, there are several methods to `prevent overfitting`, such as `dropout` and `early stopping`.
<br>
<br>
While explaining overfitting and underfitting, it was explained that they can be judged using `test data`, but to be more precise, it is more desirable in practice to separate test data for `two purposes`. Each purpose is test data for `overfitting monitoring` and `hyperparameter tuning`, and test data only for `performance evaluation`. And the former test data is called `verification data`. Remember earlier that we said we should **split our data into three parts: training data, validation data and test data**. The training process of a typical deep learning model considering `overfitting prevention` is as follows:
<ul>
<li> Step 1. Divide the given data into training data, validation data, and test data. e.g. it can be divided into a 6:2:2 ratio.</li>
<li> Step 2. Train the model with training data. (epoch +1)</li>
<li> Step 3. Evaluate the model with validation data and calculate the accuracy and error(loss) for the validation data.</li>
<li> Step 4. If the error of the verification data has increased, it is a sign of overfitting, so after learning ends, go to Step 5. If not, go to Step 2. again.</li>
<li> Step 5. Now that model learning is complete, evaluate the model with test data.</li>
