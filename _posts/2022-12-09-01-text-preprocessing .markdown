---
title: "01 NLP: Text Preprocessing "
tags:
  - NLP
  - Text Preprocessing
  - Word Tokenisation
---

💡 This post introduces text preprocessing in NLP.
{: .notice--warning}
### Text preprocessing
Text preprocessing is the process of `preprocessing text` for the purpose of the problem to be solved. Just as cooking can be messy if you do not prepare the ingredients properly when cooking, the natural language processing techniques you will learn later will not work properly if you do not properly `pre-process the text`.

## 1. Tokenisation
If the corpus data obtained from NLP, such as `crawling`, is not pre-processed according to the need, `Tokenisation`, `Cleaning` and `Normalisation` are performed
to suit the intended purpose of using the data.
<br>
<br>
The task of dividing a given corpus into units called `tokens` is called `tokenisation`. Although the units of tokens vary from context to context, we usually define `tokens` in terms of `meaningful units`. Here, we understand the concept of tokenisation by mentioning various situations that can occur about tokenisation. Subsequently, `tokenisation` is performed by practicing through `NLTK` and `KoNLPY`.

## 2. Word Tokenisation
When tokens are based on `words`, it is called `word tokenisation`. However, here, a word is also considered as a word phase or a string with meaning in addition to a word unit.
<br>
<br>
Let's take an example. Let's do a simple world tokenisation task that excludes characters like `punctuation` from the input below. `Punctuation marks` are symbols such as periods(.), commas(,), question marks(?), semicolons(;), and exclamation points(!).

```
Input: Success is not final, failure is not fatal: it is the courage to continue that counts.
```
Output: Excluding punctuation marks from the inputs, the result of the tokenisation operation is:
```
Output: "Success", "is", "not", "final", "failure", "is", "not", "fatal", "it", "is", "the", "courage", "to", "continue", "that", "counts"
```
`Tokenisation` in this example is very simple. After erasing punctuation marks, it was cut based on `whitespace`. However, this example only shows the most basic example of `tokenisation`.
<br>
<br>
Usually, tokenisation tasks are **not solved** by performing a `cleaning operation` to `remove` all `punctuation` or `special characters`. In some cases, tokens `lose meaning` if you **remove all punctuation or special characters**. Unlike English, where word tokens are effectively separated by even `spacing`, it is difficult to distinguish word tokens in Korean using only `spaces`.

## 3. The moment of choice during Tokenisation
When doing `tokenisation`, there are unexpected cases that require you to think about the criteria for tokenisation. Of course, these selection can be made based on the criteria that do not affect the purpose, depending on what purpose the data will be used for. e.g. in English-speaking languages, we will show you the problem of **choosing how to classify words** containing an `apostrophe(')` as a `token`.
```
Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop.
```
How can we tokenize `Don't` and `Jone's` in the presence of an `apostrophe`? There are many options.
<ul>
  <li> Don't</li>
  <li> Don t</li>
  <li>Dont</li>
  <li>John's</li>
  <li>John's</li>
  <li>Jone</li>
  <li>Jones</li>
</ul>
Among them, you can design your own tokenisation tool to produce the desired result, but if the result of using the existing tools matches your purpose, you can use the tool. `NLTK` provides tools for **tokenising an English corpus**. Among them, we will use `word_tokenize` and` WordPuncTokenizer` to see how to handle `apostrophes`.
```
from nltk.tokenize import word_tokenize
from nltk.tokenize import WordPunctTokenizer
from tensorflow.keras.preprocessing.text import text_to_word_sequence
```
First, let's use `word_tokenize`.
```
print(`Word Tokenisation 1 : `, word_tokenize("Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop."))
```
```
Word Tokenisation 1 : [`Do`, "n't", `be`, `fooled`, `by`, 'the', 'dark', 'sounding', 'name', ',''Mr.', 'Jone', "'s", 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop', '.']
```
You can see that word_tokenize separated `Don't` into `Do` and `n't`, while `Jone's` separated into `Jone` and `s`. So, how does `wordPunctTokenizer` handle a corpus with an `apostrophe`?
```
print(`Word Tokenisation 2 :`, wordPunctTokenizer().tokenize("Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop."))
```
```
['Don', "'", 't', 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', ',', 'Mr', '.', 'Jone', "'", 's', 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop', '.']  
```
Since `WordPunctTokenizer` has a feature of **classifying punctuation marks separately**, unlike `word_tokenize`, which was checked earlier, `Don't` is separated into `Don`, `'`, and  `t`, and similarly, `Jone's` is separated into `Jone`, `'`, and `s`. `Keras` also supports `text_to_word_sequence` as a `tokenisation tool`.
```
print('Word Tokenisation 3 :',text_to_word_sequence("Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop."))
```
```
Word Tokenisation 3 : ["don't", 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', 'mr', "jone's", 'orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop']
```
Kera's text_to_word_sequence by default converts all alphabets to `lowercase` while removing punctuation such as `periods`, `commas`, and `exclamation marks`. But in case like `don't` or `jone's`, you can see that the **apostrophe is preserved**.

