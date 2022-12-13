---
title: "02 NLP: What is Machine Learning? "
tags:
  - NLP
  - Machine Learning
---
💡 This post is about the concept of machine learning, a concept that includes deep learning.
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
