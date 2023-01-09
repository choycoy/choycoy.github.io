---
title: "19 NLP: Language Model"
tags:
  - NLP
  - Language Model
  - Statistical Language Model(LSM)
---
💡 This post introduces
{: .notice--warning}

A `Language Model` is a model that **assigns probabilities to word sequences(sentences)**. When there are certain sentences, the machine says this sentence is appropriate!, This sentence makes no sense! If it can accurately judge that, like a `human`, the performance of machine's nlp can be said to be excellent.
<br>
<br>
In this post, we learn about the `Statistical Language Model(LSM)` based on `statistics`. Statistical-based language models have many limitations in approximating the nlp we actually use, and these days, as artificial neural networks have solved many of these limitations, Statistical-base language models have decreased in use. But nonetheless, an understanding of the `statistics-based methodology` helps to get a holistic view of the language model.

## 03-01 What is a Language Model?
A Language Model(LM) is a model that **assigns probabilities** to `word sequences(sentences)` to model the phenomenon of language.
<br>
<br>
Methods for creating language models can be largely divided into methods using `statistics` and method using `artificial neural networks`. Recently. methods using artificial neural networks have shown better performance than methods using statistics. Recently, hot nlp technologies such as `GPT` and `BERT` were also created using the concept of an artificial neural network language model. In this post, we will learn about the concept of `language models` and the traditional approach to language models, `statistical language models`.

### 1. Language Model
A language model is a model whose job is to **assign probabilities to word sequences**. To paraphrase this a bit, a language model is a model that **finds the most natural word sequences**. The most commonly used way to assign probabilities to a sequence of words is to have a `language model` **predict the next word given the previous words**.
<br>
<br>
Another type of language model is one that predicts an `empty middle word` given words on either side. This is similar to the **blank reasoning problem** in the high school entrance exam in which the word in the middle of sentence is left blank and the word in the blank is guessed through the context on both sides. This type of language model will be covered in `BERT` chapter, until then we'll focus on how to **predict the next word from previous words**.
<br>
<br>
**Language Modelling**, which adds `-ing` to a language model, refers to the task of predicting unknown words from given words. In other words, language modelling is when a language model predicts the next word from previous words.
<br>
<br>
Standford University, famous for nlp, like its language model to a grammar, This is because the job of a language model is to tell us **how appropriate a combination of words or sentences are**, just like `grammar` does.

### 2. Probability Assignment of Word Sequences
Why is it necessary to assign probabilities to word sequences in nlp? Let's take an example. The capital `P` stands for `Probability`.

#### a. Machine Translation:
```
P(나는 버스를 탔다) > P(나는 버스를 태운다)
```
: The language model `compares` the two sentences and determines that the sentence on the left has a **higher Probability**.

#### b. Spell Correction
```
선생님이 교실로 부리나케
P(달려갔다) > P(잘려갔다)
```
:The language model `compares` the two sentences and determines that the sentence on the left has a **higher Probability**.

#### c. Speech Recognition
```
P(나는 메롱을 먹는다) < P(나는 메론을 먹는다)
```
:The language model `compares` the two sentences and determines that the sentence on the left has a **higher Probability**.

The language model uses `probabilities` as above **to determine more appropriate sentences**.

### 3. Predict the next word from given previous words
A language model is a model that assigns probabilities to word sequences. And the most common way to assign probabilities to word sequences is to **predict the next word given the previous words**. Let's express this as a `conditional probability`.

#### A. probabilities of word sequences
If we set the word as `w`, word sequence as `W`, the probability of `W` containing `n` words is:
<br>
![pro](https://user-images.githubusercontent.com/40441643/211271998-173d505b-cf8e-4d59-9cc2-c270bb1918d2.PNG)

#### B. Next word probability
Let's express the probability of **next word** appearing as an expression. Given `n-1` words listed, the probability of the `nth` word is:
<br>
![pro2](https://user-images.githubusercontent.com/40441643/211275134-4d0a142f-ea97-4872-aed1-e25fcd12eaa3.PNG)
<br>
<br>
The symbol in `|` stands for the **conditional probability**.
<br>
<br>
For example, the probability of 5th word is as follows.
<br>
![pro3](https://user-images.githubusercontent.com/40441643/211275460-54f53b1f-6cdf-4cfe-b375-0655932b9b49.PNG)
<br>
<br>
The whole word sequence `W` is known after all words have been predicted, so the probability of a sequence of words is:
<br>
![pro4](https://user-images.githubusercontent.com/40441643/211276195-05f5a283-020e-4aae-b44f-c323ddf3dfb1.PNG)
<br>
<br>
### 4. A simple Intuition in Language Models
There is a sentence `비행기를 타려고 공항에 갔는데 지각을 하는 바람에 비행기를 [?]`. One could easily guess `놓쳤다` which word would be come after `비행기를`. This is because based on our knowledge, we put several possible words as **candidates** and judged that the word missed was the most likely.
<br>
<br>
So, if we give the above sentence to a machine and ask it to predict the word that will came after `비행기를`, how can it make the `prediction` as accurately as possible? The machines are similar. Considering which words came before it, predict the probabilities of `several possible words` and select the word with the **highest probability**.

### 5. Examples of language models in search engines
![google](https://user-images.githubusercontent.com/40441643/211278117-fd1eded5-d96a-4223-9c60-e5b5e8bb24b0.PNG)
<br>
<br>
A **search engine** uses a `language model` that predicts the next word for a sequene of words entered.