## 4. Things to consider in tokenisation
Tokenisation cannot be considered as simply cutting off punctuation from a corpus based on `white space`. This requires a more sophisticated algorithm, and here's why.
<br>
1. Do not simply exclude punctuation marks or special characters
<br>
When filtering out words from your corpus, simply **excluding punctuation or special character** is `not correct`. When refining the corpus, even `punctuation marks` are classified as a `token`. To take the most basic example, a period(.) is helpful in
**determining the boundary of a sentence**, so you may not exclude a period(.) when extracting words.
<br>
<br>
Another example is when **words themselves have punctuation marks**, such as `ph.D` or `AT&T`. Also, the `special character dollar` or `forward slash(/)` can mean a price, such as `$45.55` and, `01/02/06` can also mean a date. Normally in this case you might not want to treat `45.55` as one and separate them into `45` and `55`.
<br>
<br>
In some cases, a `comma (,)` is **inserted between numbers**. Usually when expressing a number, there is a `comma` in units of `three digits`, such as `123,456,789`.
2. When there is a space between an abbreviation and a words
<br>
In tokenisation, the `apostrophe(')` in English languages often acts as a `re-expansion` of `compressed words`. For example, `what's` is short for `what is`, and `we're` is short for `we are`. In the example above, `re` is called `clitic`. In other words, it refers to the form that occurs when a word is used as an `abbreviation`. For example, when there is `I'm` reduced from `I am`, it is said to fold `m`.
<br>
<br>
Let's look at the word `New York` or the word `rock 'n' roll`. These words are one word, but there is a `space` between them. Depending on the purpose of use, even if there is a `space` **between a single word**, it may need to be viewed as a **single token**, so the tokenisation task must also have the ability to recognise those words as one.
3. Standard Tokenisation Example
<br>
To help you understand, let's introduce the `rules` of `Penn Treebank Tokenisation`, one of the standard tokenisation methods, and check the results of tokenisation.
<br>
<br>
```
Rule 1. Keep words made up of hyphens as one.
Rule 2. Separate words with an apostrophe `fold`, such as doesn't
```
Enter the sentence below as input to the standard.
<br>
<br>
"Starting a home-based restaurant may be an ideal. It doesn't have a food chain or restaurant of their own."
```
from nltk.tokenize import TreebankWordTokenizer
tokenizer = TreebankWordTokenizer()
text = "Starting a home-based restaurant may be an ideal. it doesn't have a food chain or restaurant of their own."
print('Treebank Wordtokenizer : ', tokenizer.tokenize(text))
```
```
Treebank Wordtokenizer : ['Starting', 'a', 'home-based', 'restaurant', 'may', 'be', 'an', 'ideal.', 'it', 'does', "n't", 'have', 'a', 'food', 'chain', 'or', 'restaurant', 'of', 'their', 'own', '.']
```
Looking at the results, according to Rule 1 and Rule 2, respectively, `home-based` is treated as `one token`, and in the case of `dosen't`, `does` and `n't` are separated.
## 5. Sentence tokenisation
This time, we will discuss the case where **the token unit is a sentence**. This task is to **classify sentences into sentences** within the corpus you have, and is sometimes called `sentence segmentation`. Usually, if the corpus you have is in an unrefined state, the corpus is not divided into sentence units, so you may need `sentence tokenisation` to suit intended use.
<br>
<br>
How can we classify `sentence-by-sentence` from a given corpus? Intuitively? or `period(.)` or` !`. You might think that it would be okay to cut off the sentence as a standard, but that is not necessarily the case. This is because `!` or `?` serve as fairly **clear boundaries for separating sentences**, but **periods do not**. A `period` can appear even if it is not at the end of a sentence.
<br>
```
e.g. 1) Enter the IP 192.168.56.31 server, save the log file, and send the results to aaa@gmail.com. After that, let's go have lunch.
e.g. 2) Since I'm actively looking for Ph.D. students, I get the same question a dozen times every year.
```
For example, what if we applied `sentence tokenisation` based on `period` to the example above? In the second example, year., recognising the end of the sentence for the first time correctly predicted the end of the sentence. However, assuming that sentences are simply separated by `period(.)`, the expected result is not produced because the period appears several times before the end of the sentence.
<br>
<br>
You can define your own rules according to the language of the nationality of the corpus you are using, or how special characters are used within that corpus. Achieving 100% accuracy is not an easy task, because if you make a type in your corpus data or if the structure of your sentences is messed up, the rules you have set may be useless.
<br>
<br>
`NLTK` supports `sent_tokenize`, which **tokenises English sentences**. Let's practice `sentence tokenisation` with `NLTK`.
```
from ntl.tokenize import sent_tokenize
text = "His barber kept his word. But keeping such a huge secret to himself was driving him crazy. Finally, the barber went up a mountain and almost to the edge of a cliff. He dug a hole in the midst of some reeds. He looked about, to make sure no one was near."
print(`Sentence Tokenisation 1: `, sent_tokenize(text))
```
```
Sentence Tokenisation 1: ['His barber kept his word.', 'But keeping such a huge secret to himself was driving him crazy.', 'Finally, the barber went up a mountain and almost to the edge of a cliff.', 'He dug a hole in the midst of some reeds.', 'He looked about, to make sure no one was near.']
```
The code above distinguishes a sentence from multiple sentences stored in text. If you look at the output, you can see that it has successfully identified all sentences. Then, this time, let's practice the case where there are many periods in the middle of the sentence.
```
text = "I am actively looking for Ph.D. students. and you are a Ph.D student."
print('Sentence Tokeniation 2 :',sent_tokenize(text))
```
```
Sentence Tokenisation 2 : ['I am actively looking for Ph.D. students.', 'and you are a Ph.D student.']
```
You can see that `NLTK` successfully recognises `ph.D` as a `word` in a sentence because it simply does not separate sentences with `period` as `separators`.
