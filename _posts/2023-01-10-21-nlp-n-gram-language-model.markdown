---
title: "21 NLP: N-gram Language Model"
tags:
  - NLP
  - N-gram Language Model
---
💡 This post introduces N-gram Language Model and its limitations.
{: .notice--warning}
# 03-03 N-gram Language Model
The `n-gram language model` is a type of SLM as it still uses a `Statistical approach` based on `counts`. However, unlike the language model discussed earlier, it uses an approach that **considers only some words** rather than all previously encountered words. And at this time, it determines **how many words are seen**, which is the meaning of `n` in `n-gram`.

## 1. Reduction in cases of failure to count in the corpus.
A limitation of `SLM` is that training corpus may not have sentences or words for which you want to compute probabilities. And the longer the sentence you want to compute probabilities for, the more likely that sentence does not exit in the corpus you have. In other words, there is a `high probability` that it cannot be counted. However, if you reduce the words you refer to as follows, you can increase your chances of being counted.
<br>
<br>
![reduction](https://user-images.githubusercontent.com/40441643/211460011-faf5068c-bee6-4841-98ed-99b080ed31f9.PNG)
<br>
<br>
For example, why don't you think of the probability of `is` coming out when `An adorable little boy` comes out as the probability of `is` coming out when a `boy` comes out? It's more likely that your corpus contains the `shorter word sequence` `boy` is than it does `An adorable little boy is`. If it feels like a bit of an over-generalisation, an alternative is to think of as the probability of `is` coming out when `little boy` comes out as shown below.
<br>
<br>
![reduction2](https://user-images.githubusercontent.com/40441643/211460570-1cb28f4e-6373-42d1-b0d8-d5ae0c400e6b.PNG)
<br>
<br>
In other words, in order to obtain the probability of `is` appearing when `An adorable little boy` appeared earlier, it was necessary to count the number of times `An adorable little boy is` appeared. It does not count all words, but **approximates it by counting only a random number of preceding words**. This will increase your chances of counting that sequence of words in your corpus.

## 2. N-grams
At this time, `n-gram` is used as a criterion for **determining an arbitrary number**. An `n-gram` is a sequence of **n consecutive words**. It is divided into `n-word clusters` from the corpus you have and considers them as one `token`. For example, when there is a sentence `An adorable little boy is spreading smiles`, if we obtain all n-grams for each `n`, we get the following.
<br>
<br>
`uni` grams: an, adorable, little, boy, is, spreading, smiles
`bi` grams: an adorable, adorable little, little boy, boy is, is spreading, spreading smiles
`tri` grams: an adorable little, adorable little boy, little boy is, boy is spreading, is spreading smiles
`4-` grams: an adorable little body, adorable little boy is, little boy is spreading, boy is spreading smiles.
<br>
<br>
When `n-grams` are used, they are named unigrams when `n` is `1`, bigrams when `n` is `2`, trigrams when `n` is `3`, and names by attaching a number in front of gram when `n` is `4 or more`. Depending on the source, unigrams, bigrams, and trigrams are also referred to as 1-gram, 2-gram, and 3-gram, respectively. Let's design a `language model` using `n-grams`.
<br>
<br>
In a language model using `n-grams`, the prediction of the `next word` depends only on **n-1 words**. For example, if you want to predict the word that will come after 'An adorable little boy is spreading', let's say use a 4-gram language model with `n = 4`. In this case, predicting the word that comes after spreading only takes into account the **first 3 words** corresponding to `n-1`.
<br>
<br>
![eg](https://user-images.githubusercontent.com/40441643/211465765-642e7241-030b-466a-940b-e0e334b2870b.png)
![eg2](https://user-images.githubusercontent.com/40441643/211465864-167811ac-97f7-4a5d-a598-8790cdb93ba0.PNG)
<br>
<br>
Let's say that `boy is spreading` appears 1,000 times in your corpus. And let's say `boy is spreading insults` appeared 500 times and `boy is spreading smiles` appeared 200 times. Then there is a `50%` chance that `insults` will appear after `boy is spreading`, and a `20%` chance that `smiles` will appear. Depending on the stochastic selection, we judge that the `insults` are more **correct**.
<br>
<br>
![eg3](https://user-images.githubusercontent.com/40441643/211466236-cdeba133-e1e9-4f3f-aed5-2d16fd5cd03f.PNG)


## 3. Limitations of N-gram Language Model
Earlier, we confirmed how the language model works through `4-grams`. However, a little question remains. The 4-gram language model we saw earlier remove the modifier `an adorable little`, which was a word in front of the given sentence, and did not reflect it. However, if it was a language model that predicted the next word for the actions of a `small and loving boy`, taking into account all the modifiers of `small and loving`, the negative contents f `the small and lovely boy` and `spreading insults` would be `laughed`. Could it have been chosen instead of something positive?
<br>
<br>
Of course, it depends on how you assume the corpus data, and it's not a sentence that does not make sense at all, but what I want to point out here is that `n-gram` sometimes do not end sentences the way they want to because they **see only a few words** in front of them. When reading a sentence, the `context` of the front and back parts may not be connected at all. In conclusion, it is inevitably less accurate than a language model that considers the entire sentence. Based on this, let's summarise the limitations of the `n-gram` model.

### (1) Sparsity Problem
Although it was possible to increase the probability of being counted in the corpus realistically by looking at only some words rather than seeing all the preceding words in the sentence, the n-gram language model still has a sparsity problem for `n-grams`.

### (2) Choosing n is a trade-off problem
There is a trade-off in deciding `n` how many words to look at in front. Choosing an arbitrary number `n` of `2` rather than `1` can improve the performance of the language model in almost all cases. For example, it is more accurate to predict the `next word` by looking at `is spreading` rather than just looking at `spreading`. In this case, if the training data were adequate, the language model would not pick a **verb** at least after spreading.
<br>
<br>
The sparsity problem becomes increasingly severe if we choose a **large** `n`, since the probability of `counting that n-gram` in the actual training corpus becomes small. There is also the problem that the `model size increases` as `n increases`. This is because we need to **count every n-gram** in the corpus by default.
<br>
<br>
Choosing a **small** `n` will result in good `counts` in the training corpus, but the **accuracy of the approximation will be far from the real probability distribution**. That's why you need to choose an appropriate `n`. Due to the aforementioned trade-off problem, it is recommended that **n should not exceed a maximum of 5** to increase accuracy.
<br>
<br>
Let's take a famous example to see how `n` affects performance. According to shared data from Stanford University, the Wall Street Journal reported that the following performance was obtained when an `n-gram language model` was trained on 38 million word tokens and tested on 15 million test data. As you will learn later, **the lower the perplexity value, the better the performance**.
<br>
![eg4](https://user-images.githubusercontent.com/40441643/211469769-c3ff1de2-adad-4404-aea3-09a3eb8b5577.PNG)
<br>
<br>
The above results show that the **performance increases** whenever `n is increased` from `1 to 2` and `2 to 3`.

## 4. Collection of corpus suitable for domain
Depending on the field and the application, the `probability distribution` of certain words is naturally different. For example, in the marketing field, marketing words will appear frequently, and in the medical field, medical-related words will appear frequently. In this case, if the corps used for the language model is the corpus of the **corresponding domain**, the possibility of the language model generating the **correct language increases** naturally.

## 5. Neural Network Based Language Model
Although it will not be covered here, there are several generalisation methods to overcome the limitations of `N-gram Language Model`, such as adding numbers to the `denominator` and `numerator` to **prevent them from becoming 0** when counted. However, fundamentally, the weakness of the `n-gram` language model has not been completely resolved, and as an alternative to this, language models using **artificial neural networks**, which are generally superior to `N-gram language models`, are widely used.
