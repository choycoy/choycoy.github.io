---
title: "00 NLP: Natural Language Processing "
tags:
  - NLP
---

💡 This post introduces NLP.
{: .notice--warning}
- Intuition
<br>
Have you ever wondered how robots such as Alexa or home assistants sound so humanlike? How do they understand you? All of this is because of the magic of `Natural Language Processing` or` NLP`. Using NLP you can make machines sound human-like and even ‘understand’ what you’re saying.

- What is NLP?
<br>
Humans communicate with each other using `words` and `text`. The way that humans convey information to each other is called **Natural Language**. Every day humans share a large quality of information with each other in various languages as `speech` or `text`.
<br>
<br>
However, computers cannot interpret the data, which is in natural language, as they communicate in 1s and 0s. The data produced is precious and can offer valuable insights. Hence, you need computers to be able to be understand, emulate and respond intelligently to human speech.
<br>
<br>
Natural Language Processing or NLP refers to the branch of `Artificial Intelligence` which gives the machines the ability to **read**, **understand** and **derive meaning** from `human languages`.
NLP combines the field of `linguistics` and` computer science` to decipher language structure and guidelines and to **make models** which can comprehend, break down and separate significant details from `text` and `speech`.

- Machine Learning Workflow
<br>
Since Deep Learning is a branch of Machine Learning, Deep Learning Workflow can be considered as a Machine Learning Workflow.
- Machine Learning Workflow: the process of acquiring the data and Machine Learning.
<br>
<br>
![machine](https://user-images.githubusercontent.com/40441643/206641312-d987a7ad-3338-40df-bd11-7cb26ac8f863.PNG)

1. Acquisition
<br>
Machine Learning requires data to be trained on. In the case of natural language processing, natural language data is called a `corpus`, and the meaning of the `corpus` refers to `a set of texts` collected from a specific domain for research or research purpose. The file formats of text data are diverse, such as txt files, csv files, and xml files, and the sources are diverse, such as `voice data`, data collected through web collectors, and movie reviews.

2. Inspection and exploration
<br>
Once the data has been collected, the next step is to **examine** and **explore** the `data`. Here, you need to understand the structure of the data, `noise data`, and how to `refine the data` for **applying machine learning**.
<br>
<br>
This step is also called the `exploratory data analysis(EDA) step`, which means the process of **finding out** `the characteristics of the data `and their inherent structural relationships by examining the independent variable, dependent variable, variable type, and data type of the variable. `Visualisation` and `simple statistical tests` are also performed during this process.

3. Preprocessing and Cleaning
<br>
Once you've figured out your data, you're ready to move on to one of the most challenging tasks in machine learning workflow: `data preprocessing`. This step includes many steps, such as `tokenisation`, `refinement`, `normalisation`, and `stopword removal` in the case of natural language processing. In order to do fast and accurate data processing, knowledge of various `libraries` for the toll you are using is required. e.g. tool for python. For really challenging preprocesssing, machine learning is sometimes used in preprocessing.

4. Modelling and Training
<br>
After the data preprocessing is finished, the modelling step, which is the step of `writing code for machine learning`, enters. After modelling is completed by **selecting an appropriate machine learning algorithm**, the preprocessed data is trained by the machine through the machine learning algorithm. This is also called `training`, and the two terms are used interchangeably, After the machine finishes learning on the data and is properly trained, the machine can then perform natural language processing tasks such as `machine translation`, `speech recognition`, and `text classification`, which are the tasks we want.
<br>
<br>
The caveat here is that in most cases you should not be training all the data on the machine. You should use only the `training data` for `training`, leaving some of the data for `testing`. Only then, after the machine learns, it can measure **how much the current performance** is trough the `test data` and prevent `overfitting`.
<br>
<br>
In fact, the best is `training`, `validation` and `testing` rather than splitting the two into `training and testing`. The data is divided into three parts and only training data is used for training.
<br>
<br>
What is the difference between `validation` and `testing`? To compare it into the CSAT, it can be seen as a study guide for training, a `mock test` for `verification`, and an SAT test for `testing`. You can solve the homework and take the CSAT, but there is also a way to take a mock test to `verify` and `supplement` **what is lacking**. In fact, in the case of business, verification data is almost essential.
<br>
<br>
The `validation data` is the `performance of the current model`. In other words, it is used to **judge how well the machine has learned with training data**. The test data is used to evaluate the final performance of the model, and is not used to improve the model's performance, but to quantify and evaluate the model's performance. In simple terms, it is the stage of `scoring` if compared to a test.

5. Evaluation
<br>
As mentioned earlier, once the machine has been trained, its performance will be evaluated with test data. The evaluation method measures **how close the data predicted by the machine** is to the `actual correct answer` on the test data.

6. Deployment
<br>
In the evaluation stage, if the machine judged to have been successfully trained, the completed model is deployed. However, if there is a situation where the model needs to be updated due to the overall feedback on the completed model, you can `return` the `collection stage`.
