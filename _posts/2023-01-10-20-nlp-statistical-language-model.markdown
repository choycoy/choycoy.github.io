---
title: "20 NLP: Statistical Language Model(SLM)"
tags:
  - NLP
  - Language Model
  - Statistical Language Model(SLM)
  - Sparsity Problem
  - Conditional Probability
  - Count-based approach
---
💡 This post introduces Statistical Language Model(SLM), Conditional Probability, Count-based approach and Sparsity Problem as Count-based approach's limitation.
{: .notice--warning}

# 03-02 Statistical Language Model(SLM)
An introduction to `Statistical language models`, a traditional approach to language models. Let's learn how a Statistical language model models a language with a statistical approach. Statistical Language Model is abbreviated as `SLM`.

## 1. Conditional probability
The `Conditional probability` has the following relationship for two probabilities `P(A)` and `P(B)`.
<br>
<br>
![conpro](https://user-images.githubusercontent.com/40441643/211445888-4a57adc0-cbae-4ee2-8e65-6e8bd5873006.PNG)
<br>
<br>
Let's generalise to more probabilities. When the four probabilities have a `conditional probability relationship`, it can be expressed as follows.
<br>
<br>
![conpro2](https://user-images.githubusercontent.com/40441643/211446183-8fa0a61b-0114-4dac-b4ce-8858d59d5b52.PNG)
<br>
<br>
This is called the `chain rule of conditional probability`. Let's generalise `n` probabilities not four probabilities.
<br>

![conpro3](https://user-images.githubusercontent.com/40441643/211447002-4c7be0cc-51c0-4ba9-a2a3-4492d92e1cc5.PNG)
<br>
<br>
Let's use the definition of `conditional probability` to find the probability of a sentence.

## 2.Probabilities for sentences
Probability of the sentence 'An adorable little boy is spreading smiles', P(An adorable little boy is spreading smiles). Let's express it as an expression.
<br>
<br>
Each word is influenced by the `previous word` due to a relationship called **context**. And from all the words one sentence is completed. Therefore, we will use `conditional probability` to **find the probability of a sentence**. If we rewrite the above-mentioned generalisation of `conditional probability` in terms of sentence probability, the probability of each word is composed of **the product of the probabilities** of each word appearing as the `next word` given the previous word.
<br>
![pro5](https://user-images.githubusercontent.com/40441643/211448751-47e7e533-d670-442e-8709-2b4d755bbcdf.PNG)
<br>
<br>
Applying the expression to the above sentence, we get:
<br>
<br>
![pro6](https://user-images.githubusercontent.com/40441643/211449132-eef61e51-67dc-4c26-947d-00afb5c3f32e.PNG)
<br>
<br>
Multiply the predicted probabilities for each word to get the sentence probabilities.

## 3. Count-based approach
We know that to get the probability of a sentence, we **multiply all the predicted probabilities of the next word**. So how does `SLM` find the probability of the `next word` from the `previous word`?: Calculate the probability based on the **correct answer count**.
<br>
<br>
When `an adorable little boy` comes out, let's calculate the probability that `is`, which is P(is|An adorable little boy).
<br>
<br>
![count](https://user-images.githubusercontent.com/40441643/211450388-d96886f3-a778-419f-a1d1-3f5166637d08.PNG)
<br>
<br>
The probability is as above. For example, suppose `An adorable little boy` appeared `100 times` in the machine-learned corpus data, followed by is `30 times`. In this case, P(is|An adorable little boy) is `30%`

## 4. Limitations of Count-Based Approach - Sparsity Problem
A language model **approximates the probability distribution** of a language used in real life. There is no way to find out exactly, but in reality, there is a probability of `is` that will come out when `An adorable little boy` comes out. Let's call this the probability distribution of real natural language, the `probability distribution` in reality. The goal of a language model is to **train a large corpus of machines to approximates the probability distribution** in real world through a `language model`. By the way, if you want to access it based on the `count`, you have a `corpus`. In other words, the data the machine trains on is really **massive**.
<br>
<br>
![count](https://user-images.githubusercontent.com/40441643/211450388-d96886f3-a778-419f-a1d1-3f5166637d08.PNG)
<br>
<br>
For example, in the case of obtaining P(is|An little boy) as above, if there was no word sequence called `An adorable little boy is` in the **corpus** trained by the machine, the probability of this word sequence is `0`. Or `An adorable little boy`, the probability is undefined because the denominator would be zero. So is it an accurate modelling practice to refer to this probability as `zero` or `undefined` because there is **no word sequence in the corpus**? No, there is a word sequence called `An adorable little boy is` and it is also grammatically appropriate, so the answer is likely to be correct. This problem of **failing to accurately model a language due to not observing enough data** is called a `Sparsity problem`.
<br>
<br>
As a way to alleviate the above problem, there are `n-gram language models`, which you will learn immediately, and various `generalisation techniques` such as `smoothing` and `backoff`, which are not covered in this blog. However, it has not been a fundamental solution to the `sparsity problem`. Eventually, due to these limitations, the trend in language models shifts from `statistical language models` to `artificial neural network language models`.
