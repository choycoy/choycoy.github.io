---
title: "15 NLP: One-Hot Encoding"
tags:
  - NLP
  - One-Hot Encoding
  - Keras' to_categorical()
---
💡 This post introduces One-Hot Encoding, practice in Keras and its limitations.
{: .notice--warning}

A computer or machine can handle `numbers better` than letters. To this end, `nlp` has several techniques for **converting letters into numbers**. `One-Hot Encoding` is the most basic expression method of **expressing words** among many techniques, and it is an `expression method` that must be learned for `machine learning` and `deep learning`.
<br>
<br>
Before learning about `one-hot encoding`. let's define a **vocabulary**. Word set is a concept that will continue to emerge in `nlp` in the future, so you need to understand it here. `A word set` is a **set of different words**. We need to pay more attention to the different definitions of words so that there is no confusion here. `Vocabulary` basically considers **variation of words** as other words, such as `book and books`. In this document, we will take the words in the word set and turn the **letters into numbers**. More specifically, you will learn about several methods, including `one-hot encoding` to turn the into **vectors**.
<br>
<br>
The first thing to do for `one-hot encoding` is to **create a set of words**. When all the words of a text are **grouped together** without allowing `duplicates`, it is called `a word set`. It then proceeds with `integer encoding`, which gives this set of words a `unique integer`. If there are a total of 5,000 words in the text, the size of the word set is 5,000. Let's say you want to `index` each word in this word set of `5,000 words` from `1 to 5,000`. For example, number 150 for book, number 171 for dog, number 192 for love, and number 212 for books.
<br>
<br>
Now let's say we gave each word a `unique integer index`. What if we want to treat the words that have been **converted into these numbers as vectors**?

## 1. What is One-Hot Encoding?
`One-Hot Encoding` is **a vector representation of words** in which the size of a set of words is **the dimension of the vector**, and a value `1` is assigned to the index of the word to be represented, and `0` is assigned to other indices. A `vector` expressed in this way is called a `one-hot vector`.
<br>
<br>
`One-Hot encoding` can be summarised in two steps. First, it does `integer encoding`. In other words, it gives each word a `unique integer`. Second, consider the unique integer of the word you want to express as an `index` and assign `1` to the corresponding position, and assign `0` to the index position of other words. Let's create a `one-hot vector` using a Korean sentence as an example.
```
Sentence: 나는 자연어 처리를 배운다
```

Tokenise sentences using the `Okt morpheme analyzer`.
```
from konlpy.tag impot Okt

okt = Okt()
tokens = okt.morphs("나는 자연어 처리를 배운다")
print(tokens)
```

```
['나', '는', '자연어', '처리', '를', '배운다']
```

Give each token a unique integer. Right now, because the sentences are short, we do not consider the frequency of each word, but we often give integers by **sorting the words in order of frequency**.
```
word_to_index = {word : index for index, word in enumerate(tokens)}
print('the word set:', word_to_index)
```

```
the word set: {'나': 0, '는': 1, '자연어': 2, '처리': 3, '를': 4, '배운다': 5}
```

I created a function that when you input a token, creates a `one-hot vector` for that token.
```
def one_hot_encoding(word, word_to_index)
  one_hot_vector = [0]*(len(word_to_index))
  index = word_to_index[word]
  one_hot_vector[index] = 1
  return one_hot_vector
```

Let's get the `one-hot vector` of the word `자연어`.
```
one_hot_encoding("자연어",word_to_index)
```

```
[0, 0, 1, 0, 0, 0]  
```

`자연어` is an integer of `2`, so a `one-hot vector` will result in a vector with `index 2` having a value of `1` and the rest of the values being `0`.

## 2. One-Hot Encoding with Keras
Although the above code was written in `Python` to understand `one-hot encoding`, `Keras` supports a useful tool `to_categorical()` to perform `one-hot encoding`. This time, we will sequentially proceed with `integer encoding` and `one-hot encoding` using only `Keras`.

```
text = "나랑 점심 먹으러 갈래 점심 메뉴는 햄버거 갈래 갈래 햄버거 최고야"
```

Given the above statement, the `integer encoding` using `Keras Tokenizer` is as follows:
```
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.utils import to_categorical

text = "나랑 점심 먹으러 갈래 점심 메뉴는 햄버거 갈래 갈래 햄버거 최고야"

tokenizer = Tokenizer()
tokenizer.fit_ont_texts([text])
print('the word set:', tokenizer.word_index)
```

```
the word set: {'갈래': 1, '점심': 2, '햄버거': 3, '나랑': 4, '먹으러': 5, '메뉴는': 6, '최고야': 7}
```

If you have a text composed of only words in the `vocabulary` created as above, you can convert it to an `integer sequences` with `texts_to_sequences`. Let's check this by creating `sub_text`, which is a subtext consisting only of some words in the generated word set.
```
sub_text = "점심 먹으러 갈래 메뉴는 햄버거 최고야"
encoded = tokenizer.texts_to_sequences([sub_text])[0]
print(encoded)
```

```
[2, 5, 1, 6, 3, 7]
```

What we've done so far is something we've already learned from previous integer encoding exercises. Now, with the result, let's proceed with `one-hot encoding`. `Keras` supports `to_categorical` which performs `one-hot encoding` from integer encoded results.
```
one_hot = to_categorical(encoded)
print(one_hot)
```

```
[[0. 0. 1. 0. 0. 0. 0. 0.] # one-hot vector of index 2
 [0. 0. 0. 0. 0. 1. 0. 0.] # one-hot vector of index 5
 [0. 1. 0. 0. 0. 0. 0. 0.] # one-hot vector of index 1
 [0. 0. 0. 0. 0. 0. 1. 0.] # one-hot vector of index 6
 [0. 0. 0. 1. 0. 0. 0. 0.] # one-hot vector of index 3
 [0. 0. 0. 0. 0. 0. 0. 1.]] # one-hot vector of index 7
```

The above result is that the sentence `점심 먹으러 갈래 메뉴는 햄버거 최고야` is encoded as an integer as `[2, 5, 1, 6, 3, 7]`, and then `one-hot encoding` is performed using the `index` of each encoded result.

## 3. Limitations of One-Hot Encoding
This representation method has disadvantage that the space required to **store the vector** continues to increase as the number of words is increased. In other words, the **dimension of the vector is increased**. For `one-hoot vectors`, the `size of the word set` is the `number of dimensions of the vector`. For example, if you take a corpus of `1,000` words and create a `one-hot vector`, **each word is a vector with 1,000 dimensions**. In other words, each word has only one value of `1`, an 999 has values are vectors with a value of `0`. This is a very **inefficient representation** in terms of `storage space`.
<br>
<br>
`One-hot vectors` also have the disadvantage of **not representing word similarity**. For example, by `one-hot encoding` for four words `wolf`, `tiger`, `dog` and `cat`,[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1 , 0] and [0, 0, 0, 1]. In this case, the `one-hot vector` cannot express the `similarity` between `dogs` and `wolves`, and `tigers` and `cats`. More extremely, if you have the words `puppy`, `dog` or `refrigerator`, you don't know which of the words `dog` or `refrigerator` is **more similar to the word dog**.
<br>
<br>
The disadvantage of **not knowing the similarity between words** can be a problem in `search systems`. For example, let's say you search for the word `Sapporo Accommodation` in the web research bar to go on a trip. A proper search system should be able to show results for `similar words` such as `Sapporo guesthouse`, `Sapporo ryokan`, and `Sapporo hotel` for the **search term** `Sapporo lodging`. But if we can't calculate the `similarity between words`, we won't be able to show **related search terms** like `guest guesthouse`, `ryokan` and `hotel`.
<br>
<br>
In order to solve these disadvantages, there are two main `vectorisation techniques` in a `multidimensional space` by **reflecting the latent meaning of a word**. First, there are **count-based vectorisation method** such as `LSA(Late Semantic Analysis)` and `HAL`, and second, **prediction-based vectorisation** such as `NNLM`, `RNNLM`, `Word2Vec`, and `FastText`. And there is a method called `GloVe` that uses both `count-based` and `prediction-based` methods.
